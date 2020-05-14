---
layout: post
title:  25 days of Serverless - Challenge 2
date:   2020-04-22 00:01:12 +0530
description: Solution to 25 days of Serverless - Challenge 2
header-img: /img/headers/25daysserverless.PNG
tags: 
- Azure Logic Apps
- 25 Days of Serverless
---

For [Challenge #2 of the '25 days of Serverless' challenge](https://github.com/microsoft/25-days-of-serverless/tree/master/week-1/challenge-2){:target="_blank"}, we are to implement a solution to send Lucy reminders regarding coffee-making steps and candle-lighting at various times accordingly.

I decide to implement based on the sample scenario listed on the challenge:

![Challenge](/img/posts/2020-04-22-25daysserverless2019-challenge-2/challenge.PNG)

## Solution

For this, I figure the easiest way will be to use:

- **Azure Logic Apps** as the service
- **Scheduling and looping functions** for coordination
- **Email connector & action** for sending emails

Below is my solution in the **Logic App Designer**. I created 2 streams:

- Coffee-making
- Candle lighting

Here is the [ARM Template](https://github.com/thebernardlim/25-days-of-serverless/tree/master/challenge-2){:target="_blank"}

![Solution](/img/posts/2020-04-22-25daysserverless2019-challenge-2/logicappdesigner.PNG)

As candle-lighting is required every 10 minutes, I used the **'Until'** control, where inside I used a **'Delay' 10 seconds** control, which after that, fires off an email.

![LightCandles](/img/posts/2020-04-22-25daysserverless2019-challenge-2/lightcandles.PNG)

For coffee-making, I followed the timeline as per the one listed above and made use of **'Delay Until'** controls where I compared it against an **Add Minutes** expression from the *Start Time*. This variable is set at the **Initialize StartDateTime variable** action.

![InitializeStartDateTimeVar](/img/posts/2020-04-22-25daysserverless2019-challenge-2/initstartvar.PNG)

Reason for 2 streams:

- Candle lighting activity has no dependency from coffee-making and recurs every 10 minutes.
- Dependency factor in the coffee-making stream. It only makes sense to be able to distribute coffee only after the coffee has been successfully made etc. This allows me to use **Configure to run after** antod ensure that the subsequent action should only be performed after the preceding action succeeded.

![Configureafter](/img/posts/2020-04-22-25daysserverless2019-challenge-2/configureafter.PNG)

Also for configuration purposes, I used **Parameters** to store stuff such as the Sender Email, rather than hardcoding at every **Send Email** action.

![EmailParameter](/img/posts/2020-04-22-25daysserverless2019-challenge-2/logicappparameter.PNG)

For my **'Send Email'** action, the 'To' refers to this email parameter **'Email'**

![sendemail](/img/posts/2020-04-22-25daysserverless2019-challenge-2/email.PNG)