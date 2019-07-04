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


# Configura notifiche di push
{: #configure_push_notifications}

Per poter inviare notifiche di push ai dispositivi iOS, Android o Windows, {{ site.data.keyword.mfserver_short_notm }} deve essere innanzitutto configurato con i dettagli FCM per Android, un certificato APNS per iOS o le credenziali WNS per Windows 8.1 Universal / Windows 10 UWP.
{: shortdesc}

Le notifiche possono essere inviate utilizzando le seguenti opzioni:
* tutti i dispositivi (broadcast)
* dispositivi con registrazione a specifiche tag
* un singolo ID dispositivo
* ID utente
* solo dispositivi iOS
* solo dispositivi Android
* solo dispositivi Windows
* in base all'utente autenticato.

## Configurazione delle notifiche
{: #setting-up-notifications }

L'abilitazione del supporto delle notifiche comporta diversi passi di configurazione sia in {{ site.data.keyword.mfserver_short_notm }} che nell'applicazione client. Continua a leggere per la configurazione del lato server oppure passa a [Configurazione del lato client](#tutorials-to-follow-next).

Sul lato server, la configurazione necessaria include: configurazione del fornitore richiesto (APNS, FCM o WNS) e associazione dell'ambito "push.mobileclient".

### CFM (Firebase Cloud Messaging)
{: #firebase-cloud-messaging }

Google [ha dichiarato obsoleto GCM](https://developers.google.com/cloud-messaging/faq) e ha integrato Cloud Messaging con Firebase. Se stai utilizzando un progetto GCM, assicurati di [migrare le applicazioni client GCM su Android a FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: note}

I dispositivi Android utilizzano il servizio Firebase Cloud Messaging (FCM) per le notifiche di push.

Per configurare FCM:

1. Visita il sito [Firebase Console](https://console.firebase.google.com/?pli=1).
2. Crea un progetto e specifica un nome progetto.
3. Fai clic sull'icona a forma di ingranaggio Settings e seleziona **Project settings**.
4. Fai clic sulla scheda **Cloud Messaging** per generare un **Server API Key** e un **Sender ID** e fai clic su **Save**.

Puoi anche configurare FCM utilizzando l'[API REST per il {{ site.data.keyword.mobilefirst_notm }} servizio Push](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) oppure l'[API REST per il {{ site.data.keyword.mobilefirst_notm }} servizio di amministrazione](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi).
{: note}

Se la tua organizzazione ha un firewall che limita il traffico da o verso Internet, devi eseguire la procedura riportata di seguito:  
* Configura il firewall per consentire la connettività con FCM in modo che le tue applicazioni client FCM ricevano i messaggi.
* Le porte da aprire sono 5228, 5229 e 5230. Di norma, FCM utilizza solo 5228, ma a volte utilizza 5229 e 5230.
* FCM non fornisce un IP specifico, quindi devi consentire al tuo firewall di accettare le connessioni in uscita a tutti gli indirizzi IP contenuti nei blocchi di IP elencati nell'ASN 15169 di Google.
* Assicurati che il tuo firewall accetti le connessioni in uscita da {{ site.data.keyword.mfserver_short_notm }} a fcm.googleapis.com sulla porta 443.
{: note }

<img class="gifplayer" alt="Immagine dell'aggiunta delle credenziali GCM" src="images/gcm-setup.png"/>

### APNS (Apple Push Notifications Service)
{: #apple-push-notifications-service }

I dispositivi iOS utilizzano APNS (Apple Push Notification Service) per le notifiche di push.  

Per configurare APNS:

1. Genera un certificato della notifica di push per lo sviluppo o la produzione. Per i passi dettagliati, fai riferimento alla sezione `Per iOS` [qui](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1).
2. In {{ site.data.keyword.mfp_oc_short_notm }} → **[tua applicazione] → Push → Push Settings**, seleziona il tipo di certificato e fornisci il file e la password del certificato. Quindi, fai clic su **Save**.

  * Affinché le notifiche di push vengano inviate, è necessario che i seguenti server siano accessibili da un'istanza {{ site.data.keyword.mfserver_short_notm }}:
    * Server sandbox:
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * Server di produzione:
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * Durante la fase di sviluppo, utilizza il file certificato sandbox apns-certificate-sandbox.p12.
  * Durante la fase di produzione, utilizza il file certificato di produzione apns-certificate-production.p12.
    * Il certificato di produzione APNS può essere verificato solo quando l'applicazione che lo utilizza è stata inoltrata correttamente all'App Store di Apple.
    {: note }

MobileFirst non supporta i certificati Universal.
{: note }

Puoi anche configurare APNS utilizzando l'[API REST per il {{ site.data.keyword.mobilefirst_notm }} servizio Push](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) oppure l'[API REST per il {{ site.data.keyword.mobilefirst_notm }} servizio di amministrazione](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc).
{: note}

<img class="gifplayer" alt="Immagine dell'aggiunta delle credenziali APNS" src="images/apns-setup.png"/>

### WNS (Windows Push Notifications Service)
{: #windows-push-notifications-service }

I dispositivi Windows utilizzano WNS (Windows Push Notifications Service) per le notifiche di push.  

Per configurare WNS:

1. Segui le [istruzioni fornite da Microsoft](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx) per generare i valori **Package Security Identifier (SID)** e **Client secret**.
2. In {{ site.data.keyword.mfp_oc_short_notm }} → **[tua applicazione] → Push → Push Settings**, aggiungi questi valori e fai clic su **Save**.

Puoi anche configurare WNS utilizzando l'[API REST per il {{ site.data.keyword.mobilefirst_notm }} servizio Push](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) oppure l'[API REST per il {{ site.data.keyword.mobilefirst_notm }} servizio di amministrazione](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc)

<img class="gifplayer" alt="Immagine dell'aggiunta delle credenziali WNS" src="images/wns-setup.png"/>

### Associazione dell'ambito
{: #scope-mapping }

Associa l'elemento di ambito **push.mobileclient** all'applicazione.

1. Carica {{ site.data.keyword.mfp_oc_short_notm }} e vai a **[tua applicazione] → Security → Scope-Elements Mapping**, fai clic su **New**.
2. Scrivi "push.mobileclient" nel campo **Scope element**. Quindi, fai clic su **Add**.

Elenco di altri ambiti disponibili: 

|**Ambiti** | **Descrizione**|
|---|---|
|apps.read | Autorizzazione per leggere la risorsa dell'applicazione. |
|apps.write | Autorizzazione per creare, aggiornare, eliminare la risorsa dell'applicazione. |
|gcmConf.read | Autorizzazione per leggere le impostazioni di configurazione GCM (Chiave API e SenderId). |
|gcmConf.write | Autorizzazione per aggiornare, eliminare le impostazioni di configurazione di GCM. |
|apnsConf.read | Autorizzazione per leggere le impostazioni di configurazione di APNS. |
|apnsConf.write | Autorizzazione per aggiornare, eliminare le impostazioni di configurazione di APNS. |
|devices.read | Autorizzazione per leggere il dispositivo. |
|devices.write | Autorizzazione per creare, aggiornare, eliminare il dispositivo. |
|subscriptions.read | Autorizzazione per leggere le sottoscrizioni. |
|subscriptions.write | Autorizzazione per creare, aggiornare, eliminare le sottoscrizioni. |
|messages.write | Autorizzazione per inviare le notifiche di push. |
|webhooks.read | Autorizzazione per leggere le notifiche evento. |
|webhooks.write | Autorizzazione per inviare le notifiche evento.|
|smsConf.read | Autorizzazione per leggere le impostazioni di configurazione di SMS.|
|smsConf.write | Autorizzazione per aggiornare, eliminare le impostazioni di configurazione di SMS.|
|wnsConf.read | Autorizzazione per leggere le impostazioni di configurazione di WNS.|
|wnsConf.write | Autorizzazione per aggiornare, eliminare le impostazioni di configurazione di WNS.|
{: caption="Tabella 1. Descrizioni dell'ambito" caption-side="top"}

<img class="gifplayer" alt="Associazione dell'ambito" src="images/scope-mapping.png"/>

### Notifiche autenticate
{: #authenticated-notifications }

Le notifiche autenticate sono notifiche che vengono inviate a uno o più `userIds`.  

Associa l'elemento di ambito **push.mobileclient** al controllo di sicurezza utilizzato per l'applicazione.  

1. Carica {{ site.data.keyword.mfp_oc_short_notm }} e vai a **[tua applicazione] → Security → Scope-Elements Mapping**, fai clic su **New** oppure modifica una voce di associazione dell'ambito esistente.
2. Seleziona un controllo di sicurezza. Quindi, fai clic su **Add**.

  <img class="gifplayer" alt="Notifiche autenticate" src="images/authenticated-notifications.png"/>

## Definizione di tag
{: #defining-tags }
In {{ site.data.keyword.mfp_oc_short_notm }} → **[tua applicazione] → Push → Tag**, fai clic su **Nuovo**.  
Specifica `Nome tag` e `Descrizione` e fai clic su **Salva**.

<img class="gifplayer" alt="Aggiunta di tag" src="images/adding-tags.png"/>

Le sottoscrizioni collegano una registrazione e una tag. Quando un dispositivo annulla la registrazione a una tag, tutte le sottoscrizioni associate verranno annullate automaticamente dal dispositivo stesso. In uno scenario in cui esistono più utenti di un dispositivo, le sottoscrizioni devono essere implementate nelle applicazioni mobili in base a criteri di accesso utente. Ad esempio, la chiamata di sottoscrizione viene effettuata dopo che un utente si è collegato correttamente ad un'applicazione e la chiamata di annullamento della sottoscrizione viene effettuata esplicitamente come parte della gestione dell'azione di disconnessione.

## Esercitazioni da seguire dopo
{: #tutorials-to-follow-next }

* [Invia notifiche di push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

Con la configurazione del lato server eseguita, configura il lato client e gestisci le notifiche ricevute.

* [>Gestione delle notifiche di push nelle applicazioni client](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
