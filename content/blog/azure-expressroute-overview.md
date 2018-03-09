---
author: sjohner
comments: true
date: 2017-11-17 14:58:26+00:00
layout: post
slug: azure-expressroute-overview
title: Circuits, Peerings, Pricing, Latency... All you need to know about Azure ExpressRoute
wordpress_id: 2257
categories:
- Azure
- Cloud
tags:
- Availability
- Azure
- Bandwidth
- BGP
- Circuit
- Cloud Exchange
- Cost
- ExpressRoute
- Failover
- Microsoft Peering
- Office 365
- Peering
- Pricing
- Private Peering
- Public Peering
- Round Trip Times
- RTT
- Security
- Virtual Circuit
- Virtual Network
- Virtual Network Gateway
- VPN
---

Since I was dealing with Azure ExpressRoute lately, I will try to summarize some important aspects in a blog post to provide kind of a high-level overview of Azure ExpressRoute.

To get started, one should know that there are basically three options to connect on premise networks to Microsoft Azure:



 	
  1. Internet

 	
  2. Virtual Private Network (VPN)

 	
  3. Azure ExpressRoute


Both internet and VPN connections may be suitable for you. When you are based in countries where good Internet connectivity is available (e.g. Switzerland), a VPN connection may be sufficient for most of the scenarios.

ExpressRoute basically provides a private, dedicated, high-throughput network connection between on-premises and Microsoft Azure. In general, reasons to go with ExpressRoute are the following:

 	
  * Predictable performance

 	
  * Private Layer 2 connection

 	
  * Higher bandwith (up to 10Gbps)

 	
  * Connectivity to Azure Public Services (e.g. Websites, Backup, etc.)


You may also get reasonable bandwidth and latency by using a VPN but nobody will guarantee you that this is always the case. One more thing to consider when going for a VPN connection is that bandwidth will be limited to what Azure VPN gateways have to offer (which is 1.25Gbps at time of writing).

For most companies dealing with network connectivity from on premise locations to Azure it is probably the easiest and most cost efficient way to start with VPN connections and change to ExpressRoute if cloud adoption and traffic increases.

