---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 使用 Secure Gateway 服務建立與內部部署系統的安全連線
{: #integrate_secure_gateway}

當您建置企業行動應用程式時，通常會想要整合應用程式與現有記錄系統。若要從行動應用程式存取及利用內部部署資料中心內所儲存的資料，您可以搭配使用 [Mobile Foundation 服務](https://cloud.ibm.com/catalog/services/mobile-foundation)與 [IBM Cloud](https://cloud.ibm.com/) 中的 [Secure Gateway 服務](https://cloud.ibm.com/catalog/services/secure-gateway)。Secure Gateway 服務提供快速、簡單且安全的連線功能，並建立 IBM Cloud 與您要連接至之遠端系統間的通道。

本指導教學說明如何使用 Secure Gateway 服務，從 IBM Cloud 上執行的 Mobile Foundation 配接器存取內部部署資料中心內的 HTTP 端點。

## 必要條件
{: #prereq_int_sec_gw}

若要完成本指導教學，您將需要在企業防火牆內有可公開記錄資料系統的 HTTP 端點。或者，使用[本範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) `Node.js` 專案以在本端環境中建立測試端點。

請導覽至專案資料夾，並執行 HTTP 伺服器。

```bash
npm install
node app.js
```
{: codeblock}

## 使用 Secure Gateway 服務的整合情境
{: #secure_gateway}

下圖說明本指導教學中所述之整合情境內使用的架構。

![架構圖](images/SecureGatewayArchi.png "裝置、雲端服務和內部部署網路的架構圖")

## 實作 Secure Gateway 整合
{: #implementing_sg_integration}

### 建立 Secure Gateway 服務實例
登入 IBM Cloud，並建立 [Secure Gateway 服務](https://cloud.ibm.com/catalog/services/secure-gateway/)實例。

![IBM Cloud](images/SecureGatewayInst.gif "從 IBM Cloud 型錄建立 Secure Gateway 實例")

建立 Secure Gateway 服務實例後，請使用下列步驟在 IBM Cloud 與內部部署環境之間配置 Secure Gateway 服務。

### 新增閘道
{: #add_gateway}

在 Secure Gateway 服務儀表板中，按一下**新增閘道**，以提供任何所需的閘道名稱來建立新的閘道。

![新增閘道](images/AcmeAddGateway.gif "新增閘道使用者介面步驟")


### 新增 Secure Gateway 用戶端
{: #add_sg_client}

![新增用戶端 2](images/AcmeAddClient.gif "新增用戶端使用者介面步驟")

從**用戶端**標籤的新閘道內，按一下**連接用戶端**。

您可以使用您選擇的任何用戶端，並在內部部署環境上執行 Secure Gateway 用戶端。Secure Gateway 主控台中提供設定 Secure Gateway 用戶端的步驟。

在本指導教學中，我們將使用 Docker 容器選項來執行 Secure Gateway 用戶端。
請使用下列步驟：
*   在內部部署機器上安裝 Docker（若尚未安裝）。
*   啟動終端機，並使用服務主控台中所顯示的指令，以在容器上執行 Secure Gateway 用戶端。
    ```bash
    docker run -it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId` 可以在主控台中找到，如先前的影像中所示。

### 新增目的地
{: #add_destination}

![新增目的地](images/AcmeAddDest.gif "新增目的地使用者介面步驟")

從**目的地**標籤的新閘道內，按一下**新增目的地**。

Secure Gateway 服務可讓您從 IBM Cloud 環境連接至內部部署端點，或從內部部署環境連接至雲端資源。在此情境中，請選取內部部署作為資源位置，並提供內部部署資源的主機名稱/IP 及埠。也請提供您選擇的目的地名稱。

若要將要求從 Secure Gateway 用戶端轉遞至資源端點，您需要將資源新增至存取清單。
在 Secure Gateway 用戶端的終端機（在此情境中，於 Docker 運行環境中執行為容器）上執行下列指令，以將資源新增至存取清單。

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` 及 `resourcePort` 指的是內部部署資源端點的詳細資料。

現在已配置目的地。Secure Gateway 服務將移入雲端主機及埠詳細資料，而這些詳細資料可以用來存取雲端環境中的內部部署資源。

![目的地標籤](images/AcmeCloudPopulate.gif "主機和埠詳細資料畫面")

### 使用 Mobile Foundation 及 Mobile Foundation 配接器來配置 Secure Gateway 服務
{: #configuration_sg_mfp}

在本指導教學中，我們將使用 IBM Cloud 上的 Mobile Foundation 服務實例來配置 Mobile Foundation Server。IBM cloud 上的 Mobile Foundation 服務可協助將 Liberty 運行環境上的 Mobile Foundation Server 佈建為 Cloud Foundry 應用程式。Mobile Foundation 服務可讓您採用本端環境上所開發的任何 Mobile Foundation 專案，並在 IBM Cloud 上執行它。

### IBM Cloud 上的 Mobile Foundation Server 設定
{: #mf_server_setup}

從 IBM Cloud 主控台中，建立 [Mobile Foundation 服務](https://cloud.ibm.com/catalog/services/mobile-foundation)實例。

從 Mobile Foundation 服務主控台中，建立 [Mobile Foundation Server ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/)。


### 建置及部署 Mobile Foundation 配接器
{: #deploying_mf_adapter}

在本指導教學中，我們將使用 Mobile Foundation 配接器來連接至 Secure Gateway 端點。請[下載 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) Mobile Foundation JavaHTTP 配接器。

在「Mobile Foundation 作業主控台」中，使用 [mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli) 指令來建置及部署配接器。
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

在[這裡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)，瞭解建置及部署配接器。
{: tip}

提供 JavaHTTP 配接器中資源端點的雲端主機及埠詳細資料（從上節取得）。

![AdapterConfiguration ](images/AdapterConfiguration.png "Java HTTP 配置頁面")

其中 `cap-sg-prd-5.securegateway.appdomain.cloud` 及 `18946` 分別是 Secure Gateway 主機及埠。

現在已配置 Mobile Foundation 配接器，而且現在已啟用 Mobile Foundation 服務，使用 Secure Gateway 服務來與企業內的內部部署系統搭配運作。

### 建立及登錄 Mobile Foundation 範例應用程式
{: #registering_sample_app}

在[這裡](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/)下載 Mobile Foundation 範例應用程式，並遵循 `Readme` 下的指示，然後在「Mobile Foundation 作業主控台」中登錄應用程式。

執行應用程式，並提供要登入的認證，然後按一下*登入* 按鈕。按一下*提取 Acme 寫入程序* 按鈕，以透過 Secure Gateway 使用「Mobile Foundation 作業主控台」中所部署的「JavaHTTP 配接器」來呼叫內部部署端點。接收來自內部部署環境的所需資料。

![應用程式接收內部部署資料](images/AcmePublishersApp.gif "範例應用程式接收資料")

透過在 Secure Gateway 服務上配置多個目的地，以及部署 Mobile Foundation 配接器來連接至端點的個別雲端主機，您可以連接至多個內部部署端點。您也可以配置具有額外安全的 Secure Gateway 服務，確保與端點的通訊是透過 HTTPS 及應用程式端安全所進行。您可以在[這裡](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg)找到詳細資料。


## 摘要
{: #summary_int_sec_gw}

使用本指導教學，您應該可以使用 Secure Gateway 服務，在 IBM Cloud 上執行之 Mobile Foundation 配接器與內部部署 HTTP 端點間建立安全連線。
