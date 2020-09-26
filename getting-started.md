---
copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

subcollection: databases-for-redis

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started Tutorial
{: #getting-started}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-redis_full}} deployment.

## Before you begin

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://ibm.biz/databases-for-redis-signup){:new_window}.

- And a {{site.data.keyword.databases-for-redis}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/databases-for-redis/). Give your deployment a memorable name that appears in your account's Resource List.

- [Download and install {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI Plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference). The {{site.data.keyword.cloud_notm}} CLI tool is what you use to communicate with {{site.data.keyword.cloud_notm}} from your terminal or command line, and the plugin contains the commands that you use to communicate with your database deployments.

## 1. Setting the Admin Password

You have to set the admin password before you can use it to connect. To set the password through the UI, open the _Settings_ tab and use the _Change Password_ panel to set a new admin password.

![The Admin Password Panel in _Settings_](images/settings-admin-password.png)

Alternatively, use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI cloud databases plugin to set the admin password with the command line.
```
ibmcloud cdb user-password <deployment-name> admin <new-password>
```

## 2. Downloading and Installing Redli

To get a feel for connecting, sending, and retrieving data with Redis, you might want to start with connecting with a CLI client. All Cloud Databases use TLS/SSL secured connections, so you have to use a Redis CLI client that has support for TLS/SSL, like [redli](https://github.com/IBM-Cloud/redli).

To install redli, download and install the package for your system from its [releases page](https://github.com/IBM-Cloud/redli/releases). Uncompress the files, give the binary executable permissions, and move it to your path. For example, to install it on a Mac download the `redli....darwin_amd64.tar.gz` file and run
```
tar zxvf redli_0.4.4_darwin_amd64.tar.gz
chmod +x redli
sudo cp redli /usr/local/bin
```

Unfortunately, the native redis-cli client does not have support for TLS/SSL connections, and deployments require that  connections are secure. If you want to use redis-cli, you can set up something like [stunnel](https://www.stunnel.org/index.html) to handle the TLS/SSL connection. More information is on the [Connecting with a cli client](/docs/databases-for-redis?topic=databases-for-redis-connecting-cli-client#connecting-with-redis-cli) page.
{: .tip}

## 3. Connecting to Redis

### Method a. - Connecting through IBM Cloud CLI

Assuming at this point you have redli, ibmcloud CLI, and the cloud databases plug-in installed, you can start a connection to your deployment. Login with `ibmcloud login`, and connect with `ibmcloud cdb cxn -s`.
```
ibmcloud cdb cxn -s <deployment-name>
```
It prompts for the admin password and uses redli to connect. You can now store and retrieve data with your Redis deployment. 

### Method b. -  Connecting through Redli

You can also set up a connection to your deployment through redli yourself without using the ibmcloud CLI.

Connection Strings for your deployment are displayed on the _Dashboard Overview_, in the _Connections_ panel. The _CLI_ tab contains information that a CLI client uses to make a connection to your deployment. The three pieces you need are the self-signed certificate, the certificate name, and the connection string in the _CLI endpoint_ field. 

![The CLI Connections Panel](images/getting-started-connection-cli.png)

Save the _contents_ of the TLS certificate to a file and name the file with the TLS certificate _name_. Remember where the file is saved.

The connection string in the _CLI endpoint_ field is the fully formatted command to make a CLI connection to your deployment using redli. Navigate to where you have saved the certificate on your system or provide the full path to the certificate to the `REDIS_CERTFILE` environment variable. Set the admin username and password in the environment as `USERNAME` and `PASSWORD`. Use the CLI connection string to start redli.
```
REDIS_CERTFILE=101afa63-91d0-11e9-a88d-5a059876d90f redli -u rediss://$USERNAME:$PASSWORD@da4103eb-f1ef-4f2d-8b41-0bfd98cb65bc.8117147f814b4b2ea643610826cd2046.databases.appdomain.cloud:30174/0
```

## 4. Using Redis

Now that you have a connection open, which looks like
```
Connected to 4.0.10
>
```
you can start storing and retrieving data.
```
Connected to 4.0.10
> set foo bar
OK
> get foo
"bar"
>
```

The Redis documentation has an introduction to the many Redis data types, with examples. There is also a complete command reference.
- [An introduction to Redis data types and abstractions](https://redis.io/topics/data-types-intro)
- [Command Reference - Redis](https://redis.io/commands/)

## Next Steps

If you are just using Redis for the first time, it is a good idea to take a tour through the [official Redis documentation](https://redis.io/documentation). 

If you are planning to use Databases-for-Redis for your applications, check out some of our other pages on 
- [Connecting an external application](/docs/databases-for-redis?topic=databases-for-redis-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-redis?topic=databases-for-redis-ibmcloud-app)

Also, to ensure the stability of your applications and your database, check out the pages on 
- [High-Availability](/docs/databases-for-redis?topic=databases-for-redis-high-availability)
- [Performance](/docs/databases-for-redis?topic=databases-for-redis-performance)

Tutorial for migrating your data to {{site.data.keyword.databases-for-redis}}
- [A How-To for Migrating Redis to IBM Cloud Databases for Redis](https://www.ibm.com/cloud/blog/a-how-to-for-migrating-redis-to-ibm-cloud-databases-for-redis) 

