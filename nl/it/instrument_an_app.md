---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, instrumenting cordova app, instrumenting iOS app, instrumenting android app

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
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
{:web: .ph data-hd-programlang='Web'}

# Dota di strumentazione la tua applicazione
{: #instrument_your_app}

Mobile Analytics è una funzione integrata nel servizio {{ site.data.keyword.mobilefoundation_short }}. Mobile Analytics fornisce delle informazioni approfondite fondamentali sulle prestazioni e sull'utilizzo delle applicazioni per i proprietari di applicazioni e gli sviluppatori di applicazioni mobili.

Devi dotare di strumentazione la tua applicazione mobile per utilizzare la funzione Mobile Analytics per monitorare il tuo utilizzo dell'applicazione, le prestazioni e per ottenere altre statistiche. Dotare di strumentazione la tua applicazione è un processo che si articola nei seguenti passi:

1.  Importa e installa l'SDK client Mobile Analytics.
2.  Dota di strumentazione la tua applicazione in base al tipo di dati di analisi che desideri raccogliere.

Le seguenti sezioni forniscono i dettagli per questi passi, per ciascuna delle piattaforme supportate.

### Dota di strumentazione un'applicazione Android
{: #instrument_android_app}
{: android}
#### Passo 1: importa e installa l'SDK client Mobile Analytics per Android
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

L'SDK client Mobile Analytics viene distribuito con Gradle, un gestore dipendenze per i progetti Android. Gradle
scarica automaticamente le risorse utente dai repository e le rende disponibili alla tua applicazione Android.
{: android}

1. Crea un progetto [Android Studio ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://developer.android.com/sdk/index.html){: new_window} oppure apri un progetto esistente.
{: android}

2. Apri il file `build.gradle` che si trova nel tuo **modulo dell'applicazione**.
{: android}

  Il tuo progetto Android può avere due file `build.gradle`, uno per il progetto e uno per il modulo dell'applicazione. Assicurati di usare il file del **modulo dell'applicazione**.
  {: tip}
  {: android}

3. Trova la sezione `Dependencies` del file `build.gradle` e aggiungi una dipendenza di compilazione per l'SDK client {{site.data.keyword.mobileanalytics_short}}. La tua istruzione relativa ai repository deve essere simile a questo esempio di codice: Your repositories statement must be similar to the following code example:
{: android}

  ```
  dependencies {
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationanalytics:8.0.+@aar'
    // other dependencies
  }
  ```
  {: codeblock}
  {: android}

  La prima dipendenza è per l'SDK client Mobile Analytics per acquisire e registrare gli eventi di runtime dell'applicazione e la seconda dipendenza è per abilitare il feedback degli utenti   all'interno dell'applicazione interagendo con l'utente dell'applicazione. La seconda dipendenza è richiesta solo se abiliti il feedback degli utenti all'interno dell'applicazione
  {: android}

4. Sincronizza il tuo progetto con Gradle facendo clic su **Tools &gt; Android &gt; Sync Project with Gradle Files**.
{: android}

5. Apri il file `AndroidManifest.xml` per il tuo progetto Android. Puoi trovare questo file in **app > manifests**. Aggiungi l'autorizzazione di accesso a internet e di accesso all'ubicazione sotto l'elemento `<manifest>`:
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />

  ```
  {: codeblock}
  Se stai utilizzando una versione di SDK superiore alla >= 1.2, devi inserire le seguenti righe nell'elemento `<application>` del file `AndroidManifest.xml`.
  {: android}

  ```
    <activity
  android:name="com.ibm.mobilefirstplatform.clientsdk.android.ui.UIActivity"
  android:label="@string/app_name"
  android:launchMode="singleTask">
  <intent-filter>
  <action android:name="android.intent.action.MAIN" />
  <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  </activity>
  ```
  {: codeblock}
  {: android}

#### Passo 2: dota di strumentazione la tua applicazione Android in base al tipo di dati di analisi che desideri raccogliere.
{: #instrument_app_based_on_data_android }
{: android}

1. Inizializza la tua applicazione per acquisire e inviare i dati di analisi al servizio Mobile Analytics.  Innanzitutto, aggiungi le seguenti istruzioni `import` all'inizio della tua classe di applicazione o attività:
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   Successivamente, utilizza l'SDK client Mobile Analytics per configurare o inizializzare l'acquisizione dei dati di analisi.  
   Aggiungi il codice di inizializzazione nel metodo `onCreate` dell'attività principale nella tua applicazione Android oppure in un'ubicazione più indicata per il tuo progetto dell'applicazione.
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   Prima di richiamare il metodo init, devi assicurarti che la tua applicazione integri il codice richiesto per autenticare e autorizzare il dispositivo con il servizio MobileFoundation.  Questo passo è un passo comune richiesto da tutte le applicazioni di servizi Mobile Foundation e non è specifico per l'acquisizione di dati di analisi. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   Con l'inizializzazione completa, la tua applicazione è ora abilitata ad acquisire le informazioni sui dispositivi e i log di SDK di Mobile Analytics senza ulteriore codice aggiunto.  Eventuali ulteriori API e codice trattati nelle seguenti sezioni sono facoltativi e possono essere aggiunti in base al tipo di dati di analisi che desideri acquisire.
   {: android}

2. Per acquisire gli eventi del ciclo di vita dell'applicazione come ad esempio le informazioni sulle sessioni e sugli arresti anomali di un'applicazione, configura la tua applicazione aggiungendo la seguente riga di codice:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. Per acquisire gli eventi di rete che acquisiscono le interazioni di rete dell'applicazione, configura la tua applicazione aggiungendo la seguente riga di codice:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. Hai ora inizializzato la tua applicazione per l'acquisizione dei dati di analisi.  Devi quindi inviare i dati acquisiti al servizio Mobile Analytics.
   Usa la seguente API per inviare i dati di analisi al servizio Mobile Analytics.
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  Sei libero di decidere quando richiamare questa API nel tuo flusso dell'applicazione per inviare i dati di analisi acquisiti al servizio Mobile Analytics.  Fino a questo invio, tutti i dati di analisi acquisiti sono archiviati localmente sul dispositivo.
  {: android}

5. Per acquisire e inviare i log dell'applicazione al servizio Mobile Analytics, utilizza le API del logger dell'SDK client Mobile Analytics.     
   Il framework di registrazione dell'SDK client Mobile Analytics supporta i seguenti livelli di log, che sono elencati da quello meno dettagliato a quello più dettagliato, con le linee guida di utilizzo consigliate:
    * FATAL - utilizzalo per i blocchi o gli arresti anomali irreversibili. Il livello FATAL è riservato per la registrazione degli errori irreversibili, che si presentano agli utenti come un arresto anomalo dell'applicazione
    * ERROR - utilizzalo per le eccezioni impreviste o gli errori di protocollo di rete imprevisti
    * WARN - utilizzalo per registrare le avvertenze di utilizzo che non sono considerate errori critici, come ad esempio l'utilizzo di API obsolete o una risposta di rete lenta
    * INFO - utilizzalo per la notifica di eventi di inizializzazione e altri dati che potrebbero essere importanti senza però essere urgenti
    * DEBUG - utilizzalo per la notifica delle istruzioni di debug per aiutare gli sviluppatori a risolvere i difetti delle applicazioni.
    Quando il livello del logger è impostato su DEBUG, puoi ottenere anche i log dell'SDK client Mobile Analytics, che sono inclusi quando invii i log.
   {: note}
   {: android}

   I seguenti frammenti di codice mostrano un utilizzo del Logger di esempio per Android:
   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);

   //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
   Logger logger = Logger.getInstance("loggerName");

   // Log messages with different levels
   // Debug message for feature 1
   // Info message for feature 2
   logger.debug("debug message");
   //the logger.debug message is not logged because the logLevelFilter is set to Info
   logger.info("info message");

   // Send logs to the Mobile Analytics
   Logger.send();
   ```
   {: codeblock}
   {: android}

6.  Per ottenere informazioni approfondite sui modelli di onboarding degli utenti (nuovi utenti rispetto a quelli che ritornano), devi associare un'identità utente alla tua sessione dell'applicazione.  Tale passo può essere eseguito richiamando la seguente API
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    Tutti i dati di analisi acquisiti e archiviati localmente vengono inviati al servizio Mobile Analytics solo quando viene richiamata l'API WLAnalytics.send().
    {: android}

7.  Per ottenere informazioni approfondite sui modelli di interazione della rete HTTP delle tue applicazioni, come ad esempio le API HTTP richiamate, il numero di richieste e i tempi di risposta medi, devi utilizzare la classe WLResourceRequest dell'SDK client del servizio Mobile Foundation per effettuare le chiamate HTTP.  Anche se hai inizializzato l'analisi per l'acquisizione degli eventi di rete, l'utilizzo di `WLResourceRequest` abilita Mobile Analytics ad agganciarsi alle chiamate di rete e ad acquisire i dati pertinenti.  Effettua le tue chiamate HTTP come nel seguente codice.
    ```java
      WLResourceRequest request = new WLResourceRequest(new URI(url), WLResourceRequest.GET);
            request.send(new WLResponseListener() {

                @Override
                public void onSuccess(WLResponse wlResponse) {
                    //handle success of HTTP call and use wlResponse
                }

                @Override
                public void onFailure(WLFailResponse wlFailResponse) {
                    //handle failure of HTTP call and use wlFailResponse
                }
            });
    ```
    {: codeblock}
    {: android}

8.  Per ottenere i log e l'analisi degli arresti anomali, puoi richiamare la seguente API durante l'avvio della tua applicazione per controllare se si è verificato un arresto anomalo nella sessione precedente e inviare di conseguenza i log degli arresti anomali acquisiti al servizio Mobile Analytics.
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  Per definire l'analisi personalizzata e definire i tuoi dati di analisi oltre quanto supportato inerentemente nell'SDK client, puoi utilizzare l'API di registrazione personalizzata
    ```java
        //create a JSON to capture the custom data
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("FromPage", "LoginPage");

        //log the custom data with a message
        WLAnalytics.log("log message", jsonObject);

        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.send();
    ```
    {: codeblock}
    {: android}

    I dati personalizzati registrati possono essere tracciati su grafici personalizzati che puoi definire nella console Mobile Analytics per ricavare informazioni approfondite personalizzate.
    {: android}

10. Utilizza il feedback degli utenti all'interno dell'applicazione per approfondire la tua analisi delle prestazioni dell'applicazione. Puoi abilitare **utenti e tester** della tua applicazione a fornire un feedback contestuale completo ai proprietari dell'applicazione. I **proprietari dell'applicazione** ottengono un feedback in tempo reale dai loro utenti sull'esperienza di utilizzo dell'applicazione su cui i **proprietari dell'applicazione** e gli **sviluppatori** possono intervenire ulteriormente.  Questa funzione porta una notevole agilità nella manutenzione dell'applicazione.  Utilizza la seguente API per passare la tua applicazione alla modalità di feedback interattiva in qualsiasi gestore dell'azione nella tua applicazione, ad esempio quando gestisci il clic di un pulsante o la selezione di una voce di menu.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### Dota di strumentazione un'applicazione iOS
{: #instrument_ios_app}
{: ios}

Passi per dotare di strumentazione la tua applicazione iOS:
{: ios}

#### Passo 1: importa e installa l'SDK client Mobile Analytics per iOS
{: #install_analytics_sdk_ios }
{: ios}

[![Compatibile con CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![Compatibile con CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

L'SDK Swift è disponibile per iOS e watchOS.
{: ios}

1. Aggiungi quanto segue al podfile esistente, che si trova nella root del progetto Xcode
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
       pod 'IBMMobileFirstPlatformFoundationAnalytics'
   end
   ```
   {: codeblock}
   {: ios}

2. Da una finestra di riga di comando, vai alla root del progetto Xcode ed esegui questo comando
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### Passo 2: dota di strumentazione la tua applicazione iOS in base al tipo di dati di analisi che desideri raccogliere.
{: #instrument_app_based_on_data_ios }
{: ios}

1. Inizializza la tua applicazione per abilitare l'invio dei log a Mobile Analytics.
   L'SDK Swift è disponibile per iOS e watchOS. Importa i framework `BMSCore` e `BMSAnalytics` aggiungendo le seguenti istruzioni `import` all'inizio del tuo file di progetto `AppDelegate.swift`:
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   Successivamente, utilizza l'SDK client Mobile Analytics per configurare o inizializzare l'acquisizione dei dati di analisi.
   Aggiungi il codice di inizializzazione in un'ubicazione ottimale per il tuo progetto di applicazione.
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   Prima di richiamare il metodo init, devi assicurarti che la tua applicazione integri il codice richiesto per autenticare e autorizzare il dispositivo con il servizio MobileFoundation.  Questo passo è un passo comune richiesto da tutte le applicazioni di servizi Mobile Foundation e non è specifico per l'acquisizione di dati di analisi. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   Con l'inizializzazione completa, la tua applicazione è ora abilitata ad acquisire le informazioni sui dispositivi e i log di SDK di Mobile Analytics senza ulteriore codice aggiunto.  Eventuali ulteriori API e codice trattati nelle seguenti sezioni sono facoltativi e possono essere aggiunti in base al tipo di dati di analisi che desideri acquisire.
   {: ios}

2. Per acquisire gli eventi del ciclo di vita dell'applicazione come ad esempio le informazioni sulle sessioni e sugli arresti anomali di un'applicazione, configura la tua applicazione aggiungendo la seguente riga di codice:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. Per acquisire gli eventi di rete che acquisiscono le interazioni di rete dell'applicazione, configura la tua applicazione aggiungendo la seguente riga di codice:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. Hai ora inizializzato la tua applicazione per l'acquisizione dei dati di analisi.  Devi quindi inviare i dati acquisiti al servizio Mobile Analytics.
   Usa la seguente API per inviare i dati di analisi al servizio Mobile Analytics.
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    Sei libero di decidere quando richiamare questa API nel tuo flusso dell'applicazione per inviare i dati di analisi acquisiti al servizio Mobile Analytics.  Fino a questo invio, tutti i dati di analisi acquisiti sono archiviati localmente sul dispositivo.
    {: ios}

5. Per acquisire e inviare i log dell'applicazione al servizio Mobile Analytics, utilizza le API del logger dell'SDK client Mobile Analytics.
   Il framework di registrazione dell'SDK client Mobile Analytics supporta i seguenti livelli di log, che sono elencati da quello meno dettagliato a quello più dettagliato, con le linee guida di utilizzo consigliate:
    * FATAL - utilizzalo per i blocchi o gli arresti anomali irreversibili. Il livello FATAL è riservato per la registrazione degli errori irreversibili, che si presentano agli utenti come un arresto anomalo dell'applicazione
    * ERROR - utilizzalo per le eccezioni impreviste o gli errori di protocollo di rete imprevisti
    * WARN - utilizzalo per registrare le avvertenze di utilizzo che non sono considerate errori critici, come ad esempio l'utilizzo di API obsolete o una risposta di rete lenta
    * INFO - utilizzalo per la notifica di eventi di inizializzazione e altri dati che potrebbero essere importanti senza però essere urgenti
    * DEBUG - utilizzalo per la notifica delle istruzioni di debug per aiutare gli sviluppatori a risolvere i difetti delle applicazioni.
    Quando il livello del logger è impostato su DEBUG, puoi ottenere anche i log dell'SDK client Mobile Analytics, che sono inclusi quando invii i log.
   {: note}
   {: ios}

    Attieniti all'[esercitazione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) per i frammenti di codice per il Logger per aggiungere le funzionalità di registrazione nelle applicazioni iOS.

6.  Per ottenere informazioni approfondite sui modelli di onboarding degli utenti (nuovi utenti rispetto a quelli che ritornano), devi associare un'identità utente alla tua sessione dell'applicazione.  Questa associazione può essere eseguita richiamando la seguente API
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    Tutti i dati di analisi acquisiti e archiviati localmente vengono inviati al servizio Mobile Analytics solo quando viene richiamata l'API `WLAnalytics.sharedInstance().send()`.
    {: ios}

7.  Per ottenere informazioni approfondite sui modelli di interazione della rete HTTP delle tue applicazioni, come ad esempio le API HTTP richiamate, il numero di richieste e i tempi di risposta medi, devi utilizzare la classe WLResourceRequest dell'SDK client del servizio Mobile Foundation per effettuare le chiamate HTTP.  Anche se hai inizializzato l'analisi per l'acquisizione degli eventi di rete, l'utilizzo di  `WLResourceRequest` abilita Mobile Analytics ad agganciarsi alle chiamate di rete e ad acquisire i dati pertinenti.  Effettua le tue chiamate HTTP come nel seguente codice.
    ```Swift
      let request = WLResourceRequest( url: URL(string: "/adapters/JavaAdapter/users"), method: WLHttpMethodGet )

      request!.send { (response, error) -> Void in
          if(error == nil){
              //handle failure of HTTP call and use wlFailResponse
          }
          else{
              //handle success of HTTP call and use wlResponse
          }
      }
    ```
    {: codeblock}
    {: ios}

8.  Per ottenere i log e l'analisi degli arresti anomali, puoi richiamare la seguente API durante l'avvio della tua applicazione per controllare se si è verificato un arresto anomalo nella sessione precedente e inviare i log degli arresti anomali acquisiti al servizio Mobile Analytics.
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  Per definire l'analisi personalizzata e definire i tuoi dati di analisi oltre quanto supportato inerentemente nell'SDK client, puoi utilizzare l'API di registrazione personalizzata
    ```Swift
        //create an array object with key value pair as custom data
        let eventObject = ["FromPage": "LoginPage"]

        //log the custom data with a message
        WLAnalytics.sharedInstance().log("log message", withMetadata: eventObject);

        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.sharedInstance().send();
    ```
    {: codeblock}
    {: ios}

    I dati personalizzati registrati possono essere tracciati su grafici personalizzati che puoi definire nella console Mobile Analytics per ricavare informazioni approfondite personalizzate.
    {: ios}

10. Utilizza il feedback degli utenti all'interno dell'applicazione per approfondire la tua analisi delle prestazioni dell'applicazione.   Puoi abilitare **utenti e tester** della tua applicazione a fornire un feedback contestuale completo ai proprietari dell'applicazione. I **proprietari dell'applicazione** ottengono un feedback in tempo reale dai loro utenti sull'esperienza di utilizzo dell'applicazione su cui i **proprietari dell'applicazione** e gli **sviluppatori** possono intervenire ulteriormente.  Questa funzione porta una notevole agilità nella manutenzione dell'applicazione.  Utilizza la seguente API per passare la tua applicazione alla modalità di feedback interattiva in qualsiasi gestore dell'azione nella tua applicazione, ad esempio quando gestisci il clic di un pulsante o la selezione di una voce di menu.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Dota di strumentazione un'applicazione Cordova
{: #instrument_cordova_app}
{: cordova}
Passi per dotare di strumentazione la tua applicazione Cordova:
{: cordova}

#### Passo 1: importa e installa i plugin SDK client Foundation e Analytics per Cordova
{: #install_analytics_sdk_cordova }
Il plug-in Cordova Mobile Analytics ti consente di dotare di strumentazione la tua applicazione mobile.
{: cordova}

#### Installazione dell'SDK client Cordova
{: #install_cordova_sdk}
{: cordova}

1. Crea un progetto [Cordova ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://cordova.apache.org/#getstarted){: new_window} oppure apri un progetto esistente.
    {: cordova}

2. Aggiungi le piattaforme Android o iOS di tua scelta alla tua applicazione Cordova. Esegui uno o entrambi i seguenti comandi dalla riga di comando.<br/>
    **Android**:
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS**:
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. Aggiungi il plugin sdk client Foundation `cordova-plugin-mfp`.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. Aggiungi il plugin sdk client Analytics facoltativo `cordova-plugin-mfp-analytics` che è per la funzionalità di feedback all'interno dell'applicazione per la tua applicazione
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. Verifica che il plug-in sia stato installato correttamente eseguendo questo comando:
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### Passo 2: dota di strumentazione la tua applicazione Cordova in base al tipo di dati di analisi che desideri raccogliere.
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. Nelle applicazioni Cordova, non è richiesta alcuna configurazione e l'inizializzazione è integrata.

   Prima di richiamare uno qualsiasi dei seguenti metodi di analisi, devi assicurarti che la tua applicazione integri il codice necessario per autenticare e autorizzare il dispositivo presso il servizio MobileFoundation. Questo passo è un passo comune richiesto da tutte le applicazioni di servizi Mobile Foundation e non è specifico per l'acquisizione di dati di analisi. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   Con l'inizializzazione completa, la tua applicazione è ora abilitata ad acquisire le informazioni sui dispositivi e i log di SDK di Mobile Analytics senza ulteriore codice aggiunto.  Eventuali ulteriori API e codice trattati nelle seguenti sezioni sono facoltativi e possono essere aggiunti in base al tipo di dati di analisi che desideri acquisire.
   {: cordova}

2. Per abilitare l'acquisizione degli eventi del ciclo di vita, è necessario eseguirne l'inizializzazione nella piattaforma nativa dell'applicazione Cordova.
    * Per la piattaforma Android:
      * Apri il file **[cartella root dell'applicazione Cordova] → platforms → android → src → com → sample → [nome-applicazione] → MainActivity.java**.
      * Cerca il metodo `onCreate` e abilita le attività `LIFECYCLE` e `NETWORK` seguendo questa procedura:
        * Per abilitare la registrazione degli eventi del ciclo di vita del client, aggiungi la seguente riga:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Per abilitare la registrazione degli eventi di rete del client, aggiungi la seguente riga:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Crea il progetto Cordova eseguendo il comando: `cordova build`.
    * Per la piattaforma iOS:
      * Apri la cartella **[cartella root dell'applicazione Cordova] → platforms → ios → Classes** e trova il file **AppDelegate.swift** (Swift).
      * Per abilitare le attività `LIFECYCLE` e `NETWORK`, esegui questi passi.
        * Per abilitare la registrazione degli eventi del ciclo di vita del client, aggiungi la seguente riga:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Per abilitare la registrazione degli eventi di rete del client, aggiungi la seguente riga:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Crea il progetto Cordova eseguendo il comando: `cordova build`.

3. Hai ora inizializzato la tua applicazione per la raccolta dei dati di analisi. Puoi quindi inviare i dati di analisi a Mobile Analytics.
   Utilizza le seguenti API per avviare la registrazione e inviare l'analisi dell'utilizzo:
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. Per acquisire e inviare i log dell'applicazione al servizio Mobile Analytics, utilizza le API del logger dell'SDK client Mobile Analytics.
   Il framework di registrazione dell'SDK client Mobile Analytics supporta i seguenti livelli di log, che sono elencati da quello meno dettagliato a quello più dettagliato, con le linee guida di utilizzo consigliate:
    * FATAL - utilizzalo per i blocchi o gli arresti anomali irreversibili. Il livello FATAL è riservato per la registrazione degli errori irreversibili, che si presentano agli utenti come un arresto anomalo dell'applicazione
    * ERROR - utilizzalo per le eccezioni impreviste o gli errori di protocollo di rete imprevisti
    * WARN - utilizzalo per registrare le avvertenze di utilizzo che non sono considerate errori critici, come ad esempio l'utilizzo di API obsolete o una risposta di rete lenta
    * INFO - utilizzalo per la notifica di eventi di inizializzazione e altri dati che potrebbero essere importanti senza però essere urgenti
    * DEBUG - utilizzalo per la notifica delle istruzioni di debug per aiutare gli sviluppatori a risolvere i difetti delle applicazioni.
    Quando il livello del logger è impostato su DEBUG, puoi ottenere anche i log dell'SDK client Mobile Analytics, che sono inclusi quando invii i log.
    * TRACE - utilizzato per i punti di entrata e di uscita del metodo
   {: note}
   {: cordova}

   I seguenti frammenti di codice mostrano un utilizzo del Logger di esempio per Cordova:
    ```Javascript
    // Set the logging level (optional)
    // The default setting is DEBUG
    WL.Logger.config({"level": "INFO"});

    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    var logger = WL.Logger.create({ pkg: 'loggerName' });

    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug","debug message");
    //the logger.debug message is not logged because the logLevelFilter is set to Info
    logger.info("info","info message");

    // Send logs to the Mobile Analytics
    logger.send();
    ```
    {: codeblock}
    {: cordova}

5.  Per ottenere informazioni approfondite sui modelli di onboarding degli utenti (nuovi utenti rispetto a quelli che ritornano), devi associare un'identità utente alla tua sessione dell'applicazione.  Non ci sono API specifiche disponibili dal livello JavaScript. Tale operazione può però essere eseguita nel codice nativo richiamando la seguente API:

    **Android:**
      ```java
        WLAnalytics.setUserContext("userName or userIdentity");
      ```
      {: codeblock}
      {: cordova}

    **iOS:**
      ```Swift
        WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
      ```
      {: codeblock}
      {: cordova}

    Tutti i dati di analisi acquisiti e archiviati localmente vengono inviati al servizio Mobile Analytics solo quando viene richiamata l'API WL.Analytics.send() nel livello javascript [oppure] l'API WLAnalytics.send() in android nativa [oppure] l'API WLAnalytics.sharedInstance().send() in iOS nativa.
    {: cordova}

6. Per ottenere informazioni approfondite sui modelli di interazione della rete HTTP delle tue applicazioni, come ad esempio le API HTTP richiamate, il numero di richieste e i tempi di risposta medi, devi utilizzare la classe WLResourceRequest dell'SDK client del servizio Mobile Foundation per effettuare le chiamate HTTP.  Anche se hai inizializzato l'analisi per l'acquisizione degli eventi di rete, l'utilizzo di  `WLResourceRequest` abilita Mobile Analytics ad agganciarsi alle chiamate di rete e ad acquisire i dati pertinenti.  Effettua le tue chiamate HTTP utilizzando il seguente codice.
    ```Javascript
      var resourceRequest = new WLResourceRequest("url-path", WLResourceRequest.GET);
      resourceRequest.send().then(
        onSuccess: function(response) {
            resultText = "Successfully called the resource: " + response.responseText;
        },

        onFailure: function(response) {
            resultText = "Failed to call the resource:" + response.errorMsg;
        }
      );
    ```
    {: codeblock}
    {: cordova}

7. Per definire l'analisi personalizzata e definire i tuoi dati di analisi oltre quanto supportato inerentemente nell'SDK client, puoi utilizzare l'API di registrazione personalizzata
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    I dati personalizzati registrati possono essere tracciati su grafici personalizzati che puoi definire nella console Mobile Analytics per ricavare informazioni approfondite personalizzate.
    {: cordova}

8. Utilizza il feedback degli utenti all'interno dell'applicazione per approfondire la tua analisi delle prestazioni dell'applicazione.   Puoi abilitare **utenti e tester** della tua applicazione a fornire un feedback contestuale completo ai proprietari dell'applicazione. I **proprietari dell'applicazione** ottengono un feedback in tempo reale dai loro utenti sull'esperienza di utilizzo dell'applicazione su cui i **proprietari dell'applicazione** e gli **sviluppatori** possono intervenire ulteriormente.  Questo passo porta una notevole agilità nella manutenzione dell'applicazione.  Utilizza la seguente API per passare la tua applicazione alla modalità di feedback interattiva in qualsiasi gestore dell'azione nella tua applicazione, ad esempio quando gestisci il clic di un pulsante o la selezione di una voce di menu.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Dota di strumentazione un'applicazione Web
{: #instrument_web_app}
{: web}

Passi per dotare di strumentazione la tua applicazione Web:
{: web}

#### Passo 1: importa e installa l'SDK client Mobile Analytics per Web
{: #install_analytics_sdk_web }
{: web}

#### Installazione dell'SDK client Web
{: #install_web_sdk}
L'SDK Mobile Analytics ti consente di dotare di strumentazione la tua applicazione web.
{: web}

1. Vai alla cartella root della tua applicazione web ed esegui il seguente comando. Aggiungi l'SDK alla tua applicazione web utilizzando [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### Passo 2: dota di strumentazione la tua applicazione Web in base al tipo di dati di analisi che desideri raccogliere.
{: #instrument_app_based_on_data_web }
{: web}

1. Fai riferimento ai file ibmmfpf.js e ibmmfpfanalytics.js nell'elemento `head` della tua pagina html principale.

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. Inizializza l'SDK Web Mobile Foundation specificando i valori di root di contesto e ID applicazione nel file JavaScript principale della tua applicazione web

    ```Javascript
      var wlInitOptions = {
        mfpContextRoot : '/mfp', // "mfp" is the default context root in the Mobile Foundation
        applicationId : 'com.sample.mywebapp' // Replace with your own value.
      };

      WL.Client.init(wlInitOptions).then (
        function() {
          // Application logic.
        }
      );
    ```
    {: codeblock}
    {: web}

   Prima di richiamare uno qualsiasi dei seguenti metodi di analisi, devi assicurarti che la tua applicazione integri il codice necessario per autenticare e autorizzare il dispositivo presso il servizio MobileFoundation. Questo passo è un passo comune richiesto da tutte le applicazioni di servizi Mobile Foundation e non è specifico per l'acquisizione di dati di analisi. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   Con l'inizializzazione completa, la tua applicazione è ora abilitata ad acquisire le informazioni sui dispositivi e i log di SDK di Mobile Analytics senza ulteriore codice aggiunto.  Eventuali ulteriori API e codice trattati nelle seguenti sezioni sono facoltativi e possono essere aggiunti in base al tipo di dati di analisi che desideri acquisire.
   {: web}

3. Aggiungi la seguente riga alla tua logica dell'applicazione per acquisire gli eventi del ciclo di vita e gli eventi di rete dell'applicazione.

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. Hai ora inizializzato la tua applicazione per l'acquisizione dei dati di analisi.  Devi quindi inviare i dati acquisiti al servizio Mobile Analytics. Usa la seguente API per inviare i dati di analisi al servizio Mobile Analytics.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    Sei libero di decidere quando richiamare questa API nel tuo flusso dell'applicazione per inviare i dati di analisi acquisiti al servizio Mobile Analytics.  Fino a questo invio, tutti i dati di analisi acquisiti sono archiviati localmente sul dispositivo.
    {: web}

5. Per acquisire e inviare i log dell'applicazione al servizio Mobile Analytics, utilizza le API del logger dell'SDK client Mobile Analytics.
   Il framework di registrazione dell'SDK client Mobile Analytics supporta i seguenti livelli di log, che sono elencati da quello meno dettagliato a quello più dettagliato, con le linee guida di utilizzo consigliate:
    * FATAL - utilizzalo per i blocchi o gli arresti anomali irreversibili. Il livello FATAL è riservato per la registrazione degli errori irreversibili, che si presentano agli utenti come un arresto anomalo dell'applicazione
    * ERROR - utilizzalo per le eccezioni impreviste o gli errori di protocollo di rete imprevisti
    * WARN - utilizzalo per registrare le avvertenze di utilizzo che non sono considerate errori critici, come ad esempio l'utilizzo di API obsolete o una risposta di rete lenta
    * INFO - utilizzalo per la notifica di eventi di inizializzazione e altri dati che potrebbero essere importanti senza però essere urgenti
    * DEBUG - utilizzalo per la notifica delle istruzioni di debug per aiutare gli sviluppatori a risolvere i difetti delle applicazioni.
    Quando il livello del logger è impostato su DEBUG, puoi ottenere anche i log dell'SDK client Mobile Analytics, che sono inclusi quando invii i log.
    * TRACE - utilizzato per i punti di entrata e di uscita del metodo
   {: note}
   {: web}

   I seguenti frammenti di codice mostrano un utilizzo del Logger di esempio per l'applicazione Web:
   ```Javascript
    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish

    var logger = ibmmfpfanalytics.logger.pkg("loggerName");
    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug message");
    logger.info("info message");

    // Send logs to the Mobile Analytics
    ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

   Per l'SDK Web, il livello non può essere impostato dal client. Tutta la registrazione viene inviata al server finché la configurazione non viene modificata richiamando il profilo di configurazione del server.
   {: note}
   {: web}

6. Per ottenere informazioni approfondite sui modelli di onboarding degli utenti (nuovi utenti rispetto a quelli che ritornano), devi associare un'identità utente alla tua sessione dell'applicazione.  Tale operazione può essere eseguita richiamando la seguente API
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    Tutti i dati di analisi acquisiti e archiviati localmente vengono inviati al servizio Mobile Analytics solo quando viene richiamata l'API ibmmfpfanalytics.send().
    {: web}

7.  Per ottenere informazioni approfondite sui modelli di interazione della rete HTTP delle tue applicazioni, come ad esempio le API HTTP richiamate, il numero di richieste e i tempi di risposta medi, devi utilizzare la classe WLResourceRequest dell'SDK client del servizio Mobile Foundation per effettuare le chiamate HTTP.  Anche se hai inizializzato l'analisi per l'acquisizione degli eventi di rete, l'utilizzo di  `WLResourceRequest` abilita Mobile Analytics ad agganciarsi alle chiamate di rete e ad acquisire i dati pertinenti.  Effettua le tue chiamate HTTP utilizzando il seguente codice.
    ```Javascript
      var resourceRequest = new WL.ResourceRequest("url_path", WLResourceRequest.GET);
      resourceRequest.send().then(function(response) {
            console.log('handle success' + JSON.stringify(data));
        },function(error){
            console.log('handle failure: ' + error);
        });
    ```
    {: codeblock}
    {: web}

8.  Per definire l'analisi personalizzata e definire i tuoi dati di analisi oltre quanto supportato inerentemente nell'SDK client, puoi utilizzare l'API di registrazione personalizzata:

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    I dati personalizzati registrati possono essere tracciati su grafici personalizzati che puoi definire nella console Mobile Analytics per ricavare informazioni approfondite personalizzate.
    {: web}

## Cosa fare ora?
{: #next_steps_analytics}

Prova un facile esempio da [qui](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples).  In meno di 5 minuti puoi eseguire l'applicazione di esempio e farti un'idea di cosa fa l'API. Puoi anche vedere come vengono mostrate le analisi come informazioni approfondite diverse nella console Analytics.  

Mobile Foundation Analytics Service fornisce le API REST per aiutare gli sviluppatori con l'importazione (POST) e l'esportazione (GET) di dati di analisi.

Prova l'API REST di analisi sui documenti Swagger da [qui](https://ma-server.us-south.mobile-analytics-prod.cloud.ibm.com/analytics-service/).
