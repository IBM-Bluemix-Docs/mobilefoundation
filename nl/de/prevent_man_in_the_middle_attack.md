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

# Man-in-the-Middle-Attacken verhindern
{: #prevent_man_in_the_middle_attack}


Bei der Kommunikation über öffentliche Netze ist es wichtig, Informationen sicher zu senden und zu empfangen. Das Protokoll, das am häufigsten verwendet wird, um diese Kommunikation zu sichern, ist SSL/TLS. SSL/TLS verwendet digitale Zertifikate, um Authentifizierungs- und Verschlüsselungsdaten bereitzustellen. Diese Protokolle basieren auf der Zertifikatskettenprüfung und sind anfällig für eine Reihe gefährlicher Angriffe, einschließlich von Man-in-the-Middle-Angriffen, die auftreten, wenn eine nicht berechtigte Partei den gesamten Datenverkehr zwischen dem mobilen Gerät und den Back-End-Systemen anzeigen und ändern kann.
{: shortdesc}

Reduzieren Sie mithilfe von [Certificate Pinning](#cert_pinning) Angriffe auf die Sicherheit und verhindern Sie Man-in-the-Middle-Angriffe (MitM).

Diese Option ist optional und nur für Professional-Pläne verfügbar.
{: note}

Der Prozess des Certificate Pinning setzt sich aus den folgenden Schritten zusammen:

1. [Konfigurieren Sie das Zertifikat](#certsetup).
2. Konfigurieren Sie den Client-Code so, dass vor dem Durchführen einer geschützten Anforderung die richtige Methode für die [Certificate Pinning-API](#certpinapi) aufgerufen wird.

Während des SSL-Handshake (erste Anforderung an den Server) überprüft das Mobile Foundation-Client-SDK, ob der öffentliche Schlüssel des Serverzertifikats mit dem öffentlichen Schlüssel des Zertifikats übereinstimmt, das in der App gespeichert ist.

Sie müssen ein Zertifikat verwenden, das von einer Zertifizierungsstelle erworben wurde. Selbst signierte Zertifikate werden **nicht unterstützt**.
{: note}

## Certificate Pinning
{: #cert_pinning}

Protokolle, die auf auf einer Zertifikatskettenprüfung wie SSL/TLS basieren, sind anfällig für eine Reihe gefährlicher Angriffe, einschließlich von Man-in-the-Middle-Angriffen, die auftreten, wenn eine nicht berechtigte Partei den gesamten Datenverkehr zwischen dem mobilen Gerät und den Back-End-Systemen anzeigen und ändern kann. Um darauf zu vertrauen, dass ein Zertifikat echt und gültig ist, wird es digital von einem Stammzertifikat signiert, das zu einer anerkannten Zertifizierungsstelle (CA, Certificate Authority) gehört. Betriebssysteme und Browser verwalten Listen von anerkannten CA-Stammzertifikaten, so dass sie die Zertifikate, die die Zertifizierungsstellen ausgestellt und signiert haben, ohne großen Aufwand überprüfen können.

IBM Mobile Foundation stellt eine API für das **Certificate Pinning** bereit. Dies wird von nativen iOS-, nativen Android- und plattformunabhängigen Cordova MobileFirst-Anwendungen unterstützt.

## Prozess des Certificate Pinning
{: #certpinprocess}

Beim Certificate Pinning wird einem Host sein erwarteter öffentlicher Schlüssel zugeordnet. Sie können Ihren Client-Code so konfigurieren, dass er nur ein bestimmtes Zertifikat für Ihren Domänennamen akzeptiert und nicht jedes Zertifikat, das einem anerkannten CA-Stammzertifikat entspricht, das vom Betriebssystem oder vom Browser erkannt wird. Eine Kopie des Zertifikats wird in Ihre Clientanwendung eingefügt. Während des SSL-Handshake (erste Anforderung an den Server) überprüft das MobileFirst-Client-SDK, ob der öffentliche Schlüssel des Serverzertifikats mit dem öffentlichen Schlüssel des Zertifikats übereinstimmt, das in der App gespeichert ist.

Sie können auch mehrere Zertifikate mit Ihrer Clientanwendung verknüpfen. Platzieren Sie eine Kopie aller Zertifikate in Ihrer Clientanwendung. Während des SSL-Handshake (erste Anforderung an den Server) überprüft das MobileFirst-Client-SDK, ob der öffentliche Schlüssel des Serverzertifikats mit dem öffentlichen Schlüssel eines der Zertifikate übereinstimmt, die in der App gespeichert sind.
{: note}

* **Wichtig**
    * Einige Betriebssysteme für mobile Geräte stellen das Ergebnis der Gültigkeitsprüfung für das Zertifikat möglicherweise in den Cache. Daher ruft der Code die API-Methode für das Certificate Pinning **vor** dem Durchführen einer geschützten Anwendung auf. Andernfalls kann es sein, dass nachfolgende Anforderungen die Zertifikatsprüfung und Certificate Pinning-Prüfung überspringen.
    * Stellen Sie sicher, dass nur Mobile Foundation-APIs für die gesamte Kommunikation mit dem zugehörigen Host verwendet werden, selbst nach dem Certificate Spinning. Die Verwendung von APIs anderer Anbieter für die Interaktion mit demselben Host kann zu einem unerwarteten Verhalten führen, z. B. dem Caching eines nicht geprüften Zertifikats durch das Betriebssystem für mobile Geräte.
    * Bei einem weiteren Aufruf der API-Methode für das Certifiate Pinning wird die vorherige Pinning-Operation überschrieben.

Wenn der Pinning-Prozess erfolgreich ist, wird mit dem öffentlichen Schlüssel im bereitgestellten Zertifikat die Integrität des MobileFirst Server-Zertifikats während des SSL/TLS-Hndshakes der geschützten Anforderung geprüft. Wenn der Pinning-Prozess fehlschlägt, werden alle SSL/TLS-Anforderungen an den Server von der Clientanwendung zurückgewiesen.

## Zertifikatskonfiguration
{: #certsetup}

Sie müssen ein Zertifikat verwenden, das von einer Zertifizierungsstelle erworben wurde. Selbst signierte Zertifikate werden **nicht unterstützt**. Aus Gründen der Kompatibilität mit den unterstützten Umgebungen müssen Sie sicherstellen, dass das verwendete Zertifikat im **DER**-Format (Distinguished Encoding Rules) gemäß Standard X.690 der International Telecommunications Union codiert ist.

Das Zertifikat muss sowohl in MobileFirst Server als auch in Ihre Anwendung eingefügt werden. Fügen Sie das Zertifikat wie folgt ein:

* In MobileFirst Server (WebSphere Application Server, WebSphere Application Server Liberty oder Apache Tomcat): Informationen zum Konfigurieren von SSL/TLS und Zertifikaten finden Sie in der Dokumentation zum Ihrem entsprechenden Anwendungsserver.
* In Ihrer Anwendung:
    * Natives iOS: Fügen Sie das Zertifikat zum **Anwendungsbundle** hinzu.
    * Natives Android: Fügen Sie das Zertifikat in den Ordner **assets** ein.
    * Cordova: Fügen Sie das Zertifikat in den Ordner **app-name\www\certificates** ein (wenn der Ordner nicht existiert, erstellen Sie ihn).

## API für das Certificate Pinning
{: #certpinapi}

Das Certificate Pinning besteht aus der folgenden überladenen API-Methode, wobei eine Methode den Parameter ``certificateFilename`` aufweist. Hierbei steht ``certificateFilename`` der Name der Zertifikatsdatei. Die zweite Methode verfügt über den Parameter ``certificateFilenames``, wobei ``certificateFilenames`` ein Bereich von Namen der Zertifikatsdateien ist.

### Android
{: #certpinapiandroid}

* Einzelnes Zertifikat:

    Syntax: pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

    Beispiel:

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* Mehrere Zertifikate:

    Syntax: pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    Beispiel:

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

Die Certificate Pinning-Methode löst in zwei Fällen eine Ausnahme aus:

* Die Datei ist nicht vorhanden.
* Die Datei liegt im falschen Format vor.

### iOS
{: #certpinapiios}

* Certificate Pinning für ein einzelnes Zertifikat

    Syntax: pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    Die Certificate Pinning-Methode löst in zwei Fällen eine Ausnahme aus:

    * Die Datei ist nicht vorhanden.
    * Die Datei liegt im falschen Format vor.

* Certificate Pinning für mehrere Zertifikate

    Syntax: pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    Die Certificate Pinning-Methode löst in zwei Fällen eine Ausnahme aus:

    * Keine der Zertifikatdateien ist vorhanden.
    * Keine der Zertifikatsdateien liegt im korrekten Format vor.

**In Objective-C**:

Beispiel: Einzelnes Zertifikat:

```objectc
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

Beispiel: Mehrere Zertifikate:

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**In Swift**:

Beispiel: Einzelnes Zertifikat:

```swift
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

Beispiel: Mehrere Zertifikate:

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

Die Certificate Pinning-Methode löst in zwei Fällen eine Ausnahme aus:

* Die Datei ist nicht vorhanden.
* Die Datei liegt im falschen Format vor.

### Cordova
{: #certpinapicordova}

* Certificate Pinning für ein einzelnes Zertifikat:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* Certificate Pinning für mehrere Zertifikate:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

Die Certificate Pinning-Methode gibt eine Zusicherung (Promise) zurück:

* Die Certificate Pinning-Methode ruft die Methode `onSuccess` auf, wenn das Pinning erfolgreich war.
* Die Certificate Pinning-Methode löst den Callback `onFailure` in den beiden folgenden Situationen aus:
  * Die Datei ist nicht vorhanden.
  * Die Datei hat das falsche Format.

Wenn zu einem späteren Zeitpunkt eine geschützte Anforderung an einen Server gestellt wird, für dessen Zertifikat kein Certificate Pinning durchgeführt wurde, wird die Callback-Operation `onFailure` dieser Anforderung (z. B. `obtainAccessToken` oder `WLResourceRequest`) aufgerufen.

Weitere Informationen zur API-Methode für das Certificate Pinning finden Sie in der [API-Referenz](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks).
{: note}
