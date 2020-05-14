---
layout: post
title:  Azure Streaming Analytics - Export / Import Streaming Analytics Jobs
date:   2020-01-29 00:01:12 +0530
description: Export / Import Streaming Analytics using Visual Studio Code
header-img: "/img/streaminganalytics.png"
tags: 
- Azure Streaming Analytics
---

Unlike other services, there is no 'Export Template' option within the portal as shown in an example below:
![export template](/img/posts/2020-01-29-azure-tips-export-import-streaming-analytics/export-template-icon.png)

### Exporting

1. One way to export a Streaming Analytics job is through **Visual Studio Code**

2. Install **Azure Stream Analytics Tools** extension through the **Extensions** tab <p/>
![sa icon](/img/posts/2020-01-29-azure-tips-export-import-streaming-analytics/sa-icon.PNG)

3. Click on the **Azure** tab. **Stream Analytics** extension should appear. <p/>
![sa extension](/img/posts/2020-01-29-azure-tips-export-import-streaming-analytics/sa-extension.PNG)

4. Sign in to your Azure account and a list of existing Streaming Analytics jobs should appear.

5. Browse over the job you would like to export and click on the 'Download' button. The job template will not be saved to your local.
![sa jobs](/img/posts/2020-01-29-azure-tips-export-import-streaming-analytics/sa-jobs.PNG)

### Importing / Submitting Jobs

1. Open the downloaded / exported Streaming Analytics job folder in Visual Studio

2. Click on the **.asaql** file

3. On the top of the file, there will be a **Submit to Azure** option. On click, here you can choose the subscription you would like to deploy the job to. <p/>
![sa jobs1](/img/posts/2020-01-29-azure-tips-export-import-streaming-analytics/sa-submit.PNG)
