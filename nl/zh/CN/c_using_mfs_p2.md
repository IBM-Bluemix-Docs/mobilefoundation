---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	使用 Professional 1 Application 套餐
{: #using_mobilefoundation_p2}

使用 Professional 1 Application 套餐，用户可以创建 1 个支持多个移动操作系统的移动应用程序。创建 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服务实例后，请阅读以下步骤以开始使用该服务。

## 先决条件
{: #prerequisites_p2}

配置 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服务实例之前，请考虑以下内容。
* 仅 {{site.data.keyword.Db2_on_Cloud_short}} {{site.data.keyword.Bluemix_notm}} 套餐支持 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application。

* 您应该拥有对 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例凭证的访问权，才能配置 {{site.data.keyword.mobilefoundation_short}} 服务实例的设置。

> **注**：{{site.data.keyword.Db2_on_Cloud_short}} 服务实例可以存在于您的 {{site.data.keyword.Bluemix_notm}} `组织`或您有权访问的任何其他`组织`内的任何`空间`中。请确保您有权访问 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的`空间`。


## 添加数据库连接
{: #configure_dashdb_p2}

###  首要步骤
{: #firststeps_p2}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服务实例之后，请按照下面的过程开始进行使用。

### 设置与 Db2 on Cloud 服务实例的连接
{: #connect_dashdb_p2}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服务实例后，您将看到*概述*页面，您需要在该页面中指定 {{site.data.keyword.mobilefoundation_short}} 服务实例应该连接到的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的连接信息。

如果还没有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例，也可以新建一个。

按照以下步骤，创建新的 Db2 on Cloud 服务实例：

1. 在*概述*页面上选择**创建新服务**部分。

+ 如果需要 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例具有高可用性，请在**高可用性配置**选项上选择`是`。

+ 检查套餐详细信息，然后单击**创建**。

这样就创建了新的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例，也就提供了具有 8 GB RAM、2 个 vCPU 和 500 GB 存储的专用 {{site.data.keyword.Db2_on_Cloud_short}} 实例。

按照以下步骤，连接到现有的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例或刚创建的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例：

1. 选择 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

+ 从所选`组织`内可用的空间列表中，选择具有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的 {{site.data.keyword.Bluemix_notm}} `空间`。   
> **注：**如果未看到 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的`组织`和`空间`列表，请检查您是否是该`组织`和`空间`的成员。您需要对组织和空间具有 *Developer* 角色访问权，因为 {{site.data.keyword.mobilefoundation_short}} 服务从 {{site.data.keyword.Db2_on_Cloud_short}} 服务访问凭证。

+ 选择 {{site.data.keyword.Db2_on_Cloud_short}} `服务名称`和`凭证`以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。

+  测试与指定的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的连接。

+  单击**添加**。此操作可在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 数据库服务实例中创建需要的表。

几秒钟后，可以访问“`概述`”页面，其中为您提供教程和视频，可帮助您开始使用 {{site.data.keyword.mobilefoundation_short}} 服务。

> **注**：您无法更改已配置为由 {{site.data.keyword.mobilefoundation_short}} 服务实例使用的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。但是，您可以在多个 {{site.data.keyword.mobilefoundation_short}} 服务实例上使用同一 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例，因为每个 {{site.data.keyword.mobilefoundation_short}} 服务实例都会在所选 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例中创建自己的模式。


## 启动 MobileFirst 服务器
{: #start_mobilefoundation_p2}

* 要使用缺省设置启动 {{site.data.keyword.mfserver_short_notm}}，请单击**启动基本服务器**。

* 此选择将为 {{site.data.keyword.mfserver_long_notm}} 供应以下设置：
    -  1 GB 内存。此大小足够用于开发、中等测试活动和小规模生产工作负载。

    -	自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。

    供应服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：

      -	正在运行的服务器的状态（状态和大小）。

      -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。

      -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

*	单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。

通过控制台，您可以管理移动应用程序、适配器和移动设备，还可以使用服务器作为移动后端以及发送推送通知等。



##  添加 Mobile Analytics 服务
{: #adding_analytics_server_prof}

 现在，您可以将 Mobile Analytics 服务添加到 {{site.data.keyword.mobilefoundation_short}} 服务实例来监视 {{site.data.keyword.mobilefirst}} 服务器上的移动应用程序。

 Professional 套餐会创建 Mobile Analytics 服务实例。

 <!--Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui){: new_window}.-->

 * 单击**添加分析**，将 Mobile Analytics 服务添加到 {{site.data.keyword.mobilefoundation_short}} 服务实例。

   供应过程启动。此过程会花费几分钟，并且进度指示器会指示此操作的进度。  

 * 从 {{site.data.keyword.mfp_oc_short_notm}} 启动 Mobile Analytics 服务控制台。

 * 在 {{site.data.keyword.mfserver_short_notm}} 与 Mobile Analytics 服务之间启用单点登录。为 Mobile Analytics 服务配置与 {{site.data.keyword.mfserver_short_notm}} 服务器相同的 LTPA 密钥和用户凭证。您可以像登录 {{site.data.keyword.mfp_oc_short_notm}} 一样，使用相同的 `username` 和 `password` 登录到 Mobile Analytics 控制台。

 有关 Mobile Analytics 的更多信息，请参阅 [MobileFirst Foundation Operational Analytics ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}。

> **注：**删除 {{site.data.keyword.mobilefoundation_short}} 服务实例将除去 Mobile Analytics 服务实例。

##  删除 Mobile Analytics 服务
{: #deleting_analytics_server_prof}

您现在可以从 {{site.data.keyword.mobilefoundation_short}} 服务仪表板删除已添加到 {{site.data.keyword.mobilefoundation_short}} 服务实例的 Mobile Analytics 服务。

* 单击**删除分析**以删除已添加到 {{site.data.keyword.mobilefoundation_short}} 服务实例的 Mobile Analytics 服务。

 单击**删除分析**以删除分析服务器实例。删除分析实例的过程需要 10 分钟左右。可以刷新屏幕以查看更新的状态。删除分析实例将重新启用**添加分析**按钮。如果选择重新添加 Mobile Analytics 服务，可单击此按钮。


## 重新创建 MobileFirst 服务器
{: #recreate_mobilefoundation_p2}

*	单击**重新创建**以重新创建服务器。

* 此操作将停止现有服务器并删除数据。将会使用更新的版本（如果可用）来创建新的服务器实例。此操作会花费几分钟才能完成。

> **注**：先前服务器实例中的数据（包括有关应用程序和适配器的信息）会持久存储在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例中。在重新创建服务器时会使用这些数据。

##	设置高级配置
{: #using_mfs_advanced_p2}

使用“`概述`”页面中的**使用高级配置启动服务器**，可使用高级或定制设置创建服务器。还可以通过单击**配置**选项卡更新服务器设置，以定制服务器配置。{{site.data.keyword.mobilefoundation_short}} 为您提供对某些高级设置的访问权。

*	在**拓扑**选项卡中，可以选择服务器大小以及所需的服务器实例数。缺省 1 GB 服务器足够开发和轻量测试使用。
  - 根据您的需要，选择正确的服务器大小。

  - **实例**显示已创建的节点数。

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here.-->

请参阅 [{{site.data.keyword.mobilefoundation_long}} 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}，以了解更多详细信息。
