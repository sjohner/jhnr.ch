---
title: "Create Sp With Terraform"
date: 2019-03-16T20:53:51+01:00
draft: true
---

If you are using Azure, you most probably already stumbled upon the term Service Principal

Sometimes you will come across the requirement, that a Service Principal needs to be able to create other Service Principals. For example, think about using a Service Principal for non-interactive logons with the Azure CLI. Or using a CI/CD pipeline to deploy your infrastructure. 

Of course there are several ways to create a Service Principal in Azure. There is the portal of course, you can use ARM, Azure CLI oder PowerShell module and last but not least you can also use Terraform. In this example, I will use Terraform. Terraform is a cloud-agnostic toolkit to deal with infrastructure code. There are plenty of resources out there which explain in detail what Terraform is and how it can be used.

Terraform brings so called providers

Since some months, a specific Azure Active Directory provider is available for Terraform. It does provide basic resources to manage Azure Active Directory with Terraform. For example you can manage users and groups with this provider.

First of all we need to create the Service Principal (or enterprise application DIFFERENCE)

After that, we need to assign it a password. Terraform comes with a provider called "random" which brings a handy resource called "random_string". I recommend using this resource to create a random password for your Service Principal

That's it, you are good to go and should now be able to login with your Service Principal

az login with Service Prinipal

