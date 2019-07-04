---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Direct Update, CDN support, secure direct update

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configurazione avanzata dell'aggiornamento diretto
{: #advanced_direct_update_configuration}

Qui sono descritti alcuni dei metodi avanzati in cui puoi configurare e utilizzare la funzione di aggiornamento diretto.

## Personalizzazione della IU dell'aggiornamento diretto
{: #customize_du_ui}

La IU dell'aggiornamento diretto presentata all'utente finale, può essere personalizzata.
Aggiungi quanto segue all'interno della funzione `wlCommonInit()` in **index.js**:
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implementa la logica dell'aggiornamento diretto personalizzata
};
```
{: codeblock}

*directUpdateData* è un oggetto JSON che contiene la proprietà downloadSize che rappresenta la dimensione file (in byte) del pacchetto di aggiornamento da scaricare dal server Mobile Foundation.
*directUpdateContext* è un oggetto JavaScript che espone le funzioni .start() e .stop(), che avviano e arrestano il flusso dell'aggiornamento diretto.

Se le risorse web sono più recenti nel server Mobile Foundation che nell'applicazione, vengono aggiunti dati di verifica dell'aggiornamento diretto alla risposta del server. Quando il framework lato client di Mobile Foundation rileva questa verifica dell'aggiornamento diretto, richiama la funzione `wl_directUpdateChallengeHandler.handleDirectUpdate`.

La funzione fornisce una progettazione dell'aggiornamento diretto predefinita: viene visualizzata una finestra di dialogo del messaggio predefinita quando un aggiornamento diretto è disponibile e viene visualizzata una schermata di avanzamento predefinita quando viene avviato il processo di aggiornamento diretto. Puoi implementare il comportamento dell'interfaccia utente dell'aggiornamento diretto personalizzato o personalizzare la casella di dialogo dell'aggiornamento diretto sovrascrivendo questa funzione e implementando la tua logica.

Nel seguente codice di esempio, una funzione `handleDirectUpdate` implementa un messaggio personalizzato nella finestra di dialogo dell'aggiornamento diretto. Aggiungi questo codice al file `www/js/index.js` del progetto Cordova.
Ulteriori esempi di una IU dell'aggiornamento diretto personalizzata:
* Viene creata una finestra di dialogo utilizzando un framework JavaScript di terze parti (come ad esempio Dojo o jQuery Mobile, Ionic, …)
* IU completamente nativa eseguendo un plugin Cordova
* Un HTML alternativo presentato all'utente con opzioni
e così via.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Crea una finestra di dialogo.
        'Custom dialog body text',
        // Gestisci i pulsanti della finestra di dialogo.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

Puoi avviare il processo di aggiornamento diretto eseguendo il metodo `directUpdateContext.start()` quando l'utente fa clic sul pulsante della finestra di dialogo. Viene visualizzata la schermata di avanzamento predefinita, che è simile a quella nelle versioni precedenti del server Mobile Foundation.

Questo metodo supporta i seguenti tipi di richiamo:
* Quando non viene specificato alcun parametro, il server Mobile Foundation utilizza la schermata di avanzamento predefinita.
* Quando viene fornita una funzione listener come `directUpdateContext.start(directUpdateCustomListener)`, il processo di aggiornamento diretto viene eseguito in background mentre invia gli eventi del ciclo di vita al listener. Il listener personalizzato deve implementare i seguenti metodi:

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

I metodi del listener vengono avviati durante il processo di aggiornamento diretto in base alle seguenti regole:
* `onStart` viene richiamato con il parametro `totalSize` che include la dimensione del file aggiornato.
* `onProgress` viene richiamato più volte con lo stato `DOWNLOAD_IN_PROGRESS`, `totalSize` e `completedSize` (il volume che è stato scaricato finora).
* `onProgress` viene richiamato con lo stato `UNZIP_IN_PROGRESS`.
* `onFinish` viene richiamato con uno dei seguenti codici di stato finali:

| Codice di stato | Descrizione |
|:------------|:------------|
| `SUCCESS` | L'aggiornamento diretto è terminato senza errori. |
| `CANCELED` | L'aggiornamento diretto è stato annullato (ad esempio perché è stato richiamato il metodo `stop()`). |
| `FAILURE_NETWORK_PROBLEM` | Si è verificato un problema con una connessione di rete durante l'aggiornamento. |
| `FAILURE_DOWNLOADING` | Il file non è stato scaricato completamente. |
| `FAILURE_NOT_ENOUGH_SPACE` | Non c'era spazio sufficiente nel dispositivo per scaricare e spacchettare il file di aggiornamento. |
| `FAILURE_UNZIPPING` | Si è verificato un problema durante lo spacchettamento del file di aggiornamento. |
| `FAILURE_ALREADY_IN_PROGRESS` | Il metodo start è stato richiamato mentre l'aggiornamento diretto era ancora in esecuzione. |
| `FAILURE_INTEGRITY` | L'autenticità del file di aggiornamento non può essere verificata. |
| `FAILURE_UNKNOWN` | Errore interno non previsto. |
{: caption="Tabella 1. Codici di stato" caption-side="top"}

Se implementi un listener di aggiornamento diretto personalizzato, devi assicurarti che l'applicazione venga ricaricata quando il processo di aggiornamento diretto viene completato e il metodo `onFinish()` è stato richiamato. Devi anche richiamare `wl_directUpdateChalengeHandler.submitFailure()` se l'aggiornamento diretto non viene completato correttamente.

Il seguente esempio mostra un'implementazione di un listener di aggiornamento diretto personalizzato:
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //mostra la finestra di dialogo di avanzamento personalizzata
  },
  onProgress: function(status,totalSize,completedSize){
    //aggiorna la finestra di dialogo di avanzamento personalizzata
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //mostra il messaggio di esito positivo
      WL.Client.reloadApp();
    }
    else {
      //mostra il messaggio di errore personalizzato

      //submitFailure deve essere richiamato in caso di errore
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

## Esecuzione degli aggiornamenti diretti senza IU
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}} supporta l'aggiornamento diretto senza IU quando l'applicazione è in primo piano.

Per eseguire gli aggiornamenti diretti senza IU, implementa `directUpdateCustomListener`. Fornisci le implementazioni della funzione vuote ai metodi `onStart` e `onProgress`. Le implementazioni vuote comportano l'esecuzione del processo di aggiornamento diretto in background.

Per completare il processo di aggiornamento diretto, l'applicazione deve essere ricaricata. Sono disponibili le seguenti opzioni:
* Anche il metodo `onFinish` può essere vuoto. In questo caso, l'aggiornamento diretto verrà applicato dopo il riavvio dell'applicazione.
* Puoi implementare una finestra di dialogo personalizzata che informa o richiede all'utente di riavviare l'applicazione. (Consulta il seguente esempio.)
* Il metodo `onFinish` può applicare un ricaricamento dell'applicazione richiamando `WL.Client.reloadApp()`.

Questo è un esempio di implementazione di `directUpdateCustomListener`:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

Implementa la funzione `wl_directUpdateChallengeHandler.handleDirectUpdate`. Trasmetti l'implementazione `directUpdateCustomListener` che hai creato come un parametro della funzione. Assicurati che venga richiamato `directUpdateContext.start(directUpdateCustomListener`). Questa è un'implementazione `wl_directUpdateChallengeHandler.handleDirectUpdate` di esempio:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

Quando l'applicazione viene inviata in background, il processo di aggiornamento diretto viene sospeso.
{: note}

## Gestione di un errore dell'aggiornamento diretto
{: #scenario-handling-a-direct-update-failure }
Questa sezione illustra come gestire un errore di aggiornamento diretto che potrebbe verificarsi, ad esempio, a causa della perdita di connettività. In questo scenario, all'utente è impedito l'utilizzo dell'applicazione nella modalità offline. Viene visualizzata una finestra di dialogo che offre all'utente l'opzione per riprovare.

1.  Crea una variabile globale per archiviare il contesto dell'aggiornamento diretto in modo che puoi utilizzarlo successivamente quando si verifica un errore con l'aggiornamento diretto. Ad esempio:
    ```JavaScript
    var savedDirectUpdateContext;
    ```
    {: codeblock}

2.  Implementa un gestore delle verifiche dell'aggiornamento diretto. Salva il contesto dell'aggiornamento diretto qui. Ad esempio:
    ```JavaScript
    wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

      savedDirectUpdateContext = directUpdateContext; // salva il contesto dell'aggiornamento diretto

      var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

      WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
        text : WL.ClientMessages.update,
    handler : function() {
          directUpdateContext.start(directUpdateCustomListener);
    }
      }]);
    };
    ```
    {: codeblock}

3.  Crea una funzione che avvia il processo di aggiornamento diretto utilizzando il contesto dell'aggiornamento diretto. Ad esempio:
    ```JavaScript
    restartDirectUpdate = function () {
      savedDirectUpdateContext.start(directUpdateCustomListener); // utilizza il contesto dell'aggiornamento diretto salvato per riavviarlo
};
    ```
    {: codeblock}

4.  Implementa `directUpdateCustomListener`. Aggiungi il controllo dello stato al metodo `onFinish`. Se lo stato inizia con un `FAILURE`, apri una finestra di dialogo solo modale con l'opzione **Riprova**. Ad esempio:
    ```JavaScript
    var directUpdateCustomListener = {
      onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
          WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
            text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
      }
    };
    ```
    {: codeblock}

    Quando l'utente fa clic sul pulsante **Riprova**, l'applicazione riavvia il processo di aggiornamento diretto.
    {: note}

## Aggiornamento diretto completo e delta
{: #delta-and-full-direct-update }
Gli aggiornamenti diretti delta abilitano un'applicazione a scaricare solo i file che sono stati modificati dall'ultimo aggiornamento invece di tutte le risorse web dell'applicazione. Questo riduce il tempo di scaricamento, conserva la larghezza di banda e migliora l'esperienza utente generale.

Un **aggiornamento delta** è possibile solo se le risorse web dell'applicazione client sono una versione dietro l'applicazione che è al momento distribuita sul server. Le applicazioni client che sono più di una versione dietro all'applicazione al momento distribuita (il che significa che l'applicazione è stata distribuita al server almeno due volte dal suo aggiornamento), ricevono un **aggiornamento completo** (il che significa che tutte le risorse web vengono scaricate e aggiornate).
{: note}

Vedi l'esempio di aggiornamento diretto per l'applicazione Cordova nella sezione **Esempi**. Questa applicazione dimostra come creare una finestra di dialogo di aggiornamento diretto personalizzata anziché la finestra di dialogo fornita per impostazione predefinita.  

## Supporto CDN
{: #cdn_support}

Puoi configurare le richieste di aggiornamento diretto per essere fornite da un CDN (Content Delivery Network) anziché dal server Mobile Foundation.

L'utilizzo di un CDN anziché del server Mobile Foundation per fornire le richieste di aggiornamento diretto presenta i seguenti vantaggi:

* Rimuove i sovraccarichi di rete dal server Mobile Foundation.
* Aumenta le velocità di trasferimento superiori al limite di 250 MB/secondo quando si forniscono le richieste da un server Mobile Foundation.
* Assicura un'esperienza di aggiornamento diretto più uniforme per tutti gli utenti indipendentemente dalla loro ubicazione geografica.

### Requisiti generali
{: #general-requirements }
Per fornire le richieste di aggiornamento diretto da un CDN, assicurati che la tua configurazione sia conforme alle seguenti condizioni:

* Il CDN deve essere un proxy inverso davanti al server Mobile Foundation (o di fronte a un altro proxy inverso se necessario).
* Quando crei l'applicazione dal tuo ambiente di sviluppo, imposta il server di destinazione sull'host e sulla porta CDN anziché sull'host e sulla porta del server Mobile Foundation. Ad esempio, quando esegui il comando della CLI Mobile Foundation `mfpdev server add`, fornisci l'host e la porta CDN.
* Nel pannello di gestione CDN, devi contrassegnare i seguenti URL di aggiornamento diretto per la memorizzazione nella cache per garantire che il CDN trasmetta tutte le richieste al server Mobile Foundation ad eccezione delle richieste di aggiornamento diretto. Per le richieste di aggiornamento diretto, il CDN determina se ha ottenuto il contenuto. Se è così, lo restituisce senza andare al server Mobile Foundation; in caso contrario, va al server Mobile Foundation, ottiene l'archivio di aggiornamento diretto (file .zip) e lo memorizza per le successive richieste di quell'URL specifico. Per le applicazioni che sono create con la v8.0 di {{site.data.keyword.mobilefoundation_short}}, l'URL di aggiornamento diretto è: `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`.
Il prefisso `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` è costante per tutte le richieste di runtime. Ad esempio: `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

