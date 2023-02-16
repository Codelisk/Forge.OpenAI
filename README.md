# Forge.OpenAI
OpenAI API client library for .NET. This is not an official library, I was developed it for myself, for public and it is free to use.

## Installing

To install the package add the following line to you csproj file replacing x.x.x with the latest version number:

```
<PackageReference Include="Forge.OpenAI" Version="x.x.x" />
```

You can also install via the .NET CLI with the following command:

```
dotnet add package Forge.OpenAI
```

If you're using Visual Studio you can also install via the built in NuGet package manager.

## Setup

By default, this library uses Microsoft Dependency Injection, however it is not necessary.

You can register the client services with the service collection in your _Startup.cs_ / _Program.cs_ file in your application.

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddForgeOpenAI(options => {
        options.AuthenticationInfo = Configuration["OpenAI:ApiKey"]!;
    });
}
``` 

Or in your _Program.cs_ file.

```c#
public static async Task Main(string[] args)
{
    var builder = WebAssemblyHostBuilder.CreateDefault(args);
    builder.RootComponents.Add<App>("app");

    builder.Services.AddForgeOpenAI(options => {
        options.AuthenticationInfo = builder.Configuration["OpenAI:ApiKey"]!;
    });

    await builder.Build().RunAsync();
}
```

Or

```c#
public static async Task Main(string[] args)
{
    using var host = Host.CreateDefaultBuilder(args)
        .ConfigureServices((builder, services) =>
        {
            services.AddForgeOpenAI(options => {
                options.AuthenticationInfo = builder.Configuration["OpenAI:ApiKey"]!;
            });
        })
        .Build();
}
```

You should provide your OpenAI API key and optionally your organization to boot up the service.
If you do not provide it in the configuration, service automatically lookup the necessary
information in your environment variables, in a Json file (.openai) or
in an environment file (.env).

Example for environment variables:

OPENAI_KEY or OPENAI_API_KEY or OPENAI_SECRET_KEY or TEST_OPENAI_SECRET_KEY are checked for the API key

ORGANIZATION key checked for the organzation


Example for Json file:

{
    "apikey": "your_api_key",
    "organization": "organization_id"
}


Environment file must contains key/value pairs in this format {key}={value}

For the '_key_', use one of the same value which described in Environment Variables above.

Example for environment file:

OPENAI_KEY=your_api_key

ORGANIZATION=optionally_your_organization


## Options

OpenAI and the dependent services require OpenAIOptions, which can be provided manually or it will happened,
if you use dependency injection. If you need to use multiple OpenAI service instances at the same time,
you should provide this options individually with different settings and authentication credentials.

In the options there are many Uri settings, which was not touched normally. The most important option
is the AuthenticationInfo property, which contains the ApiKey and and Organization Id.

Also, there is an additional option, called HttpMessageHandlerFactory, which constructs the HttpMessageHandler
for the HttpClient in some special cases, for example, if you want to override some behavior of the HttpClient.

There is a built-in logging feature, just for testing and debugging purposes, called LogRequestsAndResponses,
which persists all of requests and responses in a folder (LogRequestsAndResponsesFolder). With this feature,
you can check the low level messages. I do not recommend to use it in production environment.


## Examples

If you would like to learn more about the API capabilities, please visit https://platform.openai.com/docs/api-reference
If you need to generate an API key, please visit: https://platform.openai.com/account/api-keys

I have created a playground, which is part of this solution. It covers all of the features, which this library provides.
Feel free to run through these examples and play with the settings.

Also here is the OpenAI playground, where you can also find examples about the usage:
https://platform.openai.com/playground/p/default-chat?lang=node.js&mode=complete&model=text-davinci-003

