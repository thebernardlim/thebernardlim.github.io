---
layout: post
title: DataTable is not a function
date: 2021-03-18 00:02:12 +0530
subtitle: DataTable is not a function solutions
header-mask: 0.5
tags:
  - DataTable
  - JQuery
---

I came across this problem recently when I was trying to use the [jQuery DataTable plugin](https://datatables.net/) for tables in one of my webpages.
Error: "$(...).DataTable is not a function"
![Image](/img/posts/2021-03-18-datatable-is-not-a-function/error1.PNG)

I was assuming it is supposedly very easy to solve, however to my surprise, the solutions I found online did not really help, which ended up with a lot of time wasted.

The solution which you will mostly find online will advice on the following:

- jQuery DataTables library is missing.
- jQuery library is loaded after jQuery DataTables.
- Multiple versions of jQuery library is loaded.

I ensured that my solution had no jQuery duplicates and the 'jQuery DataTable' script was only **loaded after** 'jQuery' main script. However the problem still persisted.
![Image](/img/posts/script_no_defer.PNG)

Ultimately, the alternative solution that worked for me was to add a **'defer' attribute** to the 'jQuery DataTable' script tag.
[defer](https://www.w3schools.com/tags/att_script_defer.asp) attribute ensures that script is only executed when page has finished parsing, which means the 'jQuery DataTable' script will only be executed AFTER the 'jQuery' main script.

Hoped this will be useful.
