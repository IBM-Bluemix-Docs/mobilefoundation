---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

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


# 配置推送通知
{: #configure_push_notifications}

為了將推送通知傳送至 iOS、Android 或 Windows 裝置，{{ site.data.keyword.mfserver_short_notm }} 必須先配置 Android 的 FCM 詳細資料（適用於 iOS 的 APNS 憑證或適用於 Windows 8.1 Universal / Windows 10 UWP 的 WNS 憑證）。
{: shortdesc}

然後，可以使用下列選項來傳送通知：
* 所有裝置（播送）
* 登錄至特定標籤的裝置
* 單一裝置 ID
* 使用者 ID
* 僅限 iOS 裝置
* 僅限 Android 裝置
* 僅限 Windows 裝置
* 根據已鑑別使用者。

## 設定通知
{: #setting-up-notifications }

啟用通知支援包括 {{ site.data.keyword.mfserver_short_notm }} 及用戶端應用程式中的數個配置步驟。請繼續讀取伺服器端設定，或跳至[用戶端設定](#tutorials-to-follow-next)。

在伺服器端上，必要的設定包括：配置所需的供應商（APNS、FCM 或 WNS），並對映 "push.mobileclient" 範圍。

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

Google [已淘汰 GCM](https://developers.google.com/cloud-messaging/faq) 並整合了 Cloud Messaging 與 Firebase。如果您使用 GCM 專案，請確保您[將 Android 上的 GCM 用戶端應用程式移轉至 FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)。
{: note}

Android 裝置使用 Firebase Cloud Messaging (FCM) 服務來進行推送通知。

若要設定 FCM，請執行下列動作：

1. 造訪 [Firebase 主控台](https://console.firebase.google.com/?pli=1)。
2. 建立專案並提供專案名稱。
3. 按一下「設定」的「齒輪」圖示，並選取**專案設定**。
4. 按一下 **Cloud Messaging** 標籤以產生**伺服器 API 金鑰**及**寄件者 ID**，然後按一下**儲存**。

您也可以使用 [{{ site.data.keyword.mobilefirst_notm }} Push 服務的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) 或 [{{ site.data.keyword.mobilefirst_notm }} 管理服務的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi) 來設定 FCM。
{: note}

如果您組織的防火牆會限制往返網際網路的資料流量，則必須執行下列步驟：  
* 配置防火牆以容許與 FCM 連線，以便讓 FCM 用戶端應用程式接收訊息。
* 要開啟的埠是 5228、5229 及 5230。FCM 通常只會使用 5228，但有時會使用 5229 及 5230。
* FCM 不會提供特定的 IP，因此您必須容許防火牆接受 Google ASN 15169 所列 IP 區塊包含之所有 IP 位址的送出連線。
* 確保防火牆在埠 443 上接受從 {{ site.data.keyword.mfserver_short_notm }} 到 fcm.googleapis.com 的送出連線。
{: note }

<img class="gifplayer" alt="新增 GCM 認證的影像" src="images/gcm-setup.png"/>

### Apple Push Notifications Service
{: #apple-push-notifications-service }

iOS 裝置會使用 Apple 的 Push Notification Service (APNS) 來進行推送通知。  

若要設定 APNS，請執行下列動作：

1. 產生開發或正式作業的推送通知憑證。如需詳細步驟，請參閱[這裡](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)中的`適用於 iOS` 一節。
2. 在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的應用程式] → 推送 → 推送設定**中，選取憑證類型並提供憑證的檔案及密碼。然後，按一下**儲存**。

  * 若要傳送推送通知，必須可從 {{ site.data.keyword.mfserver_short_notm }} 實例存取下列伺服器。
    * 沙盤推演伺服器：
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * 正式作業伺服器：
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * 在開發階段期間，請使用 apns-certificate-sandbox.p12 沙盤推演憑證檔。
  * 在正式作業階段期間，請使用 apns-certificate-production.p12 正式作業憑證檔。
    * 只有在利用 APNS 正式作業憑證的應用程式順利提交到 Apple App Store 時，才能測試該憑證。
    {: note }

MobileFirst 不支援通用憑證。
{: note }

您也可以使用 [{{ site.data.keyword.mobilefirst_notm }} Push 服務的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) 或 [{{ site.data.keyword.mobilefirst_notm }} 管理服務的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc) 來設定 APNS。
{: note}

<img class="gifplayer" alt="新增 APNS 認證的影像" src="images/apns-setup.png"/>

### Windows Push Notifications Service
{: #windows-push-notifications-service }

Windows 裝置會使用 Windows Push Notifications Service (WNS) 來進行推送通知。  

若要設定 WNS，請執行下列動作：

1. 遵循 [Microsoft 所提供的指示](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx)來產生**套件安全 ID (SID)**及**用戶端密碼**值。
2. 在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的應用程式] → 推送 → 推送設定**中新增這些值，然後按一下**儲存**。

您也可以使用 [{{ site.data.keyword.mobilefirst_notm }} Push 服務的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) 或 [{{ site.data.keyword.mobilefirst_notm }} 管理服務的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc) 來設定 WNS。

<img class="gifplayer" alt="新增 WNS 認證的影像" src="images/wns-setup.png"/>

### 範圍對映
{: #scope-mapping }

將 **push.mobileclient** 範圍元素對映至應用程式。

1. 載入 {{ site.data.keyword.mfp_oc_short_notm }}，並導覽至 **[您的應用程式] → 安全 → 範圍元素對映**，按一下**新建**。
2. 在**範圍元素**欄位中寫入 "push.mobileclient"。然後，按一下**新增**。

更多可用範圍的清單：

|**範圍** | **說明**|
|---|---|
|apps.read | 讀取應用程式資源的許可權。|
|apps.write | 建立、更新、刪除應用程式資源的許可權。|
|gcmConf.read | 讀取 GCM 配置設定（API 金鑰及傳送端 ID）的許可權。|
|gcmConf.write | 更新、刪除 GCM 配置設定的許可權。|
|apnsConf.read | 讀取 APNS 配置設定的許可權。|
|apnsConf.write | 更新、刪除 APNS 配置設定的許可權。|
|devices.read | 讀取裝置的許可權。|
|devices.write | 建立、更新、刪除裝置的許可權。|
|subscriptions.read | 讀取訂閱的許可權。|
|subscriptions.write | 建立、更新、刪除訂閱的許可權。|
|messages.write | 傳送推送通知的許可權。|
|webhooks.read | 讀取事件通知的許可權。|
|webhooks.write | 傳送事件通知的許可權。|
|smsConf.read | 讀取 SMS 配置設定的許可權。|
|smsConf.write | 更新、刪除 SMS 配置設定的許可權。|
|wnsConf.read | 讀取 WNS 配置設定的許可權。|
|wnsConf.write | 更新、刪除 WNS 配置設定的許可權。|
{: caption="表 1. 範圍說明" caption-side="top"}

<img class="gifplayer" alt="範圍對映" src="images/scope-mapping.png"/>

### 已鑑別通知
{: #authenticated-notifications }

已鑑別通知是傳送給一個以上 `userIds` 的通知。  

將 **push.mobileclient** 範圍元素對映至用於應用程式的安全檢查。  

1. 載入 {{ site.data.keyword.mfp_oc_short_notm }}，並導覽至 **[您的應用程式] → 安全 → 範圍元素對映**，按一下**新建**或編輯現有的範圍對映項目。
2. 選取安全檢查。然後，按一下**新增**。

  <img class="gifplayer" alt="已鑑別通知" src="images/authenticated-notifications.png"/>

## 定義標籤
{: #defining-tags }
在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的應用程式] → 推送 → 標籤**中，按一下**新建**。  
提供適當的`標籤名稱`及`說明`，然後按一下**儲存**。

<img class="gifplayer" alt="新增標籤" src="images/adding-tags.png"/>

訂閱會將裝置登錄與標籤連結在一起。從標籤取消登錄裝置時，即會從裝置本身自動取消訂閱所有相關聯的訂閱。在一個裝置有多個使用者的情境中，必須根據使用者登入準則，在行動應用程式中實作訂閱。例如，在使用者順利登入應用程式之後提出訂閱呼叫，並在登出動作處理期間明確提出取消訂閱呼叫。

## 後續要遵循的指導教學
{: #tutorials-to-follow-next }

* [傳送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

在目前設定伺服器端之後，請設定用戶端並處理收到的通知。

* [處理用戶端應用程式中的推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