Nell'esempio, sono presenti ulteriori parametri di richiesta che fanno comunque parte della richiesta.

* Il CDN deve consentire la memorizzazione nella cache dei parametri di richiesta. Due differenti archivi dell'aggiornamento diretto possono essere diversi solo per i parametri di richiesta.
* Il CDN deve supportare TTL nella risposta di aggiornamento diretto. Il supporto è necessario per supportare più aggiornamenti diretti della stessa versione.
* Il CDN non deve modificare o rimuovere le intestazioni HTTP che sono utilizzate nel protocollo server-client.

### Esempio di configurazione CDN
{: #example-cdn-configuration }
Questo esempio si basa sull'utilizzo della configurazione CDN Akamai che memorizza nella cache l'archivio dell'aggiornamento diretto. Le seguenti attività sono completate dall'amministratore della rete, dall'amministratore di Mobile Foundation e dall'amministratore di Akamai:

#### Amministratore della rete
{: #network-administrator }
Crea un altro dominio nel DNS per il tuo server Mobile Foundation. Ad esempio, se il tuo dominio del server è `yourcompany.com` devi creare un altro dominio come ad esempio `cdn.yourcompany.com`.
Nel DNS del nuovo dominio `cdn.yourcompany.com`, imposta un `CNAME` sul nome del dominio fornito da Akamai. Ad esempio, `yourcompany.com.akamai.net`.

#### Amministratore di Mobile Foundation
{: #mobilefoundation-administrator }
Imposta il nuovo dominio `cdn.yourcompany.com` come URL del server Mobile Foundation per le applicazioni Mobile Foundation. Ad esempio, per un'attività di creazione Ant, la proprietà è:
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Amministratore Akamai
{: #akamai-administrator }
1. Apre il gestore delle proprietà Akamai e imposta la proprietà **nome host** sul valore del nuovo dominio.

    ![Imposta la proprietà nome host sul valore del nuovo dominio](images/direct_update_cdn_3.jpg "Imposta la proprietà nome host sul valore del nuovo dominio")

2. Nella scheda Default Rule, configura l'host e la porta originali del server Mobile Foundation e imposta il valore **Custom Forward Host Header** sul dominio appena creato.

    ![Imposta il valore Custom Forward Host Header sul dominio appena creato](images/direct_update_cdn_4.jpg "Imposta il valore Custom Forward Host Header sul dominio appena creato")

3. Dall'elenco **Caching Option**, seleziona **No Store**.

    ![Dall'elenco Caching Option, seleziona No Store](images/direct_update_cdn_5.jpg "Dall'elenco Caching Option, seleziona No Store")

4. Dalla scheda **Static Content configuration**, configura i criteri corrispondenti in base all'URL di aggiornamento diretto dell'applicazione. Ad esempio, crea una condizione che indica `If Path matches one of direct_update_URL`.

    ![Configura i criteri corrispondenti in base all'URL di aggiornamento diretto dell'applicazione](images/direct_update_cdn_6.jpg "Configura i criteri corrispondenti in base all'URL di aggiornamento diretto dell'applicazione")

5. Configura il comportamento della chiave della cache per utilizzare tutti i parametri di richiesta nella chiave della cache (devi far ciò per memorizzare nella cache diversi archivi di aggiornamento diretto per versioni o applicazioni differenti). Ad esempio, dall'elenco **Behavior**, seleziona `Include all parameters (preserve order from request)`.

    ![Configura il comportamento della chiave della cache per utilizzare tutti i parametri di richiesta nella chiave della cache](images/direct_update_cdn_8.jpg "Configura il comportamento della chiave della cache per utilizzare tutti i parametri di richiesta nella chiave della cache")

6. Imposta dei valori simili ai seguenti per configurare il comportamento della cache per memorizzare l'URL di aggiornamento diretto e per impostare TTL.

      ![Imposta i valori per configurare il comportamento della memorizzazione nella cache](images/direct_update_cdn_7.jpg "Imposta i valori per configurare il comportamento della memorizzazione nella cache")

| Campo | Valore |
|:------|:------|
| Opzione di memorizzazione nella cache | Cache |
| Applica la rivalutazione degli oggetti obsoleti | Fornire obsoleto se è impossibile la convalida |
| Durata massima | 3 minuti |
{: caption="Tabella 2. Campi e valori per la configurazione del comportamento della memorizzazione nella cache" caption-side="top"}

## Aggiornamento diretto sicuro
{: #secure-dc }

Disabilitato per impostazione predefinita, l'aggiornamento diretto sicuro evita ad aggressori di terze parti di modificare le risorse web che vengono trasmesse dal server Mobile Foundation (o da un CDN (Content Delivery Network)) all'applicazione client.

### Abilitazione dell'autenticità dell'aggiornamento diretto
{: #enable-direct-update-authenticity}
Utilizzando uno strumento preferito, estrai la chiave pubblica dal keystore del server Mobile Foundation e convertila in base64.  
Il valore prodotto dovrebbe essere poi utilizzato come indicato nella seguente procedura:

1. Apri una finestra **Riga di comando** e passa alla root del progetto Cordova.
2. Esegui il comando: `mfpdev app config` e seleziona l'opzione **Chiave pubblica dell'autenticità dell'aggiornamento diretto**.
3. Fornisci la chiave pubblica e conferma.

Ogni successiva distribuzione dell'aggiornamento diretto alle applicazioni client sarà protetta dall'autenticità dell'aggiornamento diretto.

Perché l'aggiornamento diretto sicuro funzioni, un file keystore definito dall'utente deve essere distribuito nel server Mobile Foundation e una copia della chiave pubblica corrispondente deve essere inclusa nell'applicazione client distribuita.

Questo argomento descrive come associare una chiave pubblica alle applicazioni client nuove ed esistenti che sono state aggiornate. Per ulteriori informazioni sulla configurazione del keystore nel server Mobile Foundation, consulta [Configuring the Mobile Foundation server keystore ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

Il server fornisce un keystore integrato che può essere utilizzato per la verifica dell'aggiornamento diretto sicuro per le fasi di sviluppo.

Dopo aver associato la chiave pubblica all'applicazione client e averla ricreata, non devi caricarla nuovamente in {{ site.data.keys.mf_server }}. Tuttavia, se hai precedentemente pubblicato l'applicazione sul mercato senza la chiave pubblica, devi ripubblicarla.
{: note}

Per scopi di sviluppo, la seguente chiave pubblica fittizia predefinita viene fornita con il server Mobile Foundation:

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

Non utilizzare la chiave pubblica per scopi di produzione.
{: note}

### Generazione e distribuzione del keystore
{: #generating-and-deploying-the-keystore }
Ci sono molti strumenti disponibili per la generazione dei certificati e per l'estrazione delle chiavi pubbliche da un keystore. Il seguente esempio illustra le procedure con il programma di utilità JDK keytool e openSSL.

1. Estrai la chiave pubblica dal file keystore distribuito in {{ site.data.keys.mf_server }}.  
   La chiave pubblica deve essere codificata in Base64.
   {: note}

   Ad esempio, supponi che il nome alias è `mfp-server` e il file keystore è **keystore.jks**.  
   Per generare un certificato, immetti il seguente comando:

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   Viene generato un file certificato.  
   Immetti il seguente comando per estrarre la chiave pubblica:

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   Keytool da solo non può estrarre le chiavi pubbliche nel formato Base64.
   {: note}

2. Attieniti ad una delle seguenti procedure:
    * Copia il testo risultante, senza gli indicatori `BEGIN PUBLIC KEY` e `END PUBLIC KEY` nel file della proprietà mfpclient dell'applicazione, immediatamente dopo `wlSecureDirectUpdatePublicKey`.
    * Da un prompt dei comandi, immetti il seguente comando: `mfpdev app config direct_update_authenticity_public_key <public_key>`

    Per `<public_key>`, incolla il testo risultante dal passo 1, senza gli indicatori `BEGIN PUBLIC KEY` e `END PUBLIC KEY`.

3. Esegui il comando di build cordova per salvare la chiave pubblica nell'applicazione.
