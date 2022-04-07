---
layout: post
title: Azure App Services vs Azure VM (Virtual Machines)
author: Bernard Lim
date: 2022-04-03 00:04:40 +0530
subtitle: How to choose between Azure App Services vs Azure VM (Virtual Machine)?
header-mask: 0.2
tags:
  - Azure App Service
  - Azure Virtual Machine
---

# Azure App Services vs Azure Virtual Machines

Recently I have been spending a lot more time working with Azure VMs and App Services. I find that there could be some confusion as to what is the right choice to choose to hosting your apps.

So far, the best resource out there should be the [official docs](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree){:target="\_blank"} which has a very well written article to help you choose which is the right Compute Service to go for.

However I feel there are a couple of points that might not be listed, hence in this article, I intend to provide my thought process, my experiences and provide more food for thought to help you make the right decision.

As a professional, I believe it is important for us to make the right decisions based on facts and figures, rather than by opting for the latest shiny toys. Virtual Machines might sound "boring" as they have been there since forever, however we should not jump aboard anything newer without actually investigating deeply if that is what we actually need.

## Team Technical Capabilities

- Does your team has server administration and Azure Infrastructure knowledge?
- VMs require self-management of the following:

  - OS Patches
  - Security Setup (Firewall, SSL Certificates)
  - Setup of Web Server
  - Network (Virtual Network, Subnet, Network Security Group)

- Examples of scenarios to think about:

  - Setting up of web server (1 time setup)
    - Installing correct .NET Runtime / Hosting Bundle
    - IIS Setup
    - SSL Certificate installation
  - Paying attention to security patches and various updates

    - Scheduling downtime to do the above installations & upgrades.
    - Is it worth the risks that patches might bring?
    - What if installing a patch brings about a conflict of some sort with another application?

  - When a critical error happens, there are multiple levels of diagnosis that might be done and would involve more than just investigating the app itself. Diagnosing errors requires deeper understanding of the OS and webservers. It also requires ability to work under pressure as it is usually time-critical especially when it comes to Production environment.
    - Server level (OS, Network, Runtime)
    - Web Server level (IIS, Kestrel)

- More control in VMs also means higher chances of issues happening from a server / setup level. Some examples I had recently:
  - App unable to load after deployment due to some system issues. In our test VM, everything was ok. But in PROD, it just failed no matter what we tried. We were pretty confident both VMs were very similar setup, but guess that wasn't the case.
  - In another scenario, we faced this [issue](https://thebernardlim.com/dotnet-core-http500/){:target="\_blank"}, where a .NET 3.1 app could not load albeit following all the installation instructions diligently to install the .NET Runtime.

## Capability

- What is the purpose of the app? Is it just a web application or a full-fledged SQL Server?

- The compute power of Azure App Services is **limited**, as there is only A Series or Dv2 Series VMs to choose from (at time of writing)
  For Azure VMs, you have many choices such as the B Series (Burstable workloads), F Series (Compute Optimized), E & G Series (Memory Optimized) and many more.

- This means that there is no way for an App Service to run more resource-intensive apps.

## Scalability (Horizontal Scaling)

- In VMs, horizontal scaling is achieved via Scale Sets
  - Require separate load balancer
  - If existing VM does not belong to Scale Set, might need a bit of configuration work to enable.
- Azure App Service provides an **integrated load balancer** and can easily increase instances (Just by slider!)

## Control & Familiarity

- One benefit of VM is having full control of the machine.
- Ability to group related apps / resources together in a machine. For example, running Windows Services along with Web Apps. Can be easier to maintain as apps/services are in a single place. Of course this might not be the best option too as the VM goes down, all your apps/services will go down too!
- Another benefit of VMs is ability to use local filesystem as per normal. Easy to navigate around as everyone is familiar. App Service is a abit more difficult as its done via the browser. There are also limitations in how you use the filesystem if I am not wrong.

## Costs

- Azure App Service can actually be more expensive as compared to VMs.
  At time of writing, the Production-level App Service is a Dv2-Series equivalent of Azure VM
- Just as a simple comparison:
  ![MS Docs Screenshot](/img/posts/2022-04-03-azure-app-service-vs-azure-vm/vm-appservice-costs-comparison.PNG)

  - App Service - P1v3 plan is a DsV2 Azure VM 2 Core, 8 GB RAM, 250 GB Storage. Costs **$275.21 USD** per month.
  - Azure VMs - DsV5 Series Azure VM 2 Cores, 8 GB RAM, 256 GB Standard SSD Storage. Costs **$182.92 USD** per month. <br/>

- VMs are **only charged on disk costs** when stopped.
- Azure App Service will **continue charging even though apps are unused or when stopped**. <br/>
  There are ways to temporarily stop Azure App Services from charging, however it is not as straightforward. <br/>
  One way is to move to the Free Tier, however the Free Tier is only limited to 1 GB in storage space and no SSL capabiliites.
  It is highly likely, one would have more than 1 GB worth of data hence this might not be feasible.

## Other Thoughts

- Another question to ask is "Although we can, do we need to?". For example, yes we have a team to handle server-related stuff if we opt for VMs, however do we need to continue working this way? Extra work = Extra personnel = Extra time & costs

- Although App Services might cost more on paper, depending on your team structure, there could already be costs saving made as there is minimal infrastructure involvement.

- With VMs, one can also say
  > "With extra power, comes extra responsibilities"

## Conclusion

Ultimately, it comes down to the organization's current capabilities and what the project needs. At the end of the day, the goal is to fully support the business and users, which matters above anything else.

Please feel free to share your thoughts. I believe I might continue to update this post as I explore further with Azure VMs and App Services.
