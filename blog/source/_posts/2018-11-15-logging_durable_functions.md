layout: post
title: Durable Functions Logging Best Practices
authorId: simon_timms
date: 2018-11-15
---

[Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview) provide a way to run a series of functions in an orchestrated concert*. The product is relatively new and it is difficult to find a whole lot of guidance on how to do things with it. I found logging to be an area where some guidance was needed. So I'm writing some.

<!--more-->

First up let me say that this is a work in progress. If you disagree with anything please comment. If you have additional ideas please comment. I want this to be a discussion because, as my wife keeps reminding me, I don't know everything. Disclaimers aside let's get into it. 

1. Use the ILogger which is injected into your functions. This logger can add all sorts of great contextual information to your log messages behind the scene and it is right there for you.

2. Use App Insights. There are other ways to get and the logs for functions but App Insights gives you a search engine, alerting, filtering,... It is also really [easy to enable](https://docs.microsoft.com/en-us/azure/azure-functions/functions-monitoring) there is even an option to turn in on when you create new functions app.

3. Use the appropriate log level. Different log levels exist for a reason. When you're logging to app insights the error messages actually go to a different schema from the regular log messages so it is really easy to pick out when exceptions occur. 

4. Log very little in the Orchestrator. Every time your function enters the orchestrator it re-runs everything skipping the remote function calls which have already been run. This means that if you have an orchestrator which is called frequently then you're going to get a log entry for every single entry to the function. This clutters up your logs and doesn't add anything of value.

Here is an orchestrator function used to search for where somebody has hidden cookies. The log entries on lines 7 and 9 are going to appear in the logs with a very high frequency because this function will be run at least as many times as there are cookie locations + once for getting the possible cookie locations. The log entry on line 20, however, will appear only once. The reason is that each time an external function returns the orchestrator is run again to allow it to determine what actions it would like to take as a result of the returning function.

```csharp
[FunctionName("ScatterDemo")]
public async static Task RunOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context,
    [Inject]IConfigService configService,
    ILogger log)
{
    log.LogInformation("Starting to process cookie retrieval");
    var cookieLocations = (await configService.GetPossibleCookieLocations());
    log.LogInformation("Going to process {count} locations", cookieLocations.Count());
    foreach (var tuple in cookieLocations)
    {
        var (location, probabilityOfCookies) = tuple;
            tasks.Add(context.CallActivityWithRetryAsync<string>("ScatterDemo_SearchcookieLoc",
                new RetryOptions(firstRetryInterval: TimeSpan.FromSeconds(5),
                                    maxNumberOfAttempts: 3),
            (location)));
        await Task.WhenAll(tasks);
    }
    await context.CallActivityAsync<string>("EatCookies", recalculatedCookieLocations);
    log.LogInformation($"Done eating all cookies");
}
```


5. Log on entry to and exit from the functions triggered from the orchestrator. You still get the log entry you would have got by logging in the orchestrator but only once now.

6. If your function fans out log the parameters in every log message from the workers. It is possible to correlate together log statements from a single function execution by filtering by the operation id. However it is difficult to find that operation id in the first place without having the parameters to search on. 

![Durable Functions logs in app insights by operation id](/images/durable_functions/operationid.png)

The parameters can be put into the log function's parameters list to take advantage of structured logging but that means you won't be able to easily find them in the streaming logs. Of course App Insights understands these parameters and allows you to build a query targeting the specific fields from the structured logging. Personally I put the parameters both in the message and in the parameters array via a log template like

```csharp
log.LogInformation("Going to process {count} locations", cookieLocations.Count());
```
The value appears in the custom dimensions in app insights and in the full log message.


7. Don't be a afraid to log a bunch. There are very few cases where I feel like I've logged too much and countless cases where I find myself redeploying a function just to add some additional logging. 

That's what I came up with but I'd love to hear thought on all of this. 


*Wasn't that a great metaphor? 