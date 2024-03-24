---
layout: post
title: Azure OpenAI DALL-E - Generate Images using Postman
author: Bernard Lim
date: 2024-03-24 00:12:30 +0530
subtitle:
header-mask: 0.2
tags:
  - Azure
  - AI
---

Here is a simple guide on how to generate images using Azure OpenAI DALL-E via Postman.

Before starting off, I am assuming you will already have a Azure OpenAI subscription that is ready to be used.

---

## Azure OpenAI Studio

1. Go to Azure OpenAI Studio : https://oai.azure.com/
2. Click on **DALL-E** under the **Playground** section

   ![DALL-E](/img/posts/2024-03-24-azure-openai-dalle-postman/oai-dalle.png)

3. Click on 'View Code'
   ![DALL-E-2](/img/posts/2024-03-24-azure-openai-dalle-postman/oai-dalle-2.png)

4. Select 'json' from the dropdown, and copy the **OAI API Key** (hidden text).
   ![DALL-E-3](/img/posts/2024-03-24-azure-openai-dalle-postman/oai-dalle-3.png)

## Postman

1. Create a new **POST** Request with the following values:

   - Query Params
     `api-version : 2023-06-01-preview`
   - Request Headers

     `api-key : {Paste the OAI API Key copied previously)`

   - Body

     Paste this JSON as the Request Body and edit as needed:

     ```
     {
       "prompt": {Add Your Image Prompt here},
       "n": 1,
       "size": "1024x1024"
     }
     ```

2. On response, you might get a `"status": "notRunning"` as response. This is normal.
   ![DALL-E-6](/img/posts/2024-03-24-azure-openai-dalle-postman/oai-dalle-6.png)

3. Open the _Response Headers_ tab and copy the URL from **operation-location** header

   ![DALL-E-4](/img/posts/2024-03-24-azure-openai-dalle-postman/oai-dalle-4.png)

4. Make another request, this time a **GET** (instead of a **POST**) request with the URL copied from Step 3.
5. Response returned should return the generated image URL
   ![DALL-E-5](/img/posts/2024-03-24-azure-openai-dalle-postman/oai-dalle-5.png)

---

More references on generating images using Azure OpenAI DALL-E:

- https://learn.microsoft.com/en-us/training/modules/generate-images-azure-openai/4-dall-e-rest-api
