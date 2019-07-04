---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, MIM attack, certificate pinning

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 防止中间人攻击
{: #prevent_man_in_the_middle_attack}


通过公用网络进行通信时，安全地发送和接收信息至关重要。广泛用于保护这些通信的协议是 SSL/TLS。SSL/TLS 使用数字证书来提供认证和加密。这些协议依赖于证书链验证，并且容易受到一些危险的攻击（包括中间人攻击），发生这些攻击时，未经授权方能够查看和修改在移动设备与后端系统间传递的所有流量。
{: shortdesc}

通过[证书锁定](#cert_pinning)可减少安全攻击，并防止中间人 (MitM) 攻击。

此功能为可选，且仅适用于专业套餐。
{: note}

证书锁定过程包含以下步骤：

1. 执行[证书设置](#certsetup)。
2. 配置客户机代码，以在发出安全请求之前，先调用正确的[证书锁定 API](#certpinapi) 方法。

在 SSL 握手（向服务器发出的第一个请求）期间，Mobile Foundation 客户机 SDK 会验证服务器证书的公用密钥是否与应用程序中存储的证书的公用密钥相匹配。

您必须使用从认证中心购买的证书。**不支持**自签名证书。
{: note}

## 证书锁定
{: #cert_pinning}

依赖于证书链验证（如 SSL/TLS）的协议容易受到一些危险的攻击（包括中间人攻击），发生这些攻击时，未经授权方能够查看和修改在移动设备与后端系统间传递的所有流量。要信任某个证书真实有效，该证书必须通过属于可信认证中心 (CA) 的根证书进行数字签名。操作系统和浏览器维护有可信 CA 根证书的列表，以便可以轻松验证 CA 签发和签名的证书。

IBM Mobile Foundation 提供了用于启用**证书锁定**的 API。本机 iOS、本机 Android 和跨平台 Cordova MobileFirst 应用程序中均支持此 API。

## 证书锁定过程
{: #certpinprocess}

证书锁定是将主机与其期望的公用密钥相关联的过程。可以将客户机代码配置为仅接受您域名的特定证书，而不接受与操作系统或浏览器识别的可信 CA 根证书对应的任何证书。证书副本会放入客户机应用程序中。在 SSL 握手（向服务器发出的第一个请求）期间，MobileFirst 客户机 SDK 会验证服务器证书的公用密钥是否与应用程序中存储的证书的公用密钥相匹配。

您还可以将多个证书与客户机应用程序锁定。 将所有证书的副本放入客户机应用程序中。在 SSL 握手（向服务器发出的第一个请求）期间，MobileFirst 客户机 SDK 会验证服务器证书的公用密钥是否与应用程序中存储的其中一个证书的公用密钥相匹配。
{: note}

* **重要信息**
    * 某些移动操作系统可能会高速缓存证书验证检查结果。因此，代码在发出安全请求**之前**调用证书锁定 API 方法。否则，任何后续请求都可能跳过证书验证和锁定检查。
    * 确保仅将 Mobile Foundation API 用于与相关主机的所有通信，即使在证书锁定之后也是如此。使用第三方 API 与同一主机进行交互可能会导致意外行为，例如移动操作系统对未验证的证书进行高速缓存。
    * 再次调用证书锁定 API 方法会覆盖先前的锁定操作。

如果锁定过程成功，那么在安全请求 SSL/TLS 握手期间，将使用所提供证书内的公用密钥来验证 MobileFirst 服务器证书的完整性。如果锁定过程失败，那么客户机应用程序将拒绝针对服务器的所有 SSL/TLS 请求。

## 证书设置
{: #certsetup}

您必须使用从认证中心购买的证书。**不支持**自签名证书。为了与支持的环境兼容，请确保使用以 **DER**（特异编码规则，根据国际电信联盟 X.690 标准定义）格式编码的证书。

证书必须放入 MobileFirst 服务器和应用程序中。如下所示放置证书：

* 在 MobileFirst 服务器（WebSphere Application Server、WebSphere Application Server Liberty 或 Apache Tomcat）中，请查阅特定应用程序服务器的文档，以获取有关如何配置 SSL/TLS 和证书的信息。
* 在应用程序中：
    * 对于本机 iOS，向应用程序**捆绑软件**添加证书
    * 对于本机 Android，将证书放入 **assets** 文件夹中
    * 对于 Cordova，将证书放入 **app-name\www\certificates** 文件夹中（如果此文件夹尚不存在，请进行创建）

## 证书锁定 API
{: #certpinapi}

证书锁定包含以下重载的 API 方法，其中一个方法具有 ``certificateFilename`` 参数，其中 ``certificateFilename`` 是证书文件的名称，另一个方法具有 ``certificateFilenames`` 参数，其中 ``certificateFilenames`` 是一组证书文件名称。

### Android
{: #certpinapiandroid}

* 单个证书：

    语法：pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

    示例：

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* 多个证书：

    语法：pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    示例：

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

在以下两种情况下，证书锁定方法将引发异常：

* 文件不存在
* 文件格式错误

### iOS
{: #certpinapiios}

* 单个证书锁定

    语法：pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    在以下两种情况下，证书锁定方法将引发异常：

    * 文件不存在
    * 文件格式错误

* 多个证书锁定

    语法：pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    在以下两种情况下，证书锁定方法将引发异常：

    * 不存在任何证书文件
    * 不存在格式正确的证书文件

**在 Objective-C 中**：

示例：单个证书：

```objectc
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

示例：多个证书：

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**在 Swift 中**：

示例：单个证书：

```swift
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

示例：多个证书：

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

在以下两种情况下，证书锁定方法将引发异常：

* 文件不存在
* 文件格式错误

### Cordova
{: #certpinapicordova}

* 单个证书锁定：

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* 多个证书锁定：

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

证书锁定方法会返回 Promise：

* 如果锁定成功，那么证书锁定方法调用 `onSuccess` 方法。
* 在以下两种情况下，证书锁定方法将触发 `onFailure` 回调：
  * 文件不存在。
  * 文件格式错误。

之后，如果对未锁定其证书的服务器发出安全请求，那么将调用特定请求（例如，`obtainAccessToken` 或 `WLResourceRequest`）的 `onFailure` 回调。

在 [API 参考](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks)中了解有关证书锁定 API 方法的更多信息。
{: note}
