---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Push Notifications, notifications, unicast notifications, tag notifications, interactive notifications, silent notifications, configure DataPower

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
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

# Push-Benachrichtigungen
{: #push_notifications}

Benachrichtigungen ermöglichen einem mobilen Gerät, Nachrichten zu empfangen, die ein Server mit einer Push-Operation gesendet hat. Benachrichtigungen werden unabhängig davon empfangen, ob die Anwendung im Vordergrund oder Hintergrund ausgeführt wird.  
{: shortdesc}

{{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} stellt einheitliche API-Methoden zum Senden von Push-Benachrichtigungen an iOS-, Android-, Windows 8.1 Universal-, Windows 10 UWP- und Cordova-Anwendungen (iOS, Android) bereit. Die Benachrichtigungen werden von {{site.data.keyword.mfserver_short_notm }} an den Anbieter (Apple, Google, Microsoft, SMS Gateways) und von dort an die betreffenden Geräte gesendet. Der einheitliche Benachrichtigungsmechanismus macht den gesamten Prozess der Kommunikation mit Benutzern und Geräten für den Entwickler vollkommen transparent.

## Geräteunterstützung
{: #device-support}
Push-Benachrichtigungen werden für die folgenden Plattformen in {{site.data.keyword.mobilefoundation_short }} unterstützt:

* iOS 8.x oder höher
* Android 4.x oder höher
* Windows 8.1, Windows 10

## Formen von Push-Benachrichtigungen
{: #push-notifications-forms}
Es gibt verschiedene Arten von Benachrichtigungen:

* **Alert** (iOS, Android, Windows): Eine Popup-Textnachricht
* **Sound** (iOS, Android, Windows): Eine Audiodatei, die beim Empfang einer Benachrichtigung abgespielt wird
* **Badge** (iOS), Tile (Windows): Eine grafische Darstellung für einen kurzen Text oder ein Bild
* **Banner** (iOS), Toast (Windows) - Eine Popup-Textnachricht in der Geräteanzeige, die wieder ausgeblendet wird
* **Interactive** (iOS 8 und höher): Aktionsschaltflächen im Banner einer empfangenen Benachrichtigung
* **Silent** (iOS 8 und höher): Senden von Benachrichtigungen, ohne den Benutzer zu stören

## Arten von Push-Benachrichtigungen
{: #push-notification-types }

### Tagbasierte Benachrichtigungen
{: #tag-notifications-overview}

Tagbasierte Benachrichtigungen sind Hinweisnachrichten, die an alle Geräte gesendet werden, die einen bestimmten Tag abonniert haben.  

Bei tagbasierten Benachrichtigungen ist es möglich, die Benachrichtigungen ausgehend von Themenbereichen oder Topics bestimmten Segmenten zuzuordnen. Empfänger der Benachrichtigungen können angeben, dass sie nur Benachrichtigungen zu einem bestimmten Thema oder einem sie interessierenden Topic empfangen möchten. Bei tagbasierten Benachrichtigungen gibt es zu diesem Zweck eine Möglichkeit, Empfänger Segmenten zuzuordnen. Mit diesem Feature können Tags definiert werden, um dann Nachrichten auf der Basis von Tags zu senden und zu empfangen. Eine Nachricht wird nur an die Geräte gesendet, die einen Tag abonniert haben.

### Broadcastbenachrichtigungen
{: #broadcast-notifications-overview}

Broadcastbenachrichtigungen sind eine Form der tagbasierten Push-Benachrichtigungen, die an alle eingeschriebenen Geräte gesendet werden und standardmäßig für alle Push-fähigen {{ site.data.keyword.mobilefirst_notm }}-Anwendungen durch das Abonnement eines reservierten Tags `Push.all` aktiviert werden. Der Tag wird automatisch für jedes Gerät erstellt. Broadcastbenachrichtigungen können inaktiviert werden, indem das Abonnement des reservierten Tags `Push.all` beendet wird.

### Unicastbenachrichtigungen
{: #unicast-notifications-overview}

Unicastbenachrichtigungen oder mit OAuth geschützte authentifizierte Benachrichtigungen sind Benachrichtigungen, die an ein bestimmtes Gerät oder an bestimmte Benutzer-IDs gesendet werden. Die Benutzer-ID (userID) im Benutzerabonnement kann aus dem zugrunde liegenden Sicherheitskontext stammen.

### Interaktive Benachrichtigungen
{: #interactive-notifications-overview}

Wenn eine interaktive Benachrichtigung eingeht, können Benutzer Aktionen ausführen, ohne die Anwendung zu öffnen. Beim Eintreffen einer interaktiven Benachrichtigung zeigt das Gerät die Benachrichtigung und Aktionsschaltflächen an. Derzeit werden interaktive Benachrichtigungen auf Geräten mit iOS-Version 8 und höher unterstützt. Wenn eine interaktive Benachrichtigung an ein iOS-Gerät mit einer älteren Version als 8 gesendet wird, werden die Benachrichtigungsaktionen nicht angezeigt.

Machen Sie sich mit der Handhabung [interaktiver Benachrichtigungen](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications) vertraut.

### Benachrichtigungen im Hintergrund
{: #silent-notifications-overview}

Benachrichtigungen im Hintergrund erfolgen ohne Anzeige von Alerts oder andere Störungen des Benutzers. Wenn eine Benachrichtigung im Hintergrund eingeht, wird der Handling-Code der Anwendung im Hintergrund ausgeführt, ohne die Anwendung in den Vordergrund zu bringen. Zurzeit werden Benachrichtigungen im Hintergrund auf iOS-Geräten der Version 7 oder einer aktuelleren Version unterstützt. Wenn die Benachrichtigung im Hintergrund an iOS-Geräte mit einer älteren Version als Version 7 gesendet wird und die Anwendung im Hintergrund ausgeführt wird, wird die Benachrichtigung ignoriert. Wenn die Anwendung im Vordergrund ausgeführt wird, wird die Callback-Methode für Benachrichtigungen aufgerufen.

Machen Sie sich mit der Handhabung von [Benachrichtigungen im Hintergrund](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications) vertraut.

Unicastbenachrichtigungen enthalten keine Tags in den Nutzdaten. Die Benachrichtigung kann an mehrere Geräte oder Benutzer gesendet werden, wenn im Zielblock der API 'Message (POST)' mehrere Geräte_IDs (deviceIDs) bzw. Benutzer-IDs (userIDs) angegeben werden.
{: note}

## Proxy-Einstellungen
{: #proxy-settings}

Verwenden Sie die Proxy-Einstellungen, um den optionalen Proxy festzulegen, über den Benachrichtigungen an APNS und FCM gesendet werden. Verwenden Sie die Konfigurationseigenschaften **push.apns.proxy.* ** und **push.gcm.proxy.***, um den Proxy festzulegen. Weitere Informationen finden Sie in der [Liste der JNDI-Eigenschaften für den {{ site.data.keyword.mfserver_short_notm }}-Push-Service](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

WNS bietet keine Proxyunterstützung.
{: note}

### WebSphere DataPower als Endpunkt für Push-Benachrichtigungen verwenden
{: #proxy-settings-datapower-endpoint}

Sie können DataPower so einrichten, dass Benachrichtigungsanforderungen von {{site.data.keyword.mfserver_short_notm }} akzeptiert und an FCM und WNS weitergeleitet werden.

APNs werden nicht unterstützt.
{: note}

#### {{ site.data.keyword.mfserver_short_notm }} konfigurieren
{: #proxy-settings-config-mfp-server}

Konfigurieren Sie in der Datei `server.xml` die folgende JNDI-Eigenschaft.
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

Dabei steht `host` für den Hostnamen von DataPower und `port` für die Nummer des Ports, an dem der HTTPS-Front-Side-Handler für FCM und WNS konfiguriert ist.

#### DataPower konfigurieren
{: #proxy-settings-config-datapower}

1. Melden Sie sich an der DataPower-Appliance an.
2. Navigieren Sie zu **Services > Gateway für mehrere Protokolle > Neues Gateway für mehrere Protokolle**.
3. Geben Sie einen Namen an, mit dem Sie die Konfiguration identifizieren können.
4. Wählen Sie den XML-Manager aus, geben Sie 'Richtlinien für Gateway für mehrere Protokolle' als Standard an und setzen Sie 'URL-Umschreibungsrichtlinie' auf 'keine'.
5. Wählen Sie das Optionsfeld **Statisches Backend** aus und wählen Sie eine der folgenden Optionen für **URL für Standard-Backend festlegen** aus:
    - Für FCM:	`https://gcm-http.googleapis.com`
    - Für WNS:	`https://hk2.notify.windows.com`
6. Wählen Sie für 'Antworttyp' und 'Anforderungstyp' die Option 'Weiterleiten' aus.

### Zertifikat generieren
{: #proxy-settings-generate-cert}

Um ein Zertifikat zu generieren, wählen Sie eine der folgenden Optionen aus:

* Für FCM:
    1. Geben Sie in der Befehlszeile den Befehl `openssl` ein, um die FCM-Zertifikate abzurufen.
    2. Führen Sie den folgenden Befehl aus:
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
        {: codeblock}
    3. Kopieren Sie den Inhalt von *-----BEGIN CERTIFICATE-----  bis -----END CERTIFICATE-----* und speichern Sie ihn in einer Datei mit der Erweiterung `.pem`.

* Für WNS:
    1. Verwenden Sie in der Befehlszeile den Befehl `openssl`, um die WNS-Zertifikate abzurufen.
    2. Führen Sie den folgenden Befehl aus:
        ```
        openssl s_client -connect https://hk2.notify.windows.com:443
        ```
        {: codeblock}
    3. Kopieren Sie den Inhalt von *-----BEGIN CERTIFICATE-----  bis -----END CERTIFICATE-----* und speichern Sie ihn in einer Datei mit der Erweiterung `.pem`.

### Back-Side-Einstellungen
{: #proxy-settings-backside-settings}

* Für FCM und WNS:
    1. Erstellen Sie ein Verschlüsselungszertifikat.
        a. Navigieren Sie zu **Objekte > Verschlüsselungskonfiguration** und klicken Sie auf **Verschlüsselungszertifikat**.
        b. Geben Sie einen Namen für die Identifikation des Verschlüsselungszertifikats an.
        c. Klicken Sie auf **Hochladen**, um das generierte FCM-Zertifikat hochzuladen.
        d. Setzen Sie **Kennwortalias** auf 'keiner'.
        e. Klicken Sie auf **Schlüssel generieren**.
        ![Verschlüsselungszertifikat konfigurieren](images/bck_1.gif "Verschlüsselungszertifikat konfigurieren")

    2. Erstellen Sie einen Validierungsberechtigungsnachweis für die Verschlüsselung.
        a. Navigieren Sie zu Objekte > Verschlüsselungskonfiguration** und klicken Sie auf **Validierungsberechtigungsnachweis für Verschlüsselung**.
        b. Geben Sie einen eindeutigen Namen an.
        c. Wählen Sie für 'Zertifikate' das im vorherigen Schritt (Schritt 1) erstellte Verschlüsselungszertifikat aus.
        d. Wählen Sie für **Zertifikatsvalidierungsmodus** die Option 'Übereinstimmung mit genauem Zertifikat' oder 'Direkter Aussteller' aus.
        e. Klicken Sie auf **Anwenden**.

        ![Validierungsberechtigungsnachweise für Verschlüsselung konfigurieren](images/bck_2.gif "Validierungsberechtigungsnachweise für Verschlüsselung konfigurieren")

    3. Erstellen Sie einen Validierungsberechtigungsnachweis für die Verschlüsselung:
        a. Navigieren Sie zu **Objekte > Verschlüsselungskonfiguration** und klicken Sie auf **Verschlüsselungsprofil**.
        b. Klicken Sie auf **Hinzufügen**.
        c. Geben Sie einen eindeutigen Namen an.
        d. Wählen Sie für **Validierungsberechtigungsnachweise** im Dropdown-Menü den im vorherigen Schritt (Schritt 2) erstellten Validierungsberechtigungsnachweis aus und setzen Sie die Berechtigungsnachweise zur Identifikation auf **keine**.
        e. Klicken Sie auf **Anwenden**.

        ![Verschlüsselungsprofil konfigurieren](images/bck_3.gif "Verschlüsselungsprofil konfigurieren")

    4. Erstellen Sie ein SSL-Proxy-Profil:

        a. Navigieren Sie zu **Objekte** > **Verschlüsselungskonfiguration** > **SSL-Proxy-Profil**.
        b. Wählen Sie eine der folgenden Optionen aus:

            - Legen Sie für SMS den Wert für **SSL-Proxy-Profil** auf 'keines' fest.
            - Führen Sie für FCM und WNS mit geschützter Back-End-URL (HTTPS) die folgenden Schritte aus:
                i. Klicken Sie auf **Hinzufügen**.
                ii. Geben Sie einen Namen für die spätere Identifizierung des SSL-Proxy-Profils an.
                iii. Wählen Sie für **SSL-Richtung** im Dropdown-Menü die Option **Weiterleiten** aus.
                iv. Wählen Sie für '(Client) Verschlüsselungsprofil weiterleiten' das in Schritt 3 erstellte Verschlüsselungsprofil aus.
                v. Klicken Sie auf **Anwenden**.

        ![SSL-Proxy-Profil konfigurieren](images/bck_4.gif "SSL-Proxy-Profil konfigurieren (veraltet)")

    5. Wählen Sie im Fenster 'Gateway für mehrere Protokolle' unter **Backend-Einstellungen** die Option **Proxy-Profil** als **SSL-Clienttyp** und das in Schritt 4 erstellte SSL-Proxy-Profil aus.

        ![SSL-Proxy-Profil konfigurieren](images/bck_5.gif "Fenster mit Gateway für mehrere Protokolle")

### Front-Side-Einstellungen
{: #proxy-settings-frontside-settings}

* Für FCM und WNS:

    1. Erstellen Sie ein Schlüssel-Zertifikat-Paar und geben Sie als allgemeinen Namen (CN, Common Name) den DataPower-Hostnamen an:
        a. Navigieren Sie zu **Administration** > **Sonstiges** und klicken Sie auf **Verschlüsselungstools**.
        b. Geben Sie den Hostnamen von DataPower als Wert für dem allgemeinen Namen (CN) ein.
        c. Wählen Sie **Privaten Schlüssel exportieren** aus, wenn Sie den privaten Schlüssel später exportieren möchten, und klicken Sie auf **Schlüssel generieren**.

        ![Schlüssel/Zertifikat-Paar erstellen](images/frnt_1.gif "Schlüssel/Zertifikat-Paar erstellen")

    2. Erstellen Sie einen Berechtigungsnachweis zur Verschlüsselungsidentifikation:
        a. Navigieren Sie zu **Objekte** > **Verschlüsselungskonfiguration** und klicken Sie auf **Berechtigungsnachweise zur Verschlüsselungsidentifikation**.
        b. Klicken Sie auf **Hinzufügen**.
        c. Geben Sie einen eindeutigen Namen an.
        d. Wählen Sie für den Verschlüsselungsschlüssel und das Zertifikat den im vorherigen Schritt (Schritt 1) erstellten Schlüssel und das Zertifikat aus dem Listenfeld aus.
        e. Klicken Sie auf **Anwenden**.

        ![Berechtigungsnachweis zur Verschlüsselungsidentifikation erstellen](images/frnt_2.gif "Berechtigungsnachweis zur Verschlüsselungsidentifikation erstellen")

    3. Erstellen Sie ein Verschlüsselungsprofil:

        a. Navigieren Sie zu **Objekte** > **Verschlüsselungskonfiguration** und klicken Sie auf **Verschlüsselungsprofil**.
        b. Klicken Sie auf **Hinzufügen**.
        c. Geben Sie einen eindeutigen Namen an.
        d. Wählen Sie für 'Berechtigungsnachweise zur Identifikation' im Listenfeld den im vorherigen Schritt (Schritt 2) erstellten Berechtigungsnachweis für die Identifikation aus. Setzen Sie 'Berechtigungsnachweis für Validierung' auf 'keiner'.
        e. Klicken Sie auf **Anwenden**.

        ![Verschlüsselungsprofil erstellen](images/frnt_3.gif "Verschlüsselungsprofil erstellen")

    4. Erstellen Sie ein SSL-Proxy-Profil:

        a. Navigieren Sie zu **Objekte** > **Verschlüsselungskonfiguration** > **SSL-Proxy-Profil**.
        b. Klicken Sie auf **Hinzufügen**.
        c. Geben Sie einen eindeutigen Namen an.
        d. Wählen Sie für 'SSL-Richtung' die Option **Zurücknehmen** im Listenfeld aus.
        e. Wählen Sie für '(Server) Verschlüsselungsprofil zurücknehmen' das in vorherigen Schritt (Schritt 3) erstellte Verschlüsselungsprofil aus.  
        f. Klicken Sie auf **Anwenden**.

        ![SSL-Proxy-Profil konfigurieren](images/frnt_4.gif "SSL-Proxy-Profil konfigurieren")

    5. Erstellen Sie einen HTTPS-Front-Side-Handler:

        a. Navigieren Sie zu	**Objekte** > **Protokollhandler** > **HTTPS-Front-Side-Handler**.
        b. Klicken Sie auf **Hinzufügen**.
        c. Geben Sie einen eindeutigen Namen an.
        d. Wählen Sie für **Lokale IP-Adresse**entweder den richtigen Aliasnamen aus oder lassen Sie den Aliasnamen auf den Standardwert (0.0.0.0).
        e. Geben Sie einen verfügbaren Port an.
        f. Aktivieren Sie für **Zulässige Methoden und Versionen** die Kontrollkästchen 'HTTP 1.0', 'HTTP 1.1', 'POST-Methode', 'GET-Methode', 'URL mit ?', 'URL mit #', URL mit ..' aus.
        g. Wählen Sie **Proxy-Profil** als SSL-Servertyp aus.
        h. Wählen Sie für 'SSL-Proxy-Profil (veraltet)' das im vorherigen Schritt (Schritt 4) erstellte SSL-Proxy-Profil aus.
        i. Klicken Sie auf **Anwenden**.

        ![HTTPS-Front-Side-Handler konfigurieren](images/frnt_5.gif "HTTPS-Front-Side-Handler konfigurieren")

    6. Wählen Sie auf der Seite zum Konfigurieren des Gateways für mehrere Protokolle unter **Front-Side-Einstellungen** den in Schritt 5 erstellten HTTPS-Front-Side-Handler als **Front-Side-Protokoll** aus und klicken Sie auf **Anwenden**.

        ![Allgemeine Konfiguration](images/frnt_6.gif "Allgemeine Konfiguration")

    Das Zertifikat, das von DataPower in den Front-Side-Einstellungen verwendet wird, ist ein selbst signiertes Zertifikat. Verbindungen zu DataPower können erst hergestellt werden, wenn dieses Zertifikat zu dem von {{ site.data.keyword.mobilefirst_notm }} verwendeten JRE-Keystore hinzugefügt wurde.

## Weitere Schritte
{: #next-steps }

Führen Sie die erforderliche Konfiguration der serverseitigen und clientseitigen Seite aus, um Push-Benachrichtigungen zu senden und zu empfangen:

* [Push-Benachrichtigungen konfigurieren](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [Push-Benachrichtigungen senden](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [Handhabung von Push-Benachrichtigungen in Clientanwendungen](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [Benachrichtigungen im Hintergrund](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Interaktive Benachrichtigungen](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
