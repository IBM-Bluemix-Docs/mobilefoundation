---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, MIM attack, certificate pinning

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 防止攔截式攻擊
{: #prevent_man_in_the_middle_attack}


透過公用網路通訊時，必須安全地傳送及接收資訊。廣泛用來保護這些通訊安全的通訊協定是 SSL/TLS。SSL/TLS 使用數位憑證來提供鑑別及加密。這些通訊協定依賴憑證鏈驗證，且容易遭到許多危險的攻擊（包括攔截式攻擊），而這些攻擊發生於未獲授權方可以檢視及修改所有透過行動裝置與後端系統之間的資料流量時。
{: shortdesc}

透過[憑證綁定](#cert_pinning)減少「安全攻擊」，並防止攔截式 (MitM) 攻擊。

此為選用特性，僅適用於「專業」方案。
{: note}

憑證綁定處理程序由下列步驟組成：

1. 執行[憑證設定](#certsetup)。
2. 先配置用戶端程式碼來呼叫正確的[憑證綁定 API](#certpinapi) 方法，再提出安全的要求。

在 SSL 信號交換期間（對伺服器的第一個要求），Mobile Foundation Client SDK 會驗證伺服器憑證的公開金鑰是否符合應用程式中所儲存憑證的公開金鑰。

您必須使用購買自憑證管理中心的憑證。**不支援**自簽憑證。
{: note}

## 憑證綁定
{: #cert_pinning}

根據憑證鏈驗證的通訊協定（例如 SSL/TLS）容易遭到許多危險的攻擊（包括攔截式攻擊），而這些攻擊發生於未獲授權方可以檢視及修改所有透過行動裝置與後端系統之間的資料流量時。若要信任憑證不是偽造的且有效，請由屬於授信憑證管理中心 (CA) 的主要憑證對其進行數位簽署。作業系統及瀏覽器會維護授信 CA 主要憑證清單，因此，它們可以輕鬆地驗證 CA 已發出並簽署的憑證。

IBM Mobile Foundation 提供 API 來啟用**憑證綁定**。已在原生 iOS、原生 Android 及跨平台 Cordova MobileFirst 應用程式中支援它。

## 憑證綁定處理程序
{: #certpinprocess}

憑證綁定是建立主機與其預期公開金鑰之關聯的處理程序。您可以配置用戶端程式碼僅接受您網域名稱的特定憑證，而非對應至作業系統或瀏覽器所辨識授信 CA 主要憑證的任何憑證。憑證的副本放在用戶端應用程式中。在 SSL 信號交換期間（對伺服器的第一個要求），MobileFirst Client SDK 會驗證伺服器憑證的公開金鑰是否符合應用程式中所儲存憑證的公開金鑰。

您也可以綁定多個憑證與用戶端應用程式。請將所有憑證的副本放在用戶端應用程式中。在 SSL 信號交換期間（對伺服器的第一個要求），MobileFirst Client SDK 會驗證伺服器憑證的公開金鑰是否符合應用程式中所儲存之其中一個憑證的公開金鑰。
{: note}

* **重要事項**
    * 部分行動作業系統可能會快取憑證驗證檢查結果。因此，在提出安全的要求**之前**，您的程式碼會呼叫憑證綁定 API 方法。否則，任何的後續要求都可能會跳過憑證驗證及綁定檢查。
    * 務必僅將 Mobile Foundation API 用於所有與相關主機的通訊，甚至在憑證綁定之後也是一樣。使用協力廠商 API 以與相同主機互動，可能會導致非預期的行為，例如由行動作業系統快取未經驗證的憑證。
    * 第二次呼叫憑證綁定 API 方法會置換前一個綁定作業。

如果綁定處理程序成功，則會使用所提供憑證內的公開金鑰來驗證安全要求 SSL/TLS 信號交換期間的 MobileFirst Server 憑證的完整性。如果綁定處理程序失敗，則用戶端應用程式會拒絕所有對伺服器的 SSL/TLS 要求。

## 憑證設定
{: #certsetup}

您必須使用購買自憑證管理中心的憑證。**不支援**自簽憑證。針對與所支援環境的相容性，請務必使用以 **DER**（識別編碼規則，如「國際電信聯盟 X.690」標準所定義）格式編碼的憑證。

憑證必須放在 MobileFirst Server 及應用程式中。請如下放置憑證：

* 在 MobileFirst Server（WebSphere Application Server、WebSphere Application Server Liberty 或 Apache Tomcat），如需如何配置 SSL/TLS 及憑證的相關資訊，請參閱特定應用程式伺服器的文件。
* 在應用程式中：
    * 若為原生 iOS，將憑證新增至應用程式**組合**
    * 若為原生 Android，將憑證放在 **assets** 資料夾中
    * 若為 Cordova，將憑證放在 **app-name\www\certificates** 資料夾（如果還沒有此資料夾，則請予以建立）

## 憑證綁定 API
{: #certpinapi}

憑證綁定由下列上線 API 方法所組成，其中，一種方法具有參數 `certificateFilename`，而 `certificateFilename` 是憑證檔名稱，而且第二種方法具有參數 `certificateFilenames`，而 `certificateFilenames` 是憑證檔名稱的陣列。

### Android
{: #certpinapiandroid}

* 單一憑證：

    語法：pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

    範例：

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* 多個憑證：

    語法：pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    範例：

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

憑證綁定方法會在兩種情況下引發異常狀況：

* 檔案不存在
* 檔案的格式錯誤

### iOS
{: #certpinapiios}

* 單一憑證綁定

    語法：pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    憑證綁定方法會在兩種情況下引發異常狀況：

    * 檔案不存在
    * 檔案的格式錯誤

* 多個憑證綁定

    語法：pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    憑證綁定方法會在兩種情況下引發異常狀況：

    * 憑證檔不存在
    * 憑證檔的格式不正確

**在 Objective-C 中**：

範例：單一憑證：

```objectc
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

範例：多個憑證：

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**在 Swift 中**：

範例：單一憑證：

```swift
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

範例：多個憑證：

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

憑證綁定方法將會在兩種情況下引發異常狀況：

* 檔案不存在
* 檔案的格式錯誤

### Cordova
{: #certpinapicordova}

* 單一憑證綁定：

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* 多個憑證綁定：

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

憑證綁定方法會傳回承諾：

* 如果綁定成功，則憑證綁定方法會呼叫 `onSuccess` 方法。
* 在下列兩種情況下，憑證綁定方法會觸發 `onFailure` 回呼。
  * 檔案不存在。
  * 檔案的格式錯誤。

稍後，如果對未綁定其憑證的伺服器提出安全的要求，則會呼叫特定要求的 `onFailure` 回呼（例如，`obtainAccessToken` 或 `WLResourceRequest`）。

請在 [API 參考資料](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks)中進一步瞭解憑證綁定 API 方法。
{: note}
