---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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


# 鑑別及安全
{: #basic_authentication}

MobileFirst 安全架構是以 [OAuth 2.0](http://oauth.net/) 通訊協定為基礎。根據此通訊協定，可以透過**範圍**定義用於存取資源的必要許可權來保護資源。若要存取受保護的資源，用戶端必須提供相符的**存取記號**，以封裝可授與用戶端的授權範圍。

OAuth 通訊協定會區隔授權伺服器與資源管理所在之資源伺服器的角色。

* 授權伺服器會管理用戶端授權及記號產生。
* 資源伺服器使用授權伺服器來驗證用戶端所提供的存取記號，並確定它符合所要求資源的保護範圍。

安全架構以實作 OAuth 通訊協定的授權伺服器為主軸，並公開用戶端與其互動以取得存取記號的 OAuth 端點。安全架構提供構成要素，以透過授權伺服器及基礎 OAuth 通訊協定來實作自訂授權邏輯。依預設，MobileFirst Server 也會充當**授權伺服器**。不過，您可以配置 IBM WebSphere DataPower 應用裝置來充當授權伺服器，並與 MobileFirst Server 互動。

用戶端應用程式接著可以使用這些記號來存取**資源伺服器**上的資源，而資源伺服器可以是 MobileFirst Server 本身或外部伺服器。資源伺服器會檢查記號的有效性，以確定可以將所要求資源的存取權授與給用戶端。資源伺服器與授權伺服器之間的區隔，可用來對在 MobileFirst Server 外部執行的資源施行安全保護。

應用程式開發人員藉由定義每個受保護資源的必要範圍，以及實作安全檢查和盤查處理程式，來保護對其資源的存取權。伺服器端安全架構及用戶端 API 會透通地處理 OAuth 訊息交換以及與授權伺服器的互動，讓開發人員可以專注於授權邏輯。

## 授權實體
{: #acs_authorization_entitiesty}

### 存取記號
{: #acs_access_tokens}

MobileFirst 存取記號是說明用戶端授權許可權的數位簽署實體。授與特定範圍的用戶端授權要求並鑑別用戶端之後，授權伺服器的記號端點會將包含所要求存取記號的 HTTP 回應傳送給用戶端。

#### 存取記號的結構

MobileFirst 存取記號包含下列資訊：

* **用戶端 ID**：用戶端的唯一 ID。
* **範圍**：已授與記號的範圍（請參閱 OAuth 範圍）。此範圍未包含必要的應用程式範圍。
* **記號有效期限**：記號變成無效（到期）的時間（以秒為單位）。

#### 記號有效期限
{: #token-expiration}

在超過所授與存取記號的有效期限之前，所授與的存取記號都會保持有效。存取記號的有效期限設為範圍內所有安全檢查的有效期限中的最短有效期限。但是，如果到最短有效期限之前的期間，比應用程式的最長記號有效期限還要長，則記號的有效期限會設為現行時間加上最長有效期限。預設的最長記號有效期限（有效性持續時間）為 3,600 秒（1 小時），但是設定 `maxTokenExpiration` 內容的值即可加以配置。

##### 配置最長存取記號有效期限
{: #acs_config-max-access-tokens}

使用下列其中一種替代方法，來配置應用程式的最長存取記號有效期限：

* 使用「MobileFirst 作業主控台」
    1. 選取 **[您的應用程式]** → **安全**標籤。
    2. 在**記號配置**區段中，將**最長記號有效期限（秒）**欄位的值設為偏好的值，然後按一下**儲存**。您隨時可以重複此程序以變更最長記號有效期限，或選取**還原預設值**以還原預設值。

* 編輯應用程式的配置檔

    1. 從 CLI 中，導覽至專案根目錄資料夾並執行下列指令。
      ```bash
      mfpdev app pull
      ```
    2. 開啟配置檔，其位於 `[project-folder]\mobilefirst` 資料夾中。
    3. 編輯檔案：定義 `maxTokenExpiration` 內容，並將其值設為最長存取記號有效期限（以秒為單位）：
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. 執行下列指令，來部署已更新的配置 JSON 檔案：`mfpdev app push`。

##### 存取記號回應結構
{: #acs_access-tokens-structure}

存取記號要求的成功 HTTP 回應，會包含具有存取記號及額外資料的 JSON 物件。以下是來自授權伺服器的有效記號回應範例：

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

記號回應 JSON 物件具有下列內容物件：

* **token_type**：根據 [OAuth 2.0 Bearer Token 用法規格](https://tools.ietf.org/html/rfc6750)，記號類型一律為 "*Bearer*"。
* **expires_in**：存取記號的有效期限（以秒為單位）。
* **access_token**：產生的存取記號（實際存取記號會比範例中顯示的存取記號還要長）。
* **scope**：所要求的範圍。

**expires_in** 及 **scope** 資訊也會包含在記號本身之內 (**access_token**)。

**附註**：如果您使用低階 `WLAuthorizationManager` 類別並自行管理用戶端與授權及資源伺服器之間的 OAuth 互動，或者，如果您使用機密用戶端，則有效存取記號回應的結構有關。如果您要使用高階 `WLResourceRequest` 類別，以封裝用於存取受保護資源的 OAuth 流程，則安全架構會為您處理存取記號回應的處理程序。請參閱[用戶端安全 API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) 及[機密用戶端](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/)。

### 重新整理記號
{: #acs_refresh_tokens}

「重新整理記號」是特殊類型的記號，可用來在存取記號到期時取得新的存取記號。若要要求新的存取記號，可以呈現有效的重新整理記號。重新整理記號是長期存在的記號，但相較於存取記號，會保持較長的有效持續時間。

應用程式使用重新整理記號時請小心，因為它們可以容許使用者永久保持已鑑別。應用程式提供者不會定期鑑別使用者的社交媒體應用程式、電子商務應用程式、產品型錄瀏覽及這類公用程式應用程式，可以使用重新整理記號。頻繁要求進行使用者鑑別的應用程式必須避免使用重新整理記號。MobileFirst 重新整理記號

MobileFirst 重新整理記號是說明用戶端授權許可權的數位簽署實體，例如存取記號。重新整理記號可以用來取得相同範圍的新存取記號。授與特定範圍的用戶端授權要求並鑑別用戶端之後，授權伺服器的記號端點會將包含所要求存取記號及重新整理記號的 HTTP 回應傳送給用戶端。存取記號到期時，用戶端會將重新整理記號傳送至授權伺服器的記號端點，以取得一組新的存取記號及重新整理記號。

#### 重新整理記號的結構
{: #structure_refresh_tokens}

與 MobileFirst 存取記號類似，MobileFirst 重新整理記號包含下列資訊：

* **用戶端 ID**：用戶端的唯一 ID。
* **範圍**：已授與記號的範圍（請參閱 OAuth 範圍）。此範圍未包含必要的應用程式範圍。
* **記號有效期限**：記號變成無效（到期）的時間（以秒為單位）。

##### 記號有效期限
{: #str-token-expiration}

重新整理記號的記號有效期限會比一般存取記號有效期限還要長。在超過所授與重新整理記號的有效期限之前，重新整理記號都會保持有效。在此有效期限內，用戶端可以使用重新整理記號，以取得一組新的存取記號及重新整理記號。重新整理記號具有固定有效期限 30 天。每次用戶端順利接收到一組新的存取記號及重新整理記號時，都會重設重新整理記號期限，因此提供給用戶端永不到期記號的經驗。存取記號期限規則會與**存取記號**小節中所述相同。

##### 啟用重新整理記號特性
{: #acs_enable-refresh-token}

分別在用戶端及伺服器端上使用下列內容，即可啟用重新整理記號特性。

用戶端內容 (Android)：
*檔名*：mfpclient.properties
*內容名稱*：wlEnableRefreshToken
*內容值*：true
例如，
*wlEnableRefreshToken*=true

用戶端內容 (iOS)：
*檔名*：mfpclient.plist
*內容名稱*：wlEnableRefreshToken
*內容值*：true
例如，
*wlEnableRefreshToken*=true

伺服器端內容：
*檔名*：server.xml
*內容名稱*：mfp.security.refreshtoken.enabled.apps
*內容值*：應用程式組合 ID，用 ";" 分隔

例如，

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
將不同的組合 ID 用於不同的平台。

##### 重新整理記號回應結構
{: #refresh-token-response-structure}

以下是來自授權伺服器的有效重新整理記號回應範例：

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

除了說明為存取記號回應結構一部分的其他內容物件之外，重新整理記號回應還具有其他內容物件 `refresh_token`。

**附註**：相較於存取記號，重新整理記號會長期存在。因此，使用重新整理記號特性時請小心。不需要定期使用者鑑別的應用程式是用於使用重新整理記號特性的理想候選項目。

MobileFirst 在從 CD Update 3 開始的 iOS 上支援重新整理記號特性。

#### 安全檢查
{: #acs_securitychecks}

安全檢查是一種伺服器端實體，可實作用於保護伺服器端應用程式資源的安全邏輯。安全檢查的一個簡單範例就是使用者登入安全檢查，可接收使用者認證，並針對使用者登錄來驗證認證。另一個範例是預先定義的 MobileFirst 應用程式確實性安全檢查，可驗證行動應用程式確實性，因此防止發生不正當地嘗試存取應用程式的資源。相同的安全檢查也可以用來保護數個資源。

安全檢查一般會發出安全盤查，而安全盤查需要用戶端以特定方式回應才能通過檢查。此信號交換會在 OAuth 存取記號取得流程期間發生。用戶端使用**盤查處理程式**，以處理來自安全檢查的盤查。

##### 內建安全檢查
{: #builtin-sec-checks}

以下是可用的預先定義安全檢查：

* 應用程式確實性
* LTPA 型單一登入 (SSO)
* 直接更新

#### 盤查處理程式實體
{: #challengehandler_entity}

嘗試存取受保護的資源時，用戶端可能會面對盤查。盤查是一個問題、安全測試、由伺服器發出以確定容許用戶端存取此資源的提示。此盤查最常見的是認證的要求，例如使用者名稱及密碼。

盤查處理程式是一種用戶端實體，可實作用戶端安全邏輯及相關使用者互動。

**重要事項**：接收到盤查之後，即無法予以忽略。您必須回答或取消它。忽略盤查可能會導致非預期的行為。

### 範圍
{: #scopes}

您可以指派資源的範圍來保護資源（例如配接器）不受未獲授權的存取。

範圍定義為一個以上以空格區隔之範圍元素 ("scopeElement1 scopeElement2...") 的字串，或定義為空值以套用預設範圍 (RegisteredClient)。除非您停用資源的資源保護，否則 MobileFirst 安全架構需要有任何配接器資源的存取記號，即使資源未獲指派範圍也是一樣。

#### 範圍元素
{: #scopeelements}

範圍元素可以是下列其中一項：

* 安全檢查的名稱。
* 任意關鍵字（例如 `access-restricted` 或 `deletePrivilege`），以定義此資源所需的安全等級。此關鍵字稍後會對映至安全檢查。

#### 範圍對映
{: #scopemapping}

依預設，您在**範圍**中寫入的**範圍元素**會對映至同名的**安全檢查**。比方說，如果您寫入稱為 `PinCodeAttempts` 的安全檢查，則可以在範圍內使用同名的範圍元素。

「範圍對映」容許將範圍元素對映至安全檢查。用戶端要求範圍元素時，此配置會定義需要套用的安全檢查。例如，您可以將範圍元素 `access-restricted` 對映至 `PinCodeAttempts` 安全檢查。

如果您要根據嘗試存取資源的應用程式以不同的方式保護資源，則範圍對映十分有用。您也可以將範圍對映至零個以上的安全檢查清單。

例如：範圍 = `access-restricted deletePrivilege`

* 在應用程式 A 中
    * `access-restricted` 對映至 `PinCodeAttempts`。
    * `deletePrivilege` 對映至空字串。
* 在應用程式 B 中
    * `access-restricted` 對映至 `PinCodeAttempts`。
    * `deletePrivilege` 對映至 `UserLogin`。

若要將範圍元素對映至空字串，請不要在**新增範圍元素對映**蹦現功能表中選取任何安全檢查。

![範圍對映](/images/scope_mapping.png "新增新的範圍元素對映畫面")

您也可以手動編輯具有必要配置的應用程式配置 JSON 檔案，並將變更推送回 MobileFirst Server。

1. 從**指令行視窗**中，導覽至專案的根資料夾，然後執行 `mfpdev app pull`。
2. 開啟配置檔，其位於 `[project-folder]\mobilefirst` 資料夾中。
3. 定義 `scopeElementMapping` 內容，以編輯檔案。在此內容中，定義各包含所選取範圍元素名稱的資料配對，以及元素所對映的零個以上以空格區隔之安全檢查的字串。例如：

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. 執行下列指令，來部署已更新的配置 JSON 檔案：
  ```bash
mfpdev app push
```

您也可以將已更新的配置推送至遠端伺服器。請參閱「使用 MobileFirst CLI 管理 MobileFirst 構件」指導教學。
{: note}

### 保護資源
{: #protecting-resources}

在 OAuth 模型中，受保護的資源是需要存取記號的資源。您可以使用 MobileFirst 安全架構來保護 MobileFirst Server 實例上所管理的兩個資源，以及外部伺服器上的資源。您可以藉由指派範圍，定義用於存取資源的必要許可權來保護資源。

您可以使用各種方式來保護資源：

#### 必要的應用程式範圍
{: #mandatoryappscope}

在應用程式層次，您可以定義將套用至應用程式所使用之所有資源的範圍。除了所要求資源範圍的安全檢查之外，安全架構還會執行這些檢查（如果存在的話）。

**附註**：
   * 存取未受保護的資源時，不會套用必要的應用程式範圍。
   * 已針對資源範圍授與的存取記號未包含必要的應用程式範圍。

在「MobileFirst 作業主控台」中，從導覽資訊看板的**應用程式**區段中選取您的應用程式，然後選取**安全**標籤。在**必要的應用程式範圍**下，選取**新增至範圍**。

![必要的應用程式範圍](/images/mandatory-application-scope.png "配置必要的應用程式範圍畫面")

您也可以手動編輯具有必要配置的應用程式配置 JSON 檔案，並將變更推送回 MobileFirst Server。

1. 從**指令行視窗**中，導覽至專案的根資料夾，然後執行 `mfpdev app pull`。
2. 開啟配置檔，其位於 **project-folder\mobilefirst** 資料夾中。
3. 編輯檔案，方法是定義 `mandatoryScope` 內容，以及將內容值設為包含以空格區隔的所選取範圍元素清單的範圍字串。例如，

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. 執行下列指令，來部署已更新的配置 JSON 檔案：mfpdev app push。

您也可以將已更新的配置推送至遠端伺服器。

#### 保護配接器資源
{: #protectadapterres}

在您的配接器中，您可以針對 Java 方法或 JavaScript 資源程序或是整個 Java 資源類別指定保護範圍。範圍定義為一個以上以空格區隔之範圍元素 ("scopeElement1 scopeElement2...") 的字串，或定義為空值以套用預設範圍。如需保護配接器資源的詳細資料，請參閱[保護配接器](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters)。

### 停用資源保護
{: #disablingresprotection}

您可以針對特定 Java 或 JavaScript 配接器資源或是針對整個 Java 類別，停用預設 MobileFirst 資源保護，如下列 Java 及 JavaScript 小節所述。停用資源保護時，MobileFirst 安全架構不需要記號，即可存取資源。

#### 停用 Java 資源 OAuth 保護
{: #disablejavaresoauthprotection}

若要整個停用 Java 資源方法或類別的 OAuth 保護，請將 `@OAuthSecurity` 註釋新增至資源或類別宣告，以及將 `enabled` 元素的值設為 `false`：

```
@OAuthSecurity(enabled = false)
```

註釋之 `enabled` 元素的預設值為 `true`。將 `enabled` 元素設為 `false` 時，會忽略 `scope` 元素，而且會解除保護資源或資源類別。

**附註**：當您將範圍指派給未受保護類別的方法時，儘管有此類別註釋，還是會保護該方法，除非您將資源註釋的 `enabled` 元素設為 `false`。

請檢閱下列範例：

下列程式碼會停用 `helloUser` 方法的資源保護：

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

下列程式碼會停用 `MyUnprotectedResources` 類別的資源保護：

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### 停用 JavaScript 資源 OAuth 保護
{: #disablejavascriptresoauthprotection}

若要整個停用 JavaScript 配接器資源（程序）的 OAuth 保護，請在 **adapter.xml** 檔案中，將 <procedure> 元素的 `secured` 屬性設為 `false`：

```javascript
<procedure name="procedureName" secured="false">
```

將 `secured` 屬性設為 `false` 時，會忽略 `scope` 屬性，而且會解除保護資源。

請檢閱下列範例：

下列程式碼會停用 `userName` 程序的資源保護：

```javascript
<procedure name="userName" secured="false">
```

### 定義未受保護的資源
{: #defunprotectedresources}

未受保護的資源是不需要存取記號的資源。MobileFirst 安全架構不會管理對未受保護資源的存取權，而且不會驗證或檢查可存取這些資源之用戶端的身分。因此，不支援未受保護資源的「直接更新」、封鎖裝置存取或遠端停用應用程式這類特性。

### 保護外部資源
{: #protecextresources}

若要保護外部資源，您可以將具有存取記號驗證模組的資源過濾器新增至外部資源伺服器。記號驗證模組會先使用安全架構之授權伺服器的內部檢查端點來驗證 MobileFirst 存取記號，再將 OAuth 用戶端存取權授與給資源。您可以使用 MobileFirst 運行環境的 MobileFirst REST API，針對任何外部伺服器建立您自己的存取記號驗證模組。或者，使用其中一個提供的 MobileFirst 延伸規格來保護外部 Java 資源。
