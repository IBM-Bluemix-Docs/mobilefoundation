---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: getting started, mobile foundation, plans, configure mobile foundation server, sample app, setup

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# 入門指導教學
{: #getting-started}

{{site.data.keyword.mobilefoundation_long}} 可加速設定 {{site.data.keyword.mfp_full}} 環境，您可以使用該環境來開發、測試及執行企業行動應用程式。{{site.data.keyword.mobilefoundation_short}} 提供下列不同的服務方案：
* **Developer**：在使用者帳戶中佈建 Foundation Server 實例。允許任何數目的應用程式，而所有應用程式之間的連接裝置總數限制為 10。免費，且僅供用於開發及測試用途。
* **Professional Per Device**：在使用者帳戶中佈建 Foundation Server 實例，並依積極連接的裝置數收費。
* **Professional 1 Application**：在使用者帳戶中佈建 Foundation Server 實例，並允許任意數目的使用者和裝置積極地連接（僅限於單一應用程式）。    
{: shortdesc}

您可以在[這裡](https://cloud.ibm.com/catalog/services/mobile-foundation)檢閱所有可用的方案。
{: note}

遵循本入門指導教學，建立使用其中一個支援方案的 {{site.data.keyword.mobilefoundation_short}} 服務實例。然後，您可以登錄應用程式。下載並編輯已登錄的應用程式、部署配接器，最後測試應用程式。

## 開始之前
{: #prereqs-gs}

您需要有 {{site.data.keyword.Bluemix}} 帳戶及 {{site.data.keyword.mobilefoundation_short}} 服務實例。

## 步驟 1：建立 {{site.data.keyword.mobilefoundation_short}} 服務實例
{: #step1create}

1. 在 {{site.data.keyword.Bluemix_notm}} ** 型錄**中，選取 [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation)。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 選擇您要建立服務實例的地區、組織及空間。
4. 選取**定價方案**，然後按一下**建立**。

## 步驟 2：建置行動通道
{: #buildmobilechannel}


### 針對 {{site.data.keyword.mobilefoundation_short}}: Developer 方案
{: #buildchanneldevplan}

建立 {{site.data.keyword.mobilefoundation_short}}: Developer 的實例之後，您可以完成下列步驟來開始建置行動通道。

* 您可以立即存取與使用 Mobile Foundation Server。

  此選項會使用下列設定來建立 {{site.data.keyword.mfserver_long_notm}}：
  *	1 GB 的記憶體。此大小足以進行開發及輕量型測試活動使用。

  * 使用 CLI 存取 Mobile Foundation Server 時需要認證，您可以從 IBM Cloud 主控台的導覽窗格按一下**服務認證**來取得認證。

### 針對 {{site.data.keyword.mobilefoundation_short}}：Professional Per Device 方案
{: #buildchannelprofdeviceplan}

在建立 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 服務的實例之後，您可以完成下列步驟來開始建置行動通道。

  1.  連接至 {{site.data.keyword.Bluemix_notm}} 上的現有 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務。

      1.  選取 {{site.data.keyword.Bluemix_notm}} `Organization`，其中存在 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

      + 從所選取 `Organization` 的可用空間清單中選取 {{site.data.keyword.Bluemix_notm}} `Space`，其中存在 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

      + 選取 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} `Service Name` 及 `Credentials`，以連接至現有 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

      + 按一下**測試連線**，以測試與所選取 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例的連線。

      + 按一下**新增**，接著在要求確認所選取 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務的蹦現視窗上按一下**繼續**。此動作會在配置的 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 資料庫服務實例中建立必要的表格。

      將 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 連線新增至 {{site.data.keyword.mobilefoundation_short}} 實例之後，即無法進行變更。
      {: note}
  2.  建立及啟動伺服器。

      1. 使用預設配置建立 {{site.data.keyword.mobilefirst_notm}} 伺服器實例，然後按一下**啟動基本伺服器**。

      + 此選項會使用下列設定佈建 {{site.data.keyword.mfserver_long_notm}}：
          - 兩個節點，各具有 1 GB 記憶體。此大小適合進行開發、中等測試活動及小規模正式作業工作負載。

          -	自動產生 `username` 及 `password`。您可以在伺服器啟動並執行時存取它們。

          會啟動伺服器的建立處理程序。此處理程序大約需要 10 分鐘的時間，並且會出現一個訊息視窗，指出此作業的進度。完成時，會顯示一個儀表板，您可以在其中查看：
            -	執行中伺服器的狀態（狀態、大小）。
            -	為您建立伺服器路徑。您可以使用行動應用程式中的這個路徑來連接至 {{site.data.keyword.mfserver_short_notm}}。
            -	用來存取 {{site.data.keyword.mfp_oc_short_notm}} 的 `username` 和 `password`。會隱藏 `password`。按一下**顯示密碼**圖示，即可進行視覺化。

      +	按一下**啟動主控台**，以啟動 {{site.data.keyword.mfp_oc_short_notm}}。      

      若要使用拓蹼、安全及其他伺服器配置的進階配置來建立 {{site.data.keyword.mobilefirst_notm}} 伺服器實例，請按一下**使用進階配置啟動伺服器**。如需相關資訊，請參閱[設定進階配置](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#using_mfs_advanced_p5)。
      {: tip}

### 針對 {{site.data.keyword.mobilefoundation_short}}：Professional 1 Application 方案
{: #buildchannelprof1appplan}

建立 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服務的實例之後，只要完成下列步驟，就可以開始建置行動通道。

  1.  連接至 {{site.data.keyword.Bluemix_notm}} 上的現有 {{site.data.keyword.Db2_on_Cloud_long}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL_full}} 服務。

      1.  選取 {{site.data.keyword.Bluemix_notm}} `Organization`，其中存在 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

      + 從所選取 `Organization` 的可用空間清單中選取 {{site.data.keyword.Bluemix_notm}} `Space`，其中存在 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

      + 選取 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} `Service Name` 及 `Credentials`，以連接至現有 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例。

      + 按一下**測試連線**，以測試與所選取 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 服務實例的連線。

      + 按一下**新增**，接著在要求確認所選取 {{site.data.keyword.Db2_on_Cloud_short}} 或 {{site.data.keyword.composeForPostgreSQL}} 服務的蹦現視窗上按一下**繼續**。此動作會在配置的 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 資料庫服務實例中建立必要的表格。

      將 {{site.data.keyword.Db2_on_Cloud_short}}（**精簡**方案以外的任何方案）或 {{site.data.keyword.composeForPostgreSQL}} 連線新增至 {{site.data.keyword.mobilefoundation_short}} 實例之後，即無法進行變更。
      {: note}

  2.  建立及啟動伺服器。

      1. 使用預設配置建立 {{site.data.keyword.mobilefirst_notm}} 伺服器實例，然後按一下**啟動基本伺服器**。

        `基本伺服器實例包括單一節點及 1 GB 的記憶體。`

      + 自動產生 `username` 及 `password`。您可以在伺服器啟動並執行時存取它們。  

        會啟動伺服器的建立處理程序。此處理程序大約需要 10 分鐘的時間，並且會出現一個訊息視窗，指出此作業的進度。完成時，會顯示一個儀表板，您可以在其中查看：
          -	執行中伺服器的狀態（狀態、大小）。
          -	為您建立伺服器路徑。您可以使用行動應用程式中的這個路徑來連接至 {{site.data.keyword.mfserver_short_notm}}。
          -	用來存取 {{site.data.keyword.mfp_oc_short_notm}} 的 `username` 和 `password`。會隱藏 `password`。按一下**顯示密碼**圖示，即可進行視覺化。

      +  按一下**啟動主控台**，以啟動 {{site.data.keyword.mfp_oc_short_notm}}。  

      若要使用拓蹼、安全及其他伺服器配置的進階配置來建立 {{site.data.keyword.mobilefirst_notm}} 伺服器實例，請按一下**使用進階配置啟動伺服器**。如需相關資訊，請參閱[設定進階配置](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#using_mfs_advanced_p2)。
      {: tip}

移至[使用 Mobile Foundation 服務來設定 MobileFirst Server ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/){: new_window}，以進一步瞭解開始使用 {{site.data.keyword.mobilefoundation_short}}。
{: note}

## 步驟 3：在 {{site.data.keyword.mobilefoundation_short}} 中登錄應用程式
{: #registerapp}

建立及啟動 Mobile Foundation 伺服器實例之後，即可執行下列步驟來登錄 Android 應用程式。

  1.  透過載入 URL `http://<your-server-host>:<server-port>/mfpconsole` 來呼叫 {{site.data.keyword.mfp_oc_short_notm}}。使用在佈建時所產生的 `username` 及 `password`。

  + 在 {{site.data.keyword.mfp_oc_short_notm}} **儀表板**中，按一下**應用程式**旁邊的**新建**。

  + 提供 *MFPStarterAndroid* 作為**應用程式名稱**。

  + 將**平台**選擇為 *Android*。

  + 提供 *com.ibm.mfpstarterandroid* 作為**應用程式 ID**。

  + 輸入 *1.0* 作為**版本**。

  + 按一下**登錄應用程式**。

## 步驟 4：下載範例應用程式
{: #downloadapp}

  1.  從 {{site.data.keyword.mfp_oc_short_notm}} **儀表板**中，選取**應用程式**下的 **MFPStarterAndroid**。

  + 按一下**取得入門範本程式碼**，然後選取以下載 Android 應用程式範例。

## 步驟 5：編輯範例應用程式
{: #editapp}

  1. 將稍早步驟中的已下載範例 Android 應用程式匯入至 Android Studio。

  + 從 Android Studio 的**專案**資訊看板功能表中，選取 **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java** 檔案。

    * 新增下列匯入項目
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * 貼上下列程式碼 Snippet，並取代 `WLAuthorizationManager.getInstance().obtainAccessToken` 呼叫

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Will print "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## 步驟 6：部署配接器
{: #deployadapter}

  1. 下載此[配接器構件](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download}，並使用**動作 → 部署配接器**從 {{site.data.keyword.mfp_oc_short_notm}} 進行部署。

## 步驟 7：測試應用程式
{: #testapp}

  1. 在 Android Studio 中，從**專案**資訊看板功能表中，選取 **app → src → main →assets → mfpclient.properties** 檔案，然後使用正確的 MobileFirst Server 值來編輯 `protocol`、`host` 及 `port` 內容。

   值一般是 https、*your-server-address* 及 443。
   {: tip}

  2. 按一下 Android Studio 中的**執行應用程式**。
     * 您會看到在裝置模擬器上啟動的應用程式。
     * 按一下應用程式中的 **Ping MobileFirst Server**，這會顯示 `Connected to MobileFirst Server`。
     * 如果應用程式能夠連接至 MobileFirst Server，所部署的 Java 配接器會進行資源要求呼叫。
     * 配接器回應接著會列印在 Android Studio 的 LogCat 視圖中。


## 後續步驟
{: #nextsteps-gs}

您可以遵循[快速入門指導教學 ![外部鏈結圖示](../../icons/launch-glyph.svg "快速入門指導教學")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} 來使用其他範例應用程式，以及探索 {{site.data.keyword.mobilefoundation_short}} 的運作。

「快速入門」的指導教學說明適用於 iOS、Android、Web、Cordova、Windows、React Native、Ionic 及 Xamarin 應用程式之 {{site.data.keyword.mobilefoundation_short}} 的運作。
