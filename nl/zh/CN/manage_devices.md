---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 管理设备
{: #manage_devices}

可以在 Mobile Foundation Operations Console 中管理设备访问权和设备状态。设备通过名为“设备标识”的标识进行唯一标识，该标识由 Mobile Foundation 客户机 SDK 分配。您还可以设置设备的显示名称。将对“设备标识”和“设备显示名称”字段建立索引以用于搜索。

应用程序开发者可使用 `WLClient` 类的 `setDeviceDisplayName` 方法来设置设备显示名称。请参阅[此处](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/)以获取 `WLClient` 文档。Java 适配器开发者还可以使用 `com.ibm.mfp.server.registration.external.model MobileDeviceData` 类的 `setDeviceDisplayName` 方法来设置设备显示名称。
{: tip}

Mobile Foundation 服务器会维护访问服务器的每个设备的状态信息。
可能的状态值有：
* 活动
* 丢失
* 失窃
* 到期
* 禁用

设备的缺省状态为**活动**，这指示不会阻止通过此设备进行的访问。可以将状态更改为**丢失**、**失窃**或**禁用**，以阻止通过此设备访问应用程序资源。您可以随时复原为**活动**状态以重新允许访问。

**到期**状态是一种特殊状态，由 Mobile Foundation 服务器在设备达到预配置的不活动持续时间后进行设置。此状态用于许可证跟踪，不会影响设备访问权。状态为**到期**的设备重新连接到服务器时，其状态会复原为**活动**，并且会向该设备授予对服务器的访问权。

## 阻止设备
{: #block_devices}

1. 在 Mobile Foundation Operations Console 中，转至**设备**页面。
2. 使用**搜索**字段可通过提供与设备关联的用户标识来搜索设备，或通过提供设备的显示名称（如果已在设备上设置）来搜索设备。
3. 对于搜索结果中返回的每个设备，可以看到设备标识、显示名称、设备型号、操作系统以及与该设备关联的用户标识列表。
4. “设备状态”列会显示设备的状态。可以将设备的状态更改为**丢失**、**失窃**或**禁用**，以阻止通过此设备访问受保护的应用程序资源。

   将状态更改回**活动**将复复原始访问权。
   {: note}


## 注销设备
{: #unregister_devices}

1. 在 Mobile Foundation Operations Console 中，转至**设备**页面。
2. 使用**搜索**字段可通过提供与设备关联的用户标识来搜索设备，或通过提供设备的显示名称（如果已在设备上设置）来搜索设备。
3. 对于搜索结果中返回的每个设备，可以看到设备标识、显示名称、设备型号、操作系统以及与该设备关联的用户标识列表。
4. 从**操作**列中选择*注销*。

   注销设备会删除该设备上安装的所有 Mobile Foundation 应用程序的注册数据。另外，还将删除设备显示名称、与此设备关联的用户列表以及应用程序为此设备注册的公共属性。
   {: note}


如果要**阻止**设备，请勿**注销**设备。注销设备将除去与设备相关的所有数据。如果用户尝试使用该设备访问 Mobile Foundation 服务器，系统将提示该用户注册设备，注册操作将在服务器中为该设备创建新的设备标识。这意味着设备会重新获得对 Mobile Foundation 服务器的访问权。
{: important}
