---
title: "Test Azure Infrastructure With Inspec"
date: 2018-10-22T10:35:34+02:00
draft: true
---

There are many different ways of working with Azure or any other cloud environment. If you are working with any cloud environment, you probably have noticed that each way comes with some advantages and disadvantages.

# Portal
Most people are pretty comfortable with the portal experience of a specific cloud provider and it definitively has some big advantages:
* It is browser based
* You dont need to setup anything
* It is great for visually exploring and inspecting services. 

But as you are avancing in your cloud journey, you will see that using the portal comes with some cevats:
* Everything is performed manually
* Therefore it is error prone
* It lacks process integration (DevOps, ITSM)

# Scripting
Probably the next step in evolution from manually using the cloud provider portal is to script the whole thing
* Scripts can be integrated into processes (DevOps, ITSM)
* It is less error prone since there is no human interaction

However scripts can
* be very complex and therefore need a lot of scripting knowledge
* require logic to be hand built
* be as flexible as you require it to be

# Template based deployments
With template based deployments you describe what you want to build. This is basically known under the term _Infrastructure as Code (IaC)_. Azure Resource Manager (ARM) does this for Azure, Cloud Formation for AWS and with Terraform you can deploy IaC to different (cloud) providers.
* IaC can be integrated into processes (DevOps, ITSM)
* It is less error prone since there is no human interaction
* It can handle more complex logic
* Sometimes you get state management (Terraform)

When using template based deployments you will probably run into the following disadvantages:
* Deep knowledge of templating and the corresponding environment / cloud provider is needed
* Template based deployments are not as flexible as for example script based deployments

