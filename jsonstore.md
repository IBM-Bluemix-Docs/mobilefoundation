---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: JSONStore, offline storage, jsonstore error codes

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

# JSONStore
{: #jsonstore}

The {{site.data.keyword.mobilefoundation_short}} **JSONStore** is an optional client-side API that provides a lightweight, document-oriented storage system. JSONStore enables persistent storage of **JSON documents**. Documents in an application are available in JSONStore even when the device that is running the application is offline. This persistent, always-available storage can be useful to give users access to documents when, for example, there's no network connection available in the device.
{: shortdesc}

Because it’s familiar to developers, relational database terminology is used in this documentation at times to help explain JSONStore. There are many differences between a relational database and JSONStore however. For example, the strict schema that is used to store data in relational databases is different from JSONStore's approach. With JSONStore, you can store any JSON content, and index the content that you need to search.

## Key features
{: #key-features-jsonstore }

* Data indexing for efficient searching
* Mechanism for tracking local-only changes to the stored data
* Support for many users
* AES 256 encryption of stored data provides security and confidentiality. You can segment protection by user with password-protection, if more than one user on a single device.

A single store can have many collections, and each collection can have many documents. It’s also possible to have a MobileFirst application that consists of multiple stores. For information, see JSONStore multiple user supports.

## Support level
{: #support-level-jsonstore }

* JSONStore is supported in Native iOS and Android applications (no support for native Windows&trade; (Universal and UWP)).
* JSONStore is supported in Cordova iOS, Android, and Windows&trade; (Universal and UWP) applications.

## General JSONStore Terminology
{: #general-jsonstore-terminology }

### Document
{: #document }

A document is the basic building block of JSONStore.

A JSONStore document is a JSON object with an automatically generated identifier (`_id`) and JSON data. It’s similar to a record or a row in database terminology. The value of `_id` is always a unique integer inside a specific collection. Some functions like `add`, `replace`, and `remove` in the `JSONStoreInstance` class take an Array of Documents or Objects. These methods are useful to perform operations on various Documents or Objects at a time.

Single document:

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

Array of documents:

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Collection
{: #collection }

A JSONStore collection is similar to a table, in database terminology.  
The following code example isn't the way that the documents are stored on disk, but it’s a good way to visualize what a collection looks like at a high level.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Store
{: #store }

A store is the persistent JSONStore file that consist of one or more collections.  
A store is similar to a relational database, in database terminology. A store is also referred to as a JSONStore.

### Search fields
{: #search-fields }

A search field is a key-value pair.  
Search fields are keys that are indexed for fast lookup times, similar to column fields or attributes, in database terminology.

Extra search fields are keys that are indexed but that aren’t part of the JSON data that is stored. These fields define the key whose values (in the JSON collection) are indexed and can be used to search more quickly.

Valid data types are: String, Boolean, Number, and Integer. These types are only type hints, there’s no type validation. Furthermore, these types determine how indexable fields are stored. For example, `{age: 'number'}` will index 1 as 1.0 and `{age: 'integer'}` will index 1 as 1.

Search fields and extra search fields:

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

It’s only possible to index keys inside an object, not the object itself. Arrays are handled in a pass-through fashion, which means that you can’t index an array or a specific index of the array (arr[n]), but you can index objects inside an array.

Indexing values inside an array:

```javascript
var searchFields = {
    'people.name' : 'string', // matches carlos and tim on myObject
    'people.age' : 'integer' // matches 99 and 100 on myObject
};

var myObject = {
    people : [
        {name: 'carlos', age: 99},
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### Queries
{: #queries }

Queries are objects that use search fields or extra search fields to look for documents.  
These examples presume that the name search field is of type string and the age search field is of type integer.

Find documents with `name` that matches `carlos`:

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

Find documents with `name` that matches ``carlos`` and `age` matches `99`:

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### Query parts
{: #query-parts }

Query parts are used to build more advanced searches. Some JSONStore operations, such as some versions of `find` or `count` take query parts. Everything within a query part is joined by `AND` statements, while query parts themselves are joined by `OR` statements. The search criteria returns a match only if everything within a query part is **true**. You can use more than one query part to search for matches that satisfy one or more of the query parts.

Find with query parts operate only on top-level search fields. For example: `name`, and not `name.first`. Use multiple collections where all search fields are top-level to get around this behavior. The query parts operations that work with non top-level search fields are: `equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike`, and `notLeftLike`. The behavior is undetermined if you use non top-level search fields.

## Features table
{: #features-table }

Compare JSONStore features to those features of other data storage technologies and formats.

JSONStore is a JavaScript API for storing data inside Cordova applications that use the MobileFirst plug-in, an Objective-C API for native iOS applications, and a Java API for native Android applications. For reference, here is a comparison of different JavaScript storage technologies to see how JSONStore compares to them.

JSONStore is similar to technologies such as LocalStorage, Indexed DB, Cordova Storage API, and Cordova File API. The table shows how some features that are provided by JSONStore compare with other technologies. The JSONStore feature is only available on iOS and Android devices and simulators.

| Feature                                            | JSONStore      | LocalStorage | IndexedDB | Cordova storage API | Cordova file API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Android Support (Cordova &amp; Native Applications)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| iOS Support (Cordova & Native Applications)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal and Windows 10 UWP (Cordova Applications)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| Data encryption	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Maximum Storage	                                 |Available Space |    ~5 MB     |   ~5 MB 	 | Available Space	   | Available Space  |
| Reliable Storage (See note)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| Keep Track of Local Changes	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Multi-user support                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Indexing	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| Type of Storage	                                 | JSON documents | Key-value pairs | JSON documents | Relational (SQL) | Strings     |
{: caption="Table 1. Feature comparison" caption-side="top"}

Reliable Storage means that your data isn’t deleted unless one of the following events occurs:
* The application is removed from the device.
* One of the methods that removes data is called.
{: note}

## Multiple User Support
{: #multiple-user-support }

With JSONStore, you can create many stores that consist of different collections in a single MobileFirst application.

The init (JavaScript) or open (Native iOS and Native Android) API can take an options object with a user name. Different stores are separate files in the file system. The user name is used as the file name of the store. These separate stores can be encrypted with different passwords for security and privacy reasons. Calling the closeAll API removes access to all the collections. It's also possible to change the password of an encrypted store by calling the changePassword API.

An example use case would be various employees that share a physical device (for example an iPad or Android tablet) and a MobileFirst application. Multiple user support is useful when the employees work different shifts and handle private data from different customers, while they use the MobileFirst application.

## Security
{: #security-jsonstore }

You can secure all of the collections in a store by encrypting them.

To encrypt all of the collections in a store, pass a password to the `init` (JavaScript) or `open` (Native iOS and Native Android) API. If no password is passed, none of the documents in the store collections are encrypted.

Some security artifacts (for example salt) are stored in the keychain (iOS), shared preferences (Android) and the credential locker (Windows Universal 8.1 and Windows 10 UWP). The store is encrypted with a 256-bit Advanced Encryption Standard (AES) key. All keys are strengthened with Password-Based Key Derivation Function 2 (PBKDF2). You can choose to encrypt data collections for an application, but you can't switch between encrypted and plain-text formats, or to mix formats within a store.

The key that protects the data in the store is based on the user password that you provide. The key doesn’t expire, but you can change it by calling the changePassword API.

The data protection key (DPK) is the key that is used to decrypt the contents of the store. The DPK is kept in the iOS keychain even if the application is uninstalled. To remove both the key in the keychain and everything else that JSONStore puts in the application, use the destroy API. This process isn't applicable to Android because the encrypted DPK is stored in shared preferences and wiped out when the application is uninstalled.

The first time that JSONStore opens a collection with a password, which means that the developer wants to encrypt data inside the store, JSONStore needs a random token. That random token can be got from the client or from the server.

When the localKeyGen key is present in the JavaScript implementation of the JSONStore API, and it has a value of true, a cryptographically secure token is generated locally. Otherwise, the token is generated by contacting the server, requiring connectivity to the MobileFirst Server. This token is required only the first time that a store is opened with a password. The native implementations (Objective-C and Java) generate a cryptographically secure token locally by default, or you can pass one through the secureRandom option.

The tradeoff is between the following:
* opening a store offline and trusting the client to generate that random token (less secure) or
* opening the store with access to the MobileFirst Server (requires connectivity) and trusting the server (more secure)

### Security Utilities
{: #security-utilities }

The MobileFirst client-side API provides some security utilities to help protect your user's data. Features like JSONStore are great if you want to protect JSON objects. However, it’s not suggested to store binary blobs in a JSONStore collection.

Instead, store binary data on the file system, and store the file paths and other metadata inside a JSONStore collection. If you want to protect files like images, you can encode them as base64 strings, encrypt it, and write the output to disk.

To decrypt the data, you can look up the metadata in a JSONStore collection, read the encrypted data from the disk, and decrypt it using the metadata that was stored.

This metadata can include the key, salt, Initialization Vector (IV), type of file, path to the file, and others.

Learn more about [JSONStore Security Utilities](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities).
{: tip}

### Windows 8.1 Universal and Windows 10 UWP encryption
{: #windows-81-universal-and-windows-10-uwp-encryption }

You can secure all of the collections in a store by encrypting them.

JSONStore uses [SQLCipher](http://sqlcipher.net/){: external} as its underlying database technology. SQLCipher is a build of SQLite that is produced by Zetetic, LLC adds a layer of encryption to the database.

JSONStore uses SQLCipher on all platforms. On Android and iOS a free, open source version of SQLCipher is available, known as the Community Edition, which is incorporated into the versions of JSONStore that is included in Mobile Foundation. The Windows versions of SQLCipher are only available under a commercial license and can’t be directly redistributed by Mobile Foundation.

Instead, JSONStore for Windows 8 Universal include SQLite as the underlying database. To encrypt data for either of these platforms, you need to acquire your own version of SQLCipher and swap out the SQLite version that is included in Mobile Foundation.

If you don’t need encryption, the JSONStore is fully functional (minus encryption) by using the SQLite version in Mobile Foundation.

#### Replacing SQLite with SQLCipher for Windows Universal and Windows UWP
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }

1. Run the SQLCipher for Windows Runtime 8.1/10 extension that comes with the SQLCipher for Windows Runtime Commercial Edition.
2. After the extension finishes installing, locate the SQLCipher version of the **sqlite3.dll** file that was just created. There’s one for x86, one for x64, and one for ARM.

   ```bash
   C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
   ```
   {: codeblock}

3. Copy and replace this file to your MobileFirst application.

   ```bash
   <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
   ```
   {: codeblock}

## Performance
{: #performance-jsonstore }

The following are factors that can affect JSONStore performance.

### Network
{: #network-jsonstore }

* Check network connectivity before you perform operations, such as sending all dirty documents to an adapter.
* The amount of data that is sent over the network to a client heavily affects performance. Send only the data that is required by the application, instead of copying everything inside your backend database.
* If you’re using an adapter, consider setting the compressResponse flag to true. That way, responses are compressed, which generally uses less bandwidth and has a faster transfer time than without compression.

### Memory
{: #memory-jsonstore }

* When you use the JavaScript API, JSONStore documents are serialized and deserialized as Strings between the Native (Objective-C, Java, or C#) Layer and the JavaScript Layer. One way to mitigate possible memory issues is by using limit and offset when you use the find API. That way, you limit the amount of memory that is allocated for the results and can implement things like pagination (show X number of results per page).
* Instead of using long key names that are eventually serialized and deserialized as Strings, consider mapping those long key names into smaller ones (for example: `myVeryVeryVerLongKeyName` to `k` or `key`). Ideally, you map them to short key names when you send them from the adapter to the client, and map them to the original long key names when you send data back to the backend.
* Consider splitting the data inside a store into various collections. Have small documents over various collections instead of monolithic documents in a single collection. This consideration depends on how closely related the data is and the use cases for said data.
* When you use the add API with an array of objects, it’s possible to run into memory issues. To mitigate this issue, call these methods with fewer JSON objects at a time.
* JavaScript and Java™ have garbage collectors, while Objective-C has Automatic Reference Counting. Allow it to work, but don’t depend on it entirely. Try to null references that are no longer used and use profiling tools to check that memory usage is going down when you expect it to go down.

### CPU
{: #cpu-jsonstore }

* The amount of search fields and extra search fields that are used affect performance when you call the add method, which does the indexing. Only index the values that are used in queries for the find method.
* By default, JSONStore tracks local changes to its documents. This behavior can be disabled, hence saving a few cycles, by setting the `markDirty` flag to **false** when you use the add, remove, and replace APIs.
* Enabling security adds some overhead to the `init` or `open` APIs and other operations that work with documents inside the collection. Consider whether security is really required. For example, the open API is much slower with encryption because it must generate the encryption keys that are used for encryption and decryption.
* The `replace` and `remove` APIs depend on the collection size as they must go through the whole collection to replace or remove all occurrences. Because it must go through each record, it must decrypt every one of them, which makes it much slower when encryption is used. This performance hit is more noticeable on large collections.
* The `count` API is relatively expensive. However, you can keep a variable that keeps the count for that collection. Update it every time that you store or remove things from the collection.
* The `find` APIs (`find`, `findAll`, and `findById`) are affected by encryption, since they must decrypt every document to see whether it’s a match or not. For find by query, if a limit is passed, it’s potentially faster as it stops when it reaches the limit of results. JSONStore doesn’t need to decrypt the rest of the documents to figure out if any other search results remain.

## Concurrency
{: #concurrency-jsonstore }

### Concurrency in JavaScript
{: #javascript-jsonstore }

Most of the operations that can be performed on a collection, such as add and find, are asynchronous. These operations return a jQuery promise that is resolved when the operation completes successfully and rejected when a failure occurs. These promises are similar to success and failure callbacks.

A jQuery Deferred is a promise that can be resolved or rejected. The following examples aren’t specific to JSONStore, but are intended to help you understand their usage in general.

Instead of promises and callbacks, you can also listen to JSONStore `success` and `failure` events. Perform actions that are based on the arguments that are passed to the event listener.

Example promise definition:

```javascript
var asyncOperation = function () {
  // Assumes that you have jQuery defined via $ in the environment
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```
{: codeblock}

Example promise usage:

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Example callback definition:

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

Example callback usage:

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Example events:

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```
{: codeblock}

### Concurrency in Objective-C
{: #objective-c-jsonstore }

When you use the Native iOS API for JSONStore, all operations are added to a synchronous dispatch queue. This behavior ensures that operations that touch the store are run in order on a thread that isn't the main thread. For more information, see the Apple documentation at [Grand Central Dispatch (GCD)](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: external}.

### Concurrency in Java
{: #java-jsonstore }

When you use the Native Android API for JSONStore, all operations are run on the main thread. You must create threads or use thread pools to have asynchronous behavior. All store operations are thread-safe.

## Analytics
{: #analytics-jsonstore }

You can collect key pieces of analytics information that are related to JSONStore

### File information
{: #file-information }

If the JSONStore API is called with the analytics flag set to **true**, then the file information is collected once per application session. An application session is defined as loading the application into memory and removing it from memory. You can use this information to determine how much space is being used by JSONStore content in the application.

### Performance metrics
{: #performance-metrics }

Performance metrics are collected every time a JSONStore API is called with information about the start and end times of an operation. You can use this information to determine how much time various operations take in milliseconds.

### JSONStore example for iOS
{: #ios-example}

```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

### JSONStore example for Android
{: #android-example }

```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

### JSONStore example for JavaScript
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## Working With External Data
{: #working-with-external-data }
You can work with external data in several different concepts: **Pull** and **Push**.

### Pull
{: #pull }
Many systems use the term pull to refer to getting data from an external source.  
There are three important pieces:

#### Pull from external data source
{: #external-data-source-pull }
This source can be a database, a REST or SOAP API, or many others. The only requirement is that it must be accessible from either the MobileFirst Server or directly from the client application. Ideally, you want this source to return data in JSON format.

#### Transport Layer for Pull
{: #transport-layer-pull }
This source is how you get data from the external source into your internal source, a JSONStore collection inside the store. One alternative is an adapter.

#### Internal data source API for Pull
{: #internal-data-source-api-pull }
This source is the JSONStore APIs that you can use to add JSON data to a collection.

You can populate the internal store with data that is read from a file, an input field, or hardcoded data in a variable. It doesn’t have to come exclusively from an external source that requires network communication.
{: note}

All of the following code examples are written in pseudocode that looks similar to JavaScript.

Use  adapters for the Transport Layer. Some of the advantages of using adapters are XML to JSON, security, filtering, and decoupling of server-side code and client-side code.
{: note}

##### External data source: Backend REST endpoint
{: #external-data-source-backend-rest-endpoint-head}
Imagine that you have a REST endpoint that read data from a database and returns it as an array of JSON objects.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

The data that is returned can look like the following example:

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

##### Transport Layer: adapter
{: #transport-layer-adapter}
Imagine that you created an adapter that is called people and you defined a procedure that is called getPeople. The procedure calls the REST endpoint and returns the array of JSON objects to the client. You might want to do more work here, for example, return only a subset of the data to the client.

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

On the client, you can use the WLResourceRequest API to get the data. Additionally, you might want to pass some parameters from the client to the adapter. One example is a date with the last time that the client got new data from the external source through the adapter.

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

You might want to take advantage of the `compressResponse`, `timeout`, and other parameters that can be passed to the `WLResourceRequest` API.  
{: note}

Optionally, you can skip the adapter and use something like jQuery.ajax to directly contact the REST endpoint with the data that you want to store.

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### Internal data source API: JSONStore
{: #internal-data-source-api-jsonstore}
After you get the response from the backend, work with that data by using JSONStore.
JSONStore provides a way to track local changes. It enables some APIs to mark documents as dirty. The API records the last operation that was performed on the document, and when the document was marked as dirty. You can then use this information to implement features like data synchronization.

The change API takes the data and some options:

###### replaceCriteria
{: #replacecriteria}
  
These search fields are part of the input data. They're used to locate documents that are already inside a collection. For example, if you select:

```javascript
['id', 'ssn']
```
{: codeblock}

As the replace criteria, pass the following array as the input data:

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

And the `people` collection already consists of the following document:

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

The `change` operation locates a document that matches exactly the following query:

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

Then, the `change` operation performs a replacement with the input data and the collection consists of:

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

The name was changed from `Carlitos` to `Carlos`. If more than one document matches the replace criteria, then all documents that match are replaced with the respective input data.

###### addNew 
{: #addnew}
 
When no documents match the replace criteria, the change API looks at the value of this flag. If the flag is set to **true**, the change API creates a new document and adds it to the store. Otherwise, no further action is taken.

###### markDirty 
{: #markdirty}
 
Determines whether the change API marks documents that are replaced or added as dirty.

An array of data is returned from the adapter:

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

You can use other APIs to track changes to the local documents that are stored. Always get an accessor to the collection that you perform operations on.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

Then, you can add data (array of JSON objects) and decide whether you want it to be marked dirty or not. Typically, you want to set the markDirty flag to false when you get changes from the external source. Set the flag to true when you add data locally.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

You can also replace a document, and opt to mark the document with the replacements as dirty or not.

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

Similarly, you can remove a document, and opt to mark the removal as dirty or not. Documents that are removed and marked dirty don’t show up when you use the find API. However, they're still inside the collection until you use the `markClean` API, which physically removes the documents from the collection. If the document isn’t marked as dirty, it’s physically removed from the collection.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### Push
{: #push }
Many systems use the term push to refer to sending data to an external source.

There are three important pieces:

#### Internal data source API for Push
{: #internal-data-source-api-push }
This source is the JSONStore API that returns documents with local-only changes (dirty).

#### Transport Layer for Push
{: #transport-layer-push }
This source is how you want to contact the external data source to send the changes.

#### Push to external data source
{: #external-data-source-push }
This source is typically a database, REST or SOAP endpoint, among others, that receives the updates that the client made to the data.

All of the following code examples are written in pseudocode that looks similar to JavaScript.

Use adapters for the Transport Layer. Some of the advantages of using adapters are XML to JSON, security, filtering, and decoupling of server-side code and client-side code.
{: note}

##### Internal data source API: JSONStore  
{: #internal-data-source-api-jsonstore-head}
After you get an accessor to the collection, call the `getAllDirty` API to get all documents that are marked as dirty. These documents have local-only changes that you want to send to the external data source through a transport layer.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

The `dirtyDocs` argument looks like the following example:

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

The fields are:
* `_id`: Internal field that JSONStore uses. Every document is assigned a unique one.
* `json`: The data that was stored.
* `_operation`: The last operation that was performed on the document. Possible values are add, store, replace, and remove.
* `_dirty`: A time stamp that is stored as a number to signify when the document was marked dirty.

##### Transport Layer: MobileFirst adapter  
{: #transport-layr-mobilefirst-adapter }
You can choose to send dirty documents to an adapter. Assume that you have a `people` adapter that is defined with an `updatePeople` procedure.

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

You might want to take advantage of the `compressResponse`, `timeout`, and other parameters that can be passed to the `WLResourceRequest` API.
{: note}

On the MobileFirst Server, the adapter has the `updatePeople` procedure, which might look like the following example:

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Instead of relaying the output from the `getAllDirty` API on the client, you might have to update the payload to match a format that is expected by the backend. You might have to split the replacements, removals, and inclusions into separate backend API calls.

Optionally, you can iterate over the `dirtyDocs` array and check the `_operation` field. Then, send replacements to one procedure, removals to another procedure, and inclusions to another procedure. The previous example sends all dirty documents in bulk to the adapter.

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

Optionally, you can skip the adapter and contact the REST endpoint directly.

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### External data source: Backend REST endpoint
{: #external-data-source-backend-rest-endpoint }
The backend accepts or rejects changes, and then relays a response back to the client. After the client looks at the response, it can pass documents that were updated to the markClean API.

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

After documents are marked as clean, they don’t show up in the output from the `getAllDirty` API.

## Troubleshooting JSONStore
{: #troubleshooting-jsonstore }

## Provide information when you ask for help
{: #provide-information-when-you-ask-for-help }
It is better to provide more information than to risk not providing enough information. The following list is a good starting point for the information that is required to help with JSONStore issues.

* Operating system and version. For example, Windows XP SP3 Virtual Machine or Mac OSX 10.8.3.
* Eclipse version. For example, Eclipse Indigo 3.7 Java EE.
* JDK version. For example, Java SE Runtime Environment (build 1.7).
* {{ site.data.keys.product }} version. For example, IBM Worklight V5.0.6 Developer Edition.
* iOS version. For example, iOS Simulator 6.1 or iPhone 4S iOS 6.0 (deprecated, see Deprecated features and API elements).
* Android version. For example, Android Emulator 4.1.1 or Samsung Galaxy Android 4.0 API Level 14.
* Windows version. For example, Windows 8, Windows 8.1, or Windows Phone 8.1.
* adb version. For example, Android Debug Bridge version 1.0.31.
* Logs, such as Xcode output on iOS or logcat output on Android.

## Try to isolate the issue
{: #try-to-isolate-the-issue }
Follow these steps to isolate the issue to more accurately report a problem.

1. Reset the emulator (Android) or simulator (iOS) and call the destroy API to start with a clean system.
2. Ensure that you are running on a supported production environment.
    * Android >= 2.3 ARM v7/ARM v8/x86 emulator or device
    * iOS >= 6.0 simulator or device (deprecated)
    * Windows 8.1/10 ARM/x86/x64 simulator or device
3. Try to turn off encryption by not passing a password to the init or open APIs.
4. Look at the SQLite database file that is generated by JSONStore. Encryption must be turned off.

   * Android emulator:

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * iOS Simulator:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Windows 8.1 Universal / Windows 10 UWP simulator

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

    JavaScript only implementation that runs on a web browser (Firefox, Chrome, Safari, Internet Explorer) does not use an SQLite database. The file is stores in HTML5 LocalStorage.
    {: note}
   * Look at the `searchFields` with `.schema` and select data with `SELECT * FROM <collection-name>;`. To exit sqlite3, type `.exit`. If you pass a user name to the init method, the file is called **the-username.sqlite**. If you do not pass a user name, the file is called **jsonstore.sqlite** by default.
5. (Android only) Enable verbose JSONStore.

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```
    {: codeblock}

6. Use the debugger.

## Common issues
{: #common-issues-jsonstore }
Understanding the following JSONStore characteristics can help resolve some of the common issues that you might encounter.  

* The only way to store binary data in JSONStore is to first encode it in base64. Store file names or paths instead of the actual files in JSONStore.
* Accessing JSONStore data from native code is possible only in {{ site.data.keys.v62_product_full }} V6.2.0.
* There is no limit on how much data you can store inside JSONStore, beyond limits that are imposed by the mobile operating system.
* JSONStore provides persistent data storage. It is not only stored in memory.
* The init API fails when the collection name starts with a digit or symbol. IBM Worklight V5.0.6.1 and later returns an appropriate error: `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* There is a difference between a number and an integer in search fields. Numeric values like `1` and `2` are stored as `1.0` and `2.0` when the type is `number`. They are stored as `1` and `2` when the type is `integer`.
* If an application is forced to stop or crashes, it always fails with error code -1 when the application is started again and the `init` or `open` API is called. If this problem happens, call the `closeAll` API first.
* The JavaScript implementation of JSONStore expects code to be called serially. Wait for an operation to finish before you call the next one.
* Transactions are not supported in Android 2.3.x for Cordova applications.
* When you use JSONStore on a 64-bit device, you might see the following error: `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* This error means that you have 64-bit native libraries in your Android project, and JSONStore does not currently work when you use these libraries. To confirm, go to **src/main/libs** or **src/main/jniLibs** under your Android project, and check whether you have the x86_64 or arm64-v8a folders. If you do, delete these folders, and JSONStore can work again.
* In some cases (or environments), the flow enters `wlCommonInit()`  before JSONStore plugin is initialized. This causes JSONStore related API calls to fail. The `cordova-plugin-mfp` bootstrap calls `WL.Client.init` automatically which triggers the `wlCommonInit` function when it is completed. This initialization process is different for the JSONStore plugin. The JSONStore plugin does not have a way to _halt_ the `WL.Client.init` call. Across different environments, it may happen that the flow enters `wlCommonInit()` before `mfpjsonjslloaded` completes.
To ensure the ordering of `mfpjsonjsloaded` and `mfpjsloaded` events, the developer has the option of calling `WL.CLient.init`
manually. This will remove the need for having platform specific code.

  Use the following steps to configure calling of `WL.CLient.init` manually:                             

  1. In `config.xml`, change the `clientCustomInit` property to **true**.

  + In `index.js` file:                                   
    * add the following line at the beginning of the file:                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * leave the `WL.JSONStore.init` call in `wlCommonInit()`                    

    * add the following function:  
    ```javascript                                         
    function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }
    ```                                                                     

This will wait for the `mfpjsonjsloaded` event (outside of `wlCommonInit`), this will ensure that the script has been loaded and will subsequently call `WL.Client.init`, which will trigger `wlCommonInit`, this will then call `WL.JSONStore.init`.

## Store internals
{: #store-internals }
See an example of how JSONStore data is stored.

The key elements in this simplified example:

* `_id` is the unique identifier (for example, AUTO INCREMENT PRIMARY KEY).
* `json` contains an exact representation of the JSON object that is stored.
* `name` and age are search fields.
* `key` is an extra search field.

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |
{: caption="Table 2. JSONStore data key elements example" caption-side="top"}

When you search by using one of the following queries or a combination of them: `{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}`, the returned document is `{_id: 1, json: {name: 'carlos', age: 99} }`.

The other internal JSONStore fields are:

* `_dirty`: Determines whether the document was marked as dirty or not. This field is useful to track changes to the documents.
* `_deleted`: Marks a document as deleted or not. This field is useful to remove objects from the collection, to later use them to track changes with your backend and decide whether to remove them or not.
* `_operation`: A string that reflects the last operation to be performed on the document (for example, replace).

## JSONStore errors
{: #jsonstore-errors }
### JavaScript errors
{: #javascript-errors }
JSONStore uses an error object to return messages about the cause of failures.

When an error occurs during a JSONStore operation (for example the `find`, and `add` methods in the `JSONStoreInstance` class) an error object is returned. It provides information about the cause of the failure.

```javascript
var errorObject = {
  src: 'find', // Operation that failed.
  err: -50, // Error code.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Error message.
  col: 'people', // Collection name.
  usr: 'jsonstore', // User name.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Document that is related to the failure.
  res: {...} // Response from the server.
}
```
{: codeblock}
Not all the key/value pairs are part of every error object. For example, the doc value is only available when the operation failed because of a document (for example the `remove` method in the `JSONStoreInstance` class) failed to remove a document.

### Objective-C errors
{: #objective-c-errors }
All of the APIs that might fail take an error parameter that takes an address to an NSError object. If you don not want to be notified of errors, you can pass in `nil`. When an operation fails, the address is populated with an NSError, which has an error and some potential `userInfo`. The `userInfo` might contain extra details (for example, the document that caused the failure).

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java&trade; errors
{: #java-errors }
All of the Java&trade; API calls throw a certain exception, depending on the error that happened. You can either handle each exception separately, or you can catch `JSONStoreException` as an umbrella for all JSONStore exceptions.

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### List of error codes
{: #list-of-error-codes }
List of common error codes and their description:

|Error Code      | Description |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Unrecognized error. |
| -75 OS\_SECURITY\_FAILURE | This error code is related to the requireOperatingSystemSecurity flag. It can occur if the destroy API fails to remove security metadata that is protected by operating system security (Touch ID with passcode fallback), or the init or open APIs are unable to locate the security metadata. It can also fail if the device does not support operating system security, but operating system security usage was requested. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore is closed. Try calling the open method in the JSONStore class first to enable access to the store. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | There was a problem with rolling back the transaction. |
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |Cannot call removeCollection while a transaction is in progress. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | Cannot call destroy while there are transactions in progress. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | Cannot call closeAll while there are transactions in place. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | Cannot initialize a store while there are transactions in progress. |
| -43 TRANSACTION_FAILURE | There was a problem with transactions. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | Cannot commit to rolled back a transaction when there is no transaction is progress |
| -41 TRANSACTION\_IN\_POGRESS | Cannot start a new transaction while another transaction is in progress. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Something is wrong with FIPS. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Problem getting the file information from the file system. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Problem replacing documents from a collection. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Problem removing documents from a collection. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Problem storing the Data Protection Key (DPK). |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Problem indexing input data. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Check that the types that you are passing to the searchFields are string, integer, number, or boolean. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | An operation on an array of documents, for example the replace method can fail while it works with a specific document. The document that failed is returned and the transaction is rolled back. On Android, this error also occurs when trying to use JSONStore on unsupported architectures. |
| -10 ACCEPT\_CONDITION\_FAILED | The accept function that the user provided returned false. |
| -9 OFFSET\_WITHOUT\_LIMIT | To use offset, you must also specify a limit. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Validation error, must be a positive integer. |
| -7 INVALID_USERNAME | Validation error (Must be [A-Z] or [a-z] or [0-9] only). |
| -6 USERNAME\_MISMATCH\_DETECTED | To log out, a JSONStore user must call the closeAll method first. There can be only one user at a time. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |A problem with the destroy method while it tried to delete the file that holds the contents of the store. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Problem with the destroy method while it tried to clear the keychain (iOS) or shared user preferences (Android). |
| -3 INVALID\_KEY\_ON\_PROVISION | Passed the wrong password to an encrypted store. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | Search fields are not dynamic. It is not possible to change search fields without calling the destroy method or the removeCollection method before you call the init or openmethod with the new search fields. This error can occur if you change the name or type of the search field. For example: `{key: 'string'}` to `{key: 'number'}` or `{myKey: 'string'}` to `{theKey: 'string'}`. |
| -1 PERSISTENT\_STORE\_FAILURE | Generic Error. A malfunction in native code, most likely calling the init method. |
| 0 SUCCESS | In some cases, JSONStore native code returns 0 to indicate success. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | Validation error. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | Validation error. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | Validation error. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | Validation error. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | Validation error. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | Validation error. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | Validation error. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |The query that selects all documents that are marked dirty failed. An example in SQL of the query would be: SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | To use functions like the push and load methods in the JSONStoreCollection class, an adapter must be passed to the init method. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | Validation error |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | Validation error |
| 12 ADAPTER_FAILURE | Problem calling WL.Client.invokeProcedure, specifically a problem in connecting to the adapter. This error is different from a failure in the adapter that tries to call a backend. |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | Validation error |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | Calling the enhance method in the JSONStoreCollection class to replace an existing function (find and add) is not allowed. |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Push sends the document to an adapter but JSONStore fails to mark the document as not dirty. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | To initiate a collection with a password there must be connectivity to the {{ site.data.keys.mf_server }} because it returns a 'secure random token'. IBM Worklight V5.0.6 and later allows developers to generate the secure random token locally passing {localKeyGen: true} to the init method via the options object. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | Could not load data because WL.Client.invokeProcedure called the failure callback. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | The load object that was passed to the init method did not pass the validation. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | There is a problem with the key that is used in the load object when you call the add method. |
| 20 UNDEFINED\_PUSH\_OPERATION | No procedure is defined for pushing dirty documents to the server. For example: the init method (new document is dirty, `operation = 'add'`) and the push method (finds the new document with `operation = 'add'`) were called, but no add key with the add procedure was found in the adapter that is linked to the collection. Linking an adapter is done inside the init method. |
| 21 INVALID\_ADD\_INDEX\_KEY | Problem with extra search fields. |
| 22 INVALID\_SEARCH\_FIELD | One of your search fields is invalid. Verify that none of the search fields that are passed in are _id,json,_deleted, or _operation. |
| 23 ERROR\_CLOSING\_ALL | Generic Error. An error occurred when native code called the closeAll method. |
| 24 ERROR\_CHANGING\_PASSWORD | Unable to change the password. The old password passed was wrong, for example. |
| 25 ERROR\_DURING\_DESTROY | Generic Error. An error occurred when native code called the destroy method. |
| 26 ERROR\_CLEARING\_COLLECTION | Generic Error. An error occurred in when native code called the removeCollection method. |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | Validation error. |
| 28 INVALID\_SORT\_OBJECT | The provided array for sorting is invalid because one of the JSON objects is invalid. The correct syntax is an array of JSON objects, where each object contains only a single property. This property searches the field with which to sort, and whether it is ascending or descending. For example: {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | The provided array for filtering the results is invalid. The correct syntax for this array is an array of strings, in which each string is either a search field or an internal JSONStore field. For more information, see Store internals. |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | Validation error when the array is not an array of only JSON objects. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | Validation error. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | Validation error. |
{: caption="Table 3. Common error codes" caption-side="top"}
