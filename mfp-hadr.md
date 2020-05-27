---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-27"

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
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}

# High Availability and Disaster Recovery
{: #overview_hadr}

## High Availability
{: #about_ha}

{{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} for {{site.data.keyword.cloud_notm}} is a highly available, regional service that expedites the setting up of an IBM® {{site.data.keyword.mobilefoundation_short}} environment by using which you can develop, test, and run enterprise mobile apps.

### Locations, tenancy, and availability
{: #about_locations}

{{site.data.keyword.mobilefoundation_short}} service is a multi-tenant, regional service.

You can create {{site.data.keyword.mobilefoundation_short}} service in one of the supported {{site.data.keyword.cloud_notm}} regions (Dallas, London, Sydney, Washington, and Frankfurt), which represent the geographic area where your {{site.data.keyword.mobilefoundation_short}} server is set up. Each {{site.data.keyword.cloud_notm}} region contains multiple availability zones to meet local access, low latency, and security requirements for the respective region.

As you plan your back-end connections with your {{site.data.keyword.mobilefoundation_short}} service instance, keep in mind that provisioning {{site.data.keyword.mobilefoundation_short}} service in a region that is nearest to your back-end is more likely to result in faster, more reliable connections. Choose a specific region if the users, apps, or services that connect {{site.data.keyword.mobilefoundation_short}} server are geographically concentrated.

The region at which users are set is different from the region at which services are set might experience higher latency.
{: note}

If you want to make your {{site.data.keyword.mobilefoundation_short}} server Highly Available, you need to follow the [HA-DR topology](https://www.ibm.com/cloud/blog/build-resilient-backend-to-your-applications-using-ibm-cloud-mobile-foundation) for {{site.data.keyword.mobilefoundation_short}}.

## Disaster Recovery
{: #disaster_recovery}

IBM {{site.data.keyword.mobilefoundation_short}} for {{site.data.keyword.cloud_notm}} allows its users to choose Db2, {{site.data.keyword.composeForPostgreSQL}} and {{site.data.keyword.databases-for-postgresql}}-ICD. 

If you have not setup [HA-DR topology](https://www.ibm.com/cloud/blog/build-resilient-backend-to-your-applications-using-ibm-cloud-mobile-foundation), do following steps to recover disaster.

* In a disaster incident, declared by these databases, follow the backup and recovery procedures as documented:
   * The **{{site.data.keyword.composeForPostgreSQL}}** has scheduled backups available. In a disaster, follow the instructions as documented [here](/docs/ComposeForPostgreSQL?topic=ComposeForPostgreSQL-dashboard-backups).
   * **IBM® Cloud Databases for PostgreSQL** offers Point-In-Time Recovery (PITR). Follow the instructions as documented [here](/docs/databases-for-postgresql?topic=databases-for-postgresql-pitr).
   * **Db2 on IBM Cloud** offers backup and recovery process as documented [here](/docs/Db2onCloud?topic=Db2onCloud-bnr).
* In an incident of a disaster, create a new instance of {{site.data.keyword.mobilefoundation_short}} service on {{site.data.keyword.cloud_notm}} in an alternative region and restore your database backups that are exported from the old instances of DB2/PostgresSQL.
