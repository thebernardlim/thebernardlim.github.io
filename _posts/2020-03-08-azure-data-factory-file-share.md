---
layout: post
title:  Azure Data Factory - How to connect to Azure File Storage?
date:   2020-03-08 00:01:12 +0530
description: Steps to show how to connect to Azure File Storage / Share in Azure Data Factory
header-img: "img/headers/adf.png"
tags: 
- Azure Data Factory
---

When trying to use Azure File Storage as a source in Azure Data Factory, you might wonder where you would get the credentials to connect to Azure File Storage in Data Factory. Here are the steps:

1. Create 'New Dataset'. Choose **'Azure File Storage'**.

    ![Azure File Storage Dataset](/img/posts/2020-03-08-azure-data-factory-file-share/azure-file-storage-dataset.PNG)

2. Proceed as per normal until you reach the **'New Linked service'** window. Here it will require 3 fields to be filled: **Host, User name, Password**.

    ![New linked service prompt](/img/posts/2020-03-08-azure-data-factory-file-share/file-storage-prompt.PNG)

3. These field information can be obtained through **File share** window of the respective File Storage in **Storage Accounts**. Click on **Connect** and a window similar to below will show.

    ![Azure File Share](/img/posts/2020-03-08-azure-data-factory-file-share/file-storage-prompt.PNG)

4. A window similar to below will show. Copy the contents of the grey area.

    ![Azure File Share Credentials](/img/posts/2020-03-08-azure-data-factory-file-share/file-share-credentials.PNG)

5. Inspect the content and spot a similar excerpt to the below. This is the **Host, User name, Password** you will require to connect to your file share as a Linked Service.

    ![Azure File Share Credentials 2](/img/posts/2020-03-08-azure-data-factory-file-share/file-share-credentials-2.PNG) 
