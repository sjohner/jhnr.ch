---
author: sjohner
comments: true
date: 2018-07-30T20:13:56+02:00
slug: migrate-existing-azure-payg-subscriptions-to-ea
title: "Migrate existing Azure pay as you go subscriptions to Enterprise Agreement"
categories:
- Azure
- Cloud
tags:
- Subscription
- Migration
- Enterprise Agreement
- Pay as you go
- Account
- RBAC
- Tenant
---

There will be a point in time where you have to migrate an existing Azure pay as you go (PAYG) subscription to your Enteprise Agreement. Imagine your company bought a small and innovative startup which is working with Azure. However they did not have an Enterprise Agreement with Microsoft yet and are paying their Azure bills by credit card. Not what you want in an enterprise environment.

Basically there are two options how you can integrate PAYG subscriptions into an existing Enterprise Agreement. Either you can move the Azure account with all associated subscriptions or you can move individual subscriptions.

# Moving the account

You can add the account which is associated with the pay as you go subscription to the Enterprise Agreement. This will move all the subscriptions under this account and convert them to EA subscriptions.

If the existing account is moved with all its subscriptions to an EA Enrollment then there is no impact on the subscriptions service endpoint or Identity endpoint and RBAC role assignments are preserved. Its just a change on the billing endpoint wherein your subscription is converted to Microsoft Azure Enterprise offer and billing is be tagged to EA. Everything in your subscriptions keeps working as it is.

If you decide to add the account to EA and automatically convert all associated subscription to EA, follow below steps. Note that you probably need to enable Microsoft accounts.

[![Azure enterprise portal authentication level](/images/ea-portal-enrollment-authlevel.png)](/images/ea-portal-enrollment-authlevel.png)

From the Enterprise Portal, click Manage

1. Click the Account tab
2. Click + Add an account
3. Enter the Microsoft Account or Work or School Account associated with the existing account
4. Confirm the Microsoft Account or Work or School Account associated with the existing account
5. Provide a name you would like to use to identify this account in reporting
6. Click Add
7. You can add an additional account by selecting the + Add an Account option again, or return to the homepage by selecting the Admin button
8. If you click to view the Account page, the newly added account will appear in a "Pending" status

[![Add account in Azure enterprise portal](/images/ea-portal-add-account.png)](/images/ea-portal-add-account.png)

Confirm Account Ownership

1. Log into the email account associated to the Microsoft Account or Work or School Account you provided
2. Open the email notification titled something like "Invitation to Activate your Account on the Microsoft Azure Service from Microsoft Volume Licensing"
3. Click the Log into the Microsoft Azure Enterprise Portal link in the invitation
4. Click Sign in
5. Enter your Microsoft Account or Work or School Account and Password to login and confirm account ownership

[![Add account in Azure enterprise portal](/images/email-activate-ea-account.png)](/images/email-activate-ea-account.png)

# Moving individual subscriptions

If you want to move only specific subscriptions from another account to an Enterprise Agreement you would have to work with Microsoft Support to complete the transfer.

>Be aware that when moving a specific subscription from one Account X associated to Tenant A to another Account Y associated to another Tenant B (which is under EA enrollment), then the existing role assignments would be lost, since in this scenario the subscription is moving across different tenants.
However if Account X and Y both are under same Azure tenant then RBAC role assignment wont be lost, since the subscription is moved between two different accounts under same tenant.

If you decide to move only specific subscriptions from  PAYG account to an Enterprise Agreement, contact [Microsoft Support](https://support.microsoft.com/en-us/getsupport?tenant=classiccommercial&locale=en-us&supportregion=en-us&pesid=15736&sd=&oaspworkflow=start_1.0.0.0&wf=0&ccsid=636238732967168112&forceorigin=esmc).