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

# Notifiche di push
{: #push_notifications}

Le notifiche costituiscono la capacità di un dispositivo mobile di ricevere messaggi di cui un server ha eseguito il push. Le notifiche vengono ricevute indipendentemente dal fatto che l'applicazione sia in esecuzione in primo piano o in background.  
{: shortdesc}

{{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} fornisce una serie unificata di metodi API per inviare notifiche di push alle applicazioni iOS, Android, Windows 8.1 Universal, Windows 10 UWP e Cordova (iOS, Android). Le notifiche vengono inviate da {{ site.data.keyword.mfserver_short_notm }} all'infrastruttura del fornitore (Apple, Google, Microsoft, SMS Gateways) e da lì ai dispositivi interessati. Il meccanismo di notifica unificata rende trasparente, per lo sviluppatore, l'intero processo di comunicazione con gli utenti e i dispositivi.

## Supporto per il dispositivo
{: #device-support}
Le notifiche di push sono supportate per le seguenti piattaforme in {{ site.data.keyword.mobilefoundation_short }}:

* iOS 8.x o successivo
* Android 4.x o successivo
* Windows 8.1, Windows 10

## Formati delle notifiche di push
{: #push-notifications-forms}
Le notifiche possono avere diversi formati:

* **Avviso** (iOS, Android, Windows) - un messaggio di testo a comparsa
* **Sonora** (iOS, Android, Windows) - viene riprodotto un file sonoro quando viene ricevuta una notifica
* **Badge** (iOS), Tile (Windows) - una rappresentazione grafica che consente un testo breve o un'immagine
* **Banner** (iOS), Toast (Windows) - un messaggio di testo a scomparsa nello schermo del dispositivo
* **Interattiva** (iOS 8 e superiore) - pulsanti di azione all'interno del banner di una notifica ricevuta
* **Silenziosa** (iOS 8 e superiore) - invio di notifiche senza disturbare l'utente

## Tipi di notifiche di push
{: #push-notification-types }

### Notifiche basate su tag
{: #tag-notifications-overview}

Le notifiche basate su tag sono messaggi di notifica destinati a tutti i dispositivi che hanno una sottoscrizione a una specifica tag.  

Le notifiche basate su tag consentono la segmentazione di notifiche sulla base di argomenti o di aree di argomento. I destinatari della notifica possono scegliere di ricevere le notifiche solo se sono relative a un oggetto o a un argomento di interesse. Pertanto, la notifica basata su tag fornisce un mezzo per segmentare i destinatari. Con questa funzione puoi definire le tag e inviare o ricevere messaggi in base alle tag. Un messaggio viene indirizzato solo ai dispositivi che hanno una sottoscrizione a una tag.

### Notifiche broadcast
{: #broadcast-notifications-overview}

Le notifiche broadcast sono una forma di notifiche di push basate su tag indirizzate a tutti i dispositivi sottoscritti e sono abilitate per impostazione predefinita per qualsiasi applicazione {{ site.data.keyword.mobilefirst_notm }} abilitata per il push tramite una sottoscrizione a una tag `Push.all` riservata (creata automaticamente per ogni dispositivo). Le notifiche broadcast possono essere disabilitate annullando la sottoscrizione alla tag `Push.all` riservata.

### Notifiche Unicast
{: #unicast-notifications-overview}

Le notifiche Unicast o le notifiche autenticate dall'utente vengono protette con OAuth. Questi messaggi di notifica sono indirizzati ad un particolare dispositivo o a uno userID. Il valore di userID nella sottoscrizione utente può derivare dal contesto di sicurezza sottostante.

### Notifiche interattive
{: #interactive-notifications-overview}

Con la notifica interattiva, quando arriva una notifica, gli utenti possono agire senza aprire l'applicazione. Quando arriva una notifica interattiva, il dispositivo mostra i pulsanti di azione insieme al messaggio di notifica. Al momento, le notifiche interattive sono supportate sui dispositivi con iOS versione 8 e successive. Se una notifica interattiva viene inviata su un dispositivo iOS con una versione precedente alla 8, le azioni di notifica non verranno visualizzate.

Apprendi come gestire le [notifiche interattive](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications).

### Notifiche silenziose
{: #silent-notifications-overview}

Le notifiche silenziose sono notifiche che non visualizzano avvisi altrimenti disturberebbero l'utente. Quando arriva una notifica silenziosa, il codice che gestisce l'applicazione viene eseguito in background senza portare l'applicazione in primo piano. Al momento, le notifiche silenziose sono supportate sui dispositivi iOS con versione 7 e successive. Se la notifica silenziosa viene inviata ai dispositivi iOS con una versione precedente alla 7, la notifica viene ignorata se l'applicazione è in esecuzione in background. Se l'applicazione è in esecuzione in primo piano, viene richiamato il metodo di callback della notifica.

Apprendi come gestire le [notifiche silenziose](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications).

Le notifiche Unicast non contengono tag nel payload. Il messaggio di notifica può essere indirizzato a più dispositivi o utenti specificando rispettivamente più deviceID o userID, nel blocco di destinazione dell'API message POST.
{: note}

## Impostazioni del proxy
{: #proxy-settings}

Utilizza le impostazioni del proxy per impostare il proxy facoltativo le cui notifiche vengono inviate a APNS e FCM. Puoi impostare il proxy utilizzando le proprietà di configurazione **push.apns.proxy.* ** e **push.gcm.proxy.***. Per ulteriori informazioni, vedi [List of JNDI properties for {{ site.data.keyword.mfserver_short_notm }} push service](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

WNS non ha un supporto proxy.
{: note}

### Utilizzo di WebSphere DataPower come endpoint della notifica di push
{: #proxy-settings-datapower-endpoint}

Poi configurare DataPower per accettare le richieste di notifica da {{ site.data.keyword.mfserver_short_notm }} e reindirizzarle a FCM e WNS.

Gli APN non sono supportati.
{: note}

#### Configurazione di {{ site.data.keyword.mfserver_short_notm }}
{: #proxy-settings-config-mfp-server}

In `server.xml`, configura la seguente proprietà JNDI.
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

dove `host` è il nome host di DataPower e `port` è il numero di porta su cui è configurato HTTPS Front Side Handler per FCM e WNS.

#### Configurazione di DataPower
{: #proxy-settings-config-datapower}

1. Accedi all'applicazione DataPower.
2. Vai a **Services > Multi-Protocol Gateway > New Multi-Protocol Gateway**.
3. Specifica un nome con cui puoi identificare la configurazione.
4. Seleziona XML Manager, Multi-Protocol Gateway Policy come valori predefiniti e URL Rewrite Policy su nessuno.
5. Seleziona il pulsante di opzione **static-backend** e seleziona una qualsiasi delle seguenti opzioni per **set Default Backend URL**:
    - Per FCM,	`https://gcm-http.googleapis.com`
    - Per WNS,	`https://hk2.notify.windows.com`
6. Seleziona il tipo di risposta e il tipo di richiesta da passare.

### Generazione di un certificato
{: #proxy-settings-generate-cert}

Per generare il certificato, scegli una delle seguenti opzioni:

* Per FCM:
    1. Dalla riga di comando, immetti `Openssl` per ottenere i certificati FCM.
    2. Esegui il seguente comando:
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
        {: codeblock}
    3. Copia il contenuto da *-----BEGIN CERTIFICATE-----  a -----END CERTIFICATE-----* e salvalo in un file con l'estensione `.pem`.

* Per WNS:
    1. Dalla riga di comando, utilizza `Openssl` per ottenere i certificati WNS.
    2. Esegui il seguente comando:
        ```
        openssl s_client -connect https://hk2.notify.windows.com:443
        ```
        {: codeblock}
    3. Copia il contenuto da *-----BEGIN CERTIFICATE-----  a -----END CERTIFICATE-----* e salvalo in un file con l'estensione `.pem`.

### Impostazioni backside
{: #proxy-settings-backside-settings}

* Per FCM e WNS:
    1. Crea un certificato di crittografia.
        a. Vai a **Objects > Crypto Configuration e fai clic su Crypto certificate**.
        b. Specifica un nome con cui puoi identificare il certificato di crittografia.
        c. Fai clic su **Upload** per caricare il certificato FCM generato.
        d. Imposta **Password Alias** su nessuno.
        e. Fai clic su **Generate key**.
        ![Configura certificato di crittografia](images/bck_1.gif "Configura certificato di crittografia")

    2. Crea una credenziale di convalida di crittografia.
        a. Vai a Objects > Crypto Configuration** e fai clic su **Crypto Validation Credential**.
        b. Specifica un nome univoco.
        c. Per Certificates, seleziona il certificato di crittografia che hai creato nel passo precedente - passo 1.
        d. Per **Certificate Validation Mode**, seleziona Match exact certificate or immediate issuer.
        e. Fai clic su **Apply**.

        ![Configura credenziali di convalida di crittografia](images/bck_2.gif "Configura credenziali di convalida di crittografia")

    3. Crea una credenziale di convalida di crittografia:
        a. Vai a **Objects > Crypto Configuration** e fai clic su **Crypto Profile**.
        b. Fai clic su **Add**.
        c. Specifica un nome univoco.
        d. Per **Validation Credentials**, seleziona la credenziale di convalida creata nel passo precedente - passo 2, dal menu a discesa, imposta Identification Credentials su **nessuno**.
        e. Fai clic su **Apply**.

        ![Configura profilo di crittografia](images/bck_3.gif "Configura profilo di crittografia")

    4. Crea un profilo proxy SSL:

        a. Vai a **Objects** > **Crypto Configuration** > **SSL Proxy Profile**.
        b. Scegli una delle seguenti opzioni:

            - Per SMS, seleziona **SSL Proxy Profile** come nessuno.
            - Per FCM e WNS con un URL di backend protetto (HTTPS), completa questi passi:
                i. Fai clic su **Add**.
                ii. Specifica un nome con cui puoi identificare il profilo proxy ssl in un secondo momento.
                iii. Seleziona **SSL Direction** come **Forward** dal menu a discesa.
                iv. Per Forward (Client) Crypto Profile, seleziona il profilo di crittografia creato nel passo 3.
                v. Fai clic su **Apply**.

        ![Configura il profilo proxy SSL](images/bck_4.gif "Configura il profilo proxy SSL (obsoleto)")

    5. Nella finestra Multi-Protocol Gateway, in **Back side settings**, seleziona  **Proxy Profile** come **tipo di client SSL** e seleziona il profilo proxy SSL creato nel passo 4.

        ![Configura il profilo proxy SSL](images/bck_5.gif "Finestra Multi-Protocol Gateway")

### Impostazioni Frontside
{: #proxy-settings-frontside-settings}

* Per FCM e WNS:

    1. Crea una coppia chiave-certificato con il valore CN (Common Name) come il nome host di DataPower:
        a. Vai a **Administration** > **Miscellaneous** e fai clic su **Crypto Tools**.
        b. Immetti il nome host di DataPower come il valore per CN (Common Name).
        c. Seleziona **Export private key** se intendi esportare la chiave privata in un secondo momento e fai clic su **Generate Key**.

        ![Creazione di una coppia chiave-certificato](images/frnt_1.gif "Creazione di una coppia chiave-certificato")

    2. Crea una credenziale di identificazione di crittografia:
        a. Vai a **Objects** > **Crypto Configuration** e fai clic su **Crypto Identification Credentials**.
        b. Fai clic su **Add**.
        c. Specifica un nome univoco.
        d. Per Crypto Key e Certificate, seleziona la chiave e il certificato che hai generato nel passo precedente - passo 1 dalla casella di elenco.
        e. Fai clic su **Apply**.

        ![Creazione di una credenziale di identificazione di crittografia](images/frnt_2.gif "Creazione di una credenziale di identificazione di crittografia")

    3. Crea un profilo di crittografia:

        a. Vai a **Objects** > **Crypto Configuration** e fai clic su **Crypto Profile**.
        b. Fai clic su **Add**.
        c. Specifica un nome univoco.
        d. Per Identification Credentials, seleziona la credenziale di identificazione che hai creato dal passo precedente - passo 2 dalla casella di elenco. Imposta le credenziali di convalida su nessuno.
        e. Fai clic su **Apply**.

        ![Configura profilo di crittografia](images/frnt_3.gif "Configura profilo di crittografia")

    4. Crea un profilo proxy SSL:

        a. Vai a **Objects** > **Crypto Configuration** > **SSL Proxy Profile**.
        b. Fai clic su **Add**.
        c. Specifica un nome univoco.
        d. Seleziona SSL Direction come **Reverse** dalla casella di elenco.
        e. Per Reverse (Server) Crypto Profile, seleziona il profilo di crittografia che hai creato nel passo precedente - passo 3.  
        f. Fai clic su **Apply**.

        ![Configura il profilo proxy SSL](images/frnt_4.gif "Configura il profilo proxy SSL")

    5. Crea un HTTPS Front Side Handler:

        a. Vai a **Objects** > **Protocol Handlers** > **HTTPS Front Side Handler**.
        b. Fai clic su **Add**.
        c. Specifica un nome univoco.
        d. Per **Local IP address**, seleziona l'alias corretto o lascialo impostato sul valore predefinito (0.0.0.0).
        e. Specifica una porta disponibile.
        f. Per **Allowed methods and versions**, seleziona HTTP 1.0, HTTP 1.1, il metodo POST, il metodo GET, l'URL con ?, l'URL con #, l'URL con ..
        g. Seleziona **Proxy Profile** come tipo di server SSL.
        h. Per il profilo proxy SSL (obsoleto), seleziona il profilo proxy ssl che hai creato nel passo precedente - passo 4.
        i. Fai clic su **Apply**.

        ![Configura HTTPS Front Side Handler](images/frnt_5.gif "Configura HTTPS Front Side Handler")

    6. Nella pagina Configure Multi-Protocol Gateway, in **Front side settings**, seleziona l'https front side handler come **Front Side Protocol**, creato nel passo 5, e fai clic su **Apply**.

        ![Configurazione generale](images/frnt_6.gif "Configurazione generale")

    Il certificato che viene utilizzato da DataPower nelle impostazioni Frontside, è di tipo autofirmato. Le connessioni a DataPower avranno esito negativo a meno che il certificato non venga aggiunto al keystore JRE utilizzato da {{ site.data.keyword.mobilefirst_notm }}.

## Passi successivi
{: #next-steps }

Completa la configurazione richiesta del lato server e del lato client per poter inviare e ricevere le notifiche di push:

* [Configura notifiche di push](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [Invia notifiche di push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [>Gestione delle notifiche di push nelle applicazioni client](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [Notifiche silenziose](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notifiche interattive](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
