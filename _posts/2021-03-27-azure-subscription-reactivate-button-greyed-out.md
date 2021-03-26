---
layout: post
title: Azure Subscription 'Reactivate' button disabled / greyed-out
date: 2021-03-27 00:02:12 +0530
subtitle: Enable Azure Subscription after deactivation
header-mask: 0.5
tags:
  - Azure Subscription
---

On completion of the first 30 days of Microsoft Azure's free trial, your **'Free Trial'** Azure Subscription will be disabled.
To fix this, the subscription needs to be changed to the **'Pay-As-You-Go'** plan instead of the 'Free Trial' plan which it is currently on.
![Image](/img/posts/2021-03-27-azure-subscription-reactivate-button-greyed-out/reactivate_greyed_out.PNG)

# Steps

1. Access the [Pay-As-You-Go](https://azure.microsoft.com/en-in/offers/ms-azr-0003p/){:target="\_blank"} page. Click on **'Purchase Now'** button
   ![Image](/img/posts/2021-03-27-azure-subscription-reactivate-button-greyed-out/pay_as_you_go.PNG)

2. Click on **Upgrade your account to pay-as-you-go pricing'**
   ![Image](/img/posts/2021-03-27-azure-subscription-reactivate-button-greyed-out/pay_as_you_go_2.PNG)

3. Choose your preferred plan. As per screenshot, **Basic** plan has been chosen.
   ![Image](/img/posts/2021-03-27-azure-subscription-reactivate-button-greyed-out/pay_as_you_go_3.PNG)

4. That is all there is. After a while a notification popup will appear saying that Subscription has been upgraded.
   ![Image](/img/posts/2021-03-27-azure-subscription-reactivate-button-greyed-out/subscription_upgraded.PNG)

One thing to note, it might take awhile for the services to fully kick in. An example is if you try creating Budgets right after switching plans, it might not work right away.
