---

copyright:
  years: 2019
lastupdated: "2019-12-16"

keywords: mobile analytics, updating mobile foundation service instances, updating instances, latest level

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

# Updating your {{site.data.keyword.mobilefoundation_short}} service instances to the latest level
{: #updating_your_mobile_foundation_service_instances}

The {{site.data.keyword.mobilefoundation_short}} service publishes regular updates to the {{site.data.keyword.mobilefoundation_short}} server Professional Per Device and Professional1 Application plans.
{: shortdesc}

These updates will have the latest fixes and feature updates. You can plan and update to the latest level of {{site.data.keyword.mobilefoundation_short}}. It is recommended that you are always on the latest level.

To check if your {{site.data.keyword.mobilefoundation_short}} server has updates, login to the {{site.data.keyword.cloud_notm}} console, select your Mobile Service instance on the dashboard. Click on **Manage** on the left-hand side navigation menu. You will see a notice if there is a server update available.

To update to the latest level, you simply need to click **Update server** button that is available at the end of the same page.

Updating the {{site.data.keyword.mobilefoundation_short}} service results in restarting of the corresponding Cloud Foundry application. As a result, there may be a downtime. The updates need to be planned accordingly.
{: important}

For production deployments, it is recommended that you first update your test instance. You will need to verify your Mobile Application client, by running all your acceptance tests on the test {{site.data.keyword.mobilefoundation_short}} service instance before updating the production instance.

