# Rate limiting and best practices

As a general principle, your application should limit the number of messages it posts into an individual chat or channel conversation.  This ensures an optimal experience that does not feel “spammy” to your end users.

To protect Microsoft Teams, the bot APIs rate limit incoming requests.  Apps that go over this limit will receive and `HTTP 429 Too Many Requests` error status.  All requests are subject to the same rate limiting policy, including sending messages, channel enumeration, and roster fetches.
Because the exact values of rate limits are subject to change, we recommend your application implement the right back-off behavior when Teams API returns `HTTP 429 Too Many Requests`.

### Hitting rate limits

When issuing a bot framework SDK operation, you can handle  `Microsoft.Rest.HttpOperationException` and check for the status code.
```csharp
try
{
    // Perform Bot framework operation 
    // for example, await connector.Conversations.UpdateActivityAsync(reply);
}
catch (HttpOperationException ex)
{
    if (ex.Response != null && (uint)ex.Response.StatusCode ==  429)
    {
        //Perform retry of the above operation/Action method
    }
}
```

### Best practices
In general, you should take simple precautions to avoid hitting `HTTP 429` responses.  For instance, avoid issuing multiple requests to the same 1:1 or channel conversation. Instead, consider batching the API requests.

Using an exponential backoff with a random jitter is the recommended way to handle 429s.  This would ensure that multiple requests do not introduce collisions on retries. Following are some of the ways you can handle transient errors.

### Example backoff algorithm 1
Here is a sample using exponential backoff via Transient Fault Handling Application Block 

You can perform backoff and retries using [Transient Fault Handling libraries](https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx).  Add the Transient Fault Handling Application Block to Your Solution – follow the msdn guidelines for obtaining and installing the [Nuget package](https://msdn.microsoft.com/en-us/library/hh680891(v=pandp.50).aspx).

```csharp
public class BotSdkTransientExceptionDetectionStrategy : ITransientErrorDetectionStrategy
{
    // List of error codes on retry on
    List<int> transientErrorStatusCodes = new List<int>() { 429 };

    public bool IsTransient(Exception ex)
    {
        var httpOperationException = ex as HttpOperationException;
        if (httpOperationException != null)
        {
            return httpOperationException.Response != null &&
                    transientErrorStatusCodes.Contains((int) httpOperationException.Response.StatusCode);
        }

        return false;
    }
}
```

### Example backoff algorithm 2
Here is another example that Performing an exponential backoff with a randomized jitter.

```csharp
/**
* The first parameter specifies the number of retries before failing the operation.
* The second parameter specifies the minimum and maximum backoff time respectively.
* The last parameter is used to add a randomized  +/- 20% delta to avoid numerous clients all retrying simultaneously.
*/
var exponentialBackoffRetryStrategy = new ExponentialBackoff(3, TimeSpan.FromSeconds(2),
                        TimeSpan.FromSeconds(20), TimeSpan.FromSeconds(1));


// Define the Retry Policy
var retryPolicy = new RetryPolicy(new BotSdkTransientExceptionDetectionStrategy(), fixedIntervalRetryStrategy);

//Execute any bot sdk action
await retryPolicy.ExecuteAsync(() => connector.Conversations.ReplyToActivityAsync((Activity)reply)).ConfigureAwait(false);
```

You can also perform a `System.Action` method execution with the retry policy described above.  The library mentioned also allows you for specifying a fixed interval or a linear backoff mechanism. 
You can also have the value and strategy read from a configuration file to make it easy to fine tune and tweak values in runtime. 

Check out this [handy guide](https://docs.microsoft.com/en-us/azure/architecture/patterns/retry) on various retry patterns.

