---
author: sjohner
comments: true
date: 2013-04-11 19:08:50+00:00
layout: post
slug: how-to-change-scsm-admin-group
title: How to Change SCSM Admin Group
categories:
- Service Manager
tags:
- Admin Group
- Database
- SQL
---

Today I ran into a issue where I had to change the SCSM admin group defined at installation time. Since there was accidentally used a Active Directory global security group instead of a domain local security group when installing SCSM, I got curious if it is possible to change the group in SCSM configuration. Well, it is not that easy but there is a pretty handy way of changing the group by altering the SCSM database. Credits for this solution go to Dieter Gasser who published his findings on his [blog](http://blog.dietergasser.com/2012/05/30/scsm-2012-upgrade-group-is-not-a-valid-active-directory-group-or-user/).

If you ever run into the situation that you have to change your SCSM admin group, you can do this by altering ___MOMManagementGroupInfo___ table which is found in both _DWStagingAndConfig_ as well as _ServiceManager_ databases.<!-- more -->

If you open your SQL Management Studio you should find your  ___MOMManagementGroupInfo___ table easily. Once you are there, right click on the table and choose to edit top 200 rows. No worries, there is only one row in this table so you should not have a problem to find the appropriate row ;)

[![Edit DWH Admin Group](/images/editdwhadmingroup.png)](/images/editdwhadmingroup.png)

Now you have to change the value of the _MOMAdminGroup_ field to contain your new Active Directory group. Make sure you include the domain name (e.g. SCSMLABscsmadmins)

[![Change Admin Group](/images/changeadmingroup.png?w=560)](/images/changeadmingroup.png)

For those of you who prefer to update the database by using a SQL query, I posted one below. You just have to change the database to use (either _ServiceManager_ or _DWStagingAndConfig_), the name of your old Active Directory group you want to replace and of course the name of your new Active Directory group.
 

    
    USE [ServiceManager]
    GO
    
    UPDATE [dbo].[__MOMManagementGroupInfo__]
    SET [MOMAdminGroup] = '<YOUR OLD GROUPNAME>'
    WHERE [MOMAdminGroup] = '<YOUR NEW GROUPNAME>'
    GO



