# Errors and Error Handling in JavaScript

* Author: Mahdhi Rezvi
* Link: https://blog.bitsrc.io/errors-and-error-handling-in-javascript-52d448b8183d

7 types of native javascript errors
reference: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error 
Internal error: not standardized yet.

programmer mistakes -> bugs
genuine problems -> failures

why error handling
1. users have no idea when error happens.
Aways assume users are stupid. The way we handle user errors is to categorize errors into system errors (including bugs and failures) and user errors (user did something unexpected, e.g. malformed data input). We always give meaning error messages for the 2nd case, so user knows how to correct their behavior. And for the 1st case, we sometimes give suggestions, e.g. try again or wait for a few minutes, or show a general message like "oops, something went wrong", together with how to contact css, debug info, e.g. session, request ids, timestamp, cluster info or stack trace if possible. So customers can contact our CSS if they need further assistance. And devs can use those GUID to track what exactly happened during that window (with extra info from the telemetry system).
2. devs need to log errors.
Not only for debug and repro, but also those exceptions need to be logged into telemetry. So we can have a report to show most frequent errors each week for weekly service review. And devs can use the GUIDs provided by customers for further investigations (super useful when on-call)

How to error handling
1. try-catch
note: the errors will be swallowed in the catch block if you dont throw it again.
2. promise catch
note: one thing I noticed was, the exception message would be wrapper with extra words like "from promises", which is annoying when testing exception in jasmine.
BTW, should the author also mention RxJS?
3. Callback error
NodeJS
4. error event
NodeJS
5. onError
Native JS (3,4 and 5 seems to be the same thing?)
6. Error boundaries
Learning React now. Seems to be some builtin error handling support from React.

Conclusion:
Nothing fancy. As this article already mentioned, error handling should be an area of expertise of EVERY developer.