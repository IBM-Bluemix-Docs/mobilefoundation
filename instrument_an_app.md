---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: mobile analytics, instrumenting cordova app, instrumenting iOS app, instrumenting android app

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
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

# Instrument your app
{: #instrument_your_app}

{{site.data.keyword.mobileanalytics_short}} is a feature that is embedded within the {{site.data.keyword.mobilefoundation_short}} service. {{site.data.keyword.mobileanalytics_short}} provides key application usage and application performance insights for mobile application developers and application owners.

You need to instrument your mobile application to use the {{site.data.keyword.mobileanalytics_short}} feature to monitor your application usage, performance and to get other statistics. 
Instrumenting you application has the following steps:

1. Import and install the Mobile Analytics Client SDK.
1. Instrument your application based on the type of analytics data you want to collect.

The following sections provide the details for these steps, for each of the supported platforms.

## Instrument an Android application
{: #instrument_android_app}
{: android}

### Step 1: Import and install the {{site.data.keyword.mobileanalytics_short}} Client SDK for Android
{: #install_analytics_sdk_android}
{: android}
[![Maven Central-mfp](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation){: external}
[![Maven Central-mfp Analytics](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics){: external}
{: android}

The {{site.data.keyword.mobileanalytics_short}} Client SDK is distributed with Gradle, a dependency manager for Android projects. Gradle automatically downloads artifacts from repositories and makes them available to your Android application.
{: android}

1. Create an [Android Studio](http://developer.android.com/sdk/index.html){: external} project or open an existing project.
{: android}

1. Open the `build.gradle` file that is in your **app module**.
{: android}

   Your Android project might have two `build.gradle` files: one for the project and one for the app module. Make sure to use the **app module** file.
   {: tip}
   {: android}

1. Find the `Dependencies` section of the `build.gradle` file and add a compile dependency for the {{site.data.keyword.mobileanalytics_short}} Client SDK. Your repositories statement must be similar to the following code example:
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

   The first dependency is for the {{site.data.keyword.mobileanalytics_short}} client sdk for capturing and logging application runtime events and the second dependency is for enabling in-app user feedback interacting with the application user. The second dependency is only required if you enable in-app user feedback.
   {: android}

1. Synchronize your project with Gradle by clicking **Tools &gt; Android &gt; Sync Project with Gradle Files**.
{: android}

1. Open the `AndroidManifest.xml` file for your Android project. You can find this file in **app > manifests**. Add internet access and location access permission under the `<manifest>` element:
{: android}

   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```
   {: codeblock}

   If you're using SDK version greater than >= 1.2, then you need to put the following lines inside the `<application>` element of the `AndroidManifest.xml` file.
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

### Step 2: Instrument your Android application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_android }
{: android}

1. Initialize your application to capture and send analytics data to {{site.data.keyword.mobileanalytics_short}} service. First, add the following `import` statements to the beginning of your application or activity class:
{: android}

   ```Java
   import com.worklight.common.Logger;
   import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   Next, use the Mobile Analytics Client SDK to set up or initialize the capture of analytics data. 

   Add the initialization code in the `onCreate` method of the main activity in your Android application, or in a location that works best for your application project.
   {: android}

   ```Java
   WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   Before calling the init method, you must ensure that your application embeds the required code to authenticate and authorize the device with the {{site.data.keyword.mobilefoundation_short}} service. This step is a common step that is required of all {{site.data.keyword.mobilefoundation_short}} services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   With initialization complete, your application is now enabled to capture device information and {{site.data.keyword.mobileanalytics_short}} SDK logs with no further code added. Any further APIs and code that is discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: android}

1. To capture application lifecycle events such as application session and crash information, configure your application by adding the following line of code:

   ```Java
   WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
   ```
   {: codeblock}
   {: android}

1. To capture network events that capture the applications network interactions, configure your application by adding the following line of code:

   ```Java
   WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
   ```
   {: codeblock}
   {: android}

1. You now initialized your application to capture analytics data. Next, you must send the captured data to the {{site.data.keyword.mobileanalytics_short}} service.

   Use the following API to send analytics data to the {{site.data.keyword.mobileanalytics_short}} service.

   ```java
   WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

   You are free to decide when to call this API in your application flow to send the captured analytics data to the {{site.data.keyword.mobileanalytics_short}} service. Until this sends, all captured analytics data is stored locally on the device.
   {: android}

1. To capture and send application logs to the Mobile Analytics service, use the {{site.data.keyword.mobileanalytics_short}} Client SDK logger APIs. 

   The {{site.data.keyword.mobileanalytics_short}} Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:

   * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
   * ERROR - Use for unexpected exceptions or unexpected network protocol errors
   * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
   * INFO - Use for reporting initialization events and other data that might be important, but not urgent
   * DEBUG - Use for reporting debug statements to help developers resolve application defects.

   When the logger level is set DEBUG, you also get Mobile Analytics Client SDK logs, which are included when you send logs.
   {: note}
   {: android}

   The following code snippets show sample Logger usage for Android:

   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);

   //Create a logger instance. You can create multiple loggers to organize your logs as you wish
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

1. To get insights on user onboarding patterns (new users versus returning users), you must associate a user identity with your application session. This step can be done by calling the following API:

   ```java
   WLAnalytics.setUserContext("userName or userIdentity");
   ```
   {: codeblock}
   {: android}

   All analytics data that is captured and stored locally is sent to the Mobile Analytics service only when the WLAnalytics.send() API is called.
   {: android}

1. To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the {{site.data.keyword.mobilefoundation_short}} Service Client SDK's WLResourceRequest class to make the HTTP calls. Although you initialized Analytics for the capture of Network events, by using `WLResourceRequest` enables the {{site.data.keyword.mobileanalytics_short}} to hook into the network calls and capture the relevant data. Make your HTTP calls as in the following code.

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

1. To get Crash Analytics and logs, you might call the following the API during start of your application to check whether the previous session fail and send the capture crash logs to the {{site.data.keyword.mobileanalytics_short}} service.

   ```java
   if (Logger.isUnCaughtExceptionDetected()) {
         //add any other handling you may want and call the following APIs
         WLAnalytics.send();
         Logger.send();
   }
   ```
   {: codeblock}
   {: android}

1. To define Custom Analytics and define your own analytics data over what is supported inherently in the Client SDK, you might use the custom logging API

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

   The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
   {: android}

1. Use In-App User Feedback to deepen your application performance analysis. You can enable your application **users and testers** to provide rich contextual feedback to the app owners. **App owners** get real-time feedback from its users on the application usage experience which **App owners** and **Developers** can further act on. This feature brings in significant agility to application up-keep. Use the following API to switch your application to Interactive Feedback Mode in any action handler in your application, for example, when you handle the click of a button or the selection of a menu item.

   ```java
   WLAnalytics.triggerFeedbackMode();
   ```
   {: codeblock}
   {: android}

## Instrument an iOS application
{: #instrument_ios_app}
{: ios}

Steps to instrument your iOS application:
{: ios}

### Step 1: Import and install the Mobile Analytics Client SDK for iOS
{: #install_analytics_sdk_ios}
{: ios}

[![CocoaPods Compatible - {{site.data.keyword.mobilefoundation_short}}](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation){: external}
[![CocoaPods Compatible - {{site.data.keyword.mobileanalytics_short}}](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics){: external}
{: ios}

The Swift SDK is available for iOS and watchOS.
{: ios}

1. Add the following to the existing podfile, which is at the root of the Xcode project

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

1. From a Command-line window, go to the root of the Xcode project and run the following command

   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

### Step 2: Instrument your iOS application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_ios}
{: ios}

1. Initialize your application to enable sending logs to the Mobile Analytics.
   The Swift SDK is available for iOS and watchOS. Import the `BMSCore` and `BMSAnalytics` frameworks by adding the following `import` statements to the beginning of your `AppDelegate.swift` project file:

   ```Swift
   import IBMMobileFirstPlatformFoundation
   ```
   {: codeblock}
   {: ios}

   Next, use the Mobile Analytics Client SDK to set up or initialize the capture of analytics data.
   Add the initialization code in a location that works best for your application project.
   {: ios}

   ```Swift
   WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   Before calling the init method, you must ensure that your application embeds the required code to authenticate and authorize the device with the MobileFoundation service. This step is a common step that is required of all Mobile Foundation services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   With initialization, complete your application is now enabled to capture device information and Mobile Analytics SDK logs with no further code added. Any further APIs and code that is discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: ios}

1. To capture application lifecycle events such as application session and crash information configure your application by adding the following line of code:

   ```Swift
   WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
   ```
   {: codeblock}
   {: ios}

1. To capture network events that capture the applications network interactions, configure your application by adding the following line of code:

   ```Swift
   WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
   ```
   {: codeblock}
   {: ios}

1. You now initialized your application to capture analytics data. Next, you must send the captured data to the Mobile Analytics service.
   Use the following API to send analytics data to the Mobile Analytics service.

   ```Swift
   WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

   You are free to decide when to call this API in your application flow to send the captured analytics data to the Mobile Analytics service. Until this, send all captured analytics data is stored locally on the device.
   {: ios}

1. To capture and send application logs to the Mobile Analytics service, use the Mobile Analytics Client SDK logger APIs.
   The Mobile Analytics Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
   * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
   * ERROR - Use for unexpected exceptions or unexpected network protocol errors
   * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
   * INFO - Use for reporting initialization events and other data that might be important, but not urgent
   * DEBUG - Use for reporting debug statements to help developers resolve application defects.

   When the logger level is set DEBUG, you also get Mobile Analytics Client SDK logs, which are included when you send logs.
   {: note}
   {: ios}

   Follow [the tutorial](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/){: external} for code snippets for Logger to add logging capabilities in iOS applications.

1. To get insights on user onboarding patterns (new users versus returning users), you must associate a user identity with your application session. This association can be done by calling the following API:

   ```Swift
   WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
   ```
   {: codeblock}
   {: ios}

   All analytics data that is captured and stored locally is sent to the Mobile Analytics service only when the `WLAnalytics.sharedInstance().send()` API is called.
   {: ios}

1. To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the Mobile Foundation Service Client SDK's WLResourceRequest class to make the HTTP calls. Although you initialized Analytics for the capture of Network events, the use of  `WLResourceRequest` enables the Mobile Analytics to hook into the network calls and capture the relevant data. Make your HTTP calls as in the following code.

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

1. To get Crash Analytics and logs, you might call the following the API during start of your application to check whether the previous session fails and send the capture crash logs to the Mobile Analytics service.

   ```Swift
   if (OCLogger.isUnCaughtExceptionDetected()) {
         //add any other handling you may want and call the following APIs
         WLAnalytics.sharedInstance().send();
         OCLogger.send();
   }
   ```
   {: codeblock}
   {: ios}

1. To define Custom Analytics and define your own analytics data over what is supported inherently in the Client SDK, you might use the custom logging API

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

   The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
   {: ios}

1. Use In-App User Feedback to deepen your application performance analysis. You can enable your application **users and testers** to provide rich contextual feedback to the app owners. **App owners** get real-time feedback from its users on the application usage experience which **App owners** and **Developers** can further act on. This feature brings in significant agility to application up-keep. Use the following API to switch your application to Interactive Feedback Mode in any action handler in your application, for example, when you handle the click of a button or the selection of a menu item.

   ```Swift
   WLAnalytics.sharedInstance().triggerFeedbackMode();
   ```
   {: codeblock}
   {: ios}

## Instrument a Cordova application
{: #instrument_cordova_app}
{: cordova}
Steps to instrument your Cordova application:
{: cordova}

### Step 1: Import and install the Foundation and Analytics Client SDK plug-in for Cordova
{: #install_analytics_sdk_cordova }
The Mobile Analytics Cordova plug-in enables you to instrument your mobile application.
{: cordova}

#### Installing the Cordova Client SDK
{: #install_cordova_sdk}
{: cordova}

1. Create a [Cordova](http://cordova.apache.org/#getstarted){: external} project or open an existing project.
   {: cordova}

1. Add Android or iOS platforms of your choice to your Cordova application. Run one or both of the following commands from the command line.<br/>
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

1. Add the Foundation client sdk plug-in `cordova-plugin-mfp`.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
1. Add optional Analytics client sdk plug-in `cordova-plugin-mfp-analytics` that is for in-app feedback capability to your app
   {: cordova}
   ```bash
   cordova plugin add cordova-plugin-mfp-analytics
   ```
   {: codeblock}
   {: cordova}
1. Verify that the plug-in installed successfully by running the following command:
   {: cordova}
   ```bash
   cordova plugin list
   ```
   {: codeblock}
   {: cordova}

### Step 2: Instrument your Cordova application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. In Cordova applications, no setup is required and initialization is built in.

   Before calling any of the following analytic methods, you must ensure that your application embeds the required code to authenticate and authorize the device with the {{site.data.keyword.mobilefoundation_short}} service. This step is a common step that is required of all {{site.data.keyword.mobilefoundation_short}} services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   With initialization, complete your application is now enabled to capture device information and {{site.data.keyword.mobileanalytics_short}} SDK logs with no further code added. Any further APIs and code that is discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: cordova}

1. To enable the capture of the lifecycle events, it must be initialized in the native platform of the Cordova app.
   * For the Android platform:
      * Open the  **[Cordova application root folder] → platforms → android → src → com → sample → [app-name] → MainActivity.java** file.
      * Look for the `onCreate` method and enable `LIFECYCLE` and `NETWORK` activities by following the steps:
         * To enable client lifecycle event logging, add the following line:
            ```Java
            WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
            ```
            {: codeblock}
            {: cordova}
         * To enable client network-event logging, add the following line:
            ```Java
            WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
            ```
            {: codeblock}
            {: cordova}
      * Build the Cordova project by running the command: `cordova build`.
   * For the iOS platform:
      * Open the **[Cordova application root folder] → platforms → ios → Classes** folder and find the **AppDelegate.swift** (Swift) file.
      * To enable `LIFECYCLE` and `NETWORK` activities, perform the following steps.
         * To enable client lifecycle event logging, add the following line:
            ```Swift
            WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
            ```
            {: codeblock}
            {: cordova}
         * To enable client network-event logging, add the following line:
            ```Swift
            WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
            ```
            {: codeblock}
            {: cordova}
      * Build the Cordova project by running the command: `cordova build`.

1. You now initialized your application to collect analytics data. Next, you can send analytics data to Mobile Analytics.
   Use the following APIs to start recording and sending usage analytics:
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

1. To capture and send application logs to the {{site.data.keyword.mobileanalytics_short}} service, use the {{site.data.keyword.mobileanalytics_short}} Client SDK logger APIs.
   The {{site.data.keyword.mobileanalytics_short}} Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
   * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
   * ERROR - Use for unexpected exceptions or unexpected network protocol errors
   * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
   * INFO - Use for reporting initialization events and other data that might be important, but not urgent
   * DEBUG - Use for reporting debug statements to help developers resolve application defects.
   When the logger level is set DEBUG, you also get {{site.data.keyword.mobileanalytics_short}} Client SDK logs, which are included when you send logs.
   * TRACE - used for method entry and exit points
   {: note}
   {: cordova}

   The following code snippets show sample Logger usage for Cordova:
   ```Javascript
   // Set the logging level (optional)
   // The default setting is DEBUG
   WL.Logger.config({"level": "INFO"});

   //Create a logger instance. You can create multiple loggers to organize your logs as you wish
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

1. To get insights on user onboarding patterns (new users versus returning users), you must associate a user identity with your application session. There is no specific API available from JavaScript level. But this can be done in the native code by calling the following API:

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

    All analytics data that is captured and stored locally is sent to the {{site.data.keyword.mobileanalytics_short}} service only when the WL.Analytics.send() API in JavaScript level [or] WLAnalytics.send() API in android native [or] WLAnalytics.sharedInstance().send() API in iOS native is called.
    {: cordova}

1. To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the {{site.data.keyword.mobilefoundation_short}} Service Client SDK's WLResourceRequest class to make the HTTP calls. Although you initialized Analytics for the capture of Network events, the use of `WLResourceRequest` enables the {{site.data.keyword.mobileanalytics_short}} to hook into the network calls and capture the relevant data. Make your HTTP calls by using following code.
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

1. To define Custom Analytics and define your own analytics data over what is supported inherently in the Client SDK, you might use the custom logging API
   ```Javascript
   //create custom data as key value pair
   WL.Analytics.log({"FromPage" : 'LoginPage'});

   //Send the captured custom data and log to the Mobile Analytics service
   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

   The logged custom data can be plotted over custom charts you can define in the {{site.data.keyword.mobileanalytics_short}} console to derive custom insights.
   {: cordova}

1. Use In-App User Feedback to deepen your application performance analysis. You can enable your application **users and testers** to provide rich contextual feedback to the app owners. **App owners** get real-time feedback from its users on the application usage experience which **App owners** and **Developers** can further act on. This step brings in significant agility to application up-keep. Use the following API to switch your application to Interactive Feedback Mode in any action handler in your application, for example, when you handle the click of a button or the selection of a menu item.
   ```Javascript
   WL.Analytics.triggerFeedbackMode();
   ```
   {: codeblock}
   {: cordova}

## Instrument a Web application
{: #instrument_web_app}
{: web}

Steps to instrument your Web application:
{: web}

### Step 1: Import and install the {{site.data.keyword.mobileanalytics_short}} Client SDK for Web
{: #install_analytics_sdk_web }
{: web}

#### Installing the Web Client SDK
{: #install_web_sdk}
The {{site.data.keyword.mobileanalytics_short}} SDK enables you to instrument your web application.
{: web}

1. Navigate to the root folder of your web application and run the following command. Add the SDK to your web application by using [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)
   ```bash
   npm install ibm-mfp-web-sdk
   ```
   {: codeblock}
   {: web}

### Step 2: Instrument your Web application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_web }
{: web}

1. Reference the ibmmfpf.js and ibmmfpfanalytics.js file in the `head` element of your main html page

   ```html
   <head>
     ....
     <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
     <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
   </head>
   ```
   {: codeblock}
   {: web}

1. Initialize the Mobile Foundation Web SDK by specifying the context root and application ID values in the main JavaScript file of your web application

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

   Before calling any of the following analytic methods, you must ensure that your application embeds the required code to authenticate and authorize the device with the {{site.data.keyword.mobilefoundation_short}} service. This step is a common step that is required of all Mobile Foundation services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   With initialization, complete your application is now enabled to capture device information and {{site.data.keyword.mobileanalytics_short}} SDK logs with no further code added. Any further APIs and code that is discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: web}

1. Add the following line to your application logic to capture application lifecycle events and network events.

   ```Javascript
   ibmmfpfanalytics.logger.config({analyticsCapture: true});
   ```
   {: codeblock}
   {: web}

1. You now initialized your application to capture analytics data. Next, you must send the captured data to the {{site.data.keyword.mobileanalytics_short}} service. Use the following API to send analytics data to the {{site.data.keyword.mobileanalytics_short}} service.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

   You are free to decide when to call this API in your application flow to send the captured analytics data to the M{{site.data.keyword.mobileanalytics_short}} service. Until this sends, all captured analytics data is stored locally on the device.
   {: web}

1. To capture and send application logs to the {{site.data.keyword.mobileanalytics_short}} service, use the {{site.data.keyword.mobileanalytics_short}} Client SDK logger APIs.
   The {{site.data.keyword.mobileanalytics_short}} Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
   * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
   * ERROR - Use for unexpected exceptions or unexpected network protocol errors
   * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
   * INFO - Use for reporting initialization events and other data that might be important, but not urgent
   * DEBUG - Use for reporting debug statements to help developers resolve application defects.
      When the logger level is set DEBUG, you also get {{site.data.keyword.mobileanalytics_short}} Client SDK logs, which are included when you send logs.
   * TRACE - used for method entry and exit points
   {: note}
   {: web}

   The following code snippets show sample Logger usage for Web app:
   ```Javascript
   //Create a logger instance. You can create multiple loggers to organize your logs as you wish

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

   For the web SDK, the level cannot be set by the client. All logging is sent to the server until the configuration is changed by retrieving the server configuration profile.
   {: note}
   {: web}

1. To get insights on user onboarding patterns (new users versus returning users), you must associate a user identity with your application session. This can be done by calling the following API:
   ```Javascript
   ibmmfpfanalytics.setUserContext("userName or userIdentity");
   ```
   {: codeblock}
   {: web}

   All analytics data that is captured and stored locally is sent to the {{site.data.keyword.mobileanalytics_short}} service only when the ibmmfpfanalytics.send() API is called.
   {: web}

1. To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the {{site.data.keyword.mobilefoundation_short}} Service Client SDK's WLResourceRequest class to make the HTTP calls. Although you initialized Analytics for the capture of Network events, the use of `WLResourceRequest` enables the {{site.data.keyword.mobileanalytics_short}} to hook into the network calls and capture the relevant data. Make your HTTP calls by using the following code.
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

1. To define Custom Analytics and define your own analytics data over what is supported inherently in the Client SDK, you might use the custom logging API:

   ```Javascript
   //custom data is sent with the addEvent method
   ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

   //Send the captured custom data and log to the Mobile Analytics service
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

   The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
   {: web}

## What to do next?
{: #next_steps_analytics}

Try a simple sample from [here](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples). In less than 5 minutes you can run the sample application and get a feel of what the API does. You can also notice how analytics shows up as different insights on the Analytics console. 

{{site.data.keyword.mobilefoundation_short}} Analytics Service provides ReST APIs to help developers with importing (POST) and exporting (GET) analytics data.
