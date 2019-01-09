---
author: sjohner
comments: true
date: 2019-01-09T20:22:08+01:00
slug: microsoft-azure-datacenter-switzerland
title: Overview of Microsoft Azure datacenters in Switzerland
categories:
- Azure
- Cloud
tags:
- Virtual Machines
- Azure
- Switzerland
- Office 365
- Dynamics
---

This post should serve as an overview on all things related to Microsoft datacenters in Switzerland. [Microsoft announced](https://news.microsoft.com/fr-ch/2018/03/14/microsoft-cloud-services-expand-in-europe-including-new-swiss-datacenters/) that they will expand its cloud services to Switzerland in 2019 by adding two cloud regions located in the cantons of Geneva and Zurich. Azure, Office 365 and Dynamics 365 will be delivered from Switzerland. [Sources say](https://www.inside-it.ch/articles/52231) that Microsoft is building its Swiss cloud services with [e-shelter](https://www.e-shelter.de/de/location/datacenter-zurich) for Zurich region and [Safe Host](https://www.safehost.com/) for Geneva region.

[![Azure Datacenter Switzerland Overview](/images/azure-switzerland-datacenter.png)](/images/azure-switzerland-datacenter.png)

# What services will be available in these newly announced Swiss regions?

Services will be available in the following order:

1. Azure
2. Office 365
3. Dynamics 365

Azure will [reportedly](https://www.procloud.ch/aktuelle-informationen-zu-den-microsoft-datacenter-schweiz-azure-schweiz-office-365-schweiz-dynamics-schweiz/) launch with IaaS offerings (Virtual Machines, Storage, Backup, Compute, Networking, etc.). However, you can also expect popular PaaS services like the Azure SQL Database to be available soon.

[According to Bloomberg](https://www.bloomberg.com/news/articles/2018-12-11/ubs-said-to-prepare-cloud-move-to-hidden-microsoft-data-centers) UBS reached a deal worth hundreds of millions of Swiss francs to use Azure cloud services. Whether the deal took place before or after the decision to build Azure regions in Switzerland is unknown. Although it seems that UBS does not put customer data into the cloud, it is likely that they will be consuming services out of the newly announced Swiss Azure datacenters. In this case, UBS may also have a significant influence on the order in which services are offered.

# When will these services become available?

Microsoft does not communicate specific dates but [they did state]((https://www.procloud.ch/aktuelle-informationen-zu-den-microsoft-datacenter-schweiz-azure-schweiz-office-365-schweiz-dynamics-schweiz/)) that the first available service will be Azure somewhere in 2019, Office 365 will follow 3-6 month later and last but not least Dynamics will become available again 3-6 months later. This means that if Azure would be launched any time in summer, Office 365 might not be available from Switzerland until late 2019 or Q1/2020.

# What to expect regarding prices?

No word from Microsoft about pricing yet. But based on the experience of the also relatively new French datacenters, one can assume that Office 365 will be available at the same price from the Swiss datacenters as it is already offered from the European datacenters.

The same probably goes for Azure pricing. In this sense, services from the Swiss Azure cloud can be expected to add about 10% to the prices in the North and West Europe regions as it is the case for the French datacenters.

# Will my data stay in Switzerland?

[In an interview](https://www.itreseller.ch/Artikel/87153/Microsoft_Stillstand_ist_keine_Option.html), Marc Holitscher (Microsoft Switzerland CTO at this time) assured that data within Azure Switzerland regions will stay in Switzerland. However for services which are operated on a global scale (like Azure Active Directory) data will still leave Switzerland regions though (as it does in other regions as well). Microsoft assures in their contracts to comply with the relevant laws of specific regions. Accordingly, the Swiss Data Protection Act also applies to data stored in Switzerland regions.

Most important Office 365 services should be available from Switzerland as well whereas other O365 services which are only available from US (like Sway or Yammer) will not. Moving Office 365 core data to new Switzerland regions should be possible as already known for other regions. Data moves can take up to 24 months to complete. See [Microsoft documentation](https://docs.microsoft.com/en-us/Office365/Enterprise/moving-data-to-new-datacenter-geos) for more details.
