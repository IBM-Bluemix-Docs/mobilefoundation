---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: mobile analytics, set up alerts, alert definitions

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

# Set up alerts
{: #set_up_alerts}

{{site.data.keyword.mobileanalytics_short}} provides the Alerts feature, which allows defining alerts to monitor your applications. You can configure thresholds, which if exceeded triggers alerts to notify the {{site.data.keyword.mobileanalytics_short}} console monitor. The triggered alerts can be visualized on the console or can be configured to be relayed to a custom webhook for appropriate handling.

This feature provides the means for detecting and alerting threshold breaches observed over application error logs, application crashes, and network transactions. Reactive thresholds and alerts bring agility in responses to application performance conditions without having to continuously monitor the corresponding views / charts.

This feature is in BETA.
{: note}

The following sections detail the creation, management of alerts and their viewing on the {{site.data.keyword.mobileanalytics_short}} console.

## Creating an alert definition for Application Logs (App Logs)
{: #creating_alert_def}

You can create an alert definition that is based on App Logs.  For example, if you wish to monitor your application logs every 5 minutes to check whether your application of a specific version logged errors more than three times, then here is how you can set this up.

1. In the Mobile Analytics console, click **Definitions** to go to the alert definitions page.
1. Click **Create Alert**.
1. Provide the following values:
   * **Alert Name**: *Alert for app logs*
   * **Message**: *Error message Alert*
   * **Query Frequency**: *5 minutes*
   * **Event Type**: *App Logs*
      * **Property**: *Log Level*
         * **Value**: *Error*
         * **Threshold**: *Total for Application Instance*

            If you choose the *Average for Application* option, the app logs are averaged by the number of devices. For example, if you have two devices and one device sends six app logs while the other device sends three app logs, the average is 4.5 app logs.
            {: note}

         * **Operator**: *is greater than or equals*
         * **Threshold value**: *3*
1. Click **Next** and provide the following value:
   * **Method**: *Analytics Console Only*<br/>
      Additionally, choose the *Analytics Console and Network Post* option, if you want to send a POST message with a JSON payload to your customized URL. The following fields are available if you choose this option.
   * **Network Post URL**
   * **Headers**
   * **Authentication Type**
   * **POST Request Body**
1. Click **Save**.  

You now created an alert definition to trigger an alert at the end of every 5-minute interval when the number of app logs reached the threshold of 3 or more error logs.

This alert stays live and active, monitoring by the setup frequency until the alert definition is disabled or deleted.
{: note}

## Creating an alert definition for application crashes
{: #creating_alert_crashes}

Following is an example of setting up alerts around application crashes.  This alert monitors in every 2 minutes, all application crashes and triggers an alert if the number of crashes that are observed crosses a count of 5.

1. In the Mobile Analytics console, click **Definitions** to go to the alert definitions page.
1. Click **Create Alert**.
1. Provide the following values:
   * **Alert Name**: *Alert for Application Crashes*
   * **Message**: *App Crash Alert*
   * **Query Frequency**: *2 minutes*
   * **Event Type**: *Application Crashes*
      * **Application Name**: *Any application*
      * **Any application**: *Any version*
   * **Threshold**: *Crash Count*
   * **Operator**: *is greater than or equals*
   * **Threshold value**: *5*
1. Click **Distribution Method** or **Next** and provide the following value:
   * **Method**: *Analytics Console Only*<br/>
      Additionally, choose the *Analytics Console and Network Post* option, if you want to send a POST message with a JSON payload to your customized URL. The following fields are available if you choose this option.
      * **Network Post URL**
      * **Headers**
      * **Authentication Type**
      * **POST Request Body**
1. Click **Save**.  

This alert stays live and active, monitoring by the setup frequency until the alert definition is disabled or deleted.
{: note}

## Managing alert definitions
{: #managing_alert_def}

You manage your alert definitions from the alert **Definitions** page.

For each alert definition you can do the following operations,
* Toggle the check-box under the **Enabled** column to enable or disable a specific alert definition.
* If you want to create a copy of an alert definition and change some values, click the duplicate icon.
* Click the pencil icon, if you want to edit an alert definition.
* Click the trash icon, if you want to delete an alert definition.

## Viewing alerts
{: #viewing_alerts}

You can view the alerts, which were triggered from the alert **Logs** page.

1. In the Mobile Analytics console, click **Logs**.
1. Click the **+** icon for any of the alerts. This action displays the alert definition and the **Alert Instances** section.
   If the corresponding alert definition wasn’t deleted or modified, you can edit the alert definition by clicking **Edit Alert**. Otherwise, the **Edit Alert** button is unavailable and the following message displays:
   `This alert definition has since been modified or deleted.`
1. You can optionally select an alert and click the trash icon to delete the alert.
