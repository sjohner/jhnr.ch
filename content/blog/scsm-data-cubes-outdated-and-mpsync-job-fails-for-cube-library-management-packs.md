---
author: sjohner
comments: true
date: 2013-08-16 14:53:41+00:00
slug: scsm-data-cubes-outdated-and-mpsync-job-fails-for-cube-library-management-packs
title: SCSM Data Cubes outdated and MPSync Job fails for Cube Library Management Packs
categories:
- Service Manager
tags:
- Data Warehouse
- Database
- Debugging
- Management Pack
- Service Pack 1
- SQL
---

As you probably know, you can do some pretty cool reporting stuff with Service Manager 2012 by using data cubes. Data cubes allow IT staff to get relevant information by using Excel to slice and dice data from SCSM data warehouse.

If you like to learn more about data cubes in Service Manager, check out this helpful [series of blog posts by Marcel Zehner](http://marcelzehner.ch/2012/01/02/data-cubes-in-service-manager-2012-part-13-overview-and-basic-handling-of-data-cubes)

However, lately I faced the problem that current data was missing. As you may have read in the above posts, there exist some data warehouse jobs which are responsible of processing these cubes regularly so that they contain current data.

In my case Service Manager was unable to run these jobs which of course caused reporting not being available for the days the jobs were not running. Starting the cube processing jobs manually unfortunately did not help. I just got the following error message, saying that no resource for the specified culture or neutral culture could be found.

[![Error Processing Config Item And Work Item Cube](/images/errorprocessingconfigitemandworkitemcube.png)](/images/errorprocessingconfigitemandworkitemcube.png)

First I was a bit confused by this error message. But after some time, I noticed that some Management Packs did not sync successful to the data warehouse. Namely it was Service Manager Configuration Management Cube Library together with some other MP's like Service Manager Service Catalog Cube Library.

[![DWH Management Packs](/images/dwhmanagementpacks.png?w=696)](/images/dwhmanagementpacks.png)

Stopping and restarting the Data Warehouse jobs did not help in any way so I had to dig a bit deeper.

I noticed that every time I restarted the deployment of a failed Management Pack, the following error and warning messages in the Event Log of the Data Warehouse Management Server appeared.

[![Error PK already Defined](/images/errorpkalreadydefined.png?w=696)](/images/errorpkalreadydefined.png)

The errors state that a table already has a primary key defined on it.

```sql
Data Access Layer rejected retry on SqlError:
Request: Ral_ExecuteSql -- (statement=exec('IF OBJECT_ID(''[dbo].[WorkItemImpactsServiceFact_2013_Feb]'') IS NULL
BEGIN
CREATE TABLE [dbo].[WorkItemImpactsServiceFa...), (RETURN_VALUE=0)
Class: 16
Number: 1779
Message: Table 'ReviewerVotedByUserFact_2013_Jul' already has a primary key defined on it.
```

A quick internet search discovered that this error was known and happened in some environments after the installation of Service Pack 1 but mostly the _System Center Data Warehouse Base Library_ Management Pack is affected. It seems that the MP Sync process tries to create a Primary Key although one already exists, which causes the sync to fail. There exists a [Microsoft KB article ](http://support.microsoft.com/kb/2853442)which describes how to resolve the issue by running a query on DWRepository database to get the some SQL scripts which will drop and add constraint of the primary key of fact tables on DWRepository database. However, since _System Center Data Warehouse Base Library_ was synced properly I did not use the script provided by Microsoft in this case.

To recover from the various sync errors, it is necessary to drop the primary key from the tables shown in the various error messages . Use the following SQL query to drop a constraint. You should of course adapt the query to alter the tables shown in the error messages on your system.

```sql
USE DWRepository ALTER TABLE
dbo.ReviewActivityHasReviewerFact_2013_Jul drop constraint
[PK_ReviewActivityHasReviewerFact_2013_Jul]
```

In my case I had to drop about 20 different PK's to get rid of the first bunch of error messages. Try to redeploy a failed Management Pack afterwards. Because of the dependencies of Management Packs it could happen that you will see some new errors caused by other Management Packs. After a while and some redeployments you should see that more and more Management Packs deploying properly to the data warehouse.

Just when I thought it was over, another error message popped out in the event log when I tried to sync a Cube Library Management Pack (like _Service Manager Configuration Management Cube Library_)

[![Error Cube](/images/errorcube.png)](/images/errorcube.png)

This one was pretty clear in saying that the existing Data Cube has to be deleted. This can easily be done with the SQL Management Studio. Fletcher Kelly wrote a [nice blog post](http://fskelly.wordpress.com/2013/02/27/cubes-missing-from-data-warehouse-and-throwing-error-from-within-the-scsm-console/) on how to recover from this error.

[![DWH Delete Cube](/images/dwhdeletecube.png)](/images/dwhdeletecube.png)

Finally all the Management Packs were deployed properly to the Data Warehouse and after processing the data cubes, current data was available. By the way: Did you notice the last processed date after you started the deployment of a cube which was previously deleted in your SQL database? ;-)

[![Cube Running](/images/cuberunning.png?w=696)](/images/cuberunning.png)