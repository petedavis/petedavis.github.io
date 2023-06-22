---
title: "Orchard Core: Up and Running!"
date: 2017-04-21T12:16:03+09:30
layout: post
authors: ["Peter Davis"]
categories: ["Orchard Core", "Development"]
# description: "Making sure your website runs fast and loads quickly is a fundamental part of the web design and seo process."
thumbnail: "https://source.unsplash.com/dCgbRAQmTQA/640x360"
image: "https://source.unsplash.com/dCgbRAQmTQA/1600x900"
cascade:
  featured_image: "https://source.unsplash.com/dCgbRAQmTQA/1600x900"
---

The foundations of Orchard Core are pretty amazing, and the [performance without caching rendered HTML output is stunning](https://www.youtube.com/watch?v=lSvtxQkaUq4). However getting up and running to develop your own modules and themes has never been simpler now that the [entire CMS feature set is just a set of Nuget packages](https://www.youtube.com/watch?v=WCmfa1gHsQo&t=374s).

If you have never built the Orchard Core project from source, [and tried to follow the demo as presented by Sebastien Ros](https://www.youtube.com/watch?v=WCmfa1gHsQo&t=374s), then you will most likely have hit some nuget package dependency issues.

Here is a quick process to get Orchard Core CMS up and running from the AppVeyour builds.

## Step 1: Create an Empty ASP.NET Core Web Application

First create a new empty ASP.NET Core Web Application.

## Step 2: Install Orchard Core Nuget Packages

> ### Update - 24/04/2017
>
> You should no longer need the other feeds that have the nuget package dependencies for Orchard Core, they have now been mirrored in the orchardcore-preview feed on MyGet. Therefore just use the single orchardcore-preview feed.

Now we need to add NuGet.config file next to your solution file that adds all the required feeds for both the Orchard Core preview packages, and the Orchard Core package dependencies that are also in various forms of Beta release. This is basically the same setup as the Orchard Core source code, except we have added one more feed for built packages.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!-- only use the feeds below -->
    <clear/>
    <!-- Orchard Core dependency feeds -->
    <add key="aspnet-contrib" value="https://www.myget.org/F/aspnet-contrib/api/v3/index.json" />
    <add key="NuGet" value="https://api.nuget.org/v3/index.json" />
    <add key="Orchard2" value="https://www.myget.org/F/orchard2/api/v3/index.json" />
    <add key="icunet" value="https://www.myget.org/F/icu-dotnet/api/v3/index.json" />

    <!-- The Orchard Core CMS packages build by AppVeyor -->
    <add key="OrchardCore" value="https://www.myget.org/F/orchardcore-preview/api/v3/index.json" />
  </packageSources>
</configuration>
```

Now add the OrchardCore.Cms package to your project

```bash
Install-Package OrchardCore.Cms -Prerelease
```

## Step 3: Configure Orchard Core CMS

Edit the Startup.cs to add the OrchardCms to the services collection.

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddOrchardCms();
}
```

Configure the application builder to use the MVC modules.

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    loggerFactory.AddConsole();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseModules();
}
```

## Step 4: Run Orchard Core CMS

Now Press F5 and you should be now at the setup screen for the Orchard Core CMS.
