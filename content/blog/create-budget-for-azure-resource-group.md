---
author: sjohner
comments: true
date: 2018-07-12T22:43:55+02:00
layout: post
slug: create-budget-for-azure-resource-group
title: "Create budget for Azure resource group and get notified in case of overspending"
categories:
- Azure
- Cloud
tags:
- Azure
- Cost
- Management
- Notification
- Cloudyn
- Spending
- Limit
- API
- ARM
- Postman
- JSON
- REST
---

Azure (or any other cloud) customers normally are very interested in how much money a given project or application is burning. Microsoft provides multiple instruments to manage cost including [Azure Cost Management](https://azure.microsoft.com/en-us/services/cost-management/) (powered by Cloudyn). Azure Cost Management can help you to manage cost for your Azure and other cloud environments by providing you the tools to monitor, allocate, and optimize your cloud costs. For example, Cloudyn helps you manage budgets at a workload level and monitoring the spend on cloud services.

However in my understanding, Azure Cost Management (or Cloudyn) targets cloud cost management on a higher level where for example a department wants to know what services they are running in which subscriptions and how they could optimize their spending as a whole. In my opinion, Cost Management is not (yet) ready to be used in a very granular fashion for example when it comes down to analyzing costs based on resource groups. But very often resource groups serve as cost boundaries and workload therefor share a subscription. Yet they need to be budgeted for individually. That's where budgets kick in. For making these granular controls available, Azure offers an ARM API to set and manage a budget at the subscription, resource group and even resource scope. 

With the budgets API you can for example address the following scenarios:

* A budget for the subscription with no constraints.
* A resource group budget with no constraints.
* A budget for multiple resource groups within a subscription.
* A budget for multiple resources within a resource group or a subscription.
* Budgets based on usage on a subscription or resource group.

When looking at budget API there are some things to keep in mind:

1. Budgets are currently __only supported for Enterprise customers__. The budget API is part of the Azure consumption APIs which currently __only support Enterprise Enrollments and Web Direct Subscriptions__ (with a few exceptions). The APIs are continually updated to support other types of Azure subscriptions.
2. Calls to the budgets API enforce a user context. So you will need to call the budgets API in the context of a user and not a service principal. [ARMClient](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) is a good option to get a user token to use with the request
3. Usage based budgets require a meter, the constraint enforced by the budget API is to enforce a single unit of measure for all meters within a budget. For instance, if you are budgeting your compute hours, you cannot include meters for networking that measure the GB transferred in the same budget.

I recommend watching [Managing costs with the Azure Budgets API and Action Groups](https://channel9.msdn.com/Shows/Azure-Friday/Managing-costs-with-the-Azure-Budgets-API-and-Action-Groups) on Azure Friday to get a good overview on the budgets API.

Creating a budget for a resource group by using ARM REST API is fairly easy once you have an access token for the ARM API. Check out this [ARMClient: a command line tool for the Azure API](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) post by [David Ebbo](https://twitter.com/davidebbo) for details on how to get a token with ARMClient or [Using the Azure ARM REST API – Get Access Token](https://blogs.technet.microsoft.com/stefan_stranger/2016/10/21/using-the-azure-arm-rest-apin-get-access-token/) by [Stefan Stranger](https://twitter.com/sstranger).

Basically you can get an access token using ARMClient with the following command.

```
ARMClient.exe token <SubscriptionId>
```

I use [Postman](https://www.getpostman.com/) to talk to the ARM API. If you do as well, [Using Postman to call Azure REST APIs](https://blogs.msdn.microsoft.com/benjaminperkins/2017/01/03/using-postman-to-call-azure-rest-apis/) might be a good starting point. This post also shows how to get the access token using Fiddler.

The resource group for which you want to create a budget has to be present in the given subscription. All whats needed now is a PUT request which contains the budget definition in the request body.

```
PUT https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<RGname>/providers/Microsoft.Consumption/budgets/mybudget?api-version=2018-03-31
```

To authenticate yourself to the ARM API you will need to add an appropriate Authorization header which contains your bearer access token (that you got with ARMClient for example). Furthermore Content-Type header has to be set to _application/json_

```
Authorization: bearer <Your access token>
Content-Type: application/json
```

[![Screenshot of Postman request header](/images/budget-postman-header.png)](/images/budget-postman-header.png)

The body of the request contains the budget definition as json. Check out the [Budgets API documentation](https://docs.microsoft.com/en-us/rest/api/consumption/budgets) for details. When updating a budget definition you will need to specify the eTag of the current budget to handle concurrent update scenarios. This field will be used to determine whether your updating the latest version of the budget or not.

A note about start date and end date: the budget start date __must be on or after June 1, 2017__. Future start date should not be more than three months. Furthermore, __past start date should be selected within the time grain period__. If no end date is provided, it will default to 10 years from the start date.

{{< gist sjohner e62a851cdcae8dc7c198b2c8d9d3505f >}}

[![Screenshot of Postman request body](/images/budget-postman-body.png)](/images/budget-postman-body.png)

In the above sample, I added _Owner_ and _Contributor_ as contact roles. This means that once the budget reaches its notification threshold all the Contributors and Owners of the given resource group are notified. They will receive an e-mail similar to the one below making sure they are aware that the defined threshold is reached and outlining the actual spending. Instead of using contact roles you might want to check out contact groups where you can specify [action groups](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups) which might already be available in your environment. The cool thing about action groups is that you can trigger for example an Azure Automation runbook or a Logic App to take some action or remediation.

[![Screenshot of budget alert e-mail notification](/images/budget-alert-notification.png)](/images/budget-alert-notification.png)

Besides creating your budgets you may want to update or even delete it at some point in time. Updating can easily be done by just sending another PUT request which contains your existing budget but with the updated values in the body of the request. Same as creating a new budget but since you are going to update an existing budget you will need to include the current _eTag_ of the budget in the request body.

You can find the current eTag by querying the API for information about a specific budget:

```
GET https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<RGname>/providers/Microsoft.Consumption/budgets/mybudget?api-version=2018-03-31
```

You can also list all existing budgets for a specific subscription whith the following request:

```
GET https://management.azure.com/subscriptions/<SubscriptionId>/providers/Microsoft.Consumption/budgets?api-version=2018-06-30
```

Deleting a budget is also fairly simple:

```
DELETE https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<RGname>/providers/Microsoft.Consumption/budgets/mybudget?api-version=2018-06-30
```

Besides using the ARM REST API directly, you can also create a budget by creating an ARM template and apply it to a resource group. This might come in handy if you are automating budget creating within a resource group provisioning script for example. Microsoft already provides an ARM template in their [Azure quickstart templates repository on Github](https://github.com/Azure/azure-quickstart-templates/tree/master/create-budget) which can be used for that purpose.

{{< gist sjohner 305945169b9bece38ccd0416af829e3a >}}
