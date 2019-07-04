---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, adapter security

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

# 保护适配器
{: #protecting_adapters}

在适配器中，可以为 Java 方法或 JavaScript 资源过程指定保护作用域，也可以为整个 Java 资源类指定保护作用域，如以下 [Java](#protect-java-adapter-resources) 和 [JavaScript](#protect-javascript-adapter-resources) 部分中所概述。作用域定义为由一个或多个以空格分隔的 scope 元素构成的字符串（“scopeElement1 scopeElement2 ...”），或者定义为空值以应用缺省作用域。

缺省 MobileFirst 作用域为 `RegisteredClient`，需要访问令牌才能访问资源；缺省作用域会验证资源请求是否来自向 MobileFirst 服务器注册的应用程序。除非[禁用资源保护](#disabling-resource-protection)，否则始终会应用此保护措施。因此，即使没有为资源设置作用域，资源也仍受保护。

`RegisteredClient` 是保留的 MobileFirst 关键字。请勿使用此名称定义定制的 scope 元素或安全性检查。
{: note}

## 保护 Java 适配器资源
{: #protect-java-adapter-resources}

要向 JAX-RS 方法或类分配保护作用域，请向该方法或类声明添加 `@OAuthSecurity` 注释，并将注释的 `scope` 元素设置为首选作用域。将 `YOUR_SCOPE` 替换为由一个或多个 scope 元素构成的字符串（“scopeElement1 scopeElement2 ...”）：

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

类作用域适用于类中的所有方法，但自身具有 `@OAuthSecurity` 注释的方法除外。

将 `@OAuthSecurity` 注释的 enabled 元素设置为 `false` 时，将忽略 scope 元素。请参阅[禁用 Java 资源保护](#disabling-java-resource-protection)。
{: note}

### 示例
{: #protect-java-adap-res-example}
以下代码使用包含 `UserAuthentication` 和 `Pincode` scope 元素的作用域来保护 `helloUser` 方法：

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

以下代码使用预定义的 `LtpaBasedSSO` 安全性检查来保护 `WebSphereResources` 类：

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

## 保护 JavaScript 适配器资源
{: #protect-javascript-adapter-resources}

要向 JavaScript 过程分配保护作用域，请在 **adapter.xml** 文件中将 <procedure> 元素的 scope 属性设置为首选作用域。将 `PROCEDURE_NANE` 替换为过程的名称，将 `YOUR SCOPE` 替换为由一个或多个 scope 元素构成的字符串（“scopeElement1 scopeElement2 ...”）：

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

将 <procedure> 元素的 `secured` 属性设置为 false 时，将忽略 `scope` 属性。请参阅[禁用 JavaScript 资源保护](#disabling-javascript-resource-protection)。
{: note}

### 示例
{: #protect-javascript-adap-res-example}

以下代码使用包含 `UserAuthentication` 和 `Pincode` scope 元素的作用域来保护 `userName` 过程：

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## 禁用适配器资源保护
{: #disabling-adapter-resource-protection}

您可以对特定 Java 或 JavaScript 适配器资源禁用[缺省 MobileFirst 资源保护](#protecting_adapters_resources)，也可以对整个 Java 类禁用缺省 MobileFirst 资源保护，如以下 Java 和 JavaScript 部分中所概述。禁用资源保护后，MobileFirst 安全框架不需要令牌来访问资源。

### 禁用 Java 资源保护
{: #disabling-java-resource-protection}

要完全禁用对某个 Java 资源方法或类的 OAuth 保护，请将 `@OAuthSecurity` 注释添加到资源或类声明，并将 `enabled` 元素的值设置为 `false`：

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

注释的 `enabled` 元素的缺省值为 `true`。如果将 `enabled` 元素设置为 `false`，那么会忽略 `scope` 元素，因此资源或资源类将不受保护。

在向不受保护的类的方法分配作用域后，该方法将受到保护，而不论类注释是何内容，除非您将资源注释的 `enabled` 元素设置为 `false`。
{: note}

#### 示例
{: #disble-java-adap-res-example}

以下代码禁用对 `helloUser` 方法的资源保护：

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

以下代码禁用对 `MyUnprotectedResources` 类的资源保护：

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### 禁用 JavaScript 资源保护
{: #disabling-javascript-resource-protection}

要完全禁用对某个 JavaScript 适配器资源（过程）的 OAuth 保护，请在 **adapter.xml** 文件中将 <procedure> 元素的 `secured` 属性设置为 `false`：

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

如果将 `secured` 属性设置为 `false`，那么会忽略 `scope` 属性，因此资源将不受保护。

#### 示例
{: #disable-javascript-res-prot-example}

以下代码禁用对 `userName` 过程的资源保护：

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## 不受保护的资源
{: #unprotected-resources}

不受保护的资源是不需要访问令牌的资源。MobileFirst 安全框架不会管理对不受保护资源的访问，也不会验证或检查访问这些资源的客户机的身份。因此，不受保护的资源不支持 Direct Update、阻止设备访问或远程禁用应用程序等功能。
