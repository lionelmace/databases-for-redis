---
copyright:
  years: 2017, 2021
lastupdated: "2021-11-30"

keywords: redis, databases

subcollection: databases-for-redis

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Setting the Admin Password
{: #admin-password}

The {{site.data.keyword.databases-for-redis_full}} service is provisioned with authentication enabled, and comes with an admin user. 

If you are using Redis 5.x and below, the admin user is the only user for your deployment. Redis didn't have support for multiple users until version 6.x.
{: .tip}

You have to set the admin password before you can use it to connect. To set the password through the {{site.data.keyword.cloud_notm}} dashboard, select _Manage_ from the service dashboard to open the management panel for your service. Open the _Settings_ tab, and use the _Change Database Admin Password_ panel to set a new admin password.

![The Change Database Admin Password Panel in Settings](images/settings-admin-password.png){: caption="Figure 1. The Change Database Admin Password Panel in _Settings_" caption-side="bottom"}

## Setting the admin password via the command line
{: #admin-password-cli}

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI cloud databases plugin to set the admin password with the command line.

For example, to set the admin password for a deployment named "example-deployment", use the following command.
```shell
ibmcloud cdb user-password example-deployment admin <newpassword>
```

## Setting the admin password via the API
{: #admin-password-api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/deployments/{id}/users/{username}` endpoint to set the admin password.

```shell
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users/admin' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"password":"newradminpasswordsupersecure21"}'
```

For more information, see the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#set-database-level-user-s-password).