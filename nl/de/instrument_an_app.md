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

# App instrumentieren
{: #instrument_your_app}

Mobile Analytics ist ein Feature, das im {{ site.data.keyword.mobilefoundation_short }}-Service integriert ist. Der Service stellt wichtige Erkenntnisse zur Nutzung und Leistung einer Anwendung für die Entwickler mobiler Anwendungen und die Anwendungseigner bereit.

Sie müssen Ihre mobile Anwendung instrumentieren, um Mobile Analytics zum Überwachen der Anwendungsnutzung und Leistung und Erfassen weiterer Statistikdaten verwenden zu können. Dazu müssen Sie die folgenden Schritte ausführen:

1.  Importieren und installieren Sie das Mobile Analytics-Client-SDK.
2.  Instrumentieren Sie Ihre Anwendung basierend auf dem Typ der Analysedaten, die Sie erfassen möchten.

In den folgenden Abschnitten finden Sie die Details zu diesen Schritten für jede unterstützte Plattform.

### Android-Anwendung instrumentieren
{: #instrument_android_app}
{: android}
#### Schritt 1: Mobile Analytics-Client-SDK for Android importieren und installieren
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

Im Lieferumfang des Client-SDKs für Mobile Analytics ist derzeit Gradle enthalten, ein Abhängigkeitsmanager für Android-Projekte. Gradle lädt Artefakte automatisch aus Repositorys herunter und stellt sie für Ihre Android-Anwendung zur Verfügung.
{: android}

1. Erstellen Sie ein [Android Studio-Projekt ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://developer.android.com/sdk/index.html){: new_window} oder öffnen Sie ein vorhandenes Projekt.
{: android}

2. Öffnen Sie die Datei `build.gradle`, die sich in Ihrem **App-Modul** befindet.
{: android}

  Ihr Android-Projekt kann über zwei Dateien des Typs `build.gradle` verfügen: eine für das Projekt und eine für das App-Modul. Stellen Sie sicher, dass die Datei für das **App-Modul** verwenden.
  {: tip}
  {: android}

3. Suchen Sie den Abschnitt `dependencies` in der Datei `build.gradle` und fügen Sie eine Kompilierungsabhängigkeit für das {{site.data.keyword.mobileanalytics_short}}-Client-SDK hinzu. Die Anweisung für die Repositorys sollte dem folgenden Codebeispiel ähneln:
{: android}

  ```
  dependencies {
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationanalytics:8.0.+@aar'
    // andere Abhängigkeiten
  }
  ```
  {: codeblock}
  {: android}

  Die erste Abhängigkeit ist für das Mobile Analytics-Client-SDK für das Erfassen und Protokollieren von Anwendungslaufzeitereignissen
  und die zweite Abhängigkeit für das Aktivieren von Benutzerfeedback in der App für den Anwendungsbenutzer. Die zweite Abhängigkeit ist nur erforderlich, wenn Sie das Benutzerfeedback in der App aktivieren.
  {: android}

4. Synchronisieren Sie das Projekt mit Gradle durch Klicken auf **Tools &gt; Android &gt; Projekt mit Gradle-Dateien synchronisieren**.
{: android}

5. Öffnen Sie die Datei `AndroidManifest.xml` für Ihr Android-Projekt. Sie finden diese Datei unter **app > manifests**. Fügen Sie unter dem Element `<manifest>` eine Internet- und Positionszugriffsberechtigung hinzu:
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />

  ```
  {: codeblock}
  Wenn Sie eine höhere SDK-Version als > 1.2 verwenden, müssen Sie die folgenden Zeilen in das Element `<application>` der Datei `AndroidManifest.xml` einfügen.
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

#### Schritt 2: Android-Anwendung basierend auf dem Typ der erfassten Analysedaten instrumentieren
{: #instrument_app_based_on_data_android }
{: android}

1. Initialisieren Sie Ihre Anwendung, um Analysedaten zu erfassen und an den Mobile Analytics-Service zu senden.  Fügen Sie zunächst die folgenden `import`-Anweisungen zum Anfang Ihrer Anwendung oder Aktivitätsklasse hinzu:
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   Als Nächstes verwenden Sie das Mobile Analytics-Client-SDK, um die Erfassung von Analysedaten zu konfigurieren oder zu initialisieren.  
   Fügen Sie den Initialisierungscode in der Methode `onCreate` der Hauptaktivität der Android-Anwendung oder an einer Position hinzu, die sich am besten Ihr Anwendungsprojekt eignet.
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   Bevor Sie die Initialisierungsmethode aufrufen, müssen Sie sicherstellen, dass Ihre Anwendung den erforderlichen Code einbettet, um die Einheit mit dem Mobile Foundation-Service zu authentifizieren und zu autorisieren.  Dies ist ein gängiger Schritt, der für alle Mobile Foundation-Serviceanwendungen erforderlich ist und nicht speziell für die Erfassung von Analysedaten verwendet wird. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   Nach Abschluss der Initialisierung ist Ihre Anwendung nun für die Erfassung von Einheitendaten und Mobile Analytics-SDK-Protokollen aktiviert, ohne dass weiterer Code hinzugefügt wurde.  Alle weiteren APIs und Code, die in den folgenden Abschnitten beschrieben werden, sind optional und können basierend auf der Art der Analysedaten, die Sie erfassen möchten, hinzugefügt werden.
   {: android}

2. Um Anwendungslebenszyklusereignisse wie Anwendungssitzungs- und Absturzinformationen zu erfassen, konfigurieren Sie Ihre Anwendung durch Hinzufügen des folgenden Codes:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. Wenn Sie Netzereignisse erfassen möchten, die die Anwendungsnetzinteraktionen aufzeichnen, konfigurieren Sie Ihre Anwendung durch Hinzufügen des folgenden Codes:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. Sie haben Ihre Anwendung jetzt initialisiert, um Analysedaten zu erfassen.  Als Nächstes müssen Sie die erfassten Daten an den Mobile Analytics-Service senden.
   Verwenden Sie die folgende API, um Analysedaten an den Mobile Analytics-Service zu senden.
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  Sie können frei entscheiden, wann diese API in Ihrem Anwendungsablauf aufgerufen werden soll, um die erfassten Analysedaten an den Mobile Analytics-Service zu senden.  Bis zum Senden dieser Daten werden alle erfassten Analysedaten lokal auf dem Gerät gespeichert.
  {: android}

5. Zum Erfassen und Senden von Anwendungsprotokollen an den Mobile Analytics-Service verwenden Sie die Protokollierungs-APIs des Mobile Analytics-Client-SDKs.     
   Das Protokollierungsframework des Mobile Analytics-Client-SDKs unterstützt die folgenden Protokollebenen, die nachfolgend nach Ausführlichkeit mit empfohlenen Richtlinien für die Verwendung aufgeführt sind:
    * FATAL - Verwenden Sie FATAL für Abstürze oder Blockierungen, nach denen keine Wiederherstellung mehr möglich ist. Die Ebene FATAL ist für die Protokollierung nicht behebbarer Fehler reserviert, die den Benutzern als Anwendungsabsturz angezeigt werden.
    * ERROR - Verwenden Sie ERROR für nicht erwartete Ausnahmen oder Netzprotokollfehler.
    * WARN - Verwenden Sie WARN zur Protokollierung von Warnungen zur Nutzung, die keine kritischen Fehler sind, beispielsweise zur Nutzung veralteter APIs oder zu langsamen Netzantworten.
    * INFO - Verwenden Sie INFO zum Melden von Initialisierungsereignissen oder anderen Daten, die wichtig sein können, aber als nicht dringend erforderlich eingestuft werden.
    * DEBUG - Verwenden Sie DEBUG zum Melden von Debuganweisungen, um Entwickler bei der Behebung von Anwendungsfehlern zu unterstützen.
    Wenn die Protokollierungsebene auf DEBUG gesetzt ist, erhalten Sie auch Protokolle des Mobile Analytics-Client-SDKs, die beim Senden von Protokollen enthalten sind.
   {: note}
   {: android}

   Das folgende Code-Snippet zeigt ein Beispiel für die Nutzung der Protokollfunktion für Android:
   ```java
   // Protokollebene festlegen (optional)
   // Die Standardeinstellung ist Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);

   //Erstellen Sie eine Protokollierungsinstanz.  Sie können mehrere Protokollierungsinstanzen erstellen, um Ihre Protokolle wie gewünscht zu organisieren
   Logger logger = Logger.getInstance("loggerName");

   // Protokollnachrichten mit verschiedenen Ebenen
   // Debugnachricht für Feature 1
   // Informationsnachricht für Feature 2
   logger.debug("Debugnachricht");
   //Die Nachricht 'logger.debug' wird nicht protokolliert, da 'logLevelFilter' auf 'Info' gesetzt ist
   logger.info("Informationsnachricht");

   // Protokolle an Mobile Analytics senden
   Logger.send();
   ```
   {: codeblock}
   {: android}

6.  Um Einblicke zu Mustern der Benutzereinbindung (neue Benutzer im Vergleich zu wiederkehrenden Benutzern) zu erhalten, müssen Sie Ihrer Anwendungssitzung eine Benutzeridentität zuordnen.  Dies kann durch Aufrufen der folgenden API erreicht werden:
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    Alle Analysedaten, die lokal erfasst und lokal gespeichert werden, werden nur dann an den Mobile Analytics-Service gesendet, wenn die API 'WLAnalytics.send()' aufgerufen wird.
    {: android}

7.  Um Einblicke in Muster der HTTP-Netzinteraktionen Ihrer Anwendungen zu erhalten, wie z. B. HTTP-APIs, Anzahl der Anforderungen und durchschnittliche Antwortzeiten, müssen Sie die Klasse 'WLResourceRequest' des Mobile Foundation Service-Client-SDKs verwenden, um die HTTP-Aufrufe abzusetzen. Obwohl Sie Analytics für die Erfassung von Netzereignissen initialisiert haben, ermöglicht die Verwendung von `WLResourceRequest` die Anbindung von Mobile Analytics an Netzaufrufe und die Erfassung der relevanten Daten.  Führen Sie die HTTP-Aufrufe wie mit dem folgenden Code dargestellt durch.
    ```java
      WLResourceRequest request = new WLResourceRequest(new URI(url), WLResourceRequest.GET);
            request.send(new WLResponseListener() {

                @Override
                    public void onSuccess(WLResponse wlResponse) {
                    //Erfolg von HTTP-Aufruf verarbeiten und 'wlResponse' verwenden
                }

                @Override
                public void onFailure(WLFailResponse wlFailResponse) {
                    //Fehler von HTTP-Aufruf verarbeiten und 'wlFailResponse' verwenden
                }
            });
    ```
    {: codeblock}
    {: android}

8.  Um Absturzanalysen und Protokolle zu erhalten, können Sie während des Starts der Anwendung die folgende API aufrufen, um zu prüfen, ob die vorherige Sitzung abgestürzt ist, und entsprechend die erfassten Absturzprotokolle an den Mobile Analytics-Service zu senden.
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //Weiteres gewünschtes Handling hinzufügen und die folgenden APIs aufrufen
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  Um benutzerdefinierte Analysen zu definieren und eigene Analysedaten anzugeben, die über das hinausgehen, was im Client-SDK unterstützt wird, können Sie die angepasste Protokollierungs-API verwenden.
    ```java
        //JSON-Objekt zum Erfassen der angepassten Daten erstellen
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("FromPage", "LoginPage");

        //Angepasste Daten mit einer Nachricht protokollieren
        WLAnalytics.log("Protokollnachricht", jsonObject);

        //Erfasste angepasste Daten senden und im Mobile Analytics-Service protokollieren
        WLAnalytics.send();
    ```
    {: codeblock}
    {: android}

    Die protokollierten angepassten Daten können in angepassten Diagrammen dargestellt werden, die Sie in der Mobile Analytics-Konsole definieren können, um benutzerdefinierte Erkenntnisse abzuleiten.
    {: android}

10. Verwenden Sie das Benutzerfeedback in der App, um Ihre Analyse der Anwendungsleistung zu vertiefen. Sie können die **Benutzer und Tester** der Anwendung aktivieren, um den Anwendungseignern kontextreiches Feedback zur Verfügung zu stellen. **Anwendungseigner** erhalten von den Benutzern in Echtzeit Feedback zu Erfahrungen bei der Anwendungsnutzung, die **Anwendungseigner** und **Entwickler** weiterverarbeiten können.  Dies bringt eine hohe Agilität bei der Pflege von Anwendungen mit sich.  Verwenden Sie die folgende API, um Ihre Anwendung in einem beliebigen Aktionshandler in Ihrer Anwendung in den interaktiven Feedback-Modus umzuschalten, z. B. bei der Handhabung eines Klicks auf eine Schaltfläche oder bei der Auswahl eines Menüpunkts.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### iOS-Anwendung instrumentieren
{: #instrument_ios_app}
{: ios}

Schritte zum Instrumentieren der iOS-Anwendung:
{: ios}

#### Schritt 1: Mobile Analytics-Client-SDK for iOS importieren und installieren
{: #install_analytics_sdk_ios }
{: ios}

[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

Das Swift-SDK ist für iOS und watchOS verfügbar.
{: ios}

1. Fügen Sie den folgenden Code zur vorhandenen Poddatei hinzu, die sich im Stammverzeichnis des Xcode-Projekts befindet.
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

2. Navigieren Sie in einem Befehlszeilenfenster zum Stammverzeichnis des Xcode-Projekts und führen Sie den folgenden Befehl aus:
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### Schritt 2: iOS-Anwendung basierend auf dem Typ der erfassten Analysedaten instrumentieren
{: #instrument_app_based_on_data_ios }
{: ios}

1. Initialisieren Sie Ihre Anwendung, um das Senden von Protokollen an Mobile Analytics zu aktivieren.
   Das Swift-SDK ist für iOS und watchOS verfügbar. Importieren Sie die Frameworks `BMSCore` und `BMSAnalytics`, indem Sie die folgenden `import`-Anweisungen zum Anfang der Projektdatei `AppDelegate.swift` hinzufügen:
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   Als Nächstes verwenden Sie das Mobile Analytics-Client-SDK, um die Erfassung von Analysedaten zu konfigurieren oder zu initialisieren.
   Fügen Sie den Initialisierungscode an einer Position hinzu, die sich am besten für Ihr Anwendungsprojekt eignet.
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   Bevor Sie die Initialisierungsmethode aufrufen, müssen Sie sicherstellen, dass Ihre Anwendung den erforderlichen Code einbettet, um die Einheit mit dem Mobile Foundation-Service zu authentifizieren und zu autorisieren.  Dies ist ein gängiger Schritt, der für alle Mobile Foundation-Serviceanwendungen erforderlich ist und nicht speziell für die Erfassung von Analysedaten verwendet wird. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   Nach Abschluss der Initialisierung ist Ihre Anwendung nun für die Erfassung von Einheitendaten und Mobile Analytics-SDK-Protokollen aktiviert, ohne dass weiterer Code hinzugefügt wurde.  Alle weiteren APIs und Code, die in den folgenden Abschnitten beschrieben werden, sind optional und können basierend auf der Art der Analysedaten, die Sie erfassen möchten, hinzugefügt werden.
   {: ios}

2. Um Anwendungslebenszyklusereignisse wie Anwendungssitzungs- und Absturzinformationen zu erfassen, konfigurieren Sie Ihre Anwendung durch Hinzufügen des folgenden Codes:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. Wenn Sie Netzereignisse erfassen möchten, die die Anwendungsnetzinteraktionen aufzeichnen, konfigurieren Sie Ihre Anwendung durch Hinzufügen des folgenden Codes:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. Sie haben Ihre Anwendung jetzt initialisiert, um Analysedaten zu erfassen.  Als Nächstes müssen Sie die erfassten Daten an den Mobile Analytics-Service senden.
   Verwenden Sie die folgende API, um Analysedaten an den Mobile Analytics-Service zu senden.
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    Sie können frei entscheiden, wann diese API in Ihrem Anwendungsablauf aufgerufen werden soll, um die erfassten Analysedaten an den Mobile Analytics-Service zu senden.  Bis zum Senden dieser Daten werden alle erfassten Analysedaten lokal auf dem Gerät gespeichert.
    {: ios}

5. Zum Erfassen und Senden von Anwendungsprotokollen an den Mobile Analytics-Service verwenden Sie die Protokollierungs-APIs des Mobile Analytics-Client-SDKs.
   Das Protokollierungsframework des Mobile Analytics-Client-SDKs unterstützt die folgenden Protokollebenen, die nachfolgend nach Ausführlichkeit mit empfohlenen Richtlinien für die Verwendung aufgeführt sind:
    * FATAL - Verwenden Sie FATAL für Abstürze oder Blockierungen, nach denen keine Wiederherstellung mehr möglich ist. Die Ebene FATAL ist für die Protokollierung nicht behebbarer Fehler reserviert, die den Benutzern als Anwendungsabsturz angezeigt werden.
    * ERROR - Verwenden Sie ERROR für nicht erwartete Ausnahmen oder Netzprotokollfehler.
    * WARN - Verwenden Sie WARN zur Protokollierung von Warnungen zur Nutzung, die keine kritischen Fehler sind, beispielsweise zur Nutzung veralteter APIs oder zu langsamen Netzantworten.
    * INFO - Verwenden Sie INFO zum Melden von Initialisierungsereignissen oder anderen Daten, die wichtig sein können, aber als nicht dringend erforderlich eingestuft werden.
    * DEBUG - Verwenden Sie DEBUG zum Melden von Debuganweisungen, um Entwickler bei der Behebung von Anwendungsfehlern zu unterstützen.
    Wenn die Protokollierungsebene auf DEBUG gesetzt ist, erhalten Sie auch Protokolle des Mobile Analytics-Client-SDKs, die beim Senden von Protokollen enthalten sind.
   {: note}
   {: ios}

    Im [Lernprogramm ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) finden Sie Code-Snippets für die Protokollierung zum Hinzufügen von Protokollfunktionen in iOS-Anwendungen.

6.  Um Einblicke zu Mustern der Benutzereinbindung (neue Benutzer im Vergleich zu wiederkehrenden Benutzern) zu erhalten, müssen Sie Ihrer Anwendungssitzung eine Benutzeridentität zuordnen.  Dies kann durch Aufrufen der folgenden API erreicht werden:
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    Alle Analysedaten, die lokal erfasst und lokal gespeichert werden, werden nur dann an den Mobile Analytics-Service gesendet, wenn die API `WLAnalytics.sharedInstance().send()` aufgerufen wird.
    {: ios}

7.  Um Einblicke in Muster der HTTP-Netzinteraktionen Ihrer Anwendungen zu erhalten, wie z. B. HTTP-APIs, Anzahl der Anforderungen und durchschnittliche Antwortzeiten, müssen Sie die Klasse 'WLResourceRequest' des Mobile Foundation Service-Client-SDKs verwenden, um die HTTP-Aufrufe abzusetzen. Obwohl Sie Analytics für die Erfassung von Netzereignissen initialisiert haben, ermöglicht die Verwendung von `WLResourceRequest` die Anbindung von Mobile Analytics an Netzaufrufe und die Erfassung der relevanten Daten.  Führen Sie die HTTP-Aufrufe wie mit dem folgenden Code dargestellt durch.
    ```Swift
      let request = WLResourceRequest( url: URL(string: "/adapters/JavaAdapter/users"), method: WLHttpMethodGet )

      request!.send { (response, error) -> Void in
          if(error == nil){
              //Fehler von HTTP-Aufruf verarbeiten und 'wlFailResponse' verwenden
          }
          else{
              //Erfolg von HTTP-Aufruf verarbeiten und 'wlResponse' verwenden
          }
      }
    ```
    {: codeblock}
    {: ios}

8.  Um Absturzanalysen und Protokolle zu erhalten, können Sie während des Starts der Anwendung die folgende API aufrufen, um zu prüfen, ob die vorherige Sitzung abgestürzt ist, und die erfassten Absturzprotokolle an den Mobile Analytics-Service zu senden.
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //Weiteres gewünschtes Handling hinzufügen und die folgenden APIs aufrufen
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  Um benutzerdefinierte Analysen zu definieren und eigene Analysedaten anzugeben, die über das hinausgehen, was im Client-SDK unterstützt wird, können Sie die angepasste Protokollierungs-API verwenden.
    ```Swift
        //Array-Objekt mit Schlüssel/Wert-Paar als angepasste Daten erstellen
        let eventObject = ["FromPage": "LoginPage"]

        //Angepasste Daten mit einer Nachricht protokollieren
        WLAnalytics.sharedInstance().log("Protokollnachricht", withMetadata: eventObject);

        //Erfasste angepasste Daten senden und im Mobile Analytics-Service protokollieren
        WLAnalytics.sharedInstance().send();
    ```
    {: codeblock}
    {: ios}

    Die protokollierten angepassten Daten können in angepassten Diagrammen dargestellt werden, die Sie in der Mobile Analytics-Konsole definieren können, um benutzerdefinierte Erkenntnisse abzuleiten.
    {: ios}

10. Verwenden Sie das Benutzerfeedback in der App, um Ihre Analyse der Anwendungsleistung zu vertiefen.   Sie können die **Benutzer und Tester** der Anwendung aktivieren, um den Anwendungseignern kontextreiches Feedback zur Verfügung zu stellen. **Anwendungseigner** erhalten von den Benutzern in Echtzeit Feedback zu Erfahrungen bei der Anwendungsnutzung, die **Anwendungseigner** und **Entwickler** weiterverarbeiten können.  Dies bringt eine hohe Agilität bei der Pflege von Anwendungen mit sich.  Verwenden Sie die folgende API, um Ihre Anwendung in einem beliebigen Aktionshandler in Ihrer Anwendung in den interaktiven Feedback-Modus umzuschalten, z. B. bei der Handhabung eines Klicks auf eine Schaltfläche oder bei der Auswahl eines Menüpunkts.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Cordova-Anwendung instrumentieren
{: #instrument_cordova_app}
{: cordova}
Schritte zum Instrumentieren der Cordova-Anwendung:
{: cordova}

#### Schritt 1: Foundation- und Analytics-Client-SDK-Plug-in für Cordova importieren und installieren
{: #install_analytics_sdk_cordova }
Mit dem Cordova-Plug-in von Mobile Analytics können Sie Ihre mobile Anwendung instrumentieren.
{: cordova}

#### Cordova-Client-SDK installieren
{: #install_cordova_sdk}
{: cordova}

1. Erstellen Sie ein [Cordova-Projekt ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://cordova.apache.org/#getstarted){: new_window} oder öffnen Sie ein vorhandenes Projekt.
    {: cordova}

2. Fügen Sie Ihrer Cordova-Anwendung Android- oder iOS-Plattformen Ihrer Wahl hinzu. Führen Sie einen oder beide der folgenden Befehle über die Befehlszeile aus.<br/>
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

3. Fügen Sie das Foundation-Client-SDK-Plug-in `cordova-plugin-mfp` hinzu.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. Fügen Sie das optionale Analytics-Client-SDK-Plug-in `cordova-plugin-mfp-analytics` hinzu, das für die appinterne Feedbackfunktion für Ihre App dient.
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. Überprüfen Sie, ob das Plug-in erfolgreich installiert wurde, indem Sie den folgenden Befehl ausführen:
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### Schritt 2: Cordova-Anwendung basierend auf dem Typ der erfassten Analysedaten instrumentieren
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. In Cordova-Anwendungen ist keine Konfiguration erforderlich und die Initialisierung ist integriert.

   Bevor Sie eine der folgenden Analysemethoden aufrufen, müssen Sie sicherstellen, dass Ihre Anwendung den erforderlichen Code einbettet, um die Einheit mit dem Mobile Foundation-Service zu authentifizieren und zu autorisieren. Dies ist ein gängiger Schritt, der für alle Mobile Foundation-Serviceanwendungen erforderlich ist und nicht speziell für die Erfassung von Analysedaten verwendet wird. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   Nach Abschluss der Initialisierung ist Ihre Anwendung nun für die Erfassung von Einheitendaten und Mobile Analytics-SDK-Protokollen aktiviert, ohne dass weiterer Code hinzugefügt wurde.  Alle weiteren APIs und Code, die in den folgenden Abschnitten beschrieben werden, sind optional und können basierend auf der Art der Analysedaten, die Sie erfassen möchten, hinzugefügt werden.
   {: cordova}

2. Um die Erfassung der Lebenszyklusereignisse zu ermöglichen, muss sie in der nativen Plattform der Cordova-App initialisiert werden.
    * Für die Android-Plattform:
      * Öffnen Sie die Datei **[Stammordner der Cordova-Anwendung] → platforms → android → src → com → sample → [app-name] → MainActivity.java**.
      * Suchen Sie nach der Methode `onCreate` und aktivieren Sie die Aktivitäten `LIFECYCLE` und `NETWORK`, indem Sie die folgenden Schritte ausführen:
        * Fügen Sie die folgende Zeile hinzu, um die Lebenszyklusereignisprotokollierung für den Client zu aktivieren:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Fügen Sie die folgende Zeile hinzu, um die Netzereignisprotokollierung für den Client zu aktivieren:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Erstellen Sie das Cordova-Projekt, indem Sie den Befehl `cordova build` ausführen.
    * Für die iOS-Plattform:
      * Öffnen Sie den Ordner **[Stammordner der Cordova-Anwendung] → platforms → ios → Classes** und suchen Sie nach der Datei **AppDelegate.swift** (Swift).
      * Führen Sie die folgenden Schritte aus, um die Aktivitäten `LIFECYCLE` und `NETWORK` zu aktivieren.
        * Fügen Sie die folgende Zeile hinzu, um die Lebenszyklusereignisprotokollierung für den Client zu aktivieren:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Fügen Sie die folgende Zeile hinzu, um die Netzereignisprotokollierung für den Client zu aktivieren:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Erstellen Sie das Cordova-Projekt, indem Sie den Befehl `cordova build` ausführen.

3. Sie haben nun Ihre Anwendung initialisiert, um Analysedaten zu erfassen. Als Nächstes können Sie Analysedaten an Mobile Analytics senden.
   Verwenden Sie die folgenden APIs, um die Aufzeichnung zu starten und Analysedaten zur Nutzung zu senden:
   ```Javascript
   // Aufgezeichnete Analysedaten zur Nutzung an Mobile Analytics senden

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. Zum Erfassen und Senden von Anwendungsprotokollen an den Mobile Analytics-Service verwenden Sie die Protokollierungs-APIs des Mobile Analytics-Client-SDKs.
   Das Protokollierungsframework des Mobile Analytics-Client-SDKs unterstützt die folgenden Protokollebenen, die nachfolgend nach Ausführlichkeit mit empfohlenen Richtlinien für die Verwendung aufgeführt sind:
    * FATAL - Verwenden Sie FATAL für Abstürze oder Blockierungen, nach denen keine Wiederherstellung mehr möglich ist. Die Ebene FATAL ist für die Protokollierung nicht behebbarer Fehler reserviert, die den Benutzern als Anwendungsabsturz angezeigt werden.
    * ERROR - Verwenden Sie ERROR für nicht erwartete Ausnahmen oder Netzprotokollfehler.
    * WARN - Verwenden Sie WARN zur Protokollierung von Warnungen zur Nutzung, die keine kritischen Fehler sind, beispielsweise zur Nutzung veralteter APIs oder zu langsamen Netzantworten.
    * INFO - Verwenden Sie INFO zum Melden von Initialisierungsereignissen oder anderen Daten, die wichtig sein können, aber als nicht dringend erforderlich eingestuft werden.
    * DEBUG - Verwenden Sie DEBUG zum Melden von Debuganweisungen, um Entwickler bei der Behebung von Anwendungsfehlern zu unterstützen.
    Wenn die Protokollierungsebene auf DEBUG gesetzt ist, erhalten Sie auch Protokolle des Mobile Analytics-Client-SDKs, die beim Senden von Protokollen enthalten sind.
    * TRACE - Verwenden Sie TRACE für Eintritts- und Ausstiegspunkte für Methoden.
   {: note}
   {: cordova}

   Das folgende Code-Snippet zeigt ein Beispiel für die Nutzung der Protokollfunktion für Cordova:
    ```Javascript
    // Protokollebene festlegen (optional)
    // Die Standardeinstellung ist DEBUG
    WL.Logger.config({"Ebene": "INFO"});

    //Erstellen Sie eine Protokollierungsinstanz.  Sie können mehrere Protokollierungsinstanzen erstellen, um Ihre Protokolle wie gewünscht zu organisieren.
    var logger = WL.Logger.create({ pkg: 'loggerName' });

    // Protokollnachrichten mit verschiedenen Ebenen
    // Debugnachricht für Feature 1
    // Informationsnachricht für Feature 2
    logger.debug("Debug","Debugnachricht");
    //Die Nachricht 'logger.debug' wird nicht protokolliert, da 'logLevelFilter' auf 'Info' gesetzt ist
    logger.info("Info","Informationsnachricht");

    // Protokolle an Mobile Analytics senden
    logger.send();
    ```
    {: codeblock}
    {: cordova}

5.  Um Einblicke zu Mustern der Benutzereinbindung (neue Benutzer im Vergleich zu wiederkehrenden Benutzern) zu erhalten, müssen Sie Ihrer Anwendungssitzung eine Benutzeridentität zuordnen.  Auf der JavaScript-Ebene sind keine spezifischen APIs verfügbar. Dies kann jedoch mit nativen Code durch das Aufrufen der folgenden API durchgesetzt werden:

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

    Alle Analysedaten, die lokal erfasst und lokal gespeichert werden, werden nur dann an den Mobile Analytics-Service gesendet, wenn die API 'WL.Analytics.send()' auf JavaScript-Ebene [oder] die API 'WLAnalytics.send()' im nativen Android [oder] die API 'WLAnalytics.sharedInstance().send()' im nativen iOS aufgerufen wird.
    {: cordova}

6. Um Einblicke in Muster der HTTP-Netzinteraktionen Ihrer Anwendungen zu erhalten, wie z. B. HTTP-APIs, Anzahl der Anforderungen und durchschnittliche Antwortzeiten, müssen Sie die Klasse 'WLResourceRequest' des Mobile Foundation Service-Client-SDKs verwenden, um die HTTP-Aufrufe abzusetzen. Obwohl Sie Analytics für die Erfassung von Netzereignissen initialisiert haben, ermöglicht die Verwendung von `WLResourceRequest` die Anbindung von Mobile Analytics an Netzaufrufe und die Erfassung der relevanten Daten.  Führen Sie die HTTP-Aufrufe wie mit dem folgenden Code dargestellt durch.
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

7. Um benutzerdefinierte Analysen zu definieren und eigene Analysedaten anzugeben, die über das hinausgehen, was im Client-SDK unterstützt wird, können Sie die angepasste Protokollierungs-API verwenden.
    ```Javascript
        //Angepasste Daten als Schlüssel/Wert-Paar erstellen
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Erfasste angepasste Daten senden und im Mobile Analytics-Service protokollieren
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    Die protokollierten angepassten Daten können in angepassten Diagrammen dargestellt werden, die Sie in der Mobile Analytics-Konsole definieren können, um benutzerdefinierte Erkenntnisse abzuleiten.
    {: cordova}

8. Verwenden Sie das Benutzerfeedback in der App, um Ihre Analyse der Anwendungsleistung zu vertiefen.   Sie können die **Benutzer und Tester** der Anwendung aktivieren, um den Anwendungseignern kontextreiches Feedback zur Verfügung zu stellen. **Anwendungseigner** erhalten von den Benutzern in Echtzeit Feedback zu Erfahrungen bei der Anwendungsnutzung, die **Anwendungseigner** und **Entwickler** weiterverarbeiten können.  Dies bringt eine hohe Agilität bei der Pflege von Anwendungen mit sich.  Verwenden Sie die folgende API, um Ihre Anwendung in einem beliebigen Aktionshandler in Ihrer Anwendung in den interaktiven Feedback-Modus umzuschalten, z. B. bei der Handhabung eines Klicks auf eine Schaltfläche oder bei der Auswahl eines Menüpunkts.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Webanwendung instrumentieren
{: #instrument_web_app}
{: web}

Schritte zum Instrumentieren der Webanwendung:
{: web}

#### Schritt 1: Mobile Analytics-Web-Client-SDK importieren und installieren
{: #install_analytics_sdk_web }
{: web}

#### Web-Client-SDK installieren
{: #install_web_sdk}
Mit dem Mobile Analytics-SDK können Sie Ihre Webanwendung instrumentieren.
{: web}

1. Wechseln Sie in den Stammordner Ihrer Webanwendung und führen Sie den folgenden Befehl aus. Fügen Sie das SDK mithilfe von [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk) zu Ihrer Webanwendung hinzu.
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### Schritt 2: Webanwendung basierend auf dem Typ der erfassten Analysedaten instrumentieren
{: #instrument_app_based_on_data_web }
{: web}

1. Referenzieren Sie die Dateien 'ibmmfpf.js' und 'ibmmfpfanalytics.js' im Element `head` auf Ihrer HTML-Hauptseite.

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. Initialisieren Sie das Mobile Foundation-Web-SDK, indem Sie die Werte für das Kontextstammelement und die Anwendungs-ID in der JavaScript-Hauptdatei Ihrer Webanwendung angeben.

    ```Javascript
      var wlInitOptions = {
        mfpContextRoot : '/mfp', // "mfp" ist der Standardkontextstamm in Mobile Foundation
        applicationId : 'com.sample.mywebapp' // Durch Ihren eigenen Wert ersetzen.
      };

      WL.Client.init(wlInitOptions).then (
        function() {
          // Anwendungslogik.
        }
      );
    ```
    {: codeblock}
    {: web}

   Bevor Sie eine der folgenden Analysemethoden aufrufen, müssen Sie sicherstellen, dass Ihre Anwendung den erforderlichen Code einbettet, um die Einheit mit dem Mobile Foundation-Service zu authentifizieren und zu autorisieren. Dies ist ein gängiger Schritt, der für alle Mobile Foundation-Serviceanwendungen erforderlich ist und nicht speziell für die Erfassung von Analysedaten verwendet wird. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   Nach Abschluss der Initialisierung ist Ihre Anwendung nun für die Erfassung von Einheitendaten und Mobile Analytics-SDK-Protokollen aktiviert, ohne dass weiterer Code hinzugefügt wurde.  Alle weiteren APIs und Code, die in den folgenden Abschnitten beschrieben werden, sind optional und können basierend auf der Art der Analysedaten, die Sie erfassen möchten, hinzugefügt werden.
   {: web}

3. Fügen Sie die folgende Zeile zur Anwendungslogik hinzu, um Anwendungslebenszyklusereignisse und Netzereignisse zu erfassen.

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. Sie haben Ihre Anwendung jetzt initialisiert, um Analysedaten zu erfassen.  Als Nächstes müssen Sie die erfassten Daten an den Mobile Analytics-Service senden. Verwenden Sie die folgende API, um Analysedaten an den Mobile Analytics-Service zu senden.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    Sie können frei entscheiden, wann diese API in Ihrem Anwendungsablauf aufgerufen werden soll, um die erfassten Analysedaten an den Mobile Analytics-Service zu senden.  Bis zum Senden dieser Daten werden alle erfassten Analysedaten lokal auf dem Gerät gespeichert.
    {: web}

5. Zum Erfassen und Senden von Anwendungsprotokollen an den Mobile Analytics-Service verwenden Sie die Protokollierungs-APIs des Mobile Analytics-Client-SDKs.
   Das Protokollierungsframework des Mobile Analytics-Client-SDKs unterstützt die folgenden Protokollebenen, die nachfolgend nach Ausführlichkeit mit empfohlenen Richtlinien für die Verwendung aufgeführt sind:
    * FATAL - Verwenden Sie FATAL für Abstürze oder Blockierungen, nach denen keine Wiederherstellung mehr möglich ist. Die Ebene FATAL ist für die Protokollierung nicht behebbarer Fehler reserviert, die den Benutzern als Anwendungsabsturz angezeigt werden.
    * ERROR - Verwenden Sie ERROR für nicht erwartete Ausnahmen oder Netzprotokollfehler.
    * WARN - Verwenden Sie WARN zur Protokollierung von Warnungen zur Nutzung, die keine kritischen Fehler sind, beispielsweise zur Nutzung veralteter APIs oder zu langsamen Netzantworten.
    * INFO - Verwenden Sie INFO zum Melden von Initialisierungsereignissen oder anderen Daten, die wichtig sein können, aber als nicht dringend erforderlich eingestuft werden.
    * DEBUG - Verwenden Sie DEBUG zum Melden von Debuganweisungen, um Entwickler bei der Behebung von Anwendungsfehlern zu unterstützen.
    Wenn die Protokollierungsebene auf DEBUG gesetzt ist, erhalten Sie auch Protokolle des Mobile Analytics-Client-SDKs, die beim Senden von Protokollen enthalten sind.
    * TRACE - Verwenden Sie TRACE für Eintritts- und Ausstiegspunkte für Methoden.
   {: note}
   {: web}

   Das folgende Code-Snippet zeigt ein Beispiel für die Nutzung der Protokollfunktion für eine Webanwendung:
   ```Javascript
    //Erstellen Sie eine Protokollierungsinstanz.  Sie können mehrere Protokollierungsinstanzen erstellen, um Ihre Protokolle wie gewünscht zu organisieren.

    var logger = ibmmfpfanalytics.logger.pkg("loggerName");
    // Protokollnachrichten mit verschiedenen Ebenen
    // Debugnachricht für Feature 1
    // Informationsnachricht für Feature 2
    logger.debug("Debugnachricht");
    logger.info("Informationsnachricht");

    // Protokolle an die Mobile Analytics senden
    ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

   Für das Web-SDK kann die Ebene nicht vom Client festgelegt werden. Die gesamte Protokollierung wird an den Server gesendet, bis die Konfiguration durch Abrufen des Serverkonfigurationsprofils geändert wird.
   {: note}
   {: web}

6. Um Einblicke zu Mustern der Benutzereinbindung (neue Benutzer im Vergleich zu wiederkehrenden Benutzern) zu erhalten, müssen Sie Ihrer Anwendungssitzung eine Benutzeridentität zuordnen.  Dies kann durch Aufrufen der folgenden API erreicht werden:
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    Alle Analysedaten, die lokal erfasst und lokal gespeichert werden, werden nur dann an den Mobile Analytics-Service gesendet, wenn die API 'ibmmfpfanalytics.send()' aufgerufen wird.
    {: web}

7.  Um Einblicke in Muster der HTTP-Netzinteraktionen Ihrer Anwendungen zu erhalten, wie z. B. HTTP-APIs, Anzahl der Anforderungen und durchschnittliche Antwortzeiten, müssen Sie die Klasse 'WLResourceRequest' des Mobile Foundation Service-Client-SDKs verwenden, um die HTTP-Aufrufe abzusetzen. Obwohl Sie Analytics für die Erfassung von Netzereignissen initialisiert haben, ermöglicht die Verwendung von `WLResourceRequest` die Anbindung von Mobile Analytics an Netzaufrufe und die Erfassung der relevanten Daten.  Führen Sie die HTTP-Aufrufe wie mit dem folgenden Code dargestellt durch.
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

8.  Um benutzerdefinierte Analysen zu definieren und eigene Analysedaten anzugeben, die über das hinausgehen, was im Client-SDK unterstützt wird, können Sie die angepasste Protokollierungs-API verwenden.
    

    ```Javascript
        //Angepasste Daten werden mit der Methode 'addEvent' gesendet
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Erfasste angepasste Daten senden und im Mobile Analytics-Service protokollieren
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    Die protokollierten angepassten Daten können in angepassten Diagrammen dargestellt werden, die Sie in der Mobile Analytics-Konsole definieren können, um benutzerdefinierte Erkenntnisse abzuleiten.
    {: web}

## Nächste Schritte
{: #next_steps_analytics}

Probieren Sie [hier](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples) ein einfaches Beispiel aus.  In weniger als 5 Minuten können Sie die Beispielanwendung ausführen und sich ein Bild davon machen, was mit der API möglich ist. Sie können auch feststellen, wie Analysen als unterschiedliche Einblicke in die Analysekonsole angezeigt werden.  

Mobile Foundation Analytics Service stellt REST-APIs bereit, die Entwicklern beim Importieren (POST) und beim Exportieren (GET) von Analysedaten helfen.

Testen Sie [hier](https://ma-server.us-south.mobile-analytics-prod.cloud.ibm.com/analytics-service/) die REST-API für Analysen für Swagger-Dokumente.
