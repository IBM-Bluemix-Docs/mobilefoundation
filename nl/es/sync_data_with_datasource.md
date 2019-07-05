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

# Sincronización de datos con un origen de datos
{: #sync_data_with_datasource}

Puede sincronizar automáticamente los datos entre una recopilación de JSONStore en un dispositivo con cualquier base de datos CouchDB remota, incluida [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management).

## Configuración de la sincronización entre JSONStore y Cloudant
{: #set_up_sync}

Para configurar la sincronización automática entre JSONStore y Cloudant, complete los pasos siguientes:

1. Defina la **política de sincronización** en la app móvil.
2. Despliegue el **adaptador de sincronización** en Mobile Foundation.

### Definición de la política de sincronización
{: #define_sync_policy}

El método de sincronización entre una recopilación de JSONStore y una base de datos Cloudant está definido por la **política de sincronización**. Especifique la **política de sincronización** en la app para cada recopilación.
Se debe inicializar una recopilación de JSONStore con un campo de **política de sincronización**. La **política de sincronización** puede ser una de las tres siguientes políticas:

1. `SYNC_DOWNSTREAM`
Utilice la política `SYNC_DOWNSTREAM` cuando desee descargar datos desde Cloudant en la recopilación de JSONStore. Esta política se suele utilizar para datos estáticos que son necesarios para el almacenamiento fuera de línea. Por ejemplo, la lista de precios de elementos de un catálogo. Cada vez que se inicialice la recopilación en el dispositivo, los datos se renuevan desde la base de datos Cloudant remota. Mientras que se descarga toda la base de datos por primera vez, las renovaciones siguientes descargarán solo delta, que se compone de los cambios realizados en la base de datos remota.
  
Revise el ejemplo de uso siguiente:

   * Android:
  
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
   ```

   * iOS: 
  
   ```objc
   openOptions.syncPolicy = SYNC_DOWNSTREAM;
   ```

   * Cordova: 
  
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
}
   ```

2. `SYNC_UPSTREAM`
  Utilice esta política cuando desee enviar por push datos locales a una base de datos Cloudant. Por ejemplo, la carga de datos de ventas capturados fuera de línea a una base de datos Cloudant. Cuando una recopilación está definida con la política `SYNC_UPSTREAM`, cualquier registro nuevo añadido a la recopilación crea un registro nuevo en Cloudant. De forma similar, cualquier documento modificado en la recopilación en el dispositivo modificará el documento en Cloudant, y los documentos suprimidos en la recopilación también se suprimirán de la base de datos Cloudant.

Revise el ejemplo de uso siguiente:

   * Android:
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
   ```

   * iOS:
   ```objc
   openOptions.syncPolicy = SYNC_UPSTREAM;
   ```

   * Cordova:
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
}
   ```

3. `SYNC_NONE`
  `SYNC_NONE` es la política predeterminada. Seleccione esta política para que no tenga lugar la sincronización.

La **política de sincronización** se atribuye a una recopilación JSONStore. Si se inicializa una recopilación con una **política de sincronización** en concreto, no se debería modificar. La modificación de la **política de sincronización** puede dar lugar a resultados indeseables.

### Despliegue del adaptador de sincronización
{: #deploy_sync_adapter}

`syncAdapterPath`
Esta configuración tiene el nombre del adaptador desplegado.

Revise el ejemplo de uso siguiente:

   * Android:
   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```

   * iOS:
   ```objc
    openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova o Ionic:
   ```javascript
    collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
   ```

Descargue el adaptador de `JSONStoreSync` desde [aquí](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/), configure las credenciales de Cloudant en la vía de acceso `src/main/adapter-resources/adapter.xml` y despliéguelo en el servidor de Mobile.

Configure las credenciales en la base de datos de Cloudant de fondo en la consola de operaciones Mobile Foundation.

### Realización de la operación de sincronización manualmente
{: #performing_sync_manual}

Si tiene que realizarse una sincronización en sentido ascendente o en sentido descendente en cualquier momento después de la inicialización explícitamente, se puede utilizar la siguiente API:

`sync()`

Esta API realiza una sincronización en sentido descendente si la recopilación de llamadas tiene una política de sincronización establecida `SYNC_DOWNSTREAM`. Si la política de sincronización está establecida en `SYNC_UPSTREAM`, se realizará una sincronización en sentido ascendente desde JSONStore a la base de datos Cloudant. La sincronización se lleva a cabo en los documentos que se han añadido, suprimido o reemplazado.

Revise el ejemplo de uso siguiente: 

  * Android:
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS:
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

  * Cordova:
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
