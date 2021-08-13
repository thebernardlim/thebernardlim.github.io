---
layout: post
title: Azure App Service Plans - How to temporarily disable App Services Plans?
author: Bernard Lim
date: 2021-08-13 00:01:12 +0830
subtitle: How to temporarily disable App Service Plans to save costs
header-mask: 0.2
tags: 
  - Azure App Service Plans
  - App Services
---

# Overview

The ease of deploying apps into the cloud is as easy as a few clicks. This could also mean a high chance of creeping costs that we might accidentally incur if we create too many resources (as it is almost too easy to create them) and forget to manage them properly.

One popular service will be the **App Services Plans** and **App Services**.

- App Service Plans comes in various plans, some are **free**, some are **chargeable**.
- In an App Service Plan, you can host apps, i.e. App Services**App Services**.

### Key Distinction

- App Service Plans types & statuses **determines if it is chargeable**
- App Services statuses **DO NOT have an impact on chargeability**, i.e. If an app is stopped, it might still be charged!

**Hourly charges of the "Basic" App Service Plan:**
![App Services Pricing](/img/posts/2021-08-13-appservice-disable-temporarily-save-costs/appservice_pricing_1.png)

# Problem

What if you have a scenario where you have a number of apps hosted on an App Service Plan, which are currently not being used, and that you would want to temporarily stop them so that you will not be charged?

Currently Azure does not provide a "**Disable**" option. Only a "**Delete**" is provided.

![App Services Stop](/img/posts/2021-08-13-appservice-disable-temporarily-save-costs/appservice_delete.png)

This is one option if you permanently do not want to use your apps, but what if you just want to stop this temporarily?

Unlike VMs, as mentioned earlier, stopping the **App Services** in an **App Service Plan** will still mean you are being charged!

![App Services Stop](/img/posts/2021-08-13-appservice-disable-temporarily-save-costs/appservice_app_stop.png)

# Solution

In the **App Service Plan** tab, Use the "**Scale Up**" option to **scale down** to the "**Free Tier**" plan.

![App Services Scale Down](/img/posts/2021-08-13-appservice-disable-temporarily-save-costs/appservice_scale_down.png)

When the apps are ready to be used once more, scale back up to your preferred plan.

---
