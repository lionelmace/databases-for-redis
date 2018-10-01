---
copyright:
  years: 2017,2018
lastupdated: "2018-09-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Command line Tools for {{site.data.keyword.databases-for-redis}}

Use the {{site.data.keyword.cloud_notm}} CLI and the cloud databases plug-in to access and manage {{site.data.keyword.databases-for-redis_full}}.

## The {{site.data.keyword.cloud_notm}} CLI

The {{site.data.keyword.cloud_notm}} CLI is a general-purpose developer tool that provides access to your {{site.data.keyword.cloud_notm}} account and services through a command line interface.

An introduction and installation instructions are available on the [{{site.data.keyword.cloud_notm}} CLI Overview](https://console.{DomainName}/docs/cli/index.html#overview) page. If you install the CLI from the cURL command that is provided, you get a selection of extra plug-ins and extensions for multiple IDEs.

You can install just the stand-alone package from the [Installing the stand-alone IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/ibmcloud/download_cli.html#install_use) page. 

## The cloud databases plug-in

Once you have the {{site.data.keyword.cloud_notm}} CLI, [login](https://console.{DomianName}/docs/cli/reference/ibmcloud/bx_cli.html#ibmcloud_login) and grab the cloud databases plug-in. 

`ibmcloud plugin install cloud-databases`

Use `ibmcloud cdb help` for a list of commands and usage, or see the [documentation]() for a command reference. 

## Using the `cdb` commands

Run `ibmcloud cdb help` for other commands and usage information. 

## Using the plug-in for database administration

If you need to open a connection directly to your Redis, the `ibmcloud cdb deployment-connections` command handles everything that is involved in creating the client connection. For example, to connect to a deployment named  "NewRedis", use the following command.

```
ibmcloud cdb deployment-connections NewRedis -start
```
Or
```
ibmcloud cdb cxn NewRedis -s
```

The command prompts for the admin password and then runs the `redli` command line client to connect to the database. For more information on connecting to Redis directly, see the [connecting with a command line client](./connecting-cli-client.html) page.

## The {{site.data.keyword.cloud_notm}} CLI and IAM

Access to services via {{site.data.keyword.cloud_notm}} CLI is governed through Integrated Access Management. In order to use the CLI to view or manage a service (or to grant privileges to another user on your account), you have to set the correct permissions. For more information about IAM management, see the [IAM Getting Started tutorial](https://console.{DomainName}/docs/iam/quickstart.html#getstarted).

For more information on how IAM affects {{site.data.keyword.databases-for-redis}}, see the [reference page](./reference-access-management.html)