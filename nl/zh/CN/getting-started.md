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

# 入门教程
{: #getting-started}

{{site.data.keyword.mobilefoundation_long}} 加快设置 {{site.data.keyword.mfp_full}} 环境，您可使用此环境开发、测试和运行企业移动应用程序。{{site.data.keyword.mobilefoundation_short}} 提供以下不同的服务套餐：
* **Developer**：在用户帐户中供应 Foundation Server 的实例。允许任意数量的应用程序，所有应用程序上连接设备的总数限制为 10。免费，只能用于开发和测试目的。
* **Professional Per Device**：在用户帐户中供应 Foundation Server 的实例，并按主动连接的设备数量收费。
* **Professional 1 Application**：在用户帐户中供应 Foundation Server 的实例，并且允许仅针对单个应用程序主动连接任意数量的用户和设备。    
{: shortdesc}

您可以在[此处](https://cloud.ibm.com/catalog/services/mobile-foundation)查看所有可用的套餐。
{: note}

通过遵循此入门教程，创建使用其中一个受支持套餐的 {{site.data.keyword.mobilefoundation_short}} 服务实例。然后，您可以注册应用程序。下载并编辑已注册的应用程序，部署适配器，最后测试应用程序。

## 开始之前
{: #prereqs-gs}

您需要 {{site.data.keyword.Bluemix}} 帐户和 {{site.data.keyword.mobilefoundation_short}} 服务的实例。

## 步骤 1：创建 {{site.data.keyword.mobilefoundation_short}} 服务的实例
{: #step1create}

1. 在 {{site.data.keyword.Bluemix_notm}} **目录**中，选择 [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation)。这将打开服务配置屏幕。
2. 对您的服务实例命名，或者使用预设的名称。
3. 选择想要在其中创建服务实例的区域、组织和空间。
4. 选择**价格套餐**，然后单击**创建**。

## 步骤 2：构建移动通道
{: #buildmobilechannel}


### 对于 {{site.data.keyword.mobilefoundation_short}}: Developer 套餐
{: #buildchanneldevplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Developer 的实例后，可以通过完成以下步骤开始构建移动通道。

* 您可以立即访问和使用 Mobile Foundation 服务器。

  此选择将创建具有以下设置的 {{site.data.keyword.mfserver_long_notm}}：
  *	1 GB 内存。此大小足够开发和轻量测试活动使用。

  * 要使用 CLI 访问 Mobile Foundation 服务器，您将需要凭证。在 IBM Cloud 控制台的导航窗格中单击**服务凭证**即可获取凭证。

### 对于 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 套餐
{: #buildchannelprofdeviceplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 服务的实例后，可以通过完成以下步骤开始构建移动通道。

  1.  连接到 {{site.data.keyword.Bluemix_notm}} 上的现有 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务。

      1.  选择 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

      + 从所选`组织`中可用空间的列表中，选择 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `空间`。

      + 选择 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} `服务名称`和`凭证`，以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例。

      + 通过单击**测试连接**，测试与所选 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例的连接。

      + 在要求对所选 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务进行确认的弹出窗口上，单击**添加**，然后单击**继续**。此操作将在配置的 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 数据库服务实例中创建必需的表。

      在添加与 {{site.data.keyword.mobilefoundation_short}} 实例的 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 连接之后，您将无法对其进行更改。
      {: note}
  2.  创建并启动服务器。

      1. 使用缺省配置创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，然后单击**启动基本服务器**。

      + 此选择将为 {{site.data.keyword.mfserver_long_notm}} 供应以下设置：
          - 2 个节点，每个节点 1 GB 内存。此大小适合用于开发、中等测试活动和小规模生产工作负载。

          -	自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。

          创建服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：
            -	正在运行的服务器的状态（状态和大小）。
            -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。
            -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

      +	单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。      

      要使用拓扑、安全性和其他服务器配置的高级配置来创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，请单击**使用高级配置启动服务器**。有关更多信息，请参阅[设置高级配置](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#using_mfs_advanced_p5)。
      {: tip}

### 对于 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 套餐
{: #buildchannelprof1appplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服务的实例后，可以通过完成以下步骤开始构建移动通道。

  1.  连接到 {{site.data.keyword.Bluemix_notm}} 上的现有 {{site.data.keyword.Db2_on_Cloud_long}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL_full}} 服务。

      1.  选择 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

      + 从所选`组织`中可用空间的列表中，选择 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `空间`。

      + 选择 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} `服务名称`和`凭证`，以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例。

      + 通过单击**测试连接**，测试与所选 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例的连接。

      + 在要求对所选 {{site.data.keyword.Db2_on_Cloud_short}} 或 {{site.data.keyword.composeForPostgreSQL}} 服务进行确认的弹出窗口上，单击**添加**，然后单击**继续**。此操作将在配置的 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 数据库服务实例中创建必需的表。

      在添加与 {{site.data.keyword.mobilefoundation_short}} 实例的 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 连接之后，您将无法对其进行更改。
      {: note}

  2.  创建并启动服务器。

      1. 使用缺省配置创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，然后单击**启动基本服务器**。

        `基本服务器实例包含单一节点和 1 GB 内存。`

      + 自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。  

        创建服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：
          -	正在运行的服务器的状态（状态和大小）。
          -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。
          -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

      +  单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。  

      要使用拓扑、安全性和其他服务器配置的高级配置来创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，请单击**使用高级配置启动服务器**。有关更多信息，请参阅[设置高级配置](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#using_mfs_advanced_p2)。
      {: tip}

转至[使用 Mobile Foundation 服务设置 MobileFirst 服务器 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/){: new_window}，以了解有关开始使用 {{site.data.keyword.mobilefoundation_short}} 的更多信息。
{: note}

## 步骤 3：在 {{site.data.keyword.mobilefoundation_short}} 中注册应用程序
{: #registerapp}

创建并启动 Mobile Foundation 服务器实例后，您可以执行以下步骤注册 Android 应用程序。

  1.  通过装入 URL `http://<your-server-host>:<server-port>/mfpconsole` 来调用 {{site.data.keyword.mfp_oc_short_notm}}。使用供应时生成的 `username` 和 `password`。

  + 在 {{site.data.keyword.mfp_oc_short_notm}} **仪表板**中，单击**应用程序**旁边的**新建**。

  + 提供 *MFPStarterAndroid* 作为**应用程序名称**。

  + **选择平台**为 *Android*。

  + 提供 *com.ibm.mfpstarterandroid* 作为**应用程序标识**。

  + 输入 *1.0* 作为**版本**。

  + 单击**注册应用程序**。

## 步骤 4：下载样本应用程序
{: #downloadapp}

  1.  在 {{site.data.keyword.mfp_oc_short_notm}} **仪表板**中，选择**应用程序**下的 **MFPStarterAndroid**。

  + 单击**获取入门模板代码**，然后选择下载 Android 应用程序样本。

## 步骤 5：编辑样本应用程序
{: #editapp}

  1. 将早先步骤中下载的样本 Android 应用程序导入到 Android Studio。

  + 在 Android Studio 的**项目**侧边栏菜单中，选择 **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java** 文件。

    * 添加以下导入项
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * 粘贴以下代码片段，将调用替换为 `WLAuthorizationManager.getInstance().obtainAccessToken`

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

## 步骤 6：部署适配器
{: #deployadapter}

  1. 下载此[适配器工件](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download}，并从 {{site.data.keyword.mfp_oc_short_notm}} 使用**操作 → 部署适配器**进行部署。

## 步骤 7：测试应用程序
{: #testapp}

  1. 在 Android Studio 的**项目**侧边栏菜单中，选择 **app → src → main →assets → mfpclient.properties** 文件，然后使用 MobileFirst 服务器的正确值编辑 `protocol`、`host` 和 `port` 属性。

   值通常是 https、*your-server-address* 和 443。
   {: tip}

  2. 单击 Android Studio 中的**运行应用程序**。
     * 您将看到应用程序在设备仿真器上启动。
     * 在应用程序中，单击**对 MobileFirst 服务器执行 Ping 操作**，这将显示`已连接到 MobileFirst 服务器`。
     * 如果应用程序能够连接到 MobileFirst 服务器，那么部署的 Java 适配器将执行资源请求调用。
     * 然后将适配器响应打印到 Android Studio 的 LogCat 视图中。


## 后续步骤
{: #nextsteps-gs}

您可以遵循[快速入门教程 ![外部链接图标](../../icons/launch-glyph.svg "快速入门教程")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} 来使用更多样本应用程序并探索 {{site.data.keyword.mobilefoundation_short}} 的工作方式。

“快速入门”提供了一些教程，其中解释了 {{site.data.keyword.mobilefoundation_short}} 针对 iOS、Android、Web、Cordova、Windows、React Native、Ionic 和 Xamarin 应用程序的工作方式。
