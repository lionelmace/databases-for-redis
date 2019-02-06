---
copyright:
  years: 2017,2018
lastupdated: "2018-11-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}



# Pricing

A {{site.data.keyword.databases-for-redis}} Standard plan deploys as one highly available Redis cluster with two data members. Your data is replicated on both members. The Standard plan is priced based on the total amount of disk storage, RAM, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-redis}} deployments have a minimum of 1 GB of disk and 1 GB of RAM per data member.

## Cost Breakdown

**Disk storage per data member** - gigabytes of disk that is allocated to a {{site.data.keyword.databases-for-redis}} data member, or the size of your data.  
**RAM per data member** - gigabytes of RAM allocated to a {{site.data.keyword.databases-for-redis}} data member.  
**Backup storage** - amount of storage used for backups by a {{site.data.keyword.databases-for-redis}} deployment. 

Resources | Breakdown | Price
-------|-------|-------
1 GB-Month disk | 2 members x 1 GB x $0.58 | $1.16
1 GB-Month RAM | 2 members x 1 GB  x $5 | $10
{: caption="Table 1. Pricing example for two data members" caption-side="top"}

Total per month = $11.60/Month  
Total per hour = $.016/Hour

All prices here are in US dollars. To see pricing in your local currency, you can to use the [pricing calculator](https://{DomainName}/pricing/configure/service/databases-for-redis).
{: .tip}


## Using the Pricing Calculator

For pricing estimation, input your total consumption across two data members into the calculator. This is roughly double the size of your data because your data is replicated to both members. For example, 1 GB of disk and 1 GB of RAM across two data members would be priced at 2 GB of disk and 2 GB of RAM respectively. 

![Pricing calculator estimation with 1 GB of disk and 1 GB of RAM, per member](images/pricing-calc.png)

## Backups Pricing

Users also receive their total disk space purchased, per database, in free backup storage. For example, in a month, if you have a {{site.data.keyword.databases-for-redis}} deployment that has 1 GB of disk per member and two data members, you receive 2 GB of backup storage free for that month. If your backup storage utilization is greater than 2 GB for the month in this scenario, each GB is charged at an overage $0.03/month. Backups are compressed, so most deployments will not go over the allotted credit.

## Scaling per Member

{{site.data.keyword.databases-for-redis}} deployments have minimum and maximum allocation for disk and RAM. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.
- Disk minimum - 1 GB per member
- Disk maximum - 4 TB per member
- Disk step granularity through API/CLI - 1 GB total, 512 MB per member
- RAM minimum - 1 GB per member
- RAM maximum - 112 GB per member
- RAM step granularity through API/CLI - 256 MB total, 128 MB per member