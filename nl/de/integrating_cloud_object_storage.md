---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-24"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

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
{:xml: .ph data-hd-programlang='xml'}

# {{ site.data.keyword.cos_full_notm }} in {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} verwenden
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) bietet auf Unternehmen abgestimmte Funktionen, die speziell für die Erstellung und Bereitstellung der nächsten Generation von kognitiven, kontextabhängigen und personalisierten mobilen Apps entwickelt wurden. {{site.data.keyword.cos_full_notm }} (COS) ist ein flexibler, kosteneffizienter und skalierbarer Cloudspeicher für unstrukturierte Daten. In diesem Handbuch wird erläutert, wie eine mobile Anwendung, die {{site.data.keyword.mobilefoundation_short}} verwendet, Daten verbinden und abrufen oder Daten in {{site.data.keyword.cos_full_notm }} über eine Ionic-Anwendung hochladen kann. Die Ionic-Anwendung, der Adapter und die zugehörigen Dateien für dieses How-to-Lernprogramm sind [hier](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) verfügbar.
{: shortdesc}


## Voraussetzungen
{: #cos-prerequisites}

1. Installieren Sie die [CLI (mfpdev-Befehle)](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html), indem Sie den Befehl `npm install -g mfpdev-cli` ausführen. Diese CLI wird verwendet, um die Ionic-App zu registrieren und den Adapter auf dem MF-Server zu implementieren. Alternativ können diese Aktivitäten auch über das MF-Server-Dashboard ausgeführt werden.

2. Installieren Sie die [{{ site.data.keyword.cloud_notm}}-CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli) auf Ihrer Maschine.

3. Installieren Sie die Ionic-ClI, indem Sie den Befehl `npm install -g ionic` ausführen.

4. Installieren Sie Cordova, indem Sie den Befehl `npm install -g cordova` ausführen.


## {{ site.data.keyword.mobilefoundation_short}}-Server konfigurieren
{: #mobile-foundation-server-setup}

Der {{ site.data.keyword.mobilefoundation_short}}-Server wird unter {{ site.data.keyword.cloud_notm}} konfiguriert. Richten Sie eine {{site.data.keyword.cloud_notm}}-Instanz des {{site.data.keyword.mobilefoundation_short}}-Servers wie folgt ein:

* Suchen Sie im {{ site.data.keyword.cloud_notm}}-Katalog nach '*{{ site.data.keyword.mobilefoundation_short}}*'. Klicken Sie auf die Kachel für **{{site.data.keyword.mobilefoundation_short}}**.

    ![MFP-Katalog](images/mfp_catalog.png)

* Geben Sie einen geeigneten Namen für die {{site.data.keyword.mobilefoundation_short}}-Serverinstanz an und klicken Sie auf **Erstellen**.

    ![BMMFP - Neu](images/bmmfpnew.png)

* Es wird eine neue MF-Serverinstanz erstellt und nach Anmeldeberechtigungen gefragt.

    ![MFP-Anmeldung](images/mfp_login.png)

* Die Berechtigungsnachweise für die Anmeldung beim MF-Server befinden sich auf der Registerkarte **Berechtigungsnachweise** im Menü. 

    ![MF-Berechtigungsnachweise](images/mfp_credentials.png)

* Geben Sie diese Berechtigungsnachweise an, um sich anzumelden und das MF-Dashboard aufzurufen.

    ![MF-Dashboard](images/mfp_dashboard.png)


## Cloud Object Storage konfigurieren
{: #cloud-object-storage-setup}

* Suchen Sie im {{ site.data.keyword.cloud_notm}}-Katalog nach '*Cloud Object Storage*'. Klicken Sie auf die Kachel für **Object Storage**.

    ![Katalog](images/catalog.png)

* Geben Sie einen Namen für Ihre COS-Instanz an und klicken Sie auf **Erstellen**.

    ![COS erstellen](images/cos_create.png)

* Klicken Sie als Nächstes in den Menüoptionen im Fensterbereich auf **Buckets**. Geben Sie einen geeigneten Namen (in diesem Beispiel heißt das Bucket `sharedgallery`) für das Bucket an und klicken Sie auf **Erstellen**.

    ![Bucket erstellen](images/bucketcreate.png)

* Nach der Erstellung des Buckets sieht die Landing-Page des Buckets wie folgt aus und stellt Optionen zum direkten Hochladen von Daten zur Verfügung.

    ![Landing-Page des Buckets](images/bucketlanding.png)

* Fügen Sie hier die drei Dateien aus dem Ordner **Short stories** im [Beispiel](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) hinzu.


## Ionic-App von MFP-COS und Java-Adapter
{: #mfp-cos-ionic-app-and-java-adapter}

Laden Sie dieses [Git-Repository](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) herunter oder klonen Sie es. Dieser Repository setzt sich aus zwei Hauptkomponenten zusammen:

1. MF-Java-Adapter
2. Mobile Ionic-App

### 'mfpdev-cli' konfigurieren
{: #configuring-mfpdev-cli}

Fügen Sie die Serverdetails zur CLI hinzu. Führen Sie in der Eingabeaufforderung den Befehl	`mfpdev server add` aus.

```
? Geben Sie den Namen des neuen Serverprofils ein:
```

Geben Sie einen Namen für den Server an und drücken Sie die Eingabetaste. In diesem Beispiel ist der angegebene Name `mfpserver`.

```
? Geben Sie die vollständig qualifizierte URL des Servers ein:
```

Geben Sie die URL des Servers ein. Für den MF-Server unter {{site.data.keyword.cloud_notm}} enthält die Registerkarte mit den Serviceberechtigungsnachweisen die URL. Der HTTPS-Port für den MF-Server unter {{ site.data.keyword.cloud_notm}} ist 443 und der HTTP-Port für die MF-Serverinstanz lautet 80.

```
? Geben Sie die Anmelde-ID für den MobileFirst Server-Administrator ein: (admin)
```

Geben Sie `admin` ein und drücken Sie die **Eingabetaste**.

```
? Geben Sie das Administratorkennwort für MobileFirst Server ein:
```

Geben Sie das in den Serviceberechtigungsnachweisen verfügbare Kennwort ein.

```
? Administratorkennwort für diesen Server speichern?: (j/n)
```

Geben Sie auf der Basis Ihrer Vorgabe *j/n* ein und geben Sie die von den Eingabeaufforderungen anforderten Details ein.

```
? Geben Sie das Zeitlimit für MobileFirst Server-Verbindungen in Sekunden ein: 30
```

Legen Sie 30 Sekunden als das Standardzeitlimit fest.

```
? Server als Standardserver definieren?: (j/n)
```

Geben Sie *j* ein und drücken Sie die **Eingabetaste**.

***Erwartete Ausgabe***:

```
Serverkonfiguration wird geprüft...
Die folgenden Laufzeiten sind derzeit auf diesem Server installiert: mfp
Serverprofil 'mfpserver' erfolgreich hinzugefügt.
```

### MF-Java-Adapter konfigurieren
{: #configuring-the-mf-java-adapter}

Um eine Verbindung zu Ihrer COS-Instanz herzustellen, müssen einige Details Ihrer COS-Instanz in der Datei `adapter.xml` angegeben werden. Geben Sie Werte für die folgenden Felder ein:

1. **endpointURL**: Dieses Feld ist die URL des öffentlichen Endpunkts für Ihr COS-Objekt. Diese URL befindet sich im COS-Dashboard unter **Buckets (Optionen im Menü) -> <name_des_buckets> (`sharedgallery` in diesem Beispiel) -> Konfiguration -> Endpunkte -> Öffentlich**.
2. **AuthToken**: In diesem Lernprogramm wird die IAM-Authentifizierung verwendet.

Damit der Java-Adapter eine Verbindung zu Ihrer Instanz von COS herstellen kann, ist eine Authentifizierung erforderlich, die IAM oder HMAC verwendet. Im Folgenden sind die Schritte zum Abrufen des IAM-Tokens angegeben. Weitere Einzelheiten zu IAM- und HMAC-Authentifizierungsprozessen finden Sie [hier](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions).

#### IAM-Oauth-Token mit der {{site.data.keyword.cloud_notm}}-CLI abrufen
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. Stellen Sie zunächst sicher, dass Sie über einen API-Schlüssel verfügen. Rufen Sie den API-Schlüssel über [{{site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users) ab.
2. Melden Sie sich mithilfe der CLI bei der {{ site.data.keyword.cloud_notm}}-Plattform an.

  ```bash
	ibmcloud login --apikey <wert>
  ```
  {: codeblock}
  Ihre Ausgabe ähnelt dem folgenden Snippet:

	```text
	Authenticating...
	OK

	Zielgruppenspezifisches Konto <kontoname> (<konto-id>)

	Standard der zielgruppenspezifischen Ressourcengruppe

	API-Endpunkt:     https://api.us-south.cf.cloud.ibm.com (API version: 	2.128.0)
	Region:           us-south
	User:             <e-mail-adresse>
	Account:          <kontoname> (<konto-id>)
	Resource group:   default
	```

3. Führen Sie den folgenden Befehl in der Befehlszeilenschnittstelle aus, um alle Serviceinstanzen für Ihr {{site.data.keyword.cloud_notm}}-Konto zu erhalten.

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	Expected output:

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <kontoname> as <e-mail-adresse>...
	OK
	Name                                               Location     	State    Type               Tags
	<name_der_ressourceninstanz>                           global       	active   service_instance
	```

4. Führen Sie den folgenden Befehl aus, um die Details zu Ihrer COS-Instanz abzurufen. <instanzname> ist der Name Ihres COS-Service ('newObject' für dieses Lernprogramm).

  ```bash
	ibmcloud resource service-instance <instanzname>
  ```
  {: codeblock}

	Expected output:

	```text
	Retrieving service instance <instanzname> in resource group 	Default under account <kontoname> as<e-mail-adresse>...
	OK

	Name:                  <name_der_ressourceninstanz>
	ID:                    <id_der_ressourceninstanz>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. Führen Sie den folgenden Befehl aus, um das IAM-Token abzurufen:
  ```bash
	ibmcloud iam oauth-tokens
  ```

	Expected output:

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <aktualisierungstoken>
	```

Nachdem Sie die Werte für *endpointURL* und *authToken* hinzugefügt haben, müssen Sie den Adapter erstellen. Navigieren Sie in der Eingabeaufforderung zum Stammordner des Adapters und führen Sie den folgenden Befehl aus:

```bash
mfpdev adapter build
```
{: codeblock}

Die Datei '*.adapter' wird im Ordner 'target' erstellt. Führen Sie den folgenden Befehl aus:

```bash
mfpdev adapter deploy
```
{: codeblock}

Der Adapter wird für die MF-Instanz bereitgestellt.

Alternativ kann der Adapter auch im Dashboard des MF-Servers implementiert werden. Öffnen Sie dazu das Dashboard und klicken Sie im Menü auf **Adapter -> Neu**, um die Seite zum Erstellen eines neuen Adapters (wie nachfolgend dargestellt) zu öffnen.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

Klicken Sie dann auf **Adapter bereitstellen** und laden Sie die Datei '.adapter' aus dem Ordner **target** hoch.


### Ionic-App konfigurieren
{: #configuring-the-ionic-app}

Führen Sie in der App die folgenden Schritte aus:

1. Navigieren Sie zu dem Ordner, der die Ionic-Anwendung enthält.

2. Fügen Sie das Cordova-MF-Plug-in hinzu.

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Fügen Sie die Android- oder iOS-Plattform hinzu.

  ```bash
	ionic cordova platform add android
  ```
	Oder

	```bash
	ionic cordova platform add ios
	```

4. Registrieren Sie die App beim MF-Server, indem Sie den folgenden Befehl ausführen:

  ```bash
	mfpdev app register
	```

	Alternativ kann die App auch im Dashboard des MF-Servers registriert werden. Öffnen Sie dazu das Dashboard und klicken Sie im Menü auf **Anwendungen -> Neu**.

	Die folgende Seite wird geladen.

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	Geben Sie die angeforderten Details ein. Geben Sie einen Namen für Ihre Anwendung in das Textfeld 'Anwendungsname' ein. Wählen Sie die erforderliche Plattform aus.

	Bei Android akzeptiert das Textfenster **Paket** die *Anwendungs-ID*. Dieser Parameter befindet sich in der Datei 'AndroidManifest.xml' als Paket Ihrer Android-Anwendung. Das Textfeld **Version** muss mit dem Wert für *versionName* aus der Datei 'AndroidManifest.xml' gefüllt werden.

	Für iOS akzeptiert das Feld **Bundle-ID** die *Anwendungs-ID* (Groß-/Kleinschreibung muss beachtet werden). Dieser Parameter befindet sich in der Datei 'mfpclient.plist' Ihrer iOS-Anwendung. Das Textfeld **Version** muss mit dem Wert für *version* aus der Datei 'mfpclient.plist' in der iOS-Anwendung gefüllt werden.

5. Führen Sie 'ionic cordova prepare' aus, damit Änderungen unverändert an die hinzugefügten Umgebungen weitergeleitet werden.
6. Führen Sie
  ```bash
	ionic cordova build android
  ```
	oder
  ```bash
	ionic cordova build ios
  ```
	aus, um sicherzustellen, dass die Typscriptänderungen zu den Umgebungen hinzugefügt werden.

7. Hängen Sie das Gerät an oder führen Sie einen Emulator oder Simulator aus und setzen Sie den folgenden Befehl ab.

	```bash
  ionic cordova run android
	```
	Oder
	```bash
	ionic cordova run ios
  ```

### In der Ionic-Anwendung navigieren
{: #navigating-the-ionic-application}

Die Ionic-Anwendung zeigt eine Liste von Short Storys an, die zuvor (als letzter Schritt im Abschnitt [Cloud Object Storage konfigurieren](#cloud-object-storage-setup)) von der erstellten COS-Instanz hochgeladen wurden. Wenn Sie eine bestimmte Story-Option auswählen, wird die ausgewählte Story geladen und angezeigt. Es wird auch eine Option zum Hinzufügen einer Story bereitgestellt, die dann in die COS-Instanz hochgeladen wird.

Die anfängliche COS-Objektliste sieht folgendermaßen aus.

![COS vorher](images/cos_before.png)

Auf der Startseite der Anwendung können Benutzer entweder *Alle Storys abrufen* oder eine *Story hinzufügen*

![Startseite der App](images/app-home-screen.png)

Wenn Sie auf **Alle Storys abrufen** klicken, werden die in COS verfügbaren Storys angezeigt.

![App - Story auswählen](images/app-select-story.png)

Wählen Sie eine zu ladende Story aus dem Dropdown-Menü aus und klicken Sie auf **Laden**.

![App - Story geladen](images/app-story-loaded.png)

Klicken Sie anschließend auf die Schaltfläche **Story hinzufügen**, um eine eigene Story hinzuzufügen. Geben Sie einen Titel für die Story und den Inhalt der Story in den Textbereich ein und klicken Sie auf **Hinzufügen**.

![App - Eingabe hinzufügen](images/app-add-input.png)

Sobald die Story hinzugefügt wurde, wird eine Nachricht darüber angezeigt, dass die Story erfolgreich hinzugefügt wurde.

![App - Story hinzugefügt](images/app-story-added.png)

Dem COS-Dashboard wurde nun auch die Story von der Anwendung hinzugefügt.

![COS - hinzugefügt](images/cos_added.png)


Das IAM-OAuth-Token, das mit `ibmcloud iam oauth-tokens` abgerufen werden kann, hat ein Ablaufdatum und der Adapter schlägt mit der Ausnahme `403 - Forbidden` fehl. Es muss daher sichergestellt werden, dass das Token für den bereitgestellten Adapter gültig ist, damit die App ordnungsgemäß funktionieren kann.
{: note}
