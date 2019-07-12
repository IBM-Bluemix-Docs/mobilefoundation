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

# JSONStore - erweitert
{: #advanced_jsonstore}

## Sicherheit in JSONStore
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

Sie können alle Sammlungen in einem Speicher sichern, indem Sie ein Kennwort an die Funktion `init` übergeben. Wenn kein Kennwort übergeben wird, werden die Dokumente aller Sammlungen im Speicher nicht verschlüsselt.
{: cordova}

Die Datenverschlüsselung ist nur in Android-, iOS-, Windows 8.1 Universal- und Windows 10 UWP-Umgebungen verfügbar.
Einige Sicherheitsmetadaten werden in der Schlüsselkette (iOS), den gemeinsam genutzten Vorgaben (Android) oder dem Fach für Berechtigungsnachweise (Windows 8.1) gespeichert.
Der Speicher wird mit einem 256-Bit-AES-Schlüssel (AES - Advanced Encryption Standard) verschlüsselt. Alle Schlüssel werden mit Password-Based Key Derivation Function 2 (PBKDF2) verstärkt.
{: cordova}

Sperren Sie mit `closeAll` den Zugriff auf alle Sammlungen, bis Sie `init` erneut aufrufen. Wenn Sie sich `init` als eine Anmeldefunktion vorstellen, können Sie `closeAll` als die entsprechende Abmeldefunktion betrachten. Ändern Sie das Kennwort mit `changePassword`.
{: cordova}

Die Verschlüsselung wird nur in iOS unterstützt. Standardmäßig verwendet das Mobile Foundation Cordova-SDK für iOS zur Verschlüsselung APIs, die von iOS bereitgestellt werden. Gehen Sie wie folgt vor, wenn Sie diese lieber durch OpenSSL ersetzen möchten:
{: cordova}

* Fügen Sie das Plug-in `cordova-plugin-mfp-encrypt-utils` hinzu:
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* Verwenden Sie in der Anwendungslogik `WL.SecurityUtils.enableNativeEncryption(false)`, um die Option für OpenSSL zu aktivieren.
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

Sie können alle Sammlungen in einem Speicher sichern, indem Sie ein `JSONStoreOpenOptions`-Objekt mit einem Kennwort an die Funktion `openCollections` übergeben. Wenn kein Kennwort übergeben wird, werden die Dokumente aller Sammlungen im Speicher nicht verschlüsselt.
{: ios}

Einige Sicherheitsmetadaten werden in der Schlüsselkette (iOS) gespeichert.
Der Speicher wird mit einem 256-Bit-AES-Schlüssel (AES - Advanced Encryption Standard) verschlüsselt. Alle Schlüssel werden mit Password-Based Key Derivation Function 2 (PBKDF2) verstärkt.
{: ios}

Sperren Sie mit `closeAllCollections` den Zugriff auf alle Sammlungen, bis Sie `openCollections` erneut aufrufen. Wenn Sie sich `openCollections` als eine Anmeldefunktion vorstellen, können Sie `closeAllCollections` als die entsprechende Abmeldefunktion betrachten.
{: ios}

Ändern Sie das Kennwort mit `changeCurrentPassword`.
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
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

<!--### Android
{: #security_jsonstore_android} -->

Sie können alle Sammlungen in einem Speicher sichern, indem Sie ein `JSONStoreInitOptions`-Objekt mit einem Kennwort an die Funktion `openCollections` übergeben. Wenn kein Kennwort übergeben wird, werden die Dokumente aller Sammlungen im Speicher nicht verschlüsselt.
{: android}

Einige Sicherheitsmetadaten werden in gemeinsam genutzten Vorgaben (Android) gespeichert.
Der Speicher wird mit einem 256-Bit-AES-Schlüssel (AES - Advanced Encryption Standard) verschlüsselt. Alle Schlüssel werden mit Password-Based Key Derivation Function 2 (PBKDF2) verstärkt.
{: android}

Sperren Sie mit `closeAllCollections` den Zugriff auf alle Sammlungen, bis Sie `openCollections` erneut aufrufen. Wenn Sie sich `openCollections` als eine Anmeldefunktion vorstellen, können Sie `closeAllCollections` als die entsprechende Abmeldefunktion betrachten.
{: android}

Ändern Sie das Kennwort mit `changeCurrentPassword`.
{: android}

```java
Context  context = getContext();
    try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  JSONStoreInitOptions options = new JSONStoreInitOptions();
  options.setPassword("123");
  WLJSONStore.getInstance(context).openCollections(collections, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
    }
```
{: codeblock}
{: android}

## Unterstützung mehrerer Benutzer in JSONStore
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

Sie können in einer einzigen MobileFirst-Anwendung mehrere Speicher erstellen, die verschiedene Sammlungen enthalten. Die Funktion `init` kann ein Optionsobjekt mit einem Benutzernamen annehmen. Wenn kein Benutzername angegeben wird, lautet der Standardbenutzername *jsonstore*.
{: cordova}

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {username: 'yoel'};
WL.JSONStore.init(collections, options).then(function () {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}
{: cordova}

<!--### iOS
{: #multiple_user_jsonstore_ios} -->

Sie können in einer einzigen MobileFirst-Anwendung mehrere Speicher erstellen, die verschiedene Sammlungen enthalten. Die Funktion `init` kann ein Optionsobjekt mit einem Benutzernamen annehmen. Wenn kein Benutzername angegeben wird, lautet der Standardbenutzername *jsonstore*.
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
  // Fehler behandeln
}
```
{: codeblock}
{: ios}

<!--### Android
{: #multiple_user_jsonstore_android} -->

Sie können in einer einzigen Mobile Foundation-Anwendung mehrere Speicher erstellen, die verschiedene Sammlungen enthalten. Die Funktion `openCollections` kann ein Optionsobjekt mit einem Benutzernamen annehmen. Wenn kein Benutzername angegeben wird, lautet der Standardbenutzername *jsonstore*.
{: android}

```java
Context  context = getContext();
    try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  JSONStoreInitOptions options = new JSONStoreInitOptions();
  options.setUsername("yoel");
  WLJSONStore.getInstance(context).openCollections(collections, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
    }
```
{: codeblock}
{: android}

## Adapterintegration
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

Die Adapterintegration ist optional und bietet Möglichkeiten zum Senden von Daten aus einer Sammlung an einen Adapter und zum Abrufen von Daten aus einem Adapter in eine Sammlung.
Sie können diese Ziele mithilfe von `WLResourceRequest` oder `jQuery.ajax` erreichen, wenn Sie mehr Flexibilität benötigen.
{: cordova}

1. Erstellen Sie einen Adapter mit dem Namen **JSONStoreAdapter**.
2. Definieren Sie seine Prozeduren `addPerson`, `getPeople`, `pushPeople`, `removePerson` und `replacePerson`.
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
3. Verwenden Sie `WLResourceRequest`, um Daten von einem Adapter zu laden.
   ```javascript
   try {
     var resource = new WLResourceRequest("adapters/JSONStoreAdapter/getPeople", WLResourceRequest.GET);
     resource.send()
     .then(function (responseFromAdapter) {
          var data = responseFromAdapter.responseJSON.peopleList;
     },function(err){
          // Fehler behandeln
     });
    } catch (e) {
        alert("Failed to load data from adapter " + e.Messages);
    }
   ```
   {: cordova}   
4. Beim Aufruf von `getPushRequired` wird ein Array so genannter "vorläufiger Dokumente" zurückgegeben. Dies sind Dokumente mit lokalen Änderungen, die auf dem Back-End-System nicht vorhanden sind. Diese Dokumente werden an den Adapter gesendet, wenn  `push` aufgerufen wird.
   ```javascript
   var collectionName = 'people';
   WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // Erfolg behandeln
   }).fail(function (error) {
      // Fehler behandeln
   });
   ```
   {: codeblock}
   {: cordova}
   Damit JSONStore die Dokumente nicht als "vorläufig" markiert, müssen Sie die Option `{markDirty:false}` an `add`, `replace` und `remove` übergeben.
   {: tip}
   {: cordova}
5. Sie können die vorläufigen Dokumente auch mit der API `getAllDirty` abrufen.
   ```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        // Erfolg behandeln
    }).fail(function (errorObject) {
        // Fehler behandeln
});
   ```
   {: codeblock}
   {: cordova}
6. Wenn Sie Änderungen per Push-Operation an einen Adapter übergeben wollen, müssen Sie `getAllDirty` aufrufen, um eine Liste der Dokumente mit Änderungen abzurufen, und danach `WLResourceRequest` verwenden. Nachdem die Daten gesendet wurden und eine erfolgreiche Antwort empfangen wurde, müssen Sie `markClean` aufrufen.
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
          // Erfolg behandeln
        }).fail(function (errorObject) {
          // Fehler behandeln.
        });

    } catch (e) {
        alert("Failed To Push Documents to Adapter");
    }
   ```
   {: codeblock}
   {: cordova}
