---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: JSONStore, offline storage, add jsonstore to cordova, add jsonstore to iOS, add jsonstore to android, jsonstore methods, jsonstore operations

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
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

# Configure Offline Storage
{: #configure_offline_storage}

The {{site.data.keyword.mobilefoundation_short}} JSONStore is an optional client-side API that provides a lightweight, document-oriented storage system. JSONStore enables persistent storage of JSON documents. Documents in an application are available in JSONStore even when the device that is running the application is offline. This persistent, always-available storage can be useful to give users access to documents when, for example, there's no network connection available in the device. For an overview of JSONStore concepts and terminology, see [here](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore).

For advanced offline storage configuration, see [here](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

## Configure offline storage for Cordova or Ionic apps
{: #configure_offline_storage_cordova}
{: cordova}

Make sure the {{site.data.keyword.mobilefoundation_short}} Cordova SDK was added to the project.
{: cordova}

Follow the [Adding the Mobile Foundation SDK to Cordova applications](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: external} tutorial.
{: tip}
{: cordova}

### Adding JSONStore to your Cordova project
{: #adding_jsonstore_cordova}
{: cordova}

1. Open a command line window and navigate to your Cordova project folder.
1. Run the command:

   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

### Initialize JSONStore collection
{: #initialize_jsonstore_cordova}   
{: cordova}

Use `init` to start one or more JSONStore collections.
Starting or provisioning a collection means creating the persistent storage that contains the collection and documents, if it does not exist. If a password is passed to options, the persistent storage is encrypted with the password.
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

### Get an accessor to your Cordova JSONStore collection
{: #get_jsonstore_cordova}
{: cordova}

Use `get` to create an accessor to the collection. You must call `init` before you call get otherwise the result of `get` will be *undefined*.
{: cordova}

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

The variable *people* can now be used to perform operations on the *people* collection such as `add`, `find`, and `replace`.
{: cordova}

### Add documents to a Cordova collection
{: #add_jsonstore_cordova}
{: cordova}

Use `add` to store data as documents inside a collection.
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

### Find documents inside a Cordova collection
{: #find_jsonstore_cordova}
{: cordova}

* Use `find` to locate a document inside a collection by using a query.
* Use `findAll` to retrieve all the documents inside a collection.
* Use `findById` to search by the document unique identifier.
{: cordova}

The default behavior for find is to do a "fuzzy" search.
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

### Replace documents inside a Cordova collection
{: #replace_jsonstore_cordova}
{: cordova}

Use `replace` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.
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

This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.
{: cordova}

### Remove documents from a Cordova collection
{: #remove_jsonstore_cordova}
{: cordova}

Use `remove` to delete a document from a collection.
Documents are not erased from the collection until you call push.
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

### Remove an entire Cordova collection
{: #remove_collection_jsonstore_cordova}
{: cordova}

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.
{: cordova}

### Destroy Cordova JSONStore
{: #destroy_jsonstore_cordova}
{: cordova}

Use `destroy` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts
{: cordova}

## Configure offline storage for iOS apps
{: #configure_offline_storage_ios}
{: ios}

Make sure that the Mobile Foundation Native SDK was added to the Xcode project.
{: ios}

Follow the [Adding the Mobile Foundation SDK to iOS applications](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/){: external} tutorial.
{: tip}
{: ios}

### Adding JSONStore to your iOS project
{: #adding_jsonstore_ios}
{: ios}

1. Add the following to the existing `podfile`, at the root of the Xcode project.

   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}

1. From command line, go to the root of the Xcode project and run the command:

   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}

1. Whenever you want to use JSONStore, make sure that you import the JSONStore header:

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

### Open iOS JSONStore collection
{: #open_ios}
{: ios}

Use `openCollections` to open one or more JSONStore collections.
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

### Get an accessor to your iOS JSONStore collection
{: #get_jsonstore_ios}
{: ios}

Use `getCollectionWithName` to create an accessor to the collection. You must call `openCollections` before you call `getCollectionWithName`.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

The variable collection can now be used to perform operations on the `people` collection such as `add`, `find`, and `replace`.
{: ios}

### Add documents to an iOS collection
{: #add_jsonstore_ios}
{: ios}

Use `addData` to store data as documents inside a collection.
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

### Find documents inside an iOS collection
{: #find_jsonstore_ios}
{: ios}

Use `findWithQueryParts` to locate a document inside a collection by using a query. Use `findAllWithOptions` to retrieve all the documents inside a collection. Use `findWithIds` to search by the document unique identifier.
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

### Replace documents inside an iOS collection
{: #replace_jsonstore_ios}
{: ios}

Use `replaceDocuments` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.
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

This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.
{: ios}

### Remove documents from an iOS collection
{: #remove_jsonstore_ios}
{: ios}

Use `removeWithIds` to delete a document from a collection. Documents aren’t erased from the collection until you call `markDocumentClean`.
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

### Remove an entire iOS collection
{: #remove_collection_jsonstore_ios}
{: ios}

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.
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

### Destroy an iOS JSONStore
{: #destroy_jsonstore_ios}
{: ios}

Use `destroyData` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts
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

## Configure offline storage for Android apps
{: #configure_offline_storage_android}
{: android}

Make sure that the Mobile Foundation Native SDK was added to the Android Studio project.
{: android}

Follow the [Adding the Mobile Foundation SDK to Android applications](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/){: external} tutorial.
{: tip}
{: android}

### Adding JSONStore to your Android project
{: #adding_jsonstore_android}
{: android}

1. From **Android → Gradle Scripts**, select the `build.gradle (Module: app)` file.
1. Add the following to the existing `dependencies` section:

   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}

1. Add the following to the `DefaultConfig` section of your `build.gradle` file:

   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ```
   {: codeblock}
   {: android}

   We add the `abiFilters` to ensure that the apps having JSONStore will run in any of the previously specified architectures. This is required as JSONStore is dependent on a third party library which supports only these architectures.
   {: note}
   {: android}

### Open an Android JSONStore collection
{: #open_android}
{: android}

Use `openCollections` to open one or more JSONStore collections.
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

### Get an accessor to your Android JSONStore collection
{: #get_jsonstore_android}
{: android}

Use `getCollectionByName` to create an accessor to the collection. You must call `openCollections` before you call `getCollectionByName`.
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

The variable collection can now be used to perform operations on the `people` collection such as `add`, `find`, and `replace`.
{: android}

### Add documents to an Android collection
{: #add_jsonstore_android}
{: android}

Use `addData` to store data as documents inside a collection.
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

### Find documents inside an Android collection
{: #find_jsonstore_android}
{: android}

Use `findDocuments` to locate a document inside a collection by using a query. Use `findAllDocuments` to retrieve all the documents inside a collection. Use `findDocumentById` to search by the document unique identifier..
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

### Replace documents inside an Android collection
{: #replace_jsonstore_android}
{: android}

Use `replaceDocuments` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.
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

This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.
{: android}

### Remove documents from an Android collection
{: #remove_jsonstore_android}
{: android}

Use `removeDocumentById` to delete a document from a collection. Documents aren’t erased from the collection until you call `markDocumentClean`.
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

### Remove an entire Android collection
{: #remove_collection_jsonstore_android}
{: android}

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.
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

### Destroy an Android JSONStore
{: #destroy_jsonstore_android}
{: android}

Use `destroy` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts
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
