---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, restrict backend access, tampered apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# 防止遭竄改的應用程式存取後端
{: #prevent_tampered_apps_accessing_backend}

應用程式確實性有助於在啟用任何服務之前檢查應用程式有效性，因此，防止遭竄改的應用程式存取後端服務。
{: shortdesc}

若要適當地保護應用程式安全，請啟用預先定義的 MobileFirst 應用程式確實性安全檢查 (`appAuthenticity`)。啟用時，這項檢查會先驗證應用程式的確實性，再向任何服務提供它。正式作業環境中的應用程式應該啟用此特性。

若要啟用應用程式確實性，可以遵循 **MobileFirst 作業主控台 → [您的應用程式] → 確實性**中的畫面上指示，或檢閱下面的資訊。

<dl>
  <dt>可用性</dt>
  <dd>在 Cordova 及原生應用程式中，所有支援的平台（iOS、Android、Windows 8.1 Universal、Windows 10 UWP）都提供應用程式確實性。</dd>
  <dt>限制</dt>
  <dd>在 iOS 中，應用程式確實性不支援 **Bitcode**。因此，使用應用程式確實性之前，請在 Xcode 專案內容中停用 Bitcode。</dd>
</dl>

## 應用程式確實性流程
{: #appauthenticityflow}

應用程式確實性安全檢查是在將應用程式登錄至 MobileFirst Server 期間執行，而這項登錄是在第一次應用程式實例嘗試連接至伺服器時進行。依預設，不會重新執行確實性檢查。

一旦啟用應用程式確實性之後，如果客戶需要在其應用程式中建立任何變更，則需要升級應用程式版本。

請參閱[配置應用程式確實性](#configappauthenticity)，以瞭解如何自訂此行為。

### 啟用應用程式確實性
{: #enableappauthenticity}

若要在應用程式中啟用應用程式確實性，請執行下列動作：

1. 在慣用的瀏覽器中，開啟「MobileFirst 作業主控台」。
2. 從導覽資訊看板中選取應用程式，然後按一下**確實性**功能表項目。
3. 切換**狀態**框中的**開啟/關閉**按鈕。

![啟用應用程式確實性](/images/enable_application_authenticity.png)

MobileFirst Server 會在第一次嘗試連接至伺服器時驗證應用程式確實性。若也要將此驗證套用至受保護的資源，請將 appAuthenticity 安全檢查新增至保護範圍。

### 停用應用程式確實性
{: #disableappauthenticity}

應用程式在開發期間的部分變更可能會導致它讓確實性驗證失敗。相應地，建議在開發處理程序期間停用應用程式確實性。正式作業環境中的應用程式應該啟用此特性。

若要停用應用程式確實性，請切換回**狀態**框中的**開啟/關閉**按鈕。

## 配置應用程式確實性
{: #configappauthenticity}

依預設，在用戶端登錄期間只會檢查「應用程式確實性」。不過，就像任何其他安全檢查一樣，您可以遵循「保護資源」下的指示，以從主控台決定使用 `appAuthenticity` 安全檢查來保護應用程式或資源。

您可以使用下列內容來配置預先定義的應用程式確實性安全檢查：

* `expirationSec`：預設為 3600 秒/1 小時。除非確實性記號到期，否則請定義持續時間。

確實性檢查完成之後，除非記號根據的設定值已過期，否則不會重新執行。

若要配置 `expirationSec` 內容，請執行下列動作：

1. 載入「MobileFirst 作業主控台」，並導覽至 **[您的應用程式] → 安全 → 安全檢查配置**，然後按一下**新建**。
2. 搜尋 `appAuthenticity` 範圍元素。
3. 設定新值（以秒為單位）。

    ![以秒數配置有效期限](/images/configuring_expirationSec.png)

## 建置時期密碼 (BTS)
{: #buildtimesecret}

「建置時期密碼 (BTS)」是**加強確實性驗證的選用工具**，但僅適用於 iOS 應用程式。此工具會將在建置時期判定的密碼注入應用程式，而且稍後會將此密碼用於確實性驗證處理程序。

您可以從 **MobileFirst 作業主控台 → 下載中心**下載 BTS 工具。

若要以 Xcode 使用 BTS 工具，請執行下列動作：

1. 在**建置階段**標籤下，按一下 **+** 按鈕，然後建立新的**執行 Script 階段**。
2. 複製「BTS 工具」的路徑，並將其貼入您已建立的新「執行 Script 階段」。
3. 將該執行 Script 階段拖曳至**編譯來源階段**上方。

建置應用程式的正式作業版本時，應該使用此工具。

## 疑難排解應用程式確實性
{: #troubleshooting-app-authenticity}

### 重設
{: #trblreset}

應用程式確實性演算法會在其驗證中使用應用程式資料及 meta 資料。啟用應用程式確實性之後連接至伺服器的第一個裝置會提供應用程式的「指紋」（包含這項資料的一部分）。

將新資料提供給演算法，可能會重設此指紋。這在開發期間可能十分有用（例如，以 Xcode 變更應用程式之後）。若要重設指紋，請從 mfpadm CLI 中使用 **reset** 指令。

重設指紋之後，appAuthenticity 安全檢查會繼續如以前一般地運作（使用者不會察覺這項作業）。

### 驗證類型
{: #trblvalidationtypes}

MobileFirst Platform Foundation 提供應用程式的靜態及動態應用程式確實性。對於用來產生應用程式確實性種子的演算法及屬性，這些驗證類型會不同。依預設，應用程式確實性在啟用時會使用動態驗證演算法。兩種驗證類型都確保應用程式的安全。動態應用程式確實性使用嚴格驗證，並檢查應用程式確實性。針對靜態應用程式確實性，我們使用稍微放鬆的演算法，而這類演算法不會使用動態應用程式確實性中使用的所有驗證檢查。

動態應用程式確實性可以從「MobileFirst 主控台」中配置。內部演算法會根據主控台中所選擇的選項來產生應用程式確實性資料。針對靜態應用程式確實性，則需要使用 mfpadm CLI。

如需啟用靜態應用程式確實性，以及若要切換驗證類型，請使用 mfpadm CLI：

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE 可以是動態或靜態。
