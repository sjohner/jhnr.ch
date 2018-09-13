---
author: sjohner
comments: true
date: 2018-09-13 12:58:26+00:00
slug: azure-dedicated-vm-instances
title: Dedicated virtual machine instances on Azure
categories:
- Azure
- Cloud
tags:
- Virtual Machines
- Azure
- Dedicated Instances
- Isolated Instances
- Nested Virtualization
- AWS
---

Azure isolated virtual machine instances are isolated to a specific hardware type and dedicated to a single customer. When utilizing an isolated size Azure guarantees that your virtual machine will be the only one running on that specific server instance. Unfortunately Azure does not (yet) offer a broad variety of dedicated (or isolated how it is called in Azure jargon) virtual machine instances. At time of writing customers can only choose of seven different sizes when they want to use isolated virtual machines. This is ridiculously little compared to AWS, where customers can choose from [more than 70 different dedicated instance sizes](https://aws.amazon.com/ec2/purchasing-options/dedicated-instances/). AWS also offers [EC2 dedicated hosts](https://aws.amazon.com/ec2/dedicated-hosts/) which is a physical server with EC2 instance capacity fully dedicated to customer use.

As you will see in the list of isolated virtual machine sizes available on Azure, unfortunately only very big (and expensive) sizes are available. At the moment there is no possibility to run smaller isolated virtual machines on Azure. At least not without nested virtualization.

# Nested virtualization

Although Azure does not have a dedicated host offering like AWS or small isolated instance sizes, you might be able to achieve something very similar by using nested virtualization. [Azure supports nested virtual machines](https://azure.microsoft.com/en-us/blog/nested-virtualization-in-azure/) (something which is not yet available on AWS) since July 2017 for selected virtual machine sizes. For quick reference, all v3 virtual machines support nested virtualization (e.g. E\_v3 or D\_v3)

With nested virtualization it is possible to build something similar to the AWS dedicated host offering. For example take a Standard\_E64i\_v3 instance, enable nested virtualization and you are able to run approx 16 virtual machines each with 4 vCPUs, 27 GiB of memory and 100 GiB of temp storage. This is equivalent to a Standard\_DS12\_v2 instance which costs you $0.557/h. In this example you would get similar specs for $0.475/h but in a dedicated environment.

To be fair, I need to mention that with such a setup you would have to manage and operate all these nested virtual machines as well as your hypervisor manually, which is probably not why you are going to the cloud and is not the case of course if you get 16 Standard\_DS12\_v2 instances from Azure.

# Isolated Virtual Machine Sizes

If you have workloads that require a high degree of isolation from other customers (e.g. for workloads involving elements like compliance and regulatory requirements), the current (as of September 2018) isolated virtual machine offerings include:

| Size                | vCPU | Memory | Temp storage GiB | Max temp storage throughput (IOPS) | Max NICs / Network bandwidth | Price (West Europe) |
|---------------------|------|--------|------------------|------------------------------------|------------------------------|---------------------|
| Standard\_E64is\_v3 | 64   | 432    | 864              | 128000                             | 8 / 30000                    | $7.600/h            |
| Standard\_E64i\_v3  | 64   | 432    | 1600             | 96000                              | 8 / 30000                    | $7.600/h            |
| Standard\_M128ms    | 128  | 3892   | 4096             | 160000                             | 8 / 30000                    | $46.489/h           |
| Standard\_GS5       | 32   | 448    | 896              | 160000                             | 8 / 20000                    | $11.100/h           |
| Standard\_G5        | 32   | 448    | 6144             | 96000                              | 8 / 20000                    | $11.100/h           |
| Standard\_DS15\_v2  | 20   | 140    | 280              | 80000                              | 8 / 25000                    | $2.785/h            |
| Standard\_D15\_v2   | 20   | 140    | 1000             | 60000                              | 8 / 25000                    | $2.785/h            |

Below you will find a list of "standard" virtual machine offerings for comparison:

| Size                 | vCPU | Memory | Temp storage GiB | Max temp storage throughput (IOPS) | Max NICs / Network bandwidth | Price (West Europe) |
|----------------------|------|--------|------------------|------------------------------------|------------------------------|---------------------|
| Standard\_A2\_v2     | 2    | 4      | 20               | 2000                               | 2 / 500                      | $0.130/h            |
| Standard\_D4\_v3     | 4    | 16     | 100              | 6000                               | 2 / 2000                     | $0.424/h            |
| Standard\_E4\_v3     | 4    | 32     | 100              | 6000                               | 2 / 2000                     | $0.504/h            |
| Standard\_DS12\_v2   | 4    | 28     | 56               | 16000                              | 4 / 3000                     | $0.557/h            |

> When comparing above pricing you probably noticed that prices for isolated virtual machines are much higher. However when comparing vm sizes not only number of vCPU and memory is relevant but also other factors such as storage throughput, network bandwidth, max number of data disk, etc. Depending on the workload which is planned to run on a specific virtual machine, these factors might be even more important than vCPU and memory.

You can find more information about isolation as well as a current list of available isolated virtual machine offerings in Azure in the [Microsoft documentation](https://docs.microsoft.com/en-us/azure/security/azure-isolation)