7. Erweitern Sie die API-Kerndefinitionen mit `enhance` entsprechend Ihren Anforderungen, indem Sie Funktionen zu einem Sammlungsprototyp hinzufügen. In diesem Beispiel (dem folgenden Code-Snippet) wird gezeigt, wie Sie mithilfe von `enhance` die Funktion `getValue` hinzufügen, die sich auf die Sammlung `keyvalue` auswirkt. Sie nimmt als einzigen Parameter "key" (eine Zeichenfolge) an und gibt ein einziges Ergebnis zurück.
   ```javascript
   var collectionName = 'keyvalue';
    WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
        var deferred = $.Deferred();
        var collection = this;
        // Exakte Suche nach dem Schlüssel durchführen
        collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject);
        return deferred.promise();
    });

    // Syntax:
    var key = 'myKey';
    WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
        // Erfolg behandeln
        // Das Ergebnis ist ein Array mit Dokumenten mit den Ergebnissen der Suche.
    }).fail(function () {
        // Fehler behandeln
});
   ```
   {: codeblock}
   {: cordova}
8. Im Bereich **Beispiele** finden Sie das JSONStore-Beispiel für Cordova-Apps. Dieses Projekt enthält eine Cordova-Anwendung, die das JSONStore-API-Set verwendet. [Hier](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) können Sie ein JavaScript-Adapter-Maven-Projekt herunterladen.
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

