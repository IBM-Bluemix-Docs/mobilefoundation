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

# 使用 Secure Gateway 服务建立与内部部署系统的安全连接
{: #integrate_secure_gateway}

构建企业移动应用程序时，通常您会希望将应用程序与现有记录系统相集成。要通过移动应用程序访问和利用内部部署数据中心内存储的数据，可以将 [IBM Cloud](https://cloud.ibm.com/) 上的 [Mobile Foundation 服务](https://cloud.ibm.com/catalog/services/mobile-foundation)和 [Secure Gateway 服务](https://cloud.ibm.com/catalog/services/secure-gateway)配合使用。Secure Gateway 服务提供了快速轻松的安全连接，并在 IBM Cloud 与要连接到的远程系统之间建立隧道。

本教程说明如何使用 Secure Gateway 服务通过在 IBM Cloud 上运行的 Mobile Foundation 适配器访问内部部署数据中心内的 HTTP 端点。

## 先决条件
{: #prereq_int_sec_gw}

要完成本教程，您将需要企业防火墙中用于公开记录数据系统的 HTTP 端点。或者，使用[此样本 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) `Node.js` 项目在本地环境中创建测试端点。

导航至项目文件夹并运行您的 HTTP 服务器。

```bash
npm install
node app.js
```
{: codeblock}

## 针对 Secure Gateway 服务的集成场景
{: #secure_gateway}

下图说明了本教程中描述的集成场景中使用的体系结构。

![体系结构图](images/SecureGatewayArchi.png "设备、云服务和内部部署网络的体系结构图")

## 实现 Secure Gateway 集成
{: #implementing_sg_integration}

### 创建 Secure Gateway 服务实例
登录到 IBM Cloud，然后创建 [Secure Gateway 服务](https://cloud.ibm.com/catalog/services/secure-gateway/)实例。

![IBM Cloud](images/SecureGatewayInst.gif "从 IBM Cloud 目录创建 Secure Gateway 实例")

创建 Secure Gateway 服务实例后，请使用以下步骤在 IBM Cloud 与内部部署环境之间配置 Secure Gateway 服务。

### 添加网关
{: #add_gateway}

在 Secure Gateway 服务仪表板中，单击**添加网关**，以通过提供任何所需的网关名称来创建新网关。

![添加网关](images/AcmeAddGateway.gif "添加网关 UI 步骤")


### 添加 Secure Gateway 客户机
{: #add_sg_client}

![添加客户机 2](images/AcmeAddClient.gif "添加客户机 UI 步骤")

在新网关内的**客户机**选项卡中，单击**连接客户机**。

可以使用所选的任何客户机，然后在内部部署环境中运行 Secure Gateway 客户机。Secure Gateway 控制台中提供了设置 Secure Gateway 客户机的步骤。

在本教程中，我们将使用 Docker 容器选项来运行 Secure Gateway 客户机。请使用以下步骤：
*   如果尚未在内部部署机器上安装 Docker，请进行安装。
*   启动终端，然后使用服务控制台中显示的命令在容器上运行 Secure Gateway 客户机。
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId` 可以在控制台中找到，如先前的图像中所示。

### 添加目标
{: #add_destination}

![添加目标](images/AcmeAddDest.gif "添加目标 UI 步骤")

在新网关内的**目标**选项卡中，单击**添加目标**。

通过 Secure Gateway 服务，可以从 IBM Cloud 环境连接到内部部署端点，或者从内部部署环境连接到云资源。对于此场景，请选择内部部署作为资源位置，并提供内部部署资源的主机名/IP 和端口。此外，请提供您选择的目标名称。

要支持将请求从 Secure Gateway 客户机转发到资源端点，您需要将该资源添加到访问列表。在 Secure Gateway 客户机的终端上运行以下命令（在此场景中，是在 Docker 运行时上作为容器运行），以将该资源添加到访问列表。

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` 和 `resourcePort` 是指内部部署资源端点的详细信息。

现在目标已配置。Secure Gateway 服务将填充云主机和端口详细信息，这些信息可用于从云环境访问内部部署资源。

![目标选项卡](images/AcmeCloudPopulate.gif "主机和端口详细信息屏幕")

### 为 Secure Gateway 服务配置 Mobile Foundation 和 Mobile Foundation 适配器
{: #configuration_sg_mfp}

在本教程中，我们将使用 IBM Cloud 上的 Mobile Foundation 服务实例来配置 Mobile Foundation 服务器。IBM Cloud 上的 Mobile Foundation 服务可帮助在 Liberty 运行时上将 Mobile Foundation 服务器作为 Cloud Foundry 应用程序供应。Mobile Foundation 服务允许您获取在本地环境中开发的任何 Mobile Foundation 项目，并在 IBM Cloud 上运行。

### IBM Cloud 上的 Mobile Foundation 服务器设置
{: #mf_server_setup}

通过 IBM Cloud 控制台创建 [Mobile Foundation 服务](https://cloud.ibm.com/catalog/services/mobile-foundation)实例。

在 Mobile Foundation 服务控制台中，创建 [Mobile Foundation 服务器 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/)。


### 构建和部署 Mobile Foundation 适配器
{: #deploying_mf_adapter}

在本教程中，我们将使用 Mobile Foundation 适配器来连接到 Secure Gateway 端点。[下载 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) Mobile Foundation JavaHTTP 适配器。

在 Mobile Foundation Operations Console 中使用 [mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli) 命令来构建和部署适配器。
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

在[此处 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/) 了解有关构建和部署适配器的信息。
{: tip}

在 JavaHTTP 适配器中提供从上一部分获取的资源端点的云主机和端口详细信息。

![AdapterConfiguration ](images/AdapterConfiguration.png "Java HTTP 配置页面")

其中，`cap-sg-prd-5.securegateway.appdomain.cloud` 和 `18946` 分别是 Secure Gateway 主机和端口。

现在，Mobile Foundation 适配器已配置，并且 Mobile Foundation 服务现在已启用，可以通过 Secure Gateway 服务在企业内使用内部部署系统。

### 创建和注册 Mobile Foundation 样本应用程序
{: #registering_sample_app}

从[此处](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/)下载 Mobile Foundation 样本应用程序，遵循 `Readme` 下的指示信息，并在 Mobile Foundation Operations Console 中注册应用程序。

运行应用程序，提供登录凭证，然后单击*登录*按钮。单击*访存 Acme 写程序*按钮，通过 Secure Gateway 使用在 Mobile Foundation Operations Console 中部署的 JavaHTTP 适配器来调用内部部署端点。从内部部署环境接收所需数据。

![应用程序接收内部部署数据](images/AcmePublishersApp.gif "样本应用程序接收数据")

通过在 Secure Gateway 服务上配置多个目标以及部署 Mobile Foundation 适配器以连接到端点的相应云主机，可以连接到多个内部部署端点。您还可为 Secure Gateway 服务配置其他安全性，以确保与端点的通信通过 HTTPS 和应用程序端安全性执行。您可以在此处找到[详细信息](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg)。


## 摘要
{: #summary_int_sec_gw}

通过使用本教程，您应该能够使用 Secure Gateway 服务在 IBM Cloud 上运行的 Mobile Foundation 适配器与内部部署 HTTP 端点之间建立安全连接。
