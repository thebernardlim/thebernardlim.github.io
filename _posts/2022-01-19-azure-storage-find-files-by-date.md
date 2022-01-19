---
layout: post
title: Azure File Storage - Find Files By Date
author: Bernard Lim
date: 2022-01-19 00:01:12 +0530
header-style: text
subtitle: Ways to query / find files by date in Azure File Storage
header-img: "img/headers/storage-account.png"
header-mask: 0.2
tags:
  - Azure
  - Azure Storage
  - Azure SDK
---

## Background

Lately, I came across a scenario where I was required to find out the following in a Azure File Share account:

- Number of files (within each folder) that were older than a year. Exclude folder if at least 1 file within folder is beyond a year.
- Total size of those files

On top of this, this File Share account has a LOT of folders & files, around 100,000 files in various sizes and some folders have sub-folders.

Also to note, the "Last Modified Date" of a directory does not equate to the lastest "Last Modified Date" of the files within the directory. Say for example, if I have 2 files, where their Date Modified is 20th March 2020 and 22nd March 2020, my folder "Last Modified Date" can be earlier than 22nd March 2020!

As we know, Azure provides a number of ways to interact with Azure Storage. However, what is the best option in the scenario above?

### Azure Storage Explorer

[Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/#security) is a standalone tool that can be installed onto your system for easy operations with Azure Storage.

The UI is simple and straight forward to use. We can easily sort files by "Last Modified" and highlight the files / folders that is within date range. Click on "Selection Statistics" and it will show the file count and total size.

This method only works for small number of files though, as there is no way to filter out files by dates. Besides they have a maximum of 100 cached items per page. For my case where I had more than 100k files, this is not a feasible approach.

\*\*Note: Screenshot shown is just an example using Blob storage and not File Share. But both are similar to illustrate this concept.

![App Services Stop](/img/posts/2022-01-19-azure-storage-find-files-by-date/storage-explorer-1.PNG)

### Storage Browser (Preview)

Storage Browser is the 'Storage Explorer' within the Azure portal. It is currently under Preview mode at time of writing.

This tool does not provide any date filtering functions, nor ability to calculate statistics for multiple files.

\*\*Note: Screenshot shown is just an example using Blob storage and not File Share. But both are similar to illustrate this concept.

![App Services Stop](/img/posts/2022-01-19-azure-storage-find-files-by-date/storage-browser-1.PNG)

### Powershell & Azure CLI

The next obvious choices were via code. With Powershell & CLI, I faced multiple timeout errors as there were just too many files to parse.

### Azure SDK

The only solution that I found works was via the Azure SDK. I used C# for this purpose.
It seemed to work, however it is very slow (no timeouts however!) and might not be the most efficient.

Below is the script.

```
static string connectionString = "{Your connection string}";
static string shareName = "{Your file share name}";
string fileName = string.Format("results_{0}.txt", DateTime.Now.ToString("yyyyMMdd_HHmmss"));
DateTime dtOneYearAgo = DateTime.Today.AddYears(-1); //1 year from today
List<string> oldDirectories = new List<string>();
long totalOldFileSizes = 0;

ShareClient share = new ShareClient(connectionString, shareName);
foreach (var x in share.GetRootDirectoryClient().GetFilesAndDirectories())
{
    ShareDirectoryClient shareDirectoryClient = share.GetDirectoryClient(x.Name);
    bool hasNewFile = false;
    long fileSizes = 0;

    foreach (var file in shareDirectoryClient.GetFilesAndDirectories())
    {
        try
        {
            ShareFileClient fileClient = shareDirectoryClient.GetFileClient(file.Name);
            ShareFileProperties fileProp = fileClient.GetProperties().Value;
            fileSizes += (long)fileProp.ContentLength;

            if (fileProp.LastModified > dtOneYearAgo)
            {
                hasNewFile = true;
                break;
            }
        }
        catch (Exception ex)
        {
            hasNewFile = true; //Don't include folder if exception found
            break;
        }
    }

    // Only add folders to list and size total if meet date criteria
    if (!hasNewFile)
    {
        oldDirectories.Add(x.Name);
        totalOldFileSizes += fileSizes;
    }

}

// oldDirectories.Count - Number of folders, where every file is older than 1 year ago
// totalOldFileSizes    - Total size of the folders, where every file is older than 1 year ago

```

### Azure Data Factory

(Will update soon)
