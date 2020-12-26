﻿# Challonge.NET

Challonge.NET is a C# implementation of the [CHALLONGE! API] (https://api.challonge.com/v1) 
built on .NET. It is based on the [Typed Client] (https://docs.microsoft.com/en-us/aspnet/core/fundamentals/http-requests?view=aspnetcore-5.0#typed-clients)
pattern and exposes asynchronous methods for interacting with the API.

## Requirements

- .NET 5.0

## Installation

The library is available through NuGet. Install as you would any other package.

## Usage

#### Console Application

Add these `using` directives to `Program.cs`:

```C#
using Challonge.Api;
using Challonge.Objects;
```
Create the client and use its methods:

```C#
class Program
{
    private static readonly HttpClient _httpClient = new();
    private static readonly ChallongeCredentials _credentials = new("username", "apiKey");
    private static readonly ChallongeClient _client = new(_httpClient, _credentials);
	
    static void Main(string[] args)
    {
        var tournaments = _client.GetTournamentsAsync().Result;
    }
}
```
#### ASP.NET Core Web Application

Challonge.NET supports dependency injection using the built-in dependency injection framework.

In `Startup.cs`, add:
```C#
using Challonge.Extensions.DependencyInjection;
```
Then, in `ConfigureServices`:
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddChallonge("username", "apiKey");
    // Consider storing your credentials outside your source code 
    // (e.g. as environment variables, in a configuration file, etc.)
}
```
Finally, add the appropriate `using` directives and inject the client into your controllers:

```C#
public class HomeController : Controller
{
    private readonly IChallongeClient _client;

    public HomeController(IChallongeClient client)
    {
        _client = client;
    }
}
```
You can also inject `IChallongeCredentials` to access the username and api key being used by the client.