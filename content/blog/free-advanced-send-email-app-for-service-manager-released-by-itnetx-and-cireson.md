---
author: sjohner
comments: true
date: 2014-01-16 14:21:11+00:00
layout: post
slug: free-advanced-send-email-app-for-service-manager-released-by-itnetx-and-cireson
title: Free Advanced Send Email App for Service Manager released by itnetx and Cireson
wordpress_id: 813
categories:
- Service Manager
tags:
- Action Log
- Attachment
- Custom Form
- E-Mail
- Exchange Connector
- Management Pack Bundle
- SCSM Console
- SendMail
- Task
---

Probably anyone who has ever used Service Manager knows the pain involved when communicating with customers or end users. Although Service Manager Exchange connector does provide a simple solution for creating and updating tickets by e-mail, this does barley solve the problem from an analysts perspective.

Analysts often have to check with someone (mostly the affected user in case of incidents) to get all the information they need or to send instructions. It would be nice if they could do this directly from Service Manager console because switching between Service Manager console and Outlook every few minutes can be pretty painful…

I recently wrote a [blog post](http://scsmlab.com/2013/07/15/effective-e-mail-communication-with-service-manager-send-message-from-scsm-console/) about a messaging solution I created for one of our customers to address these issues. Many of you seem to have the same problems when it comes down to communication with end users and were asking for a public release of the solution. I am happy to announce that [itnetx together with Cireson now released a free app](http://www.prweb.com/releases/2014/01/prweb11465908.htm) based on this solution – **the Advanced Send Email app** - that simplifies communication between analysts and end users.

<!-- more -->


All you need to do to get this working is:

  1. Download the Advanced Send Email app from [itnetx website](http://www.itnetx.ch)
  2. Import Management Pack by using Service Manager Console  
[![Import Advanced Send Email MPB](/images/importadvancedsendemailmp.png?w=604)](/images/importadvancedsendemailmp.png)
  3. Configure SMTP server settings
  4. Create message templates if needed
  5. No more messing around with end user communication :-)

Give it a try in your lab environment and let us know what you think. There is also a short documentation written by [Travis Wright ](http://twitter.com/radtravis)included in the download. In addition to this I will now explain what you get with this free app.

The free [Advanced Send Email app ](http://www.itnetx.ch)basically has the following features:

  * Send e-mail to affected user by default, but be able to add other e-mail addresses or to delete the affected user's address.
  * Possibility to add additional e-mail addresses in TO and CC
  * Message subject contains the incident's title but can be altered.
  * Predefined e-mail templates are available to the analysts
  * Attachments can be added to the message
  * The analyst is free to choose whether the message body should be logged in the incident's action log or not
  * No need to edit XML files, just import the MPB and you are done

[![Advanced Send Email Form](/images/advancedsendemailform.png)](/images/advancedsendemailform.png)

Take a look at the image above, this is the form which is shown to the analyst when starting the Send Mail task.

[![Advanced Send Email Task](/images/advancedsendemailtask.png?w=604)](/images/advancedsendemailtask.png)

The form automatically adds the e-mail address of the affected user of the selected incident as TO address. More e-mail addresses can be added by separating them by semicolon. The same way multiple CC addresses can be added. User input for TO and CC fields is validated to only allow valid E-mail address formats.

The subject field is populated with the incident's title but can be modified by the analyst. As the message is sent, the incident ID is included in the subject so that a possible reply from the end user can be handled by [Service Manager Exchange Connector](http://www.microsoft.com/en-us/download/details.aspx?id=38791).

The message field can contain up to 4000 characters and predefined templates can be chosen by the analyst. Templates shown in the dropdown are added dynamically to the dropdown based on their DisplayName attribute.

[![Advanced Send Email Settings](/images/advancedsendemailsettings.png?w=604)](/images/advancedsendemailsettings.png)

As shown in the picture above, the app can easily be configured using Admininistration settings. If your SMTP server requires authentication you can configure username and password to meet this requirements. It is also possible to use an Office 365 account to send E-mails. If no SMTP user is configured, the Service Manager Workflow Account will be used to send E-mails.

Furthermore it is possible to define a Notification Template prefix to filter the list of the available templates based on their DisplayName attribute and to add a prefix to the subject for every message sent.
