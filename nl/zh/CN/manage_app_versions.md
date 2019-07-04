---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 管理应用程序版本
{: #manage_app_versions}

Mobile Foundation 应用程序管理功能为 Mobile Foundation 服务器用户和管理员提供了对其应用程序的用户和设备访问权的详细控制。

Mobile Foundation 服务器会跟踪对移动基础架构的所有访问尝试，并存储有关应用程序、用户以及安装该应用程序的设备的信息。应用程序、用户和设备之间的映射构成了服务器的移动应用程序管理功能的基础。

通过使用 Mobile Foundation Operations Console，您可以监视和管理对资源的访问权。您还可以管理特定应用程序版本。

1.  转至 Mobile Foundation Operations Console，单击**应用程序**，选择要管理的应用程序，从显示的**版本**列表中，选择您感兴趣的特定应用程序版本。
    ![管理应用程序版本](images/app_version_management.png)

2. 在**管理**选项卡下，您将看到用于设置所选应用程序版本的应用程序状态的选项。应用程序版本支持的状态包括：
   * 活动
   * 活动并且正在通知
   * 已禁用访问
3. 可以通过在**应用程序访问 > 状态**下选择*已禁用访问*选项来禁用应用程序版本。
4. 您还可以配置为在 **Direct Update** 部分中上传 Cordova 应用程序的已更新 Web 资源。然后，可向使用此特定应用程序版本连接到 Mobile Foundation 服务器的用户提供选项，用于通过 Direct Update 来更新其应用程序。
5. 您还可以使用**操作**菜单中的以下选项，对所选应用程序版本执行以下操作：
   *  删除版本
   *  克隆版本
   *  导出版本


有关管理设备的更多信息，请参阅[管理设备](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices)。
有关远程禁用应用程序版本的更多信息，请参阅[远程禁用应用程序版本](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version)。
{: note}
