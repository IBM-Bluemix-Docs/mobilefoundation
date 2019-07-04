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

#	向应用程序添加 Mobile Foundation SDK
{: #add_sdk_to_app}

### 向应用程序添加 Android SDK
{: android}

打开 Android Studio，选择 Android 视图，然后选择 **Gradle Scripts**。选择 `build.gradle (Module: app)` 文件，然后执行以下步骤以将 Android SDK 添加到您的 Android 应用程序。
{: android}

1. 将以下行添加到 `dependencies` 部分。
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. 将以下行添加到 `android` 部分。
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. 在 Android 视图中，打开 **app → manifests → AndroidManifest.xml** 文件。在 `application` 元素之前添加以下许可权。
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. 在现有 `activity` 元素旁边添加 MobileFirst UI 活动。
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### 向应用程序添加 iOS SDK
{: ios}

此 SDK 是 IBM Mobile Foundation 的核心 SDK，包含多个 API，用于实现 Mobile Foundation 安全性、授权、日志记录、调用适配器以及其他核心功能。执行以下步骤以将 iOS SDK 添加到 iOS 应用程序。
{: ios}

1. 转至 iOS 应用程序的根文件夹，运行以下命令以创建 podfile。
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. 使用您偏爱的代码编辑器打开 podfile。
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. 使用以下命令更新 pod。
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. 使用以下命令安装 pod。
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. 打开 [ProjectName].xcworkspace 以在 Xcode 中打开项目。
6. 将 `mfpclient.plist` 文件添加到 Xcode 工作空间。
7. 在命令提示符中，导航至项目文件夹，然后向 Mobile Foundation 实例注册应用程序。
   ```bash
mfpdev app register
```
   {: codeblock}
   {: ios}

### 向应用程序添加 Cordova SDK
{: cordova}

此 SDK 是 IBM Mobile Foundation 的核心 SDK，包含多个 API，用于实现 Mobile Foundation 安全性、授权、日志记录、调用适配器以及其他核心功能。执行以下步骤以将 Cordova SDK 添加到应用程序。
{: cordova}

1. 创建 Cordova 项目。
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. 将目录切换至 Cordova 项目的根目录。
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. 使用 Cordova CLI 将一个或多个受支持平台添加到 Cordova 项目。
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. 添加 Mobile Foundation 核心 Cordova 插件。
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. 通过运行以下命令来准备应用程序资源。
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. 向 Mobile Foundation 服务器注册应用程序。
   ```bash
mfpdev app register
```
   {: codeblock}
   {: cordova}

### 向应用程序添加 React Native SDK 插件
{: reactnative}

要向现有 React Native 应用程序添加 Mobile Foundation 功能，您需要将 `react-native-ibm-mobilefirst` 插件添加到应用程序。`react-native-ibm-mobilefirst` 插件包含 Mobile Foundation SDK。执行以下步骤以将 React Native 插件添加到 React Native 应用程序。
{: reactnative}

1. 添加此插件，方法与向应用程序添加其他任何 `npm` 插件一样。
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. 第一步是创建 React Native 项目，例如 *MobileFirstApp*。使用 React Native CLI 来创建新项目。
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. 接下来，将 React Native 插件添加到应用程序。
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. 链接项目，以便将所有本机依赖项添加到 React Native 项目。
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. 对于 Android，对 `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`) 进行以下更改。
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. 将 **tools:replace='android:allowBackup'** 添加到 `application` 标记。
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
7. 对于 iOS，打开 XCode，在项目导航器中，对 `ios` 文件夹中的 `mfpclient.plist` 执行拖放操作。此步骤仅适用于 iOS 平台。
{: reactnative}
