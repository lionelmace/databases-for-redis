---
copyright:
  years: 2021, 2022
lastupdated: "2022-11-14"

keyowrds: redis, databases, upgrading, major versions, changing versions

subcollection: databases-for-redis

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:pre: .pre}
{:tip: .tip}


# Upgrading to a new Major Version
{: #upgrading}

When a major version of a database is at its end of life (EOL), upgrade to a current major version. Find the available versions of Redis on the [{{site.data.keyword.databases-for-redis_full}} the catalog](https://cloud.ibm.com/catalog/databases-for-redis) page, from the Cloud Databases CLI plug-in command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the Cloud Databases API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

## Simple upgrade path
{: #upgrading-simple}

Since most uses of {{site.data.keyword.databases-for-redis}} serve as a cache where the data tends to be transient, it is best to create a new deployment. In this simple method, provision a new deployment using the latest version of {{site.data.keyword.databases-for-redis}} and then point your application to the new deployment. This will build up the cache directly through initial use. 

## If you require data migration
{: #upgrading-req-data-migration}

If you absolutely must retain your persisted data when upgrading to the next major version of {{site.data.keyword.databases-for-redis}}, take steps to successfully migrate all of your data. You need to copy all the key-value pairs into the new deployment. Do this from your old instance to your new deployment by using a method appropriate to your use case.

{{site.data.keyword.databases-for-redis}} has provided an unsupported example script to help simplify this process. 

### The migration script
{: #upgrading-migration-script}

Warning! This script is unsupported and exists as an example of what you can build to move {{site.data.keyword.databases-for-redis}} data from one instance to another as part of a version upgrade. All source code and/or binaries that are attached to this document are referred to here as "the Program". IBM is not providing program services of any kind for the Program. IBM is providing the Program on an "AS IS" basis without warranty of any kind. IBM WILL NOT BE LIABLE FOR ANY ACTUAL, DIRECT, SPECIAL, INCIDENTAL, OR INDIRECT DAMAGES OR FOR ANY ECONOMIC CONSEQUENTIAL DAMAGES (INCLUDING LOST PROFITS OR SAVINGS), EVEN IF IBM, OR ITS RESELLER, HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.
{: .note}

Copying data from your old {{site.data.keyword.databases-for-redis}} deployment to a new version involves running a simple Python script [available on GitHub](https://github.com/IBM-Cloud/clouddatabases-migration-examples/blob/master/redis/redis_migration.py){: external}. The script copies all the keys from your source database over to your new {{site.data.keyword.databases-for-redis}} deployment. Download the script and make sure that you have Python 3.x installed. If you’re on macOS, you can use `homebrew` to install it running the command `brew install python3`, which will give you the latest version.

Next, we recommend creating a migration window to let your users know that you are doing some maintenance. That way you have some time to migrate all your data over to your new {{site.data.keyword.databases-for-redis}} deployment. If you are using Redis as a key-value store with expire times on keys, know that these expiry times are copied over to your new database. Our example does this with 10 million keys in our database, which doesn't take much time to migrate depending on your bandwidth.

### Getting the destination and source database credentials
{: #upgrading-dest-source-cred}

Now, you need to have the credentials of both the old source database and your new {{site.data.keyword.databases-for-redis}} deployment. You can get the credentials for both {{site.data.keyword.databases-for-redis}} deployments by clicking the respective databases from the IBM Cloud resources page. Then, click the Service credentials link from the left menu that takes you to the Service credentials view. From there, you can create a new credential for the destination database by clicking the `New credential` button. You can use any credentials that you’ve already created for the source respectively.

![Service credentials view](images/service-credentials.png){: caption="Figure 1. Service credentials view" caption-side="bottom"}

Another way to get this information is using the IBM Cloud CLI. Using the cdb plug-in, run:

```sh
ibmcloud cdb deployment-connections <Redis deployment name>
```
{: .pre}

This provides you with your {{site.data.keyword.databases-for-redis}} connection URI that includes the hostname and port. To get the decoded CA certificate for the database, run:
```sh
ibmcloud cdb deployment-cacert <Redis deployment name>
```
{: .pre}

After the CA certificate is decoded, you need to save that to a file to connect to the database later. If you don’t know the password for your deployment, either get that from your generated service credentials or you can create a new password by running:
```sh
ibmcloud cdb deployment-user-password <Redis deployment name> admin <new password>
```
{: .pre}

With this information, you have what you need for the destination and the source databases.

### Running the script and migrating data
{: #upgrading-script-migration-data}

Since you have all the credentials for both databases (source and new {{site.data.keyword.databases-for-redis}} destination), you're now ready to run the script. The Python script file name is pymigration.py. All you need to do now is run the code from your terminal by using the credentials from the prior steps:
```python
python pymigration.py <source host> <source password> <source port>
<destination host> <destination password> <destination port>
<destination ca certificate path> --sslsrc --ssldst
```
{: .pre}

Since you are copying data from a {{site.data.keyword.databases-for-redis}} database, you need to add the --sslsrc flag if your {{site.data.keyword.databases-for-redis}} database is SSL/TLS enabled. If it isn’t, then don’t add the flag. This makes sure that Redis is connecting to a SSL/TLS enabled database. You also need to add --ssldst since the destination database is your new {{site.data.keyword.databases-for-redis}} which also is SSL/TLS enabled. Supplementary flags that you might add are --db and --flush. Using --db, you can indicate the database that your keys are copied from, which is the database that they recopied into in your new {{site.data.keyword.databases-for-redis}} deployment. The --flush flag flushes the destination database before importing the keys from the source database. If you want to keep things fresh in your new {{site.data.keyword.databases-for-redis}} deployment, flush will delete all the keys first then import the new keys from your source database.

Running the aformentioned script that uses `OldDB` as the source for the data migration and `NewDB` as the destination for the migrated data, you get something like:
```python
python pymigration.py OldDB.databases.appdomain.cloud OldDBpassword1 88888 NewDB.databases.appdomain.cloud NewDBpassword1 99999 ~/NewDBCA  --sslsrc --ssldst  10000000 keys: 100% |###################################################| Time: 0:00:00 Keys disappeared on source during scan: 0 Keys already existing on destination: 0
```

As you can see from the results, you copied 10 million keys from `OldDB` to `NewDB`. No keys were deleted on the `OldDB` database. If you add a new key to the `OldDB` deployment and attempt to migrate the data again, you’ll see that the Keys already existing on the destination changes to 10000000 since the original 10 million keys already exist on that database.
```python
10000000 keys: 100% |###################################################| Time: 0:00:00
Keys disappeared on source during scan: 0
Keys already existing on destination: 10000000
```

## Next steps
{: #upgrading-next-steps}

After your data migration, all you need to do is swap out your application’s database connection strings with your new {{site.data.keyword.databases-for-redis}} connection string and credentials. That’s all it takes!