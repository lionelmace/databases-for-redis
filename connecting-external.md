---
copyright:
  years: 2018, 2019
lastupdated: "2019-04-10"

subcollection: databases-for-redis

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-redis_full}}. The service provides connection strings specifically for drivers and applications. Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

You have to set the admin password before you connect to the database. For more information, see the [Setting the Admin Password](/docs/services/databases-for-redis?topic=databases-for-redis-admin-password) page.
{: .tip}

## Connecting with a language's client

The information a driver needs to make a connection to your deployment is in the "redis" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for Redis, it is "URI"
`Scheme`||Scheme for a URI - for Redis, it is "rediss"
`Path`||Path for a URI - for Redis, it is the database number
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. `redis`/`URI` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Redis drivers are often able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,
```
rediss://admin:$PASSWORD@e6b2c3f8-54a6-439e-8d8a-aa6c4a78df49.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:32371/0
```

Redis has a vast array of clients for applications to use. A fairly [comprehensive list is maintained on the Redis.io site](https://redis.io/clients).

Language|Driver|Source
--------|------|---------------------
C|`hiredis`|[Link](https://github.com/redis/hiredis)
PHP|`phpredis`|[Link](https://github.com/phpredis/phpredis)
PHP|`Predis`|[Link](https://github.com/nrk/predis)
Ruby|`redis-rb`|[Link](https://github.com/redis/redis-rb)
Python|`redis-py`|[Link](https://github.com/andymccurdy/redis-py)
C#|`ServiceStack.Redis`|[Link](https://github.com/ServiceStack/ServiceStack.Redis)
C#|`StackExchange.Redis`|[Link](https://github.com/StackExchange/StackExchange.Redis)
Go|`redigo`|[Link](https://github.com/gomodule/redigo)
Go|`Radix`|[Link](https://github.com/mediocregopher/radix.v2)
Node.js|`ioredis`|[Link](https://github.com/luin/ioredis)
Node.js|`node_redis`|[Link](https://github.com/NodeRedis/node_redis)
{: caption="Table 2. Common Redis drivers" caption-side="top"}

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-redis}} are TLS 1.2 enabled, so the driver you use to connect need to be able to support TLS encryption. For {{site.data.keyword.databases-for-redis}}, this means supporting the unofficial `rediss:` protocol in connection strings. 

If your driver does not support the `rediss:` protocol or TLS/SSL connections, it is still possible to tunnel connections to the Redis database endpoint by using a TLS/SSL tunnel application such as Stunnel. An example of using Stunnel can be found on the [Connecting with a Command-line Client](/docs/services/databases-for-redis?topic=databases-for-redis-connecting-cli-client) page, where it is used to connect the `redis-cli` application.

### Using the self-signed certificate

1. Copy the certificate information from the _Connections_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.







 
