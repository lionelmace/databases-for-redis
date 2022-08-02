---
copyright:
  years: 2018, 2022
lastupdated: "2022-08-02"

keywords: redis, getting started redis, deploy redis, redli

subcollection: databases-for-redis

---

{:shortdesc: .shortdesc}
{:external: .external target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started
{: #getting-started}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-redis_full}} deployment.

## Before you begin
{: #before-begin}

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://ibm.biz/databases-for-redis-signup){: external}.

- You also need a {{site.data.keyword.databases-for-redis}} deployment, which can be provisioned from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/databases-for-redis/). Give your deployment a memorable name that appears in your account's Resource List.

- Download and install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI Plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference). Use the {{site.data.keyword.cloud_notm}} CLI tool to communicate with {{site.data.keyword.cloud_notm}} from your terminal or command line, and the plugin contains communication commands for your database deployments.

For more information, see [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices).

## 1. Setting the Admin Password
{: #setting-admin-password}

Before connecting, set your admin password. To set the password through the UI, open _Settings_ and use the _Change Database Admin Password_ panel to set a new admin password.

![The Change Database Admin Password Panel in Settings](images/settings-admin-password.png){: caption="Figure 1. The Change Database Admin Password Panel in Settings" caption-side="bottom"}

Alternatively, use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI cloud databases plugin to set the admin password with the command line.
```sh
ibmcloud cdb user-password <deployment-name> admin <new-password>
```

## 2. Downloading and Installing Redli
{: #downloading-installing-redli}

To get a feel for connecting, sending, and retrieving data with Redis, start by connecting with a CLI client. All {{site.data.keyword.cloud_notm}} Databases use TLS/SSL secured connections, so use a Redis CLI client that supports TLS/SSL, like [Redli](https://github.com/IBM-Cloud/redli){: .external}.

To install Redli, [download](https://github.com/IBM-Cloud/redli/releases){: .external} and install the package for your system. Uncompress the files, give the binary executable permissions, and move it to your path. For example, to install it on a Mac, download the `redli....darwin_amd64.tar.gz` file and run

```sh
tar zxvf redli_0.4.4_darwin_amd64.tar.gz
chmod +x redli
sudo cp redli /usr/local/bin
```

Unfortunately, the native Redis CLI client does not support TLS/SSL connections and deployments require secure connections. To use Redis CLI, set up something like [Stunnel](https://www.stunnel.org/index.html){: .external} to handle the TLS/SSL connection. For more information, see [Connecting with a cli client](/docs/databases-for-redis?topic=databases-for-redis-connecting-cli-client#connecting-with-redis-cli).
{: .tip}

## 3. Connecting to Redis
{: #connecting-redis}

### Method A. - Connecting through IBM Cloud CLI
{: #connecting-ibm-cloud-cli}

Now that you've installed and set up Redli, the {{site.data.keyword.cloud_notm}} CLI tool, and the {{site.data.keyword.cloud_notm}} Databases plugin, you can now connect to your deployment. Log in with `ibmcloud login`, and connect with `ibmcloud cdb cxn -s`.

```sh
ibmcloud cdb cxn -s <deployment-name>
```
When prompted for the admin password, use Redli to connect. You can now store and retrieve data with your {{site.data.keyword.databases-for-redis_full}} deployment. 

### Method B. -  Connecting through Redli
{: #connecting-redli}

You can also connect to your deployment through Redli yourself, without using the {{site.data.keyword.cloud_notm}} CLI.

Connection strings for your deployment are displayed in the _Dashboard Overview_, in the _Endpoints_ panel. The _CLI_ tab contains information that a CLI client uses to connect to your deployment. To connect, you need the self-signed certificate, the certificate name, and the connection string in the _CLI endpoint_ field. 

![CLI Endpoints panel](images/cli-endpoints-pane.png){: caption="Figure 1. CLI Endpoints panel" caption-side="bottom"}

Download or save the _contents_ of the TLS certificate to a file and name the file with the TLS certificate _name_. Remember where the file is saved.

The connection string in the _CLI endpoint_ field is the fully-formatted command to make a CLI connection to your deployment using Redli. Navigate to where you have saved the certificate on your system, or provide the full path to the certificate to the `REDIS_CERTFILE` environment variable. Set the admin user name and password in the environment as `USERNAME` and `PASSWORD`. Use the CLI connection string to start Redli.

```sh
REDIS_CERTFILE=101afa63-91d0-11e9-a88d-5a059876d90f redli -u rediss://$USERNAME:$PASSWORD@da4103eb-f1ef-4f2d-8b41-0bfd98cb65bc.8117147f814b4b2ea643610826cd2046.databases.appdomain.cloud:30174/0
```

## 4. Using Redis
{: #using-redis}

Now that you have a connection open, which looks like
```sh
Connected to 4.0.10
>
```
you can start storing and retrieving data.
```sh
Connected to 4.0.10
> set foo bar
OK
> get foo
"bar"
>
```

The Redis documentation has an introduction to the many Redis data types, with examples. There is also a complete command reference.
- [An introduction to Redis data types and abstractions](https://redis.io/topics/data-types-intro){: .external}
- [Command Reference - Redis](https://redis.io/commands/){: .external}

## Next Steps
{: #redis-next-steps}

If you are using Redis for the first time, see the [official Redis documentation](https://redis.io/documentation){: .external}. 

To use {{site.data.keyword.databases-for-redis_full}} with your applications, see
- [Connecting an external application](/docs/databases-for-redis?topic=databases-for-redis-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-redis?topic=databases-for-redis-ibmcloud-app)

To ensure the stability of your applications and your database, see
- [High-Availability](/docs/databases-for-redis?topic=databases-for-redis-high-availability)
- [Performance](/docs/databases-for-redis?topic=databases-for-redis-performance)

For more information on migrating your existing data to {{site.data.keyword.databases-for-redis}}, see [A How-To for Migrating Redis to IBM Cloud Databases for Redis](https://www.ibm.com/cloud/blog/a-how-to-for-migrating-redis-to-ibm-cloud-databases-for-redis). 
