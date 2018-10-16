# lightstep-tracer-csharp
The LightStep distributed tracing library for C#

[![NuGet](https://img.shields.io/nuget/v/LightStep.svg)](https://www.nuget.org/packages/LightStep) [![CircleCI](https://circleci.com/gh/lightstep/lightstep-tracer-csharp.svg?style=svg)](https://circleci.com/gh/lightstep/lightstep-tracer-csharp)

# Development Requirements
- C# 7
- .NET Core 2+
- .NET 4.5

This translates to requiring at least Visual Studio 2017 (15.0).
You may need [PostSharp](https://www.postsharp.net/) to work on the `LightStep.CSharpAspectTestApp`.

# Installation
Install the package via NuGet into your solution, or use `Install-Package LightStep`.

# Basic Usage
It's recommended to initialize the tracer once at the beginning of your application and assign it as the global tracer, as follows:
```c#
// substitute your own LS API Key here
var lsKey = "TEST_TOKEN";

// substitute your satellite endpoint (host, port) here.
var lsSettings = new SatelliteOptions("localhost", 9996, false);

// create a new tracer and register it
var tracer = new Tracer(new Options(lsKey, lsSettings));
GlobalTracer.Register(tracer);
...
```

Once you've initialized a tracer, you can begin to create and emit spans.

Please refer to the [OpenTracing C# documentation](https://github.com/opentracing/opentracing-csharp) for information on how to create spans.

You can also refer to the `examples` folder for sample code and projects. 

# Advanced Usage

There's several options that can be adjusted when instantiating a `LightStepTracer`.

## `Options`
| Property | Description |
| -------- | ----------- |
| Tags   | Default tags to apply to all spans created by the tracer.  |
| ReportPeriod  | How frequently the Tracer should batch and send Spans to LightStep (30s default) |
| ReportTimeout  | Timeout for sending spans to the Satellite  |
| AccessToken | The LightStep Project Access Key |
| Satellite | A SatelliteOptions object that specifies the host, port, and if we should use HTTPS |
| UseHttp2 | If this is true, we use HTTP/2 to communicate with the Satellite |

## `SatelliteOptions`
| Property | Description |
| -------- | ----------- |
| SatelliteHost | The hostname of a Satelite (i.e., `collector.lightstep.com`)
| SatellitePort | The port number where the Satellite is listening for HTTP traffic (defaults to 443)
| UsePlaintext | Should we use HTTP or HTTPS traffic? (Defaults to HTTP)

The C# Tracer _only_ supports Protobuf over HTTP/S, so no options to modify the transport are available.

The C# Tracer will prefer TLS 1.2 when available on all .NET Runtime versions, but should fall back to TLS 1.1 or 1.0 in that order.
