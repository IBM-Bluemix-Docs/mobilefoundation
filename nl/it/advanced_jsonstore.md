---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: JSONStore, advanced jsonstore, Cordova secure jsonstore, iOS secure jsonstore, android jsonstore, adapter integration

subcollection:  mobilefoundation
---
{:generic: .ph data-hd-programlang='generic'}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:generic: .ph data-hd-operatingsystem='generic'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# JSONStore avanzato
{: #advanced_jsonstore}

## Sicurezza in JSONStore
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

Puoi proteggere tutte le tue raccolte in un archivio passando una password alla funzione `init`. Se non viene passata alcuna password, i documenti di tutte le raccolte nell'archivio non saranno crittografati.
{: cordova}

La crittografia dei dati è disponibile solo negli ambienti Android, iOS, Windows 8.1 Universal e Windows 10 UWP.
Alcuni metadati di sicurezza sono archiviati nel keychain (iOS), nelle preferenze condivise (Android) o nella casella di sicurezza delle credenziali (Windows 8.1).
L'archivio è crittografato con una chiave AES (Advanced Encryption Standard) a 256 bit. Tutte le chiavi sono rafforzate con PBKDF2 (Password-Based Key Derivation Function 2).
{: cordova}

Utilizza `closeAll` per bloccare l'accesso a tutte le raccolte finché non richiami nuovamente `init`. Se pensi a `init` come a un accessor, puoi pensare a `closeAll` come alla sua funzione di disconnessione corrispondente. Utilizza `changePassword` per modificare la password.
{: cordova}

La crittografia è supportata solo in iOS. Per impostazione predefinita, l'SDK Mobile Foundation Cordova per iOS fa affidamento sulle API fornite da iOS per la crittografia. Se preferisci eseguirne la sostituzione con OpenSSL:
{: cordova}

* Aggiungi il plug-in `cordova-plugin-mfp-encrypt-utils`:
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* Nella logica dell'applicazione, utilizza: `WL.SecurityUtils.enableNativeEncryption(false)` per abilitare l'opzione OpenSSL.
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

Puoi proteggere tutte le raccolte in un archivio passando un oggetto `JSONStoreOpenOptions` con una password alla funzione `openCollections`. Se non viene passata alcuna password, i documenti di tutte le raccolte nell'archivio non sono crittografati.
{: ios}

Alcuni metadati di sicurezza sono archiviati nel keychain (iOS).
L'archivio è crittografato con una chiave AES (Advanced Encryption Standard) a 256 bit. Tutte le chiavi sono rafforzate con PBKDF2 (Password-Based Key Derivation Function 2).
{: ios}

Utilizza `closeAllCollections` per bloccare l'accesso a tutte le raccolte finché non richiami nuovamente `openCollections`. Se pensi a `openCollections` come a un accessor, puoi pensare a `closeAllCollections` come alla sua funzione di disconnessione corrispondente.
{: ios}

Utilizza `changeCurrentPassword` per modificare la password.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.password = "123"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

<!--### Android
{: #security_jsonstore_android} -->

Puoi proteggere tutte le raccolte in un archivio passando un oggetto `JSONStoreInitOptions` con una password alla funzione `openCollections`. Se non viene passata alcuna password, i documenti di tutte le raccolte nell'archivio non saranno crittografati.
{: android}

Alcuni metadati di sicurezza sono archiviati nelle preferenze condivise (Android).
L'archivio è crittografato con una chiave AES (Advanced Encryption Standard) a 256 bit. Tutte le chiavi sono rafforzate con PBKDF2 (Password-Based Key Derivation Function 2).
{: android}

Utilizza `closeAllCollections` per bloccare l'accesso a tutte le raccolte finché non richiami nuovamente `openCollections`. Se pensi a `openCollections` come a un accessor, puoi pensare a `closeAllCollections` come alla sua funzione di disconnessione corrispondente.
{: android}

Utilizza `changeCurrentPassword` per modificare la password.
{: android}

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  JSONStoreInitOptions options = new JSONStoreInitOptions();
  options.setPassword("123");
  WLJSONStore.getInstance(context).openCollections(collections, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

## Supporto di più utenti in JSONStore
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

Puoi creare più archivi che contengono raccolte differenti in una singola applicazione MobileFirst. La funzione `init` può prendere un oggetto options con un nome utente. Se non viene fornito alcun nome utente, il nome utente predefinito è *jsonstore*.
{: cordova}

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {username: 'yoel'};
WL.JSONStore.init(collections, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}
{: cordova}

<!--### iOS
{: #multiple_user_jsonstore_ios} -->

Puoi creare più archivi che contengono raccolte differenti in una singola applicazione MobileFirst. La funzione `init` può prendere un oggetto options con un nome utente. Se non viene fornito alcun nome utente, il nome utente predefinito è *jsonstore*.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.username = "yoel"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

<!--### Android
{: #multiple_user_jsonstore_android} -->

Puoi creare più archivi che contengono raccolte differenti in una singola applicazione Mobile Foundation. La funzione `openCollections` può prendere un oggetto options con un nome utente. Se non viene fornito alcun nome utente, il nome utente predefinito è *jsonstore*.
{: android}

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  JSONStoreInitOptions options = new JSONStoreInitOptions();
  options.setUsername("yoel");
  WLJSONStore.getInstance(context).openCollections(collections, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

## Integrazione dell'adattatore
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

L'integrazione dell'adattatore è facoltativa e fornire i modi per inviare i dati da una raccolta a un adattatore e ottenere i dati da un adattatore in una raccolta.
Puoi raggiungere questi obiettivi utilizzando `WLResourceRequest` o `jQuery.ajax` se hai bisogno di maggiore flessibilità.
{: cordova}

1. Crea un adattatore e denominalo **JSONStoreAdapter**.
2. Definisci le sue procedure `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.
   ```javascript
   function getPeople() {
    var data = { peopleList : [{name: 'chevy', age: 23}, {name: 'yoel', age: 23}] };
    WL.Logger.debug('Adapter: people, procedure: getPeople called.');
    WL.Logger.debug('Sending data: ' + JSON.stringify(data));
    return data;
    }
    function pushPeople(data) {
        WL.Logger.debug('Adapter: people, procedure: pushPeople called.');
        WL.Logger.debug('Got data from JSONStore to ADD: ' + data);
        return;
    }
    function addPerson(data) {
        WL.Logger.debug('Adapter: people, procedure: addPerson called.');
        WL.Logger.debug('Got data from JSONStore to ADD: ' + data);
        return;
    }
    function removePerson(data) {
        WL.Logger.debug('Adapter: people, procedure: removePerson called.');
        WL.Logger.debug('Got data from JSONStore to REMOVE: ' + data);
        return;
    }
    function replacePerson(data) {
        WL.Logger.debug('Adapter: people, procedure: replacePerson called.');
        WL.Logger.debug('Got data from JSONStore to REPLACE: ' + data);
        return;
    }
   ```
   {: cordova}
3. Per caricare i dati da un adattatore, utilizza `WLResourceRequest`.
   ```javascript
   try {
     var resource = new WLResourceRequest("adapters/JSONStoreAdapter/getPeople", WLResourceRequest.GET);
     resource.send()
     .then(function (responseFromAdapter) {
          var data = responseFromAdapter.responseJSON.peopleList;
     },function(err){
          //handle failure
     });
    } catch (e) {
        alert("Failed to load data from adapter " + e.Messages);
    }
   ```
   {: cordova}   
4. Il richiamo di `getPushRequired` restituisce un array di cosiddetti "documenti dirty", che sono documenti che hanno delle modifiche locali che non esistono sul sistema di back-end. Questi documenti vengono inviati all'adattatore quando viene richiamato `push`.
   ```javascript
   var collectionName = 'people';
   WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // handle success
   }).fail(function (error) {
      // handle failure
   });
   ```
   {: codeblock}
   {: cordova}
   Per evitare che JSONStore contrassegni i documenti come "dirty", passa l'opzione `{markDirty:false}` a `add`, `replace` e `remove`.
   {: tip}
   {: cordova}
5. Puoi anche utilizzare l'API `getAllDirty` per richiamare i documenti dirty.
   ```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        // handle success
    }).fail(function (errorObject) {
        // handle failure
   });
   ```
   {: codeblock}
   {: cordova}
6. Per eseguire il push delle modifiche a un adattatore, richiama `getAllDirty` per ottenere un elenco dei documenti con le modifiche e usa quindi `WLResourceRequest`. Dopo che i dati sono stati inviati e che è stata ricevuta una risposta di esito positivo, assicurati di richiamare `markClean`.
   ```javascript
   try {
     var collectionName = "people";
     var dirtyDocs;

     WL.JSONStore.get(collectionName)
     .getAllDirty()
     .then(function (arrayOfDirtyDocuments) {
        dirtyDocs = arrayOfDirtyDocuments;

        var resource = new WLResourceRequest("adapters/JSONStoreAdapter/pushPeople", WLResourceRequest.POST);
        resource.setQueryParameter('params', [dirtyDocs]);
        return resource.send();
        }).then(function (responseFromAdapter) {
            return WL.JSONStore.get(collectionName).markClean(dirtyDocs);
        }).then(function (res) {
          //handle success
        }).fail(function (errorObject) {
          // Handle failure.
        });

    } catch (e) {
        alert("Failed To Push Documents to Adapter");
    }
   ```
   {: codeblock}
   {: cordova}
7. Utilizza `enhance` per l'estendere l'API core per rispondere alle tue esigenze aggiungendo funzioni a un prototipo di raccolta. Questo esempio (il seguente frammento di codice) mostra come utilizzare `enhance` per aggiungere la funzione `getValue` che agisce sulla raccolta `keyvalue`. Prende una chiave (key) (stringa) come suo solo parametro e restituisce un singolo risultato.
   ```javascript
   var collectionName = 'keyvalue';
    WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
        var deferred = $.Deferred();
        var collection = this;
        //Do an exact search for the key
        collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject);
        return deferred.promise();
    });

    //Usage:
    var key = 'myKey';
    WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
        // handle success
        // result contains an array of documents with the results from the find
    }).fail(function () {
        // handle failure
   });
   ```
   {: codeblock}
   {: cordova}
8. Vedi l'esempio JSONStore per l'applicazione Cordova dalla sezione **Esempi**. Questo progetto contiene un'applicazione Cordova che utilizza il set di API JSONStore. Il progetto Maven dell'adattatore JavaScript può essere scaricato da [qui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

L'integrazione dell'adattatore è facoltativa e fornire i modi per inviare i dati da una raccolta a un adattatore e ottenere i dati da un adattatore in una raccolta.
Puoi raggiungere questi obiettivi utilizzando `WLResourceRequest`.
{: ios}

1. Crea un adattatore e denominalo **People**.
2. Definisci le sue procedure `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.
3. Per caricare i dati da un adattatore, utilizza `WLResourceRequest`.
   ```swift
    // Start - LoadFromAdapter
    class LoadFromAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        let responsePayload:NSDictionary = response.getResponseJson()
        let people:NSArray = responsePayload.objectForKey("peopleList") as! NSArray
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
}
    }
    // End - LoadFromAdapter

    let pull = WLResourceRequest(URL: NSURL(string: "/adapters/People/getPeople"), method: "GET")

    let loadDelegate:LoadFromAdapter = LoadFromAdapter()
    pull.sendWithDelegate(loadDelegate)
   ```
   {: codeblock}
   {: ios}
