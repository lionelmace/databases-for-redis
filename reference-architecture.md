---

Copyright:
  years: 2018
lastupdated: "2018-09-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Architecture

{{site.data.keyword.databases-for-redis_full}} is a managed cloud database service that runs in containers that are orchestrated by Kubernetes. It is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and monitoring all run in {{site.data.keyword.cloud_notm}}.

## Redis Databases

{{site.data.keyword.databases-for-redis}} deployments contain a cluster with two data members in a master/replica configuration. The cluster is spread over the region's availability zones. High-availability is managed with a quorum of three Redis sentinels. If one data member becomes unreachable, your cluster continues to operate normally.

### Configuration

By default, the {{site.data.keyword.databases-for-redis}} service is configured with a "noeviction" policy and a max memory usage of 80% of available memory.

### Storage

All backup storage for {{site.data.keyword.databases-for-redis}} is provided on {{site.data.keyword.cloud_notm}} Object Storage. 

### Full-disk Encryption

All {{site.data.keyword.databases-for-redis}} services all have encryption at rest. Your backups reside on servers that have volume-level encryption enabled.

## Portals

{{site.data.keyword.databases-for-redis}} connections are managed by 2 HAProxy portals. The portals are then placed behind a Kubernetes endpoint. This endpoint provides the connection for your applications. Having two portals behind it allows for applications to maintain connectivity if one of the portals becomes unreachable.

### Encryption in Transit

All {{site.data.keyword.databases-for-redis}} HAProxy portals are TLS/SSL enabled and support TLS version 1.2. Self-signed certificates are provided to enable applications to validate the server upon connection.

## Access Management

{{site.data.keyword.databases-for-redis}} is an IAM-integrated service. Access to the service is governed by the roles and attributes that are consistent with IAM-integrated services across the {{site.data.keyword.cloud_notm}}. For more information about IAM, see the [What is IAM?](https://console.{DomainName}/docs/iam/index.html#iamoverview) documentation. For details on how IAM affects access to {{site.data.keyword.databases-for-redis}}, see the [Managing Access](./access-management.html) page.

