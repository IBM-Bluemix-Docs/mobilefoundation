---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, sending notification, HTTP/2

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


# Push-Benachrichtigungen senden
{: #send_push_notifications}

Push-Benachrichtigungen können entweder über die {{ site.data.keyword.mfp_oc_short_notm }} oder über REST-APIs gesendet werden.
{: shortdesc}

* Von der {{ site.data.keyword.mfp_oc_short_notm }} aus können zwei Arten von Benachrichtigungen gesendet werden: tagbasierte Benachrichtigungen und Broadcastbenachrichtigungen.
* Mit den REST-APIs können alle Arten von Benachrichtigungen gesendet werden: tagbasierte und authentifizierte Benachrichtigungen sowie Broadcastbenachrichtigungen.

## Push-Benachrichtigungen über die {{ site.data.keyword.mfp_oc_short_notm }} senden
{: #sending-push-notification-from-mobilefirst-operations-console }

Benachrichtigungen können an eine einzelne Geräte-ID, an eine einzelne oder an mehrere Benutzer-IDs, nur an iOS-Geräte oder nur an Android-Geräte oder an Geräte, die Tags abonniert haben, gesendet werden.

### Tagbasierte Benachrichtigungen
{: #tag-notifications }

Tagbasierte Benachrichtigungen sind Hinweisnachrichten, die an alle Geräte gesendet werden, die einen bestimmten Tag abonniert haben. Tags stehen für Themen, die für den Benutzer von Interesse sind, und ermöglichen dem Benutzer, Benachrichtigungen zu den ihn interessierenden Themen zu erhalten.

Wählen Sie in der {{ site.data.keyword.mfp_oc_short_notm }} auf der Registerkarte **[Ihre Anwendung] → Push → Benachrichtigungen senden** die Option **Geräte nach Tags** unter **Senden an** aus und geben Sie den **Benachrichtigungstext** an. Klicken Sie dann auf **Senden**.

<img class="gifplayer" alt="Nach Tag senden" src="images/sending-by-tag.png"/>

### Broadcastbenachrichtigungen
{: #broadcast-notifications }

Broadcastbenachrichtigungen sind eine Form der tagbasierten Push-Benachrichtigungen, die an alle abonnierten Geräte gesendet werden. Broadcastbenachrichtigungen werden standardmäßig für alle Push-fähigen {{ site.data.keyword.mobilefirst_notm }}-Anwendungen durch das Abonnement eines reservierten Tags `Push.all` aktiviert. Der Tag wird automatisch für jedes Gerät erstellt. Das Abonnement des Tags `Push.all` kann programmgestützt beendet werden.

Wählen Sie in der {{ site.data.keyword.mfp_oc_short_notm }} auf der Registerkarte **[Ihre Anwendung] → Push → Benachrichtigungen senden** die Option **Alle** unter **Senden an** aus und geben Sie den **Benachrichtigungstext** an. Klicken Sie dann auf **Senden**.

<img class="gifplayer" alt="An alle senden" src="images/sending-to-all.png"/>

## Push-Benachrichtigungen mithilfe von REST-APIs senden
{: #sending-push-notifications-using-rest-apis }

Wenn sie die REST-APIs zu senden von Benachrichtigungen verwenden, können alle Arten von Benachrichtigungen gesendet werden: tagbasierte Benachrichtigungen und Broadcastbenachrichtigungen sowie authentifizierte Benachrichtigungen.

Zum Senden einer Benachrichtigung wird eine POST-Anforderung an den REST-Endpunkt abgesetzt: `imfpush/v1/apps/<application-identifier>/messages`.  
Beispiel-URL:

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

Eine Übersicht über alle REST-APIs für Push-Benachrichtigungen finden Sie im Abschnitt [REST-API-Laufzeitservices](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html) in der Benutzerdokumentation.

### Nutzdaten von Benachrichtigungen
{: #notification-payload }

Die Anforderung kann die folgenden Nutzdateneigenschaften enthalten:

|Eigenschaften von Nutzdaten| Definition
|--- | ---
|message | Die zu sendende Alertnachricht. |
|settings | Die Einstellungen sind die verschiedenen Attribute der Benachrichtigung. |
|target | Ziele können Consumer-IDs, Geräte, Plattformen oder Tags sein. Es kann nur ein Ziel festgelegt werden. |
|deviceIds | Ein Array der Geräte, die durch die Gerätekennungen repräsentiert werden. Geräte mit diesen IDs empfangen die Benachrichtigung. Es handelt sich hierbei um eine Unicastbenachrichtigung. |
|notificationType | Ganzzahliger Wert für den Kanal (Push/SMS), über den die Nachricht gesendet wird. Gültige Werte sind 1 (nur Push), 2 (nur SMS) und 3 (Push und SMS). |
|platforms | Ein Array von Geräteplattformen. Geräte, die auf diesen Plattformen ausgeführt werden, erhalten die Benachrichtigung. Unterstützte Werte sind A (Apple/iOS), G (Google/Android) und M (Microsoft/Windows). |
|tagNames | Ein Array mit Tags, die als Tagnamen angegeben sind. Geräte, die diese Tags abonniert haben, empfangen die Benachrichtigung. Verwenden Sie diesen Typ von Ziel für tagbasierte Benachrichtigungen. |
|userIds | Array mit Benutzern, repräsentiert durch die Benutzer-IDs, an die die Benachrichtigung gesendet wird. Es handelt sich hierbei um eine Unicastbenachrichtigung. |
|phoneNumber | Die Telefonnummer, die für die Registrierung der Einheit und für den Empfang von Benachrichtigungen verwendet wird. Es handelt sich hierbei um eine Unicastbenachrichtigung. |
{: caption="Tabelle 1. Eigenschaften der Nutzdaten" caption-side="top"}

**JSON-Beispiel für die Nutzdaten von Push-Benachrichtigungen**

```json
{
    "message" : {
    "alert" : "Test message",
  },
  "settings" : {
    "apns" : {
      "badge" : 1,
      "iosActionKey" : "Ok",
      "payload" : "",
      "sound" : "song.mp3",
      "type" : "SILENT",
    },
    "gcm" : {
      "delayWhileIdle" : ,
      "payload" : "",
      "sound" : "song.mp3",
      "timeToLive" : ,
    },
  },
  "target" : {
    // Die folgende Liste dient nur zur Demonstration. Gemäß Dokumentation ist immer nur jeweils 1 Ziel erlaubt.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**JSON-Beispiel für die Nutzdaten von SMS-Benachrichtigungen**

```json
{
  "message": {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds" : ["38cc1c62-03bb-36d8-be8e-af165e671cf4"]
   }
}
```

## Benachrichtigung senden
{: #sending-the-notification }

Die Benachrichtigung kann mit verschiedenen Tools gesendet werden. Nachfolgend wird zu Testzwecken **Postman** verwendet.

1. [Konfigurieren Sie einen vertraulichen Client](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
  Wenn Sie eine Push-Benachrichtigung über die REST-API senden, werden die durch Leerzeichen getrennten Bereichselemente `messages.write` und `push.application<applicationId> verwendet.`
  <img class="gifplayer" alt="Vertraulichen Client konfigurieren" src="images/push-confidential-client.png"/>
2. [Erstellen Sie ein Zugriffstoken](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token).  
3. Setzen Sie eine **POST**-Anforderung an **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages** ab.
  - Wenn Sie {{ site.data.keyword.mobilefirst_notm }} über Fernzugriff verwenden, ersetzen Sie die Werte für `hostname` und `port` durch Ihre eigenen Werte.
  - Aktualisieren Sie die Werte für die Anwendungs-ID durch Ihre eigenen Werte.
4. Legen Sie einen Header fest:
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - Ersetzen Sie den Wert nach **Bearer** durch den Wert Ihres Zugriffstokens aus Schritt (1).
    ![Berechtigungsheader](images/postman_authorization_header.png "Berechtigungsheader")
5. Legen Sie einen Hauptteil fest:
  - Aktualisieren Sie die Eigenschaften gemäß der Beschreibung im obigen Abschnitt [Nutzdaten von Berechtigungen](#notification-payload).
  - Fügen Sie beispielsweise die Eigenschaft **target** mit dem Attribut **userIds** hinzu, wenn Sie eine Benachrichtigung an bestimmte registrierte Benutzer senden möchten.
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![Berechtigungshauptteil](images/postman_json.png "Berechtigungshauptteil")

    Nachdem Sie auf die Schaltfläche **Senden** geklickt haben, empfängt das Gerät eine Benachrichtigung:

    ![Abbildung der Beispielanwendung](images/notifications-app.png "Push-Benachrichtigung auf einem Mobilgerät")

## Benachrichtigungen anpassen
{: #customizing-notifications }

Vor dem Senden der Benachrichtigung können Sie die folgenden Benachrichtigungsattribute anpassen.  

Erweitern Sie in der {{ site.data.keyword.mfp_oc_short_notm }} auf der Registerkarte **[Ihre Anwendung] → Push → Tags → Benachrichtigung senden** den Abschnitt **Angepasste iOS/Android-Einstellungen**, um die Benachrichtigungsattribute zu ändern.

### Android
{: #android }

* Benachrichtigungston, Dauer der Aufbewahrung einer Benachrichtigung im FCM-Speicher, angepasste Nutzdaten usw.
* Wenn Sie den Benachrichtigungstitel ändern möchten, fügen Sie `push_notification_tile` zur Datei **strings.xml** des Android-Projekts hinzu.

### iOS
{: #ios }

* Benachrichtigungston, angepasste Nutzdaten, Titel für Aktionsschlüssel, Benachrichtigungstyp und Badgenummer.

  ![Push-Benachrichtigungen anpassen](images/customizing-push-notifications.png "Push-Seite der Mobile First-Operationskonsole mit ausgewählter Registerkarte zum Senden von Push-Benachrichtigungen")

## HTTP/2-Unterstützung für Push-Benachrichtigungen von APNS
{: #http2-support-for-apns-push-notifications}

Der Apple Push Notification Service (APNS) unterstützt ein neues, API-basiertes HTTP/2-Netzprotokoll. Die Unterstützung für HTTP/2 bringt unter anderem folgende Vorteile mit sich:

* Die Nachrichtenlänge, die von 2 KB auf 4 KB erhöht wird, ermöglicht es, zusätzlichen Inhalt zu Benachrichtigungen hinzuzufügen.
* Zwischen Client und Server sind nicht mehr mehrere Verbindungen notwendig, sodass sich der Durchsatz verbessert.
* Für universelle Push-Benachrichtigungen werden Client-SSL-Zertifikate unterstützt.

>{{ site.data.keyword.mobilefirst_notm }} bietet jetzt neben der Unterstützung für traditionelle, auf TCP-Sockets basierende Benachrichtigungen auch Unterstützung für auf HTTP/2 basierende APNS-Push-Benachrichtigungen.

### HTTP/2 aktivieren
{: #enabling-http2}

HTTP/2-basierte Benachrichtigungen können über eine JNDI-Eigenschaft aktiviert werden.
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

Wenn die obige JNDI-Eigenschaft hinzugefügt wird, werden keine traditionellen, auf TCP-Sockets basierenden Benachrichtigungen verwendet, sondern nur die HTTP/2-basierten Benachrichtigungen.
{: note}

### Proxy-Unterstützung für HTTP/2
{: #proxy-support-for-http2}

HTTP/2-basierte Benachrichtigungen können über einen HTTP-Proxy gesendet werden. Informationen zum Aktivieren der Weiterleitung von Benachrichtigungen über einen Proxy finden Sie [hier](#proxy-support).

## Proxy-Unterstützung
{: #proxy-support }
In den Proxy-Einstellungen können Sie den optionalen Proxy festlegen, über den Benachrichtigungen an Android- und iOS-Geräte gesendet werden. Verwenden Sie die Konfigurationseigenschaften `push.apns.proxy.*` und `push.gcm.proxy.*`, um den Proxy festzulegen. Weitere Informationen finden Sie in der [Liste der JNDI-Eigenschaften für den {{ site.data.keyword.mfserver_short_notm }}-Push-Service](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

## Weitere Schritte
{: #next-tutorial-to-follow }
Da die Serverseite jetzt eingerichtet ist, fahren Sie mit dem clientseitigen Setup und der Handhabung empfangener Benachrichtigungen fort.

* [Handhabung von Push-Benachrichtigungen in Clientanwendungen](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
