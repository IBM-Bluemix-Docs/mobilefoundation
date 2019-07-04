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

# 高级 JSONStore
{: #advanced_jsonstore}

## JSONStore 中的安全性
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

您可以通过将密码传递到 `init` 函数来保护存储器中的所有集合。如果未传递密码，那么不会加密存储器中所有集合的文档。
{: cordova}

数据加密仅适用于 Android、iOS、Windows 8.1 Universal 和 Windows 10 UWP 环境。某些安全元数据存储在密钥链 (iOS)、共享首选项 (Android) 或凭据保险箱 (Windows8.1) 中。存储器利用 256 位高级加密标准 (AES) 密钥进行加密。所有密钥通过基于密码的密钥派生函数 2 (PBKDF2) 进行增强。
{: cordova}

使用 `closeAll` 可锁定对所有集合的访问，直至再次调用 `init`。如果将 `init` 当作登录函数，那么可将 `closeAll` 当作对应的注销函数。使用 `changePassword` 可更改密码。
{: cordova}

仅在 iOS 中支持加密。缺省情况下，Mobile Foundation Cordova SDK for iOS 依赖于 iOS 提供的 API 进行加密。如果想要将此项替换为 OpenSSL，请执行以下操作：
{: cordova}

* 添加 `cordova-plugin-mfp-encrypt-utils` 插件：
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* 在应用程序逻辑中，使用 `WL.SecurityUtils.enableNativeEncryption(false)` 来启用 OpenSSL 选项。
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

您可以通过将包含密码的 `JSONStoreOpenOptions` 对象传递到 `openCollections` 函数来保护存储器中的所有集合。如果未传递密码，那么不会加密存储器中所有集合的文档。
{: ios}

某些安全元数据存储在密钥链 (iOS) 中。存储器利用 256 位高级加密标准 (AES) 密钥进行加密。所有密钥通过基于密码的密钥派生函数 2 (PBKDF2) 进行增强。
{: ios}

使用 `closeAllCollections` 可锁定对所有集合的访问，直至再次调用 `openCollections`。如果将 `openCollections` 当作登录函数，那么可将 `closeAllCollections` 当作对应的注销函数。
{: ios}

使用 `changeCurrentPassword` 可更改密码。
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

您可以通过将包含密码的 `JSONStoreInitOptions` 对象传递到 `openCollections` 函数来保护存储器中的所有集合。如果未传递密码，那么不会加密存储器中所有集合的文档。
{: android}

某些安全元数据存储在共享首选项 (Android) 中。存储器利用 256 位高级加密标准 (AES) 密钥进行加密。所有密钥通过基于密码的密钥派生函数 2 (PBKDF2) 进行增强。
{: android}

使用 `closeAllCollections` 可锁定对所有集合的访问，直至再次调用 `openCollections`。如果将 `openCollections` 当作登录函数，那么可将 `closeAllCollections` 当作对应的注销函数。
{: android}

使用 `changeCurrentPassword` 可更改密码。
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

## JSONStore 中的多用户支持
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

您可以在单个 MobileFirst 应用程序中创建包含不同集合的多个存储器。`init` 函数可使用包含用户名的 options 对象。如果未提供任何用户名，那么缺省用户名为 *jsonstore*。
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

您可以在单个 MobileFirst 应用程序中创建包含不同集合的多个存储器。`init` 函数可使用包含用户名的 options 对象。如果未提供任何用户名，那么缺省用户名为 *jsonstore*。
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

您可以在单个 Mobile Foundation 应用程序中创建包含不同集合的多个存储器。`openCollections` 函数可使用包含用户名的 options 对象。如果未提供任何用户名，那么缺省用户名为 *jsonstore*。
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

## 适配器集成
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

适配器集成是可选的，它提供了将数据从集合发送到适配器以及从适配器将数据获取到集合的方法。如果需要提高灵活性，可以使用 `WLResourceRequest` 或 `jQuery.ajax` 来实现这些目标。
{: cordova}

1. 创建一个适配器并将其命名为 **JSONStoreAdapter**。
2. 定义其过程：`addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。
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
3. 要从适配器装入数据，请使用 `WLResourceRequest`。
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
4. 调用 `getPushRequired` 将返回所谓“脏文档”的数组，这些是包含后端系统上不存在的本地修订的文档。在调用 `push` 时，会将这些文档发送到适配器。
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
   为阻止 JSONStore 将文档标记为“脏”，请将 `{markDirty:false}` 选项传递到 `add`、`replace` 和 `remove`。
   {: tip}
   {: cordova}
5. 您还可以使用 `getAllDirty` API 来检索脏文档。
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
6. 要将更改推送到适配器，请调用 `getAllDirty` 以获取包含修订的文档的列表，然后使用 `WLResourceRequest`。在发送数据并且收到成功响应后，确保调用 `markClean`。
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
7. 使用 `enhance` 通过向集合原型添加函数来扩展核心 API 以适合您的需求。此示例（以下代码片段）显示如何使用 `enhance` 来添加作用于 `keyvalue` 集合的函数 `getValue`。它接受 key（字符串）作为其唯一参数并返回单个结果。
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
8. 请参阅**样本**部分中 Cordova 应用程序的 JSONStore 样本。此项目包含使用 JSONStore API 集合的 Cordova 应用程序。可从[此处](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)下载 JavaScript 适配器 Maven 项目。
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

适配器集成是可选的，它提供了将数据从集合发送到适配器以及从适配器将数据获取到集合的方法。可以使用 `WLResourceRequest` 来实现这些目标。
{: ios}

1. 创建一个适配器并将其命名为 **People**。
2. 定义其过程：`addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。
3. 要从适配器装入数据，请使用 `WLResourceRequest`。
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
4. 调用 `allDirty` 将返回所谓“脏文档”的数组，这些是包含后端系统上不存在的本地修订的文档。
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
   为阻止 JSONStore 将文档标记为“脏”，请将 `{markDirty:false}` 选项传递到 `add`、`replace` 和 `remove`。
   {: tip}
5. 要将更改推送到适配器，请调用 `allDirty` 以获取包含修订的文档的列表，然后使用 `WLResourceRequest`。在发送数据并且收到成功响应后，确保调用 `markDocumentsClean`。
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
6. 从**样本**部分下载本机 iOS Swift 应用程序项目。此项目包含使用 JSONStore API 集合的本机 iOS Swift 应用程序。可从[此处](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)下载 JavaScript 适配器 Maven 项目。
{: ios}

<!--### Android
{: #adapter_integration_android}-->

适配器集成是可选的，它提供了将数据从集合发送到适配器以及从适配器将数据获取到集合的方法。如果需要提高灵活性，可以使用 `WLResourceRequest` 之类的函数或者自己的 `HttpClient` 实例来实现这些目标。
{: android}

1. 创建一个适配器并将其命名为 **JSONStoreAdapter**。
2. 定义其过程：`addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。
3. 要从适配器装入数据，请使用 `WLResourceRequest`。
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
4. 调用 `findAllDirtyDocuments` 将返回所谓“脏文档”的数组，这些是包含后端系统上不存在的本地修订的文档。
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
   要阻止 JSONStore 将文档标记为“脏”，请将 `options.setMarkDirty(false)` 选项传递到 `add`、`replace` 和 `remove`。
   {: tip}
   {: android}
5. 要将更改推送到适配器，请调用 `findAllDirtyDocuments` 以获取包含修订的文档的列表，然后使用 `WLResourceRequest`。在发送数据并且收到成功响应后，确保调用 `markDocumentsClean`。
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
6. 从**样本**部分下载本机 Android 应用程序项目。此项目包含使用 JSONStore API 集合的本机 Android 应用程序。可从[此处](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)下载 JavaScript 适配器 Maven 项目。
{: android}
