---
author: sjohner
comments: true
date: 2015-03-29 22:04:41+00:00
layout: post
slug: recursively-get-all-review-activities-related-to-a-workitem
title: Recursively get all Review Activities related to a WorkItem
wordpress_id: 1032
categories:
- Service Manager
tags:
- PowerShell
- Review Activity
- Service Request
- SMlets
- System Center
---

In the Service Manager world, review activities (RA) are an important part of most deployments. A review activity is kind of an approval item which is used to control execution of a business process. Often when providing items for end users in the service catalog it is necessary to somehow review input of submitted requests. This is where you specify the actual users that must approve a request.

![Review Activity](/images/reviewactivity.png)

Think about the following example: One important part of Service Manager is the service catalog which is a collection of catalog items that the IT staff and infrastructure provides and makes available to end users. So for example end users are able order new software from the service catalog based on a Service Manager offering. Of course some of the offered software catalog items require a license which of in turn costs some money. Depending on what software is ordered, costs for a license can be pretty high. And since these costs are mostly charged to the department of the ordering person, the appropriate department head would probably like to review these requests and either approve or reject the order. This is where Review Activities come into play :-)

<!-- more -->


With this in mind, you can imagine that sometimes it is pretty handy to somehow be able to get all the review activities related to a certain service request. However when using System.WorkItemContainsActivity relationship on a Service Request, it returns only activities which are directly related to the corresponding Service Request. This does not handle Review Activities which are nested within other activities such as Parallel or Sequential Activities.




But fortunately like Service Requests, Activities are WorkItems too. As you can see in the following picture, the Activity class inherits from System.WorkItem class. Therefore activities can use the same _System.WorkItemContainsActivity_ relationship to host other activities like Service Requests do. In fact this makes it perfect to create a recursive function to get all the Review Activities :-)




![Activity Base Class](/images/activitybaseclass.png?w=604)




Below you will find a small PowerShell snippet, which contains a function to recursively get all nested Review Activities related to a WorkItem. You can easily alter the function to get other activity types by just changing _ReviewActivityClass_ variable to a class of your needs. I also uploaded the complete PowerShell script to [TechNet Gallery here](http://gallery.technet.microsoft.com/Service-Manager-Get-f829b2bb).


One thing to mention is that we need to use ArrayList datatype instead of the default System.Array to be able to add and remove elements. When you create an array in PowerShell you normally use the default type which is of a fixed size. [The MSDN page for the ISFixedSize Property](http://msdn.microsoft.com/en-us/library/system.array.isfixedsize(v=vs.110).aspx) helps to explain this. Note that it supports the modification of existing elements, but not the addition or removal of others.

 

    
    #Import SMLets module
    import-module smlets
    
    #Get necessary classes and relationships
    $ReviewActivityClass = Get-SCSMClass -name ^System.WorkItem.Activity.ReviewActivity$
    $WorkItemRelatesToWorkItemRelClass = Get-SCSMRelationshipClass -name ^System.WorkItemRelatesToWorkItem$
    $WorkItemContainsActivityRelClass = Get-SCSMRelationshipClass -name ^System.WorkItemContainsActivity$
    
    #Define ArrayList to store Review Activities. We need to use ArrayList instead of System.Array to be able to add and remove elements.
    #The MSDN page for the ISFixedSize Property helps to explain this. Note that it supports the modification of existing elements,
    #but not the addition or removal of others.
    [System.Collections.ArrayList]$ReviewActivities = @()
    
    #Recursively get all Review Activities for specific WorkItem
    Function getReviewActivities {
        Param
            (
            [Parameter(Mandatory=$true)]$WorkItem
            ) 
    
    	#Get all activities contained in the given WorkItem
    	$ContainedActivities =  @()
    	$ContainedActivities += Get-SCSMRelatedObject -Relationship $WorkItemContainsActivityRelClass -SMObject $WorkItem
    
    	#Check if an activity is a Review Activity and add it to the ArrayList
    	foreach ($Activity in $ContainedActivities)
    	{
    		if($Activity.ClassName -eq $ReviewActivityClass.Name)
    		{
    			$Index = $ReviewActivities.Add($Activity)
    		}
    		#If not Review Activity, recursively call getReviewActivities to get RAs in nested Activities
    		else
    		{
    			getReviewActivities -WorkItem $Activity
    		}
    	}
    }




To execute the function you just have to provide a WorkItem object e.g. a Service Request.


    
    
    $ServiceRequestObj = Get-SCSMObject -Class (Get-SCSMClass -Name System.Workitem.ServiceRequest$) -Filter "Id -eq SR12345"
    
    getReviewActivities -WorkItem $ServiceRequestObj
    



