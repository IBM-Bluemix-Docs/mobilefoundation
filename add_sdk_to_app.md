---

copyright:
  years: 2018, 2019
lastupdated: "2019-08-30"

keywords: Mobile Foundation SDK, android sdk, iOS sdk, cordova sdk, react native sdk, adding sdks to app

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

#	Add Mobile Foundation SDK to an app
{: #add_sdk_to_app}

### Adding the Android SDK to your app
{: android}

Open Android Studio, choose the Android view and choose **Gradle Scripts**. Select the `build.gradle (Module: app)` file, then perform the following steps to add the Android SDK to your android application.
{: android}

1. Add the following line to the `dependencies` section.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. Add the following line to the `android` section.
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. In the Android view, open **app → manifests → AndroidManifest.xml** file. Add the following permissions before the `application` element.
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. Add the MobileFirst UI activity next to the existing `activity` element.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### Adding the iOS SDK to your app
{: ios}

This SDK is the Core SDK for IBM Mobile Foundation consisting of APIs for implementing Mobile Foundation security, authorization, logging, calling adapters and other core functions. Perform the following steps to add the iOS SDK to your iOS application.
{: ios}

1. Go to root folder of your iOS app, run the following command to create a Podfile.
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. Using your favorite code editor, open the Podfile.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. Update pod by using following command.
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. Install pod by using following command.
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. Open [ProjectName].xcworkspace to open the project in Xcode.
6. Add the `mfpclient.plist` file to Xcode workspace.
7. From a command prompt, navigate to the project folder and register your app with your Mobile Foundation instance.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: ios}

### Adding the Cordova SDK to your app
{: cordova}

This SDK is the Core SDK for IBM Mobile Foundation consisting of APIs for implementing Mobile Foundation security, authorization, logging, calling adapters and other core functions. Perform the following steps to add the Cordova SDK to your application.
{: cordova}

1. Create a Cordova project.
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. Change directory to the root of the Cordova project.
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Add one or more supported platforms to the Cordova project by using the Cordova CLI.
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Add the Mobile Foundation core Cordova plug-in.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. Prepare the application resources by running the following command.
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Register the application to Mobile Foundation server.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### Adding the React Native SDK plug-in to your app
{: reactnative}

To add Mobile Foundation capabilities to an existing React Native app, you need to add the `react-native-ibm-mobilefirst` plug-in to your app. The `react-native-ibm-mobilefirst` plug-in contains Mobile Foundation SDK. Perform the following steps to add the React Native plug-in to your React Native application.
{: reactnative}

1. Add this plug-in in the same way that you add any other `npm` plug-in to your app.
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. The first step is to create a React Native project, for example, *MobileFirstApp*. Use the React Native CLI to create a new project.
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. Next, add the react native plug-in to your app.
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. Link your project so that all native dependencies are added to your React Native project.
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. For Android, make the following changes to `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`).
   ```xml
   <manifest
          xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. Add **tools:replace='android:allowBackup'** to the `application` tag.
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
7. For iOS, open XCode, in the project navigator, drag and drop `mfpclient.plist` from `ios` folder. This step is applicable only for iOS platform.
{: reactnative}
