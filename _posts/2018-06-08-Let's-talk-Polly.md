---
layout: post
title: We interrupt our program....
subtitle: A project at work very interesting solution and I thought it would be great to write about it.
image: /img/Polly-Logo-large.png
share-img: /img/Polly-Logo-large.png
tags: [asp.net, Polly, Resilliency, fun stuff]
---

Ok, so here I am at work and I'm in the middle of re-architecting a project that has existed for a very long time.  I developed an API to abstract away all the FTP activities we perform.  We connect to about a dozen SFTP servers across thousands of logins.  I'm not going to talk about managing those logins, or real specifics on that side.

Today, I want to espouse the usability of <a href="http://www.thepollyproject.org/" target="_blank">Polly</a>:
> Polly is a .NET resilience and transient-fault-handling library that allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.

I will leave it to the reader to get into the <a href="https://github.com/App-vNext/Polly/wiki" target="_blank">nitty-gritty</a> of Polly and there are lots of examples of its use around the Internet.

What I want to talk about and document is my solution to solve a problem we have had with SFTP operations over the years: What happens when a remote server is offline, unreachable, or reports a bogus error regardless if the login is valid or not?

Enter Polly's <a href="https://github.com/App-vNext/Polly/wiki/Circuit-Breaker" target="_blank">Circuit Breaker</a> Policies.  Circuit breakers are a technique to manage consecutive exceptions or result across call sites and apply rules to "fail fast" and manage what happens in certain instances.

We have an ASP.NET API that our internal processes call when they need to perform SFTP actions with files (put file, get file, get files, test connection, etc.).  The code that did this lived in several disparate services and anytime changes had to happen, multiple code bases had to be updated.  Add to that complexity we use a third-party FTP/SFTP component with all its versioning issues and a person can spend a lot of time just working with those processed.  Well, that person was and still is me.

