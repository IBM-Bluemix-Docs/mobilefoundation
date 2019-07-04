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

# Utilizzo di {{ site.data.keyword.cos_full_notm }} con {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) offre funzionalità di livello aziendale progettate unicamente per supportare la creazione e la distribuzione della prossima generazione di applicazioni mobili cognitive, contestuali e personalizzate. {{ site.data.keyword.cos_full_notm }} (COS) è un'archiviazione cloud flessibile, conveniente e scalabile per i dati non strutturati. Questa guida procedurale spiega in che modo un'applicazione mobile che utilizza {{ site.data.keyword.mobilefoundation_short}} può collegarsi e recuperare o caricare i dati in {{ site.data.keyword.cos_full_notm }} tramite un'applicazione Ionic. L'applicazione Ionic, l'adattatore e i file correlati per questa guida procedurale sono disponibili [qui](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).
{: shortdesc}


## Prerequisiti
{: #cos-prerequisites}

1. Installa [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) eseguendo `npm install -g mfpdev-cli`. Questa cli viene utilizzata per registrare l'applicazione Ionic e distribuire l'adattatore nel server MF. In alternativa, queste attività possono essere eseguite dal dashboard del server MF.

2. Installa la [{{ site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli) sulla tua macchina.

3. Installa Ionic cli eseguendo `npm install -g ionic`

4. Installa Cordova eseguendo `npm install -g cordova`


## Configurazione del server {{ site.data.keyword.mobilefoundation_short}}
{: #mobile-foundation-server-setup}

Il server {{ site.data.keyword.mobilefoundation_short}} viene configurato su {{ site.data.keyword.cloud_notm}}. Configura un'istanza {{ site.data.keyword.cloud_notm}} del server {{ site.data.keyword.mobilefoundation_short}} nel seguente modo:

* Nel catalogo {{ site.data.keyword.cloud_notm}}, ricerca *{{ site.data.keyword.mobilefoundation_short}}*. Fai clic sul tile **{{ site.data.keyword.mobilefoundation_short}}**.

    ![MFPCatalog](images/mfp_catalog.png)

* Specifica un nome adatto per l'istanza del server {{ site.data.keyword.mobilefoundation_short}} e fai clic su **Crea**.

    ![BMMFPNew](images/bmmfpnew.png)

* Viene creata una nuova istanza del server MF e vengono richieste le credenziali di accesso.

    ![MFPLogin](images/mfp_login.png)

* Le credenziali per accedere al server MF possono essere trovate nella scheda **Credentials** nel menu.

    ![MFPcredentials](images/mfp_credentials.png)

* Fornisci queste credenziali ed esegui l'accesso per entrare nel dashboard MF.

    ![MFPDashboard](images/mfp_dashboard.png)


## Configurazione di Cloud Object Storage
{: #cloud-object-storage-setup}

* Nel catalogo {{ site.data.keyword.cloud_notm}}, ricerca *Cloud Object Storage*. Fai clic sul tile **Object Storage**.

    ![Catalogo](images/catalog.png)

* Specifica un nome per la tua istanza COS e fai clic su **Create**.

    ![Crea COS](images/cos_create.png)

* Successivamente, fai clic su **Buckets** nelle opzioni del menu. Specifica un nome adatto (in questo esempio abbiamo scelto di denominare il bucket `sharedgallery`) per il tuo bucket e fai clic su **Create**.

    ![Crea il bucket](images/bucketcreate.png)

* Una volta creato il bucket, la pagina di destinazione del bucket avrà un aspetto come quello della seguente immagine, fornendoti le opzioni per caricare direttamente i dati.

    ![Pagina di destinazione del bucket](images/bucketlanding.png)

* Qui, aggiungi i tre file forniti nella cartella **Short stories** nell'[esempio](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).


## Applicazione Ionic MFP-COS e adattatore Java
{: #mfp-cos-ionic-app-and-java-adapter}

Scarica questo [repository git](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) oppure clonalo. Questo repository è composto da due componenti principali:

1. Un adattatore Java MF
2. Un'applicazione mobile Ionic

### Configurazione di mfpdev-cli
{: #configuring-mfpdev-cli}

Aggiungi i dettagli del server alla cli. Nel prompt di comandi, esegui	`mfpdev server add`.

```
? Enter the name of the new server profile:
```

Specifica un nome per il server e premi Invio. In questo esempio, il nome specificato è `mfpserver`.

```
? Enter the fully qualified URL of this server:
```

Immetti l'url del server. Per il server MF su {{ site.data.keyword.cloud_notm}}, la scheda delle credenziali del servizio contiene l'url. La porta per il server MF https su {{ site.data.keyword.cloud_notm}} è 443 e per l'istanza del server MF http è 80.

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

Immetti `admin` e premi **Invio**.

```
? Enter the MobileFirst Server administrator password:
```

Immetti la password disponibile nelle credenziali del servizio.

```
? Save the administrator password for this server?: (Y/n)
```

In base alle tue preferenze, immetti *Y/N* e inserisci i dettagli come richiesto dalle richieste.

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

Imposta 30 sec come timeout predefinito

```
? Make this server the default?: (Y/n)
```

Immetti *Y* e premi **Invio**.

***Output previsto***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### Configurazione dell'adattatore Java MF
{: #configuring-the-mf-java-adapter}

Per collegarti alla tua istanza COS, devi fornire alcuni dettagli della tua istanza COS nel file `adapter.xml`. Specifica i valori per i seguenti campi:

1. **endpointURL**: questo campo è l'url dell'endpoint pubblico per il tuo oggetto COS. Puoi trovare questo URL nel dashboard del tuo COS, in **Buckets (nelle opzioni del menu) -> <your-bucket-name> (`sharedgallery` in questo esempio) -> Configuration -> Endpoints -> Public**
2. **AuthToken**: in questa esercitazione, stiamo utilizzando l'autenticazione IAM.

Affinché l'adattatore java si colleghi alla tua istanza di COS, è necessaria l'autenticazione che utilizza IAM o HMAC. Di seguito viene riportata la procedura per ottenere il token IAM. Per ulteriori dettagli sui processi di autenticazione IAM e HMAC, fai clic [qui](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions).

#### Come ottenere il token IAM Oauth utilizzando la CLI {{ site.data.keyword.cloud_notm}}
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. Innanzitutto, assicurati di avere una chiave API. Ottieni la chiave API da [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users).
2. Accedi a {{ site.data.keyword.cloud_notm}} Platform utilizzando la CLI.

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  Il tuo output è simile al seguente frammento:

	```text
	Authenticating...
	OK

	Targeted account <account-name> (<account-id>)

	Targeted resource group default

	API endpoint:     https://api.us-south.cf.cloud.ibm.com (API version: 	2.128.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. Per ottenere tutte le istanze del servizio nel tuo account {{ site.data.keyword.cloud_notm}}, esegui questo comando nella CLI.

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	Output previsto:

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. Esegui il seguente comando per ottenere i dettagli della tua istanza COS. <instance-name> è il nome del tuo servizio COS (`newObject` per questa esercitazione).

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	Output previsto:

	```text
	Retrieving service instance <sinstance-name> in resource group 	Default under account <account-name> as<email-address>...
	OK

	Name:                  <resource-instance-name>
	ID:                    <resource-instance-id>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. Per ottenere il token IAM, esegui questo comando:
  ```bash
	ibmcloud iam oauth-tokens
  ```

	Output previsto:

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

Una volta che hai aggiunto i valori *endpointURL* e *authToken*, crea l'adattatore. Vai alla cartella root dell'adattatore nel prompt di comandi ed esegui

```bash
mfpdev adapter build
```
{: codeblock}

Il file `*.adapter` viene creato nella cartella `target`. Esegui questo comando:

```bash
mfpdev adapter deploy
```
{: codeblock}

L'adattatore viene distribuito nell'istanza MF.

In alternativa, l'adattatore può essere distribuito sul dashboard del server MF. Apri il dashboard del server MF, nel menu, fai clic su **Adapters -> New** per aprire la pagina come mostrato nella seguente immagine.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

Quindi, fai clic su **Deploy Adapter** e carica il file `.adapter` dalla cartella **target**.


### Configurazione dell'applicazione Ionic
{: #configuring-the-ionic-app}

Nell'applicazione, esegui questi passi:

1. Vai alla cartella che contiene l'applicazione Ionic.

2. Aggiungi il plugin MF Cordova

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Aggiungi la piattaforma Android o iOS

  ```bash
	ionic cordova platform add android
  ```
	Oppure

	```bash
	ionic cordova platform add ios
	```

4. Registra la tua applicazione sul server MF eseguendo

  ```bash
	mfpdev app register
	```

	In alternativa, l'applicazione può essere registrata nel dashboard del server MF. Apri il dashboard del server MF e, nel menu, fai clic su **Applications -> New**.

	Viene caricata la pagina mostrata nella seguente immagine.

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	Immetti i dettagli richiesti. Assegna un nome alla tua applicazione nella casella di testo 'Application Name'. Scegli la piattaforma necessaria.

	Per Android, la casella di testo **Package** accetta *Application identifier*. Puoi trovare questo parametro nel file `AndroidManifest.xml` come pacchetto della tua applicazione Android. Il campo della casella di testo **Version** deve essere completato con il valore *versionName* del file `AndroidManifest.xml`.

	Per iOS, la casella di testo **Bundle ID** accetta *Application identifier* (sensibile al minuscolo/maiuscolo). Puoi trovare questo parametro nel file `mfpclient.plist` della tua applicazione iOS. Il campo della casella di testo **Version** deve essere completato con il valore *version* del file `mfpclient.plist` nella tua applicazione iOS.

5. Esegui `ionic cordova prepare` affinché le modifiche vengano applicate agli ambienti aggiunti.
6. Esegui
  ```bash
	ionic cordova build android
  ```
	Oppure
  ```bash
	ionic cordova build ios
  ```
	per assicurarti che le modifiche typescript vengano aggiunte agli ambienti.

7. Collega il dispositivo o esegui un emulatore o un simulatore ed esegui il seguente comando.

	```bash
  ionic cordova run android
	```
	Oppure
	```bash
	ionic cordova run ios
  ```

### Navigazione nell'applicazione Ionic
{: #navigating-the-ionic-application}

L'applicazione Ionic visualizza un elenco di storie brevi che sono state caricate in precedenza (come ultimo passo nella sezione [Configurazione di Cloud Object Storage](#cloud-object-storage-setup)), dalla tua istanza COS creata. Alla selezione di una particolare opzione di storia, la storia selezionata viene caricata e visualizzata. Viene fornita anche un'opzione per aggiungere una storia che viene poi caricata nell'istanza COS.

L'elenco degli oggetti COS iniziali ha un aspetto simile a quello della seguente immagine.

![COS iniziali](images/cos_before.png)

La home page dell'applicazione offre un'opzione per eseguire *Get all stories* o *Add story*

![Schermata iniziale dell'applicazione](images/app-home-screen.png)

Facendo clic su **Get all stories**, vengono visualizzate le storie disponibili in COS.

![Selezione storia dell'applicazione](images/app-select-story.png)

Seleziona una storia da caricare dal menu a discesa e fare clic su **Load**

![Storia dell'applicazione caricata](images/app-story-loaded.png)

Successivamente, fai clic sul pulsante **Add story** per aggiungere una tua storia. Immetti un titolo per la storia e il contenuto della storia nell'area di testo e fai clic su **Add**.

![Aggiunto input dell'applicazione](images/app-add-input.png)

Una volta aggiunta la storia, viene visualizzato un messaggio indicante l'esito positivo dell'aggiunta della storia.

![Storia dell'applicazione aggiunta](images/app-story-added.png)

Ora nel dashboard COS è presente anche la storia aggiunta dall'applicazione.

![COS aggiunto](images/cos_added.png)


Il token oauth IAM ottenuto utilizzando `ibmcloud iam oauth-tokens` ha un tempo di scadenza e l'adattatore ha un malfunzionamento con eccezione `403 - Forbidden`. È pertanto necessario garantire che il token sia valido nell'adattatore distribuito affinché l'applicazione funzioni come previsto.
{: note}
