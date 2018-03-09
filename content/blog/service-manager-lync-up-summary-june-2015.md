---
author: sjohner
comments: true
date: 2015-07-07 15:58:39+00:00
layout: post
slug: service-manager-lync-up-summary-june-2015
title: Service Manager Lync Up Summary June 2015
wordpress_id: 1110
categories:
- Service Manager
tags:
- Connect
- LyncUp
- Summary
- Update Rollup 7
- UR7
---










A little late but I was finally able to listen to the recording of the June Service Manager Lync Up call :-) The call mainly focused on key improvements in UR7 which is planned for release on July 28th.
As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). **Next meeting will take place on July 21st.**


<!-- more -->


### Recent Service Manager blog posts and recordings


Service Manager related blog posts:



	
  * Create a Configuration item and Primary Owner View
[http://blogs.technet.com/b/antoni/archive/2015/06/05/howto-create-a-basic-configuration-item-view-and-typeprojection-showing-computers-with-their-primary-user-andcustodian-owner.aspx](http://blogs.technet.com/b/antoni/archive/2015/06/05/howto-create-a-basic-configuration-item-view-and-typeprojection-showing-computers-with-their-primary-user-andcustodian-owner.aspx)

	
  * Include Details from Parent Workitem in Notifications
[http://blogs.technet.com/b/antoni/archive/2014/05/24/include-details-from-parent-work-items-in-notifications.aspx](http://blogs.technet.com/b/antoni/archive/2014/05/24/include-details-from-parent-work-items-in-notifications.aspx)

	
  * SM Supports SQL Always on with Named Instances
[http://blogs.technet.com/b/servicemanager/archive/2015/04/29/service-managersupports-sql-always-on-with-named-instances.aspx](http://blogs.technet.com/b/servicemanager/archive/2015/04/29/service-managersupports-sql-always-on-with-named-instances.aspx)




Ignite Sessions about System Center on Channel 9:






	
  * Datacenter Management from the Microsoft Point of View
[http://channel9.msdn.com/events/Ignite/2015/THR0488](http://channel9.msdn.com/events/Ignite/2015/THR0488)

	
  * Windows Server & System Center Futures—Bring Azure to your Datacenter (Platform Vision & Strategy)
[http://channel9.msdn.com/events/Ignite/2015/FND1451](http://channel9.msdn.com/events/Ignite/2015/FND1451)




### Key Improvements for UR7




As already announced in May, Update Rollup 7 is planned for release on July 28th. Final payload may change.




One key improvement in this UR will be a fix for data warehouse issue where sync job enters an unstable state and hangs. This seems to happen typically when someone updates a Management Pack or re-registers the data warehouse. During this operation, the Management Pack deployment fails because of the system trying to create primary/foreign key for a table that already has primary/foreign key in it. This may cause data not being synced to the data warehouse properly.




Another great improvement will be the support of concurrent work item updates on SDK level. This has been asked a lot by the community and details can be found on Connect:


Data Collision Exception / Item has been changed by another user or process
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1062768](https://connect.microsoft.com/WindowsServer/Feedback/Details/1062768)


Currently when updating any entity in the SDK, Last Modified Date is passed while reading the item. If the date does match with the value from SCSM database, an exception is raised. New behavior would be to try to merge changes with current values and try to resubmit changed values. This should fix about 90% of cases where data collision happens.





### Upcoming console improvements




In addition to the SDK level fixes, there are basically two big changes coming to UI to enhance experience:






	
  * Refresh the form on submit to get the latest value on the form from database

	
  * Show a warning to user when the form reloads with new values




If you would like to see these additional improvements in action, Abhishek did a nice demo which is available with the recording. Unfortunately since these are pretty big fixes for the console they might not make it into UR7.










