---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, set up android app for notification, set up iOS app for notification, set up cordova app for notification, set up windows app for notification

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
{:windows: .ph data-hd-programlang='Windows'}


# Gestione delle notifiche di push nelle applicazioni client
{: #handling_push_notifications_in_client_applications}

Prima che le applicazioni iOS, Android e quelle native basate su Windows o su Cordova possano ricevere e visualizzare le notifiche di push in entrata, è necessario innanzitutto configurare l'applicazione e implementare le API.
{: shortdesc}

Fai riferimento alle seguenti sezioni per informazioni su come gestire le notifiche di push in entrata nelle applicazioni client:

### Gestione delle notifiche di push in Android
{: #handling_push_notifications_in_android}
{: android}
Prima che le applicazioni Android possano gestire tutte le notifiche di push ricevute, è necessario configurare i servizi Google Play. Una volta configurata un'applicazione, puoi utilizzare l'API di notifiche fornita da {{ site.data.keyword.mobilefirst_notm }} per registrare e annullare la registrazione dei dispositivi e per sottoscrivere e annullare la sottoscrizione a tag. In questa esercitazione, apprenderai come gestire le notifiche di push nelle applicazioni Android.
{: android}

#### Prerequisiti
{: #prereqs-andriod}
{: android}
* {{ site.data.keyword.mfserver_short_notm }} da eseguire localmente o un {{ site.data.keyword.mfserver_short_notm }} in esecuzione in remoto.
* CLI {{ site.data.keyword.mobilefirst_notm  }} installata sulla workstation dello sviluppatore
{: android}

#### Configurazione delle notifiche
{: #notifications-configuration }
{: android}

Crea un nuovo progetto Android Studio oppure utilizzane uno esistente.  
Se l'SDK Android nativo {{ site.data.keyword.mobilefirst_notm }} non è ancora presente nel progetto, segui le istruzioni presenti nell'esercitazione [Aggiunta dell'SDK {{ site.data.keyword.mobilefoundation_short }} alle applicazioni Android](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app).
{: android}

##### Configurazione del progetto
{: #project-setup }
{: android}

1. In **Android → Gradle scripts**, seleziona il file **build.gradle (Module: app)** e aggiungi le seguenti righe a `dependencies`:
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    Esiste un [difetto noto di Google](https://code.google.com/p/android/issues/detail?id=212879) che impedisce l'uso della versione più recente dei servizi Play (attualmente alla versione 9.2.0). Utilizza una versione precedente alla 9.2.0.
    {: note}
    {: android}

    E:
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    Oppure in una sola riga:
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. In **Android → app → manifests**, apri il file `AndroidManifest.xml`.
    * Aggiungi le seguenti autorizzazioni prima della tag `manifest`:

        ```xml
        <!-- Permissions -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

        <!-- GCM Permissions -->
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <permission
    	     android:name="your.application.package.name.permission.C2D_MESSAGE"
    	     android:protectionLevel="signature" />
        ```
        {: codeblock}
        {: android}

    * Aggiungi quanto segue alla tag `application`:

        ```xml
        <!-- GCM Receiver -->
        <receiver
             android:name="com.google.android.gms.gcm.GcmReceiver"
             android:exported="true"
             android:permission="com.google.android.c2dm.permission.SEND">
             <intent-filter>
                 <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                 <category android:name="your.application.package.name" />
             </intent-filter>
        </receiver>

        <!-- MFPPush Intent Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.c2dm.intent.RECEIVE" />
              </intent-filter>
        </service>

        <!-- MFPPush Instance ID Listener Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.gms.iid.InstanceID" />
              </intent-filter>
        </service>

        <activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler"
             android:theme="@android:style/Theme.NoDisplay"/>
 	    ```
      {: codeblock}
      {: android}

      Assicurati di sostituire `your.application.package.name` con il nome del pacchetto effettivo della tua applicazione.
      {: note}
      {: android}

    * Aggiungi il seguente `intent-filter` all'attività dell'applicazione.
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### API di notifiche
{: #notifications-api }
{: android}

##### Istanza MFPPush
{: #mfppush-instance }
{: android}

Tutte le chiamate API devono essere richiamate su un'istanza di `MFPPush`.  Tale operazione può essere eseguita creando un campo di livello di classe come `private MFPPush push = MFPPush.getInstance();` e richiamando poi `push.<api-call>` in tutta la classe.
{: android}

In alternativa, puoi richiamare `MFPPush.getInstance().<api_call>` per ciascuna istanza in cui devi accedere ai metodi API push.
{: android}

##### Gestori delle verifiche
{: #challenge-handlers }
{: android}

Se l'ambito `push.mobileclient` viene associato a un **controllo di sicurezza**, devi assicurarti che esistano **gestori delle verifiche** corrispondenti e che vengano registrati prima di utilizzare una qualsiasi delle API push.
{: android}

Acquisisci ulteriori informazioni sui gestori delle verifiche nell'esercitazione [sulla convalida delle credenziali ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/).
{: note}
{: android}

##### Lato client
{: #client-side }
{: android}

| Metodi Java | Descrizione |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | Inizializza MFPPush per il contesto fornito. |
| [`isPushSupported();`](#is-push-supported) | Fa in modo che il dispositivo supporti le notifiche di push. |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | Registra il dispositivo con Push Notifications Service. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | Richiama le tag disponibili in un'istanza Push Notifications Service. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | Sottoscrive il dispositivo alle tag specificate. |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | Richiama tutte le tag a cui è attualmente sottoscritto il dispositivo. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | Annulla la sottoscrizione a particolari tag. |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | Annulla la registrazione del dispositivo a Push Notifications Service. |
{: caption="Tabella 1. Metodi Java" caption-side="top"}
{: android}

###### Inizializzazione
{: #initialization}
{: android}

È necessaria affinché l'applicazione si colleghi al servizio MFPPush con il contesto di applicazione corretto.
{: android}

* Il metodo API deve essere richiamato per primo, prima di utilizzare qualsiasi altra API MFPPush.
* Registra la funzione di callback per gestire le notifiche di push ricevute.
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### Se push è supportato
{: #is-push-supported }
{: android}

Controlla se il dispositivo supporta le notifiche di push.
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: android}

###### Registra il dispositivo
{: #register-device }
{: android}

Registra il dispositivo nel Push Notifications Service.
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        // Successfully registered
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Registration failed with error
    }
});
```
{: codeblock}
{: android}

###### Ottieni le tag
{: #get-tags }
{: android}

Richiama tutte le tag disponibili da Push Notifications Service.
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully retrieved tags as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to receive tags with error
    }
});
```
{: codeblock}
{: android}

###### Sottoscrivi
{: #subscribe }
{: android}

Esegui la sottoscrizione alle tag desiderate.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Subscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to subscribe
    }
});
```
{: codeblock}
{: android}

###### Ottieni le sottoscrizioni
{: #get-subscriptions }
{: android}

Richiama le tag a cui il dispositivo è attualmente sottoscritto.
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully received subscriptions as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to retrieve subscriptions with error
    }
});
```
{: codeblock}
{: android}

###### Annulla sottoscrizione
{: #unsubscribe }
{: android}

Annulla la sottoscrizione alle tag.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Unsubscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unsubscribe
    }
});
```
{: codeblock}
{: android}

###### Annulla la registrazione
{: #unregister }
{: android}

Annulla la registrazione del dispositivo all'istanza Push Notifications Service.
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Unregistered successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unregister
    }
});
```
{: codeblock}
{: android}

#### Gestione di una notifica di push
{: #handling-a-push-notification }
{: android}

Per poter gestire una notifica di push, dovrai configurare `MFPPushNotificationListener`.  Questa operazione può essere eseguita implementando uno dei seguenti metodi.
{: android}

##### Opzione uno
{: #option-one }
{: android}

Nell'attività in cui desideri gestire le notifiche di push.
{: android}

1. Aggiungi `implements MFPPushNofiticationListener` alla dichiarazione della classe.
2. Imposta la classe in modo che sia il listener richiamando `MFPPush.getInstance().listen(this)` nel metodo `onCreate`.
2. Poi dovrai aggiungere il seguente metodo *richiesto*:
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. In questo metodo riceverai `MFPSimplePushNotification` e puoi gestire la notifica per il comportamento desiderato.
{: android}

##### Opzione due
{: #option-two }
{: android}

Crea un listener richiamando `listen(new MFPPushNofiticationListener())` su un'istanza di `MFPPush` come descritto nel seguente esempio:
```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Handle push notification here
    }
});
```
{: codeblock}
{: android}

#### Migra le tue applicazioni client su Android a FCM
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) è stato [dichiarato obsoleto](https://developers.google.com/cloud-messaging/faq) ed è integrato con Firebase Cloud Messaging (FCM). Google disattiverà la maggior parte dei servizi GCM entro Aprile 2019.
{: android}

Se stai utilizzando un progetto GCM, [migra le applicazioni client GCM su Android a FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: android}

Per ora, le applicazioni esistenti che utilizzano i servizi GCM continueranno a funzionare così come sono. Poiché Push Notifications Service è stato aggiornato per utilizzare gli endpoint FCM, andando avanti, tutte le nuove applicazioni devono utilizzare FCM.
{: android}

Dopo aver eseguito la migrazione a FCM, aggiorna il tuo progetto per utilizzare le credenziali FCM invece delle vecchie credenziali GCM.
{: note}
{: android}

##### Configurazione del progetto FCM
{: #fcm_project_setup}
{: android}

La configurazione di un'applicazione in FCM è un po' diversa da quella del vecchio modello GCM.
{: android}

1. Ottieni le tue credenziali del provider di notifiche, crea un progetto FCM e aggiungi lo stesso alla tua applicazione Android. Includi il nome pacchetto della tua applicazione come `com.ibm.mobilefirstplatform.clientsdk.android.push`. Fai riferimento alla [documentazione qui](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android), fino al passo in cui hai terminato la creazione del file `google-services.json`
   {: android}
2. Configura il tuo file Gradle. Aggiungi quanto segue nel file `build.gradle` dell'applicazione
    ```xml
    dependencies {
       ......
       compile 'com.google.firebase:firebase-messaging:10.2.6'
       .....
    }

    apply plugin: 'com.google.gms.google-services'
    ```
    {: codeblock}
    {: android}

    - Aggiungi la seguente dipendenza alla sezione `buildscript` del build.gradle root
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - Rimuovi il plugin GCM riportato di seguito dal file build.gradle, `compile  com.google.android.gms:play-services-gcm:+`
     {: android}
 3. Configura il file AndroidManifest. Le modifiche riportate di seguito sono necessarie nel file `AndroidManifest.xml`
    {: android}

    Rimuovi le seguenti voci:
    {: android}
    ```xml
        <receiver android:exported="true" android:name="com.google.android.gms.gcm.GcmReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="your.application.package.name" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <category android:name="your.application.package.name" />
            </intent-filter>
        </receiver>  

        <service android:exported="false" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID" />
            </intent-filter>
        </service>

        <uses-permission android:name="your.application.package.name.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```
    {: codeblock}
    {: android}

    Le seguenti voci devono essere modificate::
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    Modifica le voci con:
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    Aggiungi la seguente voce:
    {: android}

    ```xml
        <service android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPush"
                android:exported="true">
                <intent-filter>
                    <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
                </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

 4. Apri l'applicazione in Android Studio. Copia il file `google-services.json` che hai creato in **step-1** nella directory dell'applicazione. Tieni presente che il file `google-service.json` include il nome pacchetto che hai aggiunto.		
 5. Compila l'SDK. Crea l'applicazione.
{: android}

### Gestione delle notifiche di push in iOS
{: #handling_push_notifications_in_ios }
{: ios}

Le API di notifiche fornite da {{ site.data.keyword.mobilefirst_notm }} possono essere utilizzate per registrare e annullare la registrazione dei dispositivi e per sottoscrivere e annullare la sottoscrizione a tag. In questa esercitazione, apprenderai come gestire le notifiche di push nelle applicazioni iOS utilizzando Swift.
{: shortdesc}
{: ios}

Per informazioni sulle notifiche silenziose o su quelle interattive, vedi:

* [Notifiche silenziose](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notifiche interattive](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### Prerequisiti
{: #prereqs-ios}
{: ios}

* {{ site.data.keyword.mfserver_short }} da eseguire localmente o un {{ site.data.keyword.mfserver_short }} in esecuzione in remoto.
* {{ site.data.keyword.mfp_cli_long_notm }} installato sulla workstation dello sviluppatore
{: ios}

#### Configurazione delle notifiche
{: #notifications-configuration_ios}
{: ios}

Crea un nuovo progetto Xcode o utilizzane uno esistente.
Se l'SDK iOS nativo {{ site.data.keyword.mobilefirst_notm }} non è ancora presente nel progetto, segui le istruzioni presenti nell'esercitazione [Aggiunta dell'SDK {{ site.data.keyword.mobilefoundation_short }} alle applicazioni iOS](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: ios}

#### Aggiunta dell'SDK push
{: #adding-the-push-sdk }
{: ios}

1. Apri il **podfile** esistente del progetto e aggiungi le seguenti righe:
    ```xml
    use_frameworks!

    platform :ios, 8.0
    target "Xcode-project-target" do
         pod 'IBMMobileFirstPlatformFoundation'
         pod 'IBMMobileFirstPlatformFoundationPush'
    end

    post_install do |installer|
         workDir = Dir.pwd

         installer.pods_project.targets.each do |target|
             debugXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.debug.xcconfig"
             xcconfig = File.read(debugXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(debugXcconfigFilename, "w") { |file| file << newXcconfig }

             releaseXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.release.xcconfig"
             xcconfig = File.read(releaseXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(releaseXcconfigFilename, "w") { |file| file << newXcconfig }
         end
    end
    ```
    {: codeblock}
    {: ios}

    - Sostituisci **Xcode-project-target** con il nome della destinazione del tuo progetto Xcode.
2. Salva e chiudi il **podfile**.
3. Da una finestra di **riga di comando**, vai alla cartella root del progetto.
4. Esegui il comando `pod install`
5. Apri il progetto utilizzando il file **.xcworkspace**.
{: ios}

#### API di notifiche
{: #notifications-api-ios}
{: ios}

##### Istanza MFPPush
{: #mfppush-instance-ios}
{: ios}

Tutte le chiamate API devono essere richiamate su un'istanza di `MFPPush`. Questa operazione può essere eseguita utilizzando una `var` in un controller delle viste, ad esempio `var push = MFPPush.sharedInstance();`, e richiamando poi `push.methodName()` in tutto il controller delle viste.
{: ios}

In alternativa, puoi richiamare `MFPPush.sharedInstance().methodName()` per ciascuna istanza in cui devi accedere ai metodi API push.
{: ios}

#### Gestori delle verifiche
{: #challenge-handlers-ios}
{: ios}

Se l'ambito `push.mobileclient` viene associato a un **controllo di sicurezza**, devi assicurarti che esistano **gestori delle verifiche** corrispondenti e che vengano registrati prima di utilizzare una qualsiasi delle API push.
{: ios}

Acquisisci ulteriori informazioni sui gestori delle verifiche nell'esercitazione [sulla convalida delle credenziali ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/).
{: note}
{: ios}

#### Lato client
{: #client-side-ios}
{: ios}

| Metodi Swift | Descrizione  |
|---------------|--------------|
| [`initialize()`](#initialization) | Inizializza MFPPush per il contesto fornito. |
| [`isPushSupported()`](#is-push-supported) | Fa in modo che il dispositivo supporti le notifiche di push. |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | Registra il dispositivo con Push Notifications Service.|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | Invia il token dispositivo al server |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | Richiama le tag disponibili in un'istanza Push Notifications Service. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | Sottoscrive il dispositivo alle tag specificate. |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | Richiama tutte le tag a cui è attualmente sottoscritto il dispositivo. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | Annulla la sottoscrizione a particolari tag. |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | Annulla la registrazione del dispositivo a Push Notifications Service.              |
{: caption="Tabella 2. Metodi Swift" caption-side="top"}
{: ios}

##### Inizializzazione
{: #initialization-ios}
{: ios}

L'inizializzazione è necessaria affinché l'applicazione client si colleghi al servizio MFPPush.
{: ios}

* Il metodo `initialize` deve essere richiamato per primo, prima di utilizzare qualsiasi altra API MFPPush.
* Registra la funzione di callback per gestire le notifiche di push ricevute.
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### Se push è supportato
{: #is-push-supported-ios}
{: ios}

Controlla se il dispositivo supporta le notifiche di push.
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: ios}

##### Registra il dispositivo e invia il token dispositivo
{: #register-device--send-device-token-ios}
{: ios}

Registra il dispositivo nel Push Notifications Service.
{: ios}

```swift
MFPPush.sharedInstance().registerDevice(nil) { (response, error) -> Void in
    if error == nil {
        self.enableButtons()
        self.showAlert("Registered successfully")
        print(response?.description ?? "")
    } else {
        self.showAlert("Registrations failed.  Error \(error?.localizedDescription)")
        print(error?.localizedDescription ?? "")
    }
}
```
{: codeblock}
{: ios}

<!--`options` = `[NSObject : AnyObject]` which is an optional parameter that is a dictionary of options to be passed with your register request, sends the device token to the server to register the device with its unique identifier.-->

```swift
MFPPush.sharedInstance().sendDeviceToken(deviceToken)
```
{: codeblock}
{: ios}

Di norma, questo viene richiamato in **AppDelegate** nel metodo `didRegisterForRemoteNotificationsWithDeviceToken`.
{: note}
{: ios}

##### Ottieni le tag
{: #get-tags-ios}
{: ios}

Richiama tutte le tag disponibili da Push Notifications Service.
{: ios}

```swift
MFPPush.sharedInstance().getTags { (response, error) -> Void in
    if error == nil {
        print("The response is: \(response)")
        print("The response text is \(response?.responseText)")
        if response?.availableTags().isEmpty == true {
            self.tagsArray = []
            self.showAlert("There are no available tags")
        } else {
            self.tagsArray = response!.availableTags() as! [String]
            self.showAlert(String(describing: self.tagsArray))
            print("Tags response: \(response)")
        }
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Sottoscrivi
{: #subscribe-ios}
{: ios}

Esegui la sottoscrizione alle tag desiderate.
{: ios}

```swift
var tagsArray: [String] = ["Tag 1", "Tag 2"]

MFPPush.sharedInstance().subscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Subscribed successfully")
        print("Subscribed successfully response: \(response)")
    } else {
        self.showAlert("Failed to subscribe")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Ottieni le sottoscrizioni
{: #get-subscriptions-ios}
{: ios}

Richiama le tag a cui il dispositivo è attualmente sottoscritto.
{: ios}

```swift
MFPPush.sharedInstance().getSubscriptions { (response, error) -> Void in
   if error == nil {
       var tags = [String]()
       let json = (response?.responseJSON)! as [AnyHashable: Any]
       let subscriptions = json["subscriptions"] as? [[String: AnyObject]]
       for tag in subscriptions! {
           if let tagName = tag["tagName"] as? String {
               print("tagName: \(tagName)")
               tags.append(tagName)
           }
       }
       self.showAlert(String(describing: tags))
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

##### Annulla sottoscrizione
{: #unsubscribe-ios}
{: ios}

Annulla la sottoscrizione alle tag.
{: ios}

```swift
var tags: [String] = {"Tag 1", "Tag 2"};

// Unsubscribe from tags
MFPPush.sharedInstance().unsubscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Unsubscribed successfully")
        print(String(describing: response?.description))
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Annulla la registrazione
{: #unregister-ios}
{: ios}

Annulla la registrazione del dispositivo all'istanza Push Notifications Service.
{: ios}

```swift
MFPPush.sharedInstance().unregisterDevice { (response, error)  -> Void in
   if error == nil {
       // Disable buttons
       self.disableButtons()
       self.showAlert("Unregistered successfully")
       print("Subscribed successfully response: \(response)")
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

#### Gestione di una notifica di push
{: #handling-a-push-notification-ios}
{: ios}

Le notifiche di push vengono gestite direttamente dal framework iOS nativo. A seconda del ciclo di vita della tua applicazione, il framework iOS richiamerà metodi diversi.
{: ios}

Ad esempio, se viene ricevuta una notifica semplice mentre l'applicazione è in esecuzione, verrà attivato `didReceiveRemoteNotification` di **AppDelegate**:
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // display the alert body
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

Acquisisci ulteriori informazioni sulla gestione delle notifiche in iOS nella [documentazione Apple ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).
{: note}
{: ios}

### Gestione delle notifiche di push in Cordova
{: #handling_push_notifications_in_cordova }
{: cordova}

Prima che le applicazioni iOS, Android e Windows Cordova possano ricevere e visualizzare le notifiche di push, è necessario aggiungere il plugin Cordova **cordova-plugin-mfp-push** al progetto Cordova. Una volta configurata un'applicazione, puoi utilizzare l'API di notifiche fornita da {{ site.data.keyword.mobilefirst_notm }} per registrare e annullare la registrazione dei dispositivi, per sottoscrivere e annullare la sottoscrizione a tag e per gestire le notifiche. In questa esercitazione, apprenderai come gestire le notifiche di push nelle applicazioni Cordova.
{: shortdesc}
{: cordova}

Al momento, le notifiche autenticate **non sono supportate** nelle applicazioni Cordova a causa di un difetto. Tuttavia, viene fornita una soluzione temporanea: ciascuna chiamata API `MFPPush` può essere impacchettata da `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );`.
{: note}
{: cordova}

Per informazioni sulle notifiche silenziose o su quelle interattive in iOS, vedi:
{: cordova}

* [Notifiche silenziose](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notifiche interattive](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### Prerequisiti
{: #prereqs-cordova}
{: cordova}

* {{ site.data.keyword.mfserver_short }} da eseguire localmente o un {{ site.data.keyword.mfserver_short }} in esecuzione in remoto
* {{ site.data.keyword.mfp_cli_long_notm }}  installato sulla workstation dello sviluppatore
* CLI Cordova installata sulla workstation dello sviluppatore
{: cordova}

#### Configurazione delle notifiche
{: #notifications-configuration-cordova}
{: cordova}

Crea un nuovo progetto Cordova, o utilizzane uno esistente, e aggiungi una o più piattaforme supportate: iOS, Android, Windows.
{: cordova}

Se l'SDK Cordova {{ site.data.keyword.mobilefirst_notm }} non è ancora presente nel progetto, segui le istruzioni presenti nell'esercitazione [Aggiunta dell'SDK {{ site.data.keyword.mobilefirst_notm }} alle applicazioni Cordova](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: cordova}
{: note}

#### Aggiunta del plug-in di push
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. Da una finestra della **riga di comando**, vai alla root del progetto Cordova.  
2. Aggiungi il plug-in di push eseguendo il comando:
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. Crea il progetto Cordova eseguendo il comando:
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### Piattaforma iOS
{: #ios-platform }
{: cordova}

La piattaforma iOS richiede un passo aggiuntivo.  
{: cordova}

In Xcode, abilita le notifiche di push per la tua applicazione nella schermata **Capabilities**.
{: cordova}

Il bundleId selezionato per l'applicazione deve corrispondere all'AppId che hai precedentemente creato nel sito Apple Developer.
{: important}
{: cordova}

![immagine della posizione della funzionalità in Xcode](images/push-capability.png)
{: cordova}

#### Piattaforma Android
{: #android-platform }
{: cordova}

La piattaforma Android richiede un passo aggiuntivo.  
{: cordova}

In Android Studio, aggiungi la seguente `activity` alla tag `application`:
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler"
             android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### API di notifiche
{: #notifications-api-cordova}
{: cordova}

##### Lato client
{: #client-side-cordova}
{: cordova}

| Funzione Javascript | Descrizione |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | Inizializza l'istanza MFPPush. |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) | Fa in modo che il dispositivo supporti le notifiche di push. |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) | Registra il dispositivo con Push Notifications Service. |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) | Richiama tutte le tag disponibili in un'istanza Push Notifications Service. |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) | Esegue la sottoscrizione a una particolare tag. |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions-cordova) | Richiama le tag a cui è attualmente sottoscritto il dispositivo |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) | Annulla la sottoscrizione a una particolare tag. |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | Annulla la registrazione del dispositivo a Push Notifications Service. |
{: caption="Tabella 3. Funzioni Javascript" caption-side="top"}
{: cordova}

##### Implementazione dell'API
{: #api-implementation }
{: cordova}

###### Inizializzazione
{: #initialization-cordova}
{: cordova}

Inizializza l'istanza **MFPPush**.
{: cordova}

- È necessaria affinché l'applicazione si colleghi al servizio MFPPush con il contesto di applicazione corretto.  
- Il metodo API deve essere richiamato per primo, prima di utilizzare qualsiasi altra API MFPPush.
- Registra la funzione di callback per gestire le notifiche di push ricevute.
{: cordova}

```javascript
MFPPush.initialize (
    function(successResponse) {
        alert("Successfully intialized");
        MFPPush.registerNotificationsCallback(notificationReceived);
    },
    function(failureResponse) {
        alert("Failed to initialize");
    }
);
```
{: codeblock}
{: cordova}

###### Se push è supportato
{: #is-push-supported-cordova}
{: cordova}

Controlla se il dispositivo supporta le notifiche di push.
{: cordova}

```javascript
MFPPush.isPushSupported (
    function(successResponse) {
        alert("Push Supported: " + successResponse);
    },
    function(failureResponse) {
        alert("Failed to get push support status");
    }
);
```
{: codeblock}
{: cordova}

###### Registra il dispositivo
{: #register-device-cordova}
{: cordova}

Registra il dispositivo nel Push Notifications Service. Se non sono richieste opzioni, possono essere impostate su `null`.
{: cordova}

```javascript
var options = { };
MFPPush.registerDevice(
    options,
    function(successResponse) {
        alert("Successfully registered");
    },
    function(failureResponse) {
        alert("Failed to register");
    }
);
```
{: codeblock}
{: cordova}

###### Ottieni le tag
{: #get-tags-cordova}
{: cordova}

Richiama tutte le tag disponibili da Push Notifications Service.
{: cordova}

```javascript
MFPPush.getTags (
    function(tags) {
        alert(JSON.stringify(tags));
},
    function() {
        alert("Failed to get tags");
    }
);
```
{: codeblock}
{: cordova}

###### Sottoscrivi
{: #subscribe-cordova}
{: cordova}

Esegui la sottoscrizione alle tag desiderate.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.subscribe(
    tags,
    function(tags) {
        alert("Subscribed successfully");
    },
    function() {
        alert("Failed to subscribe");
    }
);
```
{: codeblock}
{: cordova}

###### Ottieni le sottoscrizioni
{: #get-subscriptions-cordova}
{: cordova}

Richiama le tag a cui il dispositivo è attualmente sottoscritto.
{: cordova}

```javascript
MFPPush.getSubscriptions (
    function(subscriptions) {
        alert(JSON.stringify(subscriptions));
    },
    function() {
        alert("Failed to get subscriptions");
    }
);
```
{: codeblock}
{: cordova}

###### Annulla sottoscrizione
{: #unsubscribe-cordova}
{: cordova}

Annulla la sottoscrizione alle tag.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.unsubscribe(
    tags,
    function(tags) {
        alert("Unsubscribed successfully");
    },
    function() {
        alert("Failed to unsubscribe");
    }
);
```
{: codeblock}
{: cordova}

###### Annulla la registrazione
{: #unregister-cordova}
{: cordova}

Annulla la registrazione del dispositivo all'istanza Push Notifications Service.
{: cordova}

```javascript
MFPPush.unregisterDevice(
    function(successResponse) {
        alert("Unregistered successfully");
    },
    function() {
        alert("Failed to unregister");
    }
);
```
{: codeblock}
{: cordova}

#### Gestione di una notifica di push
{: #handling-a-push-notification-cordova}
{: cordova}

Puoi gestire una notifica di push ricevuta agendo sul suo oggetto di risposta nella funzione di callback registrata.
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Gestione delle notifiche di push in Windows 8.1 Universal e Windows 10 UWP
{: #handling_push_notifications_in_windows }
{: windows}

Le API di notifiche fornite da {{ site.data.keyword.mobilefirst_notm }} possono essere utilizzate per registrare e annullare la registrazione dei dispositivi e per sottoscrivere e annullare la sottoscrizione a tag. In questa esercitazione, apprenderai come gestire le notifiche di push nelle applicazioni native Windows 8.1 Universal eWindows 10 UWP utilizzando C#.
{: windows}

#### Prerequisiti
{: #prereqs-windows}
{: windows}

* {{ site.data.keyword.mfserver_short_notm }} da eseguire localmente o un {{ site.data.keyword.mfserver_short_notm }} in esecuzione in remoto.
* CLI {{ site.data.keyword.mobilefirst_notm  }} installata sulla workstation dello sviluppatore
{: windows}

#### Configurazione delle notifiche
{: #notifications-configuration-windows}
{: windows}

Crea un nuovo progetto Visual Studio oppure utilizzane uno esistente.  
{: windows}

Se l'SDK Windows nativo {{ site.data.keyword.mobilefirst_notm }} non è ancora presente nel progetto, segui le istruzioni presenti nell'esercitazione [Aggiunta dell'SDK {{ site.data.keyword.mobilefirst_notm }} alle applicazioni Windows](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/).
{: windows}

#### Aggiunta dell'SDK push
{: #adding-the-push-sdk-windows}
{: windows}

1. Seleziona Tools → NuGet Package Manager → Package Manager Console.
2. Scegli il progetto in cui desideri installare il componente push {{ site.data.keyword.mobilefirst_notm }}.
3. Aggiungi l'SDK push {{ site.data.keyword.mobilefirst_notm }} eseguendo il comando **Install-Package IBM.MobileFirstPlatformFoundationPush**.
{: windows}

#### Configurazione WNS prerequisita
{: pre-requisite-wns-configuration }
{: windows}

1. Assicurati che l'applicazione disponga della funzionalità di notifica Toast. Può essere abilitata in Package.appxmanifest.
2. Assicurati che `Package Identity Name` e `Publisher` debbano essere aggiornati con i valori registrati con WNS.
3. (Facoltativo) Elimina il file TemporaryKey.pfx.
{: windows}

#### API di notifiche
{: #notifications-api-windows}
{: windows}

##### Istanza MFPPush
{: #mfppush-instance-windows}
{: windows}

Tutte le chiamate API devono essere richiamate su un'istanza di `MFPPush`.  Questa operazione può essere eseguita creando una variabile come ad esempio `private MFPPush PushClient = MFPPush.GetInstance();` e richiamando poi `PushClient.methodName()` in tutta la classe.
{: windows}

In alternativa, puoi richiamare `MFPPush.GetInstance().methodName()` per ciascuna istanza in cui devi accedere ai metodi API push.
{: windows}

##### Gestori delle verifiche
{: #challenge-handlers-windows}
{: windows}

Se l'ambito `push.mobileclient` viene associato a un **controllo di sicurezza**, devi assicurarti che esistano **gestori delle verifiche** corrispondenti e che vengano registrati prima di utilizzare una qualsiasi delle API push.
{: windows}

Acquisisci ulteriori informazioni sui gestori delle verifiche nell'esercitazione [sulla convalida delle credenziali ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/).
{: note}
{: windows}

#### Lato client
{: #client-side-windows}
{: windows}

| Metodi C Sharp                                                                                                | Descrizione                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            | Inizializza MFPPush per il contesto fornito.                               |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    | Fa in modo che il dispositivo supporti le notifiche di push.                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  | Registra il dispositivo con Push Notifications Service.               |
| [`GetTags()`](#get-tags-windows)                                | Richiama le tag disponibili in un'istanza Push Notifications Service. |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     | Sottoscrive il dispositivo alle tag specificate.                          |
| [`GetSubscriptions()`](#get-subscriptions-windows)              | Richiama tutte le tag a cui è attualmente sottoscritto il dispositivo.               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) | Annulla la sottoscrizione a particolari tag.                                  |
| [`UnregisterDevice()`](#unregister-windows)                     | Annulla la registrazione del dispositivo a Push Notifications Service.              |
{: caption="Tabella 4. Metodi C Sharp" caption-side="top"}
{: windows}

##### Inizializzazione
{: #initialization-windows}
{: windows}

L'inizializzazione è necessaria affinché l'applicazione client si colleghi al servizio MFPPush.
{: windows}

* Il metodo `Initialize` deve essere richiamato per primo, prima di utilizzare qualsiasi altra API MFPPush.
* Registra la funzione di callback per gestire le notifiche di push ricevute.
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### Se push è supportato
{: #is-push-supported-windows}
{: windows}

Controlla se il dispositivo supporta le notifiche di push.
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: windows}

##### Registra il dispositivo e invia il token dispositivo
{: #register-device--send-device-token-windows}
{: windows}

Registra il dispositivo nel Push Notifications Service.
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
    }
```
{: codeblock}
{: windows}

##### Ottieni le tag
{: #get-tags-windows}
{: windows}

Richiama tutte le tag disponibili da Push Notifications Service.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetTags();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
} else{
    Message = new MessageDialog("Failed to get Tags list");
}
```
{: codeblock}
{: windows}

##### Sottoscrivi
{: #subscribe-windows}
{: windows}

Esegui la sottoscrizione alle tag desiderate.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Get subscription tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //successfully subscribed to push tag
}
else
{
    //failed to subscribe to push tags
}
```
{: codeblock}
{: windows}

##### Ottieni le sottoscrizioni
{: #get-subscriptions-windows}
{: windows}

Richiama le tag a cui il dispositivo è attualmente sottoscritto.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetSubscriptions();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
}
else
{
    Message = new MessageDialog("Failed to get subcription list...");
}
```
{: codeblock}
{: windows}

##### Annulla sottoscrizione
{: #unsubscribe-windows}
{: windows}

Annulla la sottoscrizione alle tag.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// unsubscribe tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //succes
}
else
{
    //failed to subscribe to tags
}
```
{: codeblock}
{: windows}

##### Annulla la registrazione
{: #unregister-windows}
{: windows}

Annulla la registrazione del dispositivo all'istanza Push Notifications Service.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
    }
```
{: codeblock}
{: windows}

#### Gestione di una notifica di push
{: #handling-a-push-notification-windows}
{: windows}

Per poter gestire una notifica di push, dovrai configurare `MFPPushNotificationListener`.  Questa operazione può essere eseguita implementando il seguente metodo.
{: windows}

1. Crea una classe utilizzando l'interfaccia di tipo MFPPushNotificationListener

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Handle push notification here      
    }
    }
    ```
    {: codeblock}
{: windows}
2. Imposta la classe in modo che sia il listener richiamando `MFPPush.GetInstance().listen(new NotificationListner())`
3. Nel metodo onReceive riceverai la notifica di push e potrai gestire la notifica per il comportamento desiderato.
{: windows}

#### Windows Universal Push Notifications Service
{: #windows-universal-push-notifications-service }
{: windows}

Nella tua configurazione server non devono essere aperte porte specifiche.
{: windows}

WNS utilizza le richieste http o https regolari.
{: windows}