Die Adapterintegration ist optional und bietet Möglichkeiten zum Senden von Daten aus einer Sammlung an einen Adapter und zum Abrufen von Daten aus einem Adapter in eine Sammlung.
Sie können diese Ziele mithilfe von `WLResourceRequest` erreichen.
{: ios}

1. Erstellen Sie einen Adapter und geben Sie ihm den Namen **People**.
2. Definieren Sie seine Prozeduren `addPerson`, `getPeople`, `pushPeople`, `removePerson` und `replacePerson`.
3. Verwenden Sie `WLResourceRequest`, um Daten von einem Adapter zu laden.
   ```swift
    // Start - LoadFromAdapter
    class LoadFromAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        let responsePayload:NSDictionary = response.getResponseJson()
        let people:NSArray = responsePayload.objectForKey("peopleList") as! NSArray
        // Erfolg behandeln
      }

      func onFailure(response: WLFailResponse!) {
        // Fehler behandeln
      }
    }
    // Ende - LoadFromAdapter

    let pull = WLResourceRequest(URL: NSURL(string: "/adapters/People/getPeople"), method: "GET")

    let loadDelegate:LoadFromAdapter = LoadFromAdapter()
    pull.sendWithDelegate(loadDelegate)
   ```
   {: codeblock}
   {: ios}
4. Beim Aufruf von `allDirty` wird ein Array so genannter "vorläufiger Dokumente" zurückgegeben. Dies sind Dokumente mit lokalen Änderungen, die auf dem Back-End-System nicht vorhanden sind.
   ```swift
    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
    } catch let error as NSError {
      // Fehler behandeln
    }
   ```
   {: codeblock}
   {: ios}
   Damit JSONStore die Dokumente nicht als "vorläufig" markiert, müssen Sie die Option `{markDirty:false}` an `add`, `replace` und `remove` übergeben.
   {: tip}
5. Wenn Sie Änderungen per Push-Operation an einen Adapter übergeben wollen, müssen Sie `allDirty` aufrufen, um eine Liste der Dokumente mit Änderungen abzurufen, und danach `WLResourceRequest` verwenden. Nachdem die Daten gesendet wurden und eine erfolgreiche Antwort empfangen wurde, müssen Sie `markDocumentsClean` aufrufen.
   ```swift
    // Start - PushToAdapter
    class PushToAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        // Erfolg behandeln
      }

      func onFailure(response: WLFailResponse!) {
        // Fehler behandeln
      }
    }
    // Ende - PushToAdapter

    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
      let pushData:NSData = NSKeyedArchiver.archivedDataWithRootObject(dirtyDocs)

      let push = WLResourceRequest(URL: NSURL(string: "/adapters/People/pushPeople"), method: "POST")

      let pushDelegate:PushToAdapter = PushToAdapter()
      push.sendWithData(pushData, delegate: pushDelegate)

    } catch let error as NSError {
      // Fehler behandeln
    }
   ```
   {: codeblock}
   {: ios}
