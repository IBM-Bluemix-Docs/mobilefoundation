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

# 配置離線儲存空間
{: #configure_offline_storage}

Mobile Foundation JSONStore 是選用的用戶端 API，提供輕量型文件導向的儲存空間系統。JSONStore 會啟用 JSON 文件的持續性儲存空間。應用程式中的文件會在 JSONStore 中提供，即使執行應用程式的裝置已離線。例如，裝置中沒有可用的網路連線時，這個持續性且一律可用的儲存空間可協助使用者存取文件。如需 JSONStore 概念和術語的概觀，請參閱[這裡](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore)。

如需進階離線儲存空間配置，請參閱[這裡](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore)。
{: note}

### 配置 Cordova 或 Ionic 應用程式的離線儲存空間
{: #configure_offline_storage_cordova}
{: cordova}

請確定 Mobile Foundation Cordova SDK 已新增至專案。
{: cordova}

請遵循[將 Mobile Foundation SDK 新增至 Cordova 應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) 指導教學。
{: tip}
{: cordova}

#### 將 JSONStore 新增至 Cordova 專案
{: #adding_jsonstore_cordova}
{: cordova}

1. 開啟指令行視窗，並導覽至 Cordova 專案資料夾。
2. 執行下列指令：
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### 起始設定 JSONStore 集合
{: #initialize_jsonstore_cordova}   
{: cordova}

使用 `init`，來啟動一個以上 JSONStore 集合。啟動或佈建集合表示建立包含集合及文件（如果不存在）的持續性儲存空間。如果將密碼傳遞給選項，則會以該密碼加密持續性儲存空間。
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

#### 取得 Cordova JSONStore 集合的存取元
{: #get_jsonstore_cordova}
{: cordova}

使用 `get`，以建立集合的存取元。您必須在呼叫 get 之前呼叫 `init`，否則 `get` 的結果會是*未定義的*。
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

*people* 變數現在可以用來對 *people* 集合執行作業（例如 `add`、`find` 及 `replace`）。
{: cordova}

#### 新增文件至 Cordova 集合
{: #add_jsonstore_cordova}
{: cordova}

使用 `add`，將資料儲存為集合內的文件。
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

#### 尋找 Cordova 集合內的文件
{: #find_jsonstore_cordova}
{: cordova}

* 使用 `find` 可使用查詢來尋找集合內的文件。
* 使用 `findAll` 可擷取集合內的所有文件。
* 使用 `findById` 可依文件唯一 ID 進行搜尋。
{: cordova}

find 的預設行為是進行「模糊」搜尋。
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

#### 取代 Cordova 集合內的文件
{: #replace_jsonstore_cordova}
{: cordova}

使用 `replace` 可修改集合內的文件。您用來執行取代的欄位是 `_id`，這是文件唯一 ID。
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

此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 在集合內。
{: cordova}

#### 從 Cordova 集合移除文件
{: #remove_jsonstore_cordova}
{: cordova}

使用 `remove` 可從集合刪除文件。在您呼叫 push 之前，不會從集合消除文件。
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

#### 移除整個 Cordova 集合
{: #remove_collection_jsonstore_cordova}
{: cordova}

使用 `removeCollection` 可刪除集合內儲存的所有文件。此作業類似於資料庫術語中的捨棄表格。
{: cordova}

#### 破壞 Cordova JSONStore
{: #destroy_jsonstore_cordova}
{: cordova}

使用 `destroy` 可移除下列資料：
* 所有文件
* 所有集合
* 所有儲存庫
* 所有 JSONStore meta 資料及安全構件
{: cordova}

### 配置 iOS 應用程式的離線儲存空間
{: #configure_offline_storage_ios}
{: ios}

請確定 Mobile Foundation Native SDK 已新增至 Xcode 專案。
{: ios}

請遵循[將 Mobile Foundation SDK 新增至 iOS 應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) 指導教學。
{: tip}
{: ios}

#### 將 JSONStore 新增至 iOS 專案
{: #adding_jsonstore_ios}
{: ios}

1. 將下列指令新增至位於 Xcode 專案根目錄的現有 `podfile`。
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. 從指令行中，移至 Xcode 專案根目錄，然後執行下列指令：
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
3. 每當您要使用 JSONStore 時，請務必匯入 JSONStore 標頭：
   **Objective-C**：
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ```
   {: codeblock}
   **Swift：**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   
   {: codeblock}
   {: ios}

#### 開啟 iOS JSONStore 集合
{: #open_ios}
{: ios}

使用 `openCollections` 可開啟一個以上的 JSONStore 集合。
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

#### 取得 iOS JSONStore 集合的存取元
{: #get_jsonstore_ios}
{: ios}

使用 `getCollectionWithName` 可建立集合的存取元。在呼叫 `getCollectionWithName` 之前，您必須先呼叫 `openCollections`。
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

現在，變數 collection 現在可以用來對 `people` 集合執行作業，例如 `add`、`find` 及 `replace`。
{: ios}

#### 新增文件至 iOS 集合
{: #add_jsonstore_ios}
{: ios}

使用 `addData` 可將資料儲存為集合內的文件。
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### 尋找 iOS 集合內的文件
{: #find_jsonstore_ios}
{: ios}

使用 `findWithQueryParts` 可使用查詢來尋找集合內的文件。使用 `findAllWithOptions` 可擷取集合內的所有文件。使用 `findWithIds` 可依文件唯一 ID 進行搜尋。
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### 取代 iOS 集合內的文件
{: #replace_jsonstore_ios}
{: ios}

使用 `replaceDocuments` 可修改集合內的文件。您用來執行取代的欄位是 `_id`，這是文件唯一 ID。
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

此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 在集合內。
{: ios}

#### 從 iOS 集合移除文件
{: #remove_jsonstore_ios}
{: ios}

使用 `removeWithIds` 可從集合刪除文件。在您呼叫 `markDocumentClean` 之前，不會從集合消除文件。
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

#### 移除整個 iOS 集合
{: #remove_collection_jsonstore_ios}
{: ios}

使用 `removeCollection` 可刪除集合內儲存的所有文件。此作業類似於資料庫術語中的捨棄表格。
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

#### 破壞 iOS JSONStore
{: #destroy_jsonstore_ios}
{: ios}

使用 `destroyData`，以移除下列資料：
* 所有文件
* 所有集合
* 所有儲存庫
* 所有 JSONStore meta 資料及安全構件
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

### 配置 Android 應用程式的離線儲存空間
{: #configure_offline_storage_android}
{: android}

請確定 Mobile Foundation Native SDK 已新增至 Android Studio 專案。
{: android}

請遵循[將 Mobile Foundation SDK 新增至 Android 應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) 指導教學。
{: tip}
{: android}

#### 將 JSONStore 新增至 Android 專案
{: #adding_jsonstore_android}
{: android}

1. 從 **Android → Gradle Script** 中，選取 `build.gradle (Module: app)` 檔案。
2. 將下列項目新增至現有的 `dependencies` 區段：
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. 將下列指令新增至 `build.gradle` 檔案的 `DefaultConfig` 區段：
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ```
   {: codeblock}
   {: android}
   新增 `abiFilters` 是為了確保具有 JSONStore 的應用程式將在先前指定的任何架構中執行。這是必要的，因為 JSONStore 相依於只支援這些架構的協力廠商程式庫。
   {: note}
   {: android}

#### 開啟 Android JSONStore 集合
{: #open_android}
{: android}

使用 `openCollections` 可開啟一個以上的 JSONStore 集合。
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

#### 取得 Android JSONStore 集合的存取元
{: #get_jsonstore_android}
{: android}

使用 `getCollectionByName` 可建立集合的存取元。在呼叫 `getCollectionByName` 之前，您必須先呼叫 `openCollections`。
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

現在，變數 collection 現在可以用來對 `people` 集合執行作業，例如 `add`、`find` 及 `replace`。
{: android}

#### 新增文件至 Android 集合
{: #add_jsonstore_android}
{: android}

使用 `addData` 可將資料儲存為集合內的文件。
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

#### 尋找 Android 集合內的文件
{: #find_jsonstore_android}
{: android}

使用 `findDocuments` 可使用查詢來尋找集合內的文件。使用 `findAllDocuments` 可擷取集合內的所有文件。使用 `findDocumentById` 可依文件唯一 ID 進行搜尋。
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

#### 取代 Android 集合內的文件
{: #replace_jsonstore_android}
{: android}

使用 `replaceDocuments` 可修改集合內的文件。您用來執行取代的欄位是 `_id`，這是文件唯一 ID。
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

此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 在集合內。
{: android}

#### 從 Android 集合移除文件
{: #remove_jsonstore_android}
{: android}

使用 `removeDocumentById` 可從集合刪除文件。在您呼叫 `markDocumentClean` 之前，不會從集合消除文件。
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

#### 移除整個 Android 集合
{: #remove_collection_jsonstore_android}
{: android}

使用 `removeCollection` 可刪除集合內儲存的所有文件。此作業類似於資料庫術語中的捨棄表格。
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

#### 破壞 Android JSONStore
{: #destroy_jsonstore_android}
{: android}

使用 `destroy` 可移除下列資料：
* 所有文件
* 所有集合
* 所有儲存庫
* 所有 JSONStore meta 資料及安全構件
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
