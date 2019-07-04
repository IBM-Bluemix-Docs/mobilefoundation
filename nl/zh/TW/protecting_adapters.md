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

# 保護配接器
{: #protecting_adapters}

在您的配接器中，您可以針對 Java 方法或 JavaScript 資源程序或是整個 Java 資源類別指定保護範圍，如下列 [Java](#protect-java-adapter-resources) 及 [JavaScript](#protect-javascript-adapter-resources) 小節所述。範圍定義為一個以上以空格區隔之範圍元素 ("scopeElement1 scopeElement2...") 的字串，或定義為空值以套用預設範圍。

預設 MobileFirst 範圍是 `RegisteredClient`（這需要用於存取資源的存取記號），並驗證資源要求來自向 MobileFirst Server 登錄的應用程式。除非您[停用資源保護](#disabling-resource-protection)，否則一律會套用此保護。因此，即使您未設定資源的範圍，資源還是會受到保護。

`RegisteredClient` 是保留 MobileFirst 關鍵字。請不要依此名稱來定義自訂範圍元素或安全檢查。
{: note}

## 保護 Java 配接器資源
{: #protect-java-adapter-resources}

若要將保護範圍指派給 JAX-RS 方法或類別，請將 `@OAuthSecurity` 註釋新增至方法或類別宣告，以及將註釋的 `scope` 元素設為偏好的範圍。將 `YOUR_SCOPE` 取代為一個以上範圍元素的字串 ("scopeElement1 scopeElement2 ...")：

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

類別範圍會套用至類別中的所有方法，但具有其專屬 `@OAuthSecurity` 註釋的方法除外。

將 `@OAuthSecurity` 註釋的 enabled 元素設定為 `false` 時，將忽略 scope 元素。請參閱[停用 Java 資源保護](#disabling-java-resource-protection)。
{: note}

### 範例
{: #protect-java-adap-res-example}
下列程式碼會保護範圍包含 `UserAuthentication` 及 `Pincode` scope 元素的 `helloUser` 方法：

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

下列程式碼會保護具有預先定義 `LtpaBasedSSO` 安全檢查的 `WebSphereResources` 類別：

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

## 保護 JavaScript 配接器資源
{: #protect-javascript-adapter-resources}

若要將保護範圍指派給 JavaScript 程序，請在 **adapter.xml** 檔案中，將 <procedure> 元素的 scope 屬性設為偏好的範圍。將 `PROCEDURE_NANE` 取代為您程序的名稱，並將 `YOUR SCOPE` 取代為一個以上範圍元素的字串 ("scopeElement1 scopeElement2 ...")：

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

將 <procedure> 元素的 `secured` 屬性設定為 false 時，將忽略 `scope` 屬性。請參閱[停用 JavaScript 資源保護](#disabling-javascript-resource-protection)。
{: note}

### 範例
{: #protect-javascript-adap-res-example}

下列程式碼會保護範圍包含 `UserAuthentication` 及 `Pincode` 範圍元素的 `userName` 程序：

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## 停用配接器資源保護
{: #disabling-adapter-resource-protection}

您可以針對特定 Java 或 JavaScript 配接器資源或是針對整個 Java 類別，停用[預設 MobileFirst 資源保護](#protecting_adapters_resources)，如下列 Java 及 JavaScript 小節所述。停用資源保護時，MobileFirst 安全架構不需要記號，即可存取資源。

### 停用 Java 資源保護
{: #disabling-java-resource-protection}

若要整個停用 Java 資源方法或類別的 OAuth 保護，請將 `@OAuthSecurity` 註釋新增至資源或類別宣告，以及將 `enabled` 元素的值設為 `false`：

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

註釋之 `enabled` 元素的預設值為 `true`。將 `enabled` 元素設為 `false` 時，會忽略 `scope` 元素，而且會解除保護資源或資源類別。

當您將範圍指派給未受保護類別的方法時，儘管有此類別註釋，還是會保護該方法，除非您將資源註釋的 `enabled` 元素設定為 `false`。
{: note}

#### 範例
{: #disble-java-adap-res-example}

下列程式碼會停用 `helloUser` 方法的資源保護：

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

下列程式碼會停用 `MyUnprotectedResources` 類別的資源保護：

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### 停用 JavaScript 資源保護
{: #disabling-javascript-resource-protection}

若要整個停用 JavaScript 配接器資源（程序）的 OAuth 保護，請在 **adapter.xml** 檔案中，將 <procedure> 元素的 `secured` 屬性設為 `false`：

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

將 `secured` 屬性設為 `false` 時，會忽略 `scope` 屬性，而且會解除保護資源。

#### 範例
{: #disable-javascript-res-prot-example}

下列程式碼會停用 `userName` 程序的資源保護：

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## 未受保護的資源
{: #unprotected-resources}

未受保護的資源是不需要存取記號的資源。MobileFirst 安全架構不會管理對未受保護資源的存取權，而且不會驗證或檢查可存取這些資源之用戶端的身分。因此，不支援未受保護資源的「直接更新」、封鎖裝置存取或遠端停用應用程式這類特性。
