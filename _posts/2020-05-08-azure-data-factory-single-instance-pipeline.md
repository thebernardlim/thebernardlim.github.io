---
layout: post
title:  Azure Data Factory - Run single instance of pipeline at a time
date:   2020-05-08 00:01:12 +0530
subtitle: "Article showing how to run only a single instance of a pipeline at a time"
header-img: "img/headers/adf.png"
header-mask: 0.2
tags: 
- Azure Data Factory
---

I recently faced this scenario where I noticed there were multiple instances of my pipeline running concurrently. The reason this happened was because the **runtime of my pipeline exceeded that of my trigger interval**.

I googled around trying to find a proper solution for this and the best I could find was to set the **'Concurrency' value of the pipeline to '1'**. This ensured that only 1 instance of the pipeline will run at a time.

![Concurrency Setting](/img/posts/2020-05-08-azure-data-factory-single-instance-pipeline/concurrency-setting.png)

## Problem

The only problem was if there is an existing pipeline still running and the trigger interval kicks in, this **upcoming run will be queued**.

In the event, say the interval is every hour, and my pipeline run suddenly takes 10 hours, this will mean 10 pipeline runs being queued!

Apparently there is still no out-of-the-box solution for this, which led me to further investigate other options.

## Solution

One solution we can handle this will be by making use of the **Data Factory REST API**.

There is this [article](https://mrpaulandrew.com/2019/11/21/get-any-azure-data-factory-pipeline-run-status-with-azure-functions/){:target="_blank"} written that describes how he used REST API to access Data Factory pipeline and get their current statuses. There is also [sample code](https://github.com/mrpaulandrew/BlogSupportingContent/blob/master/Get%20Any%20Azure%20Data%20Factory%20Pipeline%20Run%20Status%20with%20Azure%20Functions/PipelineStatusChecker/PipelineStatusChecker/Functions.cs){:target="_blank"} provided which I used as my starting point.

In simple terms, all I needed to do was to:

1. Create a Function App.
2. Create a HTTP Function which will:
    - Send a GET request to ADF REST API to retrieve the current pipeline run status.
    - If status is 'Succeeded', send a POST request to to ADF REST API to create a pipeline run.
    - If status is not 'Succeeded', send a response to user saying new pipeline run is not ready to be created.

My code for this function can be found here under on my [GitHub repository](https://github.com/thebernardlim/azure/tree/master/function-apps/adf-utility/ADF-Utility){:target="_blank"}. The function to do this has been named as 'RunPipelineByName'

## Testing

Let's test this out!

1. In order to test this, you can install the following [script](https://github.com/thebernardlim/azure/tree/master/function-apps/adf-utility/ADF-Utility){:target="_blank"} in Postman.
2. Once installed, there will be a list of requests as per the screenshot below.

    ![Postman Script Overview](/img/posts/2020-05-08-azure-data-factory-single-instance-pipeline/postman-script-overview.PNG)

3. As we will be triggering the [ADF Pipeline 'createrun' API](https://docs.microsoft.com/en-us/rest/api/datafactory/pipelines/createrun){:target="_blank"}{:target="_blank"} which requires an access token, we will need to make first make a call to Microsoft Identity Platform to get the access token. Before this, you will need to [create an App Registration within Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app){:target="_blank"}

    ![Pipeline Security](/img/posts/2020-05-08-azure-data-factory-single-instance-pipeline/adf-pipeline-security.PNG)

    Update the global variables of the script with your respective values.

    ![Postman Variables](/img/posts/2020-05-08-azure-data-factory-single-instance-pipeline/postman-variables.PNG)

4. Once the variables are set, send a  **'Get AAD Token'**  request. This will set an **access token** to a global variable which will be used in our next API call.

5. Send a **'Run Single Instance ADF Pipeline'** request. If the 'Status' returns as 'Pipeline run successfully created' then your pipeline is now running!

    ![Pipeline Run Created](/img/posts/2020-05-08-azure-data-factory-single-instance-pipeline/postman-successful-response.PNG)
