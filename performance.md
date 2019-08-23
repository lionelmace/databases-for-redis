---

Copyright:
  years: 2019
lastupdated: "2019-07-30"

keywords: redis, databases

subcollection: databases-for-redis

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Performance
{: #performance}

{{site.data.keyword.databases-for-redis}} deployments do not auto-scale. You can scale your deployments [to your usage](/docs/services/databases-for-redis?topic=databases-for-redis-resources-scaling). There are a few factors to consider if you are concerned about the performance of your deployment.

## Memory Policies

By default, deployments are configured with a `noeviction` policy. All data is kept in memory until the `maxmemory` limit is reached and Redis returns an error if the memory limit is exceeded. The `maxmemory` is set to 85% of a data node's available memory, so your node doesn't run out of system resources. For example, the minimum size deployment has 1 GB RAM per node, and its `maxmemory` is set to 858993459 bytes.

You can scale the amount of memory to accommodate more data, and you can configure the `maxmemory` setting to tune memory usage. The [Redis documentation](https://redis.io/topics/memory-optimization#memory-allocation) has some good information on memory behavior and tuning `maxmemory`.

You can also configure your deployment to use [Redis as a cache](/docs/services/databases-for-redis?topic=databases-for-redis-redis-cache), allowing Redis to evict data out of memory once the memory limit is reached. 

## Disk IOPS

The number of Input-Output Operations per second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-redis}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). By default, a deployment starts with persistence enabled. It's possible for very busy databases to exceed the IOPS for the disk size, and increasing disk can alleviate a performance bottleneck. 

## Monitoring your deployment

Deployment owners can [monitor](/docs/services/databases-for-redis?topic=databases-for-redis-monitoring) the state of the deployment, estimate typical resource usage, and scale the deployment accordingly.

If you are planning on running operations that might put a spike in resource usage, you can manually scale your service's resources up to avoid hitting any limits that affect deployment operations.
