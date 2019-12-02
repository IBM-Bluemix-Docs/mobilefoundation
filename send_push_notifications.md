---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: push notifications, notifications, sending notification, HTTP/2

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

# Send {{site.data.keyword.mobilepushshort}}
{: #send_push_notifications}

{{site.data.keyword.mobilepushshort}} can be sent either from the {{site.data.keyword.mfp_oc_short_notm}} or by ReST APIs.
{: shortdesc}

- With the {{site.data.keyword.mfp_oc_short_notm}}, two types of notifications can be sent: tag and broadcast.
- With the ReST APIs, all forms of notifications can be sent: tag, broadcast, and authenticated.

## Sending {{site.data.keyword.mobilepushshort}} from {{site.data.keyword.mfp_oc_short_notm}}
{: #sending-push-notification-from-mobilefirst-operations-console}

Notifications can be sent to a single Device ID, a single or several User IDs, only iOS devices or only Android devices, or to devices subscribed to tags.

### Tag notifications
{: #tag-notifications}

Tag notifications are notification messages that are targeted to all the devices that are subscribed to a particular tag. Tags represent topics of interest to the user and provides the ability to receive notifications according to the chosen interest.

In the {{site.data.keyword.mfp_oc_short_notm}} → **[your application] → Push → Send Notifications tab**, select **Devices By Tags** from the **Send To** tab and provide the **Notification Text**. Then, click **Send**.

<img class="gifplayer" alt="Sending by tag" src="images/sending-by-tag.png"/>

### Broadcast notifications
{: #broadcast-notifications}

Broadcast notifications are a form of tag push notifications that are targeted to all subscribed devices. Broadcast notifications are enabled by default for any push-enabled {{site.data.keyword.mobilefirst_notm}} application by a subscription to a reserved `Push.all` tag (auto-created for every device). The `Push.all` tag can be programmatically unsubscribed.

In the {{site.data.keyword.mfp_oc_short_notm}} → **[your application] → Push → Send Notifications tab**, select `All` from the `Send To` tab, and provide the **Notification Text**. Then, click **Send**.

<img class="gifplayer" alt="Sending to all" src="images/sending-to-all.png"/>

## Sending Push Notifications by using REST APIs
{: #sending-push-notifications-using-rest-apis}

When you use the REST APIs to send notifications, all forms of notifications can be sent: tag and broadcast notifications, and authenticated notifications.

To send a notification, a request is made by using POST to the REST endpoint: `imfpush/v1/apps/<application-identifier>/messages`.  
Following is an example URL,

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

To review all {{site.data.keyword.mobilepushshort}} ReST APIs, see the [ReST API Runtime Services topic](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html) in the user documentation.

### Notification payload
{: #notification-payload}

The request can contain the following payload properties:

| Payload Properties| Definition |
| --- | --- |
| message | The alert message to be sent |
| settings | The settings are the different attributes of the notification. |
| target | Set of targets can be consumer IDs, devices, operating system, or tags. Only one of the targets can be set. |
| deviceIds | An array of the devices represented by the device identifiers. Devices with these IDs receive the unicast notification. |
| notificationType | Integer value to indicate the channel (Push or SMS) used to send message. Allowed values are 1 (for Push only), 2 (for SMS only) and 3 (for both Push and SMS) |
| platforms | An array of device platforms. Devices running on these platforms receive the notification. Supported values are A (Apple: iOS), G (Google: Android) and M (Microsoft&trade;: Windows&trade;). |
| tagNames | An array of tags that are specified as tagNames. Devices that are subscribed to these tags receive the notification. Use this type of target for tag-based notifications. |
| userIds | An array of users represented by their userIds to send the unicast notification. |
| phoneNumber | The phone number that is used for registering the device and receiving unicast notifications. |
{: caption="Table 1. Payload properties" caption-side="top"}

#### {{site.data.keyword.mobilepushshort}} Payload JSON Example

```json
{
    "message" : {
    "alert" : "Test message",
  },
  "settings" : {
    "apns" : {
      "badge" : 1,
      "iosActionKey" : "Ok",
      "payload" : "",
      "sound" : "song.mp3",
      "type" : "SILENT",
    },
    "gcm" : {
      "delayWhileIdle" : ,
      "payload" : "",
      "sound" : "song.mp3",
      "timeToLive" : ,
    },
  },
  "target" : {
    // The following list is for demonstration purposes only - per the documentation only 1 target is allowed to be used at a time.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```
{: codeblock}

#### SMS Notification Payload JSON Example

```json
{
  "message": {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds" : ["38cc1c62-03bb-36d8-be8e-af165e671cf4"]
   }
}
```
{: codeblock}

## Sending the notification
{: #sending-the-notification}

The notification can be sent by using different tools. For testing purposes, **Postman** is used to describe the following setup:

1. [Configure a Confidential Client](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/){: external}.
   Sending a {{site.data.keyword.mobilepushshort}} through the ReST API uses the space-separated scope elements `messages.write` and `push.application.<applicationId>.`
   <img class="gifplayer" alt="Configure a confidential client" src="images/push-confidential-client.png"/>

1. [Create an access token](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token){: external}.  

1. Make a **POST** request to **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages**
   - If you are using a remote {{site.data.keyword.mobilefirst_notm}}, replace the `hostname` and `port` values with your own.
   - Update the application identifier value with your own.

1. Set a Header:
   - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
   - Replace the value after **Bearer** with the value of your access token from step (1).
      ![Authorization header](images/postman_authorization_header.png "Authorization header")

1. Set a Body:
   - Update its properties as described in [Notification payload](#notification-payload).
   - For example, by adding the **target** property with the **userIds** attribute, you can send a notification to specific registered users.

      ```json
      {
           "message" : {
               "alert" : "Hello World!"
           }
      }
      ```
      {: codeblock}

      ![Authorization body](images/postman_json.png "Authorization body")

      After you click the `Send`, the device receives a notification:

      ![Image of the sample application](images/notifications-app.png "Push notification on a mobile device")

## Customizing Notifications
{: #customizing-notifications}

Before you send the notification message, you can also customize the following notification attributes.  

In the {{site.data.keyword.mfp_oc_short_notm}} → **[your application] → Push → Tags → Send Notifications tab**, expend the **iOS/Android Custom Settings** section to change notification attributes.

### Android
{: #android}

- Notification sound, how long a notification can be stored in the FCM storage, custom payload and more.
- If you want to change the notification title, then add `push_notification_tile` in the Android project's **strings.xml** file.

### iOS
{: #ios}

- Notification sound, custom payload, action key title, notification type, and badge number.

   ![Customizing push notifications](images/customizing-push-notifications.png "Mobile First Operations Console Push page with the Send Push tab selected")

## HTTP/2 Support for APNs {{site.data.keyword.mobilepushshort}}
{: #http2-support-for-apns-push-notifications}

Apple Push Notification service (APNs) supports a new API based on HTTP/2 network protocol. Support for HTTP/2 provides many benefits, including the following,

- Message length that is increased 2 - 4 KB, which enables to add extra content to notifications.
- Eliminates the need for multiple connections between client and server, which improves the throughput.
- Universal Push Notification Client SSL Certificate support.

Push Notifications in {{site.data.keyword.mobilefirst_notm}} now supports the HTTP/2 based APNs Push Notifications along with the legacy TCP Socket based notifications.

### Enabling HTTP/2
{: #enabling-http2}

HTTP/2 based notifications can be enabled by using a JNDI Property.

```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

If the JNDI property is added, legacy TCP Socket based notifications is not used and only the HTTP/2 based notifications are enabled.
{: note}

### Proxy Support for HTTP/2
{: #proxy-support-for-http2}

HTTP/2 based notifications can be sent through an HTTP Proxy. To enable routing of the notifications through a proxy, see [here](#proxy-support).

## Proxy Support
{: #proxy-support}

You can make use proxy settings to set the optional proxy through which notifications are sent to Android and iOS devices. You can set the proxy by using the `push.apns.proxy.*` and `push.gcm.proxy.*` configuration properties. For more information, see [List of JNDI properties for {{site.data.keyword.mfserver_short_notm}} push service](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

## Next steps
{: #next-tutorial-to-follow}

With the server-side set up completed, now set up the client-side and handle the received notifications by using the following tutorial.

- [Handling push notifications in Client applications](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
