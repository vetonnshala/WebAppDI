# Classic ASP.NET Web Application (.NET Framework) with Dependency Injection

Before migrating from .NET Framework to .NET Core one of the easiest pre-migration peices of work you can do which will make your life easier is to first do some maintenance and convert the majority of your logic to use both dependency injection and the new logging framework which are prevelant throughout .NET Core.

So this solution is a simple example of how-to to get dependency injection and logging working in an older .NET Framework project using the new Microsoft.Extensions.DependencyInjection and Microsoft.Extensions.Logging packages, both of these nuget packages are multi-targeted and so will work seamlessly when you finally upgrade to .NET Core.

This solution was made in Visual Studio 2019 and consists of;
- WebAppDI - ASP.NET Web Application (.NET Framework) utilising both MVC & Web API
- WebAppDILib - .NET Framework class library.

Before implementing the dependency injection, I did some maintenance on the default projects created from templates;
- I created a backup of all packages.config & web.config.
- Uninstalled all nuget packages from the Web Application project and manually deleted all binding redirects in the web.config file.
- Change Visual Studio to use nuget package reference (see https://docs.microsoft.com/en-us/nuget/reference/migrate-packages-config-to-package-reference)
- Re-installed the nuget packages 1-by-1 into the Web Application until the project compiled (note client libraries like jquery & bootstrap should be re-installed using using libman https://github.com/aspnet/LibraryManager/wiki/Using-LibMan-in-Visual-Studio however I skipped this step for the example)
- Re-added some binding redirects into web.config where explicitly required, using values from the backed-up web.config in the first step.

The general steps I took to get DI working are as follows;
- Added Microsoft.Owin & Microsoft.Owin.Host.SystemWeb
- Added Startup.cs which contains the dependency resolvers for both MVC & Web API.
- Added example MVC and Web API Controllers to both the main Web Application and the class library.
- Created a test "service" to inject into both the Web API & MVC controllers called DITestService along with an interface IDITestService.

The critical code is in Startup.cs and in the Controller constructors which is where the dependency injection magic happens!

Hope this helps...

[![Build Status](https://dev.azure.com/f2calv/github/_apis/build/status/f2calv.WebAppDI?branchName=master)](https://dev.azure.com/f2calv/github/_build/latest?definitionId=1&branchName=master)
