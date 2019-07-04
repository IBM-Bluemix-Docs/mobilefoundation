---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, authentication, using challenge handlers

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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# 设置认证
{: #step_up_authentication}

资源可以通过若干安全性检查进行保护。在此类场景中，MobileFirst 服务器会将所有相关质询同时发送给应用程序。

一个安全性检查还可能从属于另一个安全性检查。因此，务必确保能够控制何时发送质询。

例如，本教程描述了一个应用程序，该应用程序有两个通过用户名和密码保护的资源，其中第二个资源还需要额外的 PIN 码。

## 引用安全性检查
{: #referencing-a-security-check}

创建两个安全性检查：`StepUpPinCode` 和 `StepUpUserLogin`。

在此示例中，`StepUpPinCode` 从属于 `StepUpUserLogin`。用户成功登录到 `StepUpUserLogin` 后，系统应该会要求用户输入 PIN 码。出于此目的，`StepUpPinCode` 必须能够引用 `StepUpUserLogin` 类。

MobileFirst 框架提供了用于注入引用的注释。

在 `StepUpPinCode` 类中，在类级别添加以下内容：

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

**重要信息**：两个安全性检查实现都需要捆绑在同一适配器中。{.important}

为了解析此引用，框架会查找具有相应类的安全性检查，并将其引用注入到从属安全性检查中。

如果存在相同类的多项安全性检查，那么注释具有可选的 name 参数，可用于指定所引用检查的唯一名称。

## 状态机
{: #state-machine}

所有扩展 `CredentialsValidationSecurityCheck`（包括 `StepUpPinCode` 和 `StepUpUserLogin`）的类都会继承一个简单状态机。在任何给定时刻，安全性检查都可能处于以下其中一个状态：

* `STATE_ATTEMPTING`：质询已发送，并且安全性检查正在等待客户机响应。在此状态期间，将维护尝试计数。
* `STATE_SUCCESS`：已成功验证凭证。
* `STATE_BLOCKED`：已达到最大尝试次数，并且检查处于锁定状态。

可以使用继承的 `getState()` 方法来获取当前状态。

在 `StepUpUserLogin` 中，添加一种方便的方法来检查用户当前是否已登录。在教程稍后部分将使用此方法。

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## Authorize 方法
{: #the-authorize-method}

`SecurityCheck` 接口定义了名为 `authorize` 的方法。此方法负责实现安全性检查的主逻辑，例如发送质询或验证请求。

`StepUpPinCode` 扩展的 `CredentialsValidationSecurityCheck` 类已包含此方法的实现。但是，在本例中，目标是在开始 `authorize` 方法的缺省行为之前检查 StepUpUserLogin 的状态。

为此，请**覆盖** `authorize` 方法：

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

此实现会检查 `StepUpUserLogin` 引用的当前状态：

* 如果状态为 `STATE_SUCCESS`（用户已登录），那么将继续执行安全性检查的正常流程。
* 如果 `StepUpUserLogin` 处于其他任何状态，那么不会执行任何操作：不会发送质询（不管成功还是失败）。

假定资源**同时**通过 `StepUpPinCode` 和 `StepUpUserLogin` 进行保护，那么此流程将确保用户已登录后，系统才提示输入辅助凭证（PIN 码）。客户机绝不会同时接收这两个质询，尽管这两个安全性检查都已激活。

或者，如果资源**仅**通过 `StepUpPinCode` 进行保护（框架将仅激活此安全性检查），那么可以更改 `authorize` 实现以手动触发 `StepUpUserLogin`：

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        //If StepUpUserLogin is successful, continue the normal processing of StepUpPinCode
        super.authorize(scope, credentials, request, response);
    } else {
        //In any other case, process StepUpUserLogin instead.
        userLogin.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

## 检索当前用户
{: #retrieve-current-user}

在 `StepUpPinCode` 安全性检查中，您有兴趣了解当前用户的标识，以便您可以在某个数据库中查找此用户的 PIN 码。

在 `StepUpUserLogin` 安全性检查中，添加以下方法以从**授权上下文**中获取当前用户：

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

然后，在 `StepUpPinCode` 中，可以使用 `userLogin.getUser()` 方法从 `StepUpUserLogin` 安全性检查中获取当前用户，并检查此特定用户的有效 PIN 码：

```java
@Override
protected boolean validateCredentials(Map<String, Object> credentials) {
    //Get the correct PIN code from the database
    User user = userManager.getUser(userLogin.getUser().getId());

    if(credentials!=null && credentials.containsKey(PINCODE_FIELD)){
        String pinCode = credentials.get(PINCODE_FIELD).toString();

        if(pinCode.equals(user.getPinCode())){
            errorMsg = null;
            return true;
        }
        else{
            errorMsg = "Wrong credentials. Hint: " + user.getPinCode();
        }
    }
    return false;
}
```
{: codeblock}

## 质询处理程序
{: #challenge-handlers}

在客户机端，没有特殊的 API 来处理多个步骤。相反，每个质询处理程序都处理其自己的质询。在此示例中，必须注册两个单独的质询处理程序：一个用于处理来自 `StepUpUserLogin` 的质询，一个用于处理来自 `StepUpPincode` 的质询。
