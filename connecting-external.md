---
copyright:
  years: 2018, 2022
lastupdated: "2022-07-20"

keywords: redis, databases

subcollection: databases-for-redis

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-redis_full}}. The service provides connection strings specifically for drivers and applications. Connection strings are displayed in the *Endpoints* panel of your deployment's *Overview*, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

## Connection Strings for Applications
{: #connection-strings-application}

The information a driver needs to make a connection to your deployment is in the "redis" section of your connection strings. The table contains a breakdown for reference.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for Redis, it is "URI" |
| `Scheme` | | Scheme for a URI - for Redis, it is "rediss" |
| `Path` | | Path for a URI - for Redis, it is the database number |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD` |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to |
| `Composed` | `0...` | A URI combining Scheme, Authentication, Host, and Path |
| `Certificate` | `Name` | The allocated name for the self-signed certificate for database deployment |
| `Certificate` | Base64 | A base64 encoded version of the certificate. |
{: caption="Table 1. redis/URI connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Redis drivers are often able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example, if you set the connection string in the environment variable `REDIS_URL`

```bash
export REDIS_URL=rediss://admin:$PASSWORD@e6b2c3f8-54a6-439e-8d8a-aa6c4a78df49.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:32371/0
```

then the Node.js client is able to make a connection with

```bash
let connectionString = process.env.REDIS_URL;

if (connectionString === undefined) {  
  console.error("Please set the REDIS_URL environment variable");
  process.exit(1);
}

let client = null;

client = redis.createClient(connectionString, {
  tls: { servername: new URL(connectionString).hostname }
});
```

Alternatively, the connection string can be parsed and its parts sent to the connection handler, as with the following Python client example

```python
parsed = urlparse(connection_string)

r = redis.StrictRedis(
    host=parsed.hostname,
    port=parsed.port,
    password=parsed.password,
    ssl=True,
    ssl_ca_certs='/etc/ssl/certs/ca-certificates.crt',
    decode_responses=True)
```

Redis has a vast array of clients for applications to use. A fairly [comprehensive list is maintained on the Redis site](https://redis.io/clients). Some useful things to keep in mind when choosing a client are features that allow you to easily design your application for the cloud, like configuring [high-availability](/docs/databases-for-redis?topic=databases-for-redis-high-availability), security, and self-signed certificate support.

## TLS and self-signed certificate support
{: #tls-cert-support}

All connections to {{site.data.keyword.databases-for-redis}} are TLS 1.2 enabled, so the driver you use to connect need to be able to support TLS encryption.

If your driver does not support the `rediss:` protocol or TLS/SSL connections, it is still possible to tunnel connections to the Redis database endpoint by using a TLS/SSL tunnel application such as Stunnel. An example of using Stunnel can be found on the [Connecting with a Command-line Client](/docs/databases-for-redis?topic=databases-for-redis-connecting-cli-client) page, where it is used to connect the `redis-cli` application.

Deployments also come with a self-signed certificate so you can verify the server upon starting a connection. While not required, it is an additional security step that is recommended if your client supports it.

### Using the self-signed certificate
{: #using-cert}

1. Copy the certificate information from the *Endpoints* panel or the Base64 field of the connection information.
2. If needed, decode the Base64 string into text.
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate
{: #cli-support-cert}

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the client.
