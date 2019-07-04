---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: synchronization of data, sync with offline storage, jsonstore sync

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 与数据源同步数据
{: #sync_data_with_datasource}

您可以在设备上的 JSONStore 集合与任何远程 CouchDB 数据库（包括 [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management)）之间自动同步数据。

## 设置 JSONStore 与 Cloudant 之间的同步
{: #set_up_sync}

要设置 JSONStore 与 Cloudant 之间的自动同步，请完成以下步骤：

1. 在移动应用程序上定义**同步策略**。
2. 在 Mobile Foundation 上部署**同步适配器**。

### 定义同步策略
{: #define_sync_policy}

JSONStore 集合与 Cloudant 数据库之间的同步方法由**同步策略**定义。在应用程序中为每个集合指定**同步策略**。JSONStore 集合必须使用**同步策略**字段进行初始化。**同步策略**可以是以下三个策略中的一个：

1. `SYNC_DOWNSTREAM`
  要将数据从 Cloudant 下载到 JSONStore 集合时，请使用 `SYNC_DOWNSTREAM` 策略。此策略通常用于脱机存储器所需的静态数据。例如，目录中各项的价格表。每次在设备上初始化集合时，都会从远程 Cloudant 数据库刷新数据。第一次会下载整个数据库，但随后的刷新仅下载增量（即包含对远程数据库所做的更改）。
  
  
查看以下用法：

   * Android：
  
   ```java
  initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
  ```

   * iOS： 
  
   ```objc
  openOptions.syncPolicy = SYNC_DOWNSTREAM;
  ```

   * Cordova： 
  
   ```javascript
  collection.sync = {
    syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
  }
  ```

2. `SYNC_UPSTREAM`
  要将本地数据推送到 Cloudant 数据库时，请使用此策略。例如，将以脱机方式捕获到的销售数据上传到 Cloudant 数据库。使用 `SYNC_UPSTREAM` 策略定义集合时，添加到该集合的任何新记录都会在 Cloudant 中创建新记录。同样，设备上的集合中修改的任何文档都将在 Cloudant 上修改相应文档；如果从集合中删除了文档，那么还将从 Cloudant 数据库中删除相应文档。
  

查看以下用法：

   * Android：
   ```java
  initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
  ```

   * iOS：
   ```objc
  openOptions.syncPolicy = SYNC_UPSTREAM;
  ```

   * Cordova：
   ```javascript
  collection.sync = {
    syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
  }
  ```

3. `SYNC_NONE`
  `SYNC_NONE` 是缺省策略。选择此策略不会执行同步。

**同步策略**归属于 JSONStore 集合。如果某个集合使用特定**同步策略**进行了初始化，那么不应更改该策略。修改**同步策略**可能会导致意外结果。

### 部署同步适配器
{: #deploy_sync_adapter}

`syncAdapterPath`
此配置采用部署的适配器名称。

查看以下用法：

   * Android：
   ```java
 initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
 ```

   * iOS：
   ```objc
  openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
 ```

   * Cordova 或 Ionic：
   ```javascript
  collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
 ```

从[此处](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/)下载 `JSONStoreSync` 适配器，在 `src/main/adapter-resources/adapter.xml` 路径中配置 Cloudant 凭证，然后将其部署在 Mobile Foundation 服务器中。

在 Mobile Foundation Operations Console 中，将凭证配置到后端 Cloudant 数据库。

### 手动执行同步操作
{: #performing_sync_manual}

如果在初始化后的任何时候必须显式执行上游或下游同步，那么可以使用以下 API：

`sync()`

如果调用集合的同步策略设置为 `SYNC_DOWNSTREAM`，那么此 API 会执行下游同步。如果同步策略设置为 `SYNC_UPSTREAM`，那么会执行从 JSONStore 到 Cloudant 数据库的上游同步。这将针对已添加、删除或替换的文档执行同步。

查看以下用法： 

  * Android：
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS：
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

  * Cordova：
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
