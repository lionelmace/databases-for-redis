---

Copyright:
  years: 2019
lastupdated: "2019-05-06"

keywords: redis, databases

subcollection: databases-for-redis

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability and Performance
{: #high-availability}

{{site.data.keyword.databases-for-redis_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-redis}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with two data members in a master/replica configuration and kept in sync using asynchronous replication. High-availability is monitored and managed with a quorum of three [Redis sentinels](https://redis.io/topics/sentinel).

By default, data persistence is enabled on all deployments and your data is written to disk. The data persistence model uses [preamble snapshots and AOF (Append Only File)](https://redis.io/topics/persistence). The interval for Redis to write to disk (fsync) is set to [once every second](https://redis.io/topics/persistence#how-durable-is-the-append-only-file). 

You can turn off data persistence, which is useful for [configuring Redis as a cache](/docs/services/databases-for-redis?topic=databases-for-redis-redis-cache).

## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-redis}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable. It can also cause a the database to trigger a graceful fail-over, retry, and reconnect. It takes a short time for the database to determine which member is a replica and which is the leader, so you might also see a short connection interruption. Failovers generally take less than 30 seconds.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption is not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Performance

{{site.data.keyword.databases-for-redis}} deployments do not auto-scale. You can scale your deployments [to your usage](/docs/services/databases-for-redis?topic=databases-for-redis-resources-scaling). There are a few factors to consider if you are concerned about the performance of your deployment.

### Memory Policies

By default, deployments are configured with a `noeviction` policy. All data is kept in memory until the `maxmemory` limit is reached and Redis returns an error if the memory limit is exceeded. The `maxmemory` is set to 85% of a data node's available memory, so your node doesn't run out of system resources. For example, the minimum size deployment has 1 GB RAM per node, and its `maxmemory` is set to 858993459 bytes.

You can scale the amount of memory to accommodate more data, and you can configure the `maxmemory` setting to tune memory usage. The [Redis documentation](https://redis.io/topics/memory-optimization#memory-allocation) has some good information on memory behavior and tuning `maxmemory`.

You can also configure your deployment to use [Redis as a cache](/docs/services/databases-for-redis?topic=databases-for-redis-redis-cache), allowing Redis to evict data out of memory once the memory limit has been reached. 

### Disk IOPS

The number of Input-Output Operations per second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-redis}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). By default, a deployment starts with persistence enabled. It's possible for very busy databases to exceed the IOPS for the disk size, and increasing disk can alleviate a performance bottleneck. 

### Monitoring your deployment

Deployment owners can [monitor](/docs/services/databases-for-redis?topic=databases-for-redis-monitoring) the state of the deployment, estimate typical resource usage, and scale the deployment accordingly.

If you are planning on running operations that might put a spike in resource usage, you can manually scale your service's resources up to avoid hitting any limits that affect deployment operations.

## SLA

{{site.data.keyword.databases-for-redis}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-SLAs#SLAs).

