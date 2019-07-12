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


# Handhabung von Push-Benachrichtigungen in Clientanwendungen
{: #handling_push_notifications_in_client_applications}

Bevor native oder Cordova-basierte iOS-, Android- und Windows-Anwendungen Push-Benachrichtigungen empfangen und anzeigen können, muss die Anwendung eingerichtet und APIs implementiert werden.
{: shortdesc}

In den folgenden Abschnitten erfahren Sie, wie eingehende Push-Benachrichtigungen in Clientanwendungen behandelt werden:

### Handhabung von Push-Benachrichtigungen in Android
{: #handling_push_notifications_in_android}
{: android}
Sie müssen Unterstützung für Google Play Services konfigurieren, damit Android-Anwendungen empfangene Push-Benachrichtigungen verarbeiten können. Wenn eine Anwendung konfiguriert ist, kann die von {{ site.data.keyword.mobilefirst_notm }} bereitgestellte API für Benachrichtigungen verwendet werden, um Geräte zu registrieren &amp; Geräteregistrierungen aufzuheben und um Tags zu abonnieren &amp; Tagabonnements zu beenden. In diesem Lernprogramm erfahren Sie, wie Sie Push-Benachrichtigungen in Android-Anwendungen handhaben können.
{: android}

#### Voraussetzungen
{: #prereqs-andriod}
{: android}
* {{ site.data.keyword.mfserver_short_notm }} wird lokal ausgeführt oder Sie verfügen über eine remote ausgeführte {{ site.data.keyword.mfserver_short_notm }}-Instanz.
* Die {{ site.data.keyword.mobilefirst_notm  }}-CLI ist auf der Entwicklerworkstation installiert.
{: android}

#### Benachrichtigungskonfiguration
{: #notifications-configuration }
{: android}

Erstellen Sie ein neues Android Studio-Projekt oder verwenden Sie ein vorhandenes Projekt.  
Wenn das native {{ site.data.keyword.mobilefirst_notm }}-Android-SDK noch nicht im Projekt enthalten ist, folgen Sie den Anweisungen im Lernprogramm [{{ site.data.keyword.mobilefoundation_short }}-SDK zu Android-Anwendungen hinzufügen](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app).
{: android}

##### Projektkonfiguration
{: #project-setup }
{: android}

1. Wählen Sie unter **Android → Gradle-Scripts** die Datei **build.gradle (Module: app)** aus und fügen Sie die folgenden Zeilen zum Abschnitt `dependencies` hinzu:
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    Es gibt einen [bekannten Google-Fehler](https://code.google.com/p/android/issues/detail?id=212879), der die Verwendung der neuesten Play-Services-Version (zurzeit Version 9.2.0) verhindert. Verwenden Sie eine Version vor Version 9.2.0.
    {: note}
    {: android}

    Fügen Sie außerdem folgende Zeilen hinzu:
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    Oder eine einzelne Zeile:
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. Öffnen Sie unter **Android → App → Manifests** die Datei `AndroidManifest.xml`.
    * Fügen Sie am Anfang des Tags `manifest` die folgende Berechtigung hinzu:

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

    * Fügen Sie Folgendes zum Tag `application` hinzu:

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

      Sie müssen 'your.application.package.name' durch den Paketnamen Ihrer Anwendung ersetzen.
      {: note}
      {: android}

    * Fügen Sie den folgenden 'intent-filter' zur Aktivität der Anwendung hinzu.
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### API für Benachrichtigungen
{: #notifications-api }
{: android}

##### MFPPush-Instanz
{: #mfppush-instance }
{: android}

Alle API-Aufrufe müssen für eine Instanz von `MFPPush` aufgerufen werden.  Zu diesem Zweck können Sie ein Feld auf Klassenebene erstellen, z. B. `private MFPPush push = MFPPush.getInstance();`, und dann in der gesamten Klasse `push.<api-call>` aufrufen.
{: android}

Alternativ dazu können Sie `MFPPush.getInstance().<api_call>` für jede Instanz aufrufen, in der Sie auf die Push-API-Methoden zugreifen müssen.
{: android}

##### Abfrage-Handler
{: #challenge-handlers }
{: android}

Wenn der Bereich `push.mobileclient` einer **Sicherheitsprüfung** zugeordnet ist, müssen Sie sicherstellen, dass vor der Verwendung der Push-APIs passende **Abfrage-Handler** vorhanden und registriert sind.
{: android}

Weitere Informationen zu Abfrage-Handlern enthält das Lernprogramm [Berechtigungsnachweise validieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/).
{: note}
{: android}

##### Clientseite
{: #client-side }
{: android}

| Java-Methoden | Beschreibung |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | Initialisiert MFPPush für den angegebenen Kontext. |
| [`isPushSupported();`](#is-push-supported) | Unterstützt das Gerät Push-Benachrichtigungen? |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | Registriert das Gerät beim Push-Benachrichtigungsservice. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | Ruft die verfügbaren Tags einer Instanz des Push-Benachrichtigungsservice ab. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | Richtet das Geräteabonnement für die angegebenen Tags ein. |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | Ruft die derzeit vom Gerät abonnierten Tags ab. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | Beendet das Abonnement bestimmter Tags. |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | Hebt die Registrierung des Geräts beim Push-Benachrichtigungsservice auf. |
{: caption="Tabelle 1. Java-Methoden" caption-side="top"}
{: android}

###### Initialisierung
{: #initialization}
{: android}

Die Initialisierung ist erforderlich, damit die Clientanwendung mit dem richtigen Anwendungskontext eine Verbindung zum MFPPush-Service herstellen kann.
{: android}

* Die API-Methode sollte zuerst aufgerufen werden, bevor andere MFPPush-APIs verwendet werden.
* Die Callback-Funktion wird für die Handhabung empfangener Push-Benachrichtigungen registriert.
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### Wird Push unterstützt?
{: #is-push-supported }
{: android}

Es wird überprüft, ob das Gerät Push-Benachrichtigungen unterstützt.
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push wird unterstützt
} else {
    // Push wird nicht unterstützt
}
```
{: codeblock}
{: android}

###### Gerät registrieren
{: #register-device }
{: android}

Registrieren Sie das Gerät beim Push-Benachrichtigungsservice.
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<zeichenfolge>() {
    @Override
    public void onSuccess(String s) {
        // Erfolgreich registriert
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Registrierung mit Fehler fehlgeschlagen
    }
});
```
{: codeblock}
{: android}

###### Tags abrufen
{: #get-tags }
{: android}

Rufen Sie alle verfügbaren Tags vom Push-Benachrichtigungsservice ab.
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<liste<zeichenfolge>>() {
    @Override
    public void onSuccess(List<zeichenfolge> strings) {
        // Tags erfolgreich als Liste von Zeichenfolgen abgerufen
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Empfang von Tags mit Fehler fehlgeschlagen
    }
});
```
{: codeblock}
{: android}

###### Abonnieren
{: #subscribe }
{: android}

Abonnieren Sie die gewünschten Tags.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<zeichenfolge[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Abonnement erfolgreich
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Abonnement fehlgeschlagen
    }
});
```
{: codeblock}
{: android}

###### Abonnements abrufen
{: #get-subscriptions }
{: android}

Rufen Sie die derzeit vom Gerät abonnierten Tags ab.
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<liste<zeichenfolge>>() {
    @Override
    public void onSuccess(List<zeichenfolge> strings) {
        // Abonnements erfolgreich als Liste von Zeichenfolgen empfangen
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Abruf der Abonnements mit Fehler fehlgeschlagen
    }
});
```
{: codeblock}
{: android}

###### Abonnement beenden
{: #unsubscribe }
{: android}

Beenden Sie das Tagabonnement.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<zeichenfolge[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Beendigung des Abonnements erfolgreich
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Beendigung des Abonnements fehlgeschlagen
    }
});
```
{: codeblock}
{: android}

###### Registrierung aufheben
{: #unregister }
{: android}

Sie können die Registrierung des Geräts bei der Instanz des Push-Benachrichtigungsservice aufheben.
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<zeichenfolge>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Aufhebung der Registrierung erfolgreich
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Aufhebung der Registrierung fehlgeschlagen
    }
});
```
{: codeblock}
{: android}

#### Handhabung von Push-Benachrichtigungen
{: #handling-a-push-notification }
{: android}

Für die Handhabung von Push-Benachrichtigungen müssen Sie einen `MFPPushNotificationListener` einrichten.  Zu diesem Zweck können Sie eine der folgenden Methoden implementieren.
{: android}

##### Option 1
{: #option-one }
{: android}

Führen Sie in der Aktivität, in der Sie Push-Benachrichtigungen verarbeiten möchten, die folgenden Schritte aus.
{: android}

1. Fügen Sie `implements MFPPushNofiticationListener` zur Klassendeklaration hinzu.
2. Definieren Sie die Klasse als Listener, indem Sie `MFPPush.getInstance().listen(this)` in der Methode `onCreate` aufrufen.
2. Anschließend müssen Sie die folgende *erforderliche* Methode hinzufügen:
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Push-Benachrichtigungen hier verarbeiten
    }
    ```
    {: codeblock}
    {: android}

3. In dieser Methode empfangen Sie `MFPSimplePushNotification` und können für die Benachrichtigung das gewünschte Verhalten festlegen.
{: android}

##### Option 2
{: #option-two }
{: android}

Erstellen Sie einen Listener, indem Sie wie im folgende Beispiel dargestellt `listen(new MFPPushNofiticationListener())` für eine Instanz von `MFPPush` aufrufen:

```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Push-Benachrichtigungen hier verarbeiten
    }
});
```
{: codeblock}
{: android}

#### Clientanwendungen für Android auf FCM migrieren
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) wird [nicht weiter unterstützt](https://developers.google.com/cloud-messaging/faq) und wurde in Firebase Cloud Messaging (FCM) integriert. Google wird die meisten GCM-Services bis April 2019 einstellen.
{: android}

Wenn Sie ein GCM-Projekt verwenden, [migrieren Sie die GCM-Client-Apps für Android auf FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: android}

Momentan laufen die vorhandenen Anwendungen, die GCM-Services nutzen, unverändert weiter. Da der Push-Benachrichtigungsservice aktualisiert wurde und jetzt FCM-Endpunkte verwendet, müssen alle neuen Anwendungen FCM nutzen.
{: android}

Nachdem Sie die Migration auf FCM durchgeführt haben, aktualisieren Sie Ihr Projekt so, dass FCM-Berechtigungsnachweise anstelle der alten GCM-Berechtigungsnachweise verwendet werden.
{: note}
{: android}

##### Einrichtung eines FCM-Projekts
{: #fcm_project_setup}
{: android}

Die Einrichtung einer Anwendung in FCM unterscheidet sich vom alten GCM-Modell.
{: android}

1. Fordern Sie die Berechtigungsnachweise Ihres Benachrichtigungsproviders an, erstellen Sie ein FCM-Projekt und fügen Sie beides zu Ihrer Android-Anwendung hinzu. Schließen Sie den Paketnamen Ihrer Anwendung als `com.ibm.mobilefirstplatform.clientsdk.android.push` mit ein. Folgen Sie dann [hier der Dokumentation](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android) bis zu dem Schritt, in dem die Datei `google-services.json` erfolgreich erstellt wurde.
   {: android}
2. Konfigurieren Sie Ihre Gradle-Datei. Fügen Sie Folgendes zur Datei `build.gradle` der App hinzu.
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

    - Fügen Sie im Stammabschnitt `buildscript` der Datei 'build.gradle' die folgende Abhängigkeit hinzu:
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - Entfernen Sie `compile  com.google.android.gms:play-services-gcm:+` nach dem GCM-Plug-in aus der Datei 'build.gradle'
     {: android}
 3. Konfigurieren Sie die Datei 'AndroidManifest'. Die folgenden Änderungen sind in der Datei `AndroidManifest.xml` erforderlich:
    {: android}

    Entfernen Sie die folgenden Einträge:
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

    Die folgenden Einträge müssen geändert werden:
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

    Ändern Sie die Einträge wie folgt:
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

    Fügen Sie den folgenden Eintrag hinzu:
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

 4. Öffnen Sie die App in Android Studio. Kopieren Sie die Datei `google-services.json`, die Sie in **Schritt 1** erstellt haben, in das App-Verzeichnis. Beachten Sie, dass die Datei `google-service.json` den von Ihnen hinzugefügten Paketnamen enthält.		
 5. Kompilieren Sie das SDK. Erstellen Sie die Anwendung.
{: android}

### Handhabung von Pushbenachrichtigungen in iOS
{: #handling_push_notifications_in_ios }
{: ios}

Die von {{ site.data.keyword.mobilefirst_notm }} bereitgestellte API für Benachrichtigungen kann verwendet werden, um Geräte zu registrieren &amp; Geräteregistrierungen aufzuheben und um Tags zu abonnieren &amp; Tagabonnements zu beenden. In diesem Lernprogramm erfahren Sie, wie Sie Push-Benachrichtigungen in iOS-Anwendungen mit Swift handhaben können.
{: shortdesc}
{: ios}

Informationen zu Benachrichtigungen im Hintergrund und interaktiven Benachrichtigungen finden Sie in folgenden Abschnitten:

* [Benachrichtigungen im Hintergrund](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Interaktive Benachrichtigungen](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### Voraussetzungen
{: #prereqs-ios}
{: ios}

* {{ site.data.keyword.mfserver_short }} wird lokal ausgeführt oder Sie verfügen über eine remote ausgeführte {{ site.data.keyword.mfserver_short }}-Instanz.
* Die {{ site.data.keyword.mfp_cli_long_notm }} ist auf der Entwicklerworkstation installiert.
{: ios}

#### Benachrichtigungskonfiguration
{: #notifications-configuration_ios}
{: ios}

Erstellen Sie ein neues Xcode-Projekt oder verwenden Sie ein vorhandenes Xcode-Projekt.
Wenn das native {{ site.data.keyword.mobilefirst_notm }}-iOS-SDK noch nicht im Projekt enthalten ist, folgen Sie den Anweisungen im Lernprogramm [{{ site.data.keyword.mobilefoundation_short }}-SDK zu iOS-Anwendungen hinzufügen](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: ios}

#### Push-SDK hinzufügen
{: #adding-the-push-sdk }
{: ios}

1. Öffnen Sie die vorhandene **Podfile** des Projekts und fügen Sie die folgenden Zeilen hinzu:
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

    - Ersetzen Sie **Xcode-project-target** durch den Namen Ihres Xcode-Projektziels.
2. Speichern und schließen Sie die **Podfile**.
3. Navigieren Sie in einem **Befehlszeilenfenster** zum Stammordner des Projekts.
4. Führen Sie den Befehl `pod install` aus.
5. Öffnen Sie das Projekt mithilfe der Datei **.xcworkspace**.
{: ios}

#### API für Benachrichtigungen
{: #notifications-api-ios}
{: ios}

##### MFPPush-Instanz
{: #mfppush-instance-ios}
{: ios}

Alle API-Aufrufe müssen für eine Instanz von `MFPPush` aufgerufen werden. Zu diesem Zweck können Sie eine Variable `var` in einem Ansichtencontroller erstellen, z. B. `var push = MFPPush.sharedInstance();` und dann im gesamten Ansichtencontroller `push.methodName()` aufrufen.
{: ios}

Alternativ dazu können Sie `MFPPush.sharedInstance().methodName()` für jede Instanz aufrufen, in der Sie auf die Push-API-Methoden zugreifen müssen.
{: ios}

#### Abfrage-Handler
{: #challenge-handlers-ios}
{: ios}

Wenn der Bereich `push.mobileclient` einer **Sicherheitsprüfung** zugeordnet ist, müssen Sie sicherstellen, dass vor der Verwendung der Push-APIs passende **Abfrage-Handler** vorhanden und registriert sind.
{: ios}

Weitere Informationen zu Abfrage-Handlern enthält das Lernprogramm [Berechtigungsnachweise validieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/).
{: note}
{: ios}

#### Clientseite
{: #client-side-ios}
{: ios}

| Swift-Methoden | Beschreibung  |
|---------------|--------------|
| [`initialize()`](#initialization) | Initialisiert MFPPush für den angegebenen Kontext. |
| [`isPushSupported()`](#is-push-supported) | Unterstützt das Gerät Push-Benachrichtigungen? |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | Registriert das Gerät beim Push-Benachrichtigungsservice.|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | Sendet das Gerätetoken an den Server. |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | Ruft die verfügbaren Tags einer Instanz des Push-Benachrichtigungsservice ab. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | Richtet das Geräteabonnement für die angegebenen Tags ein. |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | Ruft die derzeit vom Gerät abonnierten Tags ab. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | Beendet das Abonnement bestimmter Tags. |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | Hebt die Registrierung des Geräts beim Push-Benachrichtigungsservice auf.              |
{: caption="Tabelle 2. Swift-Methoden" caption-side="top"}
{: ios}

##### Initialisierung
{: #initialization-ios}
{: ios}

Die Initialisierung ist erforderlich, damit die Clientanwendung eine Verbindung zum MFPPush-Service herstellen kann.
{: ios}

* Die Methode `initialize` sollte zuerst aufgerufen werden, bevor andere MFPPush-APIs verwendet werden.
* Die Callback-Funktion wird für die Handhabung empfangener Push-Benachrichtigungen registriert.
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### Wird Push unterstützt?
{: #is-push-supported-ios}
{: ios}

Es wird überprüft, ob das Gerät Push-Benachrichtigungen unterstützt.
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push wird unterstützt
} else {
    // Push wird nicht unterstützt
}
```
{: codeblock}
{: ios}

##### Gerät registrieren &amp; Gerätetoken senden
{: #register-device--send-device-token-ios}
{: ios}

Registrieren Sie das Gerät beim Push-Benachrichtigungsservice.
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

Dieser Aufruf erfolgt in der Regel in **AppDelegate** in der Methode `didRegisterForRemoteNotificationsWithDeviceToken`.
{: note}
{: ios}

##### Tags abrufen
{: #get-tags-ios}
{: ios}

Rufen Sie alle verfügbaren Tags vom Push-Benachrichtigungsservice ab.
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

##### Abonnieren
{: #subscribe-ios}
{: ios}

Abonnieren Sie die gewünschten Tags.
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

##### Abonnements abrufen
{: #get-subscriptions-ios}
{: ios}

Rufen Sie die derzeit vom Gerät abonnierten Tags ab.
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

##### Abonnement beenden
{: #unsubscribe-ios}
{: ios}

Beenden Sie das Tagabonnement.
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

##### Registrierung aufheben
{: #unregister-ios}
{: ios}

Sie können die Registrierung des Geräts bei der Instanz des Push-Benachrichtigungsservice aufheben.
{: ios}

```swift
MFPPush.sharedInstance().unregisterDevice { (response, error)  -> Void in
   if error == nil {
       // Schaltflächen zum Inaktivieren
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

#### Handhabung von Push-Benachrichtigungen
{: #handling-a-push-notification-ios}
{: ios}

Push-Benachrichtigungen werden direkt vom nativen iOS-Framework bearbeitet. Welche Methoden vom iOS-Framework aufgerufen werden, richtet sich nach Ihrem Anwendungslebenszyklus.
{: ios}

Wenn beispielsweise eine einfache Benachrichtigung empfangen wird, während die Anwendung aktiv ist, wird `didReceiveRemoteNotification` von **AppDelegate** ausgelöst:
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // Alerttext anzeigen
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

Weitere Informationen zur Handhabung von Benachrichtigungen unter iOS finden Sie in der [Apple-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).
{: note}
{: ios}

### Handhabung von Pushbenachrichtigungen in Cordova
{: #handling_push_notifications_in_cordova }
{: cordova}

Bevor iOS-, Android- und Windows-Cordova-Anwendungen Push-Benachrichtigungen empfangen und anzeigen können, muss das Cordova-Plug-in **cordova-plugin-mfp-push** zu dem Cordova-Projekt hinzugefügt werden. Wenn eine Anwendung konfiguriert ist, kann die {{ site.data.keyword.mobilefirst_notm }}-API für Benachrichtigungen verwendet werden, um Geräte zu registrieren &amp; Geräteregistrierungen aufzuheben, um Tags zu abonnieren &amp; Tagabonnements zu beenden und um Benachrichtigungen handhaben zu können. In diesem Lernprogramm erfahren Sie, wie Sie Push-Benachrichtigungen in Cordova-Anwendungen handhaben können.
{: shortdesc}
{: cordova}

Authentifizierte Benachrichtigungen werden in Cordova-Anwendungen aufgrund eines Fehlers derzeit **nicht unterstützt**. Es gibt jedoch eine Ausweichlösung: Jeder API-Aufruf `MFPPush` kann von `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );` eingeschlossen werden.
{: note}
{: cordova}

Informationen zu Benachrichtigungen im Hintergrund und interaktiven Benachrichtigungen in iOS finden Sie in folgenden Abschnitten:
{: cordova}

* [Benachrichtigungen im Hintergrund](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Interaktive Benachrichtigungen](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### Voraussetzungen
{: #prereqs-cordova}
{: cordova}

* {{ site.data.keyword.mfserver_short }} wird lokal ausgeführt oder Sie verfügen über eine remote ausgeführte {{ site.data.keyword.mfserver_short }}-Instanz.
* Die {{ site.data.keyword.mfp_cli_long_notm }} ist auf der Entwicklerworkstation installiert.
* Die Cordova-CLI ist auf der Entwicklerworkstation installiert.
{: cordova}

#### Benachrichtigungskonfiguration
{: #notifications-configuration-cordova}
{: cordova}

Erstellen Sie ein neues Cordova-Projekt oder verwenden Sie ein vorhandenes Cordova-Projekt. Fügen Sie mindestens eine der unterstützten Plattformen (iOS, Android, Windows) zu dem Projekt hinzu.
{: cordova}

Wenn das {{site.data.keyword.mobilefirst_notm }}-Cordova SDK nicht bereits im Projekt vorhanden ist, befolgen Sie die Anweisungen im Lernprogramm [{{ site.data.keyword.mobilefirst_notm }}-SDK zu Cordova-Anwendungen hinzufügen](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: cordova}
{: note}

#### Push-Plug-in hinzufügen
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. Navigieren Sie in einem **Befehlszeilenfenster** zu dem Stammverzeichnis des Cordova-Projekts.  
2. Fügen Sie das Push-Plug-in hinzu, indem Sie den folgenden Befehl ausführen:
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. Erstellen Sie das Cordova-Projekt, indem Sie den folgenden Befehl ausführen:
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### iOS-Plattform
{: #ios-platform }
{: cordova}

Für die iOS-Plattform ist ein zusätzlicher Schritt erforderlich.  
{: cordova}

In Xcode müssen Sie Push-Benachrichtigungen für Ihre Anwendung in der Anzeige für die **Funktionalität** aktivieren.
{: cordova}

Die für die Anwendung ausgewählte Bundle-ID (bundleId) muss mit der App-ID (AppId) übereinstimmen, die Sie zuvor auf der Apple-Developer-Site erstellt haben.
{: important}
{: cordova}

![Abbildung mit Position der Funktionalität in Xcode](images/push-capability.png)
{: cordova}

#### Android-Plattform
{: #android-platform }
{: cordova}

Für die Android-Plattform ist ein zusätzlicher Schritt erforderlich.  
{: cordova}

Fügen Sie in Android Studio die folgende Aktivität `activity` zum Tag `application` hinzu:
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### API für Benachrichtigungen
{: #notifications-api-cordova}
{: cordova}

##### Clientseite
{: #client-side-cordova}
{: cordova}

| Javascript-Funktion | Beschreibung |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | Initialisieren Sie die MFPPush-Instanz. |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) | Unterstützt das Gerät Push-Benachrichtigungen? |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) | Registriert das Gerät beim Push-Benachrichtigungsservice. |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) | Ruft alle verfügbaren Tags einer Instanz des Push-Benachrichtigungsservice ab. |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) | Abonniert einen bestimmten Tag. |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions-cordova) | Ruft die derzeit vom Gerät abonnierten Tags ab. |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) | Beendet das Abonnement eines bestimmten Tags. |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | Hebt die Registrierung des Geräts beim Push-Benachrichtigungsservice auf. |
{: caption="Tabelle 3. Javascript-Funktionen" caption-side="top"}
{: cordova}

##### API-Implementierung
{: #api-implementation }
{: cordova}

###### Initialisierung
{: #initialization-cordova}
{: cordova}

Initialisieren Sie die **MFPPush**-Instanz.
{: cordova}

- Die Initialisierung ist erforderlich, damit die Clientanwendung mit dem richtigen Anwendungskontext eine Verbindung zum MFPPush-Service herstellen kann.  
- Die API-Methode sollte zuerst aufgerufen werden, bevor andere MFPPush-APIs verwendet werden.
- Die Callback-Funktion wird für die Handhabung empfangener Push-Benachrichtigungen registriert.
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

###### Wird Push unterstützt?
{: #is-push-supported-cordova}
{: cordova}

Es wird überprüft, ob das Gerät Push-Benachrichtigungen unterstützt.
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

###### Gerät registrieren
{: #register-device-cordova}
{: cordova}

Registrieren Sie das Gerät beim Push-Benachrichtigungsservice. Wenn keine Optionen erforderlich sind, kann 'options' auf `null` gesetzt werden.
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

###### Tags abrufen
{: #get-tags-cordova}
{: cordova}

Rufen Sie alle verfügbaren Tags vom Push-Benachrichtigungsservice ab.
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

###### Abonnieren
{: #subscribe-cordova}
{: cordova}

Abonnieren Sie die gewünschten Tags.
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

###### Abonnements abrufen
{: #get-subscriptions-cordova}
{: cordova}

Rufen Sie die derzeit vom Gerät abonnierten Tags ab.
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

###### Abonnement beenden
{: #unsubscribe-cordova}
{: cordova}

Beenden Sie das Tagabonnement.
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

###### Registrierung aufheben
{: #unregister-cordova}
{: cordova}

Sie können die Registrierung des Geräts bei der Instanz des Push-Benachrichtigungsservice aufheben.
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

#### Handhabung von Push-Benachrichtigungen
{: #handling-a-push-notification-cordova}
{: cordova}

Für die Handhabung einer empfangenen Push-Benachrichtigung können Sie mit dem Antwortobjekt der Benachrichtigung in der registrierten Callback-Funktion arbeiten.
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Handhabung von Push-Benachrichtigungen in Windows 8.1 Universal und Windows 10 UWP
{: #handling_push_notifications_in_windows }
{: windows}

Die von {{ site.data.keyword.mobilefirst_notm }} bereitgestellte API für Benachrichtigungen kann verwendet werden, um Geräte zu registrieren &amp; Geräteregistrierungen aufzuheben und um Tags zu abonnieren &amp; Tagabonnements zu beenden. In diesem Lernprogramm erfahren Sie, wie Sie Push-Benachrichtigungen in nativen Windows 8.1 Universal- und Windows 10 UWP-Anwendungen mit C# handhaben können.
{: windows}

#### Voraussetzungen
{: #prereqs-windows}
{: windows}

* {{ site.data.keyword.mfserver_short_notm }} wird lokal ausgeführt oder Sie verfügen über eine remote ausgeführte {{ site.data.keyword.mfserver_short_notm }}-Instanz.
* Die {{ site.data.keyword.mobilefirst_notm  }}-CLI ist auf der Entwicklerworkstation installiert.
{: windows}

#### Benachrichtigungskonfiguration
{: #notifications-configuration-windows}
{: windows}

Erstellen Sie ein neues Visual Studio-Projekt oder verwenden Sie ein vorhandenes Visual Studio-Projekt.  
{: windows}

Wenn das {{site.data.keyword.mobilefirst_notm }}-Windows-SDK nicht bereits im Projekt vorhanden ist, befolgen Sie die Anweisungen im Lernprogramm [{{ site.data.keyword.mobilefirst_notm }}-SDK zu Windows-Anwendungen hinzufügen](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/).
{: windows}

#### Push-SDK hinzufügen
{: #adding-the-push-sdk-windows}
{: windows}

1. Wählen Sie 'Tools → NuGet Package Manager → Package Manager Console' aus.
2. Wählen Sie das Projekt aus, in dem Sie die {{site.data.keyword.mobilefirst_notm }}-Push-Komponente installieren möchten.
3. Fügen Sie das {{site.data.keyword.mobilefirst_notm }}-Push-SDK hinzu, indem Sie den Befehl **Install-Package IBM.MobileFirstPlatformFoundationPush** ausführen.
{: windows}

#### Vorausgesetzte WNS-Konfiguration
{: pre-requisite-wns-configuration }
{: windows}

1. Stellen Sie sicher, dass die Anwendung über die Popup-Benachrichtigungsfunktion (Toast) verfügt. Sie können diese Funktion in 'Package.appxmanifest' aktivieren.
2. `Package Identity Name` und `Publisher` sollten mit den registrierten Werten aus WSN aktualisiert werden.
3. (Optional) Löschen Sie die Datei 'TemporaryKey.pfx'.
{: windows}

#### API für Benachrichtigungen
{: #notifications-api-windows}
{: windows}

##### MFPPush-Instanz
{: #mfppush-instance-windows}
{: windows}

Alle API-Aufrufe müssen für eine Instanz von `MFPPush` aufgerufen werden.  Zu diesem Zweck können Sie eine Variable erstellen, z. B. `private MFPPush PushClient = MFPPush.GetInstance();`, und dann in der gesamten Klasse `PushClient.methodName()` aufrufen.
{: windows}

Alternativ dazu können Sie `MFPPush.GetInstance().methodName()` für jede Instanz aufrufen, in der Sie auf die Push-API-Methoden zugreifen müssen.
{: windows}

##### Abfrage-Handler
{: #challenge-handlers-windows}
{: windows}

Wenn der Bereich `push.mobileclient` einer **Sicherheitsprüfung** zugeordnet ist, müssen Sie sicherstellen, dass vor der Verwendung der Push-APIs passende **Abfrage-Handler** vorhanden und registriert sind.
{: windows}

Weitere Informationen zu Abfrage-Handlern enthält das Lernprogramm [Berechtigungsnachweise validieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/).
{: note}
{: windows}

#### Clientseite
{: #client-side-windows}
{: windows}

| C-Sharp-Methoden                                                                                                | Beschreibung                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            | Initialisiert MFPPush für den angegebenen Kontext.                               |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    | Unterstützt das Gerät Push-Benachrichtigungen?                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  | Registriert das Gerät beim Push-Benachrichtigungsservice.               |
| [`GetTags()`](#get-tags-windows)                                | Ruft die verfügbaren Tags einer Instanz des Push-Benachrichtigungsservice ab. |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     | Richtet das Geräteabonnement für die angegebenen Tags ein.                          |
| [`GetSubscriptions()`](#get-subscriptions-windows)              | Ruft die derzeit vom Gerät abonnierten Tags ab.               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) | Beendet das Abonnement bestimmter Tags.                                  |
| [`UnregisterDevice()`](#unregister-windows)                     | Hebt die Registrierung des Geräts beim Push-Benachrichtigungsservice auf.              |
{: caption="Tabelle 4. C Sharp-Methoden" caption-side="top"}
{: windows}

##### Initialisierung
{: #initialization-windows}
{: windows}

Die Initialisierung ist erforderlich, damit die Clientanwendung eine Verbindung zum MFPPush-Service herstellen kann.
{: windows}

* Die Methode `Initialize` sollte zuerst aufgerufen werden, bevor andere MFPPush-APIs verwendet werden.
* Die Callback-Funktion wird für die Handhabung empfangener Push-Benachrichtigungen registriert.
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### Wird Push unterstützt?
{: #is-push-supported-windows}
{: windows}

Es wird überprüft, ob das Gerät Push-Benachrichtigungen unterstützt.
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push wird unterstützt
} else {
    // Push wird nicht unterstützt
}
```
{: codeblock}
{: windows}

##### Gerät registrieren &amp; Gerätetoken senden
{: #register-device--send-device-token-windows}
{: windows}

Registrieren Sie das Gerät beim Push-Benachrichtigungsservice.
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);         
if (Response.Success == true)
{
    // Erfolgreich registriert
} else {
    // Registrierung mit Fehler fehlgeschlagen
}
```
{: codeblock}
{: windows}

##### Tags abrufen
{: #get-tags-windows}
{: windows}

Rufen Sie alle verfügbaren Tags vom Push-Benachrichtigungsservice ab.
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

##### Abonnieren
{: #subscribe-windows}
{: windows}

Abonnieren Sie die gewünschten Tags.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Abonnementtag abrufen
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //Push-Tag erfolgreich abonniert
}
else
{
    //Abonnement von Push-Tags fehlgeschlagen
}
```
{: codeblock}
{: windows}

##### Abonnements abrufen
{: #get-subscriptions-windows}
{: windows}

Rufen Sie die derzeit vom Gerät abonnierten Tags ab.
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

##### Abonnement beenden
{: #unsubscribe-windows}
{: windows}

Beenden Sie das Tagabonnement.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Tagabonnement beenden
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //Erfolg
}
else
{
    //Abonnent von Tags fehlgeschlagen
}
```
{: codeblock}
{: windows}

##### Registrierung aufheben
{: #unregister-windows}
{: windows}

Sie können die Registrierung des Geräts bei der Instanz des Push-Benachrichtigungsservice aufheben.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();         
if (Response.Success == true)
{
    // Erfolgreich registriert
} else {
    // Registrierung mit Fehler fehlgeschlagen
}
```
{: codeblock}
{: windows}

#### Handhabung von Push-Benachrichtigungen
{: #handling-a-push-notification-windows}
{: windows}

Für die Handhabung von Push-Benachrichtigungen müssen Sie einen `MFPPushNotificationListener` einrichten.  Zu diesem Zweck können Sie die folgende Methode implementieren.
{: windows}

1. Erstellen Sie eine Klasse. Verwenden Sie dazu eine Schnittstelle vom Typ 'MFPPushNotificationListener'.

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Push-Benachrichtigungen hier verarbeiten      
    }
    }
    ```
    {: codeblock}
{: windows}
2. Definieren Sie die Klasse als Listener, indem Sie `MFPPush.GetInstance().listen(new NotificationListner())` aufrufen.
3. In der Methode 'onReceive' empfangen Sie die Push-Benachrichtigung und können für die Benachrichtigung das gewünschte Verhalten festlegen.
{: windows}

#### Push-Benachrichtigungsservice unter Windows Universal
{: #windows-universal-push-notifications-service }
{: windows}

In Ihrer Serverkonfiguration muss kein bestimmter Port geöffnet sein.
{: windows}

WNS verwendet reguläre HTTP- oder HTTPS-Anforderungen.
{: windows}
