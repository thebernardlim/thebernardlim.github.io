---
layout: post
title: .NET Core HTTP 500 (500.19) Internal Server error
author: Bernard Lim
date: 2022-04-02 00:04:40 +0530
subtitle: How to resolve HTTP 500 Internal Server errors in ASP.NET Core
header-mask: 0.2
tags:
  - .NET Core
---

## Background

Recently we encountered this issue where an ASP.NET Core Web App just did not want to load. It was a .**NET Core 3.1 IIS hosted app**.

On page load, all that was shown was a **HTTP 500 Internal Server** error.
After binding to localhost in IIS, we thought we had a little better context on what was happening as it specifically shown **HTTP 500.19** with **HRESULT code 0x8007000d**.

## What To Check

- **Check Logs - Application, Windows Event Logs** <br/>
  Check to see if there is anything that could give you a hint in the logs. Weirdly, we did not have any logs at all. Even from the Event Viewer!

- **Check Configuration file** (web.config) <br/>
  According to the [Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/developer/webapps/iis/health-diagnostic-performance/http-error-500-19-webpage){:target="\_blank"}, this is because of malformed XML. We checked the config files on our app however it seems ok. The app also runs locally which means the config files are likely to be correct. <br/>
  ![MS Docs Screenshot](/img/posts/2022-04-02-dotnet-core-http500/docs-screenshot.PNG)

- **.NET Runtime** <br/>
  Check if the correct .NET Runtime is installed by running `dotnet --version` via CMD

- **Re-installing the hosting bundles** <br/>
  Try uninstalling and re-installing the [hosting bundles](https://dotnet.microsoft.com/en-us/download/dotnet) multiple times. In our case we had older .NET Core 2.1 runtimes installed previously. We weren't sure if it could be related, however sometimes it is best to start clean and only install what is required.

- **Restart** <br/>
  Try the usual IIS reset via CMD: <br/>
  `net stop was /y` <br/>
  `net start w3svc` <br/>
- **Reboot Machine** <br/>
  If all the above checks are passed and it does not work, there is no harm in even giving the server / machine a re-boot!

## Ultimate Solution

Frustratingly, we did all the above but to no avail. Ultimately, we stumbled across an article saying something about **ApplicationHost.config** and the missing `aspnetcore` element.

The file can be found here: `%WinDir%\System32\Inetsrv\Config\applicationHost.config` <br/>
Ensure the following setting is present in the **ApplicationHost.config** file. <br/>

> `<section name="aspNetCore" overrideModeDefault="Allow" />` within `<sectionGroup>`

No idea why but somehow it was missing from ours. I assume this should be automatically done when installing the Hosting Bundle. However, this solved our problem.

Hope this helps.
