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

# Configurar el almacenamiento fuera de línea
{: #configure_offline_storage}

JSONStore de Mobile Foundation es una API del lado del cliente opcional que proporciona un sistema ligero de almacenamiento orientado a documentos. JSONStore ofrece un almacenamiento persistente de documentos JSON. Los documentos en una aplicación están disponibles en JSONStore incluso cuando el dispositivo que ejecuta la aplicación está fuera de línea. Este almacenamiento persistente que siempre está disponible puede ser útil para que los usuarios accedan a documentos cuando, por ejemplo, no hay ninguna conexión de red disponible en el dispositivo. Para obtener una visión general de los conceptos y la terminología de JSONStore, consulte [esto](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore).

Para obtener una configuración de almacenamiento fuera de línea, consulte [esto](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

### Configure el almacenamiento fuera de línea para las apps Cordova o Ionic
{: #configure_offline_storage_cordova}
{: cordova}

Asegúrese de que el SDK de Cordova de Mobile Foundation se ha añadido al proyecto.
{: cordova}

Siga la guía de aprendizaje [Adición del SDK de Mobile Foundation a aplicaciones Cordova ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}
{: cordova}

#### Adición de JSONStore a un proyecto Cordova
{: #adding_jsonstore_cordova}
{: cordova}

1. Abra una ventana de línea de mandatos y vaya a la carpeta del proyecto de Cordova.
2. Ejecute el mandato:
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### Inicializar la recopilación de JSONStore
{: #initialize_jsonstore_cordova}   
{: cordova}

Utilice `init` para iniciar una o varias recopilaciones JSONStore.
Iniciar o aprovisionar una recopilación significa crear el almacenamiento persistente que contiene la recopilación y los documentos, si este no existe. Si una contraseña se pasa a las opciones, el almacenamiento persistente se cifra con la contraseña.
{: cordova}

```javascript
var collections = {
    people : {
        searchFields : {name: 'string', age: 'integer'}
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

#### Obtener un descriptor de acceso para la recopilación de Cordova JSONStore
{: #get_jsonstore_cordova}
{: cordova}

Utilice `get` para crear un descriptor de acceso a la recopilación. Debe llamar a `init` antes de llamar a `get`, de lo contrario el resultado de esta llamada será *undefined* (no definido).
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

La variable *people* se puede utilizar ahora para realizar operaciones en la recopilación *people* como, por ejemplo, `add`, `find` y `replace`.
{: cordova}

#### Añadir documentos a una recopilación de Cordova
{: #add_jsonstore_cordova}
{: cordova}

Utilice `add` para almacenar datos como documentos dentro de una recopilación.
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

#### Buscar documentos en una recopilación de Cordova
{: #find_jsonstore_cordova}
{: cordova}

* Utilice `find` para encontrar documentos dentro de una recopilación utilizando una consulta.
* Utilice `findAll` para recuperar todos los documentos dentro de una recopilación.
* Utilice `findById` para buscar documentos según su identificador exclusivo de documento.
{: cordova}

El comportamiento predeterminado para encontrar es realizar una búsqueda "difusa" ("fuzzy").
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

#### Sustituir documentos en una recopilación de Cordova
{: #replace_jsonstore_cordova}
{: cordova}

Utilice `replace` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id`, el identificador exclusivo de documento.
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

En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.
{: cordova}

#### Eliminar documentos de una recopilación de Cordova
{: #remove_jsonstore_cordova}
{: cordova}

Utilice `remove` para suprimir un documento de una recopilación.
Los documentos no se quitan de la recopilación hasta que no llame a push.
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

#### Eliminar una recopilación de Cordova completa
{: #remove_collection_jsonstore_cordova}
{: cordova}

Utilice `removeCollection` para suprimir todos los documentos que se almacenan dentro de una recopilación. Esta operación es similar a descartar una tabla en términos de una base de datos.
{: cordova}

#### Destruir Cordova JSONStore
{: #destroy_jsonstore_cordova}
{: cordova}

Utilice `destroy` para eliminar los siguientes datos:
* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes
* Todos los artefactos de seguridad y metadatos de JSONStore
{: cordova}

### Configurar el almacenamiento fuera de línea para apps de iOS
{: #configure_offline_storage_ios}
{: ios}

Asegúrese de que el SDK nativo de Mobile Foundation se haya añadido al proyecto de Xcode.
{: ios}

Siga la guía de aprendizaje [Adición del SDK de Mobile Foundation a aplicaciones iOS ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}
{: ios}

#### Adición de JSONStore a un proyecto iOS
{: #adding_jsonstore_ios}
{: ios}

1. Añada lo siguiente al `podfile` existente, en la raíz del proyecto Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. Desde línea de mandatos, vaya a la raíz del proyecto Xcode y ejecute el mandato:
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
3. Siempre que desee utilizan JSONStore, asegúrese de importar la cabecera JSONStore:
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

#### Abrir una recopilación de iOS JSONStore
{: #open_ios}
{: ios}

Utilice `openCollections` para abrir una o varias recopilaciones de JSONStore.
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

#### Obtener un descriptor de acceso para la recopilación de iOS JSONStore
{: #get_jsonstore_ios}
{: ios}

Utilice `getCollectionWithName` para crear un descriptor de acceso a la recopilación. Es necesario llamar a `openCollections` antes de llamar a `getCollectionWithName`.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

La variable collection se puede utilizar ahora para realizar operaciones en la recopilación `people` como, por ejemplo, `add`, `find` y `replace`.
{: ios}

#### Añadir documentos a una recopilación de iOS
{: #add_jsonstore_ios}
{: ios}

Utilice `addData` para almacenar datos como documentos dentro de una recopilación.
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

#### Buscar documentos en una recopilación de iOS
{: #find_jsonstore_ios}
{: ios}

Utilice `findWithQueryParts` para encontrar documentos dentro de una recopilación utilizando una consulta. Utilice `findAllWithOptions` para recuperar todos los documentos dentro de una recopilación. Utilice `findWithIds` para buscar documentos según su identificador exclusivo de documento.
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

#### Sustituir documentos en una recopilación de iOS
{: #replace_jsonstore_ios}
{: ios}

Utilice `replaceDocuments` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id`, el identificador exclusivo de documento.
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

En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.
{: ios}

#### Eliminar documentos de una recopilación de iOS
{: #remove_jsonstore_ios}
{: ios}

Utilice `removeWithIds` para suprimir un documento de una recopilación. Los documentos no se quitan de la recopilación hasta que no llame a `markDocumentClean`.
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

#### Eliminar una recopilación de iOS completa
{: #remove_collection_jsonstore_ios}
{: ios}

Utilice `removeCollection` para suprimir todos los documentos que se almacenan dentro de una recopilación. Esta operación es similar a descartar una tabla en términos de una base de datos.
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

#### Destruir un iOS JSONStore
{: #destroy_jsonstore_ios}
{: ios}

Utilice `destroyData` para eliminar los siguientes datos:
* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes
* Todos los artefactos de seguridad y metadatos de JSONStore
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

### Configure el almacenamiento fuera de línea de las apps de Android
{: #configure_offline_storage_android}
{: android}

Asegúrese de que el SDK nativo de Mobile Foundation se haya añadido al proyecto de Android Studio.
{: android}

Siga la guía de aprendizaje [Adición del SDK de Mobile Foundation a aplicaciones Android ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}
{: android}

#### Adición de JSONStore a un proyecto Android
{: #adding_jsonstore_android}
{: android}

1. En **Android → Scripts de Gradle**, seleccione el archivo `build.gradle (Módulo: app)`.
2. Añada lo siguiente a la sección de `dependencies` existente:
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. Añada lo siguiente a la sección `DefaultConfig` del archivo `build.gradle`:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
   ```
   {: codeblock}
   {: android}
   Añadimos `abiFilters` para garantizar que las apps que tengan JSONStore ejecuten en cualquiera de las arquitecturas especificadas anteriormente. Esto es necesario ya que JSONStore depende de una biblioteca tercera que sólo soporte estas arquitecturas.
   {: note}
   {: android}

#### Abrir una recopilación de Android JSONStore
{: #open_android}
{: android}

Utilice `openCollections` para abrir una o varias recopilaciones de JSONStore.
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

#### Obtener un descriptor de acceso para la recopilación de Android JSONStore
{: #get_jsonstore_android}
{: android}

Utilice `getCollectionByName` para crear un descriptor de acceso a la recopilación. Es necesario llamar a `openCollections` antes de llamar a `getCollectionByName`.
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

La variable collection se puede utilizar ahora para realizar operaciones en la recopilación `people` como, por ejemplo, `add`, `find` y `replace`.
{: android}

#### Añadir documentos a una recopilación de Android
{: #add_jsonstore_android}
{: android}

Utilice `addData` para almacenar datos como documentos dentro de una recopilación.
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

#### Buscar documentos en una recopilación de Android
{: #find_jsonstore_android}
{: android}

Utilice `findDocuments` para encontrar documentos dentro de una recopilación utilizando una consulta. Utilice `findAllDocuments` para recuperar todos los documentos dentro de una recopilación. Utilice `findDocumentById` para buscar documentos según su identificador exclusivo de documento.
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

#### Sustituir documentos en una recopilación de Android
{: #replace_jsonstore_android}
{: android}

Utilice `replaceDocuments` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id`, el identificador exclusivo de documento.
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

En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.
{: android}

#### Eliminar documentos de una recopilación de Android
{: #remove_jsonstore_android}
{: android}

Utilice `removeDocumentById` para suprimir un documento de una recopilación. Los documentos no se quitan de la recopilación hasta que no llame a `markDocumentClean`.
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

#### Eliminar una recopilación de Android completa
{: #remove_collection_jsonstore_android}
{: android}

Utilice `removeCollection` para suprimir todos los documentos que se almacenan dentro de una recopilación. Esta operación es similar a descartar una tabla en términos de una base de datos.
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

#### Destruir un Android JSONStore
{: #destroy_jsonstore_android}
{: android}

Utilice `destroy` para eliminar los siguientes datos:
* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes
* Todos los artefactos de seguridad y metadatos de JSONStore
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
