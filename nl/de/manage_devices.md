---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Geräte verwalten
{: #manage_devices}

Sie können den Gerätezugriff und den Gerätestatus über die Mobile Foundation Operations Console verwalten. Ein Gerät wird mit einer als Geräte-ID bezeichneten Kennung eindeutig identifiziert, die vom Mobile Foundation-Client-SDK zugeordnet wird. Sie können auch einen Anzeigenamen für Ihr Gerät festlegen. Sowohl das Feld für die Geräte-ID als auch das für den Anzeigenamen des Geräts werden für die Suche indexiert.

Anwendungsentwickler können mithilfe der Methode `setDeviceDisplayName` der Klasse `WLClient` den Anzeigenamen des Geräts festlegen. Die Dokumentation zum `WLClient` finden Sie [hier](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/). Die Entwickler von Java-Adaptern können den Anzeigenamen der Einheit auch mit der Methode `setDeviceDisplayName` der Klasse `com.ibm.mfp.server.registration.external.model MobileDeviceData` festlegen.
{: tip}

Der Mobile Foundation-Server verwaltet die Statusinformationen der einzelnen Geräte, die auf den Server zugreifen.
Folgende Statuswerte sind möglich:
* Aktiv
* Verloren
* Gestohlen
* Abgelaufen
* Inaktiviert

Der Standardstatus des Geräts ist **Aktiv**. Das zeigt an, dass der Zugriff von diesem Gerät nicht blockiert ist. Sie können den Status in **Verloren**, **Gestohlen** oder **Inaktiviert** ändern, um den Zugriff von dem Gerät auf Ihre Anwendungsressourcen zu blockieren. Sie können den Status **Aktiv** jederzeit wiederherstellen, damit der Zugriff wieder möglich ist.

Der Status **Abgelaufen** ist ein Sonderstatus, der vom Mobile Foundation-Server festgelegt wird, nachdem ein Gerät eine vorkonfigurierte Inaktivitätsdauer erreicht hat. Dieser Status wird für die Lizenzüberwachung verwendet und hat keine Auswirkungen auf die Zugriffsberechtigungen des Geräts. Wenn ein Gerät mit dem Status **Abgelaufen** wieder eine Verbindung zum Server herstellt, wird sein Status wieder in **Aktiv** geändert und das Gerät erhält Zugriff auf den Server.

## Geräte blockieren
{: #block_devices}

1. Navigieren Sie in der Mobile Foundation Operations Console auf die Seite **Geräte**.
2. Suchen Sie im Feld **Suche** mithilfe der Benutzer-ID, die dem Gerät zugeordnet ist, oder mithilfe seines Anzeigenamens (sofern dieser für das Gerät festgelegt wurde) nach einem Gerät.
3. Für jedes im Suchergebnis zurückgegebene Gerät werden die Geräte-ID, der Anzeigename, das Gerätemodell, das Betriebssystem sowie die Liste der Benutzer-IDs angezeigt, die diesem Gerät zugeordnet sind.
4. In der Spalte "Gerätestatus" wird der Status des Geräts angezeigt. Sie können den Status des Geräts in **Verloren**, **Gestohlen** oder **Inaktiviert** ändern, um den Zugriff von dem Gerät auf geschützte Anwendungsressourcen zu blockieren.

   Wenn Sie den Status wieder in **Aktiv** ändern, werden die ursprünglichen Zugriffsberechtigungen wiederhergestellt.
   {: note}


## Registrierung von Geräten aufheben
{: #unregister_devices}

1. Navigieren Sie in der Mobile Foundation Operations Console auf die Seite **Geräte**.
2. Suchen Sie im Feld **Suche** mithilfe der Benutzer-ID, die dem Gerät zugeordnet ist, oder mithilfe seines Anzeigenamens (sofern dieser für das Gerät festgelegt wurde) nach einem Gerät.
3. Für jedes im Suchergebnis zurückgegebene Gerät werden die Geräte-ID, der Anzeigename, das Gerätemodell, das Betriebssystem sowie die Liste der Benutzer-IDs angezeigt, die diesem Gerät zugeordnet sind.
4. Wählen Sie in der Spalte **Aktionen** die Option *Registrierung aufheben* aus.

   Wenn Sie die Registrierung eines Geräts aufheben, werden die Registrierungsdaten aller Mobile Foundation-Anwendungen gelöscht, die auf dem Gerät installiert sind. Darüber hinaus werden der Anzeigename des Geräts, die Liste der Benutzer, die ihm zugeordnet sind, sowie die öffentlichen Attribute gelöscht, die die Anwendung für dieses Gerät registriert hat.
   {: note}


Wenden Sie die Option **Registrierung aufheben** nicht auf ein Gerät an, das Sie **blockieren** möchten. Durch das Aufheben der Registrierung eines Geräts werden alle gerätebezogenen Daten gelöscht. Wenn ein Benutzer versucht, mit demselben Gerät auf den Mobile Foundation-Server zuzugreifen, wird er zum Registrieren des Geräts aufgefordert. Bei der Registrierung wird eine neue Geräte-ID für das Gerät auf dem Server erstellt. Dies bedeutet, dass das Gerät wieder Zugriff auf den Mobile Foundation-Server hat.
{: important}
