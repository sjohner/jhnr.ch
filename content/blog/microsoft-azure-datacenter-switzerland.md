---
author: sjohner
comments: true
date: 2019-02-25T15:22:08+01:00
slug: microsoft-azure-datacenter-switzerland
title: About Microsoft Azure datacenters in Switzerland
categories:
- Azure
- Cloud
tags:
- Virtual Machines
- Azure
- Switzerland
- Office 365
- Dynamics
- IaaS
- PaaS
---

This post should serve as an overview on all things related to Microsoft datacenters in Switzerland. [Microsoft announced](https://news.microsoft.com/fr-ch/2018/03/14/microsoft-cloud-services-expand-in-europe-including-new-swiss-datacenters/) that they will expand its cloud services to Switzerland in 2019 by adding two cloud regions located in the cantons of Geneva and Zurich. Azure, Office 365 and Dynamics 365 will be delivered from Switzerland. [Sources say](https://www.inside-it.ch/articles/52231) that Microsoft is building its Swiss cloud services with [e-shelter](https://www.e-shelter.de/de/location/datacenter-zurich) for Zurich region and [Safe Host](https://www.safehost.com/) for Geneva region (which is actually most probably in the canton of Vaud, as the [Safe Host Datacenter in Gland](https://www.safehost.com/en/data-centre/sh2) is much bigger and newer than the one in Geneva).

[![Azure Datacenter Switzerland Overview](/images/azure-switzerland-datacenter.png)](/images/azure-switzerland-datacenter.png)

The two regions will be paired within Switzerland which enables geo-replication for backup and disaster recovery scenarios.

# What services will be available in these newly announced Swiss regions?

Services will be available in the following order:

1. Azure
2. Office 365
3. Dynamics 365

[According to Bloomberg](https://www.bloomberg.com/news/articles/2018-12-11/ubs-said-to-prepare-cloud-move-to-hidden-microsoft-data-centers) UBS reached a deal worth hundreds of millions of Swiss francs to use Azure cloud services. Whether the deal took place before or after the decision to build Azure regions in Switzerland is unknown. Although it seems that UBS does not put customer data into the cloud, it is likely that they will be consuming services out of the newly announced Swiss Azure datacenters. In this case, UBS may also have a significant influence on the order in which services are offered and probably even on the location where the services are going to be deployed. Seems like Zürich will be getting the most love in the beginning and [certain services will mainly be introduced in "Switzerland North" region](https://www.isolutions.ch/2019/02/19/microsoft-datacenter-schweiz-informationen/).

With UBS as primary launch partner and anchor tenant for Microsoft datacenters in Switzerland, one can assume that Microsoft focuses on FSI (Financial Services Industry) market in Switzerland. This industry as well as health care and government is also very sensitive about Swiss data protection laws and FINMA regulations, which makes it a perfect target group for Swiss datacenters.

Following services are confirmed to be available from the start (see [this interview with Primo Amrein](https://www.procloud.ch/aktuelle-informationen-zu-den-microsoft-datacenter-schweiz-azure-schweiz-office-365-schweiz-dynamics-schweiz/) and [this one with Marc Holitscher](https://www.procloud.ch/aktuelle-informationen-zu-den-microsoft-datacenter-schweiz-azure-schweiz-office-365-schweiz-dynamics-schweiz/) for reference):

* Virtual machines
* Storage (including GRS)
* Backup
* Networking
* SQL Database
* Cosmos DB

Most probably the term "Networking" includes other IaaS related services like Load Balancer, VPN Gateway and maybe Express Route since this is a requirement for many enterprise grade customers. And since the regions are designed for geo-redundancy, the term "Backup" might include Azure Site Recovery as well.
Should the assumption with the focus on FSI prove true, we can probably also expect data privacy relevant services such as Key Vault, SAP and maybe HPC services in the near future.

By looking at service availability in France regions, we might get a glympse on what we **won't** see very soon in Switzerland:

* GPU enabled virtual machines
* AI services like Azure Machine learning service or bot service
* IoT services like IoT Hub, IoT Central and Time Series Insights
* Container services like AKS, Container Instances and Container Registry

## Update April 2019

During Microsoft Tech Summit Switzerland in Bern, [Primo Amrein announced and published further details](https://www.linkedin.com/pulse/more-details-services-upcoming-microsoft-swiss-primo-amrein/) on what services will launch in the upcoming Microsoft datacenters. For Azure those will be:

* [Application Gateway](https://azure.microsoft.com/en-us/services/application-gateway/)
* [Cosmos DB](https://azure.microsoft.com/en-us/services/cosmos-db/)
* [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
* [Azure Backup](https://azure.microsoft.com/en-us/services/backup/)
* [Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/)
* [Express Route](https://azure.microsoft.com/en-us/services/expressroute/)
  * At the moment, Interxion is listed as the only connectivity provider in the [official documentation](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations). More providers will be available at launch.
* [Azure Cache for Redis](https://azure.microsoft.com/en-us/services/cache/)
* [Key Vault](https://azure.microsoft.com/en-us/services/key-vault/)
* [Load Balancer](https://azure.microsoft.com/en-us/services/load-balancer/)
* [Service Bus](https://azure.microsoft.com/en-us/services/service-bus/)
* [Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric/)
* [Storage](https://azure.microsoft.com/en-us/services/storage/)
* [VPN Gateway](https://azure.microsoft.com/en-us/services/vpn-gateway/)
* [Virtual Machines](https://azure.microsoft.com/en-us/services/virtual-machines/)
  * A0 - A7-series (general purpose)
  * Av2-series (general purpose)
  * B-series (general purpose, burstable)
  * D-series (general purpose)
  * DS-series (general purpose)
  * DSv2-series (general purpose)
  * DSv3-series (general purpose)
  * Esv3-series (memory optimized)
  * Ev3-series (memory optimzed)
  * F-series (compute optimized)
  * Fs-series (compute optimized)
* [Virtual Networks](https://azure.microsoft.com/en-us/services/virtual-network/)
* [Azure Site Recovery](https://azure.microsoft.com/en-us/services/site-recovery/)
* [Azure Monitor](https://azure.microsoft.com/en-us/services/monitor/)

More Azure services will become available in the coming months after the official launch:

* Azure Web Apps
* SAP on Azure (with SAP HANA certified M-series virtual machines)
* SQL Data Warehouse
* Azure Kubernetes Services (AKS)
* Azure database for PostgreSQL / MySQL / MariaDB
* Media Services
* Power BI

Office 365 will launch with the following services providing local data residency:

* Exchange Online mailbox content
* SharePoint Online
* OneDrive for Business


# When will these services become available?

Microsoft does not communicate specific dates but [they did state]((https://www.procloud.ch/aktuelle-informationen-zu-den-microsoft-datacenter-schweiz-azure-schweiz-office-365-schweiz-dynamics-schweiz/)) that the first available service will be Azure somewhere in 2019, Office 365 will follow 3-6 month later and last but not least Dynamics will become available again 3-6 months later. This means that if Azure would be launched any time in summer, Office 365 might not be available from Switzerland until late 2019 or Q1/2020.

# What to expect regarding prices?

No word from Microsoft about pricing yet. But based on the experience of the also relatively new French datacenters, one can assume that Office 365 will be available at the same price from the Swiss datacenters as it is already offered from the European datacenters.

The same probably goes for Azure pricing. In this sense, services from the Swiss Azure cloud can be expected to add about 10%-20% to the prices in the North and West Europe regions - depending with which region you are going to compare prices - as it is the case for the French datacenters. So if you want to get a glimpse on Azure Switzerland pricing, check out pricing for French regions on [Azure Calculator](https://azure.microsoft.com/en-us/pricing/calculator/).

# Will my data stay in Switzerland?

[In an interview](https://www.itreseller.ch/Artikel/87153/Microsoft_Stillstand_ist_keine_Option.html), Marc Holitscher (Microsoft Switzerland CTO at this time) assured that data at-rest within Azure Switzerland regions will stay in Switzerland. However for services which are operated on a global scale (like Azure Active Directory) data will still leave Switzerland regions though (as it does in other regions as well). Microsoft assures in their contracts to comply with the relevant laws of specific regions. Accordingly, the Swiss Data Protection Act also applies to data stored in Switzerland regions. To operate the Swiss data centers, Microsoft founded a separate company under Swiss law. However as of my understanding, this company only operates the datacenters. The services on top are probably still going to be operated by Microsoft Corp.

Most important Office 365 services should be available from Switzerland as well whereas other O365 services which are only available from US (like Sway or Yammer) will not. Moving Office 365 core data to new Switzerland regions should be possible as already known for other regions. Data moves can take up to 24 months to complete. See [Microsoft documentation](https://docs.microsoft.com/en-us/Office365/Enterprise/moving-data-to-new-datacenter-geos) for more details.
