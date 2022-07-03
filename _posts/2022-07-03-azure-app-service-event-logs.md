---
layout: post
title: How to view Azure App Service Event Logs?
author: Bernard Lim
date: 2022-07-03 00:04:40 +0530
subtitle: How to view Azure App Service Event Logs?
header-mask: 0.2
tags:
  - Azure App Service
---

## Background

In a PAAS model, it is simple to deploy and get an app running, but what is actually more important is familiarity with the administrative tools / UI they offer, especially when things go wrong when in Production!

We recently had an issue where an Azure App Service hosted app suddenly was not able to load - **HTTP 500.30** error.
<kbd>
![HTTP 500.30 error](/img/posts/2022-07-03-azure-app-service-event-logs/http500-30.png)
</kbd>

One of the first steps was obviously looking into the **Event Log Viewer**, but in an App Service, where can we find them?

## Solution #1 - Portal

1. In the Azure App Service page, click on **Diagnose and solve problems** tab

2. In the **Diagnose and solve problems** tab, click on **Diagnostic Tools** option
   ![Diagnostic Tools](/img/posts/2022-07-03-azure-app-service-event-logs/soln1-step1.png)

3. In the **Diagnostic Tools** tab, click on **Application Event Logs** and you should be able to see the list of events.
   ![Event Logs](/img/posts/2022-07-03-azure-app-service-event-logs/soln1-step2.png)

## Solution #2 - Kudu

1. In the Azure App Service page, click on **Advanced Tools** and then **Go** in the tab
   <kbd>
   ![Advanced Tools](/img/posts/2022-07-03-azure-app-service-event-logs/soln2-step1.png)
   </kbd>

2. In the Kudu page, click on **Tools** in the menu bar, followed by **Diagnostic dump**
   ![Kudu](/img/posts/2022-07-03-azure-app-service-event-logs/soln2-step2.png)

3. It should auto or prompt for a download of a dump-xx zip file

   ![Zip](/img/posts/2022-07-03-azure-app-service-event-logs/soln2-step3.PNG)

4. Inside of this zip file, there is an **eventlog.xml** which is the App Service events logs.
   <kbd>
   ![Event Log](/img/posts/2022-07-03-azure-app-service-event-logs/soln2-step4.png)
   </kbd>
