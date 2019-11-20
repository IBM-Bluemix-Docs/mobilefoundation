---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-15"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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

# Authentication and Security
{: #basic_authentication}

The {{site.data.keyword.mobilefoundation_short}} security framework is based on the [OAuth 2.0](http://oauth.net/) protocol. According to this protocol, a resource can be protected by a **scope** that defines the required permissions for accessing the resource. To access a protected resource, the client must provide a matching **access token**, which encapsulates the scope of the authorization that is granted to the client.

The OAuth protocol separates the roles of the authorization server and the resource server on which the resource is hosted.

* The authorization server manages the client authorization and token generation.
* The resource server uses the authorization server to validate the access token that is provided by the client, and ensure that it matches the protecting scope of the requested resource.

The security framework is built around an authorization server that implements the OAuth protocol, and exposes the OAuth endpoints with which the client interacts to obtain access tokens. The security framework provides the building blocks for implementing a custom authorization logic over the authorization server and the underlying OAuth protocol. By default, {{site.data.keyword.mobilefoundation_short}} Server functions also as the **authorization server**. However, you can configure an IBM WebSphere DataPower appliance to act as the authorization server and interact with {{site.data.keyword.mobilefoundation_short}} Server.

The client application can then use these tokens to access resources on a **resource server**, which can be either the {{site.data.keyword.mobilefoundation_short}} Server itself or an external server. The resource server checks the validity of the token to make sure that the client can be granted access to the requested resource. The separation between resource server and authorization server enforces security on resources that are running outside {{site.data.keyword.mobilefoundation_short}} Server.

Application developers protect access to their resources by defining the required scope for each protected resource, and implementing security checks and challenge handlers. The server-side security framework and the client-side API handle the OAuth message exchange and the interaction with the authorization server transparently, allowing developers to focus only on the authorization logic.

## Authorization entities
{: #acs_authorization_entitiesty}

### Access Tokens
{: #acs_access_tokens}

A {{site.data.keyword.mobilefoundation_short}} access token is a digitally signed entity that describes the authorization permissions of a client. After the client’s authorization request for a specific scope is granted, and the client is authenticated, the authorization server’s token endpoint sends the client an HTTP response that contains the requested access token.

#### Structure of access token

The MobileFirst access token contains the following information:

* **Client ID**: a unique identifier of the client.
* **Scope**: the scope for which the token was granted (see OAuth scopes). This scope does not include the mandatory application scope.
* **Token-expiration time**: the time at which the token becomes invalid (expires), in seconds.

#### Token expiration
{: #token-expiration}

The granted access token remains valid until its expiration time elapses. The access token’s expiration time is set to the shortest expiration time from among the expiration times of all the security checks in the scope. But if the period until the shortest expiration time is longer than the application’s maximum token-expiration period, the token’s expiration time is set to the current time plus the maximum expiration period. The default maximum token-expiration period (validity duration) is 3,600 seconds (1 hour), but it can be configured by setting the value of the ``maxTokenExpiration`` property.

##### Configuring the maximum access-token expiration period
{: #acs_config-max-access-tokens}

Configure the application’s maximum access-token expiration period by using one of the following alternative methods:

* Using the {{site.data.keyword.mobilefoundation_short}} Operations Console
   1. Select **[your application]** → **Security** tab.
   1. In the **Token Configuration** section, set the value of the Maximum **Token-Expiration Period (seconds)** field to your preferred value, and click **Save**. You can repeat this procedure, at any time, to change the maximum token-expiration period, or select **Restore Default Values** to restore the default value.

* Editing the application's configuration file
   1. From a CLI, navigate to the project's root folder and run the following command.

      ```bash
      mfpdev app pull
      ```

   1. Open the configuration file, which is located in the `[project-folder]\mobilefirst` folder.
   1. Edit the file by defining a `maxTokenExpiration` property and set its value to the maximum access-token expiration period, in seconds:
 
      ```java
      {
          ...
          "maxTokenExpiration": 7200
      }
      ```
      {: codeblock}

   1. Deploy the updated configuration JSON file by running the command: ``mfpdev app push``.

##### Access-token response structure
{: #acs_access-tokens-structure}

A successful HTTP response to an access-token request contains a JSON object with the access token and extra data. Following is an example of a valid-token response from the authorization server:

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

The token-response JSON object has these property objects:

* **token_type**: the token type is always "*Bearer*", in accordance with the [OAuth 2.0 Bearer Token Usage specification](https://tools.ietf.org/html/rfc6750){: external}.
* **expires_in**: the expiration time of the access token, in seconds.
* **access_token**: the generated access token (actual access tokens are longer than shown in the example).
* **scope**: the requested scope.

The **expires_in** and **scope** information is also contained within the token itself (**access_token**).

The structure of a valid access-token response is relevant if you use the low level `WLAuthorizationManager` class and manage the OAuth interaction between the client and the authorization and resource servers yourself, or if you use a confidential client. If you are using the high level `WLResourceRequest` class, which encapsulates the OAuth flow for accessing protected resources, the security framework handles the processing of access-token responses for you. See [Client security APIs](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) and [Confidential clients](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
{: note}

### Refresh Tokens
{: #acs_refresh_tokens}

A Refresh token is a special type of token, which can be used to obtain a new access token when the access token expires. To request a new access token, a valid refresh token can be presented. The refresh tokens are long-lived tokens and remain valid for a longer duration of time compared to access tokens.

Refresh token must be used with caution by an application because they can allow a user to remain authenticated forever. Social media applications, e-commerce applications, product catalog browsing, and such utility applications, where the application provider does not authenticate users regularly can use refresh tokens. Applications that mandate user authentication frequently must avoid the use of refresh tokens.

#### {{site.data.keyword.mobilefoundation_short}} Refresh Token

A {{site.data.keyword.mobilefoundation_short}} refresh token is a digitally signed entity like access token that describes the authorization permissions of a client. Refresh token can be used to get new access token of the same scope. After the client’s authorization request for a specific scope is granted and the client is authenticated, the authorization server’s token endpoint sends the client an HTTP response that contains the requested access token and refresh token. When the access token expires, the client sends refresh token to authorization server’s token endpoint to get a new set of access token and refresh token.

#### Structure of refresh token
{: #structure_refresh_tokens}

Similar to the {{site.data.keyword.mobilefoundation_short}} access token, the {{site.data.keyword.mobilefoundation_short}} refresh token contains the following information:

* **Client ID**: a unique identifier of the client.
* **Scope**: the scope for which the token was granted (see OAuth scopes). This scope does not include the mandatory application scope.
* **Token-expiration time**: the time at which the token becomes invalid (expires), in seconds.

##### Token Expiration
{: #str-token-expiration}

The token expiration period for refresh token is longer than the typical access token expiry period. The refresh token that is once granted remains valid until its expiration time elapses. Within this validity period, a client can use the refresh token to get a new set of access token and refresh token. The refresh token has a fixed expiry period of 30 days. Every time the client receives a new set of access token and refresh token successfully, the refresh token expiry is reset thus giving the client an experience of a never expiring token. The access token expiry rules remain same as explained in section **Access Token**.

##### Enabling Refresh Token feature
{: #acs_enable-refresh-token}

Refresh token feature can be enabled by using the following properties on client side and server side respectively.

```
Client-side property (Android):
*File name*: mfpclient.properties
*Property name*: wlEnableRefreshToken
*Property value*: true
For example,
*wlEnableRefreshToken*=true

Client-side property (iOS): 
*File name*: mfpclient.plist
*Property name*: wlEnableRefreshToken
*Property value*: true
For example,
*wlEnableRefreshToken*=true

Server-side property:
*File name*: server.xml
*Property name*: mfp.security.refreshtoken.enabled.apps
*Property value*: application bundle id separated by ‘;’
```
{: screen}

For example,

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}

Use different bundle ids for different platforms.

##### Refresh token response structure
{: #refresh-token-response-structure}

Following is an example of a valid refresh token response from the authorization server:

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

The refresh token response has an additional property object `refresh_token` apart from the other property objects that is explained as part of the access token response structure.

The refresh tokens are long-lived compared to access tokens. Hence the refresh token feature must be used with caution. Applications where periodic user authentication is not necessary are ideal candidates for using the refresh token feature.
{: note}

MobileFirst supports refresh token feature on iOS starting CD Update 3.

#### Security Checks
{: #acs_securitychecks}

A security check is a server-side entity that implements the security logic for protecting server-side application resources. A simple example of a security check is a user-login security check that receives the credentials of a user, and verifies the credentials against a user registry. Another example is the predefined MobileFirst application-authenticity security check, which validates the authenticity of the mobile application and thus protects against unlawful attempts to access the application’s resources. The same security check can also be used to protect several resources.

A security check typically issues security challenges that require the client to respond in a specific way to pass the check. This handshake occurs as part of the OAuth access-token-acquisition flow. The client uses **challenge handlers** to handle challenges from security checks.

##### Built-in Security Checks
{: #builtin-sec-checks}

The following predefined security checks are available:

* Application Authenticity
* LTPA-based single sign-on (SSO)
* Direct Update

#### Challenge Handler entity
{: #challengehandler_entity}

When you try to access a protected resource, the client might be faced with a challenge. A challenge is a question, a security test, a prompt by the server to make sure that the client is allowed to access this resource. Most commonly, this challenge is a request for credentials, such as a user name and password.

A challenge handler is a client-side entity that implements the client-side security logic and the related user interaction.

After a challenge is received, it cannot be ignored. You must answer or cancel it. Ignoring a challenge might lead to unexpected behavior.
{: important}

### Scopes
{: #scopes}

You can protect resources, such as adapters, from unauthorized access by assigning a scope to the resource.

A scope is defined as a string of one or more space-separated scope elements (`scopeElement1 scopeElement2 …`), or null to apply the default scope (`RegisteredClient`). The {{site.data.keyword.mobilefoundation_short}} security framework requires an access token for any adapter resource, even if the resource is not assigned a scope, unless you disable resource protection for the resource.

#### Scope Elements
{: #scopeelements}

A scope element can be one of the following,

* The name of a security check.
* An arbitrary keyword such as `access-restricted` or `deletePrivilege`, which defines the level of security that is needed for this resource. This keyword is later mapped to a security check.

#### Scope Mapping
{: #scopemapping}

By default, the **scope elements** you write in your **scope** are mapped to a **security check** with the same name. For example, if you write a security check that is called `PinCodeAttempts`, you can use a scope element with the same name within your scope.

Scope Mapping allows mapping scope elements to security checks. When the client asks for a scope element, this configuration defines which security checks are required to be applied. For example, you can map the scope element `access-restricted` to your `PinCodeAttempts` security check.

Scope mapping is useful if you want to protect a resource differently depending on which application is trying to access it. You can also map a scope to a list of zero or more security checks.

For example: scope = `access-restricted deletePrivilege`

* In app A
   * `access-restricted` is mapped to `PinCodeAttempts`.
   * `deletePrivilege` is mapped to an empty string.
* In app B
   * `access-restricted` is mapped to `PinCodeAttempts`.
   * `deletePrivilege` is mapped to `UserLogin`.

To map your scope element to an empty string, do not select any security check in the **Add New Scope Element Mapping** pop-up menu.

![Scope Mapping](/images/scope_mapping.png "Add new scope element mapping screen")

You can also manually edit the application’s configuration JSON file with the required configuration and push the changes back to a {{site.data.keyword.mobilefoundation_short}} Server.

1. From a **command-line window**, navigate to the project’s root folder and run the `mfpdev app pull`.
1. Open the configuration file, which is located in the `[project-folder]\mobilefirst` folder.
1. Edit the file by defining a `scopeElementMapping` property. In this property, define data pairs that are each composed of the name of your selected scope element, and a string of zero or more space-separated security checks to which the element maps. For example:

   ```java
   "scopeElementMapping": {
       "UserAuth": "UserAuthentication",
       "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
   }
   ```
   {: codeblock}

1. Deploy the updated configuration JSON file by running the following command,

   ```bash
   mfpdev app push
   ```

You can also push updated configurations to remote servers. Refer to Using {{site.data.keyword.mobilefoundation_short}} CLI to Manage {{site.data.keyword.mobilefoundation_short}} artifacts tutorial.
{: note}

### Protecting resources
{: #protecting-resources}

In the OAuth model, a protected resource is a resource that requires an access token. You can use the {{site.data.keyword.mobilefoundation_short}} security framework to protect both resources that are hosted on an instance of {{site.data.keyword.mobilefoundation_short}} Server, and resources on an external server. You protect a resource by assigning it a scope that defines the required permissions for acquiring an access token for the resource.

You can protect your resources in various ways:

#### Mandatory application scope
{: #mandatoryappscope}

At the application level, you can define a scope that applies to all the resources used by the application. The security framework runs these checks (if exist) in addition to the security checks of the requested resource scope.

* The mandatory application scope is not applied when you access an unprotected resource.
* The access token that is granted for the resource scope does not contain the mandatory application scope.
{: note}

In the {{site.data.keyword.mobilefoundation_short}} Operations Console, select your application from the **Applications** section of the navigation sidebar, and then select the **Security** tab. Under **Mandatory Application Scope**, select **Add to Scope**.

![Mandatory Application Scope](/images/mandatory-application-scope.png "Configure mandatory application scope screen")

You can also manually edit the application’s configuration JSON file with the required configuration and push the changes back to a MobileFirst Server.

1. From a **command-line window**, navigate to the project’s root folder and run the `mfpdev app pull`.
1. Open the configuration file, which is located in the **project-folder\mobilefirst** folder.
1. Edit the file by defining a `mandatoryScope` property, and setting the property value to a scope string that contains a space-separated list of your selected scope elements. For example,

   ```java
       "mandatoryScope": "appAuthenticity PincodeValidation"
   ```
   {: codeblock}

1. Deploy the updated configuration JSON file by running the command: `mfpdev app push`.

You can also push updated configurations to remote servers.

#### Protecting adapter resources
{: #protectadapterres}

In your adapter, you can specify the protecting scope for a Java method or JavaScript resource procedure, or for an entire Java resource class. A scope is defined as a string of one or more space-separated scope elements (`scopeElement1 scopeElement2 …`), or null to apply the default scope. For more information on protecting adapter resources, see [Protecting Adapters](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters).

### Disabling resource protection
{: #disablingresprotection}

You can disable the default {{site.data.keyword.mobilefoundation_short}} resource protection for a specific Java or JavaScript adapter resource, or for an entire Java class, as outlined in the following Java and JavaScript sections. When resource protection is disabled, the {{site.data.keyword.mobilefoundation_short}} security framework does not require a token to access the resource.

#### Disabling Java resource OAuth protection
{: #disablejavaresoauthprotection}

To entirely disable OAuth protection for a Java resource method or class, add the `@OAuthSecurity` annotation to the resource or class declaration, and set the value of the `enabled` element to `false`:

```
@OAuthSecurity(enabled = false)
```
{: codeblock}

The default value of the annotation’s `enabled` element is `true`. When the `enabled` element is set to `false`, the `scope` element is ignored and the resource or resource class is unprotected.

When you assign a scope to a method of an unprotected class, the method is protected despite the class annotation, unless you set the `enabled` element of the resource annotation to `false`.
{: note}

Review the following examples:

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

#### Disabling JavaScript resource OAuth protection
{: #disablejavascriptresoauthprotection}

To entirely disable OAuth protection for a JavaScript adapter resource (procedure), in the **adapter.xml** file, set the `secured` attribute of the <procedure> element to `false`:

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

When the `secured` attribute is set to `false`, the `scope` attribute is ignored and the resource is unprotected.

Review the following example:

The following code disables resource protection for a `userName` procedure:

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

### Defining unprotected resources
{: #defunprotectedresources}

An unprotected resource is a resource that does not require an access token. The {{site.data.keyword.mobilefoundation_short}} security framework does not manage access to unprotected resources, and does not validate or check the identity of clients that access these resources. Therefore, features such as Direct Update, blocking device access, or remotely disabling an application, are not supported for unprotected resources.

### Protecting external resources
{: #protecextresources}

To protect external resources, you add a resource filter with an access-token validation module to the external resource server. The token-validation module uses the introspection endpoint of the security framework’s authorization server to validate {{site.data.keyword.mobilefoundation_short}} access tokens before the OAuth client access is granted to the resources. You can use the {{site.data.keyword.mobilefoundation_short}} ReST API for the {{site.data.keyword.mobilefoundation_short}} runtime to create your own access-token validation module for any external server. Alternatively, use one of the provided {{site.data.keyword.mobilefoundation_short}} extensions for protecting external Java resources.
