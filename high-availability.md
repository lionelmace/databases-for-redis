---

Copyright:
  years: 2019
lastupdated: "2019-08-06"

keywords: redis, databases

subcollection: databases-for-redis

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-redis_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

## Database HA Features

{{site.data.keyword.databases-for-redis}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with two data members in a master/replica configuration and kept in sync using asynchronous replication. High-availability is monitored and managed with a quorum of three [Redis sentinels](https://redis.io/topics/sentinel).

By default, data persistence is enabled on all deployments and your data is written to disk. The data persistence model uses [preamble snapshots and AOF (Append Only File)](https://redis.io/topics/persistence). The interval for Redis to write to disk (fsync) is set to [once every second](https://redis.io/topics/persistence#how-durable-is-the-append-only-file). 

You can turn off data persistence, which is useful for [configuring Redis as a cache](/docs/services/databases-for-redis?topic=databases-for-redis-redis-cache).

## HA for your Application 

Applications that communicate over networks and cloud services are subject to transient connection failures. Also, because {{site.data.keyword.databases-for-redis}} is a managed service, regular updates and database maintenance occurs as part of normal operations. These operations can cause a short blip in connectivity or trigger a failover. In the event of a failover, it takes a short time for the database to determine which member is a replica and which is the leader, so you might also see a short connection interruption.

You want to design your applications to handle a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}. 

Many Redis clients have features for error checking and handling. For example,
- The python client [`Redis.py`](https://github.com/andymccurdy/redis-py#connections) throws a `ConnectionError` when a connection fails and another command is run. It also has the ability to issue health checks periodically or before commands.
- The Java client [`Jedis`](https://github.com/xetorthio/jedis/wiki) throws a `JedisConnectionException` when it can't reach the database, and it tries to reconnect. However, it doesn't automatically retry a command, as not every command is safe to retry or run multiple times.
- The C# driver [`StackExchange.Redis`](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) has adjustable parameters for client-side timeout, reconnect, and retry logic. You can tune parameters these to suit your application requirements.

A good example of cloud-ready application development using the Node.js client `ioredis` is in the IBM Developer article [Error detection and handling with Redis](https://developer.ibm.com/articles/error-detection-and-handling-with-redis/).

In all cases, you should consult your client's documentation to help you design a stable application that checks for connectivity, reconnects when disconnected, and retries commands when it makes sense for your application to do so.
{: .tip}  

Several minutes of database unavailability or connection interruptions are not expected. Open a [support ticket](/docs/get-support?topic=get-support-getting-customer-support) with details if you have time periods longer than a minute with no connectivity. 

## Monitoring the state of your databases

{{site.data.keyword.databases-for-redis}} has [{{site.data.keyword.la_full_notm}} integration](/docs/services/databases-for-redis?topic=cloud-databases-logging) so that you can view live and historical logs.

Checking your deployment's logs helps you monitor the state of HA and replication for your deployment. If you are having persistent issues with your applications, logs can also provide insight to what is happening on your databases when you are experiencing connection failures or other disruptions. 

## SLA

{{site.data.keyword.databases-for-redis}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-SLAs#SLAs).

