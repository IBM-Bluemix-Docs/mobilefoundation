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


# Invia notifiche di push
{: #send_push_notifications}

Le notifiche di push possono essere inviate da {{ site.data.keyword.mfp_oc_short_notm }} o tramite le API REST.
{: shortdesc}

* Con {{ site.data.keyword.mfp_oc_short_notm }}, possono essere inviati due tipi di notifiche: basate su tag e broadcast.
* Con le API REST, possono essere inviati tutti i formati di notifiche: basate su tag, broadcast e autenticate.

## Invio di notifiche di push da {{ site.data.keyword.mfp_oc_short_notm }}
{: #sending-push-notification-from-mobilefirst-operations-console }

Le notifiche possono essere inviate a un singolo ID dispositivo o a diversi ID dispositivo, solo a dispositivi iOS o solo a dispositivi Android o a dispositivi con sottoscrizioni a tag.

### Notifiche basate su tag
{: #tag-notifications }

Le notifiche basate su tag sono messaggi di notifica destinati a tutti i dispositivi che hanno una sottoscrizione a una specifica tag. Le tag rappresentano argomenti di interesse per l'utente e forniscono la capacità di ricevere notifiche in base all'interesse scelto.

Nella scheda {{ site.data.keyword.mfp_oc_short_notm }} → **[tua applicazione] → Push → Send Notifications**, seleziona **Devices By Tags** dalla scheda **Send To** e specifica il **testo della notifica**. Quindi, fai clic su **Send**.

<img class="gifplayer" alt="Invio tramite tag" src="images/sending-by-tag.png"/>

### Notifiche broadcast
{: #broadcast-notifications }

Le notifiche broadcast sono una forma di notifiche di push basate su tag indirizzate a tutti i dispositivi sottoscritti. Le notifiche broadcast sono abilitate per impostazione predefinita per qualsiasi applicazione {{ site.data.keyword.mobilefirst_notm }} abilitata per il push tramite una sottoscrizione a una tag `Push.all` riservata (creata automaticamente per ogni dispositivo). L'annullamento della sottoscrizione alla tag `Push.all` può essere eseguito in modo programmatico.

Nella scheda {{ site.data.keyword.mfp_oc_short_notm }} → **[tua applicazione] → Push → Send Notifications**, seleziona **All** dalla scheda **Send To** e fornisci il **testo della notifica**. Quindi, fai clic su **Send**.

<img class="gifplayer" alt="Invio a tutti" src="images/sending-to-all.png"/>

## Invio di notifiche di push utilizzando le API REST
{: #sending-push-notifications-using-rest-apis }

Quando utilizzi le API REST per inviare le notifiche, possono essere inviati tutti i formati di notifiche: notifiche basate su tag, broadcast e autenticate.

Per inviare una notifica, viene effettuata una richiesta utilizzando POST nell'endpoint REST: `imfpush/v1/apps/<application-identifier>/messages`.  
Quello che segue è un URL di esempio:

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

Per esaminare tutte le API REST delle notifiche di push, vedi l'[argomento REST API Runtime Services](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html) nella documentazione utente.

### Payload delle notifiche
{: #notification-payload }

La richiesta può contenere le seguenti proprietà del payload:

|Proprietà payload| Definizione
|--- | ---
|message | Il messaggio di avviso da inviare |
|settings | Le impostazioni sono i diversi attributi della notifica. |
|target | La serie delle destinazioni può essere ID consumatore, dispositivi, piattaforme o tag. Può essere impostata solo una delle destinazioni. |
|deviceIds | Un array dei dispositivi rappresentati tramite gli identificativi dispositivo. I dispositivi con questi ID ricevono la notifica. Questa è una notifica unicast. |
|notificationType | Il valore intero per indicare il canale (Push o SMS) utilizzato per inviare il messaggio. I valori consentiti sono 1 (per solo Push), 2 (per solo SMS) e 3 (per sia Push che SMS) |
|platforms | Un array delle piattaforme del dispositivo. I dispositivi su cui sono in esecuzione queste piattaforme ricevono la notifica. I valori supportati sono A (Apple/iOS), G (Google/Android) e M (Microsoft/Windows). |
|tagNames | Un array delle tag specificate come tagNames. I dispositivi che hanno una sottoscrizione a queste tag ricevono la notifica. Utilizza questo tipo di destinazione per le notifiche basate su tag. |
|userIds | Un array degli utenti rappresentati dai loro userId a cui inviare la notifica. Questa è una notifica unicast. |
|phoneNumber | Il numero di telefono utilizzato per registrare il dispositivo e ricevere le notifiche. Questa è una notifica unicast. |
{: caption="Tabella 1. Proprietà del payload" caption-side="top"}

**Esempio di JSON del payload delle notifiche di push**

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
    // The following list is for demonstration purposes only - per the documentation only 1 target is allowed to be used at a time.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**Esempio di JSON del payload delle notifiche SMS**

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

## Invio della notifica
{: #sending-the-notification }

La notifica può essere inviata utilizzando strumenti diversi. A scopo di test, viene utilizzato **Postman**, i passi riportati di seguito descrivono la configurazione:

1. [Configura un client riservato](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
  L'invio di una notifica di push tramite l'API REST utilizza gli elementi dell'ambito separati da spazi `messages.write` e `push.application.<applicationId>.`
  <img class="gifplayer" alt="Configura un client riservato" src="images/push-confidential-client.png"/>
2. [Crea token di accesso](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token).  
3. Effettua una richiesta **POST** su **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages**
  - Se stai utilizzando un {{ site.data.keyword.mobilefirst_notm }} remoto, sostituisci i valori `hostname` e `port` con i tuoi.
  - Aggiorna il valore dell'identificativo dell'applicazione con il tuo.
4. Imposta un'intestazione:
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - Sostituisci il valore dopo **Bearer** con il valore del tuo token di accesso del passo (1).
    ![Intestazione di autorizzazione](images/postman_authorization_header.png "Intestazione di autorizzazione")
5. Imposta un corpo:
  - Aggiornane le proprietà come descritto in [Payload delle notifiche](#notification-payload).
  - Ad esempio, aggiungendo la proprietà **target** con l'attributo **userIds**, puoi inviare una notifica a specifici utenti registrati.
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![Corpo dell'autorizzazione](images/postman_json.png "Corpo dell'autorizzazione")

    Dopo aver fatto clic sul pulsante **Send**, il dispositivo riceve una notifica:

    ![Immagine dell'applicazione di esempio](images/notifications-app.png "Notifica push su un dispositivo mobile")

## Personalizzazione delle notifiche
{: #customizing-notifications }

Prima di inviare il messaggio di notifica, puoi anche personalizzare i seguenti attributi di notifica.  

Nella scheda {{ site.data.keyword.mfp_oc_short_notm }} → **[tua applicazione] → Push → Tags → Send Notifications**, espandi la sezione **iOS/Android Custom Settings** per modificare gli attributi della notifica.

### Android
{: #android }

* Notifica sonora, per quanto tempo una notifica può essere archiviata nell'archiviazione FCM, payload personalizzato e altro.
* Se desideri modificare il titolo della notifica, aggiungi `push_notification_tile` nel file **strings.xml** del tuo progetto Android.

### iOS
{: #ios }

* Notifica sonora, payload personalizzato, titolo chiave dell'azione, tipo di notifica e numero badge.

  ![Personalizzazione delle notifiche push](images/customizing-push-notifications.png "Pagina Mobile First Operations Console Push con la scheda Send Push selezionata")

## Supporto HTTP/2 per le notifiche di push APNs
{: #http2-support-for-apns-push-notifications}

APNs (Apple Push Notification service) supporta una nuova API basata sul protocollo di rete HTTP/2. Il supporto per HTTP/2 offre molti vantaggi, incluso quanto segue:

* Lunghezza del messaggio aumentata da 2 KB a 4 KB, ciò consente di aggiungere contenuto supplementare alle notifiche.
* Elimina la necessità di più connessioni tra client e server e ciò migliora la velocità effettiva.
* Supporto per il certificato SSL di Universal Push Notification Client.

>Le notifiche di push in {{ site.data.keyword.mobilefirst_notm }} ora supportano le notifiche di push APNs basate su HTTP/2 insieme alle notifiche socket TCP legacy.

### Abilitazione di HTTP/2
{: #enabling-http2}

Le notifiche basate su HTTP/2 possono essere abilitate utilizzando una proprietà JNDI.
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

Se viene aggiunta la proprietà JNDI, il socket TCP legacy basato sulle notifiche non viene utilizzato e vengono abilitate solo le notifiche basate su HTTP/2.
{: note}

### Supporto proxy per HTTP/2
{: #proxy-support-for-http2}

Le notifiche basate su HTTP/2 possono essere inviate tramite un proxy HTTP. Per abilitare l'instradamento delle notifiche tramite un proxy, vedi [qui](#proxy-support).

## Supporto proxy
{: #proxy-support }
Puoi utilizzare le impostazioni proxy per impostare il proxy facoltativo tramite cui le notifiche vengono inviate ai dispositivi Android e iOS. Puoi impostare il proxy utilizzando le proprietà di configurazione `push.apns.proxy.*` e `push.gcm.proxy.*`. Per ulteriori informazioni, vedi [List of JNDI properties for {{ site.data.keyword.mfserver_short_notm }} push service](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

## Passi successivi
{: #next-tutorial-to-follow }
Con la configurazione del lato server eseguita, configura il lato client e gestisci le notifiche ricevute utilizzando la seguente esercitazione.

* [>Gestione delle notifiche di push nelle applicazioni client](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
