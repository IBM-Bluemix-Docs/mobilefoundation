---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: disable apps, remote disabling of apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 远程禁用应用程序版本
{: #remotely_disable_an_app_version}

在此部分中，我们讨论如何禁用对特定移动操作系统上特定应用程序版本的用户访问权，以及如何向用户提供定制消息。

您可以使用 Mobile Foundation Operations Console 来管理应用程序访问。

1. 在控制台导航侧边栏的**应用程序**部分中，选择应用程序版本，然后选择应用程序**管理**选项卡。
2. 将状态更改为**已禁用访问**。
3. 在**最新版本的 URL** 字段中提供新应用程序版本的 URL（通常，位于相应的公共或专用应用程序商店中）。
   对于某些环境，Application Center 提供了一个 URL，用于直接访问应用程序版本的详细视图。请参阅 [Application properties](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties)。
   {: tip}

4. 在**缺省通知消息**字段中，添加当用户尝试访问应用程序时要显示的定制通知消息。以下样本消息指示用户升级到最新版本：`不再支持此版本。请升级到最新版本。`
5. 在**支持的语言环境**部分中，可以选择提供其他语言的通知消息。
6. 选择**保存**以应用更改。

用户运行已远程禁用的应用程序时，将显示一个对话窗口，其中包含配置的定制消息。针对需要访问受保护资源的任何应用程序交互，或在应用程序尝试获取访问令牌时，都将显示此消息。如果提供了版本升级 URL，那么此对话框将显示**获取新版本**按钮，以用于升级到更新版本，此外还会显示缺省**关闭**按钮。<br/>如果用户关闭对话窗口而不升级版本，那么他们可以继续使用不受保护的应用程序资源。但是，需要访问受保护资源的任何应用程序交互将使该对话窗口再次显示，并且不会授予该应用程序或用户对该资源的访问权。
