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

#	將 Mobile Foundation SDK 新增至應用程式
{: #add_sdk_to_app}

### 將 Android SDK 新增至應用程式
{: android}

開啟 Android Studio，並選擇 Android 視圖，然後選擇 **Gradle Scripts**。選取 `build.gradle (Module: app)` 檔案，然後執行下列步驟以將 Android SDK 新增至 Android 應用程式。
{: android}

1. 將下行新增至 `dependencies` 區段。
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. 將下行新增至 `android` 區段。
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
   {: codeblock}
   {: android}
3. 在 Android 視圖中，開啟 **app → manifests → AndroidManifest.xml** 檔案。將下列許可權新增至 `application` 元素之前。
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. 在現有 `activity` 元素旁邊新增 MobileFirst 使用者介面活動。
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### 將 iOS SDK 新增至應用程式
{: ios}

此 SDK 是 IBM Mobile Foundation 的 Core SDK，其中包含用於實作 Mobile Foundation 安全、授權、記載、呼叫配接器及其他核心功能的 API。請執行下列步驟，以將 iOS SDK 新增至 iOS 應用程式。
{: ios}

1. 移至 iOS 應用程式的根資料夾，並執行下列指令以建立 Podfile。
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. 使用您慣用的程式碼編輯器，開啟 Podfile。
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. 使用下列指令來更新 Pod。
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. 使用下列指令來安裝 Pod。
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. 開啟 [ProjectName].xcworkspace，以 Xcode 開啟專案。
6. 將 `mfpclient.plist` 檔案新增至 Xcode 工作區。
7. 從命令提示字元中，導覽至專案資料夾，並向 Mobile Foundation 實例登錄應用程式。
   ```bash
mfpdev app register
```
   {: codeblock}
   {: ios}

### 將 Cordova SDK 新增至應用程式
{: cordova}

此 SDK 是 IBM Mobile Foundation 的 Core SDK，其中包含用於實作 Mobile Foundation 安全、授權、記載、呼叫配接器及其他核心功能的 API。請執行下列步驟，以將 Cordova SDK 新增至應用程式。
{: cordova}

1. 建立 Cordova 專案。
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. 切換至 Cordova 專案的根目錄。
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. 使用 Cordova CLI，以將一個以上受支援的平台新增至 Cordova 專案。
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. 新增 Mobile Foundation 核心 Cordova 外掛程式。
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. 執行下列指令，以準備應用程式資源。
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. 向 Mobile Foundation 伺服器登錄應用程式。
   ```bash
mfpdev app register
```
   {: codeblock}
   {: cordova}

### 將 React Native SDK 外掛程式新增至應用程式
{: reactnative}

若要將 Mobile Foundation 功能新增至現有 React Native 應用程式，您需要將 `react-native-ibm-mobilefirst` 外掛程式新增至應用程式。`react-native-ibm-mobilefirst` 外掛程式包含 Mobile Foundation SDK。請執行下列步驟，以將 React Native 外掛程式新增至 React Native 應用程式。
{: reactnative}

1. 使用將任何其他 `npm` 外掛程式新增至應用程式的相同方式，來新增此外掛程式。
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. 第一個步驟是建立 React Native 專案，例如，*MobileFirstApp*。使用 React Native CLI 以建立新專案。
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. 接下來，將 React Native 外掛程式新增至應用程式。
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. 鏈結專案，以將所有原生相依關係新增至 React Native 專案。
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. 針對 Android，對 `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`) 進行下列變更。
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. 將 **tools:replace='android:allowBackup'** 新增至 `application` 標籤。
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
7. 針對 iOS，開啟 XCode，並在專案導覽器中，從 `ios` 資料夾拖放 `mfpclient.plist`。此步驟僅適用於 iOS 平台。
{: reactnative}
