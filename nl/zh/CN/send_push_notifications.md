---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, sending notification, HTTP/2

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
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


# 发送推送通知
{: #send_push_notifications}

可以从 {{ site.data.keyword.mfp_oc_short_notm }} 或通过 REST API 发送推送通知。
{: shortdesc}

* 通过 {{ site.data.keyword.mfp_oc_short_notm }}，可以发送以下两种类型的通知：标记通知和广播通知。
* 通过 REST API，可以发送所有形式的通知：标记通知、广播通知和已认证的通知。

## 从 {{ site.data.keyword.mfp_oc_short_notm }} 发送推送通知
{: #sending-push-notification-from-mobilefirst-operations-console }

可以将通知发送到单个设备标识、单个或多个用户标识、仅 iOS 设备或仅 Android 设备，或者已预订标记的设备。

### 标记通知
{: #tag-notifications }

标记通知是将预订了特定标记的所有设备作为目标的通知消息。标记表示用户感兴趣的主题，并提供根据所选兴趣接收通知的功能。

在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的应用程序] → 推送 → 发送通知**选项卡中，从**发送到**选项卡中选择**按标记划分的设备**，并提供**通知文本**。 然后，单击**发送**。

<img class="gifplayer" alt="按标记发送" src="images/sending-by-tag.png"/>

### 广播通知
{: #broadcast-notifications }

广播通知是标记推送通知的一种形式，用于将所有预订设备作为目标。任何支持推送的 {{ site.data.keyword.mobilefirst_notm }} 应用程序在缺省情况下都可通过预订保留的 `Push.all` 标记（为每个设备自动创建）来启用广播通知。 可以使用编程方式取消预订 `Push.all` 标记。

在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的应用程序] → 推送 → 发送通知**选项卡中，从**发送到**选项卡中选择**全部**，并提供**通知文本**。 然后，单击**发送**。

<img class="gifplayer" alt="发送到全部" src="images/sending-to-all.png"/>

## 使用 REST API 发送推送通知
{: #sending-push-notifications-using-rest-apis }

使用 REST API 发送通知时，可以发送所有形式的通知：标记通知、广播通知和已认证的通知。

要发送通知，使用 POST 向 REST 端点发出请求：`imfpush/v1/apps/<application-identifier>/messages`。  
以下是示例 URL：

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

要查看所有推送通知 REST API，请参阅用户文档中的 [REST API 运行时服务](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html)主题。

### 通知有效内容
{: #notification-payload }

请求可以包含以下有效内容属性：

|有效内容属性|定义
|--- | ---
|message |要发送的警报消息|
|settings |设置是通知的不同属性。|
|target |目标集可以是使用者标识、设备、平台或标记。只可以设置其中一个目标。|
|deviceIds |以设备标识表示的一组设备。具有这些标识的设备将收到通知。这是单点广播通知。|
|notificationType|整数值，指示用于发送消息的通道（推送或 SMS）。允许的值为 1（仅用于推送）、2（仅用于 SMS）和 3（用于推送和 SMS）
|
|platforms |一组设备平台。在这些平台上运行的设备将收到通知。受支持的值有 A (Apple/iOS)、G (Google/Android) 和 M (Microsoft/Windows)。|
|tagNames |指定为 tagNames 的一组标记。预订这些标记的设备将收到通知。此类型的目标用于基于标记的通知。|
|userIds |发送通知的一组用户（以用户标识表示）。这是单点广播通知。|
|phoneNumber |用于注册设备和接收通知的电话号码。这是单点广播通知。|
{: caption="表 1. 有效内容属性" caption-side="top"}

**推送通知有效内容 JSON 示例**

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

**SMS 通知有效内容 JSON 示例**

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

## 发送通知
{: #sending-the-notification }

可以使用不同的工具来发送通知。出于测试目的，将使用 **Postman**，以下步骤描述设置：

1. [配置保密客户机](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/)。通过 REST API 发送推送通知将使用空格分隔的作用域元素 `messages.write` 和 `push.application.<applicationId>`。
  <img class="gifplayer" alt="配置保密客户机" src="images/push-confidential-client.png"/>
2. [创建访问令牌](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token)。  
3. 向 **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages** 发出 **POST** 请求
  - 如果使用远程 {{ site.data.keyword.mobilefirst_notm }}，请将 `hostname` 和 `port` 值替换为您自己的值。
  - 使用您自己的值更新应用程序标识值。
4. 设置头：
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - 将 **Bearer** 之后的值替换为步骤 (1) 中访问令牌的值。
    ![授权头](images/postman_authorization_header.png "授权头")
5. 设置主体：
  - 更新其属性，如[通知有效内容](#notification-payload)中所述。
  - 例如，通过添加具有 **userIds** 特性的 **target** 属性，可以将通知发送到特定的注册用户。
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![授权主体](images/postman_json.png "授权主体")

    单击**发送**按钮后，设备将收到通知：

    ![样本应用程序的图像](images/notifications-app.png "移动设备上的推送通知")

## 定制通知
{: #customizing-notifications }

在发送通知消息之前，您还可以定制以下通知属性。  

在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的应用程序] → 推送 → 标记 → 发送通知**选项卡中，展开 **iOS/Android 定制设置**部分以更改通知属性。

### Android
{: #android }

* 通知声音、通知可在 FCM 存储器中存储的时间、定制有效内容等。
* 如果您想要更改通知标题，请在 Android 项目的 **strings.xml** 文件中添加 `push_notification_tile`。

### iOS
{: #ios }

* 通知声音、定制有效内容、操作键标题、通知类型和角标号。

  ![定制推送通知](images/customizing-push-notifications.png "Mobile First Operations Console 推送页面，其中选择了“发送推送”选项卡")

## APNs 推送通知支持 HTTP/2
{: #http2-support-for-apns-push-notifications}

Apple 推送通知服务 (APNs) 支持基于 HTTP/2 网络协议的新 API。HTTP/2 支持提供诸多优点，包括以下项：

* 消息长度从 2 KB 增加到 4 KB，支持向通知添加额外内容。
* 客户机与服务器之间无需多个连接，从而提高吞吐量。
* 支持通用推送通知客户机 SSL 证书。

>现在，{{ site.data.keyword.mobilefirst_notm }} 中的推送通知支持基于 HTTP/2 的 APNs 推送通知以及旧的基于 TCP 套接字的通知。

### 启用 HTTP/2
{: #enabling-http2}

可使用 JNDI 属性启用基于 HTTP/2 的通知。
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

如果添加 JNDI 属性，那么将不会使用旧的基于 TCP 套接字的通知，并且仅启用基于 HTTP/2 的通知。
{: note}

### HTTP/2 代理支持
{: #proxy-support-for-http2}

可通过 HTTP 代理发送基于 HTTP/2 的通知。要启用通过代理的通知路由，请参阅[此处](#proxy-support)。

## 代理支持
{: #proxy-support }
可使用代理设置来设置可选代理，通知将通过此可选代理发送至 Android 和 iOS 设备。 您可以使用 `push.apns.proxy.*` 和 `push.gcm.proxy.*` 配置属性来设置代理。
有关更多信息，请参阅 [List of JNDI properties for {{ site.data.keyword.mfserver_short_notm }} push service](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service)。

## 后续步骤
{: #next-tutorial-to-follow }
服务器端现已设置，下面使用以下教程设置客户机端并处理收到的通知。

* [在客户机应用程序中处理推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
