---
author: sjohner
comments: true
date: 2013-10-04 14:00:20+00:00
slug: quick-post-delete-scsm-work-items-created-before-specific-date-with-powershell
title: 'Quick Post: Delete SCSM Work Items created before specific date with PowerShell'
categories:
- Service Manager
tags:
- PowerShell
---

Based on a [post by Pete Zerger on systemcentercentral.com](http://www.systemcentercentral.com/scsm-2012-deleting-work-items-in-bulk-in-system-center-2012-service-manager-with-powershell/) I created a PowerShell command which deletes all Work Items of a specific class in your CMDB when created before a specific date.

```powershell
    #Set date
    $BeforeDate = Get-Date("31/1/2013").ToString("MM/dd/yyy HH:mm:ss")
    #List incidents which were created before $BeforeDate
    Get-SCSMClassInstance -Class (Get-SCSMClass -Name System.WorkItem.Incident) | Where {($_.CreatedDate -lt $BeforeDate)} | sort-object CreatedDate | ft -Property Id,Title,CreatedDate
    #Remove instances
    Get-SCSMClassInstance -Class (Get-SCSMClass -Name System.WorkItem.Incident) | Where {($_.CreatedDate -lt $BeforeDate)} | Remove-SCSMClassInstance
```

The commands could probably be optimized by using the _-Filter_ parameter instead of the _Where_ clause.
