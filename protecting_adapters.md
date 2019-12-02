---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: mobile foundation security, adapter security

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
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

# Protecting adapters
{: #protecting_adapters}

In your adapter, you can specify the protecting scope for a Java&trade; method or JavaScript resource procedure, or for an entire Java&trade; resource class, as outlined in the following [Java](#protect-java-adapter-resources) and [JavaScript](#protect-javascript-adapter-resources) sections. A scope is defined as a string of one or more space-separated scope elements (“scopeElement1 scopeElement2 …”), or null to apply the default scope.

The default {{site.data.keyword.mobilefoundation_short}} scope is `RegisteredClient`, which require an access token for accessing the resource and verifies that the resource request is from an application that is registered with {{site.data.keyword.mobilefoundation_short}} Server. This protection is always applied, unless you [disable resource protection](#disabling-resource-protection). Therefore, even if you do not set a scope for your resource, it is still protected.

`RegisteredClient` is a reserved {{site.data.keyword.mobilefoundation_short}} keyword. Do not define custom scope elements or security checks by this name.
{: note}

## Protecting Java&trade; adapter resources
{: #protect-java-adapter-resources}

To assign a protecting scope to a JAX-RS method or class, add the `@OAuthSecurity` annotation to the method or class declaration, and set the `scope` element of the annotation to your preferred scope. Replace `YOUR_SCOPE` with a string of one or more scope elements (“scopeElement1 scopeElement2 …”):

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

A class scope applies to all of the methods in the class, except for methods that have their own `@OAuthSecurity` annotation.

When the enabled element of the `@OAuthSecurity` annotation is set to `false`, the scope element is ignored. See [Disabling Java resource protection](#disabling-java-resource-protection).
{: note}

### Examples
{: #protect-java-adap-res-example}
The following code protects a `helloUser` method with a scope that contains `UserAuthentication` and `Pincode` scope elements:

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

The following code protects a `WebSphereResources` class with the predefined `LtpaBasedSSO` security check:

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

## Protecting JavaScript adapter resources
{: #protect-javascript-adapter-resources}

To assign a protecting scope to a JavaScript procedure, in the **adapter.xml** file, set the scope attribute of the `<procedure>` element to your preferred scope. Replace `PROCEDURE_NANE` with the name of your procedure, and `YOUR SCOPE` with a string of one or more scope elements (“scopeElement1 scopeElement2 …”):

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

When the `secured` attribute of the `<procedure>` element is set to false, the `scope` attribute is ignored. See [Disabling JavaScript resource protection](#disabling-javascript-resource-protection).
{: note}

### Example
{: #protect-javascript-adap-res-example}

The following code protects a `userName` procedure with a scope that contains `UserAuthentication` and `Pincode` scope elements:

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## Disabling adapter resource protection
{: #disabling-adapter-resource-protection}

You can disable the [default MobileFirst resource protection](#protecting_adapters_resources) for a specific Java&trade; or JavaScript adapter resource, or for an entire Java&trade; class, as outlined in the following Java and JavaScript sections. When resource protection is disabled, the MobileFirst security framework does not require a token to access the resource.

### Disabling Java resource protection
{: #disabling-java-resource-protection}

To entirely disable OAuth protection for a Java resource method or class, add the `@OAuthSecurity` annotation to the resource or class declaration, and set the value of the `enabled` element to `false`:

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

The default value of the annotation’s `enabled` element is `true`. When the `enabled` element is set to `false`, the `scope` element is ignored and the resource or resource class is unprotected.

When you assign a scope to a method of an unprotected class, the method is protected despite the class annotation, unless you set the `enabled` element of the resource annotation to `false`.
{: note}

#### Examples
{: #disble-java-adap-res-example}

The following code disables resource protection for a `helloUser` method:

```java
@GET
@Path("/{username}")
@OAuthSecurity(enabled = "false")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

The following code disables resource protection for a `MyUnprotectedResources` class:

```java
@Path("/users")
@OAuthSecurity(enabled = "false")
public class MyUnprotectedResources {
    ...
}
```
{: codeblock}

### Disabling JavaScript resource protection
{: #disabling-javascript-resource-protection}

To entirely disable OAuth protection for a JavaScript adapter resource (procedure), in the **adapter.xml** file, set the `secured` attribute of the `<procedure>` element to `false`:

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

When the `secured` attribute is set to `false`, the `scope` attribute is ignored and the resource is unprotected.

#### Example
{: #disable-javascript-res-prot-example}

The following code disables resource protection for a `userName` procedure:

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## Unprotected resources
{: #unprotected-resources}

An unprotected resource is a resource that does not require an access token. The MobileFirst security framework does not manage access to unprotected resources, and does not validate or check the identity of clients that access these resources. Therefore, features such as Direct Update, blocking device access, or remotely disabling an application, are not supported for unprotected resources.
