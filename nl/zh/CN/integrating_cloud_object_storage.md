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

# 将 {{ site.data.keyword.cos_full_notm }} 与 {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} 一起使用
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) 交付企业级功能，专门设计为支持构建和部署新一代认知、上下文和个性化移动应用程序。{{ site.data.keyword.cos_full_notm }} (COS) 是针对非结构化数据的灵活、经济有效且可缩放的云存储。此操作指南阐述使用 {{ site.data.keyword.mobilefoundation_short}} 的移动应用程序如何通过 ionic 应用程序连接到 {{ site.data.keyword.cos_full_notm }}，并进行数据的访存或上传。本操作指南教程的 ionic 应用程序、适配器和相关文件可从[此处](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)获取。
{: shortdesc}


## 先决条件
{: #cos-prerequisites}

1. 通过运行 `npm install -g mfpdev-cli` 安装 [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html)。此 CLI 用于注册 ionic 应用程序并将适配器部署到 MF 服务器。或者，可从 MF 服务器仪表板执行这些活动。

2. 在机器上安装 [{{ site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli)。

3. 通过执行 `npm install -g ionic` 安装 ionic CLI

4. 通过执行 `npm install -g cordova` 安装 cordova


## {{ site.data.keyword.mobilefoundation_short}} 服务器设置
{: #mobile-foundation-server-setup}

{{ site.data.keyword.mobilefoundation_short}} 服务器在 {{ site.data.keyword.cloud_notm}} 上进行设置。设置 {{ site.data.keyword.mobilefoundation_short}} 服务器的 {{ site.data.keyword.cloud_notm}} 实例，如下所示：

* 在 {{ site.data.keyword.cloud_notm}} 目录中，搜索 *{{ site.data.keyword.mobilefoundation_short}}*。单击 **{{ site.data.keyword.mobilefoundation_short}}** 磁贴。

    ![MFPCatalog](images/mfp_catalog.png)

* 为 {{ site.data.keyword.mobilefoundation_short}} 服务器实例提供合适的名称，然后单击**创建**。

    ![BMMFPNew](images/bmmfpnew.png)

* 新的 MF 服务器实例将进行创建，并请求登录凭证。

    ![MFPLogin](images/mfp_login.png)

* 用于登录到 MF 服务器的凭证可以在菜单中的**凭证**选项卡中找到。

    ![MFPcredentials](images/mfp_credentials.png)

* 提供这些凭证并登录以进入 MF 仪表板。

    ![MFPDashboard](images/mfp_dashboard.png)


## Cloud Object Storage 设置
{: #cloud-object-storage-setup}

* 在 {{ site.data.keyword.cloud_notm}} 目录中，搜索 *Cloud Object Storage*。单击 **Object Storage** 磁贴。

    ![目录](images/catalog.png)

* 为 COS 实例提供名称，然后单击**创建**。

    ![创建 COS](images/cos_create.png)

* 接下来，单击菜单选项中的**存储区**。为存储区提供适合的名称（在此样本中，我们选择将存储区命名为 `sharedgallery`），然后单击**创建**。

    ![创建存储区](images/bucketcreate.png)

* 创建存储区后，存储区的登录页面将类似于下图，为您提供直接上传数据的选项。

    ![存储区登录页面](images/bucketlanding.png)

* 在此处，添加在[样本](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)的 **Short stories** 文件夹中提供的三个文件。


## MFP-COS Ionic 应用程序和 Java 适配器
{: #mfp-cos-ionic-app-and-java-adapter}

下载此 [Git 存储库](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)或进行克隆。此存储库由两个主要组件组成：

1. MF Java 适配器
2. Ionic 移动应用程序

### 配置 mfpdev-cli
{: #configuring-mfpdev-cli}

向 CLI 添加服务器详细信息。在命令提示符中，执行 `mfpdev server add`。

```
? Enter the name of the new server profile:
```

为服务器提供名称，然后按 Enter 键。在此样本中，提供的名称为 `mfpserver`。

```
? Enter the fully qualified URL of this server:
```

输入服务器的 URL。对于 {{ site.data.keyword.cloud_notm}} 上的 MFC 服务器，服务凭证选项卡包含 URL。{{ site.data.keyword.cloud_notm}} 上 HTTPS MF 服务器的端口为 443，HTTP MF 服务器实例的端口为 80。

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

输入 `admin`，然后按 **Enter** 键。

```
? Enter the MobileFirst Server administrator password:
```

输入服务凭证中提供的密码。

```
? Save the administrator password for this server?: (Y/n)
```

根据您的偏好，输入 *Y/N* 并输入提示所询问的详细信息。

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

将 30 秒设置为缺省超时

```
? Make this server the default?: (Y/n)
```

输入 *Y*，然后按 **Enter** 键。

***预期的输出***：

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### 配置 MF Java 适配器
{: #configuring-the-mf-java-adapter}

要连接到 COS 实例，需要在 `adapter.xml` 文件中提供 COS 实例的一些详细信息。提供以下字段的值：

1. **endpointURL**：此字段是 COS 对象的公共端点 URL。可在 COS 仪表板的**存储区（在菜单选项上）-> <您的存储区名称>（在此样本中为 `sharedgallery`）-> 配置 -> 端点 -> 公共**下找到此 URL。
2. **AuthToken**：在本教程中，我们使用 IAM 认证。

要使 Java 适配器能够连接到 COS 实例，需要使用 IAM 或 HMAC 的认证。以下是获取 IAM 令牌的步骤。有关 IAM 和 HMAC 认证过程的进一步详细信息，请单击[此处](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions)。

#### 使用 {{ site.data.keyword.cloud_notm}} CLI 获取 IAM Oauth 令牌
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. 首先，确保您具有 API 密钥。从 [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users) 获取 API 密钥。
2. 使用 CLI 登录到 {{ site.data.keyword.cloud_notm}} 平台。

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  输出类似于以下片段：

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

3. 要获取 {{ site.data.keyword.cloud_notm}} 帐户上的所有服务实例，请在 CLI 上运行以下命令。

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	预期的输出：

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. 执行以下命令以获取 COS 实例的详细信息。<instance-name> 是 COS 服务的名称（对于本教程为 `newObject`）。

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	预期的输出：

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

5. 要获取 IAM 令牌，请执行以下命令：
  ```bash
	ibmcloud iam oauth-tokens
  ```

	预期的输出：

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

添加 *endpointURL* 和 *authToken* 值后，构建适配器。在命令提示符中导航至适配器的根文件夹并执行以下命令：
```bash
mfpdev adapter build
```
{: codeblock}

这将在 `target` 文件夹中创建 `*.adapter` 文件。执行以下命令：

```bash
mfpdev adapter deploy
```
{: codeblock}

这会将适配器部署到 MF 实例。

或者，可以在 MF 服务器仪表板上部署适配器。打开 MF 服务器仪表板，在菜单上，单击**适配器 -> 新建**以打开页面，如下图中所示。

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

然后，单击**部署适配器**，并从 **target** 文件夹上传 `.adapter` 文件。


### 配置 Ionic 应用程序
{: #configuring-the-ionic-app}

在应用程序中，执行以下步骤：

1. 导航至包含 Ionic 应用程序的文件夹。

2. 添加 cordova MF 插件

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. 添加 Android 或 iOS 平台

  ```bash
	ionic cordova platform add android
  ```
	或者

	```bash
	ionic cordova platform add ios
	```

4. 通过执行以下命令将应用程序注册到 MF 服务器

  ```bash
	mfpdev app register
```

	或者，可以在 MF 服务器仪表板上注册应用程序。打开 MF 服务器仪表板，并在菜单上，单击**应用程序 -> 新建**。

	将装入如下图中所示的页面。

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	输入请求的详细信息。在“应用程序名称”文本框中为应用程序提供名称。选择所需的平台。

	对于 Android，**程序包**文本框接受*应用程序标识*。此参数可在 Android 应用程序包 `AndroidManifest.xml` 中找到。**版本**文本框字段必须使用 `AndroidManifest.xml` 中的 *versionName* 值进行填充。对于 iOS，**捆绑软件标识**文本框接受*应用程序标识*（区分大小写）。此参数可在 iOS 应用程序的 `mfpclient.plist` 中找到。**版本**文本框字段必须使用 iOS 应用程序内 `mfpclient.plist` 文件中的 *version* 值进行填充。

5. 执行 `ionic cordova prepare` 以使更改渗透到添加的环境。
6. 执行
  ```bash
	ionic cordova build android
  ```
	或者
  ```bash
	ionic cordova build ios
  ```
	以确保将 typescript 更改添加到环境。

7. 连接设备或者运行仿真器或模拟器，并执行以下命令。

	```bash
  ionic cordova run android
	```
	或者
	```bash
	ionic cordova run ios
  ```

### 在 Ionic 应用程序中浏览
{: #navigating-the-ionic-application}

Ionic 应用程序显示先前从创建的 COS 实例上传的短故事列表（[Cloud Object Storage 设置](#cloud-object-storage-setup)部分中的最后一步）。选择特定故事选项后，将装入并显示所选的故事。还提供了用于添加故事的选项，可将故事上传到 COS 实例。

初始 COS 对象列表类似于下图。

![添加故事之前的 COS](images/cos_before.png)

该应用程序的主页提供了*获取所有故事*或*添加故事*选项

![应用程序主页屏幕](images/app-home-screen.png)

单击**获取所有故事**后，将显示 COS 上已有的故事。

![在应用程序中选择故事](images/app-select-story.png)

在下拉菜单中选择要装入的故事，然后单击**装入**

![在应用程序中已装入故事](images/app-story-loaded.png)

接下来，单击**添加故事**按钮，添加您自己的故事。输入故事的标题，并在文本区域中输入故事的内容，然后单击**添加**。

![在应用程序中添加故事并输入内容](images/app-add-input.png)

添加故事后，将显示一条成功添加故事的消息。

![在应用程序中已添加故事](images/app-story-added.png)

COS 仪表板现在也已包含从应用程序添加的故事。

![在 COS 中已添加故事](images/cos_added.png)


使用 `ibmcloud iam oauth-tokens` 获取的 IAM OAuth 令牌具有到期时间，并且适配器失败，异常为 `403 - Forbidden`。因此，必须确保令牌在已部署的适配器上有效，以使应用程序按期望运行。
{: note}
