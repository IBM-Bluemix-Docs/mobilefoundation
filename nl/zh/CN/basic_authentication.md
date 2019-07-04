---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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


# 认证和安全性
{: #basic_authentication}

MobileFirst 安全框架基于 [OAuth 2.0](http://oauth.net/) 协议。根据此协议，资源可以通过**作用域**进行保护，作用域用于定义访问资源所需的许可权。要访问受保护资源，客户机必须提供匹配的**访问令牌**，其中封装了授予客户机的权限的作用域。

OAuth 协议将授权服务器的角色与托管资源的资源服务器相分隔。

* 授权服务器管理客户机授权和令牌生成。
* 资源服务器使用授权服务器来验证客户机提供的访问令牌，并确保它与所请求资源的保护作用域相匹配。

安全框架是围绕实现 OAuth 协议的授权服务器而构建的，并公开客户机为了获取访问令牌而与之交互的 OAuth 端点。安全框架提供了构建块，用于基于授权服务器和底层 OAuth 协议实现定制授权逻辑。缺省情况下，MobileFirst 服务器还充当**授权服务器**。但是，可以配置 IBM WebSphere DataPower 设备来充当授权服务器并与 MobileFirst 服务器交互。

然后，客户机应用程序可以使用这些令牌来访问**资源服务器**上的资源，资源可以是 MobileFirst 服务器本身，也可以是外部服务器。资源服务器会检查令牌的有效性，以确保可以授予客户机对所请求资源的访问权。通过将资源服务器与授权服务器相分隔，对在 MobileFirst 服务器外部运行的资源强制实施安全性。

应用程序开发者通过为每个受保护资源定义所需的作用域，并实施安全性检查和质询处理程序来保护对其资源的访问。服务器端安全框架和客户机端 API 会透明地处理 OAuth 消息交换以及与授权服务器的交互，从而允许开发者仅关注授权逻辑。

## 授权实体
{: #acs_authorization_entitiesty}

### 访问令牌
{: #acs_access_tokens}

MobileFirst 访问令牌是一种数字签名实体，用于描述客户机的授权许可权。在授予客户机对特定作用域的授权请求并且认证客户机后，授权服务器的令牌端点会向客户机发送包含所请求访问令牌的 HTTP 响应。

#### 访问令牌的结构

MobileFirst 访问令牌包含以下信息：

* **客户机标识**：客户机的唯一标识。
* **作用域**：为其授予令牌的作用域（请参阅“OAuth 作用域”）。此作用域不包括必需的应用程序作用域。
* **令牌到期时间**：令牌变为无效（到期）的时间（以秒为单位）。

#### 令牌到期时间
{: #token-expiration}

授予的访问令牌会一直保持有效，直至达到其到期时间。访问令牌的到期时间设置为作用域内所有安全性检查的到期时间中最短的到期时间。但是，如果最短到期时间之前的时间段长于应用程序的最长令牌到期时间段，那么令牌的到期时间会设置为当前时间加上最长到期时间段。缺省最长令牌到期时间段（有效期）为 3,600 秒（1 小时），但可以通过设置 ``maxTokenExpiration`` 属性的值对其进行配置。

##### 配置最长访问令牌到期时间段
{: #acs_config-max-access-tokens}

使用以下其中一种替代方法来配置应用程序的最长访问令牌到期时间段：

* 使用 MobileFirst Operations Console
    1. 选择 **[您的应用程序]** → **安全** 选项卡。
    2. 在**令牌配置**部分中，将**最长令牌到期时间段（秒）**字段的值设置为您首选的值，然后单击**保存**。您可以随时重复此过程以更改最长令牌到期时间段，或选择**复原缺省值**以复原缺省值。

* 编辑应用程序的配置文件

    1. 在 CLI 中，导航至项目的根文件夹并运行以下命令。
      ```bash
      mfpdev app pull
      ```
    2. 打开位于 `[project-folder]\mobilefirst` 文件夹中的配置文件。
    3. 编辑该文件，以定义 `maxTokenExpiration` 属性并将其值设置为最长访问令牌到期时间段（秒）：
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. 通过运行以下命令部署更新的配置 JSON 文件：``mfpdev app push``。

##### 访问令牌响应结构
{: #acs_access-tokens-structure}

对访问令牌请求的成功 HTTP 响应包含具有访问令牌和其他数据的 JSON 对象。下面是来自授权服务器的有效令牌响应的示例：

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

令牌响应 JSON 对象具有以下属性对象：

* **token_type**：根据 [OAuth 2.0 Bearer Token Usage 规范](https://tools.ietf.org/html/rfc6750)，令牌类型始终为“*Bearer*”。
* **expires_in**：访问令牌的到期时间（秒）。
* **access_token**：生成的访问令牌（实际访问令牌的长度比示例中显示的要长）。
* **scope**：请求的作用域。

**expires_in** 和 **scope** 信息还包含在令牌 (**access_token**) 本身中。

**注**：如果使用低级别 `WLAuthorizationManager` 类并自行管理客户机与授权和资源服务器之间的 OAuth 交互，或者如果使用保密客户机，那么有效访问令牌响应的结构是相关的。如果使用的是高级别 `WLResourceRequest` 类，其中封装了用于访问受保护资源的 OAuth 流，那么安全框架会为您处理访问令牌响应。请参阅 [Client security APIs](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) 和 [Confidential clients](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/)。

### 刷新令牌
{: #acs_refresh_tokens}

刷新令牌是一种特殊类型的令牌，可用于在访问令牌到期时获取新的访问令牌。要请求新的访问令牌，可以提供有效的刷新令牌。刷新令牌是长期有效的令牌，与访问令牌相比，保持有效的持续时间更长。

应用程序必须谨慎使用刷新令牌，因为这种令牌可以允许用户永久保持已认证状态。社交媒体应用程序、电子商务应用程序、产品目录浏览之类的实用程序应用程序（其中应用程序提供者不会经常认证用户）可以使用刷新令牌。频繁要求用户认证的应用程序必须避免使用刷新令牌。
MobileFirst 刷新令牌

MobileFirst 刷新令牌是一种数字签名实体，类似于访问令牌，用于描述客户机的授权许可权。刷新令牌可用于获取相同作用域的新访问令牌。在授予客户机对特定作用域的授权请求并且认证客户机后，授权服务器的令牌端点会向客户机发送包含所请求访问令牌和刷新令牌的 HTTP 响应。访问令牌到期后，客户机会将刷新令牌发送到授权服务器的令牌端点，以获取一组新的访问令牌和刷新令牌。

#### 刷新令牌的结构
{: #structure_refresh_tokens}

与 MobileFirst 访问令牌类似，MobileFirst 访问令牌也包含以下信息：

* **客户机标识**：客户机的唯一标识。
* **作用域**：为其授予令牌的作用域（请参阅“OAuth 作用域”）。此作用域不包括必需的应用程序作用域。
* **令牌到期时间**：令牌变为无效（到期）的时间（以秒为单位）。

##### 令牌到期时间
{: #str-token-expiration}

刷新令牌的令牌到期时间段长于典型的访问令牌到期时间段。刷新令牌一旦授予后会一直保持有效，直至达到其到期时间。在此有效期内，客户机可以使用刷新令牌来获取一组新的访问令牌和刷新令牌。刷新令牌的到期时间段固定为 30 天。每次客户机成功收到一组新的访问令牌和刷新令牌时，刷新令牌到期时间都会重置，因此客户机的体验是令牌永不到期。访问令牌到期时间规则保持不变，如**访问令牌**部分中所述。

##### 启用刷新令牌功能
{: #acs_enable-refresh-token}

刷新令牌功能可以在客户机端和服务器端分别使用以下属性来启用。

客户机端属性 (Android)：
*文件名*：mfpclient.properties
*属性名称*：wlEnableRefreshToken
*属性值*：true
例如，
*wlEnableRefreshToken*=true

客户机端属性 (iOS)：
*文件名*：mfpclient.plist
*属性名称*：wlEnableRefreshToken
*属性值*：true
例如，
*wlEnableRefreshToken*=true

服务器端属性：
*文件名*：server.xml
*属性名称*：mfp.security.refreshtoken.enabled.apps
*属性值*：应用程序捆绑软件标识，用“;”分隔

例如，

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
对于不同的平台，请使用不同的捆绑软件标识。

##### 刷新令牌响应结构
{: #refresh-token-response-structure}

下面是来自授权服务器的有效刷新令牌响应的示例：

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

刷新令牌响应除了在访问令牌响应结构中说明的其他属性对象外，还有额外的属性对象 `refresh_token`。

**注**：与访问令牌相比，刷新令牌长期有效。因此，必须谨慎使用刷新令牌功能。不需要定期用户认证的应用程序非常适合使用刷新令牌功能。

从 CD 更新 3 开始，MobileFirst 在 iOS 上支持刷新令牌功能。

#### 安全性检查
{: #acs_securitychecks}

安全性检查是一种服务器端实体，用于实现保护服务器端应用程序资源的安全逻辑。安全性检查的简单示例是用户登录安全性检查，用于接收用户的凭证，并根据用户注册表来验证凭证。另一个示例是预定义的 MobileFirst 应用程序真实性安全检查，用于验证移动应用程序的真实性，从而防止非法尝试访问应用程序的资源。还可以使用同一安全性检查来保护多个资源。

安全性检查通常会发出安全质询，要求客户机以特定方式进行响应才能通过检查。此握手作为 OAuth 访问令牌获取流的一部分执行。客户机使用**质询处理程序**来处理来自安全性检查的质询。

##### 内置安全性检查
{: #builtin-sec-checks}

提供了以下预定义的安全性检查：

* 应用程序真实性
* 基于 LTPA 的单点登录 (SSO)
* Direct Update

#### 质询处理程序实体
{: #challengehandler_entity}

当您尝试访问受保护资源时，客户机可能会面临质询。质询是一个问题、一项安全性测试或一项服务器提示，用于确保允许客户机访问此资源。在最常见的情况下，此类质询是请求提供凭证，例如用户名和密码。

质询处理程序是一种客户机端实体，用于实现客户机端安全逻辑和相关用户交互。

**重要信息**：收到质询后，不能忽略不管。您必须应答或取消该质询。忽略质询可能会导致意外行为。

### 作用域
{: #scopes}

您可以通过向资源分配作用域，保护资源（例如，适配器）免遭未经授权的访问。

作用域定义为由一个或多个以空格分隔的 scope 元素构成的字符串（“scopeElement1 scopeElement2 ...”），或者定义为空值以应用缺省作用域 (RegisteredClient)。除非您对适配器资源禁用了资源保护，否则对于任何适配器资源，MobileFirst 安全框架都需要访问令牌，即便资源未分配有作用域也是如此。

#### scope 元素
{: #scopeelements}

scope 元素可以是以下其中一项：

* 安全性检查的名称。
* 任意关键字，如 `access-restricted` 或 `deletePrivilege`，用于定义此资源所需的安全性级别。此关键字稍后会映射到安全性检查。

#### 作用域映射
{: #scopemapping}

缺省情况下，在**作用域**中编写的 **scope 元素**会映射到同名的**安全性检查**。例如，如果编写名为 `PinCodeAttempts` 的安全性检查，那么可以在作用域中使用同名的 scope 元素。

作用域映射允许将 scope 元素映射到安全性检查。客户机要求 scope 元素时，此配置会定义需要应用哪些安全性检查。例如，可以将 scope 元素 `access-restricted` 映射到 `PinCodeAttempts` 安全性检查。

如果要根据哪个应用程序正在尝试访问资源而采用不同方式保护资源，那么作用域映射非常有用。您还可以将作用域映射到包含零个或更多个安全性检查的列表。

例如：scope = `access-restricted deletePrivilege`

* 在应用程序 A 中
    * `access-restricted` 映射到 `PinCodeAttempts`。
    * `deletePrivilege` 映射到空字符串。
* 在应用程序 B 中
    * `access-restricted` 映射到 `PinCodeAttempts`。
    * `deletePrivilege` 映射到 `UserLogin`。

要将 scope 元素映射到空字符串，请不要在**添加新 scope 元素映射**弹出菜单中选择任何安全性检查。

![作用域映射](/images/scope_mapping.png "添加新的 scope 元素映射屏幕")

您还可以使用必需的配置来手动编辑应用程序的配置 JSON 文件，然后将更改推送回 MobileFirst 服务器。

1. 在**命令行窗口**中，导航至项目的根文件夹并运行 `mfpdev app pull`。
2. 打开位于 `[project-folder]\mobilefirst` 文件夹中的配置文件。
3. 编辑该文件以定义 `scopeElementMapping` 属性。在此属性中，定义数据对，每个数据对包含所选 scope 元素的名称，以及该元素映射到的由零个或更多个以空格分隔的安全性检查构成的字符串。例如：

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. 通过运行以下命令部署更新的配置 JSON 文件：
  ```bash
mfpdev app push
```

您还可以将更新的配置推送到远程服务器。请参阅“使用 MobileFirst CLI 管理 MobileFirst 工件”教程。
{: note}

### 保护资源
{: #protecting-resources}

在 OAuth 模型中，受保护资源是需要访问令牌的资源。可以使用 MobileFirst 安全框架来保护在 MobileFirst 服务器实例上托管的资源以及外部服务器上的资源。通过为资源分配作用域（用于定义获取针对资源的访问令牌所需的许可权）来保护资源。

可以通过各种方式来保护资源：

#### 必需的应用程序作用域
{: #mandatoryappscope}

在应用程序级别，可以定义将应用于应用程序使用的所有资源的作用域。除了对所请求资源作用域的安全性检查外，安全框架还会运行这些检查（如果存在）。

**注**：
   * 在访问不受保护的资源时，不会应用必需的应用程序作用域。
   * 针对资源作用域授予的访问令牌不包含必需的应用程序作用域。

在 MobileFirst Operations Console 中，从导航侧边栏的**应用程序**部分中选择应用程序，然后选择**安全性**选项卡。在**必需的应用程序作用域**下，选择**添加到作用域**。

![必需的应用程序作用域](/images/mandatory-application-scope.png "配置必需的应用程序作用域屏幕")

您还可以使用必需的配置来手动编辑应用程序的配置 JSON 文件，然后将更改推送回 MobileFirst 服务器。

1. 在**命令行窗口**中，导航至项目的根文件夹并运行 `mfpdev app pull`。
2. 打开位于 **project-folder\mobilefirst** 文件夹中的配置文件。
3. 编辑该文件以定义 `mandatoryScope` 属性，并将该属性值设置为包含所选 scope 元素的空格分隔列表的作用域字符串。例如，

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. 通过运行以下命令部署更新的配置 JSON 文件：mfpdev app push。

您还可以将更新的配置推送到远程服务器。

#### 保护适配器资源
{: #protectadapterres}

在适配器中，可以为 Java 方法或 JavaScript 资源过程指定保护作用域，也可以为整个 Java 资源类指定保护作用域。作用域定义为由一个或多个以空格分隔的 scope 元素构成的字符串（“scopeElement1 scopeElement2 ...”），或者定义为空值以应用缺省作用域。有关保护适配器资源的更多信息，请参阅[保护适配器](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters)。

### 禁用资源保护
{: #disablingresprotection}

您可以对特定 Java 或 JavaScript 适配器资源禁用缺省 MobileFirst 资源保护，也可以对整个 Java 类禁用缺省 MobileFirst 资源保护，如以下 Java 和 JavaScript 部分中所概述。禁用资源保护后，MobileFirst 安全框架不需要令牌来访问资源。

#### 禁用 Java 资源 OAuth 保护
{: #disablejavaresoauthprotection}

要完全禁用对某个 Java 资源方法或类的 OAuth 保护，请将 `@OAuthSecurity` 注释添加到资源或类声明，并将 `enabled` 元素的值设置为 `false`：

```
@OAuthSecurity(enabled = false)
```

注释的 `enabled` 元素的缺省值为 `true`。如果将 `enabled` 元素设置为 `false`，那么会忽略 `scope` 元素，因此资源或资源类将不受保护。

**注**：在向不受保护的类的方法分配作用域后，该方法将受到保护，而不论类注释是何内容，除非您将资源注释的 `enabled` 元素设置为 `false`。

查看以下示例：

以下代码禁用对 `helloUser` 方法的资源保护：

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

以下代码禁用对 `MyUnprotectedResources` 类的资源保护：

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### 禁用 JavaScript 资源 OAuth 保护
{: #disablejavascriptresoauthprotection}

要完全禁用对某个 JavaScript 适配器资源（过程）的 OAuth 保护，请在 **adapter.xml** 文件中将 <procedure> 元素的 `secured` 属性设置为 `false`：

```javascript
<procedure name="procedureName" secured="false">
```

如果将 `secured` 属性设置为 `false`，那么会忽略 `scope` 属性，因此资源将不受保护。

查看以下示例：

以下代码禁用对 `userName` 过程的资源保护：

```javascript
<procedure name="userName" secured="false">
```

### 定义不受保护的资源
{: #defunprotectedresources}

不受保护的资源是不需要访问令牌的资源。MobileFirst 安全框架不会管理对不受保护资源的访问，也不会验证或检查访问这些资源的客户机的身份。因此，不受保护的资源不支持 Direct Update、阻止设备访问或远程禁用应用程序等功能。

### 保护外部资源
{: #protecextresources}

要保护外部资源，请将具有访问令牌验证模块的资源过滤器添加到外部资源服务器。令牌验证模块在授予 OAuth 客户机对资源的访问权之前，会使用安全框架的授权服务器的自省端点来验证 MobileFirst 访问令牌。可以将 MobileFirst REST API 用于 MobileFirst 运行时，以针对任何外部服务器创建您自己的访问令牌验证模块。或者，使用提供的其中一个 MobileFirst 扩展来保护外部 Java 资源。
