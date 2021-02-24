---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: Mobile Foundation SDK, android sdk, iOS sdk, cordova sdk, react native sdk, adding sdks to app

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


# Add Mobile Foundation SDK to an app
{: #add_sdk_to_app}

## Adding the Android SDK to your app

Open Android Studio, choose the Android view and choose **Gradle Scripts**. Select the `build.gradle (Module: app)` file, then perform the following steps to add the Android SDK to your android application.

1. Add the following line to the `dependencies` section.

   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}

1. Add the following line to the `android` section.

   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
   ```
   {: codeblock}

1. In the Android view, open **app → manifests → AndroidManifest.xml** file. Add the following permissions before the `application` element.

   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}

1. Add the MobileFirst UI activity next to the existing `activity` element.

   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}


## Adding the iOS SDK to your app
{: ios-sdk}

This SDK is the Core SDK for {{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} consisting of APIs for implementing {{site.data.keyword.mobilefoundation_short}} security, authorization, logging, calling adapters and other core functions. Perform the following steps to add the iOS SDK to your iOS application.

1. Go to root folder of your iOS app, run the following command to create a Podfile.

   ```bash
   pod init
   ```
   {: codeblock}

1. Using your favorite code editor, open the Podfile.

   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}

1. Update pod by using following command.

   ```bash
   pod update
   ```
   {: codeblock}

1. Install pod by using following command.

   ```bash
   pod install
   ```
   {: codeblock}

1. Open [ProjectName].xcworkspace to open the project in Xcode.
1. Add the `mfpclient.plist` file to Xcode workspace.
1. From a command prompt, navigate to the project folder and register your app with your {{site.data.keyword.mobilefoundation_short}} instance.

   ```bash
   mfpdev app register
   ```
   {: codeblock}

## Adding the Cordova SDK to your app
{: cordova-sdk}

This SDK is the Core SDK for {{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} consisting of APIs for implementing {{site.data.keyword.mobilefoundation_short}} security, authorization, logging, calling adapters and other core functions. Perform the following steps to add the Cordova SDK to your application.

1. Create a Cordova project.

   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}

1. Change directory to the root of the Cordova project.
   ```bash
   cd Hello
   ```
   {: codeblock}

1. Add one or more supported platforms to the Cordova project by using the Cordova CLI.

   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}

1. Add the {{site.data.keyword.mobilefoundation_short}} core Cordova plug-in.

   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}

1. Prepare the application resources by running the following command.

   ```bash
   cordova prepare
   ```
   {: codeblock}

1. Register the application to {{site.data.keyword.mobilefoundation_short}} server.

   ```bash
   mfpdev app register
   ```
   {: codeblock}

## Adding the React Native SDK plug-in to your app
{: reactnative}

To add {{site.data.keyword.mobilefoundation_short}} capabilities to an existing React Native app, you need to add the `react-native-ibm-mobilefirst` plug-in to your app. The `react-native-ibm-mobilefirst` plug-in contains {{site.data.keyword.mobilefoundation_short}} SDK. Perform the following steps to add the React Native plug-in to your React Native application.

1. Add this plug-in in the same way that you add any other `npm` plug-in to your app.

   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}

1. The first step is to create a React Native project, for example, *MobileFirstApp*. Use the React Native CLI to create a new project.

   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}

1. Next, add the react native plug-in to your app.

   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}

1. Link your project so that all native dependencies are added to your React Native project.

   ```bash
   react-native link
   ```
   {: codeblock}

1. For Android, make the following changes to `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`).

   ```xml
   <manifest
          xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}

1. Add **tools:replace='android:allowBackup'** to the `application` tag.

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

1. For iOS, open XCode, in the project navigator, drag and drop `mfpclient.plist` from `ios` folder. This step is applicable only for iOS platform.
