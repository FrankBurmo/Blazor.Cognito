# Blazor.Cognito

Is a library for using AWS Cognito in Blazor SPAs.

The idea behind this is to have an easy way of using AWS Cognito with Blazor (especially the client side) without relaying on javascript libraries.

[![Nuget](https://img.shields.io/nuget/v/Blazor-Auth0-ServerSide?color=green&label=Nuget%3A%20Blazor-Auth0-ServerSide)](https://www.nuget.org/packages/Blazor-Auth0-ServerSide)
[![Nuget](https://img.shields.io/nuget/v/Blazor-Auth0-ClientSide?color=green&label=Nuget%3A%20Blazor-Auth0-Clientside)](https://www.nuget.org/packages/Blazor-Auth0-ClientSide)
[![Github Actions](https://github.com/henalbrod/Blazor.Auth0/workflows/Github%20Actions%20CI/badge.svg)](https://github.com/henalbrod/Blazor.Auth0/actions)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/675aa31ceb9a4898be281c23423ca134)](https://www.codacy.com/manual/henalbrod/Blazor.Auth0?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=henalbrod/Blazor.Auth0&amp;utm_campaign=Badge_Grade)
[![GitHub license](https://img.shields.io/github/license/henalbrod/Blazor.Auth0?color=blue)](https://github.com/henalbrod/Blazor.Auth0/blob/master/LICENSE)


# About Cognito
Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0.

Learn more at:
https://aws.amazon.com/cognito/

## Prerequisites

### Blazor

>You'll want to follow the [Getting Started](https://docs.microsoft.com/en-us/aspnet/core/blazor/get-started?view=aspnetcore-3.0&tabs=visual-studio) instructions in [Blazor website](https://blazor.net)

### Auth0

> Basic knowledge of Auth0 IDaaS platform is assumed, otherwise, visiting [Auth0 docs](https://auth0.com/docs) is highly recommended.

## Installation

Install via [Nuget](https://www.nuget.org/).

>Server Side
```bash
Install-Package Blazor-Auth0-ServerSide -Version 2.0.0-Preview4
````

>Client Side
```bash
Install-Package Blazor-Auth0-ClientSide -Version 2.0.0-Preview4
````

## Usage

 **Note**: Following example is for a server-side with require authenticated user implementation, for client-side and core-hosted example implementations please refer to the [examples](https://github.com/henalbrod/Blazor.Auth0/tree/master/examples)

> #### appsettings.json or [Secrets file](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-3.0&tabs=windows#secret-manager) (recommended)

```Json
{
	"Auth0":{
		"Domain": "[Your_Auth0_Tenant_Domain]",
		"ClientId": "[Your_Auth0_Client/Application_Id]",
		"ClientSecret": "[Your_Auth0_Client/Application_Secret]",
		"Audience": "[Your_Auth0_Audience/API_Identifier]"
	}
}
```
> #### Startup.cs

```C#
// Import Blazor.Auth0
using Blazor.Auth0;
using Blazor.Auth0.Models;
// ...

public void ConfigureServices(IServiceCollection services)
{
	// Other code...

	/// This one-liner will initialize Blazor.Auth0 with all the defaults
	services.AddDefaultBlazorAuth0Authentication(Configuration.GetSection("Auth0").Get<ClientOptions>());	

	// Other code...
}

 public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
 {
    // Otrher code...

	app.UseHttpsRedirection();
	app.UseStaticFiles();
     
	// Add Blazor.Auth0 middleware     
	app.UseBlazorAuth0();

	// Other code...
 }
```

###
Replace App.razor content with the following code
> #### App.razor

```HTML
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <Authorizing>
                <p>>Determining session state, please wait...</p>
            </Authorizing>
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page. You may need to log in as a different user.</p>
            </NotAuthorized>
        </AuthorizeRouteView>
    </Found>
    <NotFound>        
        <p>Sorry, there's nothing at this address.</p>        
    </NotFound>
</Router>
```
## Support
If you found a bug, have a consultation or a feature request please feel free to [open an issue](https://github.com/henalbrod/Blazor.Auth0/issues).

When opening issues please take in account to:

* **Avoid duplication**: Please search for similar issues before.
* **Be specific**: Please don't put several problems/ideas in the same issue.
* **Use short descriptive titles**: You'll have the description box to explain yourself.
* **Include images whenever possible**: A picture is worth a thousand words.
* **Include reproduction steps for bugs**:  Will be appreciated

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

1.  Fork it ([https://github.com/henalbrod/Blazor.Auth0/fork](https://github.com/henalbrod/Blazor.Auth0/fork))
2.  Create your feature branch (`git checkout -b feature/fooBar`)
3.  Commit your changes (`git commit -am 'Add some fooBar'`)
4.  Push to the branch (`git push origin feature/fooBar`)
5.  Create a new Pull Request

* Especial thanks for its help to all the [contributors](https://github.com/henalbrod/Blazor.Auth0/graphs/contributors)

## Authors
**Henry Alberto Rodriguez** - _Initial work_ - [GitHub](https://github.com/henalbrod) -  [Twitter](https://twitter.com/henalbrod)  - [Linkedin](https://www.linkedin.com/in/henalbrod/)

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/henalbrod/Blazor.Auth0/blob/master/LICENSE) file for details.

## Acknowledgments

* I started this library based on [this great post](https://auth0.com/blog/oauth2-implicit-grant-and-spa/) from [Vittorio Bertocci](https://twitter.com/vibronet)

* This README file is based on the great examples form: [makeareadme](https://www.makeareadme.com/), [PurpleBooth](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2) & [dbader](https://github.com/dbader/readme-template/blob/master/README.md)

## Release History

**v2.0.0-Preview4**

* Upgraded to .Net Core v3.1.0-preview3

**v2.0.0-Preview3**

* Upgraded to .Net core 3.1.0-preview2


**v2.0.0-Preview2**

This relase comes with Client Side changes primarly

* New LoginMode parameter in ClientOptions 

  Redirect = Classic behavior (default)
  PopUp = Loads Universal Login inside a popup window
  
  The new PopUp behavior comes in handy to avoid the full client side app reloading

* New AuthorizePopup method in Blazor.Auth0.Authentication for client side

**v2.0.0-Preview1**

BREAKING CHANGES:

* Upgraded to .Net Core 3.1.0-preview1
* Server side projects upgraded to netcoreapp3.1
* Auth0 permissions are now accesible as an any other array claim:
```C#
policy.RequireClaim("permissions", "permission_name")
```

**v1.0.0-Preview3**
* Overall upgrade to .Net Core 3.0

**v1.0.0-Preview2**
* Overall upgrade to .Net Core 3.0 RC1
* Removed Shell.razor in Example projects
* Simplified App.razor in Example projects
* Removed local _imports.razor in Example projects

**v0.1.0.0-Preview1**
* Upgraded to .Net Core 3.0.0-preview8
* Removed AuthComponent
* New One-Liner instantiation
* Server Side full rewrite
	* Better server-side Blazor Authentication compatibility/integration
	* Cookie-based session (No more silent login iframe in server-side)
	* Refresh token support (Refreshing and Revoking)
	* Client secret
	* Server-side sliding expiration