6. Laden Sie das native iOS-Swift-Anwendungsprojekt aus dem Bereich **Beispiele** herunter. Das Projekt enthält eine native iOS-Swift-Anwendung, die das JSONStore-API-Set verwendet. [Hier](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) können Sie ein JavaScript-Adapter-Maven-Projekt herunterladen.
{: ios}

<!--### Android
{: #adapter_integration_android}-->

Die Adapterintegration ist optional und bietet Möglichkeiten zum Senden von Daten aus einer Sammlung an einen Adapter und zum Abrufen von Daten aus einem Adapter in eine Sammlung.
Sie können diese Ziele mithilfe von Funktionen wie `WLResourceRequest` oder mit Ihrer eigenen Instanz von `HttpClient` erreichen, wenn Sie mehr Flexibilität benötigen.
{: android}

1. Erstellen Sie einen Adapter mit dem Namen **JSONStoreAdapter**.
2. Definieren Sie seine Prozeduren `addPerson`, `getPeople`, `pushPeople`, `removePerson` und `replacePerson`.
3. Verwenden Sie `WLResourceRequest`, um Daten von einem Adapter zu laden.
   ```java
    WLResponseListener responseListener = new WLResponseListener() {
      @Override
      public void onFailure(final WLFailResponse response) {
        // Fehler behandeln
      }
      @Override
      public void onSuccess(WLResponse response) {
        try {
          JSONArray loadedDocuments = response.getResponseJSON().getJSONArray("peopleList");
        } catch(Exception e) {
          // Fehler beim Decodieren von JSON-Daten
        }
      }
    };

    try {
      WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/getPeople"), WLResourceRequest.GET);
      request.send(responseListener);
    } catch (URISyntaxException e) {
      // Fehler behandeln
    }
   ```
   {: codeblock}
   {: android}
4. Beim Aufruf von `findAllDirtyDocuments` wird ein Array so genannter "vorläufiger Dokumente" zurückgegeben. Dies sind Dokumente mit lokalen Änderungen, die auf dem Back-End-System nicht vorhanden sind.
   ```java
    Context  context = getContext();
    try {
      String collectionName = "people";
      JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
      List<JSONObject> dirtyDocs = collection.findAllDirtyDocuments();
      // Erfolg behandeln
    } catch(JSONStoreException e) {
      // Fehler behandeln
    }
   ```
   {: codeblock}
   {: android}
   Damit JSONStore die Dokumente nicht als "vorläufig" markiert, müssen Sie die Option `options.setMarkDirty(false)` an `add`, `replace` und `remove` übergeben.
   {: tip}
   {: android}
5. Wenn Sie Änderungen per Push-Operation an einen Adapter übergeben wollen, müssen Sie `findAllDirtyDocuments` aufrufen, um eine Liste der Dokumente mit Änderungen abzurufen, und danach `WLResourceRequest` verwenden. Nachdem die Daten gesendet wurden und eine erfolgreiche Antwort empfangen wurde, müssen Sie `markDocumentsClean` aufrufen.
   ```java
    WLResponseListener responseListener = new WLResponseListener() {
      @Override
      public void onFailure(final WLFailResponse response) {
        // Fehler behandeln
      }
      @Override
      public void onSuccess(WLResponse response) {
        // Erfolg behandeln
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
      // Fehler behandeln
    } catch (URISyntaxException e) {
      // Fehler behandeln
    }
   ```
   {: codeblock}
   {: android}
6. Laden Sie das native Android-Anwendungsprojekt aus dem Bereich **Beispiele** herunter. Das Projekt enthält eine native Android-Anwendung, die das JSONStore-API-Set verwendet. [Hier](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) können Sie ein JavaScript-Adapter-Maven-Projekt herunterladen.
{: android}
