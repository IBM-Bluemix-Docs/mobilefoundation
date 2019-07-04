---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-24"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

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

# 使用 {{ site.data.keyword.cos_full_notm }} 與 {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} 搭配
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) 提供企業級功能，專門設計用來支援建置及部署下一代的認知、環境定義及個人化行動應用程式。{{ site.data.keyword.cos_full_notm }} (COS) 是彈性、具成本效益且可擴充的雲端儲存空間，用於未結構化資料。此操作手冊說明使用 {{ site.data.keyword.mobilefoundation_short}} 的行動應用程式如何透過 Ionic 應用程式，來連接及提取資料，或將資料上傳至 {{ site.data.keyword.cos_full_notm }}。[這裡](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)提供了此操作指導教學的 Ionic 應用程式、配接器及相關檔案。
{: shortdesc}


## 必要條件
{: #cos-prerequisites}

1. 執行 `npm install -g mfpdev-cli` 來安裝 [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html)。此 CLI 可用來登錄 Ionic 應用程式，並將配接器部署至 MF 伺服器。或者，您可以從 MF 伺服器儀表板執行這些活動。

2. 在您的機器上安裝 [{{ site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli)。

3. 執行 `npm install -g ionic` 來安裝 Ionic CLI

4. 執行 `npm install -g cordova` 來安裝 Cordova


## {{ site.data.keyword.mobilefoundation_short}} 伺服器設定
{: #mobile-foundation-server-setup}

{{ site.data.keyword.mobilefoundation_short}} 伺服器是設定在 {{ site.data.keyword.cloud_notm}} 上。請如下所示設定 {{ site.data.keyword.mobilefoundation_short}} 伺服器的 {{ site.data.keyword.cloud_notm}} 實例。

* 在 {{ site.data.keyword.cloud_notm}} 型錄中，搜尋 *{{ site.data.keyword.mobilefoundation_short}}*。按一下 **{{ site.data.keyword.mobilefoundation_short}}** 磚。

    ![MFPCatalog](images/mfp_catalog.png)

* 提供 {{ site.data.keyword.mobilefoundation_short}} 伺服器實例的合適名稱，然後按一下**建立**。

    ![BMMFPNew](images/bmmfpnew.png)

* 會建立新的 MF 伺服器實例，並要求登入認證。

    ![MFPLogin](images/mfp_login.png)

* 用於登入到 MF 伺服器的認證可以在功能表中的**認證**標籤中找到。

    ![MFPcredentials](images/mfp_credentials.png)

* 提供這些認證並登入，以進入 MF 儀表板。

    ![MFPDashboard](images/mfp_dashboard.png)


## Cloud Object Storage 設定
{: #cloud-object-storage-setup}

* 在 {{ site.data.keyword.cloud_notm}} 型錄中，搜尋 *Cloud Object Storage*。按一下 **Object Storage** 磚。

    ![型錄](images/catalog.png)

* 提供 COS 實例的名稱，然後按一下**建立**。

    ![建立 COS](images/cos_create.png)

* 接下來，按一下功能表選項中的**儲存區**。為您的儲存區提供合適的名稱（在此範例中，我們選擇將儲存區命名為 `sharedgallery`），然後按一下**建立**。

    ![建立儲存區](images/bucketcreate.png)

* 建立儲存區之後，儲存區的登陸頁面類似於下列影像，為您提供直接上傳資料的選項。

    ![儲存區登陸頁面](images/bucketlanding.png)

* 在這裡，新增[範例](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)之**簡短案例**資料夾中提供的三個檔案。


## MFP-COS Ionic 應用程式及 Java 配接器
{: #mfp-cos-ionic-app-and-java-adapter}

下載這個 [Git 儲存庫](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)或複製它。此儲存庫由兩個主要元件組成：

1. MF Java 配接器
2. Ionic 行動應用程式

### 配置 mfpdev-cli
{: #configuring-mfpdev-cli}

將伺服器詳細資料新增至 CLI。在命令提示字元上執行 `mfpdev server add`。

```
? Enter the name of the new server profile:
```

提供伺服器的名稱，然後按 Enter 鍵。在此範例中，提供的名稱是 `mfpserver`。

```
? Enter the fully qualified URL of this server:
```

輸入伺服器的 URL。對於 {{ site.data.keyword.cloud_notm}} 上的 MF 伺服器，服務認證標籤包含 URL。{{ site.data.keyword.cloud_notm}} 上 https MF 伺服器的埠是 443，而 http MF 伺服器實例的埠則為 80。

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

輸入 `admin`，然後按 **Enter** 鍵。

```
? Enter the MobileFirst Server administrator password:
```

輸入服務認證中提供的密碼。

```
? Save the administrator password for this server?: (Y/n)
```

根據您的喜好設定，輸入 *Y/N*，然後輸入提示詢問的詳細資料。

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

設定 30 秒作為預設逾時值

```
? Make this server the default?: (Y/n)
```

輸入 *Y*，然後按 **Enter** 鍵。

***預期的輸出***：

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### 配置 MF Java 配接器
{: #configuring-the-mf-java-adapter}

若要連接至 COS 實例，您必須在 `adapter.xml` 檔案中提供 COS 實例的部分詳細資料。請提供下列欄位的值：

1. **endpointURL**：此欄位是 COS 物件的公用端點 URL。此 URL 可在 COS 的儀表板上找到，其位於**儲存區（在功能表選項上）-> <your-bucket-name>（在此範例中為 `sharedgallery`）-> 配置 -> 端點 -> 公用**下
2. **AuthToken**：在此指導教學中，我們使用的是 IAM 鑑別。

若要讓 Java 配接器連接至您的 COS 實例，需要使用 IAM 或 HMAC 的鑑別。以下是取得 IAM 記號的步驟。如需 IAM 及 HMAC 鑑別處理程序的進一步詳細資料，請按一下[這裡](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions)。

#### 使用 {{ site.data.keyword.cloud_notm}} CLI 取得 IAM Oauth 記號
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. 首先，確定您有 API 金鑰。請從 [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users) 取得 API 金鑰。
2. 使用 CLI 登入 {{ site.data.keyword.cloud_notm}} Platform。

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  您的輸出類似下列 Snippet：

	```text
	Authenticating...
	OK

	Targeted account <account-name> (<account-id>)

	Targeted resource group default

	API endpoint:     https://api.us-south.cf.cloud.ibm.com (API version: 	2.128.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. 若要取得 {{ site.data.keyword.cloud_notm}} 帳戶上的所有服務實例，請在 CLI 上執行下列指令。

  ```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	預期的輸出：

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. 執行下列指令來取得 COS 實例的詳細資料。<instance-name> 是 COS 服務的名稱（在此指導教學中為 `newObject`）。

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	預期的輸出：

	```text
	Retrieving service instance <sinstance-name> in resource group 	Default under account <account-name> as<email-address>...
	OK

	Name:                  <resource-instance-name>
	ID:                    <resource-instance-id>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. 若要取得 IAM 記號，請執行下列指令：
  ```bash
	ibmcloud iam oauth-tokens
  ```

	預期的輸出：

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

在您新增 *endpointURL* 及 *authToken* 值之後，請建置配接器。在命令提示字元中導覽至配接器的根資料夾並執行


```bash
mfpdev adapter build
```
{: codeblock}

`*.adapter` 檔案會建立在 `target` 資料夾中。請執行下列指令：

```bash
mfpdev adapter deploy
```
{: codeblock}

配接器會部署到 MF 實例。

或者，也可以在 MF 伺服器儀表板上部署該配接器。開啟 MF 伺服器儀表板，在功能表上，按一下**配接器 -> 新建**以開啟下列影像所顯示的頁面。

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

接著，按一下**部署配接器**，並上傳 **target** 資料夾中的 `.adapter` 檔案。


### 配置e Ionic 應用程式
{: #configuring-the-ionic-app}

在應用程式中執行下列步驟：

1. 導覽至包含 Ionic 應用程式的資料夾。

2. 新增 Cordova MF 外掛程式

  ```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. 新增 Android 或 iOS 平台

  ```bash
ionic cordova platform add android
  ```
	或

	```bash
	ionic cordova platform add ios
	```

4. 執行下列指令，將應用程式登錄至 MF 伺服器：

	```bash
	mfpdev app register
```

	或者，也可以在 MF 伺服器儀表板上登錄該應用程式。請開啟 MF 伺服器儀表板，然後在功能表上，按一下**應用程式 -> 新建**。

	即會載入下列影像所顯示的頁面。

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	輸入所要求的詳細資料。在「應用程式名稱」文字框中提供應用程式的名稱。選擇所需的平台。

	若為 Android，**套件**文字框會接受*應用程式 ID*。此參數可在作為 Android 應用程式套件一部分的 `AndroidManifest.xml` 中找到。**版本**文字框欄位則必須填入來自 `AndroidManifest.xml` 的 *versionName* 值。

	若為 iOS，**組合 ID** 文字框會接受*應用程式 ID*（區分大小寫）。此參數可在 iOS 應用程式的 `mfpclient.plist` 中找到。**版本**文字框欄位則必須填入來自 iOS 應用程式 `mfpclient.plist` 的 *version* 值。

5. 執行 `ionic cordova prepare`，讓變更過濾到新增的環境。
6. 執行
  ```bash
	ionic cordova build android
  ```
	或
  ```bash
	ionic cordova build ios
  ```
	以確保 typescript 變更會新增至環境。

7. 連接裝置或執行模擬器，並執行下列指令。

	```bash
  ionic cordova run android
	```
	或
	```bash
	ionic cordova run ios
	```

### 導覽 Ionic 應用程式
{: #navigating-the-ionic-application}

Ionic 應用程式會顯示先前從您建立的 COS 實例中上傳的簡短案例清單（如 [Cloud Object Storage 設定](#cloud-object-storage-setup)一節中的最後一個步驟）。在選取特定案例選項時，會載入並顯示所選取的案例。此外，還會提供新增案例的選項，然後將該案例上傳至 COS 實例。

起始 COS 物件清單看起來類似下列影像。

![之前的 COS](images/cos_before.png)

該應用程式的首頁提供了*取得所有案例*或*新增案例*選項

![應用程式首頁畫面](images/app-home-screen.png)

按一下**取得所有案例**時，即會顯示 COS 上可用的案例。

![應用程式選取案例](images/app-select-story.png)

從下拉功能表中選取要載入的案例，然後按一下**載入**

![應用程式載入的案例](images/app-story-loaded.png)

接著，按一下**新增案例**按鈕，以新增您自己的案例。在文字區中輸入案例及案例內容的標題，然後按一下**新增**。

![應用程式新增輸入](images/app-add-input.png)

新增案例之後，會顯示順利新增案例的訊息。

![應用程式新增的案例](images/app-story-added.png)

COS 儀表板現在也會有從應用程式新增的案例。

![新增的 COS](images/cos_added.png)


使用 `ibmcloud iam oauth-tokens` 取得的 IAM OAuth 記號具有有效期限，且配接器失敗，並出現 ``403 - Forbidden` 的異常狀況。因此，它必須確保記號在部署的配接器上是有效的，應用程式才能如預期般運作。
{: note}
