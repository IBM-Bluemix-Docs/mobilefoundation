---

copyright:
  years: 2019
lastupdated: "2019-11-05"

keywords: mobile foundation, high availability, disaster recovery, HA, HADR, DR
subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download} 

# High Availability and Disaster Recovery
{: #overview_hadr}

## High Availability
{: #about_ha}

IBM {{site.data.keyword.mobilefoundation_short}} for {{site.data.keyword.Bluemix_notm}} is a highly available, regional service that expedites the setting up of an IBM® {{site.data.keyword.mobilefoundation_short}} environment using which you can develop, test and run enterprise mobile apps.

### Locations, tenancy, and availability
{: #about_locations}

{{site.data.keyword.mobilefoundation_short}} service is a multi-tenant, regional service.

You can create {{site.data.keyword.mobilefoundation_short}} service in one of the supported {{site.data.keyword.Bluemix_notm}} regions (Dallas, London, Sydney, Washington and Frankfurt), which represent the geographic area where your {{site.data.keyword.mobilefoundation_short}} server is setup. Each {{site.data.keyword.Bluemix_notm}} region contains multiple availability zones to meet local access, low latency, and security requirements for the respective region.

As you plan your back-end connections with your {{site.data.keyword.mobilefoundation_short}} service instance, keep in mind that provisioning {{site.data.keyword.mobilefoundation_short}} service in a region that is nearest to your back-end is more likely to result in faster, more reliable connections. Choose a specific region if the users, apps, or services that connect {{site.data.keyword.mobilefoundation_short}} server are geographically concentrated. Remember that users and services set at different regions might experience higher latency.

If you want to make your {{site.data.keyword.mobilefoundation_short}} server Highly Available, you need to follow the [HA-DR topology](https://www.ibm.com/cloud/blog/build-resilient-backend-to-your-applications-using-ibm-cloud-mobile-foundation) for {{site.data.keyword.mobilefoundation_short}}.

## Disaster Recovery
{: #disaster_recovery}

IBM {{site.data.keyword.mobilefoundation_short}} for {{site.data.keyword.Bluemix_notm}} allows its users to choose DB2, {{site.data.keyword.composeForPostgreSQL}} and Database for PostgresSQL-ICD. 

If you have not setup [HA-DR topology](https://www.ibm.com/cloud/blog/build-resilient-backend-to-your-applications-using-ibm-cloud-mobile-foundation), you should do following steps to recover disaster.

* In case of a disaster incident declared by these databases, please follow the backup and recovery procedures as documented:
  * The **{{site.data.keyword.composeForPostgreSQL}}** has scheduled backups available. In case of a disaster, follow the instructions as documented [here](https://cloud.ibm.com/docs/services/ComposeForPostgreSQL?topic=compose-for-postgresql-dashboard-backups).
  * **IBM® Cloud Databases for PostgreSQL** offers Point-In-Time Recovery (PITR). Please follow the instructions as documented [here](https://cloud.ibm.com/docs/services/databases-for-postgresql?topic=databases-for-postgresql-pitr).
  * **DB2 on IBM Cloud** offers backup and recovery process as documented [here](https://cloud.ibm.com/docs/services/Db2onCloud?topic=Db2onCloud-bnr).
* In an incident of a disaster, please create a new instance of {{site.data.keyword.mobilefoundation_short}} service on {{site.data.keyword.Bluemix_notm}} in an alternate region and restore your database backups exported from the old instances of DB2/PostgresSQL.
