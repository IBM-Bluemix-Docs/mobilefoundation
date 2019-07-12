---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

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


# Push-Benachrichtigungen konfigurieren
{: #configure_push_notifications}

Für das Senden von Push- oder SMS-Benachrichtigungen an iOS-, Android- oder Windows-Geräte muss zunächst {{ site.data.keyword.mfserver_short_notm }} Server mit den FCM-Details für Android, einem APNS-Zertifikat für iOS oder mit WNS-Berechtigungsnachweisen für Windows 8.1 Universal/Windows 10 UWP konfiguriert werden.
{: shortdesc}

Benachrichtigungen können unter Verwendung der folgenden Optionen gesendet werden:
* alle Geräte (Broadcastbetrieb)
* Geräte, die für bestimmte Tags registriert sind
* eine einzelne Geräte-ID
* Benutzer-IDs
* nur iOS-Geräte
* nur Android-Geräte
* nur Windows-Geräte
* basierend auf dem authentifizierten Benutzer

## Benachrichtigungen einrichten
{: #setting-up-notifications }

Für die Aktivierung der Unterstützung für Benachrichtigungen müssen mehrere Konfigurationsschritte in {{ site.data.keyword.mfserver_short_notm }} und in der Clientanwendung ausgeführt werden. Fahren Sie mit dem Abschnitt zum serverseitigen Setup fort oder wechseln Sie zum Abschnitt [Clientseitige Konfiguration](#tutorials-to-follow-next).

Auf der Serverseite gehören zum Setup das Konfigurieren des erforderlichen Anbieters (APNS, FCM oder WNS) und die Zuordnung des Bereichs 'push.mobileclient'.

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

Google [unterstützt GCM nicht mehr](https://developers.google.com/cloud-messaging/faq) und hat das Cloud-Messaging mit Firebase integriert. Wenn Sie ein GCM-Projekt verwenden, müssen Sie [die GCM-Client-Apps für Android auf FCM migrieren](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: note}

Android-Geräte verwenden den Firebase Cloud Messaging-Service (FCM) für Push-Benachrichtigungen.

Gehen Sie wie folgt vor, um FCM zu konfigurieren:

1. Rufen Sie die [Firebase Console](https://console.firebase.google.com/?pli=1) auf.
2. Erstellen Sie ein Projekt und geben Sie einen Projektnamen an.
3. Klicken Sie auf das Zahnradsymbol für die Einstellungen und wählen Sie die **Projekteinstellungen** aus.
4. Klicken Sie auf die Registerkarte **Cloud-Messaging**, um einen **Server-API-Schlüssel** und eine **Absender-ID** zu generieren, und klicken Sie auf **Speichern**.

Sie können FCM auch mit der [REST-API für den {{ site.data.keyword.mobilefirst_notm }}-Push-Service](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) oder der [REST-API für den {{ site.data.keyword.mobilefirst_notm }}-Verwaltungsservice](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi) konfigurieren.
{: note}

Wenn Ihre Organisation über eine Firewall verfügt, die den Datenverkehr zum oder vom Internet einschränkt, müssen Sie die folgenden Schritte ausführen:  
* Konfigurieren Sie die Firewall so, dass die Konnektivität mit FCM möglich ist, damit Ihre FCM-Client-Apps Nachrichten empfangen können.
* Die Ports 5228, 5229 und 5230 müssen geöffnet werden. FCM verwendet normalerweise nur den Port 5228, manchmal aber auch die Ports 5229 und 5230.
* FCM stellt keine bestimmte IP-Adresse bereit. Stellen Sie daher sicher, dass Ihre Firewall abgehende Verbindungen zu allen IP-Adressen akzeptiert, die in den IP-Blöcken enthalten sind, die in Google ASN 15169 aufgelistet sind.
* Stellen Sie sicher, dass Ihre Firewall am Port 443 abgehende Verbindungen von {{ site.data.keyword.mfserver_short_notm }} zu fcm.googleapis.com akzeptiert.
{: note }

<img class="gifplayer" alt="Hinzufügen der GCM-Berechtigungsnachweise" src="images/gcm-setup.png"/>

### Apple Push Notifications Service
{: #apple-push-notifications-service }

iOS-Geräte verwenden den Apple Push Notification Service (APNS) für Push-Benachrichtigungen.  

Gehen Sie wie folgt vor, um APNS zu konfigurieren:

1. Generieren Sie ein Zertifikat für Push-Benachrichtigungen für die Entwicklung oder Produktion. Ausführliche Schritte finden Sie [hier](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) im Abschnitt `Für iOS`.
2. Wählen Sie in der {{ site.data.keyword.mfp_oc_short_notm }} unter **[Ihre Anwendung] → Push → Push-Einstellungen** den Zertifikatstyp aus und geben Sie die Zertifikatsdatei und das Kennwort an. Klicken Sie dann auf **Speichern**.

  * Damit Push-Benachrichtigungen gesendet werden können, müssen die folgenden Server über eine {{site.data.keyword.mfserver_short_notm }}-Instanz zugänglich sein:
    * Sandbox-Server:
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * Produktionsserver:
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * Verwenden Sie während der Entwicklungsphase die Sandbox-Zertifikatsdatei 'apns-certificate-sandbox.p12'.
  * Verwenden Sie während der Produktionsphase die Produktionszertifikatsdatei 'apns-certificate-production.p12'.
    * Das APNS-Produktionszertifikat kann erst getestet werden, wenn die Anwendung, die das Zertifikat verwendet, erfolgreich an den Apple App Store übergeben wurde.
    {: note }

MobileFirst bietet keine Unterstützung für Universal-Zertifikate.
{: note }

Sie können APNS auch mit der [REST-API für den {{ site.data.keyword.mobilefirst_notm }}-Push-Service](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) oder der [REST-API für den {{ site.data.keyword.mobilefirst_notm }}-Verwaltungsservice](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc) konfigurieren.
{: note}

<img class="gifplayer" alt="Hinzufügen der APNS-Berechtigungsnachweise" src="images/apns-setup.png"/>

### Windows Push Notifications Service
{: #windows-push-notifications-service }

Windows-Geräte verwenden den Windows Push Notifications Service (WNS) für Push-Benachrichtigungen.  

Gehen Sie wie folgt vor, um WNS zu konfigurieren:

1. Folgen Sie den [Anweisungen von Microsoft](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx) für das Generieren der Werte für **Paketsicherheits-ID (SID)** und **Geheimer Clientschlüssel**.
2. Fügen Sie diese Werte in der {{ site.data.keyword.mfp_oc_short_notm }} unter **[Ihre Anwendung] → Push → Push-Einstellungen** hinzu und klicken Sie auf **Speichern**.

Sie können WNS auch mit der [REST-API für den {{ site.data.keyword.mobilefirst_notm }}-Push-Service](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) oder der [REST-API für den {{ site.data.keyword.mobilefirst_notm }}-Verwaltungsservice](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc) konfigurieren.

<img class="gifplayer" alt="Hinzufügen der WNS-Berechtigungsnachweise" src="images/wns-setup.png"/>

### Bereichszuordnung
{: #scope-mapping }

Ordnen Sie das Bereichselement **push.mobileclient** der Anwendung zu.

1. Laden Sie die {{ site.data.keyword.mfp_oc_short_notm }}, navigieren Sie zu **[Ihre Anwendung] → Sicherheit → Zuordnung von Bereichselementen** und klicken Sie auf **Neu**.
2. Schreiben Sie 'psh.mobileclient' in das Feld **Bereichselement**. Klicken Sie dann auf **Hinzufügen**.

Liste zusätzlich verfügbarer Bereiche:

|**Bereiche** | **Beschreibung**|
|---|---|
|apps.read | Berechtigung zum Lesen der Anwendungsressource. |
|apps.write | Berechtigung zum Erstellen, Aktualisieren und Löschen der Anwendungsressource. |
|gcmConf.read | Berechtigung zum Lesen der GCM-Konfigurationseinstellungen (API-Schlüssel und Absender-ID). |
|gcmConf.write | Berechtigung zum Aktualisieren und Löschen der GCM-Konfigurationseinstellungen. |
|apnsConf.read | Berechtigung zum Lesen der APNS-Konfigurationseinstellungen. |
|apnsConf.write | Berechtigung zum Aktualisieren und Löschen der APNS-Konfigurationseinstellungen. |
|devices.read | Berechtigung zum Lesen eines Geräts. |
|devices.write | Berechtigung zum Erstellen, Aktualisieren und Löschen eines Geräts. |
|subscriptions.read | Berechtigung zum Lesen von Abonnements. |
|subscriptions.write | Berechtigung zum Erstellen, Aktualisieren und Löschen von Abonnements. |
|messages.write | Berechtigung zum Senden von Push-Benachrichtigungen. |
|webhooks.read | Berechtigung zum Lesen von Ereignisbenachrichtigungen. |
|webhooks.write | Berechtigung zum Senden von Ereignisbenachrichtigungen.|
|smsConf.read | Berechtigung zum Lesen von SMS-Konfigurationseinstellungen.|
|smsConf.write | Berechtigung zum Aktualisieren und Löschen von SMS-Konfigurationseinstellungen.|
|wnsConf.read | Berechtigung zum Lesen der WNS-Konfigurationseinstellungen.|
|wnsConf.write | Berechtigung zum Aktualisieren und Löschen der WNS-Konfigurationseinstellungen.|
{: caption="Tabelle 1. Bereichsbeschreibungen" caption-side="top"}

<img class="gifplayer" alt="Bereichszuordnung" src="images/scope-mapping.png"/>

### Authentifizierte Benachrichtigungen
{: #authenticated-notifications }

Authentifizierte Benachrichtigungen sind Benachrichtigungen, die an eine oder mehrere Benutzer-IDs (`userIds`) gesendet werden.  

Ordnen Sie das Bereichselement **push.mobileclient** der Sicherheitsprüfung zu, die für die Anwendung verwendet wird.  

1. Laden Sie die {{ site.data.keyword.mfp_oc_short_notm }} und navigieren Sie zu **[Ihre Anwendung] → Sicherheit → Zuordnung von Bereichselementen**. Klicken Sie auf **Neu** oder bearbeiten Sie einen vorhandenen Bereichszuordnungseintrag.
2. Wählen Sie eine Sicherheitsprüfung aus. Klicken Sie dann auf **Hinzufügen**.

  <img class="gifplayer" alt="Authentifizierte Berechtigungen" src="images/authenticated-notifications.png"/>

## Tags definieren
{: #defining-tags }
Klicken Sie in der {{ site.data.keyword.mfp_oc_short_notm }} unter **[Ihre Anwendung] → Push → Tags** auf **Neu**.  
Geben Sie den entsprechenden `Tagnamen` und eine `Beschreibung` an und klicken Sie auf **Speichern**.

<img class="gifplayer" alt="Hinzufügen von Tags" src="images/adding-tags.png"/>

Bei einem Abonnement werden eine Geräteregistrierung und ein Tag miteinander verbunden. Wenn die Registrierung eines Gerätes für einen Tag aufgehoben wird, beendet das Gerät selbst alle zugehörigen Abonnements. Wenn ein Gerät von mehreren Benutzern verwendet wird, sollten Abonnements auf der Basis von Benutzeranmeldungskriterien in mobilen Anwendungen implementiert werden. Der Aufruf für das Abonnement könnte beispielsweise erfolgen, nachdem sich ein Benutzer erfolgreich bei einer Anwendung angemeldet hat. Der Aufruf zum Beenden des Abonnements würde explizit im Rahmen der Abmeldeaktion abgesetzt.

## Nächste Lernprogramme
{: #tutorials-to-follow-next }

* [Push-Benachrichtigungen senden](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

Da die Serverseite jetzt eingerichtet ist, fahren Sie mit dem clientseitigen Setup und der Handhabung empfangener Benachrichtigungen fort.

* [Handhabung von Push-Benachrichtigungen in Clientanwendungen](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
