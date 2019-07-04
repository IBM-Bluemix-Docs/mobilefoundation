---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Push Notifications, notifications, unicast notifications, tag notifications, interactive notifications, silent notifications, configure DataPower

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

# 推送通知
{: #push_notifications}

通知是指行動裝置能夠接收從伺服器「推送」而來的訊息。不論應用程式是在前景還是背景執行，都會收到通知。  
{: shortdesc}

{{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} 提供一組統一的 API 方法，可將推送通知傳送至 iOS、Android、Windows 8.1 Universal、Windows 10 UWP 及 Cordova（iOS、Android）應用程式。通知會從 {{ site.data.keyword.mfserver_short_notm }} 傳送至供應商（Apple、Google、Microsoft、SMS 閘道）基礎架構，並從該處傳送到相關裝置。統一的通知機制使得與使用者及裝置進行通訊的整個處理程序，對於開發人員來說是透明的。

## 裝置支援
{: #device-support}
{{ site.data.keyword.mobilefoundation_short }} 中的下列平台支援推送通知：

* iOS 8.x 或更新版本
* Android 4.x 或更新版本
* Windows 8.1、Windows 10

## 推送通知形式
{: #push-notifications-forms}
通知可以採用數種格式：

* **警示**（iOS、Android、Windows）- 蹦現文字訊息
* **音效**（iOS、Android、Windows）- 收到通知時播放的音效檔
* **徽章** (iOS)、磚 (Windows) - 容許簡短文字或影像的圖形表示
* **橫幅** (iOS) 或快顯通知 (Windows) - 裝置顯示畫面中出現的蹦現畫面文字訊息
* **互動式**（iOS 8 及更高版本）- 已接收通知橫幅內部的動作按鈕
* **無聲自動**（iOS 8 及更新版本）- 在不干擾使用者的情況下傳送通知

## 推送通知類型
{: #push-notification-types }

### 標籤通知
{: #tag-notifications-overview}

標籤通知是通知訊息，以所有訂閱特定標籤的裝置為目標。  

以標籤為基礎的通知容許根據主旨區域或主題區隔通知。只有在通知是所感興趣的主旨或主題時，通知收件者才能選擇接收通知。因此，以標籤為基礎的通知提供一種方法來區隔收件者。憑藉此特性，您可以定義標籤，並透過標籤傳送或接收訊息。訊息只以訂閱標籤的裝置為目標。

### 播送通知
{: #broadcast-notifications-overview}

播送通知是以所有訂閱裝置為目標的標籤推送通知形式，只要訂閱保留的 `Push.all` 標籤（為每個裝置自動建立），依預設即可為任何啟用推送的 {{ site.data.keyword.mobilefirst_notm }} 應用程式啟用此功能。透過取消訂閱保留的 `Push.all` 標籤，就能停用播送通知。

### 單點播送通知
{: #unicast-notifications-overview}

使用 OAuth 可保護單點播送通知或「使用者鑑別的通知」。這些通知訊息是以特定裝置或使用者 ID 為目標。使用者訂閱中的使用者 ID 可來自基礎安全環境定義。

### 互動式通知
{: #interactive-notifications-overview}

透過互動式通知，當通知到達時，使用者不必開啟應用程式就可以執行動作。當互動式通知到達時，裝置會顯示動作按鈕以及通知訊息。目前，具有 iOS 第 8 版以上版本的裝置支援互動式通知。如果互動式通知傳送至版本低於 8 的 iOS 裝置，則不會顯示通知動作。

瞭解如何處理[互動式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)。

### 無聲自動通知
{: #silent-notifications-overview}

無聲自動通知是指不會顯示警示，也不會以其他方式干擾使用者的通知。當無聲自動通知到達時，應用程式處理程式碼會在背景中執行，而不會將應用程式帶到前景。目前，具有第 7 版以上版本的 iOS 裝置支援無聲自動通知。如果無聲自動通知傳送至版本低於 7 的 iOS 裝置，則應用程式在背景中執行時會忽略通知。如果應用程式是在前景中執行，則會呼叫通知回呼方法。

瞭解如何處理[無聲自動通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)。

單點播送通知在有效負載中未包含任何標籤。在 POST 訊息 API 的目標區塊中，透過分別指定多個裝置 ID 或使用者 ID，通知訊息可以多個裝置或使用者為目標。
{: note}

## Proxy 設定
{: #proxy-settings}

使用 Proxy 設定來設定選用的 Proxy，透過該 Proxy 可以將通知傳送給 APNS 及 FCM。您可以設定 Proxy，方法是使用 **push.apns.proxy.* ** 及 **push.gcm.proxy.*** 配置內容。如需相關資訊，請參閱 [{{ site.data.keyword.mfserver_short_notm }} 推送服務的 JNDI 內容清單](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service)。

WNS 沒有 Proxy 支援。
{: note}

### 使用 WebSphere DataPower 作為推送通知端點
{: #proxy-settings-datapower-endpoint}

您可以設定 DataPower 以接受來自 {{ site.data.keyword.mfserver_short_notm }} 的通知要求，並將其重新導向至 FCM 及 WNS。

不支援 APNS。
{: note}

#### 配置 {{ site.data.keyword.mfserver_short_notm }}
{: #proxy-settings-config-mfp-server}

在 `server.xml` 中配置下列 JNDI 內容。
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

其中，`host` 是 DataPower 的主機名稱，而 `port` 是為 FCM 及 WNS 配置「HTTPS 前端處理程式」的埠號。

#### 配置 DataPower
{: #proxy-settings-config-datapower}

1. 登入 DataPower 應用裝置。
2. 導覽至**服務 > 多重通訊協定閘道 > 新建多重通訊協定閘道**。
3. 提供您可以用來識別配置的名稱。
4. 選取「XML 管理員」、「多重通訊協定閘道原則」作為預設值，並將「URL 重新編寫原則」設為無。
5. 選取 **static-backend** 圓鈕，並針對**設定預設後端 URL** 選取下列任一選項：
    - 若為 FCM，則為	`https://gcm-http.googleapis.com`
    - 若為 WNS，則為	`https://hk2.notify.windows.com`
6. 將「回應類型」、「要求類型」選取為透通。

### 產生憑證
{: #proxy-settings-generate-cert}

若要產生憑證，請選擇下列任一選項：

* 對於 FCM：
    1. 從指令行發出 `Openssl` 來取得 FCM 憑證。
    2. 執行下列指令：
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
        {: codeblock}
    3. 複製從 *-----BEGIN CERTIFICATE-----  到 -----END CERTIFICATE-----* 的內容，並將其儲存在副檔名為 `.pem` 的檔案中。

* 對於 WNS：
    1. 從指令行使用 `Openssl` 來取得 WNS 憑證。
    2. 執行下列指令：
		```
openssl s_client -connect https://hk2.notify.windows.com:443
		```
        {: codeblock}
    3. 複製從 *-----BEGIN CERTIFICATE-----  到 -----END CERTIFICATE-----* 的內容，並將其儲存在副檔名為 `.pem` 的檔案中。

### 後端設定
{: #proxy-settings-backside-settings}

* 若為 FCM 及 WNS：
    1. 建立加密憑證。
        a. 導覽至**物件 > 加密配置**，然後按一下「加密憑證」。
        b. 提供您可以用來識別加密憑證的名稱。
        c. 按一下**上傳**以上傳產生的 FCM 憑證。
        d. 將**密碼別名**設為無。
        e. 按一下**產生金鑰**。
        ![配置加密憑證](images/bck_1.gif "配置加密憑證")

    2. 建立加密驗證認證。
        a. 導覽至**物件 > 加密配置**，然後按一下「加密驗證認證」。
        b. 提供唯一名稱。
        c. 針對「憑證」，選取您在前一個步驟（步驟 1）中建立的「加密憑證」。
        d. 針對**憑證驗證模式**，選取「符合確切憑證或直接發證者」。
        e. 按一下**套用**。

        ![配置加密驗證認證](images/bck_2.gif "配置加密驗證認證")

    3. 建立加密驗證認證：
        a. 導覽至**物件 > 加密配置**，然後按一下**加密設定檔**。
        b. 按一下**新增**。
        c. 提供唯一名稱。
        d. 針對**驗證認證**，從下拉功能表中選取前一個步驟（步驟 2）所建立的驗證認證。將「識別認證」設為**無**。
        e. 按一下**套用**。

        ![配置加密設定檔](images/bck_3.gif "配置加密設定檔")

    4. 建立 SSL Proxy 設定檔：

        a. 導覽至**物件** > **加密配置** > **SSL Proxy 設定檔**。
        b. 選擇下列任一選項：

            - 若為 SMS，請將 **SSL Proxy 設定檔**選取為無。
            - 若為使用安全後端 URL (HTTPS) 的 FCM 及 WNS，請完成下列步驟：
                i. 按一下**新增**。
                ii. 提供您稍後可以用來識別 SSL Proxy 設定檔的名稱。
                iii. 從下拉清單中將 **SSL 方向**選取為**正向**。
                iv. 針對「正向（用戶端）加密設定檔」，選取步驟 3 所建立的加密設定檔。
                v. 按一下**套用**。

        ![配置 SSL Proxy 設定檔](images/bck_4.gif "配置 SSL Proxy 設定檔（淘汰）")

    5. 在「多重通訊協定閘道」視窗的**後端設定**下，選取 **Proxy 設定檔**作為 **SSL 用戶端類型**，然後選取步驟 4 所建立的「SSL Proxy 設定檔」。

        ![配置 SSL Proxy 設定檔](images/bck_5.gif "「多重通訊協定閘道」視窗")

### 前端設定
{: #proxy-settings-frontside-settings}

* 若為 FCM 及 WNS：

    1. 建立以「通用名稱 (CN)」值作為 DataPower 主機名稱的金鑰憑證配對：
        a. 導覽至**管理** > **細項**，然後按一下**加密工具**。
        b. 輸入 DataPower 的主機名稱作為「通用名稱 (CN)」的值。
        c. 如果您計劃稍後匯出私密金鑰，請選取**匯出私密金鑰**，然後按一下**產生金鑰**。

        ![建立金鑰憑證配對](images/frnt_1.gif "建立金鑰憑證配對")

    2. 建立加密識別認證：
        a. 導覽至**物件** > **加密配置**，然後按一下**加密識別認證**。
        b. 按一下**新增**。
        c. 提供唯一名稱。
        d. 針對「加密金鑰」及「憑證」，從清單框選取前一個步驟（步驟 1）所產生的金鑰及憑證。
        e. 按一下**套用**。

        ![建立加密識別認證](images/frnt_2.gif "建立加密識別認證")

    3. 建立加密設定檔：

        a. 導覽至**物件** > **加密配置**，然後按一下**加密設定檔**。
        b. 按一下**新增**。
        c. 提供唯一名稱。
        d. 針對「識別認證」，從清單框中選取前一個步驟（步驟 2）所建立的識別認證。將「驗證認證」設定為「無」。
        e. 按一下**套用**。

        ![配置加密設定檔](images/frnt_3.gif "配置加密設定檔")

    4. 建立 SSL Proxy 設定檔：

        a. 導覽至**物件** > **加密配置** > **SSL Proxy 設定檔**。
        b. 按一下**新增**。
        c. 提供唯一名稱。
        d. 從清單框中將「SSL 方向」選取為**反向**。
        e. 針對「反向（伺服器）加密設定檔」，選取前一個步驟（步驟 3）所建立的加密設定檔。  
        f. 按一下**套用**。

        ![配置 SSL Proxy 設定檔](images/frnt_4.gif "配置 SSL Proxy 設定檔")

    5. 建立 HTTPS 前端處理程式：

        a. 導覽至**物件** > **通訊協定處理程式** > **HTTPS 前端處理程式**。
        b. 按一下**新增**。
        c. 提供唯一名稱。
        d. 針對**本端 IP 位址**，選取正確的別名或將其保留為預設值 (0.0.0.0)。
        e. 提供可用的埠。
        f. 針對**容許的方法及版本**，選取 HTTP 1.0、HTTP 1.1、POST 方法、GET 方法、含 ? 的 URL、含 # 的 URL、含 . 的 URL。
        g. 選取 **Proxy 設定檔**作為 SSL 伺服器類型。
        h. 針對 SSL Proxy 設定檔（已淘汰），選取前一個步驟（步驟 4）所建立的 SSL Proxy 設定檔。
        i. 按一下**套用**。

        ![配置 HTTPS 前端處理程式](images/frnt_5.gif "配置 HTTPS 前端處理程式")

    6. 在「配置多重通訊協定閘道」頁面的**前端設定**下，選取步驟 5 所建立的 https 前端處理程式作為**前端通訊協定**，然後按一下**套用**。

        ![一般配置](images/frnt_6.gif "一般配置")

    DataPower 在「前端設定」中使用的憑證是自簽憑證。除非將憑證新增至 {{ site.data.keyword.mobilefirst_notm }} 所使用的 JRE 金鑰儲存庫，否則與 DataPower 的連線會失敗。

## 後續步驟
{: #next-steps }

完成必要的伺服器端及用戶端設定，以傳送及接收推送通知：

* [配置推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [傳送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [處理用戶端應用程式中的推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [無聲自動通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [互動式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