4. Il richiamo di `allDirty` restituisce un array di cosiddetti "documenti dirty", che sono documenti che hanno delle modifiche locali che non esistono sul sistema di back-end.
   ```swift
    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: ios}
   Per evitare che JSONStore contrassegni i documenti come "dirty", passa l'opzione `{markDirty:false}` a `add`, `replace` e `remove`.
   {: tip}
5. Per eseguire il push delle modifiche a un adattatore, richiama `allDirty` per ottenere un elenco dei documenti con le modifiche e usa quindi `WLResourceRequest`. Dopo che i dati sono stati inviati e che è stata ricevuta una risposta di esito positivo, assicurati di richiamare `markDocumentsClean`.
   ```swift
    // Start - PushToAdapter
    class PushToAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
}
    }
    // End - PushToAdapter

    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
      let pushData:NSData = NSKeyedArchiver.archivedDataWithRootObject(dirtyDocs)

      let push = WLResourceRequest(URL: NSURL(string: "/adapters/People/pushPeople"), method: "POST")

      let pushDelegate:PushToAdapter = PushToAdapter()
      push.sendWithData(pushData, delegate: pushDelegate)

    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: ios}
6. Scarica il progetto dell'applicazione iOS Swift nativa dalla sezione **Esempi**. Il progetto contiene un'applicazione iOS Swift nativa che utilizza il set di API JSONStore. Il progetto Maven dell'adattatore JavaScript può essere scaricato da [qui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: ios}

