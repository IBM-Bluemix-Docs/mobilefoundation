---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

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


# 配置推送通知
{: #configure_push_notifications}

为了向 iOS、Android 或 Windows 设备发送推送通知，{{ site.data.keyword.mfserver_short_notm }} 首先需要配置用于 Android 的 FCM 详细信息、用于 iOS 的 APNs 证书或用于 Windows 8.1 Universal/Windows 10 UWP 的 WNS 凭证。
{: shortdesc}

然后，可以使用以下选项来发送通知：
* 所有设备（广播）
* 注册到特定标记的设备
* 单个设备标识
* 用户标识
* 仅 iOS 设备
* 仅 Android 设备
* 仅 Windows 设备
* 基于已认证的用户

## 设置通知
{: #setting-up-notifications }

要启用通知支持，需要在 {{ site.data.keyword.mfserver_short_notm }} 和客户机应用程序中执行几个配置步骤。继续了解服务器端设置，或者跳转至[客户机端设置](#tutorials-to-follow-next)。

在服务器端，必需的设置包括：配置所需供应商（APNs、FCM 或 WNS）和映射“push.mobileclient”作用域。

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

Google [不推荐使用 GCM](https://developers.google.com/cloud-messaging/faq)，并已将云消息传递与 Firebase 相集成。如果您正在使用 GCM 项目，请确保[将 Android 上的 GCM 客户机应用程序迁移到 FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)。
{: note}

Android 设备将 Firebase Cloud Messaging (FCM) 服务用于推送通知。

要设置 FCM：

1. 访问 [Firebase Console](https://console.firebase.google.com/?pli=1)。
2. 创建项目并提供项目名称。
3. 单击“设置”的“齿轮”图标并选择**项目设置**。
4. 单击**云消息传递**选项卡以生成**服务器 API 密钥**和**发送方标识**，然后单击**保存**。

您还可以使用 [{{ site.data.keyword.mobilefirst_notm }} 推送服务的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) 或 [{{ site.data.keyword.mobilefirst_notm }} 管理服务的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi) 来设置 FCM。
{: note}

如果组织的防火墙限制了与因特网之间的流量，那么必须执行以下步骤：  
* 将防火墙配置为允许与 FCM 的连接，以便 FCM 客户机应用程序能够接收消息。
* 要打开的端口是 5228、5229 和 5230。FCM 通常只使用 5228，但有时也会使用 5229 和 5230。
* FCM 没有提供特定 IP，因此您必须允许防火墙接受与 Google ASN 15169 中所列 IP 块中包含的所有 IP 地址的出局连接。
* 确保防火墙接受端口 443 上从 {{ site.data.keyword.mfserver_short_notm }} 到 fcm.googleapis.com 的出局连接。
{: note }

<img class="gifplayer" alt="添加 GCM 凭证的图像" src="images/gcm-setup.png"/>

### Apple 推送通知服务
{: #apple-push-notifications-service }

iOS 设备将 Apple 推送通知服务 (APNs) 用于推送通知。  

要设置 APNs：

1. 为开发或生产环境生成推送通知证书。有关详细步骤，请参阅[此处](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)的`针对 iOS` 部分。
2. 在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的应用程序] → 推送 → 推送设置**中，选择证书类型并提供证书的文件和密码。然后，单击**保存**。

  * 要发送推送通知，必须可从 {{ site.data.keyword.mfserver_short_notm }} 实例访问以下服务器：
    * 沙箱服务器：
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * 生产服务器：
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * 在开发阶段，使用 apns-certificate-sandbox.p12 沙箱证书文件。
  * 在生产阶段，使用 apns-certificate-production.p12 生产证书文件。
    * 仅当使用 APNs 生产证书的应用程序成功提交到 Apple App Store 时，才能测试该证书。
    {: note }

MobileFirst 不支持通用证书。
{: note }

您还可以使用 [{{ site.data.keyword.mobilefirst_notm }} 推送服务的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) 或 [{{ site.data.keyword.mobilefirst_notm }} 管理服务的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc) 来设置 APNs。
{: note}

<img class="gifplayer" alt="添加 APNs 凭证的图像" src="images/apns-setup.png"/>

### Windows 推送通知服务
{: #windows-push-notifications-service }

Windows 设备将 Windows 推送通知服务 (WNS) 用于推送通知。  

要设置 WNS：

1. 请遵循 [Microsoft 提供的指示信息](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx)来生成**包安全标识 (SID)** 和**客户机密钥**值。
2. 在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的应用程序] → 推送 → 推送设置**中，添加这些值并单击**保存**。

您还可以使用 [{{ site.data.keyword.mobilefirst_notm }} 推送服务的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) 或 [{{ site.data.keyword.mobilefirst_notm }} 管理服务的 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc) 来设置 WNS

<img class="gifplayer" alt="添加 WNS 凭证的图像" src="images/wns-setup.png"/>

### 作用域映射
{: #scope-mapping }

将 **push.mobileclient** 作用域元素映射到应用程序。

1. 装入 {{ site.data.keyword.mfp_oc_short_notm }}，然后导航至 **[您的应用程序] → 安全性 → 作用域/元素映射**，单击**新建**。
2. 在**作用域元素**字段中写入“push.mobileclient”。然后，单击**添加**。

更多可用作用域的列表：

|**作用域**| **描述**|
|---|---|
|apps.read	|允许读取应用程序资源。|
|apps.write	|允许创建、更新或删除应用程序资源。|
|gcmConf.read	|允许读取 GCM 配置设置（API 密钥和发送方标识）。|
|gcmConf.write	|允许更新或删除 GCM 配置设置。|
|apnsConf.read	|允许读取 APNs 配置设置。|
|apnsConf.write	|允许更新或删除 APNs 配置设置。|
|devices.read	|允许读取设备。|
|devices.write	|允许创建、更新或删除设备。|
|subscriptions.read	|允许读取预订。|
|subscriptions.write	|允许创建、更新或删除预订。|
|messages.write	|允许发送推送通知。|
|webhooks.read	|允许读取事件通知。|
|webhooks.write	|允许发送事件通知。|
|smsConf.read	|允许读取 SMS 配置设置。|
|smsConf.write	|允许更新或删除 SMS 配置设置。|
|wnsConf.read	|允许读取 WNS 配置设置|
|wnsConf.write	|允许更新或删除 WNS 配置设置。|
{: caption="表 1. 作用域描述" caption-side="top"}

<img class="gifplayer" alt="作用域映射" src="images/scope-mapping.png"/>

### 已认证的通知
{: #authenticated-notifications }

已认证的通知是发送到一个或多个`用户标识`的通知。  

将 **push.mobileclient** 作用域元素映射到应用程序所使用的安全性检查。  

1. 装入 {{ site.data.keyword.mfp_oc_short_notm }}，然后导航至 **[您的应用程序] → 安全性 → 作用域/元素映射**，然后单击**新建**或编辑现有作用域映射条目。
2. 选择安全性检查。然后，单击**添加**。

  <img class="gifplayer" alt="已认证的通知" src="images/authenticated-notifications.png"/>

## 定义标记
{: #defining-tags }
在 {{ site.data.keyword.mfp_oc_short_notm }} → **[您的应用程序] → 推送 → 标记**中，单击**新建**。  
提供适当的`标记名称`和`描述`，然后单击**保存**。

<img class="gifplayer" alt="添加标记" src="images/adding-tags.png"/>

预订将设备注册与标记捆绑在一起。当从标记中注销设备时，将从该设备自身自动取消预订所有相关的预订。在存在设备的多个用户的场景中，必须根据用户登录条件在移动应用程序中实现预订。例如，在用户成功登录到应用程序时发出预订调用，而在处理注销操作的过程中明确发出取消预订调用。

## 后续教程
{: #tutorials-to-follow-next }

* [发送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

服务器端现已设置，下面设置客户机端并处理收到的通知。

* [在客户机应用程序中处理推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
