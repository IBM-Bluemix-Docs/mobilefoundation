---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 直接更新
{: #direct_update}

Cordova 或 Ionic 應用程式中的 Web 資源（JavaScript、HTML、CSS 或影像檔）可以透過無線傳輸 (OTA) 方式，利用**直接更新**來進行更新，使用者不需要進行 App Store 或「Play 商店」更新。使用「直接更新」特性，企業可以確保一般使用者使用其應用程式的最新版本。若要更新應用程式，需要將應用程式的已更新 Web 資源包裝並上傳至 Mobile Foundation 實例，方法為使用 Mobile Foundation CLI 或部署已產生的保存檔。然後，「直接更新」即會自動啟動。接著，會強制執行每一位使用者對受保護資源的要求。

![直接更新運作方式的圖表](images/internal_function.jpg)

Cordova iOS 及 Cordova Android 平台中支援「直接更新」。
{: note}

對於即時生產或正式作業前測試階段，建議先實作安全的「直接更新」，再將您的應用程式發佈至應用程式市集。安全「直接更新」需要從實際 CA 簽署伺服器憑證中擷取的 RSA 金鑰組。

1. 請小心，不要在發佈應用程式之後，修改金鑰儲存庫配置。在利用新的公開金鑰重新配置應用程式，並重新發佈應用程式之前，無法鑑別已下載的更新項目。如果不執行前述兩個步驟，在用戶端上執行「直接更新」會失敗。
2. 「直接更新」只會更新應用程式的 Web 資源。若要更新原生資源，必須將新的應用程式版本提交給各自的應用程式市集。
3. 使用「直接更新」特性，並已啟用 Web 資源總和檢查特性時，會使用每一個「直接更新」來建立新的總和檢查基礎。
4. 如果已升級 Mobile Foundation Server，則它會繼續適當地提供直接更新項目。不過，如果已上傳最近建置的「直接更新」保存檔（.zip 檔案），則它可能會中止更新至舊版用戶端。原因是此保存檔包含 `cordova-plugin-mfp` 外掛程式的版本。在它將該保存檔提供給行動用戶端之前，伺服器會比較用戶端版本與外掛程式版本。如果這兩個版本相當接近（表示三個最有效位數相同），則「直接更新」即會正常發生。否則，Mobile Foundation Server 會無聲自動跳過更新。版本不符的解決方案為下載版本相同的 `cordova-plugin-mfp`，作為原始 Cordova 專案中的外掛程式，並重新產生「直接更新」保存檔。
{: tip}

在「直接更新」之後，應用程式不再使用預先包裝的 Web 資源。它會改用從應用程式的沙盤推演中下載的 Web 資源。如果清除了裝置上的應用程式快取，則會再次使用原始包裝的 Web 資源。

Web 資源的「直接更新」僅適用於應用程式的特定版本。例如，針對應用程式 2.0 版產生的更新項目將不會遞送至相同應用程式的 2.1 版。
{: note}

## 建立及部署已更新的 Web 資源
{: #creating_deploying_updates}

變更 Web 資源之後，您可以使用 Mobile Foundation CLI 來包裝 Web 資源，並將它們上傳至 Mobile Foundation 實例。

1.  「直接更新」只會更新成應用程式的 Web 資源。若要更新原生資源，必須將新的應用程式版本提交給各自的應用程式市集。
2. 如果應用程式的原生部分與上傳至 Mobile Foundation 服務的原生部分明顯不同，則 Mobile Foundation 服務會中止直接更新至用戶端。使用較新版的外掛程式更新 *cordova-plugin-mfp* 時，可能會發生此狀況。在它將該保存檔提供給行動用戶端之前，伺服器會比較用戶端版本與外掛程式版本。如果這兩個版本相當接近（表示版本 ID 中的三個最有效位數相同），則「直接更新」會正常發生。否則，Mobile Foundation Server 會無聲自動跳過更新。版本不符的解決方案為下載版本相同的 *cordova-plugin-mfp*，作為原始 Cordova 專案中的外掛程式，並重新產生「直接更新」保存檔。
{: tip}

1. 開啟指令行視窗，並導覽至 Cordova 專案的根目錄。
2. 執行下列指令：
  ```bash
  mfpdev app webupdate
  ```
  `mfpdev app webupdate` 指令會將已更新的 Web 資源包裝為 `.zip` 檔案，然後將它上傳至在開發人員工作站中執行的預設 Mobile Foundation Server。您可在 `[cordova-project-root-folder]/mobilefirst/` 資料夾中找到包裝的 Web 資源。

如需更新應用程式中 Web 內容的替代步驟，請參閱[這裡](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app)。

## 直接更新進階配置
{: #direct_update_advanced_config}

如需有關「直接更新」配置的進階主題，請參閱[這裡](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration)。