<!--### Android
{: #adapter_integration_android}-->

L'integrazione dell'adattatore è facoltativa e fornire i modi per inviare i dati da una raccolta a un adattatore e ottenere i dati da un adattatore in una raccolta.
Puoi raggiungere questi obiettivi utilizzando funzioni quali `WLResourceRequest` oppure la tua istanza di un `HttpClient` se hai bisogno di maggiore flessibilità.
{: android}

1. Crea un adattatore e denominalo **JSONStoreAdapter**.
2. Definisci le sue procedure `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.
3. Per caricare i dati da un adattatore, utilizza `WLResourceRequest`.
   ```java
    WLResponseListener responseListener = new WLResponseListener() {
      @Override
      public void onFailure(final WLFailResponse response) {
        // handle failure
      }
      @Override
      public void onSuccess(WLResponse response) {
        try {
          JSONArray loadedDocuments = response.getResponseJSON().getJSONArray("peopleList");
        } catch(Exception e) {
          // error decoding JSON data
        }
      }
    };

    try {
      WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/getPeople"), WLResourceRequest.GET);
      request.send(responseListener);
    } catch (URISyntaxException e) {
      // handle error
    }
   ```
   {: codeblock}
   {: android}
4. Il richiamo di `findAllDirtyDocuments` restituisce un array di cosiddetti "documenti dirty", che sono documenti che hanno delle modifiche locali che non esistono sul sistema di back-end.
   ```java
    Context  context = getContext();
    try {
      String collectionName = "people";
      JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
      List<JSONObject> dirtyDocs = collection.findAllDirtyDocuments();
      // handle success
    } catch(JSONStoreException e) {
      // handle failure
    }
   ```
   {: codeblock}
   {: android}
   Per evitare che JSONStore contrassegni i documenti come "dirty", passa l'opzione `options.setMarkDirty(false)` a `add`, `replace` e `remove`.
   {: tip}
   {: android}
5. Per eseguire il push delle modifiche a un adattatore, richiama `findAllDirtyDocuments` per ottenere un elenco dei documenti con le modifiche e usa quindi `WLResourceRequest`. Dopo che i dati sono stati inviati e che è stata ricevuta una risposta di esito positivo, assicurati di richiamare `markDocumentsClean`.
   ```java
    WLResponseListener responseListener = new WLResponseListener() {
      @Override
      public void onFailure(final WLFailResponse response) {
        // handle failure
      }
      @Override
      public void onSuccess(WLResponse response) {
        // handle success
      }
    };
    Context context = getContext();

    try {
      String collectionName = "people";
      JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
      List<JSONObject> dirtyDocuments = people.findAllDirtyDocuments();

      JSONObject payload = new JSONObject();
      payload.put("people", dirtyDocuments);

      WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/pushPeople"), WLResourceRequest.POST);
      request.send(payload, responseListener);
    } catch(JSONStoreException e) {
      // handle failure
    } catch (URISyntaxException e) {
      // handle error
    }
   ```
   {: codeblock}
   {: android}
6. Scarica il progetto dell'applicazione Android nativa dalla sezione **Esempi**. Il progetto contiene un'applicazione Android nativa che utilizza il set di API JSONStore. Il progetto Maven dell'adattatore JavaScript può essere scaricato da [qui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: android}
