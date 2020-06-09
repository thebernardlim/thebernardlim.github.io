---
layout: post
title:  25 days of Serverless - Challenge 3
date:   2020-06-08 00:01:12 +0530
subtitle: Solution to 25 days of Serverless - Challenge 3
header-img: "img/headers/25daysserverless.PNG"
header-mask: 0.5
tags: 
- Azure Function
- 25 Days of Serverless
---

For [Challenge #3 of the '25 days of Serverless' challenge](https://github.com/microsoft/25-days-of-serverless/tree/master/week-1/challenge-3{:target="_blank"}, we are to implement a solution to store images in a database if there is a 'png' image being pushed into a GitHub repo.

## Solution

As per the instructions, we will be required to use Azure Functions and GitHub Webhooks.

Here are the steps:

1. Create GitHub Webhook. This can be accessed through the 'Settings' tab of your Repo. Choose the 'Pushes' event as your trigger.
!['Webhooks' tab under Settings](/img/posts/2020-06-08-25daysserverless2019-challenge-3/webhook-add.PNG)

!['Webhooks' tab under Settings](/img/posts/2020-06-08-25daysserverless2019-challenge-3/webhook-add-2.PNG)

2. Do a test push to test out the webhook. In the 'Recent Deliveries' section we can see the request payload which we shall view to identify what fields we will need to parse.
!['Webhooks' tab under Settings](/img/posts/2020-06-08-25daysserverless2019-challenge-3/webhook-recent-deliveries.PNG)

![Payload sample](/img/posts/2020-06-08-25daysserverless2019-challenge-3/payload-sample.PNG)

3. Create Azure Function to parse payload. This is pretty straightforward as it was just a matter of parsing the request payload and then inserting into the database. I went with the Azure SQL route for simplicity. Code for the Azure Function can be found [here](https://github.com/thebernardlim/25-days-of-serverless/blob/master/challenge-3/PushWebHookFunction.cs){:target="_blank"}

4. In terms of authentication between Azure Function and Azure SQL, I decided to enable Managed Identity for Azure Function.
![Managed Identity](/img/posts/2020-06-08-25daysserverless2019-challenge-3/PushWebHook-managed-identity.PNG)

Once that is done, create a contained DB user to represent the Azure Function. Assign the respective rights to allow insertions into the SQL DB.

![Create Contained DB User](/img/posts/2020-06-08-25daysserverless2019-challenge-3/create-contained-db-user.PNG)

5. The integration should be ready at this point and once you push a 'png' image into your branch, the Image URLs should be now inside your database.
![Image](/img/posts/2020-06-08-25daysserverless2019-challenge-3/results-db.PNG)
