---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: JSONStore, offline storage, add jsonstore to cordova, add jsonstore to iOS, add jsonstore to android, jsonstore methods, jsonstore operations

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

# Configura l'archiviazione offline
{: #configure_offline_storage}

JSONStore Mobile Foundation è un'API lato client facoltativa che fornisce un semplice sistema di archiviazione orientato ai documenti. JSONStore abilita l'archiviazione persistente di documenti JSON. I documenti presenti in un'applicazione sono disponibili in JSONStore anche quando il dispositivo su cui è in esecuzione l'applicazione è offline. Questa archiviazione persistente e sempre disponibile può essere utile per consentire agli utenti di accedere ai documenti quando, ad esempio, non è disponibile alcuna connessione di rete nel dispositivo. Per una panoramica dei concetti e per la terminologia di JSONStore, vedi [qui](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore).

Per la configurazione avanzata dell'archiviazione offline, vedi [qui](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

### Configura l'archiviazione offline per le applicazioni Cordova o Iconic
{: #configure_offline_storage_cordova}
{: cordova}

Assicurati che l'SDK Mobile Foundation Cordova sia stato aggiunto al progetto.
{: cordova}

Attieniti all'esercitazione [Adding the Mobile Foundation SDK to Cordova applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}
{: cordova}

#### Aggiunta di JSONStore al tuo progetto Cordova
{: #adding_jsonstore_cordova}
{: cordova}

1. Apri una finestra di riga di comando e vai alla tua cartella del progetto Cordova.
2. Esegui il comando:
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### Inizializza la raccolta JSONStore
{: #initialize_jsonstore_cordova}   
{: cordova}

Utilizza `init` per avviare una o più raccolte JSONStore.
Avviare o eseguire il provisioning di una raccolta significa creare l'archiviazione persistente che contiene la raccolta e i documenti, se non esiste. Se viene passata una password a options, l'archiviazione persistente viene crittografata con la password.
{: cordova}

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};
WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}
{: cordova}

#### Ottieni un accessor alla tua raccolta Cordova JSONStore
{: #get_jsonstore_cordova}
{: cordova}

Utilizza `get` per creare un accessor alla raccolta. Devi richiamare `init` prima di richiamare get, altrimenti il risultato di `get` sarà indefinito (*undefined*).
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

La variabile *people* può ora essere utilizzata per eseguire operazioni sulla raccolta *people* quali `add`, `find` e `replace`.
{: cordova}

#### Aggiungi documenti a una raccolta Cordova
{: #add_jsonstore_cordova}
{: cordova}

Utilizza `add` per archiviare i dati come documenti all'interno di una raccolta.
{: cordova}

```javascript
var collectionName = 'people';
var options = {};
var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}
{: cordova}

#### Trova documenti all'interno di una raccolta Cordova
{: #find_jsonstore_cordova}
{: cordova}

* Utilizza `find` per individuare un documento all'interno di una raccolta utilizzando una query.
* Utilizza `findAll` per richiamare tutti i documenti all'interno di una raccolta.
* Utilizza `findById` per cercare in base all'identificativo univoco del documento.
{: cordova}

La modalità di funzionamento predefinita per find consiste nell'eseguire una ricerca "fuzzy".
{: cordova}

```javascript
var query = {name: 'yoel'};
var collectionName = 'people';
var options = {
  exact: false, //default
  limit: 10 // returns a maximum of 10 documents, default: return every document
};

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // handle success - results (array of documents found)
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}
{: cordova}

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){
  alert("Please enter a valid age to find");
}
else {
  query = {age: parseInt(age, 10)};
  var options = {
    exact: true,
    limit: 10 //returns a maximum of 10 documents
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure
  });
}
```
{: codeblock}
{: cordova}

#### Sostituisci documenti all'interno di una raccolta Cordova
{: #replace_jsonstore_cordova}
{: cordova}

Utilizza `replace` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id`, l'identificativo univoco del documento.
{: cordova}

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
};
var collectionName = 'people';
var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}
{: cordova}

Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.
{: cordova}

