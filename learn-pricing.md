---
copyright:
  years: 2017, 2022
lastupdated: "2022-09-15"

keywords: redis, databases, pricing, resources, scaling

subcollection: databases-for-redis

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Pricing
{: #pricing}

A {{site.data.keyword.databases-for-redis}} Standard plan deploys as one highly available Redis cluster with two data members. Your data is replicated on both members. The Standard plan is priced based on the total amount of disk storage, RAM, dedicated cores, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-redis}} deployments have a minimum of 1 GB of disk and 1 GB of RAM per data member.

## Cost Breakdown
{: #cost}

**Disk storage per data member** - gigabytes of disk that is allocated to a {{site.data.keyword.databases-for-redis}} data member, or the size of your data.  
**RAM per data member** - gigabytes of RAM allocated to a {{site.data.keyword.databases-for-redis}} data member.  
**Backup storage** - amount of storage used for backups by a {{site.data.keyword.databases-for-redis}} deployment. 

| Resources | Breakdown | Price |
| ------- | ------- | ------- |
| 1 GB-Month disk | 2 members x 1 GB x $0.58 | $1.16 |
| 1 GB-Month RAM | 2 members x 1 GB  x $5 | $10 |
{: caption="Table 1. Pricing example for two data members" caption-side="top"}

Total per month = $11.16/Month  
Total per hour = $.016/Hour

All prices here are in US dollars. To see pricing in your local currency, you can to use the pricing calculator.
{: .tip}

## IBM Cloud Databases enabled by IBM Cloud Satellite Pricing
{: #satellite-pricing}

{{site.data.keyword.databases-for-redis}} deployments are deployable into IBM Cloud Satellite locations. The management fee for these Cloud Databases is $45 per vCPU per month, with a 6 vCPU minimum.

| Resources | Breakdown | Price |
| ------- | ------- | ------- |
| 6 vCPUs per month | 2 members x 6 GB x $45 | $540 |
{: caption="Table 2. Pricing example for  6 vCPUs and two data members" caption-side="top"}

Total per month = $49.80/Month

## Using the Pricing Calculator
{: #pricing-calc}

Templates are provided for ease of use and provide balanced resource allocations appropriate for general purpose workloads. The **Custom** tab can be used to configure Disk, RAM, and vCPU, as desired.

For pricing estimation, use the **Add to Estimate** button on the [{{site.data.keyword.databases-for-redis}} catalog page](https://cloud.ibm.com/catalog/databases-for-redis). Input your total consumption across two data members into the calculator. This is roughly double the size of your data because your data is replicated to both members. For example, 1 GB of disk and 1 GB of RAM across two data members would be priced at 2 GB of disk and 2 GB of RAM respectively. 

![Pricing calculator estimation with 1 GB of disk and 1 GB of RAM, per member](images/pricing-calc.png){: caption="Figure 1. Pricing calculator estimation" caption-side="bottom"}

## Backups Pricing
{: #pricing-backup}

You receive your total disk space purchased, per database, in free backup storage. For example, in a given month, if you have a {{site.data.keyword.databases-for-redis}} deployment that has 20 GB of disk per member, and has three data members, you receive 60 GB of backup storage free for that month. If your backup storage utilization is greater than 60 GB for the month (in this scenario), you are charged an overage of $0.03/month per gigabyte. 

By default, {{site.data.keyword.databases-for}} provides a daily backup that is stored for 30 days. These backups, and any on-demand backups you make, all count toward the above allocation.

In the above example, if your database contains 2 GB of data and you have not taken any on-demand backups, then your total backup size is 2 GB x 30 = 60 GB. Your backup costs are nil.

If your database contains 15 GB of data and you have not taken any on-demand backups, then your total backup size is 15 GB x 30 = 450 GB. In this scenario, your backup costs are (450 GB - 60 GB) * 0.03 = $11.7 per month.

Most deployments will not ever go over the allotted credit.

## Dedicated Cores Pricing
{: #cores-pricing}

You have the option of selecting the CPU allocation for your deployment. With dedicated cores, your resource group is given a single-tenant host with a guaranteed minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. The cost of dedicated cores is $30 per core per month, and each member gets the selected number of cores. For example, if you provision a deployment with 3 dedicated cores per member, that is a total of 6 cores, and billed at $180 per month. 

Dedicated cores are an optional feature. The default `Shared CPU` setting provisions your deployment on hosts with shared compute resources and incurs no additional charge.

## Scaling per Member
{: #scaling}

{{site.data.keyword.databases-for-redis}} deployments have minimum and maximum allocation for disk and RAM. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.

| Resource | Minimum | Maximum | Scaling Granularity (API/CLI) |
| ---------- | ----- | ----- | ------- |
| Disk | 5 GB per member | 4 TB per member | 1024 MB per member |
| RAM | 1 GB per member | 112 GB per member | 128 MB per member |
| CPU (if enabled) | 3 CPUs per member | 28 CPUs per member| 1 CPU per member |
{: caption="Table 2. Per Member Scaling Limits" caption-side="top"}
