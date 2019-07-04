---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Push Notifications, notifications, unicast notifications, tag notifications, interactive notifications, silent notifications, configure DataPower

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

# 推送通知
{: #push_notifications}

通知是移动设备的一项功能，用于接收从服务器“推送”的消息。无论应用程序是在前台还是后台运行，都会收到通知。  
{: shortdesc}

{{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} 提供一组统一的 API 方法，用于向 iOS、Android、Windows 8.1 Universal、Windows 10 UWP 和 Cordova（iOS 和 Android）应用程序发送推送通知。通知从 {{ site.data.keyword.mfserver_short_notm }} 发送到供应商（Apple、Google、Microsoft 或 SMS Gateways）基础架构，然后从该处发送到相关设备。 统一通知机制使与用户和设备进行通信的整个过程对开发者透明。

## 设备支持
{: #device-support}
{{ site.data.keyword.mobilefoundation_short }} 中的以下平台支持推送通知：

* iOS 8.x 或更高版本
* Android 4.x 或更高版本
* Windows 8.1 或 Windows 10

## 推送通知形式
{: #push-notifications-forms}
通知可以采取多种形式：

* **警报**（iOS、Android 和 Windows）- 弹出式文本消息
* **声音**（iOS、Android 和 Windows）- 收到通知时播放声音文件
* **角标** (iOS) 或磁贴 (Windows) - 允许短文本或图像的图形表示
* **条幅** (iOS) 或 Toast (Windows) - 设备显示屏中会自动消失的弹出式文本消息
* **交互式**（iOS 8 和更高版本）- 已接收通知的条幅中的操作按钮
* **静默**（iOS 8 和更高版本）- 发送通知而不打扰用户

## 推送通知类型
{: #push-notification-types }

### 标记通知
{: #tag-notifications-overview}

标记通知是将预订了特定标记的所有设备作为目标的通知消息。  

基于标记的通知允许根据主题区域或主题对通知进行细分。通知接收方可以选择只在涉及感兴趣的主题时才接收通知。因此，基于标记的通知提供了一种对接收方进行细分的方法。通过此功能，您可以定义标记并按标记发送或接收消息。消息的目标对象只包括已预订某标记的设备。

### 广播通知
{: #broadcast-notifications-overview}

广播通知是标记推送通知的一种形式，将所有预订设备作为目标，任何支持推送的 {{ site.data.keyword.mobilefirst_notm }} 应用程序在缺省情况下都可通过预订保留的 `Push.all` 标记（为每个设备自动创建）来启用此类通知。可通过取消对保留的 `Push.all` 标记的预订来禁用广播通知。

### 单点广播通知
{: #unicast-notifications-overview}

单点广播通知或用户认证的通知都由 OAuth 提供保护。这些通知消息的目标是特定设备或用户标识。用户预订中的用户标识可以来自底层安全上下文。

### 交互式通知
{: #interactive-notifications-overview}

通过交互式通知，当通知到达时，用户可以在不打开应用程序的情况下执行操作。在交互式通知到达时，设备会显示操作按钮以及通知消息。 目前，在装有 iOS V8 及更高版本的设备上支持交互式通知。 如果将交互式通知发送到装有 iOS V8 之前版本的设备，那么将不会显示通知操作。

了解如何处理[交互式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)。

### 静默通知
{: #silent-notifications-overview}

静默通知是既不显示警报也不以其他方式打扰用户的通知。当静默通知到达时，应用程序处理代码将在后台运行，而不是将应用程序转到前台。目前，在装有 iOS V7 及更高版本的设备上支持静默通知。如果将静默通知发送到装有 iOS V7 之前版本的设备，那么当应用程序在后台运行时，将忽略该通知。如果应用程序在前台运行，那么将调用通知回调方法。

了解如何处理[静默通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)。

单点广播通知在有效内容中不包含任何标记。通过在 POST 消息 API 的目标块中分别指定多个设备标识或用户标识，通知消息可以将多个设备或用户作为目标。
{: note}

## 代理设置
{: #proxy-settings}

可使用代理设置来设置用于将通知发送到 APNs 和 FCM 的可选代理。您可以使用 **push.apns.proxy.* ** 和 **push.gcm.proxy.*** 配置属性来设置代理。有关更多信息，请参阅 [List of JNDI properties for {{ site.data.keyword.mfserver_short_notm }} push service](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service)。

WNS 不支持任何代理。
{: note}

### 使用 WebSphere DataPower 作为推送通知端点
{: #proxy-settings-datapower-endpoint}

您可以设置 DataPower 以接受来自 {{ site.data.keyword.mfserver_short_notm }} 的通知请求，并将其重定向到 FCM 和 WNS。

不支持 APNs。
{: note}

#### 配置 {{ site.data.keyword.mfserver_short_notm }}
{: #proxy-settings-config-mfp-server}

在 `server.xml` 中，配置以下 JNDI 属性。
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

其中，`host` 是 DataPower 的主机名，`port` 是用于为 FCM 和 WNS 配置 HTTPS 前端处理程序的端口号。

#### 配置 DataPower
{: #proxy-settings-config-datapower}

1. 登录到 DataPower 设备。
2. 导航至**服务 > 多协议网关 > 新建多协议网关**。
3. 提供用于标识配置的名称。
4. 对于“XML 管理器”和“多协议网关策略”选择“缺省值”，对于“URL 重写策略”选择“无”。
5. 选中**静态后端**单选按钮，并针对**设置缺省后端 URL** 选择以下任一选项：
    - `https://gcm-http.googleapis.com`（针对 FCM）
    - `https://hk2.notify.windows.com`（针对 WNS）
6. 对于“响应类型”和“请求类型”，选择“传递”。

### 生成证书
{: #proxy-settings-generate-cert}

要生成证书，请选择以下任何选项：

* 对于 FCM：
    1. 从命令行中发出 `Openssl` 以获取 FCM 证书。
    2. 运行以下命令：
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
        {: codeblock}
    3. 复制从 *-----BEGIN CERTIFICATE----- 到 -----END CERTIFICATE-----* 之间的内容，并将其保存在扩展名为 `.pem` 的文件中。

* 对于 WNS：
    1. 在命令行中使用 `Openssl` 以获取 WNS 证书。
    2. 运行以下命令：
		```
openssl s_client -connect https://hk2.notify.windows.com:443
		```
        {: codeblock}
    3. 复制从 *-----BEGIN CERTIFICATE----- 到 -----END CERTIFICATE-----* 之间的内容，并将其保存在扩展名为 `.pem` 的文件中。

### 后端设置
{: #proxy-settings-backside-settings}

* 对于 FCM 和 WNS：
    1. 创建加密证书。
        a. 导航至**对象 > 加密配置**，然后单击“加密证书”。
        b. 提供用于标识加密证书的名称。
        c. 单击**上传**以上传所生成的 FCM 证书。
        d. 将**密码别名**设置为“无”。
        e. 单击**生成密钥**。
        ![配置加密证书](images/bck_1.gif "配置加密证书")

    2. 创建加密验证凭证。
        a. 导航至**对象 > 加密配置**，然后单击“加密验证凭证”。
        b. 提供唯一名称。
        c. 对于证书，选择在上一步（步骤 1）中创建的加密证书。
        d. 对于**证书验证方式**，选择“完全匹配证书或直接签发者”。
        e. 单击**应用**。

        ![配置加密验证凭证](images/bck_2.gif "配置加密验证凭证")

    3. 创建加密验证凭证：
        a. 导航至**对象 > 加密配置**，然后单击**加密概要文件**。
        b. 单击**添加**。
        c. 提供唯一名称。
        d. 对于**验证凭证**，从下拉菜单中选择在上一步（步骤 2）中创建的验证凭证，将“标识凭证”设置为**无**。
        e. 单击**应用**。

        ![配置加密概要文件](images/bck_3.gif "配置加密概要文件")

    4. 创建 SSL 代理概要文件：

        a. 导航至**对象** > **加密配置** > **SSL 代理概要文件**。
        b. 选择以下任一选项：

            - 对于 SMS，为 **SSL 代理概要文件**选择“无”。
            - 对于含安全后端 URL (HTTPS) 的 FCM 和 WNS，完成以下步骤：
                i. 单击**添加**。
                ii. 提供稍后可用于标识 SSL 代理概要文件的名称。
                iii. 对于 **SSL 方向**，从下拉列表中选择**正向**。
                iv. 对于“正向（客户机）加密概要文件”，选择步骤 3 中创建的加密概要文件。
                v. 单击**应用**。

        ![配置 SSL 代理概要文件](images/bck_4.gif "配置 SSL 代理概要文件（不推荐）")

    5. 在“多协议网关”窗口的**后端设置**下，选择**代理概要文件**作为 **SSL 客户机类型**，然后选择在步骤 4 中创建的“SSL 代理概要文件”。

        ![配置 SSL 代理概要文件](images/bck_5.gif "“多协议网关”窗口")

### 前端设置
{: #proxy-settings-frontside-settings}

* 对于 FCM 和 WNS：

    1. 创建密钥证书对，并使用 DataPower 的主机名作为“通用名称 (CN)”的值：
        a. 导航至**管理** > **杂项**，然后单击**加密工具**。
        b. 输入 DataPower 的主机名作为“通用名称 (CN)”的值。
        c. 如果计划稍后导出专用密钥，请选择**导出专用密钥**，然后单击**生成密钥**。

        ![创建密钥证书对](images/frnt_1.gif "创建密钥证书对")

    2. 创建加密标识凭证：
        a. 导航至**对象** > **加密配置**，然后单击**加密标识凭证**。
        b. 单击**添加**。
        c. 提供唯一名称。
        d. 对于“加密密钥”和“证书”，从列表框中选择在上一步（步骤 1）中生成的密钥和证书。
        e. 单击**应用**。

        ![创建加密标识凭证](images/frnt_2.gif "创建加密标识凭证")

    3. 创建加密概要文件：

        a. 导航至**对象** > **加密配置**，然后单击**加密概要文件**。
        b. 单击**添加**。
        c. 提供唯一名称。
        d. 对于“标识凭证”，从列表框中选择在上一步（步骤 2）中创建的标识凭证。将“验证凭证”设置为“无”。
        e. 单击**应用**。

        ![配置加密概要文件](images/frnt_3.gif "配置加密概要文件")

    4. 创建 SSL 代理概要文件：

        a. 导航至**对象** > **加密配置** > **SSL 代理概要文件**。
        b. 单击**添加**。
        c. 提供唯一名称。
        d. 对于“SSL 方向”，从列表框中选择**逆向**。
        e. 对于“逆向（服务器）加密概要文件”，选择在上一步（步骤 3）中创建的加密概要文件。  
        f. 单击**应用**。

        ![配置 SSL 代理概要文件](images/frnt_4.gif "配置 SSL 代理概要文件")

    5. 创建 HTTPS 前端处理程序：

        a. 导航至**对象** > **协议处理程序** > **HTTPS 前端处理程序**。
        b. 单击**添加**。
        c. 提供唯一名称。
        d. 对于**本地 IP 地址**，选择正确的别名，或者保留其缺省值 (0.0.0.0)。
        e. 提供可用端口。
        f. 对于**允许的方法和版本**，选择：HTTP 1.0、HTTP 1.1、POST 方法、GET 方法、含 ? 的 URL、含 # 的 URL 、含 .. 的 URL。
        g. 选择**代理概要文件**作为“SSL 服务器类型”。
        h. 对于 SSL 代理概要文件（不推荐），选择在上一步（步骤 4）中创建的 SSL 代理概要文件。
        i. 单击**应用**。

        ![配置 HTTPS 前端处理程序](images/frnt_5.gif "配置 HTTPS 前端处理程序")

    6. 在“配置多协议网关”页面上的**前端设置**下，选择在步骤 5 中创建的 HTTPS 前端处理程序作为**前端协议**，然后单击**应用**。

        ![常规配置](images/frnt_6.gif "常规配置")

    在“前端设置”中供 DataPower 使用的证书为自签名证书。除非将证书添加到 {{ site.data.keyword.mobilefirst_notm }} 使用的 JRE 密钥库，否则与 DataPower 的连接将失败。

## 后续步骤
{: #next-steps }

执行服务器端和客户机端的必需设置，以便发送和接收推送通知：

* [配置推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [发送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [在客户机应用程序中处理推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [静默通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [交互式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
