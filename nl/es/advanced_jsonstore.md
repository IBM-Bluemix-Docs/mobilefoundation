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

# JSONStore avanzado
{: #advanced_jsonstore}

## Seguridad en JSONStore
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

Proteja todas las recopilaciones en un almacén pasando una contraseña para la función `init`. Si no se pasa una contraseña, los documentos de todas las recopilaciones en el almacén no se cifran.
{: cordova}

El cifrado de datos solo está disponible en entornos Android, iOS, Windows 8.1 Universal y Windows 10 UWP.
Algunos metadatos de seguridad se almacenan la cadena de claves (iOS), en las preferencias compartidas (Android) o en la caja de seguridad de credenciales (Windows 8.1).
El almacén se cifra con una clave AES (Advanced Encryption Standard) de 256 bits. Todas las claves están reforzadas mediante PBKDF2 (Password-Based Key Derivation Function 2).
{: cordova}

Utilice `closeAll` para bloquear el acceso a las recopilaciones hasta que llame a `init` de nuevo. Si interpreta `init` como una función de inicio de sesión, puede interpretar `closeAll` como la correspondiente función de finalización de sesión. Utilice `changePassword` para cambiar la contraseña.
{: cordova}

Solo se ofrece soporte del cifrado en iOS. De forma predeterminada, el SDK de Cordova para iOS de Mobile Foundation se basa en las API que proporciona iOS para el cifrado. Si prefiere utilizar OpenSSL:
{: cordova}

* Añada el plugin `cordova-plugin-mfp-encrypt-utils`:
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* En la lógica de la aplicación, utilice: `WL.SecurityUtils.enableNativeEncryption(false)` para habilitar la opción OpenSSL.
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

Proteja todas las recopilaciones en un almacén pasando un objeto `JSONStoreOpenOptions` con una contraseña para la función `openCollections`. Si no se pasa una contraseña, los documentos de todas las recopilaciones en el almacén no se cifran.
{: ios}

Algunos metadatos de seguridad se almacenan en la cadena de claves (iOS).
El almacén se cifra con una clave AES (Advanced Encryption Standard) de 256 bits. Todas las claves están reforzadas mediante PBKDF2 (Password-Based Key Derivation Function 2).
{: ios}

Utilice `closeAllCollections` para bloquear el acceso a las recopilaciones hasta que llame a `openCollections` de nuevo. Si interpreta `openCollections` como una función de inicio de sesión, puede interpretar `closeAllCollections` como la correspondiente función de finalización de sesión.
{: ios}

Utilice `changeCurrentPassword` para cambiar la contraseña.
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

Proteja todas las recopilaciones en un almacén pasando un objeto `JSONStoreInitOptions` con una contraseña para la función `openCollections`. Si no se pasa una contraseña, los documentos de todas las recopilaciones en el almacén no se cifran.
{: android}

Algunos metadatos de seguridad se almacenan en las preferencias compartidas (Android).
El almacén se cifra con una clave AES (Advanced Encryption Standard) de 256 bits. Todas las claves están reforzadas mediante PBKDF2 (Password-Based Key Derivation Function 2).
{: android}

Utilice `closeAllCollections` para bloquear el acceso a las recopilaciones hasta que llame a `openCollections` de nuevo. Si interpreta `openCollections` como una función de inicio de sesión, puede interpretar `closeAllCollections` como la correspondiente función de finalización de sesión.
{: android}

Utilice `changeCurrentPassword` para cambiar la contraseña.
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

## Soporte de varios usuarios en JSONStore
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

Es posible crear varios almacenes con varias recopilaciones en una única aplicación de MobileFirst. La función `init` puede tomar un objeto de opciones con un nombre de usuario. Si no se proporciona un nombre de usuario, el predeterminado es *jsonstore*.
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

Es posible crear varios almacenes con varias recopilaciones en una única aplicación de MobileFirst. La función `init` puede tomar un objeto de opciones con un nombre de usuario. Si no se proporciona un nombre de usuario, el predeterminado es *jsonstore*.
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

Es posible crear varios almacenes con varias recopilaciones en una única aplicación de MobileFirst. La función `openCollections` puede tomar un objeto de opciones con un nombre de usuario. Si no se proporciona un nombre de usuario, el predeterminado es *jsonstore*.
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

## Integración del adaptador
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

La integración del adaptador es opcional y proporciona formas de enviar datos desde una recopilación a un adaptador y obtener datos de dicho adaptador para la recopilación.
Puede lograr estos objetivos utilizando `WLResourceRequest` o `jQuery.ajax` si necesita más flexibilidad.
{: cordova}

1. Cree un adaptador con el nombre **JSONStoreAdapter**.
2. Defina sus procedimientos `addPerson`, `getPeople`, `pushPeople`, `removePerson` y `replacePerson`.
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
3. Para cargar datos desde un adaptador utilice `WLResourceRequest`.
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
4. Al llamar a `getPushRequired` se devuelve una matriz de los denominados "documentos sucios", que son documentos con modificaciones locales que no existen en el sistema de fondo. Estos documentos se envían al adaptador cuando se llama a `push`.
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
   Para evitar que JSONStore marque los documentos como "sucios", pase la opción `{markDirty:false}` para `add`, `replace` y `remove`.
   {: tip}
   {: cordova}
5. Utilice también la API `getAllDirty` para recuperar todos los documentos sucios.
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
6. Para hacer push a los cambios para un adaptador, llame a `getAllDirty` para obtener una lista de documentos con modificaciones y, a continuación, utilizar `WLResourceRequest`. Después de enviar los datos y recibir una respuesta satisfactoria asegúrese de llamar a `markClean`.
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
7. Utilice `enhance` para ampliar el núcleo de API para que se adecúe a sus necesidades añadiendo funciones al prototipo de la recopilación. En este ejemplo (el fragmento de código que se indica a continuación) se muestra cómo utilizar `enhance` para añadir la función `getValue` que funciona en la recopilación `keyvalue`. Utiliza key (serie) como su único parámetros y devuelve un resultado individual.
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
8. Consulte el ejemplo de JSONStore para la app Cordova en la sección **Ejemplos**. Este proyecto contiene una aplicación Cordova que utiliza el conjunto de API de JSONStore. Puede descargar el proyecto Maven del adaptador de JavaScript [aquí](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

La integración del adaptador es opcional y proporciona formas de enviar datos desde una recopilación a un adaptador y obtener datos de dicho adaptador para la recopilación.
Puede lograrlo utilizando `WLResourceRequest`.
{: ios}

1. Cree un adaptador con el nombre **People**.
2. Defina sus procedimientos `addPerson`, `getPeople`, `pushPeople`, `removePerson` y `replacePerson`.
3. Para cargar datos desde un adaptador utilice `WLResourceRequest`.
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
4. Al llamar a `allDirty` se devuelve una matriz de los denominados "documentos sucios", que son documentos con modificaciones locales que no existen en el sistema de fondo.
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
   Para evitar que JSONStore marque los documentos como "sucios", pase la opción `{markDirty:false}` para `add`, `replace` y `remove`.
   {: tip}
5. Para hacer push a los cambios para un adaptador, llame a `allDirty` para obtener una lista de documentos con modificaciones y, a continuación, utilizar `WLResourceRequest`. Después de enviar los datos y recibir una respuesta satisfactoria asegúrese de llamar a `markDocumentsClean`.
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
6. Descargue el proyecto de aplicación de iOS Swift nativo desde la sección **Ejemplos**. El proyecto contiene una aplicación iOS Swift nativa que utiliza el conjunto de API de JSONStore. Puede descargar el proyecto Maven del adaptador de JavaScript [aquí](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: ios}

<!--### Android
{: #adapter_integration_android}-->

La integración del adaptador es opcional y proporciona formas de enviar datos desde una recopilación a un adaptador y obtener datos de dicho adaptador para la recopilación.
Puede lograrlo utilizando funciones como, por ejemplo, `WLResourceRequest` o su propia instancia de `HttpClient` si necesita más flexibilidad.
{: android}

1. Cree un adaptador con el nombre **JSONStoreAdapter**.
2. Defina sus procedimientos `addPerson`, `getPeople`, `pushPeople`, `removePerson` y `replacePerson`.
3. Para cargar datos desde un adaptador utilice `WLResourceRequest`.
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
4. Al llamar a `findAllDirtyDocuments` se devuelve una matriz de los denominados "documentos sucios", que son documentos con modificaciones locales que no existen en el sistema de fondo.
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
   Para evitar que JSONStore marque los documentos como "sucios", pase la opción `options.setMarkDirty(false)` para `add`, `replace` y `remove`.
   {: tip}
   {: android}
5. Para hacer push a los cambios para un adaptador, llame a `findAllDirtyDocuments` para obtener una lista de documentos con modificaciones y, a continuación, utilizar `WLResourceRequest`. Después de enviar los datos y recibir una respuesta satisfactoria asegúrese de llamar a `markDocumentsClean`.
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
6. Descargue el proyecto de aplicación Android nativo desde la sección **Ejemplos**. El proyecto contiene una aplicación Android nativa que utiliza el conjunto de API de JSONStore. Puede descargar el proyecto Maven del adaptador de JavaScript [aquí](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: android}