If you are still interested in Azure ExpressRoute please continue reading and/or check out [this Azure User Group Belgium talk](https://youtu.be/zvXKoGq_9FY) by [Kristof Rennen](https://twitter.com/kristofrennen).


## What is Azure ExpressRoute?


When talking about ExpressRoute, actually in most cases an ExpressRoute circuit is referenced. An _ExpressRoute circuit_ represents a logical connection between the on-premises infrastructure and Microsoft cloud services through a connectivity provider (e.g. Equinix or interxion). So when thinking about using Azure ExpressRoute it is important to be aware that there is always a connectivity provider involved. You need to consider this when it comes to overall costs of ExpressRoute.

There are many providers offering Azure ExpressRoute connectivity with different characteristics (Cloud Exchange, Point-to-Point, Any-to-Any) and in different locations. Check out [ExpressRoute partners and peering locations](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations) or your local Microsoft representative for more details

There are many other providers offering ExchangeRoute with different pricing options and SLA. It is also possible to have ExpressRoute with different models (e.g direct point-to-point connection, Layer 3 connectivity).

It is possible to order multiple ExpressRoute circuits. Each circuit can be in the same or different regions, and can be connected to on premises through different connectivity providers. A circuit has a fixed bandwidth and is mapped to exactly one connectivity provider and one peering location (e.g. West Europe).

Below you will find kind of a big picture which hopefully gives you a good overview on how ExpressRoute works on a high level. I will reference to some of the components outlined in the picture.

[![](/images/ExpressRoute_Overview-1024x519.png)](/images/ExpressRoute_Overview.png)


## Common scenarios


There obviousely exist a bunch of reasons to implement ExpressRoute. The following scenarios are probably the most common ones:



 	
  * Storage, backup and recovery

 	
  * BI and big data (working with big amounts of data in the cloud)

 	
  * Hybrid apps (e.g. app in the cloud and database on premise)


Unlike Site2Site VPN which only works for IaaS, Azure ExpressRoute can be used with various other public services (e.g. Websites, IoT, Backup, database services).


## Peering



A peering is essentially a collection of two BGP sessions between the on premises routers and the Microsoft ExpressRoute routers. If the [Cloud Exchange Co-location or Point-to-Point Ethernet Connection connectivity models](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-connectivity-models) are used (which is the case for example when using Equinix Cloud Exchange or interxion Cloud Connect), the customer edge router (CE) would establish BGP peering with MSEEs. Provider edge customer facing (PE-CE) and Provider edge Microsoft facing (PE-MSEE) would still exist but be somewhat transparent as Layer 2 devices.

An ExpressRoute circuit can have multiple peerings associated with it:



 	
  * Azure Private Peering for Virtual Networks and Virtual Machines

 	
  * Azure Public Peering for Azure public IPs (Most of Azure services including PowerBI and Dynamics 365 for Finance and Operations)

 	
  * Microsoft Peering for Office 365 and Dynamics 365 (except Dynamics 365 for Finance and Operations which is connected through public peering)


Each of the peerings mapped to a circuit is configured identically on a pair of routers (in active-active or load sharing configuration) for high availability.

The recommended configuration is that private peering is connected directly to the core network, and the public and Microsoft peering links are connected to your DMZ.


### Private peering


Azure compute services, namely virtual machines (IaaS) and cloud services (PaaS), that are deployed within a virtual network can be connected through the private peering domain.

The private peering domain is considered to be a trusted extension of your core network into Microsoft Azure.

Bi-directional connectivity between your core network and Azure virtual networks (VNets) is possible. This peering can be used to connect to virtual machines and cloud services directly on their private IP addresses.


### Public peering


Services such as Azure Storage, SQL databases, and Websites are offered on public IP addresses. Public peering can be used to privately connect to services hosted on public IP addresses.

The public peering domain can be connected to a DMZ and connect to all Azure services on their public IP addresses from WAN without having to connect through the internet.

Connectivity is always initiated from your WAN to Microsoft Azure services. Microsoft Azure services will not be able to initiate connections into your network through this routing domain.


### Microsoft peering


Microsoft peering is used to connect to Microsoft online services (such as Office 365 services). However, Software as a Service offerings, like Office 365 and Dynamics 365, were created to be accessed securely and reliably via the Internet and most of them require Internet connection no matter if ExpressRoute is implemented or not. ExpressRoute is only recommended in specific scenarios for these applications (e.g direct networking for regulatory purposes or where a network assessment for Skype for Business connectivity requires it).

For Micrsoft Peering, Premium Addon and Authorization from Microsoft is required.


## Bandwidth options


ExpressRoutes circuits can be provisioned with the following bandwidths (as of November 2017). It is recommended to start with smaller bandwitdh and expand as ExpressRoute is up and running.



 	
  * 50 Mbps

 	
  * 100 Mbps

 	
  * 200 Mbps

 	
  * 500 Mbps

 	
  * 1 Gbps

 	
  * 2 Gbps

 	
  * 5 Gbps

 	
  * 10 Gbps


The bandwidth selected is shared across all the peerings for the circuit and it is only possible to upgrade bandwidth (no downgrade). ExpressRoute circuit bandwidth can be increased without having to tear down the connections.


## Latency


Round Trip Time (RTT) is of course heavily dependent on the locations and regions which are going to be connected. As a benchmark,  RTT is around 14ms for Zurich -  West Europe (Amsterdam) and about 32ms for Zurich - North Europe (Dublin).


## Virtual Network Gateways


A virtual network gateway is used to send network traffic between Azure virtual networks and on-premises locations. When you configure an ExpressRoute connection, you must create and configure a virtual network gateway and a virtual network gateway connection.

There exist several different gateway SKU . The higher the SKU, the more CPUs and network bandwidth are allocated to the gateway. As a result, the gateway can support higher network throughput to the virtual network. The following ExpressRoute Gateways are available at the time of writing. All of them can coexist with VPN Gateway.



 	
  * Standard SKU (1000 Mbps)

 	
  * High Performance SKU (2000 Mbps)

 	
  * Ultra Performance SKU (9000 Mbps)




## Availability and Failover



For ExpressRoute high availability, Microsoft requires a redundant pair of BGP sessions between MSEEs  and PE-MSEEs. A redundant pair of network paths is also encouraged between customer network and PE-CEs.

In terms of failover, it is possible to have two different Virtual Network Gateways - one for backup VPN and one for ExpressRoute. ExpressRoute and VPN gateways can coexist in one virtual network. However each virtual network can have only one virtual network gateway per gateway type (e.g. one ExpressRoute and one VPN Gateway).


### Site-to-Site VPN failover


It is possible to configure a Site-to-Site VPN connection as a backup for ExpressRoute. However this is only possible for virtual networks linked to the Azure private peering path. There is no VPN failover solution for services accessible through Azure public and Microsoft peerings.

However as outlined [here](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-coexist-resource-manager), there are some limitations which need to be considered:



 	
  * **Transit routing is not supported.** You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.

 	
  * **Basic SKU gateway is not supported.** You must use a non-Basic SKU gateway **for both** the ExpressRoute gateway and the VPN gateway.

 	
  * **Only route-based VPN gateway is supported.** You must use a route-based VPN Gateway.

 	
  * **Static route should be configured for your VPN gateway.** If your local network is connected to both ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network to route the Site-to-Site VPN connection to the public Internet.

 	
  * **ExpressRoute gateway must be configured first and linked to a circuit.** You must create the ExpressRoute gateway first and link it to a circuit before you add the Site-to-Site VPN gateway.




## Pricing


Of course cost is an important aspect of Azure ExpressRoute. There are various aspects to be aware of when calculating pricing of an Azure Express Route implementation:



 	
  * Express Route circuit

 	
  * Express Route provider

 	
  * Virtual network gateway

 	
  * Land line to entry point / peering location




### Pricing example


The following example pricing is based on a real sample and should give you a rough overview of the costs which might occur for an ExpressRoute implementation (as of writing in November 2017). Pricing is based on an sample Azure ExpressRoute implementation in Switzerland and of course prices vary according to the chosen provider, location of the infrastructure and Azure region you want to connect to.

In addition to the monthly fees shown below, one-time fees for providing cages in the provider peering location may apply as well. Depending on the chosen provider you may be able to just have one physical connection which can be shared to connect to additional cloud providers like Amazon AWS and Oracle.

You will notice that the cost for the Azure ExpressRoute circuits are much less than the cost for the provider.

[table id=1 /]


### Zones


Azure ExpressRoute pricing also depends on which zone is going to be connected.

For ExpressRoute, the following sub-regions correspond to Zones 1, 2, 3 and Government, where a sub-region is the lowest level geo-location that you may select to deploy your applications and associated data (e.g. West US, North Europe).

·         **Zone 1:** West US, East US, North Central US, South Central US, East US 2, Central US, West Europe, North Europe, Canada East, Canada Central

·         **Zone 2:** East Asia, Southeast Asia, Australia East, Australia Southeast, Japan East, Japan West, Korea Central, Korea South, India South, India West, India Central

·         **Zone 3:** Brazil South

·         **Government Zone:** US Gov Iowa, US Gov Virginia

For ExpressRoute deployments in region Europe (sub-regions North Europe and West Europe), outbound data transfer is therefore charged with Zone 1 pricing (CHF 0.0226 per GB as of Novemnber 2017).


### Pricing Options


There exist two pricing plans to chose from. Both options **include unlimited inbound data transfer**.

·         Metered Data (No outbound data transfer included, outbound data transfer is charged at a rate based on corresponding Zone)

·         Unlimited Data (Unlimited outbound data transfer included)

In general the _Unlimited Data_ pricing option would be a better choice if you have high levels of utilization, and the _Metered Data_ pricing option would be preferable for low levels of utilization.

Typically metered option is cheaper as long as less than 60% of the maximum bandwidth is used per month. Keep in mind that it is not possible to switch back from unlimited to metered connection.


## Connection to Azure regions


Once connected to an ExpressRoute location, **users can connect to other regions in the same geo at no additional cost** over existing plan charges. For example, once connected to an ExpressRoute location in Europe (North Europe, West Europe), customers can send or retrieve data to or from any Azure region in Europe without the need to pay an additional fee on top of their existing plan charges.

With ExpressRoute Premium Addon, an ExpressRoute circuit created in any region will have access to resources across any other region in the world. For example, a virtual network created in West US can be accessed through an ExpressRoute circuit provisioned in West Europe


## Security


I will not go into details about security since I am in no way an expert in this field. However there is one thing which sometimes is not thought of in the first place: Although ExpressRoute is a private connection, traffic flowing over the network is not encrypted. Encryption in transit of course be achieved by encrypting traffic flowing over the connection. For example by deploying physical and/or virtual encryption devices on both sides (e.g. Fortinet, F5, Steelhead, etc).