At some point in time you will probably start using template based deployments simply because in larger environments you will need deployments to be integrated into processes and because your deployments will become more and more complex. However as with application code, infrastructure code needs to be tested before it is going to be deployed into production. And furthermore it needs to be monitored for compliance while it lives in production. You probably dont want to be the company that deploys a public AWS bucket and [leaks 128 million records of American voters](https://www.zdnet.com/article/security-lapse-exposes-198-million-united-states-voter-records/)...

# Chef Inspec
That's where Chef Inspec comes into play. InSpec is Chef’s open-source language for describing security and compliance rules. With Inspec you can track the compliance of your infrastructure based on predefined policies. For example, you can describe compliance controls in InSpec and integrate these tests into any stage of your deployment pipeline or choose from a set of pre-packaged InSpec profiles. Tests are very self-explanatory, simple and Ruby-based.

There exist similar solutions like the [Secure DevOps Kit for Azure](https://azsk.azurewebsites.net/index.html) but InSpec supports all major operating systems and platforms (AWS, Azure, GCP, Vmware, etc.), allowing you the freedom to run compliance and security tests pretty much anywhere. Inspec tests let you verify your expected state against the current state of your systems. In terms of cloud providers this means that the tests are run against the cloud providers API. This means, that infrastructure has to be already deployed to the cloud before it can be verified against your expected state. As outlined in the image below, this means that you for example run Inspec tests after you executed a *terraform apply* command and continuosely to check that infrastructure is compliant.

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Inspec can be downloaded for various operating systems from [Inspec website](https://downloads.chef.io/inspec). Since Inspec is completely open source you might want to check on its source code which [can be found on Github](https://github.com/inspec/inspec-azure)

If you just want to try Inspec, instead of downloading and installing it manually, I recommend using Azure Cloud Shell. Azure Cloud Shell comes pre-installed with all the nifty tools like Git, Azure CLI, Chef, Ansible and even Inspec. Check out [Azure Cloud Shell documentation](https://docs.microsoft.com/en-us/azure/cloud-shell/features) to get an idea on what tools are pre-installed in Cloud Shell.

After you got your Cloud Shell ready or installed Inspec on your machine, you need to give it access to your Azure resources. To allow InSpec to authenticate to your Azure account you will need to [create an Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2Fen-us%2Fazure%2Fazure-resource-manager%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest).

Make sure you take note of the newly generated access key. Afterwards create an Azure credentials file in ~/.azure/credentials

This credentials file should have the following structure:

```bash
[<SUBSCRIPTION_ID>]
client_id = "<APPLICATION_ID>"
client_secret = "<KEY>"
tenant_id = "<DIRECTORY_ID>"
```

Now your are ready to run InSpec against your Azure resources.

Before you can test your infrastructure, create a new profile by executing the following command. This will create a new folder in your current directory which contains several new files and folders. In my samples, I will create a profile to check on my GRC Pool resources running on Azure.

```bash
stefan@Azure:~/learn-inspec$ inspec init profile azure_grcpool
Create new profile at C:/learn-inspec/azure_profile
 * Create directory controls
 * Create file controls/example.rb
 * Create file inspec.yml
 * Create directory libraries
 * Create file README.md
 * Create file libraries/.gitkeep
```

Make sure you are removing the example.rb control before running inspec for the first time. Then open the newly created inspec.xml file which is found in the profile directory and change default values to something meaningful

InSpec Azure resources are available as resource pack and to use them in your controls, you will need to create an inspec profile which depends on the given *inspec_azure* resource pack. While running the tests the proper extension will be downloaded from Github and used. For a full list of available Azure resources, check out [Inspec documentation](https://www.inspec.io/docs/reference/resources/#azure-resources).

# Include gist profile

Besides the profile you will need at least one Inspec control which defines what InSpec is going to test in your subscription. Controls are put in the controls folder of your newly created Inspec profile. Below you can find some examples of different controls which I use to test my GRC pool resources running on Azure. As you can see there are already many resources available for Azure and I expect that to grow over time. Some of the available resources are coming in very handy. For example you can easily check whether your subnet has a network security group assigned and if this network security group allows SSH and RDP traffic from the internet. That pretty much exactly covers the scenarios we want to check on from a security point of view. For a complete list of available resources in InSpec azure library you can refer to the [documentation](https://www.inspec.io/docs/reference/resources/#azure-resources) or the [inspec-azure source on Github](https://github.com/inspec/inspec-azure)

#include gist controls

Once you have created at least one control, go and run a check to see if your profile is valid or if it has any errors or warnings.
 ```bash
sjohner@donald:~/Source/learn-inspec$ inspec check azure_grcpool
Location:    azure_grcpool
Profile:     azure_grcpool
Controls:    5
Timestamp:   2018-10-26T15:22:03+02:00
Valid:       true

No errors or warnings
```

Perfect! To run the checks against your Azure account, execute your InSpec profile with Azure as target provider.
```bash
sjohner@donald:~/Source/learn-inspec$ inspec exec azure_grcpool -t azure://

Profile: Checking compliance of GRC Pool resources (azure_grcpool)
Version: 0.1.0
Target:  azure://08f66ad0-fa16-4b2d-b046-0a04879c3b01

  ✔  azurerm_virtual_machine: Check vm 'azl73627'
     ✔  'azl73627' Virtual Machine should exist
     ✔  'azl73627' Virtual Machine location should eq "westeurope"
     ✔  'azl73627' Virtual Machine name should eq "azl73627"
     ✔  'azl73627' Virtual Machine type should eq "Microsoft.Compute/virtualMachines"
     ✔  'azl73627' Virtual Machine installed_extensions_types should include "OmsAgentForLinux"
     ✔  'azl73627' Virtual Machine installed_extensions_names should include "OmsAgentForLinux"
  ✔  exists_resourcegroup: Check rg 'grcpool-prod-rg'
     ✔  Resource Groups with name == "grcpool-prod-rg" should exist
  ✔  azurerm_subnet: Check subnet 'default'
     ✔  'default' subnet should exist
     ✔  'default' subnet name should eq "default"
     ✔  'default' subnet type should eq "Microsoft.Network/virtualNetworks/subnets"
     ✔  'default' subnet address_prefix should eq "10.2.1.0/24"
     ✔  'default' subnet nsg should eq "subnet-default-nsg"
  ✔  azurerm_virtual_network: Check vnet 'grcpool-prod-rg-vnet'
     ✔  'grcpool-prod-rg-vnet' Virtual Network should exist
     ✔  'grcpool-prod-rg-vnet' Virtual Network location should eq "westeurope"
     ✔  'grcpool-prod-rg-vnet' Virtual Network name should eq "grcpool-prod-rg-vnet"
     ✔  'grcpool-prod-rg-vnet' Virtual Network type should eq "Microsoft.Network/virtualNetworks"
     ✔  'grcpool-prod-rg-vnet' Virtual Network subnets should eq ["default"]
     ✔  'grcpool-prod-rg-vnet' Virtual Network address_space should eq ["10.2.1.0/24"]
     ✔  'grcpool-prod-rg-vnet' Virtual Network dns_servers should eq ["1.1.1.1", "1.0.0.1"]
     ✔  'grcpool-prod-rg-vnet' Virtual Network enable_ddos_protection should eq false
  ✔  azurerm_network_security_group: Check nsg 'subnet-default-nsg'
     ✔  'subnet-default-nsg' Network Security Group should exist
     ✔  'subnet-default-nsg' Network Security Group should allow ssh from internet
     ✔  'subnet-default-nsg' Network Security Group should not allow rdp from internet
     ✔  'subnet-default-nsg' Network Security Group security_rules should not be empty
     ✔  'subnet-default-nsg' Network Security Group default_security_rules should not be empty


Profile: Azure Resource Pack (inspec-azure)
Version: 1.2.0
Target:  azure://08f66ad0-fa16-4b2d-b046-0a04879c3b01

     No tests executed.

Profile Summary: 5 successful controls, 0 control failures, 0 controls skipped
Test Summary: 25 successful, 0 failures, 0 skipped
```

Inspec is a fast and easy tool that can help you on the field of automated compliance as a code. By supporting many different platforms it enables organizations to use one tool for compliance testing throughout their infrastructure landscape. In the end it might be an option to have tests running in a continuous fashion using a CI pipeline so that any changes to your (Azure) cloud infra is tested in a non-prod environment and then promoted to production.