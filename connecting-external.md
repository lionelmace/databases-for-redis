---
copyright:
  years: 2018
lastupdated: "2018-09-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #connecting-external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-redis_full}}. The service provides connection strings specifically for drivers and applications. 

## Getting Connection Strings

The easiest way to get connection strings for an application is to create a set of _Service Credentials_ specifically for your application to connect with. Doing so also returns all the connection information as JSON in a click-to-copy field. Full documentation on generating and retrieving connection strings is on the [Getting Connection Strings](./howto-getting-connection-strings.html) page.

A [table](./howto-getting-connection-strings#the-redis-section) with a breakdown of all the connection information is included for reference.

## Connecting with a language's client

Redis has a vast array of clients for applications to use. A fairly [comprehensive list is maintained on the Redis.io site](https://redis.io/clients).

Language|Driver|Instructions/Examples
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

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-redis}} are TLS 1.2 enabled, so the driver you use to connect need to be able to support TLS encryption. For {{site.data.keyword.databases-for-redis}}, this means supporting the unofficial `rediss:` protocol in connection strings. 

If your driver does not support the `rediss:` protocol or TLS/SSL connections, it is still possible to tunnel connections to the Redis database endpoint by using a TLS/SSL tunnel application such as Stunnel. An example of using Stunnel can be found in the [Admin - Connecting section](admin-connecting.html), where Stunnel is used to connect the `redis-cli` application.

Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. In most cases, you want to decode and save a copy of the certificate, and then provide the path to the driver. For more information, see [Using the self-signed certificate](./howto-getting-connection-strings.html#using-the-self-signed-certificate).







 
