---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: mobile analytics, charts, visualize data, analytics console

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

# Visualize insights on the console
{: #visualize_insights_on_console}

To visualize insights from the analytics data that is captured and sent from your application, you must start the {{site.data.keyword.mobileanalytics_short}} console by clicking the **Analytics Console** option from the navigation of the {{site.data.keyword.mobilefoundation_short}} Operations console.

The {{site.data.keyword.mobileanalytics_short}} Console can be run in two modes:
- `Demo Mode ON`, which is purely for demonstration purposes and shows the different analytics views (charts and tables) by using simulated data feeds.
- `Demo Mode OFF`, which shows the various analytics views based on realtime data feeds coming from your applications that are [instrumented for {{site.data.keyword.mobileanalytics_short}}](/docs/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

All analytics views can be pruned by applying filters around *application name*, *version*, *device OS*, and *time period*, thus you can obtain insights from different perspectives.

To visualize insights for your application, ensure that:
- Your application is instrumented to capture and send the relevant analytics data to {{site.data.keyword.mobileanalytics_short}} service.
- You turned OFF the `Demo mode` in the Analytics console
- You apply the correct filters. For example, ensure that you select a time period when your application is deployed in the field and is active with users.

The {{site.data.keyword.mobileanalytics_short}} console provides different types of analysis of your mobile application usage and performance as categorized in the navigation pane of the Analytics console. The following sections detail the different analytics views:

## Users
{: #reports_visualized_users}

This view helps you get insights into *User Onboarding Patterns* such as the number of active users who used the app within a specified date range and a comparison of the number of new users versus existing users who return to use your app.

The charts in this view can be filtered on *app name*, *operating system* or *operating system version*.

## Sessions
{: #reports_visualized_sessions}

This view helps you get insights into your application's *Usage Patterns* in terms of *App Sessions* for the specified date range. A session is recorded when an app is brought to the foreground of a device. You get insights to what times of the day your application is most and least used and this information can lead to useful business insights. The charts in this view can be filtered on *app name*, *operating system* or *operating system version*.

## Network Requests
{: #reports_visualized_network_requests}

This view helps you get insights about your application's experience as it makes API calls to the backend systems. This view has tables and charts that provide a peek into what are the most used functions of your backend systems and what is their response time and stability and if you must consider rebalancing your backend support systems.

This view contains charts that plot against a data range that is selected, the Average RoundTrip Time of your application's outbound API calls, the number of requests made per API call, the number of succeeded requests versus failed ones grouped by the response codes. The charts in this view can be filtered on app name, operating system or operating system versions.

## Crashes
{: #reports_visualized_crashes}

This view helps you with insights as to how stable your application was between a selected time period and helps you decide whether your application design or implementation must be fixed. It provides charts that contrast the number of crashes against the total number of uses and the overall crash rate. The charts in this view can be filtered on *app name*, *operating system* or *operating system version*.

## Troubleshooting
{: #reports_visualized_troubleshooting}

This view provides all the necessary information an application developer might need to troubleshoot an application. This view provides a more detailed analysis of your application's crashes in terms of the devices that are affected, the host OS, specific time of the crash, the stacktrace at the time of the crash and also crash logs that can be downloaded for a more detailed analysis.  

Crash logs are gathered by looking up for app logs that are logged at the *FATAL level*. The Analytics Client SDK for Android and iOS native handles uncaught exceptions and logs details about them as *FATAL level* log messages. However, if Cordova, any crashes at the JavaScript layer needs to be handled by the developer and crash logs that are sent to the {{site.data.keyword.mobileanalytics_short}} service to be viewed and analyzed in the {{site.data.keyword.mobileanalytics_short}} console.
{: note}

## User Feedback
{: #reports_visualized_userfeedback}

This view provides insights into the actual interactive experience your users are undergoing while they use the app and how do they feel about it.

- **App owners** can get a detailed, context rich view of bugs, and other feedback sent by **Users and Testers** as recorded while you run the application.
- **Developers** can receive accurate application contexts to diagnose and fix bugs or feature gaps.
- **App owners** and **Developers** can use this view to also manage actions on feedback received such as recording comments or links to issues created in bug-tracking systems. An overall review status can also be set to each feedback to help in summarizing actions taken on user feedback.

## Custom Charts
{: #custom_charts}

This view extends {{site.data.keyword.mobileanalytics_short}} to custom cases where **App owners** and **Developers** would like to build their own, application-specific analytics. Using this facility, you can build your own analytics views (charts, tables, and others) around standard analytics data that is captured by the Client SDK and also custom data or application-specific data that is logged. For more information about extended analytics facility, see [here](/docs/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts).
