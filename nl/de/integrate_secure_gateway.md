---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Sichere Verbindung zu einem On-Premises-System mit dem Secure Gateway-Service herstellen
{: #integrate_secure_gateway}

Beim Erstellen mobiler Apps für Unternehmen sollen diese Apps bevorzugt in bestehende Systems of Record (SoR) integriert werden. Um über eine mobile App auf in Ihrem On-Premises-Rechenzentrum gespeicherte Daten zugreifen und diese nutzen zu können, können Sie den [Mobile Foundation-Service](https://cloud.ibm.com/catalog/services/mobile-foundation) mit dem [Secure Gateway-Service](https://cloud.ibm.com/catalog/services/secure-gateway) in [IBM Cloud](https://cloud.ibm.com/) verwenden. Der Secure Gateway-Service bietet eine schnelle, einfache und sichere Lösung zum Herstellen von Verbindungen und stellt einen Tunnel zwischen IBM Cloud und dem fernen System bereit, zu dem Sie eine Verbindung aufbauen möchten.

In diesem Lernprogramm wird erläutert, wie Sie mithilfe des Secure Gateway-Service auf HTTP-Endpunkte in Ihrem Rechenzentrum übe Mobile Foundation-Adapter zugreifen können, die in IBM Cloud ausgeführt werden.

## Voraussetzung
{: #prereq_int_sec_gw}

Zum Durchführen dieses Lernprogramms benötigen Sie einen HTTP-Endpunkt in Ihrer Unternehmensfirewall, der die Systems of Record-Daten bereitstellt. Alternativ können Sie einen Testendpunkt in Ihrer lokalen Umgebung erstellen, indem Sie [dieses `Node.js`-Beispielprojekt ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) verwenden.

Navigieren Sie zu dem Projektordner und führen Sie Ihren HTTP-Server aus.

```bash
npm install
node app.js
```
{: codeblock}

## Integrationsszenario mit dem Secure Gateway-Service
{: #secure_gateway}

Die folgende Abbildung veranschaulicht die Architektur, die im in diesem Lernprogramm erläuterten Integrationsszenario verwendet wird.

![Architekturdiagramm](images/SecureGatewayArchi.png "Architekturdiagramm des Geräts, des Cloud-Service und der lokalen Netzarchitektur")

## Secure Gateway-Integration implementieren
{: #implementing_sg_integration}

### Secure Gateway-Serviceinstanz erstellen
Melden Sie sich bei IBM Cloud an und erstellen Sie eine Instanz des [Secure Gateway-Service](https://cloud.ibm.com/catalog/services/secure-gateway/).

![IBM Cloud](images/SecureGatewayInst.gif "Secure Gateway-Instanz aus dem IBM Cloud-Katalog erstellen")

Nachdem Sie die Secure Gateway-Serviceinstanz erstellt haben, führen Sie die folgenden Schritte aus, um den Secure Gateway-Service zwischen IBM Cloud und Ihrer On-Premises-Umgebung zu konfigurieren.

### Gateway hinzufügen
{: #add_gateway}

Klicken Sie im Dashboard des Secure Gateway-Service auf **Gateway hinzufügen**, um ein neues Gateway zu erstellen. Geben Sie dazu einen Gateway-Namen Ihrer Wahl an.

![Gateway hinzufügen](images/AcmeAddGateway.gif "Gateway-Benutzerschnittstellenschritte hinzufügen")


### Secure Gateway-Client hinzufügen
{: #add_sg_client}

![Client2 hinzufügen](images/AcmeAddClient.gif "Clientbenutzerschnittstellenschritte hinzufügen")

Klicken Sie in Ihrem neuen Gateway auf der Registerkarte **Clients** auf **Client verbinden**.

Sie können jeden beliebigen Client Ihrer Wahl verwenden und den Secure Gateway-Client in Ihrer On-Premises-Umgebung ausführen. Die Schritte zum Einrichten des Secure Gateway-Clients sind in der Secure Gateway-Konsole verfügbar.

In diesem Lernprogramm verwenden Sie die Option für Docker-Container, um den Secure Gateway-Client auszuführen.
Führen Sie die folgenden Schritte aus:
*   Falls noch nicht installiert, installieren Sie Docker auf Ihrer On-Premises-Maschine.
*   Starten Sie einen Terminal und führen Sie den Secure Gateway-Client mit dem in der Servicekonsole angezeigten Befehl in einem Container aus.
    ```bash
    docker run –it ibmcom/secure-gateway-client <gateway-id>
    ```
    {: codeblock}
    Den Wert für `gateway-id` finden Sie in der Konsole, wie in der vorigen Abbildung dargestellt.

### Ziel hinzufügen
{: #add_destination}

![Ziel hinzufügen](images/AcmeAddDest.gif "Zielbenutzerschnittstellenschritte hinzufügen")

Klicken Sie in Ihrem neuen Gateway auf der Registerkarte **Ziele** auf **Ziel hinzufügen**.

Mit dem Secure Gateway-Service können Sie eine Verbindung zu einem On-Premises-Endpunkt aus der IBM Cloud-Umgebung herstellen oder eine Verbindung von einer On-Premises-Umgebung zu einer Cloud-Ressource herstellen. Wählen Sie für dieses Szenario die Option 'On-Premises' als Ressourcenposition aus, und geben Sie den Hostnamen/die IP-Adresse und den Port der On-Premises-Ressource an. Geben Sie außerdem einen Zielnamen Ihrer Wahl an.

Damit die Anforderung vom Secure Gateway-Client an den Ressourcenendpunkt weitergeleitet werden kann, müssen Sie die Ressource in die Zugriffsliste aufnehmen.
Führen Sie den folgenden Befehl auf dem Terminal des Secure Gateway-Client (der in diesem Szenario als Container zur Docker-Laufzeit ausgeführt wird), um die Ressource zur Zugriffsliste hinzuzufügen.

```
acl allow <ressourcen-host>:<ressourcen-port>
```
{: codeblock}
`ressourcen-host` und `ressourcen-port` beziehen sich auf die Details des On-Premises-Ressourcenendpunkts.

Das Ziel ist jetzt konfiguriert. Der Secure Gateway-Service füllt die Host- und Portdetails für die Cloud, die für den Zugriff auf die On-Premises-Ressource aus der Cloud-Umgebung verwendet werden können.

![Registerkarte 'Ziel'](images/AcmeCloudPopulate.gif "Anzeige mit Host- und Portdetails")

### Secure Gateway-Service mit Mobile Foundation und Mobile Foundation-Adapter konfigurieren
{: #configuration_sg_mfp}

In diesem Lernprogramm verwenden Sie die Mobile Foundation-Serviceinstanz in IBM Cloud zum Konfigurieren des Mobile Foundation-Servers. Mit dem Mobile Foundation-Service in IBM Cloud können Sie den Mobile Foundation-Server in der Liberty-Laufzeit als Cloud Foundry-Anwendung bereitstellen. Mit dem Mobile Foundation-Service können Sie jedes Mobile Foundation-Projekt, das in einer lokalen Umgebung entwickelt wurde, übernehmen und in IBM Cloud ausführen.

### Mobile Foundation-Serverkonfiguration in IBM Cloud
{: #mf_server_setup}

Erstellen Sie eine Instanz des [Mobile Foundation-Service](https://cloud.ibm.com/catalog/services/mobile-foundation) über die IBM Cloud-Konsole.

Erstellen Sie den [Mobile Foundation-Server ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/) in der Konsole des Mobile Foundation-Service.


### Mobile Foundation-Adapter erstellen und bereitstellen
{: #deploying_mf_adapter}

In diesem Lernprogramm stellen Sie mit einem Mobile Foundation-Adapter eine Verbindung zum Secure Gateway-Endpunkt her. [Laden Sie ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") den Mobile Foundation-Java-HTTP-Adapter herunter](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP).

Erstellen und implementieren Sie den Adapter in der Mobile Foundation Operations Console mit Befehlen der [mfpdev-CLI](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli).
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

Informationen zum Erstellen und Bereitstellen von Adaptern erhalten Sie [hier ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/).
{: tip}

Geben Sie die Host- und Portdetails der Cloud für den Ressourcenendpunkt im Java-HTTP-Adapter aus dem vorherigen Abschnitt an.

![Adapterkonfiguration ](images/AdapterConfiguration.png "Seite für Java-HTTP-Konfiguration")

Dabei sind `cap-sg-prd-5.securegateway.appdomain.cloud` und `18946` entsprechend der Secure Gateway-Host und -Port.

Der Mobile Foundation-Adapter ist jetzt konfiguriert und der Mobile Foundation-Service kann jetzt mit einem On-Premises-System im Unternehmen mithilfe des Secure Gateway-Service verwendet werden.

### Mobile Foundation-Beispielapp erstellen und registrieren
{: #registering_sample_app}

Laden Sie die Mobile Foundation-Beispielapp [hier](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/) herunter, befolgen Sie die Anweisungen aus der `Readme` und registrieren Sie die App in der Mobile Foundation Operations Console.

Führen Sie die App aus, stellen Sie Berechtigungsnachweise bereit, um sich anzumelden, und klicken Sie auf die Schaltfläche *Anmelden*. Klicken Sie auf die Schaltfläche *Acme Writers abrufen*, um Ihren On-Premises-Endpunkt über Secure Gateway unter Verwendung des Java-HTTP-Adapters aufzurufen, der in der Mobile Foundation Operations Console implementiert ist. Empfangen Sie die gewünschten Daten aus der On-Premises-Umgebung.

![Empfang von lokalen Daten durch die App](images/AcmePublishersApp.gif "Beispielapp mit Datenempfang")

Sie können eine Verbindung zu mehreren On-Premises-Endpunkten herstellen, indem Sie mehrere Ziele für den Secure Gateway-Service konfigurieren und Mobile Foundation-Adapter bereitstellen, um eine Verbindung zum jeweiligen Cloud-Host des Endpunkts herzustellen. Sie können den Secure Gateway-Service auch mit zusätzlicher Sicherheit konfigurieren, um sicherzustellen, dass die Kommunikation mit dem Endpunkt über die HTTPS- und die anwendungsseitige Sicherheit erfolgt. Die entsprechenden [Details finden Sie hier](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg).


## Zusammenfassung
{: #summary_int_sec_gw}

Anhand dieses Lernprogramms sollten Sie mithilfe des Mobile Foundation-Service eine sichere Verbindung zwischen den in IBM Cloud ausführten Mobile Foundation-Adaptern und einem On-Premises-Endpunkt herstellen können.
