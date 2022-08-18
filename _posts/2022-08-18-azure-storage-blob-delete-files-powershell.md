---
layout: post
title: Azure Blob - Delete blobs via Powershell
author: Bernard Lim
date: 2022-08-18 00:12:30 +0530
subtitle: How to delete old Azure Page Blob files in the absence of Lifecycle Management
header-mask: 0.2
tags:
  - Azure Blob Storage
---

## Background

Azure Blobs can easily be deleted via **Azure Storage Explorer**.
However for containers which are massive, this can be a challenge due to the pagination limits that the tool brings.

Deleting old files in large containers however, can easily be done via **Lifecycle Management** .

However there is a **caveat** that I recently was faced with.
**Lifecycle Management** only [works](https://docs.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview) with:

- Block blobs
- Append blobs

But not with **Page blobs**!

![Screenshot](/img/posts/2022-08-18-azure-storage-blob-delete-files-powershell/pic1.png)

An example that this is needed is if your SQL DB backups are made to Azure Blob Storage. There is an option (or by default) set to **Page blob types**. To delete/archive them we will unfortunately need a custom solution.

## Solution

One of the ways to do so is via **Powershell**.

Here is a script to remove files older than _x number of days_

```
$resourceGroup = "[your-resource-grp]"
$storageAccountName = "[your-storage-acct]"
$containerName = "[your-container-name]"
$daysToKeep = [number-of-days]

$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfBlobs = Get-AzStorageBlob -Container $containerName -Context $ctx
$blobs = Get-AzStorageBlob -Container $containerName -Context $ctx | Where-Object{$_.LastModified.DateTime -lt (Get-Date).AddDays($daysToKeep)} | Remove-AzStorageBlob

```

Do note the above script can be used for any blob types.
The above script can be hooked as part of a **SQL Job** as part of the **SQL Maintenance Plan** after backing up, or via any scheduler options such as Azure Functions with timers, Azure Automation etc.

Hope the above is useful as a starting point.
