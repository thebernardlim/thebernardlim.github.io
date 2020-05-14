---
layout: post
title:  Azure Data Factory - Removing Triggers without Errors
date:   2020-02-24 00:01:12 +0530
description: Removing triggers from ADF pipelines without errors
img: adf.png
tags: 
- Azure Data Factory
--- 

In the ‘Author’ page, if you attempt to remove triggers by clicking on ‘Trigger’ button, there might be a change you would attempt to click on ‘X’ next to the trigger you want to delete, i.e. trigger1 in the example below.

> ![export template](/assets/img/posts/2020-02-24-adf-remove-triggers/1.png)

However, on publish, this weird and uninformative error appears:
> Trigger cannot be activated and contain no pipelines

> ![sa icon](/assets/img/posts/2020-02-24-adf-remove-triggers/2.png)


### Solution 

Apparently, the **correct** way to delete triggers is by clicking the ‘Trigger’ tab on the left menu as per highlighted in the **red box** below.<br/>
Click on the **bin icon** as per circled in red below, publish, and now your trigger is successfully deleted!

> ![sa extension](/assets/img/posts/2020-02-24-adf-remove-triggers/3.png)
