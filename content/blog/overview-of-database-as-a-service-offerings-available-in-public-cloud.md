---
author: sjohner
comments: true
date: 2015-09-07 20:39:00+00:00
slug: overview-of-database-as-a-service-offerings-available-in-public-cloud
title: Overview of Database-as-a-Service offerings available in Public Cloud
categories:
- Cloud
tags:
- Amazon
- Database
- Database-as-a-Service
- DBaaS
- Google
- Microsoft
- MongoDB
- MySQL
- NoSQL
- PostgreSQL
- RDBMS
- SQL
---

When searching for a suitable Database-as-a-Service offering, sometimes one can't see the wood for the trees. Nearly all of the big players in public cloud business offer some kind of database service. Besides there exist plenty of smaller providers, most of them offering specific database implementations.

[![DBaaS Provider Overview](/images/daas_vendors.png?w=660)](/images/daas_vendors.png)

The above picture is far away from being complete, but it kind of shows how many players are out there offering database as a service. I picked some vendors to look at different Database-as-a-Service offerings.

When talking about DBaaS most people think about traditional [relational database management systems](https://en.wikipedia.org/wiki/Relational_database_management_system). In contrast to traditional IT, the use of [NoSQL databases](https://en.wikipedia.org/wiki/NoSQL) is pretty popular in cloud environments to overcome the limitations of relational database technology for use with modern web applications. Basically NoSQL databases have the following advantages over classic RDBMS when it comes to running databases in a cloud:

* Horizontally scalable (scaling out).
* Bigger Data Handling Capability (e.g. for big data)
* Many NoSQL stores compromise consistency in favor of availability, partition tolerance, and speed.

Examples for popular NoSQL databases are [Apache CouchDB](https://en.wikipedia.org/wiki/CouchDB), [MongoDB](https://en.wikipedia.org/wiki/MongoDB), [redis](https://en.wikipedia.org/wiki/Redis) and [Apache Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra). If you are interested in NoSQL topic, take a look at [https://www.mongodb.com/nosql-explained](https://www.mongodb.com/nosql-explained) for a detailed explanation of this technology.

Most Database-as-a-Service providers offer classic RDBMS as well as NoSQL databases. All offerings have different characteristics. Some are based on publicly available, others on proprietary technology.

In the sections below you will find several tables which describe available DBaaS offerings by service provider. I only looked at offerings which are part of "official" DBaaS offerings of the cloud service provider. I did not cover offerings which may be purchased from kind of a given marketplace run by the cloud service provider. Also since cloud offerings change at a rapid pace, it may already be outdated at time of publishing :-)

# Relational database offerings

Most of the big players offer some kind of relational database as a service. Exceptions are specialized companies like [redislab ](https://redislabs.com/)which do offer a cloud service for their own product redis cache in this case

Technologies used by most of the companies are either [MySQL](https://en.wikipedia.org/wiki/MySQL) or [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL). For those of you which are searching for a vendor specific database technology such as [Microsoft SQL](https://en.wikipedia.org/wiki/Microsoft_SQL_Server), [IBM DB2](https://en.wikipedia.org/wiki/DB2) or [Oracle DB](https://en.wikipedia.org/wiki/Oracle_Database), this is kind of a no-brainer. For the big players in cloud business, so far only the owners of the respective technology are also providing corresponding offerings. With the exception of Amazon AWS whose Amazon RDS service offers quite a variety of technologies to choose from. Of course other vendors do also offer various technologies through their marketplaces. However these offerings are mainly provided by a smaller 3rd parties like [clearDB](https://www.cleardb.com/) or [elephantSQL](http://www.elephantsql.com).
All the above mentioned technologies have in common that they are also available for on premise environments which can be kind of relevant depending on your use case.

In contrast to that, Amazon as well as Google provide kind of proprietary cloud-only relational database solutions to their customers. Google SQL as well as Amazon RDS for Aurora are only available as public cloud offerings. These products are highly optimized for use in public cloud and it may be the best choice depending on the specific use case. However one has to mention that using such an offering may result in a vendor lock-in.

<table width="477" >
<tbody >
<tr >

<td bgcolor="#67c6d4" width="175" >**Provider**
</td>

<td bgcolor="#67c6d4" width="400" >**RDBMS Offering
**
</td>
</tr>
<tr >

<td >_Microsoft Azure_
</td>

<td >SQL Database, MySQL Database1
</td>
</tr>
<tr >

<td >_Amazon AWS_
</td>

<td >RDS (MySQL, PostgreSQL, Oracle, Microsoft SQL)
</td>
</tr>
<tr >

<td >_Google Cloud_
</td>

<td >Cloud SQL
</td>
</tr>
<tr >

<td >_RedHat OpenShift_
</td>

<td >PostgreSQL, MySQL, SQLite
</td>
</tr>
<tr >

<td >_IBM Bluemix_
</td>

<td >DB2, MySQL1, PostgreSQL1
</td>
</tr>
<tr >

<td >_Oracle_
</td>

<td >Database
</td>
</tr>
<tr >

<td >_MongoLab_
</td>

<td > -
</td>
</tr>
<tr >

<td >_Heroku_
</td>

<td >PostgreSQL
</td>
</tr>
<tr >

<td >_RedisLabs_
</td>

<td > -
</td>
</tr>
<tr >

<td >_ClearDB_
</td>

<td >MySQL
</td>
</tr>
</tbody>
</table>
1 Provided by 3rd party cloud service provider (either ClearDB or elephantSQL)

# NoSQL database offerings

NoSQL database as a service offerings are mostly found with big players in cloud business. Microsoft, Amazon and Google all provide NoSQL databases for customers. However one has to be aware of the fact that all these offerings are based on a proprietary technology. These database offerings are highly optimized for use with cloud native applications and are probably first choice when creating new, elastic web applications in the cloud.
But making use of these database offerings can be kind of a point of no return to an on premise environment since technology is not available outside the cloud. And since moving to another cloud provider is nearly impossible because all of them use other proprietary technology, one need to migrate from one technology to another which can cost quite some money. One has to be aware that using such proprietary NoSQL offerings results in a pretty strong vendor lock-in.

There exist also some providers which heavily rely on publicly available technology for their NoSQL offerings. Mostly smaller players such as [Iris Couch](https://web.archive.org/web/20170107065639/http://www.iriscouch.com:80/) (for Apache CouchDB) or [MongoLab](https://mongodb.com/) (for MongoDB) are offering DBaaS based on open technology. But also the bigger cloud service providers sometimes offer NoSQL database solutions based on publicly available technology. Most of them are using MongoDB as underlying technology. However, usually these offerings are provided through marketplace and a 3rd party provider like MongoLab. But The exception proves the rule: IBM Cloudant is available directly from IBM and based on CouchDB.
In any case, if you like to make use of an NoSQL DBaaS offering, make sure you think about choosing the right technology and provider. To prevent vendor lock-in, it may be an option to prefer DBaaS offerings of smaller cloud service providers which are based on open, widely available technology.

<table width="477" >
<tbody >
<tr >

<td bgcolor="#67c6d4" width="175" >**Provider**
</td>

<td bgcolor="#67c6d4" width="400" >**NoSQL Offering
**
</td>
</tr>
<tr >

<td >_Microsoft Azure_
</td>

<td >Azure DocumentDB, MongoDB1
</td>
</tr>
<tr >

<td >_Amazon AWS_
</td>

<td >DynamoDB
</td>
</tr>
<tr >

<td >_Google Cloud_
</td>

<td >Cloud Datastore, Cloud Bigtable
</td>
</tr>
<tr >

<td >_RedHat OpenShift_
</td>

<td >MongoDB
</td>
</tr>
<tr >

<td >_IBM Bluemix_
</td>

<td >Cloudant, MongoDB1
</td>
</tr>
<tr >

<td >_Oracle_
</td>

<td >NoSQL Database2
</td>
</tr>
<tr >

<td >_MongoLab_
</td>

<td >MongoDB
</td>
</tr>
<tr >

<td >_Heroku_
</td>

<td >PostgreSQL
</td>
</tr>
<tr >

<td >_RedisLabs_
</td>

<td >-
</td>
</tr>
<tr >

<td >_ClearDB_
</td>

<td >-
</td>
</tr>
</tbody>
</table>

_1 Provided by 3rd party cloud service provider (MongoLab)_
_2 Coming soon_

# Particular database offerings and NoSQL variants

Besides common relational and NoSQL database offerings, there exist other database offerings for specific workloads like data warehouse / big data and in-memory caching. For the data warehouse offerings the same is true as for NoSQL database offerings: most of them are only available as public cloud offerings. The big vendors like Google, Amazon as well as IBM already provide services for data warehouse implementations. Microsoft announced its SQL data warehouse service back in April but this one is still in preview.

Besides Redislab who offer their own product redis cache as a service, in memory caches are also available from Amazon, Google, Microsoft and IBM whereas Microsoft and IBM offer a redis cache implementation to their customers. Amazon as well as Google have their own implementations.

<table width="477" >
<tbody >
<tr >

<td bgcolor="#67c6d4" width="175" >**Provider**
</td>

<td bgcolor="#67c6d4" width="400" >**Data Warehouse and In-Memory Cache Offerings
**
</td>
</tr>
<tr >

<td >_Microsoft Azure_
</td>

<td >SQL Data Warehouse2, RedisCache, Managed Cache Service
</td>
</tr>
<tr >

<td >_Amazon AWS_
</td>

<td >Redshift, ElastiCache
</td>
</tr>
<tr >

<td >_Google Cloud_
</td>

<td >Cloud BigQuery, Memcache
</td>
</tr>
<tr >

<td >_RedHat OpenShift_
</td>

<td >-
</td>
</tr>
<tr >

<td >_IBM Bluemix_
</td>

<td >dashDB, Redis1
</td>
</tr>
<tr >

<td >_Oracle_
</td>

<td >BigData2
</td>
</tr>
<tr >

<td >_MongoLab_
</td>

<td >-
</td>
</tr>
<tr >

<td >_Heroku_
</td>

<td >-
</td>
</tr>
<tr >

<td >_RedisLabs_
</td>

<td >Redis
</td>
</tr>
<tr >

<td >_ClearDB_
</td>

<td >-
</td>
</tr>
</tbody>
</table>

_1 Provided by 3rd party cloud service provider (redislab)_
_2 Coming soon_

# Summary

Most big players in cloud business offer database as a service. However the mostly rely on their own implementations for RDBMS and NoSQL. Customers have to keep in mind that this fact leads to a vendor lock in and that the investment may be higher when switching to another cloud provider or technology.

Some of the big vendors also offer open source database technology, like Microsoft and IBM with MySQL and MongoDB. However one has to be aware that these offerings are not provided by the vendors itself but by a 3rd party instead. Mostly these offerings are based on services provided by smaller players, like ClearDB, Mongolab or redislab, which specialize in one database technology. **If there is a need for a very specific database technology, it may make sense to choose a smaller provider over one of the big players to make use of their knowledge and their service in this particular area.**

Pricing can be very difficult to compare between different providers. Most of the time, there is a free plan which can be chosen but price range goes up to several 1000$ per month of course depending on performance. Also performance can be very hard to compare between different vendors. Some use kind of proprietary units to measure performance others specify ram, storage and concurrent connections to differ their plans.

One has to keep in mind, that** although the database is running in the cloud, high availability is not necessarily provided**. Often only higher priced plans offer high availability and redundancy or it may be available as an option.










