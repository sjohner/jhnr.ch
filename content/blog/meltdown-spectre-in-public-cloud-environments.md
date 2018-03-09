---
author: sjohner
comments: true
date: 2018-01-11 22:40:52+00:00
layout: post
slug: meltdown-spectre-in-public-cloud-environments
title: Meltdown & Spectre in public cloud environments
wordpress_id: 2333
categories:
- Azure
- Cloud
tags:
- Amazon Web Services
- AWS
- Azure
- Cloud
- GCP
- Google Cloud Platform
- Meltdown
- Oracle
- SAP
- Spectre
- Vulnerability
---

On Wednesday, Jan. 3, security researchers [publicly detailed](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html) three potential vulnerabilities named “Meltdown” and “Spectre.” A pretty detailed description of the vulnerabilities can be found on [Stratechery](https://stratechery.com/2018/meltdown-spectre-and-the-state-of-technology/) and a pretty good explanation for non-technical audience can be found on the [Cloudflare blog](https://blog.cloudflare.com/meltdown-spectre-non-technical/).
<table >
<tbody >
<tr >

<td >


**Exploited Vulnerability**



</td>

<td >**CVE**
</td>

<td >**Exploit****
Name**
</td>

<td >**Public Vulnerability Name**
</td>

<td >**Silicon Microcode Update Required on Host**
</td>
</tr>
<tr >

<td >**Spectre**
</td>

<td >[CVE-2017-5753](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5753)
</td>

<td >Variant 1
</td>

<td >Bounds Check Bypass
</td>

<td >No
</td>
</tr>
<tr >

<td >**Spectre**
</td>

<td >[CVE-2017-5715](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5715)
</td>

<td >Variant 2
</td>

<td >Branch Target Injection
</td>

<td >Yes
</td>
</tr>
<tr >

<td >**Meltdown**
</td>

<td >[CVE-2017-5754](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5754)
</td>

<td >Variant 3
</td>

<td >Rogue Data Cache Load
</td>

<td >No
</td>
</tr>
</tbody>
</table>
In an environment where multiple servers are sharing capabilities (for example virtual machines hosted in a cloud or on premise environment ), these vulnerabilities could mean it is possible for someone to access information in one virtual machine from another.

Below I tried to collect various statements from public cloud providers explaining how they protect their infrastructure from the above mentioned vulnerabilities and what are the recommended actions for their customers.


## Microsoft Azure


[https://docs.microsoft.com/de-de/azure/virtual-machines/windows/mitigate-se](https://docs.microsoft.com/de-de/azure/virtual-machines/windows/mitigate-se)


<blockquote>**The infrastructure that runs Azure and isolates customer workloads from each other is protected.** This means that other customers running on Azure cannot attack your application using these vulnerabilities.

**While an OS update is not required to isolate your applications running on Azure from other customers running on Azure, it is always a best practice to keep your OS versions up-to-date**.

In the following offerings, here are our recommended actions to update your Operating System:
<table >
<tbody >
<tr >

> <td >**Azure Cloud Services**
> </td>

> <td >Enable auto update or ensure you are running the newest Guest OS.
> </td>
</tr>
<tr >

> <td >**Azure Linux Virtual Machines**
> </td>

> <td >Install updates from your operating system provider when available.
> </td>
</tr>
<tr >

> <td >**Azure Windows Virtual Machines**
> </td>

> <td >Verify that you are running a supported antivirus application before you install OS updates. Contact your antivirus software vendor for compatibility information.

Install the [January security rollup](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002).
> </td>
</tr>
<tr >

> <td >**Other Azure PaaS Services**
> </td>

> <td >There is no action needed for customers using these services. Azure automatically keeps your OS versions up-to-date.
> </td>
</tr>
</tbody>
</table>
</blockquote>


[https://cloudblogs.microsoft.com/microsoftsecure/2018/01/09/understanding-the-performance-impact-of-spectre-and-meltdown-mitigations-on-windows-systems/](https://cloudblogs.microsoft.com/microsoftsecure/2018/01/09/understanding-the-performance-impact-of-spectre-and-meltdown-mitigations-on-windows-systems/)


<blockquote>For Windows Server, administrators should ensure they have mitigations in place at the physical server level to ensure they can isolate virtualized workloads running on the server. For on-premises servers, this can be done by applying the appropriate microcode update to the physical server, and if you are running using Hyper-V updating it using our recent Windows Update release. **If you are running on Azure, you do not need to take any steps to achieve virtualized isolation as we have already applied infrastructure updates to all servers in Azure that ensure your workloads are isolated from other customers running in our cloud. This means that other customers running on Azure cannot attack your VMs or applications using these vulnerabilities.**</blockquote>




## Amazon Web Services


[https://aws.amazon.com/de/security/security-bulletins/AWS-2018-013/](https://aws.amazon.com/de/security/security-bulletins/AWS-2018-013/)


<blockquote>**All instances across the Amazon EC2 fleet are protected from all known instance-to-instance concerns of the CVEs previously listed**. Instance-to-instance concerns assume an untrusted neighbor instance could read the memory of another instance or the AWS hypervisor. This issue has been addressed for AWS hypervisors, and no instance can read the memory of another instance, nor can any instance read AWS hypervisor memory. We have not observed meaningful performance impact for the overwhelming majority of EC2 workloads.

Recommended Customer Actions for AWS Batch, Amazon EC2, Amazon Elastic Beanstalk, Amazon Elastic Container Service, Amazon Elastic MapReduce, and Amazon Lightsail

While all customer instances are protected as described above, **we recommend that customers patch their instance operating systems to isolate software running within the same instance and mitigate process-to-process concerns of CVE-2017-5754**. For more details, refer to specific vendor guidance on patch availability and deployment</blockquote>




## Google Cloud Platform


[https://support.google.com/faqs/answer/7622138](https://support.google.com/faqs/answer/7622138)


<blockquote>**The infrastructure that runs Google Cloud products and isolates customer workloads from each other is protected.** Some Cloud products require that customers also patch/update their runtime environments and applications.</blockquote>


Check link above for more details about which products and runtime environments need patching.


## Oracle Cloud Platform


At time of writing, I did not find any information about how Oracle Cloud Platform may be affected by the above mentioned Spectre and Meltdown vulnerabilities. There are some ongoing discussions in Oracle community [here](https://community.oracle.com/thread/4110036) and [here](https://community.oracle.com/thread/4110587) but nothing specific to Oracle Cloud Platform.


## SAP Cloud Platform


SAP as well did not publish information specific to their SAP cloud offerings yet. There exists [a blog post](https://blogs.sap.com/2018/01/08/sap-contact-center-meltdown-and-spectre-vulnerability/) where they recommend all customers to apply all fixes related to Windows operating systems.