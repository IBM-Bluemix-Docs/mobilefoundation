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


# 傳送推送通知
{: #send_push_notifications}

推送通知可以從 {{ site.data.keyword.mfp_oc_short_notm }} 或透過 REST API 傳送。
{: shortdesc}

* 使用 {{ site.data.keyword.mfp_oc_short_notm }}，可以傳送兩種類型的通知：標籤和播送。
* 使用 REST API，可以傳送所有形式的通知：標籤、播送及鑑別。

## 從 {{ site.data.keyword.mfp_oc_short_notm }} 傳送推送通知
{: #sending-push-notification-from-mobilefirst-operations-console }

通知可以傳送至單一裝置 ID、單一或數個使用者 ID、僅限 iOS 裝置或僅限 Android 裝置，或是傳送至訂閱標籤的裝置。

### 標籤通知
{: #tag-notifications }

標籤通知是通知訊息，以所有訂閱特定標籤的裝置為目標。標籤代表使用者感興趣的主題，並可讓您根據所選的興趣來接收通知。

在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的應用程式] → 推送 → 傳送通知標籤**，從**傳送至**標籤選取**依標籤排列的裝置**，並提供**通知文字**。然後，按一下**傳送**。

<img class="gifplayer" alt="依標籤傳送" src="images/sending-by-tag.png"/>

### 播送通知
{: #broadcast-notifications }

播送通知是以所有訂閱裝置為目標的標籤推送通知形式。藉由訂閱保留的 `Push.all` 標籤（自動針對每個裝置建立），會依預設針對任何啟用推送的 {{ site.data.keyword.mobilefirst_notm }} 應用程式啟用播送通知。`Push.all` 標籤可以用程式設計方式取消訂閱。

在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的應用程式] → 推送 → 傳送通知標籤**，從**傳送至**標籤選取**全部**，並提供**通知文字**。然後，按一下**傳送**。

<img class="gifplayer" alt="傳送至全部" src="images/sending-to-all.png"/>

## 使用 REST API 來傳送推送通知
{: #sending-push-notifications-using-rest-apis }

使用 REST API 來傳送通知時，可以傳送所有形式的通知：標籤和播送通知，以及已鑑別的通知。

若要傳送通知，會使用 POST 向 REST 端點提出要求：`imfpush/v1/apps/<application-identifier>/messages`。  
以下為範例 URL。

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

若要檢閱所有 Push Notifications REST API，請參閱使用者文件中的 [REST API 運行環境服務主題](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html)。

### 通知有效負載
{: #notification-payload }

要求可以包含下列有效負載內容：

|有效負載內容|定義
|--- | ---
|message |要傳送的警示訊息
|
|settings |設定是通知的不同屬性。
|
|target |目標集可以是消費者 ID、裝置、平台或標籤。只能設定其中一個目標。
|
|deviceIds |裝置 ID 所代表的裝置陣列。具有這些 ID 的裝置會收到通知。這是單點播送通知。
|
|notificationType |指出用來傳送訊息之通道（推送或 SMS）的整數值。容許的值為 1（僅限推送）、2（僅限 SMS）及 3（同時適用於推送及 SMS）
|
|platforms |裝置平台的陣列。在這些平台上執行的裝置會收到通知。支援的值為 A (Apple/iOS)、G (Google/Android) 及 M (Microsoft/Windows)。
|
|tagNames |指定為 tagNames 的標籤陣列。訂閱這些標籤的裝置會收到通知。將此類型的目標用於以標籤為基礎的通知。
|
|userIds |使用者的 userId 所代表之使用者陣列，用來傳送通知。這是單點播送通知。
|
|phoneNumber |用於登錄裝置及接收通知的電話號碼。這是單點播送通知。
|
{: caption="表 1. 有效負載內容" caption-side="top"}

**Push Notifications 有效負載 JSON 範例**

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

**SMS 通知有效負載 JSON 範例**

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

## 傳送通知
{: #sending-the-notification }

可以使用不同的工具來傳送通知。為了進行測試，會使用 **Postman**，下列步驟說明設定：

1. [配置機密用戶端](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/)。
  透過 REST API 傳送推送通知會使用以空格區隔的範圍元素 `messages.write` 及 `push.application.<applicationId>`。
  <img class="gifplayer" alt="配置機密用戶端" src="images/push-confidential-client.png"/>
2. [建立存取記號](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token)。  
3. 對 **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages** 提出 **POST** 要求。
  - 如果使用遠端 {{ site.data.keyword.mobilefirst_notm }}，請將 `hostname` 和 `port` 值取代為您自己的值。
  - 使用您自己的值來更新應用程式 ID 值。
4. 設定標頭：
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - 將 **Bearer** 之後的值取代為步驟 (1) 中存取記號的值。
    ![授權標頭](images/postman_authorization_header.png "授權標頭")
5. 設定內文：
  - 如[通知有效負載](#notification-payload)所述，更新其內容。
  - 例如，藉由新增具有 **userIds** 屬性的 **target** 內容，您可以傳送通知給特定的已登錄使用者。
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![授權主體](images/postman_json.png "授權主體")

    按一下**傳送**按鈕之後，裝置會收到通知：

    ![範例應用程式的影像](images/notifications-app.png "行動裝置上的推送通知")

## 自訂通知
{: #customizing-notifications }

傳送通知訊息之前，您也可以自訂下列通知屬性。  

在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的應用程式] → 推送 → 標籤 → 傳送通知標籤**，展開 **iOS/Android 自訂設定**區段以變更通知屬性。

### Android
{: #android }

* 通知音效，通知可以儲存在 FCM 儲存空間的時間長度、自訂有效負載及其他項目。
* 如果您要變更通知標題，請在 Android 專案的 **strings.xml** 檔案中新增 `push_notification_tile`。

### iOS
{: #ios }

* 通知音效、自訂有效負載、動作鍵標題、通知類型及識別證號碼。

  ![自訂推送通知](images/customizing-push-notifications.png "MobileFirst 作業主控台推送頁面，其中選取了「傳送推送」標籤")

## APN 推送通知的 HTTP/2 支援
{: #http2-support-for-apns-push-notifications}

Apple Push Notification Service (APNs) 支援以 HTTP/2 網路通訊協定為基礎的新 API。HTTP/2 的支援提供許多優點，包括：

* 從 2 KB 增加到 4 KB 的訊息長度，可讓您將額外內容新增至通知。
* 消除用戶端與伺服器之間有多個連線的需要，以改善傳輸量。
* 通用推送通知用戶端 SSL 憑證支援。

>{{ site.data.keyword.mobilefirst_notm }} 中的 Push Notifications 現在支援以 HTTP/2 為基礎的 APNs Push Notifications，以及舊式 TCP Socket 為基礎的通知。

### 啟用 HTTP/2
{: #enabling-http2}

以 HTTP/2 為基礎的通知可以使用 JNDI 內容來啟用。
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

如果新增了 JNDI 內容，則不會使用舊式 TCP Socket 為基礎的通知，而只會啟用以 HTTP/2 為基礎的通知。
{: note}

### HTTP/2 的 Proxy 支援
{: #proxy-support-for-http2}

以 HTTP/2 為基礎的通知可以透過 HTTP Proxy 傳送。若要啟用透過 Proxy 遞送通知，請參閱[這裡](#proxy-support)。

## Proxy 支援
{: #proxy-support }
您可以利用 Proxy 設定來設定用來傳送通知給 Android 和 iOS 裝置的選用性 Proxy。您可以使用 `push.apns.proxy.*` 及 `push.gcm.proxy.*` 配置內容來設定 Proxy。如需相關資訊，請參閱 [{{ site.data.keyword.mfserver_short_notm }} 推送服務的 JNDI 內容清單](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service)。

## 後續步驟
{: #next-tutorial-to-follow }
現在設定好伺服器端之後，請使用下列指導教學設定用戶端，並處理收到的通知。

* [處理用戶端應用程式中的推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