#### Rimuovi documenti da una raccolta Cordova
{: #remove_jsonstore_cordova}
{: cordova}

Utilizza `remove` per eliminare un documento da una raccolta.
I documenti non vengono cancellati dalla raccolta finché non richiami push.
{: cordova}

```javascript
var query = {_id: 1};
var collectionName = 'people';
var options = {exact: true};
WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}
{: cordova}

#### Rimuovi un'intera raccolta Cordova
{: #remove_collection_jsonstore_cordova}
{: cordova}

Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.
{: cordova}

#### Elimina permanentemente Cordova JSONStore
{: #destroy_jsonstore_cordova}
{: cordova}

Utilizza `destroy` per rimuovere i seguenti dati:
* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore
{: cordova}

### Configura l'archiviazione offline per le applicazioni iOS
{: #configure_offline_storage_ios}
{: ios}

Assicurati che l'SDK nativo Mobile Foundation sia stato aggiunto al progetto Xcode.
{: ios}

Attieniti all'esercitazione [Adding the Mobile Foundation SDK to iOS applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}
{: ios}

#### Aggiunta di JSONStore al tuo progetto iOS
{: #adding_jsonstore_ios}
{: ios}

1. Aggiungi quanto segue al `podfile` esistente, alla root del progetto Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. Dalla riga di comando, vai alla root del progetto Xcode ed esegui il comando:
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
3. Ogni volta che vuoi usare JSONStore, assicurati di importare l'intestazione JSONStore:
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ```
   {: codeblock}
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   
   {: codeblock}
   {: ios}

#### Apri la raccolta iOS JSONStore
{: #open_ios}
{: ios}

Utilizza `openCollections` per aprire una o più raccolte JSONStore.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")

collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Ottieni un accessor alla tua raccolta iOS JSONStore
{: #get_jsonstore_ios}
{: ios}

Utilizza `getCollectionWithName` per creare un accessor alla raccolta. Devi richiamare `openCollections` prima di richiamare `getCollectionWithName`.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

La variabile collection può ora essere utilizzata per eseguire operazioni sulla raccolta `people` quali `add`, `find` e `replace`.
{: ios}

#### Aggiungi documenti a una raccolta iOS
{: #add_jsonstore_ios}
{: ios}

Utilizza `addData` per archiviare i dati come documenti all'interno di una raccolta.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do  {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Trova documenti all'interno di una raccolta iOS
{: #find_jsonstore_ios}
{: ios}

Utilizza `findWithQueryParts` per individuare un documento all'interno di una raccolta utilizzando una query. Utilizza `findAllWithOptions` per richiamare tutti i documenti all'interno di una raccolta. Utilizza `findWithIds` per cercare in base all'identificativo univoco del documento.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do  {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Sostituisci documenti all'interno di una raccolta iOS
{: #replace_jsonstore_ios}
{: ios}

Utilizza `replaceDocuments` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id`, l'identificativo univoco del documento.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

var document:Dictionary<String,AnyObject> = Dictionary()
document["name"] = "chevy"
document["age"] = 23

var replacement:Dictionary<String, AnyObject> = Dictionary()
replacement["_id"] = 1
replacement["json"] = document

do {
  try collection.replaceDocuments([replacement], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.
{: ios}

#### Rimuovi documenti da una raccolta iOS
{: #remove_jsonstore_ios}
{: ios}

Utilizza `removeWithIds` per eliminare un documento da una raccolta. I documenti non vengono cancellati dalla raccolta finché non richiami `markDocumentClean`.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Rimuovi un'intera raccolta iOS
{: #remove_collection_jsonstore_ios}
{: ios}

Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Elimina permanentemente un iOS JSONStore
{: #destroy_jsonstore_ios}
{: ios}

Utilizza `destroyData` per rimuovere i seguenti dati:
* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore
{: ios}

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

### Configura l'archiviazione offline per le applicazioni Android
{: #configure_offline_storage_android}
{: android}

Assicurati che l'SDK nativo Mobile Foundation sia stato aggiunto al progetto Android Studio.
{: android}

Attieniti all'esercitazione [Adding the Mobile Foundation SDK to Android applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}
{: android}

#### Aggiunta di JSONStore al tuo progetto Android
{: #adding_jsonstore_android}
{: android}

1. Da **Android → Gradle Scripts**, seleziona il file `build.gradle (Module: app)`.
2. Aggiungi quanto segue alla sezione `dependencies` esistente:
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. Aggiungi quanto segue alla sezione `DefaultConfig` del tuo file `build.gradle`:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ```
   {: codeblock}
   {: android}
   Aggiungiamo `abiFilters` per garantire che le applicazioni con JSONStore verranno eseguite in una qualsiasi delle architetture precedentemente specificate. Ciò è obbligatorio poiché JSONStore dipende da una libreria di terze parti che supporta solo queste architetture.
   {: note}
   {: android}

#### Apri una raccolta Android JSONStore
{: #open_android}
{: android}

Utilizza `openCollections` per aprire una o più raccolte JSONStore.
{: android}

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  WLJSONStore.getInstance(context).openCollections(collections);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

#### Ottieni un accessor alla tua raccolta Android JSONStore
{: #get_jsonstore_android}
{: android}

Utilizza `getCollectionByName` per creare un accessor alla raccolta. Devi richiamare `openCollections` prima di richiamare `getCollectionByName`.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

La variabile collection può ora essere utilizzata per eseguire operazioni sulla raccolta `people` quali `add`, `find` e `replace`.
{: android}

#### Aggiungi documenti a una raccolta Android
{: #add_jsonstore_android}
{: android}

Utilizza `addData` per archiviare i dati come documenti all'interno di una raccolta.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions();
  options.setMarkDirty(true);
  JSONObject data = new JSONObject("{age: 23, name: 'yoel'}")
  collection.addData(data, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

#### Trova documenti all'interno di una raccolta Android
{: #find_jsonstore_android}
{: android}

Utilizza `findDocuments` per individuare un documento all'interno di una raccolta utilizzando una query. Utilizza `findAllDocuments` per richiamare tutti i documenti all'interno di una raccolta. Utilizza `findDocumentById` per cercare in base all'identificativo univoco del documento.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreQueryPart queryPart = new JSONStoreQueryPart();
  // fuzzy search LIKE
  queryPart.addLike("name", name);
  JSONStoreQueryParts query = new JSONStoreQueryParts();
  query.addQueryPart(queryPart);
  JSONStoreFindOptions options = new JSONStoreFindOptions();
  // returns a maximum of 10 documents, default: returns every document
  options.setLimit(10);
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> results = collection.findDocuments(query, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

#### Sostituisci documenti all'interno di una raccolta Android
{: #replace_jsonstore_android}
{: android}

Utilizza `replaceDocuments` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id`, l'identificativo univoco del documento.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreReplaceOptions options = new JSONStoreReplaceOptions();
  // mark data as dirty
  options.setMarkDirty(true);
  JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}");
  collection.replaceDocument(replacement, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.
{: android}

#### Rimuovi documenti da una raccolta Android
{: #remove_jsonstore_android}
{: android}

Utilizza `removeDocumentById` per eliminare un documento da una raccolta. I documenti non vengono cancellati dalla raccolta finché non richiami `markDocumentClean`.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreRemoveOptions options = new JSONStoreRemoveOptions();
  // Mark data as dirty
  options.setMarkDirty(true);
  collection.removeDocumentById(1, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

#### Rimuovi un'intera raccolta Android
{: #remove_collection_jsonstore_android}
{: android}

Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

#### Elimina permanentemente un Android JSONStore
{: #destroy_jsonstore_android}
{: android}

Utilizza `destroy` per rimuovere i seguenti dati:
* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore
{: android}

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}
