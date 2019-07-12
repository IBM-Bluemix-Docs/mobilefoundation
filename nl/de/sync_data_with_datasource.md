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

# Daten mit einer Datenquelle synchronisieren
{: #sync_data_with_datasource}

Sie können Daten automatisch zwischen einer JSONStore-Sammlung auf einem Gerät und einer beliebigen fernen CouchDB-Datenbank (einschließlich einer [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management)-Datenbank) synchronisieren.

## Synchronisation zwischen JSONStore und Cloudant einrichten
{: #set_up_sync}

Führen Sie die folgenden Schritte aus, um die automatische Synchronisation zwischen JSONStore und Cloudant einzurichten:

1. Definieren Sie die **Synchronisationsrichtlinie** in Ihrer mobilen App.
2. Stellen Sie den **Synchronisationsadapter** in Mobile Foundation bereit.

### Synchronisationsrichtlinie definieren
{: #define_sync_policy}

Die Methode zur Synchronisation zwischen einer JSONStore-Sammlung und einer Cloudant-Datenbank wird durch die **Synchronisationsrichtlinie** definiert. Geben Sie die **Synchronisationsrichtlinie** für Ihre jeweilige Sammlung in Ihrer App an.
Eine JSONStore-Sammlung muss mit einem Feld **Synchronisationsrichtlinie** initialisiert werden. Die **Synchronisationsrichtlinie** kann eine der drei folgenden Richtlinien sein:

1. `SYNC_DOWNSTREAM`
  Verwenden Sie die Richtlinie `SYNC_DOWNSTREAM`, wenn Sie Daten von Cloudant in die JSONStore-Sammlung herunterladen wollen. Diese Richtlinie wird in der Regel für statische Daten verwendet, die für den Offlinespeicher erforderlich sind. Beispiel: Eine Preisliste der Artikel in einem Katalog. Jedes Mal, wenn die Datensammlung auf dem Gerät initialisiert wird, werden die Daten von der fernen Cloudant-Datenbank aktualisiert. Während beim ersten Mal die gesamte Datenbank heruntergeladen wird, wird bei den nachfolgenden Aktualisierungen nur das Delta heruntergeladen, also die Änderungen, die in der fernen Datenbank vorgenommen wurden.
  
Überprüfen Sie die folgende Verwendung: 

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
  Verwenden Sie diese Richtlinie, wenn Sie lokale Daten per Push-Operation an eine Cloudant-Datenbank übergeben wollen. Beispiel: Das Hochladen von offline erfassten Verkaufsdaten in eine Cloudant-Datenbank. Wird eine Sammlung mit der Richtlinie `SYNC_UPSTREAM` definiert, wird beim Hinzufügen von neuen Datensätzen zu der Sammlung ein neuer Datensatz in Cloudant erstellt. Analog dazu wird bei der Änderung eines Dokuments in der Sammlung auf dem Gerät dieses Dokument auch in Cloudant geändert und in der Sammlung gelöschte Dokumente werden auch in der Cloudant-Datenbank gelöscht.

Überprüfen Sie die folgende Verwendung: 

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
  `SYNC_NONE` die die Standardrichtlinie. Wählen Sie diese Richtlinie aus, wenn keine Synchronisation erfolgen soll.

Die **Synchronisationsrichtlinie** wird einer JSONStore-Sammlung zugeordnet. Wenn eine Sammlung mit einer bestimmten **Synchronisationsrichtlinie** initialisiert wird, sollte sie nicht geändert werden. Das Ändern der **Synchronisationsrichtlinie** kann zu unerwünschten Ergebnissen führen.

### Synchronisierungsadapter bereitstellen
{: #deploy_sync_adapter}

`syncAdapterPath`
Bei dieser Konfiguration wird der Name des Adapters verwendet, der bereitgestellt wird.

Überprüfen Sie die folgende Verwendung: 

   * Android:
   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Hier ist "JSONStoreCloudantSync" der Name des Adapters.
   ```

   * iOS:
   ```objc
    openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova oder Ionic:
   ```javascript
    collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
   ```

Laden Sie den `JSONStoreSync`-Adapter von [hier](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/) herunter, konfigurieren Sie die Berechtigungsnachweise für Cloudant im Pfad `src/main/adapter-resources/adapter.xml` und stellen Sie ihn auf Ihrem Mobile Foundation-Server bereit.

Konfigurieren Sie die Berechtigungsnachweise für die Back-End-Cloudant-Datenbank in der Mobile Foundation Operations Console.

### Synchronisation manuell ausführen
{: #performing_sync_manual}

Wenn irgendwann nach der Initialisierung eine vor- oder nachgeordnete Synchronisation explizit ausgeführt werden muss, kann dazu die folgende API verwendet werden:

`sync()`

Diese API führt eine nachgeordnete Synchronisation durch, wenn die aufrufende Sammlung eine Synchronisationsrichtlinie aufweist, die auf `SYNC_DOWNSTREAM` gesetzt ist. Wenn die Synchronisationsrichtlinie auf `SYNC_UPSTREAM` gesetzt ist, wird eine vorgeordnete Synchronisation von JSONStore auf die Cloudant-Datenbank ausgeführt. Die Synchronisation wird für Dokumente ausgeführt, die hinzugefügt, gelöscht oder ersetzt wurden.

Überprüfen Sie die folgende Verwendung:  

  * Android:
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS:
 ```objc
  collection.sync(); //Hier ist 'collection' das initialisierte JSONStore-Objekt 'collection'
 ```

  * Cordova:
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
