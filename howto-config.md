---
copyright:
  years: 2019
lastupdated: "2019-04-19"

keywords: redis, databases

subcollection: databases-for-redis

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Changing the Redis Configuration
{: #changing-configuration}

In {{site.data.keyword.databases-for-redis_full}} you can change some of the Redis configuration settings to tune your databases to your use-case. In a typical Redis setting, you can change the configuration from the command line by using [`CONFIG SET`](https://redis.io/commands/config-set). You can still use `CONFIG SET` on your deployment but, the changes will NOT persist if there is a failover, node restart, or other event on your deployment. Changing the configuration with `CONFIG SET` can be used for testing, evaluation, and tuning purposes.

To make permanent changes to the database configuration, you want to use the {{site.data.keyword.databases-for}} [cli-plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-configuration) or [API](https://{DomainName}/apidocs/cloud-databases-api#change-your-database-configuration) to write the changes to the configuration file for your deployment. 

To make a change, you send a JSON object with the settings that you want to change and their new values. For example, to set the `maxmemory-policy` setting, you would supply 
```
{"configuration":{"maxmemory-policy":"allkeys-lru"}}
```
to either the CLI or to the API. 

## Using the CLI

To change your configuration through the {{site.data.keyword.databases-for}} cli-plugin, use `deployment-configuration` command. 
```
ibmcloud cdb deployment-configuration <deployment name or CRN> [@JSON_FILE | JSON_STRING]
```

The command reads the changes you would like to make from the JSON object or a file. For more information, see the [reference page](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-configuration).

## Using the API

There are two deployment-configuration endpoints, one for viewing the configuration schema and one for changing the configuration. To view the configuration schema, send a `GET` request to `/deployments/{id}/configuration/schema`.

To change the configuration, send the settings you would like to change as a JSON object in the request body of a `PATCH` request to `/deployments/{id}/configuration`.

For more information, see the [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api#change-your-database-configuration)


## Available Configuration settings

To check the current value of a setting, use [`CONFIG GET`](https://redis.io/commands/config-get) from a [CLI client](/docs/services/databases-for-redis?topic=databases-for-redis-connecting-cli-client). You can check all of the settings by using `CONFIG GET *`.

Many of the settings are used to configure Redis as a cache. For more information, see the [Configuring Redis as a Cache](/docs/services/databases-for-redis?topic=databases-for-redis-redis-cache) page.

Setting|Default|Notes
----------|-----|-----------
[`maxmemory`](https://redis.io/topics/memory-optimization#memory-allocation) | 85% of your deployment's memory | Determines the amount of data you can store in Redis.
[`appendonly`](https://redis.io/topics/persistence#append-only-file)| `yes` | Enables or Disables Redis persistence.
[`maxmemory-policy`](https://redis.io/topics/lru-cache#eviction-policies) |`noeviction` | Determines eviction behavior when `maxmemory` limit is reached.
[`maxmemory-samples`](https://redis.io/topics/lru-cache#approximated-lru-algorithm) | `5`| Tunes LRU eviction algorithm when Redis is configured as a cache.
`stop-writes-on-bgsave-error` | `yes` | Redis stops accepting writes if it detects an unsuccessful backup snapshot. 
{: caption="Table 1. Configuration Settings for Redis" caption-side="top"}

