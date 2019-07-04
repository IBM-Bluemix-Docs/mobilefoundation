---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Aggiornamento diretto
{: #direct_update}

Le risorse web (file JavaScript, HTML, CSS o di immagini) nelle applicazioni Cordova o Ionic possono essere aggiornate via OTA (over-the-air) con l'**aggiornamento diretto** senza che gli utenti debbano passare attraverso l'aggiornamento dell'App Store o del Play Store. Utilizzando la funzione di aggiornamento diretto, le aziende possono garantire che gli utenti finali utilizzino la versione più recente delle loro applicazioni. Per aggiornare un'applicazione, le risorse web aggiornate dell'applicazione devono essere impacchettate e caricate nella tua istanza di Mobile Foundation utilizzando la CLI Mobile Foundation o distribuendo un file di archivio generato. L'aggiornamento diretto viene poi attivato automaticamente. Viene quindi applicato a ogni richiesta dell'utente a una risorsa protetta.

![Diagramma di come funziona l'aggiornamento diretto](images/internal_function.jpg)

L'aggiornamento diretto è supportato nelle piattaforme iOS Cordova e Android Cordova.
{: note}

Per la fase di test di produzione o di pre-produzione operativa, si consiglia di implementare l'aggiornamento diretto sicuro prima di pubblicare l'applicazione nell'app store. L'aggiornamento diretto sicuro richiede una coppia di chiavi RSA che viene estratta da un certificato server firmato CA reale.

1. Stai attento a non modificare la configurazione del keystore dopo che l'applicazione è stata pubblicata. Gli aggiornamenti scaricati non possono essere autenticati prima di riconfigurare l'applicazione con una nuova chiave pubblica e di ripubblicarla. Se non esegui i due passi precedenti, l'aggiornamento diretto potrebbe avere esito negativo nel client.
2. L'aggiornamento diretto aggiorna solo le risorse web dell'applicazione. Per aggiornare le risorse native, una nuova versione dell'applicazione deve essere inviata al rispettivo app store.
3. Quando utilizzi la funzione di aggiornamento diretto e viene abilitata la funzione di checksum delle risorse web, viene stabilita una nuova base di checksum con ogni aggiornamento diretto.
4. Se il server Mobile Foundation è stato aggiornato, continua a fornire correttamente gli aggiornamenti diretti. Tuttavia, se viene caricato un archivio dell'aggiornamento diretto creato di recente (file .zip), può arrestare gli aggiornamenti ai precedenti client. Il motivo è che l'archivio contiene la versione del plugin `cordova-plugin-mfp`. Prima di fornire tale archivio a un client mobile, il server confronta la versione del client con quella del plugin. Se entrambe le versioni sono abbastanza vicine (cioè le tre cifre più significative sono identiche), l'aggiornamento diretto viene effettuato normalmente. In caso contrario, il server Mobile Foundation salta automaticamente l'aggiornamento. Una soluzione per la mancata corrispondenza della versione è di scaricare il `cordova-plugin-mfp` con la stessa versione di quella nel tuo progetto Cordova e rigenerare l'archivio dell'aggiornamento diretto.
{: tip}

Dopo un aggiornamento diretto, l'applicazione non utilizza più le risorse web pre-impacchettate. Invece, utilizza le risorse web scaricate dal sandbox dell'applicazione. Se la cache dell'applicazione nel dispositivo viene pulita, le risorse web impacchettate originali vengono nuovamente utilizzate.

L'aggiornamento diretto delle risorse web si applica solo a una versione specifica dell'applicazione. Ad esempio, gli aggiornamenti generati per la versione 2.0 di un'applicazione non verranno consegnati alla versione 2.1 della stessa applicazione.
{: note}

## Creazione e distribuzione delle risorse web aggiornate
{: #creating_deploying_updates}

Dopo aver apportato modifiche alle tue risorse web, puoi impacchettarle e caricarle nella tua istanza di Mobile Foundation utilizzando la CLI Mobile Foundation.

1.  L'aggiornamento diretto aggiorna soltanto le risorse web dell'applicazione. Per aggiornare le risorse native, una nuova versione dell'applicazione deve essere inviata al rispettivo app store.
2. Il servizio Mobile Foundation interromperà l'aggiornamento diretto ai client se la parte nativa dell'applicazione è significativamente diversa da quella caricata sul servizio Mobile Foundation. Ciò può verificarsi quando *cordova-plugin-mfp* viene aggiornato con una versione più recente del plugin. Prima di fornire l'archivio a un client mobile, il server confronta la versione del client con la versione del plugin. Se entrambe le versioni sono abbastanza vicine (il che significa che le tre cifre più significative nell'identificativo della versione sono identiche), l'aggiornamento diretto viene effettuato normalmente. In caso contrario, il server Mobile Foundation salta automaticamente l'aggiornamento. Una soluzione per la mancata corrispondenza della versione è di scaricare il *cordova-plugin-mfp* con la stessa versione di quella nel tuo progetto Cordova e rigenerare l'archivio dell'aggiornamento diretto.
{: tip}

1. Apri una finestra della riga di comando e passa alla root del progetto Cordova.
2. Esegui il comando:
  ```bash
  mfpdev app webupdate
  ```
  Il comando `mfpdev app webupdate` impacchetta le risorse web aggiornate in un file `.zip` e lo carica sul server Mobile Foundation predefinito in esecuzione nella workstation dello sviluppatore. Le risorse web impacchettate possono essere trovate nella cartella `[cordova-project-root-folder]/mobilefirst/`.

Per la procedura alternativa per aggiornare i contenuti web nella tua applicazione, vedi [qui](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app).

## Configurazione avanzata dell'aggiornamento diretto
{: #direct_update_advanced_config}

Per argomenti avanzati sulla configurazione dell'aggiornamento diretto, vedi [qui](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration).
