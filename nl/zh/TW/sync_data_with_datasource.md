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

# 與資料來源同步資料
{: #sync_data_with_datasource}

您可以自動同步化裝置上 JSONStore 集合與任何遠端 CouchDB 資料庫（包括 [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management)）之間的資料。

## 設定 JSONStore 與 Cloudant 之間的同步化
{: #set_up_sync}

若要設定 JSONStore 與 Cloudant 之間的自動同步化，請完成下列步驟：

1. 在行動應用程式上定義**同步原則**。
2. 在 Mobile Foundation 上部署**同步配接器**。

### 定義同步原則
{: #define_sync_policy}

JSONStore 集合與 Cloudant 資料庫之間的同步化方法是透過**同步原則**所定義。針對每個集合指定應用程式中的**同步原則**。JSONStore 集合必須以**同步原則**欄位來起始設定。**同步原則**可以是下列三個原則之一：

1. `SYNC_DOWNSTREAM`
  當您要將資料從 Cloudant 下載至 JSONStore 集合時，請使用 `SYNC_DOWNSTREAM` 原則。此原則通常用於離線儲存所需的靜態資料。例如，型錄中的項目價格表。每次在裝置上起始設定集合時，都會重新整理遠端 Cloudant 資料庫中的資料。第一次下載整個資料庫時，重新整理之後只會下載差異，而差異包含對遠端資料庫所做的變更。
  
  
請檢閱下列用量：

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
  當您要將本端資料推送至 Cloudant 資料庫時，請使用此原則。例如，將離線擷取的銷售資料上傳至 Cloudant 資料庫。使用 `SYNC_UPSTREAM` 原則定義集合時，任何新增至集合的新記錄都會在 Cloudant 中建立新記錄。同樣地，裝置集合中已修改的任何文件都會修改 Cloudant 上的文件，也會從 Cloudant 資料庫中刪除集合中已刪除的文件。
  

請檢閱下列用量：

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
  `SYNC_NONE` 是預設原則。選擇此原則，不進行同步化。

**同步原則**歸因於 JSONStore 集合。如果使用特定**同步原則**來起始設定集合，則不應該變更它。修改**同步原則**可能會導致無法預期的結果。

### 部署同步配接器
{: #deploy_sync_adapter}

`syncAdapterPath`
此配置會採用已部署的配接器名稱。

請檢閱下列用量：

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

從[這裡](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/)下載 `JSONStoreSync` 配接器、在 `src/main/adapter-resources/adapter.xml` 路徑中配置 Cloudant 認證，並將它部署至 Mobile Foundation Server。

在「Mobile Foundation 作業主控台」中將認證配置至後端 Cloudant 資料庫。

### 手動執行同步作業
{: #performing_sync_manual}

如果在明確起始設定之後，隨時都必須執行上游或下游同步，則可以使用下列 API：

`sync()`

如果呼叫端集合將同步原則設為 `SYNC_DOWNSTREAM`，則此 API 會執行下游同步。如果同步原則設為 `SYNC_UPSTREAM`，則會執行從 JSONStore 到 Cloudant 資料庫的上游同步。針對已新增、已刪除或已取代的文件，執行同步。

請檢閱下列用量： 

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
