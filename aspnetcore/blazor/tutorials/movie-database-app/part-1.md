---
title: Build a Blazor movie database app (Part 1 - Create a Blazor Web App)
author: guardrex
description: This part of the Blazor movie database app tutorial explains how to create a Blazor Web App that adopts static server-side rendering (static SSR), where content is only rendered on the server.
monikerRange: '>= aspnetcore-8.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/06/2024
uid: blazor/tutorials/movie-database-app/part-1
zone_pivot_groups: tooling
---
# Build a Blazor movie database app (Part 1 - Create a Blazor Web App)

<!-- UPDATE 9.0 Activate after release

[!INCLUDE[](~/includes/not-latest-version.md)]

-->

This article is the first part of the Blazor movie database app tutorial that teaches you the basics of building an ASP.NET Core Blazor Web App with features to manage a movie database.

This article shows you how to create a Blazor Web App that adopts static server-side rendering (static SSR). Static SSR means that content is rendered on the server and sent to the client for display without the capacity for interplay between the user and .NET/C# code in the browser. JavaScript and HTML DOM events remain unaffected, but user events on the client can't be processed by the .NET runtime running on the server. The rendered UI on the client can only receive and post database data via ordinary HTML requests. In the last part of the tutorial series, *interactivity* is adopted, and Blazor's built-in event processing is demonstrated.

At the end of this tutorial, you'll have an interactive Blazor Web App that manages a database of movies.

## Prerequisites

<!-- REVIEWER NOTE

Let's not cross-link to specific tooling versions to 
avoid content versioning and save on doc maintenance 
costs.

General reminder: As usual for tutorials, there's 
minimal cross-linking to reference docs in the text 
of the series to avoid distracting the reader. We do 
link to API docs in the text. Reference doc 
cross-links go into Additional Resources sections at 
the bottoms of the articles.

-->

:::zone pivot="vs"

<!-- NOTE TO SELF: Confirm this is the link for continued use -->

[Visual Studio (latest release)](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=learn.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2022) with the **ASP.NET and web development** workload

:::zone-end

:::zone pivot="vsc"

Latest releases of:

* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET SDK](https://dotnet.microsoft.com/download/dotnet)

The Visual Studio Code (VS Code) instructions for ASP.NET Core development in this tutorial use the [.NET CLI](/dotnet/core/tools/), which is part of the .NET SDK. .NET CLI commands are issued in VS Code's integrated [**Terminal**](https://code.visualstudio.com/docs/editor/integrated-terminal), which is a [PowerShell command shell](/powershell/).

:::zone-end

:::zone pivot="cli"

[.NET SDK (latest release)](https://dotnet.microsoft.com/download/dotnet)

The [.NET CLI](/dotnet/core/tools/) is part of the .NET SDK. To issue a command that affects the project, open the command shell to the project's root folder to execute the command.

<!-- REVIEWER NOTE

I think a PS command shell would be fine to use
as well. If you want me to add a remark here that 
the dev can use PowerShell for the tutorial, let 
me know. A nice feature of PS is that the dev 
can right-click the project and select 
'Open in Terminal' to get a PS command shell 
for the app without having to fiddle around with
a change directory step.

-->

:::zone-end

Patch releases in .NET roll-forward automatically, so if you have 8.0.0 installed, the app automatically obtains NuGet packages, builds, runs locally, and publishes with the latest 8.0.x patch release.

We recommend avoiding preview tooling and preview framework releases when using .NET and Blazor for the first time, as features may be unstable while the tooling and framework are under construction for the upcoming release. 

## Create a Blazor Web App with static server-side rendering (static SSR)

:::zone pivot="vs"

In Visual Studio:

* Select **Create a new project** from the **Start Window** or select **File** > **New** > **Project** from the menu bar.
* In the **Create a new project** dialog, select **Blazor Web App** from the list of project templates. Select the **Next** button.
* In the **Configure your new project** dialog, name the project `BlazorWebAppMovies` in the **Project name** field, including matching the capitalization. Using this exact project name is important to ensure that the namespaces match for code that you copy from the tutorial into the app that you're building.
* Confirm that the **Location** for the app is suitable. Leave the **Place solution and project in the same directory** checkbox selected. Select the **Next** button.
* In the **Additional information** dialog, use the following settings:
  * **Framework**: Confirm that the [latest framework](https://dotnet.microsoft.com/download/dotnet) is selected. If Visual Studio's **Framework** dropdown list doesn't include the latest available .NET framework, [update Visual Studio](/visualstudio/install/update-visual-studio) and restart the tutorial.
  * **Authentication type**: **None**
  * **Configure for HTTPS**: Selected
  * **Interactive render mode**: **None**
  * **Interactivity location**: **Global**
  * **Include sample pages**: Selected
  * **Do not use top-level statements**: Not selected
  * Select **Create**.

The Visual Studio instructions in parts of this tutorial series use EF Core commands. EF Core commands are issued in the **Package Manager Console** (PMC), which is a [PowerShell command shell](/powershell/). To open the PMC from the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.

:::zone-end

:::zone pivot="vsc"

This tutorial assumes that you have familiarity with VS Code. If you're new to VS Code, see the [VS Code documentation](https://code.visualstudio.com/docs). The videos listed by the [Introductory Videos page](https://code.visualstudio.com/docs/getstarted/introvideos) are designed to give you an overview of VS Code's features.

Confirm that you have the latest [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and [.NET SDK](https://dotnet.microsoft.com/download/dotnet) installed.

In VS Code:

Create a new project:

* Go to the **Explorer** view and select the **Create .NET Project** button. Alternatively, you can bring up the **Command Palette** using <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>, and then type "`.NET`" and find and select the **.NET: New Project** command.

* Select the **Blazor Web App** project template from the list.

* In the **Project Location** dialog, create or select a folder for the project.

* In the **Command Palette**, name the project `BlazorWebAppMovies`, including matching the capitalization. Using this exact project name is important to ensure that the namespaces match for code that you copy from the tutorial into the app that you're building.

* Adjust the project's options by selecting **Show all template options**.

  Change **Interactive render mode** to **None**.

  Change **Interactivity location** to **Global**.

  After the preceding changes are made, the **Command Palette** shows the following:

  * **Framework**: Set to the latest public non-preview release of .NET.
  * **Authentication type**: **None**
  * **Configure for HTTPS**: **true**
  * **Interactive render mode**: **None**
  * **Interactivity location**: **Global**
  * **Include sample pages**: **true**
  * **Do not use top-level statements**: **false**

* Select **Create project** from the **Command Palette**.

<!-- This doesn't seem to be required any longer, but the
     commands are still there to create the debug assets. 
     I need to clarify what these assets provide 
     these days ... is it Hot Reload debugging? Whereas
     without them, the app can only be run. 

* Select **View** > **Command Palette** and type "`.NET`" into the search box. From the list of commands, select the "`.NET: Generate Assets for Build and Debug`" command.
-->

The Visual Studio Code (VS Code) instructions in parts of this tutorial series use the [.NET CLI](/dotnet/core/tools/), which is part of the .NET SDK. .NET CLI commands are issued in VS Code's integrated [**Terminal**](https://code.visualstudio.com/docs/editor/integrated-terminal), which defaults to a [PowerShell command shell](/powershell/). The **Terminal** is opened by selecting **New Terminal** from the **Terminal** menu in the menu bar.

:::zone-end

:::zone pivot="cli"

Confirm that you have the latest [.NET SDK](https://dotnet.microsoft.com/download/dotnet) installed.

In a command shell:

* Change to the directory using the `cd` command to where you want to create the project folder (for example, `cd c:/users/Bernie_Kopell/Documents`).
* Use the [`dotnet new` command](/dotnet/core/tools/dotnet-new) with the [`blazor` project template](/dotnet/core/tools/dotnet-new-sdk-templates#blazor) to create a new Blazor Web App project. The [`-o|--output` option](/dotnet/core/tools/dotnet-new#options) passed to the command creates the project in a new folder named `BlazorWebAppMovies` at the current shell directory location.

  > [!IMPORTANT]
  > Name the project `BlazorWebAppMovies`, including matching the capitalization, so the namespaces match for code that you copy from the tutorial to the app as you follow the tutorial.

  ```dotnetcli
  dotnet new blazor -o BlazorWebAppMovies
  ```

:::zone-end

## Run the app

:::zone pivot="vs"

Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> on the keyboard to run the app without the debugger.

Visual Studio displays the following dialog when a project isn't configured to use SSL:

![Trust self-signed certificate dialog](~/blazor/tutorials/movie-database-app/part-1/_static/trust-certificate.png)

Select **Yes** if you trust the ASP.NET Core SSL certificate.

The following dialog is displayed:

![Security warning dialog](~/blazor/tutorials/movie-database-app/part-1/_static/install-certificate.png)

Select **Yes** to acknowledge the risk and install the certificate.

For information on trusting the development certificate for the Firefox browser, see <xref:security/enforcing-ssl#trust-the-https-certificate-with-firefox-to-prevent-sec_error_inadequate_key_usage-error>.

Visual Studio:

* Compiles and runs the app.
* Launches the default browser at `https://localhost:{PORT}`, which displays the app's UI. The `{PORT}` placeholder is the random port assigned to the app when the app is created. If you need to change the port due to a local port conflict, you may do so in the project's `Properties/launchSettings.json` file.

Navigate the pages of the app to confirm that the app is working normally.

:::zone-end

:::zone pivot="vsc"

For information on trusting the HTTPS certificate for browsers other than Firefox, see the [HTTPS development certificate trust guidance](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos). When using the Firefox browser, see the [certificate trust guidance for Firefox](xref:security/enforcing-ssl#trust-the-https-certificate-with-firefox-to-prevent-sec_error_inadequate_key_usage-error).

In VS Code, press <kbd>Ctrl</kbd>+<kbd>F5</kbd> (Windows) or <kbd>⌘</kbd>+<kbd>F5</kbd> (macOS) to run the app without debugging.

At the **Select debugger** prompt in the **Command Palette** at the top of the VS Code UI, select **C#**. At the next prompt, select the HTTPS profile (`C#: BlazorWebAppMovies [https]`).

The default browser is launched at `https://localhost:{PORT}`, which displays the app's UI. The `{PORT}` placeholder is the random port assigned to the app when the app is created. If you need to change the port due to a local port conflict, you may do so in the project's `Properties/launchSettings.json` file.

Navigate the pages of the app to confirm that the app is working normally.

:::zone-end

:::zone pivot="cli"

For information on trusting the HTTPS certificate for browsers other than Firefox, see the [HTTPS development certificate trust guidance](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos). When using the Firefox browser, see the [certificate trust guidance for Firefox](xref:security/enforcing-ssl#trust-the-https-certificate-with-firefox-to-prevent-sec_error_inadequate_key_usage-error) section of that article.

In a command shell opened to the project's root folder, execute the [`dotnet watch`](/dotnet/core/tools/dotnet-watch) command to compile and start the app:

```dotnetcli
dotnet watch
```

The app is compiled and run. The app is launched at `http://localhost:{PORT}`, where the `{PORT}` placeholder is the random port assigned to the app when the app is created. If you need to change the port due to a local port conflict, you may do so in the project's `Properties/launchSettings.json` file.

Navigate the pages of the app to confirm that the app is working normally.

When an app created from the Blazor Web App project template is run with the .NET CLI, the app runs at an HTTP (insecure) endpoint because the first profile found in the app's launch settings file (`Properties/launchSettings.json`) is the HTTP (insecure) profile, which is named `http`. The HTTP profile was placed in the first position to ease the transition of adopting SSL/HTTPS security for Linux and macOS users.

One approach for running the app with SSL/HTTPS is to pass the [`-lp`|`--launch-profile` option](/dotnet/core/tools/dotnet-run#options) with the `https` profile name to the `dotnet watch` command:

```dotnetcli
dotnet watch -lp https
```

A more robust approach is to simply move the `https` profile above the `http` profile in the `Properties/launchSettings.json` file and save the change. After changing the profile order in the file, the `dotnet watch` command always uses the `https` profile by default.

:::zone-end

<!-- REVIEWER NOTE

I don't assume devs understand their tooling for stopping
an app or that a running app interferes with commands that 
must be issued along the way while working on the app, so 
there are explicit stop instructions at several points 
throughout the series on stopping a running app before
continuing. As the tutorial proceeds, I reduce the 
amount of language used where instructions have the dev
stop the app.

-->

## Stop the app

:::zone pivot="vs"

Stop the app using either of the following approaches:

* Close the browser window.
* In Visual Studio, either:
  * Use the Stop button in Visual Studio's menu bar:

    ![Stop button in Visual Studio's menu bar](~/blazor/tutorials/movie-database-app/part-1/_static/stop-button.png)

  * Press <kbd>Shift</kbd>+<kbd>F5</kbd> on the keyboard.

:::zone-end

:::zone pivot="vsc"

Stop the app using the following approach:

1. Close the browser window.
1. In VS Code, either:
   * From the **Run** menu, select **Stop Debugging**.
   * Press <kbd>Shift</kbd>+<kbd>F5</kbd> on the keyboard.

:::zone-end

:::zone pivot="cli"

Stop the app using the following approach:

1. Close the browser window.
2. In the command shell, press <kbd>Ctrl</kbd>+<kbd>C</kbd> (Windows) or <kbd>⌘</kbd>+<kbd>C</kbd> (macOS).

:::zone-end

## Examine the project files

The following sections contain an overview of the project's folders and files.

If you're building the app, you don't need to make changes to the project files in the following sections. As you read the descriptions of the folders and files, examine them in the project.

If you're only reading the articles and not building the app, you can refer to the completed sample app in the [Blazor samples GitHub repository (`dotnet/blazor-samples`)](https://github.com/dotnet/blazor-samples). Select the latest version folder in the repository. The sample folder for this tutorial's project is named `BlazorWebAppMovies`. Note that the sample app is the *finished version* of the app after following all of the steps of the tutorial series. Code in the sample doesn't always match steps of the tutorial before the end of the series.

### `Properties` folder

The `Properties` folder holds development environment configuration in the `launchSettings.json` file.

### `wwwroot` folder

The `wwwroot` folder contains static assets, such as image, JavaScript (`.js`), and stylesheet (`.css`) files.

### `Components`, `Components/Pages`, and `Components/Layout` folders

These folders contain *Razor components*, often referred to as "components," and supporting files. A component is a self-contained portion of user interface (UI) with optional processing logic. Components can be nested, reused, and shared among projects.

Components are implemented using a combination of C# and HTML markup in [Razor](xref:mvc/views/razor) component files with the `.razor` file extension.

Typically, components that are nested within other components and not directly reachable ("routable") at a URL are placed in the `Components` folder. Components that are routable via a URL are usually placed in the `Components/Pages` folder.

Although the name "Razor components" shares some naming with other ASP.NET Core content-rendering technologies, Razor components must be distinguished from *Razor views*, which are [Razor-based](xref:mvc/views/razor) markup pages for MVC apps, and *View components*, which are for rendering chunks of content rather than whole responses in Razor Pages and MVC apps.

The `Components/Layout` folder contains the following layout components and stylesheets:

* `MainLayout` component (`MainLayout.razor`): The app's main layout component.
* `MainLayout.razor.css`: Stylesheet for the app's main layout.
* `NavMenu` component (`NavMenu.razor`): Implements sidebar navigation. This component uses several `NavLink` components to render navigation links to other Razor components.
* `NavMenu.razor.css`: Stylesheet for the app's navigation menu.

### `_Imports.razor` file

The `_Imports` file (`_Imports.razor`) includes common *Razor directives* to include in the app's Razor components. Razor directives are reserved keywords prefixed with `@` that appear in Razor markup and change the way component markup or component elements are parsed or function.

### `App.razor` file

The `App` component (`App.razor`) is the root component of the app with HTML `<head>` markup, the `Routes` component (covered in the next section), and the Blazor script (`<script>` tag for `blazor.web.js`). The root component is the first component that the app loads.

### `Routes.razor` file

The `Routes` component (`Routes.razor`) sets up routing for the app.

### `appsettings.json` file

The `appsettings.json` file contains configuration data, such as connection strings.

### `Program.cs` file

The `Program.cs` file contains code to create the app and configure the request processing pipeline of the app.

A <xref:Microsoft.AspNetCore.Builder.WebApplicationBuilder> creates the app with preconfigured defaults:

```csharp
var builder = WebApplication.CreateBuilder(args);
```

Razor component services are added to the app by calling <xref:Microsoft.Extensions.DependencyInjection.RazorComponentsServiceCollectionExtensions.AddRazorComponents%2A>, which enables Razor components to render and execute code on the server:

```csharp
builder.Services.AddRazorComponents();
```

The <xref:Microsoft.AspNetCore.Builder.WebApplication> (held by the `app` variable in the following code) is built:

```csharp
var app = builder.Build();
```

Next, the HTTP request pipeline is configured.

In the development environment:

* Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A>) processes errors and displays a developer exception page during development app runs.
* [HTTP Strict Transport Security Protocol (HSTS) Middleware](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts%2A>) processes [HSTS](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html).

```csharp
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error", createScopeForErrors: true);
    app.UseHsts();
}
```

> [!WARNING]
> Don't run a production app in development mode because the developer exception page can leak sensitive information.

HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A>) enforces the HTTPS protocol by redirecting HTTP requests to HTTPS if an HTTPS port is available:

<!-- REVIEWER NOTE

BTW ... Our main doc set guidance is a bit off
on the description of the middleware in a few 
spots where it uses the word "always" when discussing
HTTPS redirection ... i.e., it doesn't qualify it
with 'when there's an HTTPS port available.'
I've made a long-range tracking issue to 
discuss this with the docs cats.

-->

```csharp
app.UseHttpsRedirection();
```

Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A>) serves static files, such as images, scripts, and stylesheets from the `wwwroot` folder:

```csharp
app.UseStaticFiles();
```

Antiforgery Middleware (<xref:Microsoft.AspNetCore.Builder.AntiforgeryApplicationBuilderExtensions.UseAntiforgery%2A>) enforces antiforgery protection for form processing:

```csharp
app.UseAntiforgery();
```

<xref:Microsoft.AspNetCore.Builder.RazorComponentsEndpointRouteBuilderExtensions.MapRazorComponents%2A> maps components defined in the root `App` component to the given .NET assembly and renders routable components:

```csharp
app.MapRazorComponents<App>();
```

The app is run by calling <xref:Microsoft.AspNetCore.Builder.WebApplication.Run%2A> on the <xref:Microsoft.AspNetCore.Builder.WebApplication> (`app`):

```csharp
app.Run();
```

## Troubleshoot with the completed sample

[!INCLUDE[](~/blazor/tutorials/movie-database-app/includes/troubleshoot.md)]

## Next steps

> [!div class="step-by-step"]
> [Next: Add and scaffold a model](xref:blazor/tutorials/movie-database-app/part-2)