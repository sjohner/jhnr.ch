---
author: sjohner
comments: true
date: 2015-06-04 07:59:53+00:00
slug: e-mail-communication-with-cased-dimensions-free-enterprise-email-app
title: E-Mail Communication with Cased Dimensions free Enterprise Email app
categories:
- Service Manager
tags:
- Action Log
- Cased Dimensions
- Communication
- E-Mail
- Service Manager
- Service Manager 2012 R2
- Task
---

Analysts working with Service Manager are sometimes facing problems when it comes down to communication with customers or end users. Although [Service Manager Exchange Connector](http://www.microsoft.com/en-us/download/details.aspx?id=38791) does provide a simple solution for creating and updating tickets by e-mail, this does barley solve the problem from an analysts perspective.

Most of you would probably love to see Analysts being able to send messages directly from Service Manager console without having to switch between Service Manager console and Outlook every few minutes…Unfortunately there is no out of the box “messaging” solution in Service Manager. So how to deal with that?

While most of you probably attended Microsoft Ignite Conference in Chicago, [Cased Dimensions](http://www.caseddimensions.com/) announced the release of their free Enterprise Email app in the [official Service Manager blog](http://blogs.technet.com/b/servicemanager/archive/2015/04/30/cased-dimensions-is-offering-enterprise-email-as-a-free-give-away-for-service-manager.aspx). The fact that the release the app for free for all of us Service Manager folks  is surely reason enough to take a deeper look into it.<!-- more -->

After registering for your free copy of Enterprise Email, you should shortly get an email from Cased Dimensions sales team which contains your personal download link as well as a valid license key. The download contains a Windows Installer file as well as installation documentation. The **installer hast to be run on your primary Management Server**.

Normally this happens to be the first Management Server that was installed in the Management Group.  If you are not sure about your primary Management Server, check this by using the following SQL Query against the Service Manager database.

```sql
    SELECT DisplayName,[PrincipalName] FROM [MTV_Computer]
    WHERE [BaseManagedEntityId] =
    (SELECT ScopedInstanceId
    FROM dbo.[ScopedInstanceTargetClass]
    WHERE [ManagedTypeId] = dbo.fn_ManagedTypeId_MicrosoftSystemCenterWorkflowTarget())
```

The installer will guide you through the installation and also prompt you for a license key at a certain stage. Right before asking for a license key it may occur that the installation fails with the following error right after installation status shows _Retrieving Product Key…_

![Enterprise Email Setup Error](/images/enterpriseemailsetuperror-png.jpg)

After talking to the Cased Dimensions support team, this turned out to be related to Windows User Account Control (UAC) which prevents the launch of the licensing wizard. Temporarily updating security settings on the server where installation is performed should do the trick.

In order to update security settings follow these steps:

1. Go to _Control Panel -> System and Security -> Administrative Tools -> Local Security Policy -> Local Policies -> Security Options_
2. Scroll down to _User Account Control_
3. Select and double click _User Account Control: Run all administrators in Admin Approval Mode_
4. Select _Disabled_ Click _Apply_ and _Ok_
5. Restart the server
6. Run the installer again

Note: _User Account Control: Run all administrators in Admin Approval Mode_ setting can be set back to _Enabled _after installation is completed. Server will need to be restarted again.

After successful installation and restart of the Service Manager console, first thing that will catch your eye is a shiny new _Send  Email _icon in the taskbar of **every work item**. Enterprise Email targets _System.WorkItem_ class and is therefore available for all Service Manager work items, even custom work item classes based on _System.WorkItem._

[![Enterprise Email Change Request](/images/enterpriseemailchangerequest.png?w=604)](/images/enterpriseemailchangerequest.png)

Running the above task opens a form where the analyst can easily create an email message to be sent to **either any related user or any other user which exists in the Service Manager CMDB**. Of course it is possible to send messages to multiple users. Unfortunately the affected user is not added as a recipient by default for new messages. I personally think this would be a nice feature though.

Besides adding related users, it is also possible to **add related attachments** or files from the local file system. Files from local file system are added to the corresponding work item as attachments after sending the message.

[![Enterprise Email Task](/images/enterpriseemailtask.png?w=604)](/images/enterpriseemailtask.png)

Cased Dimensions Enterprise Email **uses the default Service Manager notification channel**  to send email messages. Most of the times a channel already exists for other notification subscriptions so that no additional configuration is needed there.

By default, Enterprise Email automatically **set status of the corresponding work item to _Pending User Input_ when a message is sent** by an analyst. After the end user replies, work item is changed to _Active_ status. This behavior is not configurable at the moment but according to Cased Dimensions this may be possible in future versions. Personally I think most of you are going to like this feature.
Besides changing work item status, all communications using Enterprise Email are or course logged automatically in the action log section of each work item.

**Update 6/7/2015:** The guys from Cased Dimensions are currently testing a newer version of the email client that will allow you to update the status of the work item including “custom” status of a Service and Change Request.

If you want to try Cased Dimensions free Enterprise Email app by yourself you can get the installer together with more detailed information about the app from [their Service Manager app store](http://www.caseddimensions.com/scsm_enterprise_email/). I am sure the folks from Cased Dimensions are happy to provide you with a free license!
