---
title: Tutorial - .NET Web app accesses Microsoft Graph as the user | Azure
description: In this tutorial, you learn how to access data in Microsoft Graph for a signed-in user from a .NET web app.
services: microsoft-graph, app-service-web
author: rwike77
manager: CelesteDG

ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 03/08/2022
ms.author: ryanwi
ms.reviewer: stsoneff
ms.devlang: csharp
ms.custom: azureday1
#Customer intent: As an application developer, I want to learn how to access data in Microsoft Graph for a signed-in user.
---

# Tutorial: Access Microsoft Graph from a secured .NET app as the user

[!INCLUDE [clean up resources](./includes/tutorial-connect-app-access-microsoft-graph-as-user/intro.md)]

## Call Microsoft Graph with .NET

Your web app now has the required permissions and also adds Microsoft Graph's client ID to the login parameters.

Using the [Microsoft.Identity.Web library](https://github.com/AzureAD/microsoft-identity-web/), the web app gets an access token for authentication with Microsoft Graph. In version 1.2.0 and later, the Microsoft.Identity.Web library integrates with and can run alongside the App Service authentication/authorization module. Microsoft.Identity.Web detects that the web app is hosted in App Service and gets the access token from the App Service authentication/authorization module. The access token is then passed along to authenticated requests with the Microsoft Graph API.

To see this code as part of a sample application, see the: 
* [Sample on GitHub](https://github.com/Azure-Samples/ms-identity-easyauth-dotnet-storage-graphapi/tree/main/2-WebApp-graphapi-on-behalf).

> [!NOTE]
> The Microsoft.Identity.Web library isn't required in your web app for basic authentication/authorization or to authenticate requests with Microsoft Graph. It's possible to [securely call downstream APIs](tutorial-auth-aad.md#call-api-securely-from-server-code) with only the App Service authentication/authorization module enabled.
> 
> However, the App Service authentication/authorization is designed for more basic authentication scenarios. For more complex scenarios (handling custom claims, for example), you need the Microsoft.Identity.Web library or [Microsoft Authentication Library](../active-directory/develop/msal-overview.md). There's a little more setup and configuration work in the beginning, but the Microsoft.Identity.Web library can run alongside the App Service authentication/authorization module. Later, when your web app needs to handle more complex scenarios, you can disable the App Service authentication/authorization module and Microsoft.Identity.Web will already be a part of your app.

### Install client library packages

Install the [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) and [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph) NuGet packages in your project by using the .NET Core command-line interface or the Package Manager Console in Visual Studio.

#### .NET Core command line

Open a command line, and switch to the directory that contains your project file.

Run the install commands.

```dotnetcli
dotnet add package Microsoft.Identity.Web.MicrosoftGraph

dotnet add package Microsoft.Identity.Web
```

#### Package Manager Console

Open the project/solution in Visual Studio, and open the console by using the **Tools** > **NuGet Package Manager** > **Package Manager Console** command.

Run the install commands.
```powershell
Install-Package Microsoft.Identity.Web.MicrosoftGraph

Install-Package Microsoft.Identity.Web
```

### Startup.cs

In the *Startup.cs* file, the ```AddMicrosoftIdentityWebApp``` method adds Microsoft.Identity.Web to your web app. The ```AddMicrosoftGraph``` method adds Microsoft Graph support.

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Identity.Web;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

// Some code omitted for brevity.
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
                .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"))
                    .EnableTokenAcquisitionToCallDownstreamApi()
                        .AddMicrosoftGraph(Configuration.GetSection("Graph"))
                        .AddInMemoryTokenCaches();

        services.AddRazorPages();
    }
}

```

### appsettings.json

*AzureAd* specifies the configuration for the Microsoft.Identity.Web library. In the [Azure portal](https://portal.azure.com), select **Azure Active Directory** from the portal menu and then select **App registrations**. Select the app registration created when you enabled the App Service authentication/authorization module. (The app registration should have the same name as your web app.) You can find the tenant ID and client ID in the app registration overview page. The domain name can be found in the Azure AD overview page for your tenant.

*Graph* specifies the Microsoft Graph endpoint and the initial scopes needed by the app.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "fourthcoffeetest.onmicrosoft.com",
    "TenantId": "[tenant-id]",
    "ClientId": "[client-id]",
    // To call an API
    "ClientSecret": "[secret-from-portal]", // Not required by this scenario
    "CallbackPath": "/signin-oidc"
  },

  "Graph": {
    "BaseUrl": "https://graph.microsoft.com/v1.0",
    "Scopes": "user.read"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

### Call Microsoft Graph on behalf of the user

The following example shows how to call Microsoft Graph as the signed-in user and get some user information. The ```GraphServiceClient``` object is injected into the controller, and authentication has been configured for you by the Microsoft.Identity.Web library.

```csharp
// Index.cshtml.cs
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Graph;
using System.IO;
using Microsoft.Identity.Web;
using Microsoft.Extensions.Logging;

// Some code omitted for brevity.

[AuthorizeForScopes(Scopes = new[] { "user.read" })]
public class IndexModel : PageModel
{
    private readonly ILogger<IndexModel> _logger;
    private readonly GraphServiceClient _graphServiceClient;

    public IndexModel(ILogger<IndexModel> logger, GraphServiceClient graphServiceClient)
    {
        _logger = logger;
        _graphServiceClient = graphServiceClient;
    }

    public async Task OnGetAsync()
    {
        try
        {
            var user = await _graphServiceClient.Me.Request().GetAsync();
            ViewData["Me"] = user;
            ViewData["name"] = user.DisplayName;

            using (var photoStream = await _graphServiceClient.Me.Photo.Content.Request().GetAsync())
            {
                byte[] photoByte = ((MemoryStream)photoStream).ToArray();
                ViewData["photo"] = Convert.ToBase64String(photoByte);
            }
        }
        catch (Exception ex)
        {
            ViewData["photo"] = null;
        }
    }
}
```


[!INCLUDE [second-part](./includes/tutorial-connect-app-access-microsoft-graph-as-user/end.md)]