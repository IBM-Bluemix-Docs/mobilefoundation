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

# 進階 JSONStore
{: #advanced_jsonstore}

## JSONStore 中的安全
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

您可以將密碼傳遞至 `init` 函數，來保護儲存庫中所有集合的安全。如果未傳遞密碼，則不會將儲存庫中所有集合的文件加密。
{: cordova}

資料加密僅適用於 Android、iOS、Windows 8.1 Universal 及 Windows 10 UWP 環境。有些安全 meta 資料儲存在金鑰鏈 (iOS)、共用喜好設定 (Android) 或認證保險箱 (Windows 8.1) 中。儲存庫會以 256 位元的「進階加密標準 (AES)」金鑰進行加密。所有金鑰都是使用密碼型金鑰鍵衍生函數 2 (PBKDF2) 來增強。
{: cordova}

使用 `closeAll` 來鎖定所有集合的存取權，直到再次呼叫 `init` 為止。如果您將 `init` 視為登入函數，則可以將 `closeAll` 視為對應的登出函數。請使用 `changePassword` 來變更密碼。
{: cordova}

只有 iOS 才支援加密。依預設，Mobile Foundation Cordova SDK for iOS 依賴 iOS 提供的 API 以進行加密。如果您偏好使用 OpenSSL 來取代：
{: cordova}

* 新增 `cordova-plugin-mfp-encrypt-utils` 外掛程式：
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* 在應用程式邏輯中，使用 `WL.SecurityUtils.enableNativeEncryption(false)` 來啟用 OpenSSL 選項。
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

您可以將 `JSONStoreOpenOptions` 物件與密碼傳遞至 `openCollections` 函數，來保護儲存庫中所有集合的安全。如果未傳遞密碼，則不會將儲存庫中所有集合的文件加密。
{: ios}

有些安全 meta 資料儲存在金鑰鏈 (iOS) 中。儲存庫會以 256 位元的「進階加密標準 (AES)」金鑰進行加密。所有金鑰都是使用密碼型金鑰鍵衍生函數 2 (PBKDF2) 來增強。
{: ios}

使用 `closeAllCollections` 來鎖定所有集合的存取權，直到再次呼叫 `openCollections` 為止。如果您將 `openCollections` 視為登入函數，則可以將 `closeAllCollections` 視為對應的登出函數。
{: ios}

請使用 `changeCurrentPassword` 來變更密碼。
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

您可以將 `JSONStoreInitOptions` 物件與密碼傳遞至 `openCollections` 函數，來保護儲存庫中所有集合的安全。如果未傳遞密碼，則不會將儲存庫中所有集合的文件加密。
{: android}

有些安全 meta 資料儲存在共用喜好設定 (Android) 中。儲存庫會以 256 位元的「進階加密標準 (AES)」金鑰進行加密。所有金鑰都是使用密碼型金鑰鍵衍生函數 2 (PBKDF2) 來增強。
{: android}

使用 `closeAllCollections` 來鎖定所有集合的存取權，直到再次呼叫 `openCollections` 為止。如果您將 `openCollections` 視為登入函數，則可以將 `closeAllCollections` 視為對應的登出函數。
{: android}

請使用 `changeCurrentPassword` 來變更密碼。
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
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
      ```
{: codeblock}
{: android}

## JSONStore 中的多使用者支援
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

您可以在單一 MobileFirst 應用程式中建立多個包含不同集合的儲存庫。`init` 函數可接受具有使用者名稱的 options 物件。如果未提供任何使用者名稱，預設使用者名稱是 *jsonstore*。
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

您可以在單一 MobileFirst 應用程式中建立多個包含不同集合的儲存庫。`init` 函數可接受具有使用者名稱的 options 物件。如果未提供任何使用者名稱，預設使用者名稱是 *jsonstore*。
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

您可以在單一 MobileFirst 應用程式中建立多個包含不同集合的儲存庫。`openCollections` 函數可接受具有使用者名稱的 options 物件。如果未提供任何使用者名稱，預設使用者名稱是 *jsonstore*。
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
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
      ```
{: codeblock}
{: android}

## 配接器整合
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

配接器整合是選用性的，它提供了方法來將資料從集合傳送到配接器，以及將資料從配接器傳送到集合。如果您需要更大的彈性，可以使用 `WLResourceRequest` 或 `jQuery.ajax` 來達成這些目標。
{: cordova}

1. 建立配接器，並將它命名為 **JSONStoreAdapter**。
2. 定義其程序 `addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。
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
3. 若要從配接器載入資料，請使用 `WLResourceRequest`。
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
4. 呼叫 `getPushRequired` 會傳回所謂「只在本端變動過的文件」的陣列，這些文件具有本端修改，而修改並不存在於後端系統上。當呼叫 `push` 時，會將這些文件傳送到配接器。
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
   若要防止 JSONStore 將文件標示為「已變動」，請傳遞選項 `{markDirty:false}` 給 `add`、`replace` 和 `remove`。
   {: tip}
   {: cordova}
5. 您也可以使用 `getAllDirty` API 來擷取變動過的文件。
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
6. 若要將變更推送至配接器，請呼叫 `getAllDirty` 以取得有修改的文件清單，然後使用 `WLResourceRequest`。在傳送資料並收到成功的回應之後，請務必呼叫 `markClean`。
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
7. 使用 `enhance` 可將函數新增至集合原型來延伸核心 API，以符合您的需求。此範例（下列程式碼 Snippet）顯示如何使用 `enhance` 來新增作用於 `keyvalue` 集合的函數 `getValue`。它會接受一個 key（字串）作為其唯一參數，並傳回單一結果。
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
8. 參閱**範例**小節的 Cordova 應用程式 JSONStore 範例。此專案包含使用 JSONStore API 集合的 Cordova 應用程式。JavaScript 配接器 Maven 專案可以從[這裡](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)下載。
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

配接器整合是選用性的，它提供了方法來將資料從集合傳送到配接器，以及將資料從配接器傳送到集合。您可以使用 `WLResourceRequest` 來達成這些目標。
{: ios}

1. 建立配接器，並將它命名為 **People**。
2. 定義其程序 `addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。
3. 若要從配接器載入資料，請使用 `WLResourceRequest`。
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
4. 呼叫 `allDirty` 會傳回所謂「只在本端變動過的文件」的陣列，這些文件具有本端修改，而修改並不存在於後端系統上。
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
   若要防止 JSONStore 將文件標示為「已變動」，請傳遞選項 `{markDirty:false}` 給 `add`、`replace` 和 `remove`。
   {: tip}
5. 若要將變更推送至配接器，請呼叫 `allDirty` 以取得有修改的文件清單，然後使用 `WLResourceRequest`。在傳送資料並收到成功的回應之後，請務必呼叫 `markDocumentsClean`。
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
6. 從**範例**小節下載原生 iOS Swift 應用程式專案。專案包含使用 JSONStore API 集合的原生 iOS Swift 應用程式。JavaScript 配接器 Maven 專案可以從[這裡](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)下載。
{: ios}

<!--### Android
{: #adapter_integration_android}-->

配接器整合是選用性的，它提供了方法來將資料從集合傳送到配接器，以及將資料從配接器傳送到集合。如果您需要更大的彈性，可以使用例如 `WLResourceRequest` 的函數或自己的 `HttpClient` 實例來達成這些目標。
{: android}

1. 建立配接器，並將它命名為 **JSONStoreAdapter**。
2. 定義其程序 `addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。
3. 若要從配接器載入資料，請使用 `WLResourceRequest`。
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
4. 呼叫 `findAllDirtyDocuments` 會傳回所謂「只在本端變動過的文件」的陣列，這些文件具有本端修改，而修改並不存在於後端系統上。
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
   若要防止 JSONStore 將文件標示為「已變動」，請傳遞選項 `options.setMarkDirty(false)` 給 `add`、`replace` 和 `remove`。
   {: tip}
   {: android}
5. 若要將變更推送至配接器，請呼叫 `findAllDirtyDocuments` 以取得有修改的文件清單，然後使用 `WLResourceRequest`。在傳送資料並收到成功的回應之後，請務必呼叫 `markDocumentsClean`。
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
6. 從**範例**小節下載原生 Android 應用程式專案。專案包含使用 JSONStore API 集合的原生 Android 應用程式。JavaScript 配接器 Maven 專案可以從[這裡](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)下載。
{: android}
