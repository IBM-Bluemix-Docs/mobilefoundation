---

copyright:
  years: 2019
lastupdated: "2019-09-25"

keywords: mobile foundation service, notifications, activity tracker events, activity tracker, mobile-foundation.server-db.connect, mobile-foundation.server-properties.updated, mobile-foundation.server.create, mobile-foundation.server.delete

subcollection:  mobilefoundation

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Activity Tracker Events
{: #mfp_activity_tracker}

The {{site.data.keyword.cloudaccesstrailfull}} service monitors a user's interaction with the IBM Cloud services. You can search and analyze how your users and applications interact with IBM Cloud services.

Refer to the [Activity Tracker Page](https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started){: new_window} for details.

## List of events by {{site.data.keyword.mobilefoundation_short}}
{: #actions}

IBM {{site.data.keyword.mobilefoundation_short}} service is now able to send events to {{site.data.keyword.cloudaccesstrailfull}}
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

