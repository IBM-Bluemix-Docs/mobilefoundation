---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: mobile foundation security, MIM attack, certificate pinning

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

# Prevent Man-in-the-middle attack
{: #prevent_man_in_the_middle_attack}

When communicating over public networks, it is essential to send and receive information securely. The protocol widely used to secure these communications is SSL/TLS. SSL/TLS uses digital certificates to provide authentication and encryption. These protocols rely on certificate chain verification and are vulnerable to a number of dangerous attacks, including man-in-the-middle attacks, which occur when an unauthorized party is able to view and modify all traffic that passes between the mobile device and the backend systems.
{: shortdesc}

Reduce Security Attacks through [Certificate Pinning](#cert_pinning) and prevent man-in-the-middle (MitM) attacks.

This feature is optional and is available only for Professional plans.
{: note}

Certificate pinning process consists of the following steps:

1. Perform the [Certificate setup](#certsetup).
1. Configure your client code to call the correct [Certificate Pinning API](#certpinapi) method before you make a secured request.

During the SSL handshake (first request to the server), the Mobile Foundation client SDK verifies that the public key of the server certificate matches the public key of the certificate that is stored in the app.

Use a certificate that is purchased from a certificate authority. Self-signed certificates are **not supported**.
{: note}

## Certificate Pinning
{: #cert_pinning}

Protocols that rely on certificate chain verification, such as SSL/TLS, are vulnerable to a number of dangerous attacks, including man-in-the-middle attacks, which occurs when an unauthorized party is able to view and modify all the traffic that passes between the mobile device and the backend systems. To trust that a certificate is genuine and valid, it is digitally signed by a root certificate that belongs to a trusted certificate authority (CA). Operating systems and browsers maintain lists of trusted CA root certificates so that they can easily verify certificates that the CAs have issued and signed.

{{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} provides an API to enable **certificate pinning**. It is supported in native iOS, native Android, and cross-platform Cordova {{site.data.keyword.mobilefoundation_short}} applications.

## Certificate pinning process
{: #certpinprocess}

Certificate pinning is the process of associating a host with its expected public key. You can configure your client code to accept only a specific certificate for your domain name, instead of any certificate that corresponds to a trusted CA root certificate recognized by the operating system or browser. A copy of the certificate is placed in your client application. During the SSL handshake (first request to the server), the {{site.data.keyword.mobilefoundation_short}} client SDK verifies that the public key of the server certificate matches the public key of the certificate that is stored in the app.

You can also pin multiple certificates with your client application. Place a copy of all the certificates in your client application. During the SSL handshake (first request to the server), the MobileFirst client SDK verifies that the public key of the server certificate matches to the public key of one of the certificates that is stored in the app.
{: note}

- Some mobile operating systems might cache the certificate validation check result. Therefore, your code calls the certificate pinning API method **before** making a secured request. Otherwise, any subsequent request might skip the certificate validation and pinning check.
- Make sure to use only Mobile Foundation APIs for all communications with the related host, even after the certificate pinning. Using third-party APIs to interact with the same host might lead to unexpected behavior, such as caching of a non-verified certificate by the mobile operating system.
- Calling the certificate pinning API method a second time overrides the previous pinning operation.
{: important}

If the pinning process is successful, the public key inside the provided certificate is used to verify the integrity of the {{site.data.keyword.mobilefoundation_short}} Server certificate during the secured request SSL/TLS handshake. If the pinning process fails, all SSL/TLS requests to the server are rejected by the client application.

## Certificate setup
{: #certsetup}

Use a certificate that is purchased from a certificate authority. Self-signed certificates are **not supported**. For compatibility with the supported environments, make sure to use a certificate that is encoded in **DER** (Distinguished Encoding Rules, as defined in the International Telecommunications Union X.690 standard) format.

The certificate must be placed in both the {{site.data.keyword.mobilefoundation_short}} Server and in your application. Place the certificate as follows,

* In the {{site.data.keyword.mobilefoundation_short}} Server (WebSphere Application Server, WebSphere Application Server Liberty, or Apache Tomcat), refer to the documentation for your specific application server for information about how to configure SSL/TLS and certificates.
* In your application,
   * For Native iOS, add the certificate to the application **bundle**
   * For Native Android, place the certificate in the **assets** folder
   * For Cordova, place the certificate in the **app-name\www\certificates** folder (if the folder is not already there, create it)

## Certificate pinning API
{: #certpinapi}

Certificate pinning consists of the following overloaded API method, where one method has a parameter ``certificateFilename``, where ``certificateFilename`` is the name of the certificate file, and the second method has a parameter ``certificateFilenames``, where ``certificateFilenames`` is an array of names of the certificate files.

### Android
{: #certpinapiandroid}

* Single certificate:

   Syntax: pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

   Example:

   ```java
   WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
   ```

* Multiple certificates:

   Syntax: pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

   Example:

   ```java
   String[] certificates={"myCertificate.cer","myCertificate1.cer"};
   WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
   ```

The certificate pinning method raises an exception in two cases:

* The file does not exist
* The file is in the wrong format

### iOS
{: #certpinapiios}

* Single certificate Pinning

   Syntax: pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

   The certificate pinning method raises an exception in two cases:

   * The file does not exist
   * The file is in the wrong format

* Multiple certificates pinning

   Syntax: pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

   The certificate pinning method raises an exception in two cases:

   * None of the certificate file exist
   * None of the certificate file in correct format

**In Objective-C**:

* Single certificate pinning

   Example:

   ```objectc
   [[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
   ```

* Multiple certificates pinning

   Example:

   ```objectc
   NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
   [[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
   ```

**In Swift**:

* Single certificate pinning

   Example:

   ```swift
   WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
   ```

* Multiple certificates pinning

   Example:

   ```swift
   let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
   WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
   ```

The certificate pinning method raises an exception in two cases:

* The file does not exist
* The file is in the wrong format

### Cordova
{: #certpinapicordova}

* Single certificate pinning:

   ```javascript
   WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
   ```

* Multiple certificates pinning:

   ```javascript
   WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
   ```

The certificate pinning method returns a promise:

* The certificate pinning method calls the `onSuccess` method if the pinning is successful.
* The certificate pinning method triggers the `onFailure` callback in the following two cases,
   * The file does not exist.
   * The file is in the wrong format.

Later, if a secured request is made to a server whose certificate is not pinned, the `onFailure` callback of the specific request (for example, `obtainAccessToken` or `WLResourceRequest`) is called.

Learn more about the certificate pinning API method in the [API Reference](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks).
{: note}
