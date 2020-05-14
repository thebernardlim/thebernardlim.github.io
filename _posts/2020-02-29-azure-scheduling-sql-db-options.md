---
layout: post
title:  Azure Job Scheduling Options for Azure SQL DB
date:   2020-02-29 00:01:12 +0530
subtitle: Article showing/comparing a list of Azure services we can use for scheduling jobs required for Azure SQL database
header-img: "img/headers/schedule.jpg"
header-mask: 0.2
tags:
- Azure SQL
- Azure Functions
- Scheduling
---

We recently were faced with a request to run SQL scripts (stored procedures) for an Azure SQL Database on schedule.

Naturally, SQL Job Agents came into my mind. However, Azure SQL Single Databases do not support them, and the Microsoft docs recommends Azure Elastic Jobs. This was not an issue for me until I realized it is currently in preview mode!

As we only considered Generally Available services, we further explored and compared all possible services offered on Azure.

Below is a list of the Azure services that can perform scheduled jobs on Azure SQL DB and its pros and cons to each.

--- 

### Option #1: Elastic Jobs

![Elastic Job Logo](/img/posts/2020-02-29-azure-scheduling-sql-db-options/elasticjobagents-logo.png)


Click [here](http://thebernardlim.com/azure-elastic-jobs-setup/){:target="_blank"} for my detailed writeup on how to setup Azure Elastic Jobs.

#### Pros

- [Recommended by Microsoft](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-job-automation-overview#elastic-database-jobs-preview){:target="_blank"} as the replacement to SQL Agents for Azure SQL DB tiers where SQL Agents are not supported.

#### Cons

- **More work to setup.** Requires a Jobs database, custom coding (T-SQL / Azure CLI), credentials setup.
- **Not as easily maintainable** as custom coding required, when compared to visual designers.
- Still in **Preview Mode** at time of writing.
- **Monitoring not as detailed in Azure Portal.** It only shows if a job succeeds/fails, however, does not describe what the job does / what is failing. Will require T-SQL statements for detailed description of job execution.  
![Latest 100 job executions](/img/posts/2020-02-29-azure-scheduling-sql-db-options/elastic-jobs-1.png)
*Screenshot showing the 'Overview' tab of Elastic Job Agent*

#### Other Notes

- Pricing includes cost of a new database, i.e. the Jobs Database. The Jobs Database requires a minimum of S0 Service Tier and beyond. Current cost for S0 Tier: HK$0.1565/hour.

---

### Option #2: WebJobs

![WebJobs logo](/img/posts/2020-02-29-azure-scheduling-sql-db-options/webjobs-logo.png)

#### Pros

- **Easy to setup.** Just requires upload of script and setting of schedule.

#### Cons

- **Expensive option** as it requires AppService plan to be *Standard* or *Premium* because of **Always On** feature requirement.
- **Not as easily maintainable** as custom coding required, when compared to visual designers.
- **Monitoring requires custom coding (to output log lines)** to be more useful for diagnosis in the event of failures.  
![List of web jobs that ran](/img/posts/2020-02-29-azure-scheduling-sql-db-options/webjobs-1.png)*In the Kudu dashboard, you can see a list of job runs executed along with its status*  
![WebJob run details](/img/posts/2020-02-29-azure-scheduling-sql-db-options/webjobs-2.png)*On click of any job, you can see the console logs generated when running the job*

---

### Option #3: Functions

![Function Apps logo](/img/posts/2020-02-29-azure-scheduling-sql-db-options/function-apps-logo.png)


#### Pros

- **Easy to setup.** Requires some custom coding (C#, or any supported language)
- **Cheaper option of "WebJobs"** as there is no need of being **Always On.** It only [charges](https://azure.microsoft.com/en-us/pricing/details/functions/){:target="_blank"} once it is used. It also has a **monthly free grant allowance**, for a certain number of executions and execution time.

#### Cons

- **Not as easily maintainable** as custom coding required, when compared to visual designers.
- **Monitoring requires custom coding (to output log lines)** to be more useful for diagnosis in the event of failures.  
![Recent Jobs window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/function-apps-1.png) *List of recent function runs under 'Monitor' tab*  
![Recent Jobs window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/function-apps-2.png) *Here you can view the invocation details of the specific job run entry*

#### Other Notes

- Default execution timeout is from 5 - 30 mins depending on [AppService plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#service-limits){:target="_blank"}. For Dedicated plans, there is no overall limit.

---

### Option #4: Automation Accounts

![Automation Accounts logo](/img/posts/2020-02-29-azure-scheduling-sql-db-options/automation-accounts-logo.png)

#### Pros

- **Easy to setup.** Requires some custom coding (Powershell / Python).

#### Cons

- **Not as easily maintainable** as custom coding required, when compared to visual designers. Note: There is a *Graphical Runbook* option available, however will require knowledge such as the correct Powershell CMDlets to use.
- **Monitoring requires custom coding (to output log lines)** to be more useful for diagnosis in the event of failures.  
![Recent Jobs window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/auto-1.png) *Under the 'Jobs' tab, it will show you a list of recent runbook runs*  
![Job Detail window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/auto-2.png) *On click of any runbook run, you can get more details*

#### Other Notes

- Maximum **execution timeout** is **3 hours per runbook**

--- 

### Option #5: Logic Apps

![Logic App Logo](/img/posts/2020-02-29-azure-scheduling-sql-db-options/logic-apps-logo.png)

#### Pros

- **Visual design** of job. Does not require coding. Can code if required.
- **Easily maintainable** because of visual designing ability.
- **Easy to debug & monitor**. Each step in a run, can be clicked on to see details on what went wrong, which is very useful. No custom logging required to be specifically made.  
![Logic App Runs History](/img/posts/2020-02-29-azure-scheduling-sql-db-options/logic-app-2.PNG) *On the Overview page, there is a list of Runs history window showing results of recent runs*  
![Logic App Run Window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/logic-app-1.png) *On click of each run entry, here you can visually view which part of the run failed. On click of any action, you can view further details*

#### Cons

- **[Short timeout execution](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-limits-and-config#http-limits){:target="_blank"}** time (120 seconds). 

--- 

### Option #6: Data Factory

![Data Factory Logo](/img/posts/2020-02-29-azure-scheduling-sql-db-options/adf-logo.png)

#### Pros

- **Visual design** of pipeline / activities. Does not require coding.
- **Easily maintainable** because of visual designing ability.
- **Long timeout execution duration** of 7 days per pipeline activity.
- **Easy to debug & monitor**. Each activity in a pipeline run, can be clicked on to see details on what went wrong, which is very useful. No custom logging required to be specifically made.  
![Monitor Window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/df-1.png) *In the ADF UI, there is a 'Pipeline runs' and 'Trigger runs' windows which shows a list of recent runs.*  
![Pipeline Run Detail Window](/img/posts/2020-02-29-azure-scheduling-sql-db-options/df-2.png) *On click of any run entry, you can visually view the pipeline to see each activity in detail*


#### Cons
N/A

---

### Conclusion: What service should I choose?

All the 6 options above allows scheduled jobs to run against Azure SQL databases. In my case, my requirement was just to execute stored procs on a scheduled basis hence all were possible options.

Each has its pros and cons and would really depend on what we have or what we prioritize. Some scenarios below:

- If we require only **Generally Available** options, then **Elastic Jobs cannot** be considered.
- If we have an **existing AppService Plan** (that is Standard or Premium) that we would like to utilize further, we can consider **WebJobs**
- If we only **run short execution jobs a few times per month**, we can consider **Functions** or **Logic Apps**, as both are cheap and easy / quick to setup.
- If we want only strictly **Database level coding** (T-SQL), then **Elastic Jobs** can be the option.
- If we want to be able to allow admins to easily maintain (monitor, diagnose, debug or even make changes) to our scheduled jobs, then we can consider visual designer options such as **Logic Apps** or **Data Factory**

Do let me know if there are any more options I can explore or pros/cons to add to the above list. Would love to hear them out and further explore!