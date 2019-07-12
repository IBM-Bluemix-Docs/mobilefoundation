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

# Offlinespeicher konfigurieren
{: #configure_offline_storage}

Mobile Foundation JSONStore ist eine optionale clientseitige API, die ein schlankes, dokumentorientiertes Speichersystem bereitstellt. JSONStore ermöglicht das persistente Speichern von JSON-Dokumenten. Dokumente in einer Anwendung sind auch dann in JSONStore verfügbar, wenn das Gerät, auf dem die Anwendung ausgeführt wird, offline ist. Dieser persistente und immer verfügbare Speicher kann nützlich sein, da Benutzer auch dann Zugriff auf Dokumente haben, wenn das Gerät keine Netzverbindung herstellen kann. [Hier](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore) finden Sie eine Übersicht über die Konzepte und die Terminologie von JSONStore.

Informationen zur erweiterten Konfiguration von Offlinespeicher finden Sie [hier](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

### Offlinespeicher für Cordova- oder Ionic-Apps konfigurieren
{: #configure_offline_storage_cordova}
{: cordova}

Stellen Sie sicher, dass das Mobile Foundation Cordova-SDK zum Projekt hinzugefügt wurde.
{: cordova}

Folgen Sie dem Lernprogramm [Mobile Foundation-SDK zu Cordova-Anwendungen hinzufügen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}
{: cordova}

#### JSONStore zu Ihrem Cordova-Projekt hinzufügen
{: #adding_jsonstore_cordova}
{: cordova}

1. Öffnen Sie ein Befehlszeilenfenster und navigieren Sie zu Ihrem Cordova-Projektordner.
2. Führen Sie den Befehl aus:
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### JSONStore-Sammlung initialisieren
{: #initialize_jsonstore_cordova}   
{: cordova}

Verwenden Sie `init`, um eine oder mehrere JSONStore-Sammlungen zu starten.
Das Starten oder Bereitstellen einer Sammlung bedeutet, dass der persistente Speicher erstellt wird, der die Sammlung und die Dokumente enthält, falls er nicht vorhanden ist. Wird ein Kennwort an die Option übergeben, wird der persistente Speicher mit dem Kennwort verschlüsselt.
{: cordova}

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};
WL.JSONStore.init(collections).then(function (collections) {
    // Erfolg behandeln - collection.people (people's collection)
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}
{: cordova}

#### Zugriffsmechanismus für Ihre Cordova-JSONStore-Sammlung abrufen
{: #get_jsonstore_cordova}
{: cordova}

Verwenden Sie `get`, um einen Zugriffsmechanismus für die Sammlung zu erstellen. Sie müssen `init` vor "get" aufrufen. Andernfalls bringt `get` das Ergebnis *undefined*.
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

Mit der Variablen *people* lassen sich nun Operationen für die Sammlung *people* ausführen, beispielsweise `add`, `find` und `replace`.
{: cordova}

#### Dokumente zu einer Cordova-Sammlung hinzufügen
{: #add_jsonstore_cordova}
{: cordova}

Verwenden Sie `add`, um Daten als Dokumente in einer Sammlung zu speichern.
{: cordova}

```javascript
var collectionName = 'people';
var options = {};
var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}
{: cordova}

#### Dokumente in einer Cordova-Sammlung suchen
{: #find_jsonstore_cordova}
{: cordova}

* Verwenden Sie `find`, um mithilfe einer Abfrage ein Dokument in einer Sammlung zu lokalisieren.
* Verwenden Sie `findAll`, um alle Dokumente in einer Sammlung abzurufen.
* Verwenden Sie `findById`, um nach der eindeutigen Dokumentkennung zu suchen.
{: cordova}

Das Standardverhalten von "find" ist eine "unscharfe" Suche.
{: cordova}

```javascript
var query = {name: 'yoel'};
var collectionName = 'people';
var options = {
  exact: false, // Standardwert
  limit: 10 // Rückgabe von maximal 10 Dokumenten. Standardeinstellung: Rückgabe aller Dokumente
};

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // Erfolg behandeln - Ergebnisse (Array mit Dokumenten gefunden)
}).fail(function (error) {
    // Fehler behandeln
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
    limit: 10 // Rückgabe von maximal 10 Dokumenten.
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // Erfolg behandeln - Ergebnisse (Array mit Dokumenten gefunden)
  }).fail(function (errorObject) {
    // Fehler behandeln
  });
}
```
{: codeblock}
{: cordova}

#### Dokumente in einer Cordova-Sammlung ersetzen
{: #replace_jsonstore_cordova}
{: cordova}

Verwenden Sie `replace`, um Dokumente in einer Sammlung zu ändern. Zum Ersetzen verwenden Sie das Feld `_id`, die eindeutige Dokumentkennung.
{: cordova}

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
};
var collectionName = 'people';
var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}
{: cordova}

Bei diesem Beispiel wird davon ausgegangen, dass sich das Dokument `{_id: 1, json: {name: 'yoel', age: 23} }` in der Sammlung befindet.
{: cordova}

#### Dokumente aus einer Cordova-Sammlung entfernen
{: #remove_jsonstore_cordova}
{: cordova}

Verwenden Sie `remove`, um ein Dokument aus einer Sammlung zu löschen.
Dokumente werden erst aus der Sammlung entfernt, wenn Sie "push" aufrufen.
{: cordova}

```javascript
var query = {_id: 1};
var collectionName = 'people';
var options = {exact: true};
WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}
{: cordova}

#### Gesamte Cordova-Sammlung entfernen
{: #remove_collection_jsonstore_cordova}
{: cordova}

Verwenden Sie `removeCollection`, um alle Dokumente zu löschen, die in einer Sammlung gespeichert sind. Diese Operation ähnelt dem Löschen einer Tabelle in einer Datenbank.
{: cordova}

#### Cordova-JSONStore löschen
{: #destroy_jsonstore_cordova}
{: cordova}

Verwenden Sie `destroy`, um die folgenden Daten zu entfernen:
* Alle Dokumente
* Alle Sammlungen
* Alle Speicher
* Alle JSONStore-Metadaten und -Sicherheitsartefakte
{: cordova}

### Offlinespeicher für iOS-Apps konfigurieren
{: #configure_offline_storage_ios}
{: ios}

Stellen Sie sicher, dass das Mobile Foundation Native-SDK zum XCode-Projekt hinzugefügt wurde.
{: ios}

Folgen Sie dem Lernprogramm [Mobile Foundation-SDK zu iOS-Anwendungen hinzufügen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}
{: ios}

#### JSONStore zu Ihrem iOS-Projekt hinzufügen
{: #adding_jsonstore_ios}
{: ios}

1. Fügen Sie zu der vorhandenen `Podfile` im Stammverzeichnis des Xcode-Projekts Folgendes hinzu.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. Navigieren Sie in einem Befehlszeilenfenster zum Stammverzeichnis des Xcode-Projekts und führen Sie den folgenden Befehl aus:
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
3. Wenn Sie JSONStore verwenden möchten, müssen Sie den JSONStore-Header importieren:
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

#### iOS-JSONStore-Sammlung öffnen
{: #open_ios}
{: ios}

Verwenden Sie `openCollections`, um eine oder mehrere JSONStore-Sammlungen zu öffnen.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")

collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

#### Zugriffsmechanismus für Ihre iOS-JSONStore-Sammlung abrufen
{: #get_jsonstore_ios}
{: ios}

Verwenden Sie `getCollectionWithName`, um einen Zugriffsmechanismus für die Sammlung zu erstellen. Sie müssen `openCollections` vor dem Aufruf von `getCollectionWithName` aufrufen.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

Mit der Variablen "collection" lassen sich nun Operationen für die Sammlung `people` ausführen, beispielsweise `add`, `find` und `replace`.
{: ios}

#### Dokumente zu einer iOS-Sammlung hinzufügen
{: #add_jsonstore_ios}
{: ios}

Verwenden Sie `addData`, um Daten als Dokumente in einer Sammlung zu speichern.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do  {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

#### Dokumente in einer iOS-Sammlung suchen
{: #find_jsonstore_ios}
{: ios}

Verwenden Sie `findWithQueryParts`, um mithilfe einer Abfrage ein Dokument in einer Sammlung zu lokalisieren. Verwenden Sie `findAllWithOptions`, um alle Dokumente in einer Sammlung abzurufen. Verwenden Sie `findWithIds`, um nach der eindeutigen Dokumentkennung zu suchen.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// Rückgabe von maximal 10 Dokumenten. Standardeinstellung: Rückgabe aller Dokumente
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do  {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

#### Dokumente in einer iOS-Sammlung ersetzen
{: #replace_jsonstore_ios}
{: ios}

Verwenden Sie `replaceDocuments`, um Dokumente in einer Sammlung zu ändern. Zum Ersetzen verwenden Sie das Feld `_id`, die eindeutige Dokumentkennung.
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
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

Bei diesem Beispiel wird davon ausgegangen, dass sich das Dokument `{_id: 1, json: {name: 'yoel', age: 23} }` in der Sammlung befindet.
{: ios}

#### Dokumente aus einer iOS-Sammlung entfernen
{: #remove_jsonstore_ios}
{: ios}

Verwenden Sie `removeWithIds`, um ein Dokument aus einer Sammlung zu löschen. Dokumente werden erst aus der Sammlung entfernt, wenn Sie `markDocumentClean` aufrufen.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

#### Gesamte iOS-Sammlung entfernen
{: #remove_collection_jsonstore_ios}
{: ios}

Verwenden Sie `removeCollection`, um alle Dokumente zu löschen, die in einer Sammlung gespeichert sind. Diese Operation ähnelt dem Löschen einer Tabelle in einer Datenbank.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

#### iOS-JSONStore löschen
{: #destroy_jsonstore_ios}
{: ios}

Verwenden Sie `destroyData`, um die folgenden Daten zu entfernen:
* Alle Dokumente
* Alle Sammlungen
* Alle Speicher
* Alle JSONStore-Metadaten und -Sicherheitsartefakte
{: ios}

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

### Offlinespeicher für Android-Apps konfigurieren
{: #configure_offline_storage_android}
{: android}

Stellen Sie sicher, dass das Mobile Foundation Native-SDK zum Android Studio-Projekt hinzugefügt wurde.
{: android}

Folgen Sie dem Lernprogramm [Mobile Foundation-SDK zu Android-Anwendungen hinzufügen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}
{: android}

#### JSONStore zu Ihrem Android-Projekt hinzufügen
{: #adding_jsonstore_android}
{: android}

1. Wählen Sie unter **Android → Gradle-Scripts** die Datei `build.gradle (Module: app)` aus.
2. Fügen Sie zum Bereich `dependencies` Folgendes hinzu:
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. Fügen Sie zum Bereich `DefaultConfig` Ihrer Datei `build.gradle` Folgendes hinzu:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ```
   {: codeblock}
   {: android}
   `abiFilters` wird hinzugefügt, um sicherzustellen, dass die Apps mit JSONStore in allen zuvor angegebenen Architekturen funktionieren. Dies ist erforderlich, da JSONStore von einer Bibliothek eines anderen Anbieters abhängig ist, die nur diese Architekturen unterstützt.
   {: note}
   {: android}

#### Android-JSONStore-Sammlung öffnen
{: #open_android}
{: android}

Verwenden Sie `openCollections`, um eine oder mehrere JSONStore-Sammlungen zu öffnen.
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
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

#### Zugriffsmechanismus für Ihre Android-JSONStore-Sammlung abrufen
{: #get_jsonstore_android}
{: android}

Verwenden Sie `getCollectionByName`, um einen Zugriffsmechanismus für die Sammlung zu erstellen. Sie müssen `openCollections` vor dem Aufruf von `getCollectionByName` aufrufen.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

Mit der Variablen "collection" lassen sich nun Operationen für die Sammlung `people` ausführen, beispielsweise `add`, `find` und `replace`.
{: android}

#### Dokumente zu einer Android-Sammlung hinzufügen
{: #add_jsonstore_android}
{: android}

Verwenden Sie `addData`, um Daten als Dokumente in einer Sammlung zu speichern.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // Optionen hinzufügen.
  JSONStoreAddOptions options = new JSONStoreAddOptions();
  options.setMarkDirty(true);
  JSONObject data = new JSONObject("{age: 23, name: 'yoel'}")
  collection.addData(data, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

#### Dokumente in einer Android-Sammlung suchen
{: #find_jsonstore_android}
{: android}

Verwenden Sie `findDocuments`, um mithilfe einer Abfrage ein Dokument in einer Sammlung zu lokalisieren. Verwenden Sie `findAllDocuments`, um alle Dokumente in einer Sammlung abzurufen. Verwenden Sie `findDocumentById`, um nach der eindeutigen Dokumentkennung zu suchen.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreQueryPart queryPart = new JSONStoreQueryPart();
  // unscharfe Suche LIKE
  queryPart.addLike("name", name);
  JSONStoreQueryParts query = new JSONStoreQueryParts();
  query.addQueryPart(queryPart);
  JSONStoreFindOptions options = new JSONStoreFindOptions();
  // Rückgabe von maximal 10 Dokumenten. Standardeinstellung: Rückgabe aller Dokumente
  options.setLimit(10);
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> results = collection.findDocuments(query, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

#### Dokumente in einer Android-Sammlung ersetzen
{: #replace_jsonstore_android}
{: android}

Verwenden Sie `replaceDocuments`, um Dokumente in einer Sammlung zu ändern. Zum Ersetzen verwenden Sie das Feld `_id`, die eindeutige Dokumentkennung.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreReplaceOptions options = new JSONStoreReplaceOptions();
  // Daten als vorläufig markieren
  options.setMarkDirty(true);
  JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}");
  collection.replaceDocument(replacement, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

Bei diesem Beispiel wird davon ausgegangen, dass sich das Dokument `{_id: 1, json: {name: 'yoel', age: 23} }` in der Sammlung befindet.
{: android}

#### Dokumente aus einer Android-Sammlung entfernen
{: #remove_jsonstore_android}
{: android}

Verwenden Sie `removeDocumentById`, um ein Dokument aus einer Sammlung zu löschen. Dokumente werden erst aus der Sammlung entfernt, wenn Sie `markDocumentClean` aufrufen.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreRemoveOptions options = new JSONStoreRemoveOptions();
  // Daten als vorläufig markieren
  options.setMarkDirty(true);
  collection.removeDocumentById(1, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

#### Gesamte Android-Sammlung entfernen
{: #remove_collection_jsonstore_android}
{: android}

Verwenden Sie `removeCollection`, um alle Dokumente zu löschen, die in einer Sammlung gespeichert sind. Diese Operation ähnelt dem Löschen einer Tabelle in einer Datenbank.
{: android}

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}

#### Android-JSONStore löschen
{: #destroy_jsonstore_android}
{: android}

Verwenden Sie `destroy`, um die folgenden Daten zu entfernen:
* Alle Dokumente
* Alle Sammlungen
* Alle Speicher
* Alle JSONStore-Metadaten und -Sicherheitsartefakte
{: android}

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}
{: android}
