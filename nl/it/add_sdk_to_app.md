---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Mobile Foundation SDK, android sdk, iOS sdk, cordova sdk, react native sdk

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
{:reactnative: .ph data-hd-programlang='reactnative'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}

#	Aggiungi l'SDK Mobile Foundation a un'applicazione
{: #add_sdk_to_app}

### Aggiunta dell'SDK Android alla tua applicazione
{: android}

Apri Android Studio, scegli la vista Android e scegli **Gradle Scripts**. Seleziona il file `build.gradle (Module: app)`, quindi esegui i seguenti passi per aggiungere l'SDK Android alla tua applicazione android.
{: android}

1. Aggiungi la seguente riga alla sezione `dependencies`.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. Aggiungi la seguente riga alla sezione `android`.
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. Nella vista Android, apri il file **app → manifests → AndroidManifest.xml**. Aggiungi le seguenti autorizzazioni prima dell'elemento `application`.
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. Aggiungi l'attività dell'IU MobileFirst accanto all'elemento `activity` esistente.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### Adding the iOS SDK to your app
{: ios}

Questo SDK è l'SDK di base per IBM Mobile Foundation che consiste nelle API per l'implementazione di sicurezza, autorizzazione, registrazione, richiamo di adattatori e altre funzioni di base di Mobile Foundation. Esegui i seguenti passi per aggiungere l'SDK iOS alla tua applicazione iOS.
{: ios}

1. Vai alla cartella root della tua applicazione iOS ed esegui il seguente comando per creare un Podfile.
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. Utilizzando il tuo editor di codice preferito, apri il Podfile.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. Aggiorna il pod utilizzando il seguente comando.
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. Installa il pod utilizzando il seguente comando.
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. Apri [ProjectName].xcworkspace per aprire il progetto in Xcode.
6. Aggiungi il file `mfpclient.plist` allo spazio di lavoro Xcode.
7. Da un prompt di comandi, vai alla cartella del progetto e registra la tua applicazione presso la tua istanza Mobile Foundation.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: ios}

### Aggiunta dell'SDK Cordova alla tua applicazione
{: cordova}

Questo SDK è l'SDK di base per IBM Mobile Foundation che consiste nelle API per l'implementazione di sicurezza, autorizzazione, registrazione, richiamo di adattatori e altre funzioni di base di Mobile Foundation. Esegui i seguenti passi per aggiungere l'SDK Cordova alla tua applicazione.
{: cordova}

1. Crea un progetto Cordova.
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. Cambia la directory e passa alla root del progetto Cordova.
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Aggiungi una o più piattaforme supportate al progetto Cordova utilizzando la CLI Cordova.
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Aggiungi il plug-in Cordova di base di Mobile Foundation.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. Prepara le risorse dell'applicazione eseguendo il seguente comando.
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Registra l'applicazione presso il server Mobile Foundation.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### Aggiunta del plug-in SDK React Native alla tua applicazione
{: reactnative}

Per aggiungere le funzionalità di Mobile Foundation a un'applicazione React Native esistente, devi aggiungere il plug-in `react-native-ibm-mobilefirst` alla tua applicazione. Il plug-in `react-native-ibm-mobilefirst` contiene l'SDK Mobile Foundation. Esegui i seguenti passi per aggiungere il plug-in React Native alla tua applicazione React Native.
{: reactnative}

1. Aggiungi questo plug-in nello stesso modo in cui aggiungi qualsiasi altro plug-in `npm` alla tua applicazione.
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. Il primo passo consiste nel creare un progetto React Native, ad esempio *MobileFirstApp*. Utilizza la CLI React Native per creare un nuovo progetto.
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. Aggiungi quindi il plug-in React Native alla tua applicazione.
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. Collega il tuo progetto in modo che tutte le dipendenze native vengano aggiunte al tuo progetto React Native.
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. Per Android, apporta le seguenti modifiche a `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`).
   ```xml
   <manifest
          xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. Aggiungi **tools:replace='android:allowBackup'** alla tag `application`.
   ```xml
   <application
            android:name=".MainApplication"
            android:label="@string/app_name"
            android:icon="@mipmap/ic_launcher"
            android:allowBackup="false"
            android:theme="@style/AppTheme"
            tools:replace="android:allowBackup">
   ```
   {: codeblock}
   {: reactnative}
7. Per iOS, apri XCode, nel navigator del progetto trascina e rilascia `mfpclient.plist` dalla cartella `ios`. Questo passo è applicabile solo per la piattaforma iOS.
{: reactnative}
