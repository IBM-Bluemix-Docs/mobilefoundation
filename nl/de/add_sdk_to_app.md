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

#	Mobile Foundation SDK zu einer App hinzufügen
{: #add_sdk_to_app}

### Android SDK zur App hinzufügen
{: android}

Öffnen Sie Android Studio, wählen Sie die Android-Ansicht aus und wählen Sie **Gradle-Scripts** aus. Wählen Sie die Datei `build.gradle (Module: app)` aus und führen Sie dann die folgenden Schritte aus, um das Android-SDK zu Ihrer Android-Anwendung hinzuzufügen.
{: android}

1. Fügen Sie die folgende Zeile zum Abschnitt `dependencies` hinzu.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. Fügen Sie die folgende Zeile zum Abschnitt `android` hinzu.
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. Öffnen Sie in der Android-Ansicht die Datei **app → manifests → AndroidManifest.xml**. Fügen Sie die folgenden Berechtigungen vor dem Element 'application' hinzu.
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. Fügen Sie die MobileFirst-Benutzerschnittstellenaktivität neben dem vorhandenen Element `activity` hinzu.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### iOS-SDK zur App hinzufügen
{: ios}

Dieses SDK ist das Core-SDK für IBM Mobile Foundation, das sich aus APIs für die Implementierung von Mobile Foundation-Funktionen für die Sicherheit, Autorisierung, Protokollierung, aufrufenden Adaptern und anderen Funktionen zusammensetzt. Führen Sie die folgenden Schritte aus, um das iOS-SDK zur iOS-Anwendung hinzuzufügen.
{: ios}

1. Wechseln Sie in den Stammordner Ihrer iOS-App und führen Sie den folgenden Befehl aus, um eine Podfile zu erstellen.
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. Öffnen Sie die Podfile mit Ihrem bevorzugten Codeeditor.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. Aktualisieren Sie den Pod mit dem folgenden Befehl.
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. Installieren Sie Pod mit dem folgenden Befehl.
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. Öffnen Sie '[ProjectName].xcworkspace', um das Projekt in Xcode zu öffnen.
6. Fügen Sie die Datei `mfpclient.plist` zum Xcode-Arbeitsbereich hinzu.
7. Navigieren Sie von einer Eingabeaufforderung aus zum Projektordner und registrieren Sie Ihre App mit der Mobile Foundation-Instanz.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: ios}

### Cordova-SDK zur App hinzufügen
{: cordova}

Dieses SDK ist das Core-SDK für IBM Mobile Foundation, das sich aus APIs für die Implementierung von Mobile Foundation-Funktionen für die Sicherheit, Autorisierung, Protokollierung, aufrufenden Adaptern und anderen Funktionen zusammensetzt. Führen Sie die folgenden Schritte aus, um das Cordova-SDK zu Ihrer Anwendung hinzuzufügen.
{: cordova}

1. Erstellen Sie ein Cordova-Projekt.
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. Ändern Sie das Verzeichnis in das Stammverzeichnis des Cordova-Projekts.
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Fügen Sie eine oder mehrere unterstützte Plattformen zum Cordova-Projekt hinzu, indem Sie die Cordova-CLI verwenden.
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Fügen Sie das Cordova-Core-Plug-in für Mobile Foundation hinzu.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. Bereiten Sie die Anwendungsressourcen vor, indem Sie den folgenden Befehl ausführen.
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Registrieren Sie die Anwendung auf dem Mobile Foundation-Server.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### Plug-in 'React Native-SDK' zur App hinzufügen
{: reactnative}

Wenn Sie Mobile Foundation-Funktionen zu einer vorhandenen React Native-App hinzufügen möchten, müssen Sie das Plug-in `reagiernativ-ibm-mobilefirst` zu Ihrer App hinzufügen. Das Plug-in `reagiernativ-ibm-mobilefirst` enthält das Mobile Foundation-SDK. Führen Sie die folgenden Schritte aus, um das React Native-Plug-in zur React Native-Anwendung hinzuzufügen.
{: reactnative}

1. Fügen Sie dieses Plug-in auf die gleiche Art und Weise wie das andere `npm`-Plug-in zu Ihrer App hinzu.
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. Der erste Schritt besteht darin, ein React Native-Projekt zu erstellen, z. B. *MobileFirstApp*. Verwenden Sie die React Native-CLI, um ein neues Projekt zu erstellen.
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. Fügen Sie als Nächstes das React Native-Plug-in zur App hinzu.
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. Verknüpfen Sie Ihr Projekt so, dass alle nativen Abhängigkeiten zu Ihrem React Native-Projekt hinzugefügt werden.
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. Nehmen Sie für Android die folgenden Änderungen an der Datei `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`) vor.
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. Fügen Sie **tools:replace= 'android:allowBackup'** zum Tag `application` hinzu.
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
7. Öffnen Sie für iOS Xcode und ziehen Sie `mfpclient.plist` per Drag-and-Drop aus dem Ordner `ios`. Dieser Schritt gilt nur für iOS-Plattformen.
{: reactnative}
