---
author: sjohner
comments: true
date: 2018-02-13 16:45:39+00:00
layout: post
title: Why Microsoft Azure
wordpress_id: 2300
categories:
- Azure
- Cloud
tags:
- AI
- Amazon
- AWS
- Azure
- Azure AD
- Azure Stack
- Chatbot
- Cloud
- Cognitive Toolkit
- Contributions
- Dark Fiber
- Edge
- Edge Computing
- Gartner
- Google
- Google Cloud Platform
- Hybrid Cloud
- IaaS
- Marea
- Microsoft
- Microsoft Graph
- Mobile Application Development
- Multi-Cloud
- Office 365
- Open Source
- PaaS
- Public Cloud
- Research
- SaaS
- Visual Studio
- Xamarin
---

I had several arguments lately about why a business should consider Microsoft Azure for IaaS and PaaS in place of any other big cloud provider (aka AWS or Google). Let me start with the following: My boss once told me that choosing a cloud provider seems to be like a marriage. The choice you make may (hopefully) be the right one for the rest of your life. Consider that a business which is working with one cloud provider only, establishes a very tight relationship with its services. Working with cloud services, especially PaaS offerings, pretty much locks you into the providers ecosystem. As with marriage, breaking up may be ending by fighting the famous Wars of Roses.

Compared to choosing the right cloud provider, finding the right girl or boy pretty much seems like a piece of cake. Love at first sight. Boooom! No one - or the minority of people at least - seriously considers comparing features, revenue, testimonials or market share before asking him or her to tie the band of their life's. Yes, maybe there are actually some people doing this but lets forget about them for now...

Choosing THE ONE cloud provider for your business is hard if not impossible. For example how are you going to match up the providers against each other? Of course there is this **benefit analysis** where you evaluate different criteria and in the end a number pop out and tells you what provider you need to go for. But what criteria are to be used? Marketshare? Number of data centers? Quality of support? Number of Services? Gartner Magic Quadrant classification? And how the heck do you compare services which evolve faster than lightning?

