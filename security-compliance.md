---

Copyright:
  years: 2018
lastupdated: "2018-12-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Architecture, Security, and Compliance
{: #architecture-security-compliance}


## Protection Against Unauthorized Access

{{site.data.keyword.databases-for-redis_full}} use the following methods to protect data in transit or in storage.
- All {{site.data.keyword.databases-for-redis}} connections use TLS/SSL encryption for data in transit. The current supported version of this encryption is TLS 1.2.
- Access to the Account, Management Console UI, and API is secured via [Identity and Access Management (IAM)](/docs/services/databases-for-redis?topic=databases-for-redis-iam).
- Access to the database is secured through the standard access controls provided by the database. These access controls are configured to require valid database-level credentials that are obtainable only through prior access to the database or through our Management Console UI or API.
- All {{site.data.keyword.databases-for-redis}} storage is provided on storage encrypted with [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-about). Bring-your-own-key (BYOK) for encryption is also available through [Key Protect integration](/docs/services/databases-for-redis?topic=databases-for-redis-key-protect. 
- IP Whitelisting - All deployments support whitelisting IP addresses to restrict access to the service.

## Data Resilience

- Backups are included in the service. {{site.data.keyword.databases-for-redis}} backups reside in [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) and are also [encrypted](/docs/services/cloud-object-storage?topic=cloud-object-storage-security).
- Memory usage is configured with a `noeviction` policy and a max memory usage of 80% of available memory.
- The data persistence model uses [preamble snapshots and AOF (Append Only File)](https://redis.io/topics/persistence).
- All {{site.data.keyword.databases-for-redis}} deployments are configured with replication, 
deployments contain a cluster with two data members in a master/replica configuration and state is managed with a quorum of three [Redis sentinels](https://redis.io/topics/sentinel).
- If you deploy to an [{{site.data.keyword.cloud_notm}} datacenter](/docs/overview?topic=overview-data_center#data_center), your data has multiple copies and each copy resides on a different host. If you deploy to a [{{site.data.keyword.cloud_notm}} Global location](https://www.ibm.com/cloud/data-centers/), the cluster is spread over the region's availability zone locations. If one data member becomes unreachable, your cluster continues to operate normally.
 
## SOC 2 Type 1 Certification

{{site.data.keyword.IBM_notm}} provides a Service Organization Controls (SOC) 2 Type 1 report for {{site.data.keyword.databases-for-redis}}. The reports evaluate IBM's operational controls according to the criteria set by the American Institute of Certified Public Accountants (AICPA) Trust Services Principles. The Trust Services Principles define adequate control systems and establish industry standards for service providers such as IBM Cloud to safeguard their customers' data and information.

You can request an SOC 2 Type 1 report from the customer portal or contact your sales representative. Alternatively, you can open a support ticket with [IBM Cloud support](https://www.ibm.com/cloud/support){:new_window} 

## General Data Protection Regulation (GDPR) 

If you have an account with IBM Cloud, your personal data is held by {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.IBM_notm}} Data Processing Addendum (Addendum) applies to the processing of client's personal data by {{site.data.keyword.IBM_notm}} on behalf of client in order to provide {{site.data.keyword.IBM_notm}} standard services.  
[IBM DPA](https://www.ibm.com/support/customer/zz/en/dpa.html){:new_window}

{{site.data.keyword.databases-for-redis}} processes limited client Personal Information (PI) in the course of running the service and optimizing the user experience. 

{{site.data.keyword.databases-for-redis}} provides a [Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=FCCD4BA07A5D11E89D57EFEED3CB8BE9){:new_window} with its policies as a Data Processor regarding content and data protection. 

## HIPAA

{{site.data.keyword.databases-for-redis}} meets the required {{site.data.keyword.IBM_notm}} controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements. These requirements include the appropriate administrative, physical, and technical safeguards required of Business Associates in 45 CFR Part 160 and Subparts A and C of Part 164. HIPAA must be requested at the time of provisioning and requires a representative to sign a Business Associate Addendum (BAA) agreement with {{site.data.keyword.IBM_notm}}.

## Terms

- [The IBM Privacy Policy](https://www.ibm.com/privacy/us/en/)
- [The IBM Cloud Notices and Terms of Use](https://{DomainName}/docs/overview/terms-of-use/notices.html#notices)


