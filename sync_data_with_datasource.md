---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: synchronization of data, sync with offline storage, jsonstore sync

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

# Sync data with a data source
{: #sync_data_with_datasource}

You can automatically synchronize data between a JSONStore collection on a device with any remote CouchDB database, including [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management).

## Set up the synchronization between JSONStore and Cloudant
{: #set_up_sync}

To set up automatic synchronization between JSONStore and Cloudant complete the following steps:

1. Define the `Sync Policy` on your mobile app.
1. Deploy the `Sync Adapter` on {{site.data.keyword.mobilefoundation_short}}.

### Defining the Sync Policy
{: #define_sync_policy}

The method of synchronization between a JSONStore collection and a Cloudant database is defined by the `Sync Policy`. Specify the `Sync Policy` in your app for each collection.
A JSONStore collection must be initialized with a `Sync Policy` field. `Sync Policy` can be one of the following three policies:

1. `SYNC_DOWNSTREAM`
   Use the `SYNC_DOWNSTREAM` policy when you want to download data from Cloudant to the JSONStore collection. This policy is typically used for static data that is required for auxiliary storage. For example, price list of items in a catalog. Each time the collection is initialized on the device, data is refreshed from the remote Cloudant database. While the entire database is downloaded for the first time, following refresh would download only delta, consisting of the changes made on the remote database.

   Review the following usage:

   * Android:
  
      ```java
      initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
      ```
      {: codeblock}

   * iOS: 
  
      ```objc
      openOptions.syncPolicy = SYNC_DOWNSTREAM;
      ```
      {: codeblock}

   * Cordova: 
  
      ```javascript
      collection.sync = {
         syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
      }
      ```
      {: codeblock}

1. `SYNC_UPSTREAM`
   Use this policy when you want to push local data to a Cloudant database. For example, uploading of sales data captured offline to a Cloudant database. When a collection is defined with the `SYNC_UPSTREAM` policy, any new records that were added to the collection creates a new record in Cloudant. Similarly, any document that is modified in the collection on the device modifies the document on Cloudant and documents that are deleted in the collection will also be deleted from the Cloudant database.

   Review the following usage:

   * Android:

      ```java
      initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
      ```
      {: codeblock}

   * iOS:

      ```objc
      openOptions.syncPolicy = SYNC_UPSTREAM;
      ```
      {: codeblock}

   * Cordova:

      ```javascript
      collection.sync = {
        syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
      }
      ```
      {: codeblock}

1. `SYNC_NONE`
   `SYNC_NONE` is the default policy. Choose this policy for synchronization to not take place.

   The `Sync Policy` is attributed to a JSONStore collection. If a collection is initialized with a particular `Sync Policy`, it shouldn't be changed. Modifying the `Sync Policy` can lead to undesirable results.

### Deploying the sync adapter
{: #deploy_sync_adapter}

`syncAdapterPath` - This configuration takes the adapter name that is deployed.

Review the following usage:

* Android:

   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```
   {: codeblock}

* iOS:

   ```objc
   openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```
   {: codeblock}

* Cordova or Ionic:

   ```javascript
   collection.sync = {
   syncAdapterPath:"JSONStoreCloudantSync"
   }
   ```
   {: codeblock}

Download the `JSONStoreSync` adapter from [here](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/){: external}, configure Cloudant credentials in the path `src/main/adapter-resources/adapter.xml`, and deploy it in your {{site.data.keyword.mobilefoundation_short}} server.

Configure the credentials to the backend Cloudant database in the {{site.data.keyword.mobilefoundation_short}} Operations console.

### Performing the sync operation manually
{: #performing_sync_manual}

If an upstream or downstream sync must be performed at any time after the initialization explicitly, the following API can be used:

`sync()`

This API performs a downstream sync if the calling collection has a sync policy set to `SYNC_DOWNSTREAM`. If the sync policy is set to `SYNC_UPSTREAM`, an upstream sync from JSONStore to Cloudant database is performed. Sync is performed for documents that were added, deleted, or replaced.

Review the following usage: 

* Android:

   ```java
   WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
   ```
   {: codeblock}

* iOS:

   ```objc
   collection.sync(); //Here collection is the JSONStore collection object that was initialized
   ```
   {: codeblock}

  * Cordova:

   ```javascript
   WL.JSONStore.get(collectionName).sync();
   ```
   {: codeblock}
