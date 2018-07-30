---
author: sjohner
comments: true
date: 2017-05-04 16:10:09+00:00
layout: post
slug: microsoft-oms-update-deployment-for-linux
title: Microsoft OMS Update Deployment for Linux
categories:
- Linux
- Operations Managment Suite
tags:
- Azure Automation
- Deployment
- Hybrid Runbook Worker
- Linux
- OMS Agent
- Operations Management Suite
- PowerShell
- PowerShell DSC
- Schedule
- Update
- Update Management
---

About two weeks ago, Microsoft quietly introduced Update Deployments for Linux in Operations Management Suite. The Update Management solution in OMS allows you to manage updates for your Windows as well as your Linux computers. The status of available updates can be quickly assessed and you can initiate installation of required updates for your Windows and since lately also your Linux servers (supported distributions). Cool thing here: it doesn't matter where your servers are located. With OMS you can concentrate all your machines in one place no matter whether they are running in AWS, Azure, Google Cloud Platform, in our own datacenter or wherever. Just install the OMS Agent and you are good to go. For more information about using the OMS Agent for Linux, check out my previous post [Working with Operations Management Suite Agent for Linux](https://blog.jhnr.ch/2016/01/27/working-with-oms-agent-for-linux-preview/).

![Update Management Overview.png](/images/Update-Management-Overview.png)

First of all, lets check what we need to use Update Deployments for our Linux machines. Putting aside requirements for Windows Servers, the Update Management Solution supports the following distributions:



 	
  * CentOS 6 (x86/x64), and 7 (x64)

 	
  * Red Hat Enterprise 6 (x86/x64), and 7 (x64)

 	
  * SUSE Linux Enterprise Server 11 (x86/x64) and 12 (x64)

 	
  * Ubuntu 12.04 LTS and newer x86/x64


Furthermore the Linux agents must of course be installed on your box and have access to an update repository.

**Important point here:** All available updates from the given repository are going to be installed. There is no way of selecting which updates should be installed or not by using OMS. The solution always reports how up-to-date the given computer is based on what source is configured to synchronize with. This can be of course a local repository or a public repository. If you want more granular control over updates you would need to provide your own repository to make sure only packages are available which are approved.

**Another note:** OMS Agent for Linux configured to report to multiple OMS workspaces is not supported with Update Management solution (yet).

Computers which are configured for Update Management use the following components for performing assessment and update deployments (just install OMS Agent for Linux, it will take care of PowerShell DSC and Hybrid Runbook Worker components):



 	
  * [OMS agent for Windows or Linux](https://github.com/Microsoft/OMS-Agent-for-Linux)

 	
  * [PowerShell Desired State Configuration (DSC) for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux)

 	
  * [Azure Automation Hybrid Runbook Worker](https://docs.microsoft.com/en-us/azure/automation/automation-hybrid-runbook-worker)


Now lets take a look on what's happening when using OMS Update Management solution with your Linux machines. The following schema is taken from [Microsoft documentation ](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-solution-update-management)and describes the procedure when using the OMS Update Management solution.

![update-mgmt-linux-updateworkflow](/images/update-mgmt-linux-updateworkflow.png)



 	
  1. The OMS Agent for Linux scans for available updates every 3 hours and reports status to OMS. Note that it can take anywhere from 30 minutes up to 6 hours for the dashboard to display updated data from managed computers.

 	
  2. OMS users review the update assessment and define a deployment schedule by using the OMS portal

 	
  3. The Hybrid Runbook Worker running on your Linux machine checks for maintenance windows and deployments

 	
  4. If the corresponding machine is affected by an Update Deployment (either as direct member or as member of a computer group), it leverages the appropriate package manager (Yum, Apt, Zypper) to install available packages.

 	
  5. The OMS Agent for Linux reports status of the Update Deployment back to OMS


Updates are installed by runbooks in Azure Automation. When enabling this solution, any Windows or Linux computer directly connected to your OMS workspace is automatically configured as a [Hybrid Runbook Worker ](https://docs.microsoft.com/en-us/azure/automation/automation-hybrid-runbook-worker)to support the runbooks included in this solution. These runbooks are not visible and they do not require any configuration. When an Update Deployment is created, a schedule is created that starts a master update runbook at the specified time for the computers included in the deployment. This master runbook then starts a child runbook on each agent which performs installation of required updates. For each computer managed by the solution, a new Hybrid Runbook Worker Group will be listed in your OMS-Automation account following the naming convention _Hostname FQDN_GUID_. You can see the created Hybrid worker groups when looking at your OMS-Automation account in Azure.

![Update Management Hybrid Runbook Workers](/images/Update-Management-Hybrid-Runbook-Workers.png)

So once the OMS agent is installed on your server, it starts reporting Update status to OMS and you will see the Update Management dashboard being populated with data.

![Update Management Overview](/images/Update-Management-Overview.png)

As you see above, both of my Linux computers do actually miss some optional updates so lets create a deployment to install these packages. Therefore, scroll to the right in the Update Management solution and select _Manage Update Deployments. _You will get a screen where you can add a new deployment.

![Update Management Add Deployment.png](/images/Update-Management-Add-Deployment.png)

The above deployment is configured to deploy updates to my test server. This happens once but you are also able to configure weekly or monthly recurring update deployments.

**Important note here:** When you include computer groups in your update deployment, group membership is evaluated only once at the time of schedule creation. Subsequent changes to a group are not reflected. To work around this, you have to delete the scheduled update deployment and recreate it unfortunately.

Also note that your deployments must have a unique name, otherwise you will not be able to create a deployment. This is probably because the created master runbooks in your automation account are named after the deployment.

![Update Management Deployment with same name](/images/Update-Management-Deployment-with-same-name.png)

At the given time, the scheduled deployment takes place and installs all available updates for the given machine. When the deployment is completed, you can check details on affected computers, installed updates by drilling into the appropriate deployment.

![Update Management Successfull Deployment](/images/Update-Management-Successfull-Deployment.png)

OMS Update Management is a neat solution to keep track of the update status of your Linux (and of course Windows) machines. Functionality at this time is somehow limited because you still rely on a process and/or tool to approve which updates should be installed (like WSUS for Windows or Spacewall for Linux). But you can sure get a great overview of the patch status in your heterogeneous environment and you have a single point of entry to manage system updates no matter if it is Windows or Linux or if the systems are running on premise or in public cloud.






