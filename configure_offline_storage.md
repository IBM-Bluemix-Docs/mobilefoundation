---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

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


# Configure Offline Storage
{: #configure_offline_storage}

The {{site.data.keyword.mobilefoundation_short}} JSONStore is an optional client-side API that provides a lightweight, document-oriented storage system. JSONStore enables persistent storage of JSON documents. Documents in an application are available in JSONStore even when the device that is running the application is offline. This persistent, always-available storage can be useful to give users access to documents when, for example, there's no network connection available in the device. For an overview of JSONStore concepts and terminology, see [here](/docs/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore).

For advanced offline storage configuration, see [here](/docs/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

## Configure offline storage for Cordova or Ionic apps
{: #configure_offline_storage_cordova}

Make sure the {{site.data.keyword.mobilefoundation_short}} Cordova SDK was added to the project.

Follow the [Adding the Mobile Foundation SDK to Cordova applications](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: external} tutorial.
{: tip}

### Adding JSONStore to your Cordova project
{: #adding_jsonstore_cordova}

1. Open a command line window and navigate to your Cordova project folder.
1. Run the command:

   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}

### Initialize JSONStore collection
{: #initialize_jsonstore_cordova}   

Use `init` to start one or more JSONStore collections.
Starting or provisioning a collection means creating the persistent storage that contains the collection and documents, if it does not exist. If a password is passed to options, the persistent storage is encrypted with the password.

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

### Get an accessor to your Cordova JSONStore collection
{: #get_jsonstore_cordova}

Use `get` to create an accessor to the collection. You must call `init` before you call get otherwise the result of `get` will be *undefined*.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

The variable *people* can now be used to perform operations on the *people* collection such as `add`, `find`, and `replace`.

### Add documents to a Cordova collection
{: #add_jsonstore_cordova}

Use `add` to store data as documents inside a collection.

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

### Find documents inside a Cordova collection
{: #find_jsonstore_cordova}

* Use `find` to locate a document inside a collection by using a query.
* Use `findAll` to retrieve all the documents inside a collection.
* Use `findById` to search by the document unique identifier.

The default behavior for find is to do a "fuzzy" search.

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

### Replace documents inside a Cordova collection
{: #replace_jsonstore_cordova}

Use `replace` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.

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

This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.

### Remove documents from a Cordova collection
{: #remove_jsonstore_cordova}

Use `remove` to delete a document from a collection.
Documents are not erased from the collection until you call push.

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

### Remove an entire Cordova collection
{: #remove_collection_jsonstore_cordova}

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.

### Destroy Cordova JSONStore
{: #destroy_jsonstore_cordova}

Use `destroy` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts

## Configure offline storage for iOS apps
{: #configure_offline_storage_ios}

Make sure that the Mobile Foundation Native SDK was added to the Xcode project.

Follow the [Adding the Mobile Foundation SDK to iOS applications](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/){: external} tutorial.
{: tip}

### Adding JSONStore to your iOS project
{: #adding_jsonstore_ios}

1. Add the following to the existing `podfile`, at the root of the Xcode project.

   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}

1. From command line, go to the root of the Xcode project and run the command:

   ```bash
   pod install
   ```
   {: codeblock}

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

### Open iOS JSONStore collection
{: #open_ios}

Use `openCollections` to open one or more JSONStore collections.

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

### Get an accessor to your iOS JSONStore collection
{: #get_jsonstore_ios}

Use `getCollectionWithName` to create an accessor to the collection. You must call `openCollections` before you call `getCollectionWithName`.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}

The variable collection can now be used to perform operations on the `people` collection such as `add`, `find`, and `replace`.

### Add documents to an iOS collection
{: #add_jsonstore_ios}

Use `addData` to store data as documents inside a collection.

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

### Find documents inside an iOS collection
{: #find_jsonstore_ios}

Use `findWithQueryParts` to locate a document inside a collection by using a query. Use `findAllWithOptions` to retrieve all the documents inside a collection. Use `findWithIds` to search by the document unique identifier.

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

### Replace documents inside an iOS collection
{: #replace_jsonstore_ios}

Use `replaceDocuments` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.

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

This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.

### Remove documents from an iOS collection
{: #remove_jsonstore_ios}

Use `removeWithIds` to delete a document from a collection. Documents aren’t erased from the collection until you call `markDocumentClean`.

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

### Remove an entire iOS collection
{: #remove_collection_jsonstore_ios}

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.

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

### Destroy an iOS JSONStore
{: #destroy_jsonstore_ios}

Use `destroyData` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}

## Configure offline storage for Android apps
{: #configure_offline_storage_android}


Make sure that the Mobile Foundation Native SDK was added to the Android Studio project.


Follow the [Adding the Mobile Foundation SDK to Android applications](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/){: external} tutorial.
{: tip}

### Adding JSONStore to your Android project
{: #adding_jsonstore_android}


1. From **Android → Gradle Scripts**, select the `build.gradle (Module: app)` file.
1. Add the following to the existing `dependencies` section:

   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}

1. Add the following to the `DefaultConfig` section of your `build.gradle` file:

   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ```
   {: codeblock}

   We add the `abiFilters` to ensure that the apps having JSONStore will run in any of the previously specified architectures. This is required as JSONStore is dependent on a third party library which supports only these architectures.
   {: note}


### Open an Android JSONStore collection
{: #open_android}


Use `openCollections` to open one or more JSONStore collections.

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

### Get an accessor to your Android JSONStore collection
{: #get_jsonstore_android}


Use `getCollectionByName` to create an accessor to the collection. You must call `openCollections` before you call `getCollectionByName`.

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

The variable collection can now be used to perform operations on the `people` collection such as `add`, `find`, and `replace`.

### Add documents to an Android collection
{: #add_jsonstore_android}

Use `addData` to store data as documents inside a collection.


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


### Find documents inside an Android collection
{: #find_jsonstore_android}

Use `findDocuments` to locate a document inside a collection by using a query. Use `findAllDocuments` to retrieve all the documents inside a collection. Use `findDocumentById` to search by the document unique identifier..


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


### Replace documents inside an Android collection
{: #replace_jsonstore_android}

Use `replaceDocuments` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.

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

This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.

### Remove documents from an Android collection
{: #remove_jsonstore_android}

Use `removeDocumentById` to delete a document from a collection. Documents aren’t erased from the collection until you call `markDocumentClean`.

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

### Remove an entire Android collection
{: #remove_collection_jsonstore_android}

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.

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

### Destroy an Android JSONStore
{: #destroy_jsonstore_android}

Use `destroy` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts

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
