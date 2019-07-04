---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, restrict backend access, tampered apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# 阻止被篡改的应用程序访问后端
{: #prevent_tampered_apps_accessing_backend}

应用程序真实性有助于在启用任何服务之前先检查应用程序有效性，从而防止被篡改的应用程序访问后端服务。
{: shortdesc}

要正确保护应用程序，请启用预定义的 MobileFirst 应用程序真实性安全检查 (``appAuthenticity``)。此检查启用后，会先验证应用程序的真实性，然后再向应用程序提供任何服务。生产环境中的应用程序应该已启用此功能。

要启用应用程序真实性，可以遵循 **MobileFirst Operations Console → [your-application] → 真实性**中的屏幕上指示信息进行操作，或者查看以下信息。

<dl>
  <dt>可用性</dt>
  <dd>在所有支持的平台（iOS、Android、Windows 8.1 Universal 和 Windows 10 UWP）上，应用程序真实性在 Cordova 和本机应用程序中都可用。</dd>
  <dt>限制</dt>
  <dd>在 iOS 中，应用程序真实性不支持**位码**。因此，在使用应用程序真实性之前，请在 Xcode 项目属性中禁用位码。</dd>
</dl>

## 应用程序真实性流程
{: #appauthenticityflow}

应用程序真实性安全检查在应用程序注册到 MobileFirst 服务器期间运行，此操作在应用程序实例首次尝试连接到服务器时执行。缺省情况下，真实性检查不会再次运行。

启用了应用程序真实性后，如果客户需要在其应用程序中引入任何更改，那么需要升级应用程序版本。

请参阅[配置应用程序真实性](#configappauthenticity)，以了解如何定制此行为。

### 启用应用程序真实性
{: #enableappauthenticity}

要在应用程序中启用应用程序真实性，请执行以下操作：

1. 在您偏爱的浏览器中打开 MobileFirst Operations Console。
2. 从导航侧边栏中选择应用程序，然后单击**真实性**菜单项。
3. 将**状态**框中的**开/关**按钮切换成“开”。

![启用应用程序真实性](/images/enable_application_authenticity.png)

MobileFirst 服务器会在应用程序首次尝试连接到该服务器时，验证应用程序的真实性。要同时对受保护资源应用此验证，请向保护作用域添加 appAuthenticity 安全性检查。

### 禁用应用程序真实性
{: #disableappauthenticity}

开发期间对应用程序所做的某些更改可能会导致应用程序的真实性验证失败。因此，建议在开发过程中禁用应用程序真实性。生产环境中的应用程序应该已启用此功能。

要禁用应用程序真实性，请将**状态**框中的**开/关**按钮切换回“关”。

## 配置应用程序真实性
{: #configappauthenticity}

缺省情况下，仅在客户机注册期间会检查应用程序真实性。但是，正如其他任何安全性检查一样，您可以决定遵循“保护资源”下的指示信息，通过从控制台执行 ``appAuthenticity`` 安全性检查来保护应用程序或资源。

可以使用以下属性来配置预定义的应用程序真实性安全检查：

* ``expirationSec``：缺省为 3600 秒/1 小时。定义真实性令牌到期之前的持续时间。

在真实性检查完成后，直到令牌根据设置值到期后，才会再次执行真实性检查。

要配置 ``expirationSec`` 属性，请执行以下操作：

1. 装入 MobileFirst Operations Console，导航至 **[your application] → 安全性 → 安全性检查配置**，然后单击**新建**。
2. 搜索 ``appAuthenticity`` scope 元素。
3. 设置新值（以秒为单位）。

    ![配置到期时间（以秒数为单位）](/images/configuring_expirationSec.png)

## Build Time Secret (BTS)
{: #buildtimesecret}

Build Time Secret (BTS) 是一个**可选工具，用于增强真实性验证**，仅适用于 iOS 应用程序。 该工具将构建时确定的密钥注入应用程序，之后在真实性验证过程中使用该密钥。

可以从 **MobileFirst Operations Console → 下载中心**下载 BTS 工具。

要在 Xcode 中使用 BTS 工具，请执行以下操作：

1. 在**构建阶段**选项卡下，单击 **+** 按钮，并创建新的**运行脚本阶段**。
2. 复制 BTS 工具的路径，并将其粘贴到已创建的新“运行脚本阶段”中。
3. 将该运行脚本阶段拖至**编译源代码阶段**之前。

构建应用程序的生产版本时，应使用该工具。

## 对应用程序真实性进行故障诊断
{: #troubleshooting-app-authenticity}

### 重置
{: #trblreset}

应用程序真实性算法在其验证中会使用应用程序数据和元数据。启用应用程序真实性后连接到服务器的第一个设备会提供应用程序的“指纹”，指纹中包含其中一些数据。

可以重置此指纹，为算法提供新数据。在开发期间（例如，在 Xcode 中更改应用程序后），这可能非常有用。要重置指纹，请在 mfpadm CLI 中运行 **reset** 命令。

重置指纹后，appAuthenticity 安全性检查将继续像之前一样工作（这对于用户来说是透明的）。

### 验证类型
{: #trblvalidationtypes}

Mobile First Platform Foundation 为应用程序提供静态和动态应用程序真实性。这些验证类型在用于生成应用程序真实性种子值的算法和属性方面不同。缺省情况下，启用应用程序真实性后，将使用动态验证算法。这两种验证类型都可确保应用程序的安全性。动态应用程序真实性使用严格验证，并检查应用程序真实性。对于静态应用程序真实性，我们使用略宽松的算法，即不会使用动态应用程序真实性中所用的全部验证检查。

动态应用程序真实性可通过 MobileFirst 控制台进行配置。内部算法负责根据控制台中选择的选项来生成应用程序真实性数据。对于静态应用程序真实性，需要使用 mfpadm CLI。

要启用静态应用程序真实性并在验证类型之间切换，请使用 mfpadm CLI：

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE 可以是 dynamic 或 static。
