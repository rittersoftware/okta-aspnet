[<img src="https://devforum.okta.com/uploads/oktadev/original/1X/bf54a16b5fda189e4ad2706fb57cbb7a1e5b8deb.png" align="right" width="256px"/>](https://devforum.okta.com/)

[![Support](https://img.shields.io/badge/support-Developer%20Forum-blue.svg)](https://devforum.okta.com/)


Okta ASP.NET middleware
========================

This package will add authentication via Okta to your ASP.NET application. You can follow our instructions below, checkout our examples on [GitHub](https://github.com/okta/okta-aspnet) or jump to our [quickstart](https://developer.okta.com/quickstart/#/okta-sign-in-page/dotnet/aspnet4) to see how to configure your ASP.NET MVC web app or your Web API.


## What you need

* An Okta account (sign up for a [forever-free developer account](https://developer.okta.com/signup/))

## Installing the package

You can install the package by using the NuGet Package Explorer to search for [Okta.AspNet](https://nuget.org/packages/Okta.AspNet).

Or, you can use the `dotnet` command:

```
dotnet add package Okta.AspNet
```


## Using Okta with ASP.NET MVC

For step-by-step instructions, visit the **[Okta ASP.NET MVC quickstart](https://developer.okta.com/quickstart/#/okta-sign-in-page/dotnet/aspnet4)**. The quickstart will guide you through adding Okta login to your ASP.NET application.

You can also download our [sample application](https://github.com/okta/okta-aspnet).

### Usage example

Okta plugs into your OWIN Startup class with the `UseOktaMvc()` method:

```csharp
public class Startup
{
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        app.UseOktaMvc(new OktaMvcOptions
        {
            OrgUrl = "https://{yourOktaDomain}.com",
            ClientId = "{clientId}",
            ClientSecret = "{clientSecret}",
            AuthorizationServerId = "default",
            RedirectUri = "http://localhost:8080/authorization-code/callback",
            PostLogoutRedirectUri = "http://localhost:8080/Home"
        });
    }
}
```

### Configuration

The `OktaMvcOptions` class configures the Okta middleware. It has these options:


| Property                  | Required?    | Details                         |
|---------------------------|--------------|---------------------------------|
| OrgUrl                    | **Yes**      | Your Okta domain, i.e https://dev-123456.oktapreview.com  | 
| ClientId                  | **Yes**      | The client ID of your Okta Application |
| ClientSecret              | **Yes**      | The client secret of your Okta Application |
| RedirectUri               | **Yes**      | The location Okta should redirect to process a login. This is typically `http://{yourApp}/authorization-code/callback`. No matter the value, the redirect is handled automatically by this package, so you don't need to write any custom code to handle this route. |
| AuthorizationServerId     | No           | The Okta Authorization Server to use. The default value is `default`. |
| PostLogoutRedirectUri     | No           | The location Okta should redirect to after logout. If blank, Okta will redirect to the Okta login page. |
| Scope                     | No           | The OAuth 2.0/OpenID Connect scopes to request when logging in. The default value is `openid profile`. |
| GetClaimsFromUserInfoEndpoint | No       | Whether to retrieve additional claims from the UserInfo endpoint after login (not usually necessary). The default value is `false`. |
| ClockSkew                 | No           | The clock skew allowed when validating tokens. The default value is 2 minutes. |

You can store these values in `Web.config`, but take care when checking in the client secret to source control.


### That's it!

Placing the `[Authorize]` attribute on your controllers or actions will check whether the user is logged in, and redirect them to Okta if necessary.

ASP.NET automatically populates `HttpContext.User` with the information Okta sends back about the user. You can check whether the user is logged in with `User.Identity.IsAuthenticated` in your actions or views.

You can also check out an MVC example that uses this package on [GitHub](https://github.com/okta/okta-aspnet/tree/master/Okta.AspNet.Test.Mvc).


## Using Okta with ASP.NET Web API

For step-by-step instructions, visit the **[Okta ASP.NET Web API quickstart](https://developer.okta.com/quickstart/#/widget/dotnet/aspnet4)**. The quickstart will guide you through adding Okta token validation to your ASP.NET Web API.

You can also download our [sample application](https://github.com/okta/okta-aspnet).


### Usage example

Okta plugs into your OWIN Startup class with the `UseOktaWebApi()` method:

```csharp
public class Startup
{
    public void Configuration(IAppBuilder app)
    {
        app.UseOktaWebApi(new OktaWebApiOptions
        {
            OrgUrl = "https://{yourOktaDomain}.com",
            ClientId = "{clientId}",
            AuthorizationServerId = "default"
        });
    }
}
```


### Configuration

The `OktaWebApiOptions` class configures the Okta middleware. It has these options:

| Property                  | Required?    | Details                         |
|---------------------------|--------------|---------------------------------|
| OrgUrl                    | **Yes**      | Your Okta domain, i.e https://dev-123456.oktapreview.com  | 
| ClientId                  | **Yes**      | The client ID of your Okta Application |
| AuthorizationServerId     | No           | The Okta Authorization Server to use. The default value is `default`. |
| Audience                  | No           | The expected audience of incoming tokens. The default value is `api://default`. |
| ClockSkew                 | No           | The clock skew allowed when validating tokens. The default value is 2 minutes. |

You can store these values in `Web.config`.


### That's it!


Placing the `[Authorize]` attribute on your controllers or actions will require a valid access token for those routes. This package will [parse and validate the access token](https://developer.okta.com/blog/2017/06/21/what-the-heck-is-oauth#oauth-flows) and populate `Http.Context` with a limited set of user information.

Check out a minimal example that uses the Okta Sign-In Widget and jQuery in the [`Okta.AspNet.Test.WebApi` project > `Index.html`](https://github.com/okta/okta-aspnet/blob/master/Okta.AspNet.Test.WebApi/Index.html).

*Note*: To test the widget example make sure to add your base URI (i.e http://localhost:8080) as a valid Login Redirect URI in your developer console and, make sure to use the same URI in the browser.

Follow our [quickstart](https://developer.okta.com/quickstart/#/widget/dotnet/aspnet4) to see how to add authentication on other types of clients.

## Contributing

Issues and Pull Requests are welcome! To build the project, clone and build it with Visual Studio 2017 or newer.

## Getting help

If you get stuck or need help, head over to our [Dev Forum](https://devforum.okta.com) or email developers@okta.com.