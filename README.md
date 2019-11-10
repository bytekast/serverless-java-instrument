# Serverless Java Instrument Plugin (Alpha)

This [Serverless](https://github.com/serverless/serverless) plugin auto-instruments JVM-based package artifacts specified in the `package.artifact` configuration of the `serverless.yml` 

For now, the only thing the plugin does is inject additional logging post function invocation. It does this via ***bytecode instrumentation*** to your Serverless function without additional source code changes on your part.

Example post-invocation log output of an instrumented function:
```
START RequestId: 024cecab-ba19-40c7-acae-2d2f4695edbe Version: $LATEST
2019-11-10 20:35:08 <024cecab-ba19-40c7-acae-2d2f4695edbe> INFO  com.serverless.Handler:31 - received: {}
{
  "threadId" : 1,
  "context" : {
    "awsRequestId" : "024cecab-ba19-40c7-acae-2d2f4695edbe",
    "functionName" : "jtest-dev-hello",
    "functionVersion" : "$LATEST",
    "identity" : {
      "identityId" : "",
      "identityPoolId" : ""
    },
    "invokedFunctionArn" : "arn:aws:lambda:us-east-1:xxxxxxxxxx:function:jtest-dev-hello",
    "logGroupName" : "/aws/lambda/jtest-dev-hello",
    "logStreamName" : "2019/11/10/[$LATEST]e1e60c451d4141c0b881a9fb403a7b1c",
    "logger" : { },
    "memoryLimitInMB" : 1024,
    "remainingTimeInMillis" : 5374
  },
  "methodName" : "handleRequest",
  "className" : "com.serverless.Handler",
  "durationMs" : 566
}
END RequestId: 024cecab-ba19-40c7-acae-2d2f4695edbe
REPORT RequestId: 024cecab-ba19-40c7-acae-2d2f4695edbe  Duration: 682.41 ms     Billed Duration: 700 ms Memory Size: 1024 MB    Max Memory Used: 106 MB Init Duration: 430.46 ms      
```

> Additional logging, metrics and insights will be added in the future!


## Installation

First, add Serverless Java Instrument to your project:

`serverless plugin install --name serverless-java-instrument`

Then inside your project's `serverless.yml` file verify the following entry to the plugins section: `serverless-java-instrument`.

It should look something like this:

```YAML
plugins:
  - serverless-java-instrument
```

## Usage

You must have Java 8 installed. To install Java 8, see [SDKMAN](https://sdkman.io/).

At its current state, the plugin will instrument any method where the the last parameter is of type `com.amazonaws.services.lambda.runtime.Context`. See the [AWS Documentation](https://docs.aws.amazon.com/lambda/latest/dg/java-programming-model-handler-types.html) for building Lambda handlers in Java.

The instrumentation occurs automatically either after `serverless package` or before `serverless deploy`.