I decided we need a central way to handle all the FTP business that comes in or out of our organization (keep in mind this is to support a few products, other code bases still have ftp/sftp code in them but they don't do nearly the volume that this will be handling.)

After I put the SFTP processing into the web API, we still had the problem of what happens when we can't transmit a file or even get to the remote server?

Polly came to my attention through a <a href="https://www.pluralsight.com/courses/polly-fault-tolerant-web-service-requests" target="_blank">PluralSight</a> course by <a href="https://nodogmapodcast.bryanhogan.net/" target="_blank">Bryan Hogan</a>.  I realized after taking the course what Polly could help me with.  It could help us be more resilient in how we deal with the unexpected in this domain.  

If you follow along with Bryan's course, you will notice that the big use of Polly is wrapping HTTP calls.  It can not only handle exceptions with `.Handle<Exception>`, it can `.HandleResult<HttpResponseMessage>(r => !r.IsSuccessStatusCode)`.

What if we needed to handle problems when using FTP or SFTP?  There isn't anything readily available in the .NET framework to let us handle that outside of Boolean and Exceptions.

I looked to Microsoft for inspiration.  Why not implement my own class based on HttpResponseMessage?  FTPResponseMessage was born:

```csharp
public class FTPResponseMessage 
{
        
    private const FTPStatusCode defaultStatusCode = FTPStatusCode.OK;
        
    private string _reasonPhrase;
        
    private bool _disposed;
    
    private StringBuilder _connectionLog;
        
    public List<RemoteFile> RemoteFiles { get; }
        
    public void AddRemoteFile(RemoteFile file)
    {
        RemoteFiles.Add(file);
    }
        
    public string ConnectionLog
    {
        get { return _connectionLog.ToString(); }
    }

    public void AppendConnectionLog(string message) => _connectionLog.AppendLine(message);

    public FTPStatusCode StatusCode { get; set; }

    internal void SetStatusCodeWithoutValidation(FTPStatusCode value) => StatusCode = value;


    public bool IsSuccessStatusCode
    {
        get { return ((int)StatusCode >= 200) && ((int)StatusCode <= 299); }
    }

    public FTPResponseMessage()
        : this(defaultStatusCode)
    {
    }

    public FTPResponseMessage(FTPStatusCode statusCode)
    {

        if (((int)statusCode < 0) || ((int)statusCode > 999))
        {
            throw new ArgumentOutOfRangeException(nameof(statusCode));
        }

        StatusCode = statusCode;
        _connectionLog = new StringBuilder();
        RemoteFiles = new List<RemoteFile>();
    }

    internal void SetReasonPhraseWithoutValidation(string value) => _reasonPhrase = value;


    public string ReasonPhrase
    {
        get
        {
            if (_reasonPhrase != null)
            {
                return _reasonPhrase;
            }
            // Provide a default if one was not set.
            return FTPStatusDescription.Get(StatusCode);
        }
        set
        {
            _reasonPhrase = value; // It's OK to have a 'null' reason phrase.
        }
    }

    public override string ToString()
    {
        StringBuilder sb = new StringBuilder();

        sb.Append("StatusCode: ");
        sb.Append((int)StatusCode);

        sb.Append(", ReasonPhrase: '");
        sb.Append(ReasonPhrase ?? "<null>" + "'");

        sb.Append(", ConnectionLog: '");
        sb.Append(_connectionLog.ToString() ?? "<null>" + "'");

        sb.Append(", RemoteFileList: ");
        sb.Append("'" + JsonConvert.SerializeObject(RemoteFiles) + "'");
            
        return sb.ToString();
    }
}
```

    
Whew.  So, what happened here?  This class allows us to return a variety of responses, from OK, to a list of Remote Files, to Bad statuses.  I added some of my own codes to the `FTPStatusCode` enum and corresponding text to the `FTPStatusDescription` class.  Connection Log is used by the SFTP or FTP classes to log information that we might need to return to the database or act on.

Now, it is as simple as the following:

```csharp
public FTPResponseMessage GetFile(string SourceFileName, string LocalDownloadPath)
{
    FTPResponseMessage response = new FTPResponseMessage();

    if (string.IsNullOrEmpty(LocalDownloadPath))
    {
        if (!Directory.Exists(LocalDownloadPath))
        {
            Directory.CreateDirectory(LocalDownloadPath);
        }
    }

    response.AppendConnectionLog(Environment.NewLine + pSourceFileName + " is being downloaded");

    _FTPconn.DownloadFile(LocalDownloadPath, SourceFileName);

    if (File.Exists(Path.Combine(LocalDownloadPath, SourceFileName)))
    {
        if (_workingFTPLocation.DeleteAfterTransfer)
            _FTPconn.DeleteFile(SourceFileName);
    }

    return response;
}
```
        
        
The above method just downloads a file and stores it in the LocalDownloadPath location.  _FTPconn is a wrapper around the SFTP component we use.  We could extend this class to return a negative result status code if the file doesn't exist, but in this context, I'm letting the caller worry about that and take appropriate action.  I want my calls to be as simple and return as quickly as they can.

Great, so where does Polly come in?

To effectively use the Circuit Breakers, we need to have them exist in a Singleton context.  They aren't much good to me otherwise as I need to be able to trap specific FTPResponseMessage statuses.

So, if you are using these in an ASP.NET Core API, here is how you wire up your own clutch of Circuit Breaker policies.

First, you have two options as to how you get your Circuit Breakers: a Policy Registry or Just a POCO that holds all the policies.  For my solution, I'm using a <a href="https://github.com/App-vNext/Polly/wiki/PolicyRegistry" target="_blank">Policy Registry</a>.

```csharp
public class FTPCircuitBreakerPolicyRegistry
{
    public PolicyRegistry Registry { get; } = new PolicyRegistry();
    
    public FTPCircuitBreakerPolicyRegistry()
    {
            
    }

    public void AddCircuitBreakerPolicy(string name , ICircuitBreakerPolicy newPolicy)
    {
        Registry.Add(name, newPolicy);
    }
}
```

Now we have a place to hold our Circuit Breaker policies.

Next, we need to populate the registry.  Handle this in your `StartUp` class.

```csharp
private FTPCircuitBreakerPolicyRegistry GetPolicies()
{
    FTPCircuitBreakerPolicyRegistry registry = new FTPCircuitBreakerPolicyRegistry();

    // We would probably want to go to the database to get these but for now I'm hard-coded
    // a few policies manually to illustrate what I'm going for.

    // Regular Circuit breaker

    CircuitBreakerPolicy<FTPResponseMessage> first = Policy
             .HandleResult<FTPResponseMessage>(f => !f.IsSuccessStatusCode && f.ConnectionLog.Contains("timeout"))
             .CircuitBreaker(2, TimeSpan.FromMinutes(1),OnBreak, OnReset, OnHalfOpen);

    registry.AddCircuitBreakerPolicy("FTPServerA", first);

    // Advanced Circuit breaker
    CircuitBreakerPolicy<FTPResponseMessage> second = Policy
            .HandleResult<FTPResponseMessage>(f => !f.IsSuccessStatusCode && f.ConnectionLog.Contains("connection"))
            .AdvancedCircuitBreaker(.1, TimeSpan.FromSeconds(5), 10,TimeSpan.FromSeconds(30),OnBreak, OnReset, OnHalfOpen);

    registry.AddCircuitBreakerPolicy("FTPServerB", second);

    return registry;
}
```

Now, we have our registry populated.  Notice the OnBreak, OnReset, and OnHalfOpen methods:

```csharp
// This is what happens when we break the circuit
private void OnBreak(DelegateResult<FTPResponseMessage> delegateResult, TimeSpan timespan, Context context)
{
    Console.WriteLine($"\t\t\t\t\tConnection break: {delegateResult.Result.StatusCode}");
}

// This is what happens when the circuit is reset
private void OnReset(Context context)
{
    Console.WriteLine("\t\t\t\t\tConnection reset");
}

// This is what happens when the circuit is half-open
private void OnHalfOpen()
{
    Console.WriteLine("\t\t\t\t\tConnection half open");
}
```

Then all we need to do is work the magic of populating the registry, register the registry, and for the controller to be able to get a handle on it.

```csharp
FTPCircuitBreakerPolicyRegistry registry = GetPolicies();

services.AddSingleton(_ => registry);
```
```csharp
public class FTPController : Controller
{
        
    private readonly IReadOnlyPolicyRegistry<string> _circuitBreakerPolicyRegistry;
    private readonly IMapper _mapper;
    private readonly FTPContext _ftpContext;
    private readonly IConfiguration _config;
    private readonly IConnectionService _connectionService;
    
    public FTPController(IReadOnlyPolicyRegistry<string> circuitbreakerPolicyRegistry, IConfiguration config,
             IMapper mapper, FTPContext ftpContext,IConnectionService connectionSErvice)
    {
        _connectionService = connectionService;
        _circuitBreakerPolicyRegistry = circuitbreakerPolicyRegistry;
        _mapper = mapper;
        _ftpContext = ftpContext;
        _config = config;
           
    }
    
    public async Task<IActionResult> TestConnectionWithFTPInfo([FromBody] FTPLoginTest TestLogin)
    {
        _connectionService.FTPAddress = TestLogin.FTPAddress;
        _connectionService.Port = TestLogin.FTPPort;
        _connectionService.FTPModeTypeCode = TestLogin.FTPModeTypeCode;
        _connectionService.UserID = TestLogin.FTPUserID;
        _connectionService.Password = TestLogin.FTPPassword;
        await _connectionService.SetupConnection();

        var policy = _circuitBreakerPolicyRegistry.Get<IAsyncPolicy<FTPResponseMessage>>(TestLogin.FTPAddress);
        FTPResponseMessage response = await policy.ExecuteAsync(() => _connectionService.TestConnection());
        if (response.IsSuccessStatusCode)
        {
            return Ok(new StringContent(response.ReasonPhrase));
        }
        else
        {
            return new ObjectResult(response.ReasonPhrase) { StatusCode = (int)response.StatusCode };
        }
           
    }
}
```
Once the controller has a handle on the registry, we can pull it out by the FTP address and do an `.ExecuteAsync()` on it.  If something goes wrong the OnBreak, OnReset, and OnHalfOpen methods will handle it.

All the controller methods - putFile, GetFile, etc. are structured like this.

Now, we can hold onto any pending files for upload and handle reconnection to the server automatically.  Right now a human has to monitor our file log table for errors and figure out what they mean.  We've just solved several problems with this approach:
* Cut down on human interaction
* Handle network and remote server problems in an automated and consistent way
* If we put our logging on steroids, we can put a "dashboard" around this so that our DevOps crew has instant access to see how our throughput is performing
* If we get Invalid login errors over our threshold, we can be proactive and reach out to the remote trading partner and help get the problem resolved more quickly without impacting our clients.

That is Polly's Circuit Breakers in a nutshell. There is so much more to explore in Polly.  You could for example further increase resiliency by wrapping policies to take advantage of different strategies.  You could wrap a timeout policy in a retry policy, that itself is wrapped by a fallback policy to help manage timeouts.

If Polly is good enough for <a href="https://www.stevejgordon.co.uk/httpclientfactory-using-polly-for-transient-fault-handling" target="_blank">Microsoft</a>, it is good enough for me!

--Brian