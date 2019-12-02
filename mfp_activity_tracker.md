---

copyright:
  years: 2019
lastupdated: "2019-11-29"

keywords: mobile foundation service, notifications, activity tracker events, activity tracker, mobile-foundation.server-db.connect, mobile-foundation.server-properties.updated, mobile-foundation.server.create, mobile-foundation.server.delete

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

# Activity Tracker Events
{: #mfp_activity_tracker}

The {{site.data.keyword.cloudaccesstrailfull}} service monitors a user's interaction with the {{site.data.keyword.cloud_notm}} services. You can search and analyze how your users and applications interact with {{site.data.keyword.cloud_notm}} services.

Refer to the [Activity Tracker Page](https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) for details.

## List of events by {{site.data.keyword.mobilefoundation_short}}
{: #actions}

{{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} service is now able to send events to {{site.data.keyword.cloudaccesstrailfull}}
 service. You can choose to track activity on the {{site.data.keyword.mobilefoundation_short}} service by creating an instance of [{{site.data.keyword.cloudaccesstrailfull}}
 service](https://cloud.ibm.com/observe/activitytracker/create).

The following table lists the {{site.data.keyword.cloudaccesstrailfull}} events for {{site.data.keyword.mobilefoundation_short}} service:

|Action                                                      |Description                                                 |
|------------------------------------------------------------|------------------------------------------------------------|
|mobile-foundation.server-db.connect                         |Connect server to an existing DB instance and create schema |
|mobile-foundation.server-properties.updated                 |Updated service properties                                  |
|mobile-foundation.server.create                             |When server creation is initiated                           |
|mobile-foundation.server.delete                             |When service is deleted                                     |
{:caption="Table 1. List of actions that genererate an event" caption-side="top"}

