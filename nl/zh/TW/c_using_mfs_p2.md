---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation, mobile analytics, professional plan, configure database

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	使用 Professional 1 Application 方案設定
{: #using_mobilefoundation_p2}

使用 Professional 1 Application 方案，使用者可以建立 1 個含有各種行動作業系統的行動應用程式。
建立 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服務實例之後，請閱讀下列程序，以開始使用該服務。

## Professional 1 Application 方案的必要條件
{: #prerequisites_p2}

在配置 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服務實例之前，請考量下列各項。
* 只有 {{site.data.keyword.Db2_on_Cloud_short}} 及 {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}} 方案才支援 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application。

* 您需要有 {{site.data.keyword.Db2_on_Cloud_short}} 或 {{site.data.keyword.composeForPostgreSQL}} 服務實例認證的存取權，才能配置 {{site.data.keyword.mobilefoundation_short}} 服務實例的設定。

{{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例可存在於 {{site.data.keyword.Bluemix_notm}} `Organization` 的任何 `Space` 中或您具有存取權的任何其他 `Organization` 中。請確定您具有存取 {{site.data.keyword.Db2_on_Cloud_short}} 或 {{site.data.keyword.composeForPostgreSQL}} 服務實例所存在的 `Space` 的許可權。
{: note}


## 配置資料庫連線
{: #configure_dashdb_p2}

###  配置的首要步驟
{: #firststeps_p2}

建立 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服務實例之後，請遵循程序以開始使用。

### 設定 Db2 on Cloud 服務實例的連線
{: #connect_dashdb_p2}

建立 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服務實例之後，您會看到*概觀* 頁面。在這裡，您需要指定 {{site.data.keyword.mobilefoundation_short}} 服務實例必須連接至其中之 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例的連線資訊。

如果 Cloud 實例上沒有現有的 Db2，您可以建立新的 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

請遵循下列步驟，建立新的 Db2 服務實例：

1. 在*概觀* 頁面中，選取**建立新服務**區段。

+ 如果您想要高可用的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，請在**高可用性配置**選項上選取`是`。

+ 檢閱方案詳細資料，然後按一下**建立**。

即會建立新的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，其會提供具有 8 GB RAM、2 個 vCPU 及 500 GB 儲存空間的專用 {{site.data.keyword.Db2_on_Cloud_short}} 實例。

請遵循下列步驟，來連接至現有的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例或您建立的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例：

1. 選取 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 {{site.data.keyword.Bluemix_notm}} `Organization`。

+ 從所選取 `Organization` 中可用的空間清單，選取 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 {{site.data.keyword.Bluemix_notm}} `Space`。   
如果您未看到 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 `Organization` 及 `Space`，則請檢查您是否為該 `Organization` 及 `Space` 的成員。您需要具有對組織及空間的*開發人員* 角色存取權。{{site.data.keyword.mobilefoundation_short}} 服務會存取來自 {{site.data.keyword.Db2_on_Cloud_short}} 服務的認證。
{: note}

+ 選取 {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` 和 `Credentials`，以連接至現有的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例。

+  測試所指定的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例的連線。

+  按一下**新增**。此動作會在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 資料庫服務實例中建立必要的表格。

幾秒過後，您就可以存取 `Overview` 頁面，而此頁面提供指導教學及視訊，協助您開始使用 {{site.data.keyword.mobilefoundation_short}} 服務。

您無法變更配置以供 {{site.data.keyword.mobilefoundation_short}} 服務實例使用的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例。不過，您能夠在多個 {{site.data.keyword.mobilefoundation_short}} 服務實例之間使用相同的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，因為每一個 {{site.data.keyword.mobilefoundation_short}} 服務實例都會在所選取的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例中建立自己的綱目。
{: note}

## 啟動使用 Professional 1 Application 方案建立的 MobileFirst Server
{: #start_mobilefoundation_p2}

* 若要以預設值啟動 {{site.data.keyword.mfserver_short_notm}}，請按一下**啟動基本伺服器**。

* 此選項會使用下列設定來建立 {{site.data.keyword.mfserver_long_notm}}：
    -  1 GB 的記憶體。此大小就足以進行開發、中等測試活動及小規模正式作業工作負載。

    -	自動產生 `username` 及 `password`。您可以在伺服器啟動並執行時存取它們。

    會啟動伺服器的建立處理程序。此處理程序大約需要 10 分鐘的時間，並且會出現一個訊息視窗，指出此作業的進度。完成時，會顯示一個儀表板，您可以在其中查看：

      -	執行中伺服器的狀態（狀態、大小）。

      -	為您建立伺服器路徑。您可以使用行動應用程式中的這個路徑來連接至 {{site.data.keyword.mfserver_short_notm}}。

      -	用來存取 {{site.data.keyword.mfp_oc_short_notm}} 的 `username` 和 `password`。會隱藏 `password`。按一下**顯示密碼**圖示，即可進行視覺化。

*	按一下**啟動主控台**，以啟動 {{site.data.keyword.mfp_oc_short_notm}}。

使用這個主控台，您可以管理行動應用程式、配接器及行動裝置，使用伺服器作為行動後端、傳送推送通知，以及執行其他作業。



## 使用 Professional 1 Application 方案時重新建立 MobileFirst Server
{: #recreate_mobilefoundation_p2}

*	按一下**重建**，以重建伺服器。

* 此動作會停止現有的伺服器，並刪除資料。隨即會使用更新的版本（如果有的話）建立新的伺服器實例。這個動作需要數分鐘的時間才能完成。

來自舊版伺服器實例的資料（包括應用程式及配接器的相關資訊）會持續保存在已配置的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例中。這項資料可用來重建您的伺服器。
{: note}

##	在 Professional 1 Application 方案中設定進階配置
{: #using_mfs_advanced_p2}

使用 `Overview` 頁面中的**使用進階配置啟動伺服器**頁面，以使用進階或自訂設定來建立伺服器。您也可以更新伺服器設定來自訂伺服器配置，方法是按一下**配置**標籤。{{site.data.keyword.mobilefoundation_short}} 可讓您存取一些進階設定。

*	從**拓蹼**標籤中，您可以視需要選取伺服器大小和實例數目。預設 1 GB 伺服器就足以進行開發及輕量型測試。
  - 根據您的需求，選取正確的伺服器大小。

  - **實例**會顯示已建立的實例數目。

## Professional 1 Application 方案中的 Mobile Analytics
{: #mobile_analytics_p2}

Mobile Foundation: Developer 方案服務實例已包含並預先配置 Mobile Analytics 伺服器。

* 從 {{site.data.keyword.mfp_oc_short_notm}} 啟動「Mobile Analytics 主控台」。

如需 Mobile Analytics 的相關資訊，請參閱[這裡](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}。
