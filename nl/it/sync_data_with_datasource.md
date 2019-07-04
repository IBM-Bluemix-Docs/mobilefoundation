---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: synchronization of data, sync with offline storage, jsonstore sync

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Sincronizzazione dei dati con un'origine dati
{: #sync_data_with_datasource}

Puoi automaticamente sincronizzare i dati tra una raccolta JSONStore su un dispositivo con qualsiasi database CouchDB remoto, compreso [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management).

## Configura la sincronizzazione tra JSONStore e Cloudant
{: #set_up_sync}

Per configurare la sincronizzazione automatica tra JSONStore e Cloudant, completa la seguente procedura:

1. Definisci la politica di sincronizzazione (**Sync Policy**) sulla tua applicazione mobile.
2. Distribuisci l'adattatore di sincronizzazione (**Sync Adapter**) su Mobile Foundation.

### Definizione della politica di sincronizzazione
{: #define_sync_policy}

Il metodo di sincronizzazione tra una raccolta JSONStore e un database Cloudant è definito dalla politica di sincronizzazione (**Sync Policy**). Specifica la politica di sincronizzazione (**Sync Policy**) nella tua applicazione per ciascuna raccolta.
Una raccolta JSONStore deve essere inizializzata con un campo relativo alla politica di sincronizzazione (**Sync Policy**). **Sync Policy** può essere una delle tre politiche di seguito indicate:

1. `SYNC_DOWNSTREAM`
  Utilizza la politica `SYNC_DOWNSTREAM` quando vuoi scaricare dati da Cloudant alla raccolta JSONStore. Questa politica viene di norma utilizzata per i dati statici richiesti per l'archiviazione offline. Un esempio è il listino prezzi degli articoli in un catalogo. Ogni volta che la raccolta viene inizializzata sul dispositivo, i dati vengono aggiornati dal database Cloudant remoto. Mentre l'intero database viene scaricato per la prima volta, i successivi aggiornamenti scaricano solo dei delta, che consistono nelle modifiche effettuate sul database remoto.
  
Controlla il seguente utilizzo:

   * Android:
  
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
   ```

   * iOS: 
  
   ```objc
   openOptions.syncPolicy = SYNC_DOWNSTREAM;
   ```

   * Cordova: 
  
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
  }
   ```

2. `SYNC_UPSTREAM`
  Utilizza questa politica quando vuoi eseguire il push dei dati locali a un database Cloudant. Un esempio è il caricamento su un database Cloudant dei dati relativi alle vendite acquisiti offline. Quando una raccolta viene definita con la politica `SYNC_UPSTREAM`, ogni nuovo record aggiunto alla raccolta crea un nuovo record in Cloudant. Analogamente, ogni documento modificato nella raccolta sul dispositivo modificherà il documento su Cloudant e i documenti eliminati nella raccolta verranno eliminati anche dal database Cloudant..

Controlla il seguente utilizzo:

   * Android:
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
   ```

   * iOS:
   ```objc
   openOptions.syncPolicy = SYNC_UPSTREAM;
   ```

   * Cordova:
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
  }
   ```

3. `SYNC_NONE`
  `SYNC_NONE` è la politica predefinita. Scegli questa politica perché la sincronizzazione non abbia luogo.

La politica di sincronizzazione (**Sync Policy**) è attribuita a una raccolta JSONStore. Se una raccolta viene inizializzata con una specifica politica di sincronizzazione (**Sync Policy**), quest'ultima non deve essere modificata. Modificare la politica di sincronizzazione (**Sync Policy**) può produrre risultati indesiderabili.

### Distribuzione dell'adattatore di sincronizzazione
{: #deploy_sync_adapter}

`syncAdapterPath`
Questa configurazione prende il nome dell'adattatore che viene distribuito.

Controlla il seguente utilizzo:

   * Android:
   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```

   * iOS:
   ```objc
    openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova o Ionic:
   ```javascript
    collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
   ```

Scarica l'adattatore `JSONStoreSync` da [qui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/), configura le credenziali Cloudant nel percorso `src/main/adapter-resources/adapter.xml` e distribuiscilo nel tuo server Mobile Foundation.

Configura le credenziali al database Cloudant di backend nella console Mobile Foundation Operations.

### Esecuzione manuale dell'operazione di sincronizzazione
{: #performing_sync_manual}

Se deve essere eseguita una sincronizzazione upstream o downstream in qualsiasi momento dopo l'inizializzazione in modo esplicito, è possibile utilizzare la seguente API:

`sync()`

Questa API esegue una sincronizzazione downstream se la raccolta richiamante ha una politica di sincronizzazione impostata su `SYNC_DOWNSTREAM`. Se la politica di sincronizzazione è impostata su `SYNC_UPSTREAM`, viene eseguita una sincronizzazione upstream da JSONStore al database Cloudant. La sincronizzazione viene eseguita per i documenti che erano stati aggiunti, eliminati o sostituiti.

Controlla il seguente utilizzo: 

  * Android:
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS:
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

  * Cordova:
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
