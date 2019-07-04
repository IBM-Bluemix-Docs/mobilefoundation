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

#	使用 Professional Per Device 套餐进行设置
{: #using_mobilefoundation_p5}

使用 Professional Per Device 套餐，用户可以在生产环境中构建、测试和运行移动应用程序，与移动用户或设备数无关。收费根据的是每日客户机设备数。此套餐支持大型部署和高可用性。创建 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 服务实例后，请阅读以下过程以开始使用该服务。

## Professional Per Device 套餐的先决条件
{: #prerequisites_p5}

配置 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 服务实例之前，请考虑以下问题：
* 仅 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}} 套餐支持 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device。

* 您需要拥有对 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例凭证的访问权，才能配置 {{site.data.keyword.mobilefoundation_short}} 服务实例的设置。

{{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例可以存在于您的 {{site.data.keyword.Bluemix_notm}} `组织`或您有权访问的其他任何`组织`内的任何`空间`中。请确保您有权访问 {{site.data.keyword.Db2_on_Cloud_short}} 或 {{site.data.keyword.composeForPostgreSQL}} 服务实例所在的`空间`。
{: note}

## 添加数据库连接
{: #configure_dashdb_p5}

###  首要步骤
{: #firststeps_p5}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 服务实例之后，请按照下面的过程开始使用该服务。

### 设置数据库连接
{: #connect_dashdb_p5}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional Per Device 服务实例后，您将看到*概述*页面，在其中可指定 {{site.data.keyword.mobilefoundation_short}} 服务实例需要连接到的 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例的连接信息。

如果您没有现有的 {{site.data.keyword.Db2_on_Cloud_short}}（除**轻量**套餐以外的其他任何套餐）或 {{site.data.keyword.composeForPostgreSQL}} 服务实例，那么还可以创建新的服务实例。

按照以下步骤，创建新的 Db2 on Cloud 服务实例：

1. 在*概述*页面上，选择**创建新服务**部分。

+ 如果需要 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例具有高可用性，请在**高可用性配置**选项上选择`是`。

+ 检查套餐详细信息，然后单击**创建**。

这样就创建了新的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例，也就提供了具有 8 GB RAM、2 个 vCPU 和 500 GB 存储的专用 {{site.data.keyword.Db2_on_Cloud_short}} 实例。

按照以下步骤，连接到现有的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例或创建的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例：

1. 选择 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

+ 从所选`组织`内可用的空间列表中，选择具有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的 {{site.data.keyword.Bluemix_notm}} `空间`。   
如果未看到列出 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的`组织`和`空间`，请检查您是否为该`组织`和`空间`的成员。您需要对组织和空间具有 *Developer* 角色访问权，因为 {{site.data.keyword.mobilefoundation_short}} 服务从 {{site.data.keyword.Db2_on_Cloud_short}} 服务访问凭证。
{: note}
+ 选择 {{site.data.keyword.Db2_on_Cloud_short}} `服务名称`和`凭证`以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。

+  测试与指定的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的连接。

+  单击**添加**。此操作可在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 数据库服务实例中创建需要的表。

几秒钟后，可以访问“`概述`”页面，其中为您提供教程和视频，可帮助您开始使用 {{site.data.keyword.mobilefoundation_short}} 服务。

您无法更改已配置为由 {{site.data.keyword.mobilefoundation_short}} 服务实例使用的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。但是，您可以在多个 {{site.data.keyword.mobilefoundation_short}} 服务实例上使用同一 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例，因为每个 {{site.data.keyword.mobilefoundation_short}} 服务实例都会在所选 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例中创建自己的模式。
{: note}

## 启动使用 Professional Per Device 套餐创建的 MobileFirst 服务器
{: #start_mobilefoundation_p5}

* 要使用缺省设置启动 {{site.data.keyword.mfserver_short_notm}}，请单击**启动基本服务器**。

* 此选择将创建具有以下设置的 {{site.data.keyword.mfserver_long_notm}}：
    -  2 个节点，每个节点 1 GB 内存。此大小适合用于开发、中等测试活动和小规模生产工作负载。

    -	自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。

创建服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：

  -	正在运行的服务器的状态（状态和大小）。

  -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。

  -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

*	单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。


通过控制台，您可以管理移动应用程序、适配器和移动设备，还可以使用服务器作为移动后端以及发送推送通知等。



## 使用 Professional Per Device 套餐时重新创建 MobileFirst 服务器
{: #recreate_mobilefoundation_p5}

*	单击**重新创建**以重新创建服务器。

* 此操作将停止现有服务器并删除数据。将会使用更新的版本（如果可用）来创建新的服务器实例。此操作会花费几分钟才能完成。

先前服务器实例中的数据（包含有关应用程序和适配器的信息）会持久存储在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例中。在重新创建服务器时会使用这些数据。
{: note}

##	设置 Professional Per Device 套餐中的高级配置
{: #using_mfs_advanced_p5}

使用“`概述`”页面中的**使用高级配置启动服务器**，可使用高级或定制设置创建服务器。还可以通过单击**设置**选项卡更新服务器设置，以定制服务器配置。{{site.data.keyword.mobilefoundation_short}} 为您提供对某些高级设置的访问权。

*	在**拓扑**选项卡中，可以选择服务器大小以及所需的服务器实例数。缺省 1 GB 服务器足够开发和轻量测试使用。
  - 根据您的需要，选择正确的服务器大小。

  - **实例**显示已创建的实例数。

## Professional Per Device 套餐中的 Mobile Analytics
{: #mobile_analytics_p5}

Mobile Foundation: Developer 套餐服务实例中包含并预配置了 Mobile Analytics 服务器。

* 从 {{site.data.keyword.mfp_oc_short_notm}} 启动 Mobile Analytics 控制台。

有关 Mobile Analytics 的更多信息，请参阅[此处](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}。
