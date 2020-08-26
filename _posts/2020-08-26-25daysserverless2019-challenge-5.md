---
layout: post
title:  25 days of Serverless - Challenge 5
date:   2020-08-26 00:01:12 +0530
subtitle: How to use Azure Cognitive Services to do sentiment analysis - Solution to Challenge 5
header-img: "img/headers/25daysserverless.PNG"
header-mask: 0.5
tags: 
- Azure Cognitive Services
- Azure Function
- 25 Days of Serverless
---

For [Challenge #5 of the '25 days of Serverless' challenge](https://github.com/microsoft/25-days-of-serverless/tree/master/week-1/challenge-5){:target="_blank"}, we are to implement a solution to help Santa decide if each child is nice or naughty based on what he/she said.

## Solution

For this solution, we can use **Azure Functions** which will call **Azure Cognitive Services** to do sentiment analysis!
    ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/cognitive-service-icon.png)

1. Create an **Azure Cognitive Services - Text Analytics** resource. There is the option to choose a ****'Free' Pricing Tier** if this is the first Text Analytics resource in the region.  
    ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/create-text-analytics-service.PNG)

2. Once resource is created, click on **'Keys and Endpoint'** tab. Copy one of the 2 keys provided. 
    ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/keys.PNG)

3. Access this [link](https://eastasia.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/) to try out calling the **Text Analytics 'Sentiment API'**. Click on the  **region** where your Text Analytics resource is created.
    ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/text-analytics-api-main.png)

4. On click, paste the copied key from Step 2 to the **Ocp-Apim-Subscription-Key field**. Click on **'Send'** to make request.
    ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/text-analytics-api-main-2.png)

5. If successful, there should be a HTTP 200 Response followed by a sample response. As can be seen in the response, there is a **'Sentiment' field** which can use for our purpose. The values will either be of **Positive, Neutral or Negative**.
   ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/text-analytics-api-main-4.png)

6. Next step will be to analyze Santa's list of sentences from the children which we can obtain from https://aka.ms/holiday-wishes.
   It comprises of a list of sentences where:
   - Each sentence is made up of a child and what he/she says.
   - Each sentence is of different language other than language.
   - Each child can have multiple sentences
   ![Image](/img/posts/2020-08-26-25daysserverless2019-challenge-5/santa-api-response.PNG)

7. With the above, we can come up with a simple **Azure Function** where Santa can call which:
   - Calls https://aka.ms/holiday-wishes to get list of sentences
   - For each sentence:
      1. Call Language Detection API - To detect language
      2. Call Sentiment API - Where Parameters are the language detected + sentence
      3. Retrieve the 'Sentiment' value from Sentiment API response
      4. Store the value in a simple Dictionary where each key belongs to each child, where value is a Integer score.
      5. Do a simple calculation logic, where if:
         1. Positive sentiment - Add 1
         2. Negative sentiment - Minus 1
         3. Neutral - Do Nothing
      6. Generate and return response

8. Here is the [Azure Function solution](https://github.com/thebernardlim/25-days-of-serverless/tree/master/challenge-5/SentimentAnalysisFunction/Day5Function) I created in C#.

9. Also to note, there were a couple of hiccups I faced along the way:
   1. There are 2 versions of Text Analytics at time of writing: v2.1 and v3.0. I used v3.0 which seemed easier to implement.
   2. However v3.0 has some limitations such as no support of Swedish language which was one of the languages present in the provided sentences.

Thought this was a fun project to explore world of Azure AI Services with. Do try it out!\
Also realized I missed out on doing Day 4. Will do Day 4 next.