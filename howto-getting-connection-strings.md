---
copyright:
  years: 2017,2018
lastupdated: "2018-10-29"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Getting your Connection Strings
{: #connection-strings}

The {{site.data.keyword.databases-for-redis_full}} service is provisioned with authentication enabled. You need a username and password to connect and issue commands. The username for Redis is always "admin".

You have to set the admin password before you connect to the database. For more information, see the [Setting the Admin Password](/docs/services/databases-for-redis?topic=databases-for-redis-admin-password) page.
{: .tip}

The simplest way to retrieve connection information is from the [cloud databases plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference). Use the `ibmcloud cdb deployment-connections` command to display a formatted connection URI for your deployment. For example, to retrieve a connection string for a deployment named  "NewRedis", use the following command.

```
ibmcloud cdb deployment-connections NewRedis
```
Or
```
ibmcloud cdb cxn NewRedis
```

Full connection information is returned by the `ibmcloud cdb deployment-connections` command with the `--all` flag. Retrieve all the connection information for a deployment named  "NewRedis", use the following command.

```
ibmcloud cdb deployment-connections NewRedis --all
```

## Other ways of accessing the connection strings

It is possible to make the "admin" connection information available the in _Service Credentials_ panel. 
1. Navigate to the service dashboard for your service.
2. Click _Service Credentials_ to open the _Service Credentials_ panel.
3. Click **New Credential**.
4. Choose a descriptive name for the credential, if wanted. 
5. Click **Add**.

Redis does not support multiple users. Adding more service credentials does not give you more users. It just reuses the admin credential.
{: .tip}

The admin credentials appear in the table, and the connection strings are available as JSON in a click-to-copy field under _View Credentials_. The JSON object contains the same fields as Table 1 and Table 2. 

Connection information also available as JSON from the cloud databases API. The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/users/admin` endpoint to retrieve the admin connection strings. For more information, see the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f).

## Connection String Breakdown

### The Redis Section

The "Redis" section contains information that is suited for your applications that make connections to Redis.

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

For more information on using this information to connect, see the [Connecting an External Application](/docs/services/databases-for-redis?topic=databases-for-redis-external-app) page.

### The CLI Section

The "CLI" section contains information that is suited for command-line clients that make connections to Redis.

Field Name|Index|Description
----------|-----|-----------
`Bin`||The recommended binary to create a connection; in this case it is `redli`.
`Composed`||A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings and uses `Arguments` as command line parameters.
`Environment`||A list of key/values you set as environment variables.
`Arguments`|0...|The information that is passed as arguments to the command shown in the Bin field.
`Certificate`|Base64|A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded.
`Certificate`|Name|The allocated name for the self-signed certificate.
`Type`||The type of package that uses this connection information; in this case `cli`. 
{: caption="Table 2. `redis`/`cli` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

For more information on using this information, see the [Connecting with a command line client](/docs/services/databases-for-redis?topic=databases-for-redis-connecting-cli-client) page.