The question seems obvious: why not just go with AWS? That is what probably most people think when asking the above questions. Maybe they are right. I mean what could possibly go wrong? AWS clearly is leading the cloud market in **terms of IaaS and PaaS**. In the [Gartner Magic Quadrant for Cloud Infrastructure as a Service, Worldwide](https://www.gartner.com/doc/reprints?id=1-2G2O5FC&ct=150519) AWS is the uncontested leader.

[![Research image courtesy of Gartner, Inc.](https://www.gartner.com/resources/315200/315215/315215_0001.png;wa4036249d8f17fcd1?reprintKey=1-2G2O5FC)](https://www.gartner.com/resources/315200/315215/315215_0001.png;wa4036249d8f17fcd1?reprintKey=1-2G2O5FC) Source: Gartner (June 2017)[/caption]

Same picture when looking at market share. [According to a study of Synergy Research](https://www.srgresearch.com/articles/cloud-market-keeps-growing-over-40-amazon-still-increases-share), although Microsoft, Google and others are growing their revenues and continue to gain market share, the reality is that AWS remains bigger than its next five largest competitors combined.

[![Cloud Provider Competitive Positioning](/images/CIS_Q317.jpg)](/images/CIS_Q317.jpg)

In terms of revenue we get pretty much the same result: [Amazon Web Services is now a $17.5 billion business](http://www.businessinsider.com/amazon-web-services-2017-revenue-2018-2), and nobody else is even close. I hear you say that Microsoft’s commercial cloud run-rate for 2018 is north of $20 billion now. Valid point but remember that we are still primarily talking about IaaS and PaaS now. Microsoft does not disclose Azure revenue and it is believed that they earn the majority of that sum of money from its SaaS business — namely Office 365, Dynamics 365 and its EMS Suite — and not from Azure [which is believed to contribute "only" about $5 billion to the commercial cloud earnings](http://www.zdnet.com/article/microsoft-commercial-cloud-azure-vs-amazon-web-services-nice-storyline-but-misguided-comparison/).

Given the above numbers it is very hard to ignore AWS when thinking about cloud infrastructure and platform. When a company leads a market the way AWS does for cloud infrastructure and platform it probably cannot be ignored. And besides all the numbers, it is fair to say that AWS does have great products and services. [Many big businesses such as Netflix, GE, Spotify and many others ](https://aws.amazon.com/solutions/case-studies)rely heavily on AWS infrastructure which I think definitively points out that it is a very mature and reliable platform.

Although AWS dominates the IaaS and PaaS cloud market I am conviced that Azure is very competitive and that there are some very good reasons to consider Microsoft Azure as your cloud provider. I will try to point out some of them below and I am sure that there are many more good reasons why you may fall for Azure instead of other providers. Please share your thoughts in the comments section.

In order to follow up on my statements about provider lock-in, remember that as a large enterprise you may not want to focus on just one cloud provider. [According to Gartner Analyst Elias Khnaser](https://www.eliaskhnaser.com/) "Multicloud is no longer a matter of if, it is a matter of when". With respect to cloud providers, polygamy may be a conceivable option...

**SaaS applications**

Remember the above mentioned Microsoft commercial cloud run-rate of more than $20 billion for financial year 2018? [Microsoft is leading the SaaS market followed by Salesforce and Adobe](https://www.srgresearch.com/articles/microsoft-leads-saas-market-salesforce-adobe-oracle-and-sap-follow). That is actually a big argument why you want to choose Microsoft Azure as one of your cloud providers (or even as the one provider when thinking in SME size). Unlike other cloud providers, Microsoft is deeply involved in all three layers of the cloud (IaaS, PaaS and SaaS).

[![Enterprise SaaS Growth and Market Leaders Q2 2017](/images/19553521_14732414839984_rId12.jpg)](/images/19553521_14732414839984_rId12.jpg)

When talking about SaaS, of course there is Office 365. [Nearly everyone is using it](http://www.zdnet.com/article/microsoft-office-365-now-has-120-million-business-users/), including very large companies [like General Electric](https://news.microsoft.com/2015/07/20/ge-chooses-microsoft-office-365-for-employee-collaboration-and-productivity/) where Microsoft supplies over 300,000 employees with Office 365. Azure offers a high integration into Office 365 platform. Serverless offerings like Azure Functions, Logic Apps and EventGrid act as entry point into Azure for Office 365 data (e.g. Built-in bindings and connectors for Azure Functions and Logic Apps to Office 365). From there, Azure provides tight integration into other Azure services like database, analytics, intelligence and storage.

This might be an important point when thinking about line-of-business applications which require tight integration into Office products like Mail or Sharepoint. Productivity apps in particular can be very interesting to run on Azure. Think about chatbots which can integrate into Skype for Business and are able to get information about their users from Microsoft Graph. And they are not only able to get information about Office 365 users but can also generate content on Sharepoint or update calendars. Together for example with Bing Speech API you will be able to build a highly integrated app which can leverage the full power of the Microsoft Cloud.

Another example is Azure Active Directory. Azure AD is basically a SaaS application which can be used independetly of Azure. No need to use Azure services when using Azure AD for example for your Office 365 deployment. Azure AD is highly integrated in Azure and Office 365. This means you can authenticate your users through Azure AD no matter if they are just using Office 365 but also other line-of-business applications running on Azure. If you have Office 365 and Azure AD already in place, identity management for Azure basically becomes a piece of cake and vice versa.

Of course it is perfectly possible to run above mentioned scenarios in any other cloud. Microsoft does not block you in any way from using Office 365 but running your apps on AWS or Google Cloud Platform. However they are investing a lot of time and money to integrate Office 365 and other Microsoft services into Azure to make it as easy as possible for users to connect these services to their apps running on Azure. The integrated portfolio of Mixed Reality- & AI-based productivity tools together with workplace and cloud solutions may be a significant competitive advantage compared to cloud providers without an extensive workplace solution (like AWS) and pure cloud players without hybrid concepts (like Google).

Besides Office 365, Microsoft provides a vast amount of SaaS offerings which are sometimes focused on consumer space rather than enterprises. But Azure can definitively benefit from technology and collected experiences when running SaaS applications at such a massive scale.  Think about Xbox, LinkedIn, Dynamics and Bing just to name a few. Although apps like Office 365 often run in separate datacenters, many of them use Azure in some kind of way and they benefit from Azure services. Xbox and IoT Hub for example [both use Azure CosmosDB](https://azure.microsoft.com/en-us/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/) for some of their backend services and recently it was published [how Xbox telemetry processing pipeline was built with Azure HDInsight](https://azure.microsoft.com/en-us/blog/how-xbox-uses-hdinsight-to-drive-analytics-on-petabytes-of-telemetry-data/). Microsoft heavily relies on Azure and based on the feedback they get from the teams using it in their own products, the services can be improved and further developed.

**Hybrid Cloud and Edge Computing**

In many cases, regulations, previous investments or even psychology are big road block on your journey to the cloud. Hybrid Cloud gives you the option to adopt the public cloud although you are not able to move all and everything out into the public cloud. make use of best of both world.

In a hybrid world, the same application might be run either on-premises or in the cloud. Accordingly, an important part of creating a hybrid cloud is setting up a DevOps process that works identically for both. Microsoft is passionate about the value of hybrid cloud and is committed to deliver hybrid solutions for Azure. They offer a broad range of cloud and on-premises technologies that work together in a coherent way. Many of products and services address both on-premise environments as well as the cloud (e.g. SQL Server, Hyper-V, Exchange Server, Active Directory, etc.).

Providing consistent identity whether a user accesses applications on-premise or in the cloud is a core part of a successfully hybrid cloud. Azure Active Directory can provide a consistent identity and offers secure single sign-on, automated provisioning of new users, and more.



 	
  * Use Azure Active Directory for a single sign-on experience across on-premises and cloud apps from virtually anywhere.

 	
  * Azure AD supports cloud applications from Microsoft, including Office 365 and Dynamics 365. It also supports many other SaaS offerings, including Google Apps, Salesforce CRM, Dropbox, Box, Slack, Service Now, Workday, and many more.

 	
  * Access on-premises web applications securely from anywhere without a VPN with Azue AD Application Proxy


Microsoft’s broad support for hybrid identity is pretty unique among major cloud platform providers. For example, AWS Identity and Access Management focuses on managing identity for AWS itself and for resources running on AWS. Unlike Azure AD, it doesn’t provide a general solution for single sign-on that works across cloud applications from many vendors.

In a hybrid world, the same application might be run either on-premises or in the cloud. Therefore an important part of creating a hybrid cloud is setting up a DevOps process that works identically for both worlds. Azure together with its hybrid offerings (like Azure Stack) and its tight integration of Visual Studio can help with that.

Azure Stack lets developers build and deploy software the same way, whether it runs on-premises or in the cloud so that it is possible to implement consistent DevOps mechanisms across hybrid cloud. Compared to a provider which focused solely on the cloud. Amazon Web Services offers CloudFormation, for example, a technology that’s broadly analogous to ARM. But CloudFormation is limited to defining environments in the cloud—you need to do something different for an on-premises solution.

Azure has a broad offering of Hybrid cloud services for infrastructure and development stacks. Azure management and security services in general are designed for a hybrid and heterogeneous world. They can even extend to management of Windows and Linux systems running at hosting services or on other cloud platforms such as AWS.

Clearly Azure Stack is their flagship when it comes to hybrid cloud, but don't miss out on plenty of other services which offer hybrid cloud functionality:

 	
  * Azure Service Fabric

 	
  * Azure Monitor

 	
  * Azure Security Center

 	
  * Azure File Sync

 	
  * Azure Backup

 	
  * Azure Automation

 	
  * …


Also when it comes to edge computing, the hybrid cloud strategy of Microsoft might be a big difference compared to other cloud providers. Some are saying that Edge Computing will be the next wave of innovation. [Gartner defines Edge Computing as key platform-enabling technology](https://www.forbes.com/sites/louiscolumbus/2017/08/15/gartners-hype-cycle-for-emerging-technologies-2017-adds-5g-and-deep-learning-for-first-time).

[![Gartner Hype Cycle for Eamerging Technologies 2017](/images/hype-cycle-for-emerging-technologies-2017.jpg)](/images/hype-cycle-for-emerging-technologies-2017.jpg)

Clearly Azure Stack is made to bring intelligence to the edge while working closely with the cloud. For example if low latency is expected or data must be available in real time, edge computing is probably your best choice. Cloud connectivity may go down sometimes but factories need to continue running.

In such scenarios, applications should be managed the same way using the same tools, no matter whether they run on the edge or in the cloud. With Azure Stack Microsoft has an edge computing offering in place which can operate autonomously, with or without a cloud connectivity and already brings a lot and will bring a lot more Azure services to the edge of your data center (e.g. Azure Functions, IoT, Machine Learning, etc.)

**Open Source**

One of the best things about Microsoft these days is their commitment to Open Source. Open Source means more than just free software. Open Source is about contributing to the community. That's where Microsoft and especially Azure plays the cards. In contrary to other cloud providers, Microsoft is not just using Open Source technology to drive their business but is also heavily contributing to the open source community. [Microsoft is the biggest contributor to open source software as of 2017 when analyzing Github contributions](https://medium.freecodecamp.org/the-top-contributors-to-github-2017-be98ab854e87). Not even Red Hat, a company with commitment to contribute all of its code under an open source license comes close to the number of Microsoft contributors. Microsoft invests a lot of resources and people in contributing to open source projects. Not just because they have to build a good representation among in the open source space, but also because with Satya Nadella as CEO, Microsoft is truly transforming to a company which heavily relies on Open Source software and believes in giving back to the community.

[![Satya Nadella talking about Linux](/images/microsoftloveslinux.0.0-1024x576.jpg)](/images/microsoftloveslinux.0.0.jpg)

Microsoft is backing many open source projects like [Google's Kubernetes](http://www.zdnet.com/article/microsoft-to-work-with-google-on-kubernetes-container-management-platform/) and [Docker.](http://www.zdnet.com/article/docker-container-support-coming-to-microsofts-next-windows-server-release/) Microsoft is a Platinum member of the Linux Foundation and the Cloud Native Computing Foundation (whose projects include Kubernetes) and a Gold member of Cloud Foundry. Same time, the .NET is going open-source. Microsoft is also open-sourcing the full [server-side .NET core stack](http://www.zdnet.com/article/microsoft-to-open-source-more-of-net-and-bring-it-to-linux-mac-os-x/) and porting it to Linux and Mac OS X. Microsoft has already [open-sourced a tool for moving virtual machines](http://www.zdnet.com/article/free-tool-simplifies-azure-migration-between-data-centers/) from one Azure data center to another, plus other [.NET code and tools](http://www.zdnet.com/article/microsoft-open-sources-more-of-its-net-technologies/), and [many developer tools](http://www.zdnet.com/article/microsoft-still-open-sources-more-technologies-than-many-think/)

However it seems that other cloud providers like AWS and Google have (yet) a better standing in Open Source community than Microsoft Azure. In terms of AWS they seem to be monetizing open source projects more profitably than the vendors who actually invented them or released them first under an open source license. For example MySQL on AWS RDS, AWS RedShift based on PostgreSQL and AWS Elastic MapReduce.

And as SAP Global Vice President PaaS [Floyd Strimling questions on Twitter](https://twitter.com/PlatenReport/status/846093997433114626): “Does anyone really care [who contributes the most] as long as the cloud is scalable, available, and cost competitive?”. Maybe not. One could argue that developers just want innovation at a minimum of price and are not interested in any open source commitment of the cloud provider. However I think that a lot of enterprises are building on open source and there is a lot of innovation happening in open source. With their commitment to Open Source, Microsoft clearly shows that they are willing to make that innovation possible.

**Mobile Application Development Platform**

I am not a developer so I am probably not perfectly qualified to rate the developer experience on Azure. However, Gartner positions Microsoft in the Leader quadrant when it comes to Mobile Application Development Platforms. [According to Gartner](https://www.gartner.com/doc/reprints?id=1-42UFYTR&ct=170612&st=sb), Microsoft has one of the broadest offerings for professional developers. The support for iOS, Android, Windows, Cordova, React Native and Unity plus the introduction of Visual Studio Mobile Center for DevOps makes Microsoft's a very compelling platform. Moreover, Azure is highly integrated in Microsoft development tools such as Visual Studio and Visual Studio Online.

The Microsoft development platform is not just about front-end development tools like Visual Studio and Xamarin ([Xamarin SDKs are available under the MIT license](https://open.xamarin.com) by the way). Visual Studio Mobile Center support life cycle management and DevOps activities for mobile apps together with tightly integrated testing (via Xamarin Test Cloud) and analytics (HockeyApp). Further more there are plenty of mobile services available via Microsoft Azure. For example building apps with Azure Mobile Apps and application and data integration via Azure Logic Apps.

[![Xamarin with Microsoft’s Azure technology](/images/Blog-Graphic-2-1024x432.jpg)](/images/Blog-Graphic-2.jpg)

The platform Microsoft offers also has strong support for chatbots, virtual assistants, augmented reality and provides one of the largest sets of cognitive services (probably together with Google). As already pointed out, Microsoft is committed to open-source communities and many services on Azure are either using well-established on open source projects (e.g. Azure Container Service) or are published as open source projects by Microsoft (e.g. CNTK, the machine learning/AI framework used in its Cortana voice assistant).

**Fast and reliable global network**

Microsoft is investing heavily in network infrastructure. According to research by [Florian Klaffenbach, Microsoft TSP – Azure (hybrid) Infrastructure](https://twitter.com/FloKlaffenbach) at Microsoft Germany, the Microsoft global network - which is managed by the Azure network team - is one of the two largest backbone networks in the world. Its capacity has grown by 700 percent in the last three years and [over 30'000 miles of dark fiber are connecting Azure datacenters all over the world](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3203). According to [PeeringDB.com](https://www.peeringdb.com/net/694) and tools like [Hurrican Electric Peering Map](https://bgp.he.net/AS8075#_graph4), Microsoft maintains around 60 Public Peering Locations at nearly every Internet Exchange on the globe and around 40 private Peering Locations to enable Network Provider PNIs.

Latest investment is the [MAREA cable](https://blogs.technet.microsoft.com/hybridcloud/2016/05/26/microsoft-and-facebook-to-build-subsea-cable-across-atlantic/), which is an over 4000 miles submarine cable between Virginia Beach, USA and Bilbao, Spain. It was jointly developed with Facebook and is the highest-capacity subsea cable to cross the Atlantic. Also it is the first route from United States to Europe which does not connect from New York to London.

The Microsoft global network plays an important part in delivering a good cloud service experience. It connects 150 datacenters in more than 40 regions around the world. Azure traffic between Microsoft datacenters always stays on the Microsoft network and does not flow over the Internet. This is true for all traffic between Microsoft services anywhere in the world regardless of the source and destination region. So when you enter on of the data centers with your network traffic, lets say West US (California), you will stay on the Microsoft network the whole way through until you exit lets say in West Europe (Amsterdam).

Microsoft is very committed about building the fastest and most reliable global network of any public cloud provider. With its own backbone network, Microsoft remains independent from telcos and stays in control of capacity and resiliency.

[![Azure Inter-DC dark fiber backbone](/images/Azure-Inter-DC-dark-fiber-backbone-1024x467.png)](/images/Azure-Inter-DC-dark-fiber-backbone.png)

**Microsoft Research**

[Microsoft Research](https://www.microsoft.com/en-us/research/) is awesome! [More than 5'000 computer scientists, physicists, engineers, and mathematicians are working at Microsoft Research](https://blogs.microsoft.com/blog/2016/09/29/the-next-milestone-in-microsofts-journey/) to solve difficult world problems through technological innovation. To meet this goal, they collaborate with academic, government and industry researchers all over the world. Some of the smartest people in the world are working for Microsoft Research, including Turing Award and Dijkstra Prize winners.

People at Microsoft Research are working in many different fields like Human-computer interaction to artificial intelligence, quantum computing, social sciences and economics just to name a few. A lot of great projects and products were started at Microsoft Research including for example [Project Sopris](https://www.wired.com/story/project-sopris-iot-security/), the [Seeing AI app](https://www.microsoft.com/en-us/seeing-ai/), Project [InnerEye](https://www.microsoft.com/en-us/research/project/medical-image-analysis/) and [Project Emma](https://www.microsoft.com/en-us/research/project/project-emma/), a wearable that helps with Parkinson's tremors. Many research teams also make source code available to the public like with the [Language Integrated Quantum Operations Simulator](https://stationq.github.io/Liquid/) or the [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK/).

[![Project Emma on Youtube](/images/youtube-screenshot-project-emma.png)](https://youtu.be/R6rAlFYDffQ "This invention helped me write again - BBC Stories")
