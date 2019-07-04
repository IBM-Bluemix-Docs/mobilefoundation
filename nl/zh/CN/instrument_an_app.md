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

# 检测应用程序
{: #instrument_your_app}

Mobile Analytics 是 {{ site.data.keyword.mobilefoundation_short }} 服务中嵌入的功能。Mobile Analytics 为移动应用程序开发者和应用程序所有者提供关键的应用程序使用情况和应用程序性能洞察。

您需要检测移动应用程序以使用 Mobile Analytics 功能来监视应用程序使用情况、性能以及获取其他统计信息。检测应用程序包含以下步骤：

1.  导入并安装 Mobile Analytics 客户机 SDK。
2.  根据要收集的分析数据的类型来检测应用程序。

以下各部分针对每个支持的平台提供这些步骤的详细信息。

### 检测 Android 应用程序
{: #instrument_android_app}
{: android}
#### 步骤 1：导入并安装用于 Android 的 Mobile Analytics 客户机 SDK
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

Mobile Analytics 客户机 SDK 随 Gradle 一起分发，Gradle 是 Android 项目的依赖项管理器。Gradle 会自动从存储库下载工件，并使其可供 Android 应用程序使用。
{: android}

1. 创建 [Android Studio ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://developer.android.com/sdk/index.html){: new_window} 项目或打开现有项目。
{: android}

2. 打开**应用程序模块**中的 `build.gradle` 文件。
{: android}

  Android 项目可能有两个 `build.gradle` 文件：一个用于项目，一个用于应用程序模块。确保使用的是**应用程序模块**文件。
  {: tip}
  {: android}

3. 找到 `build.gradle` 文件中的 `Dependencies` 部分，然后为 {{site.data.keyword.mobileanalytics_short}} 客户机 SDK 添加 compile 依赖项。存储库语句必须类似于以下代码示例：
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

  第一个依赖项针对 Mobile Analytics 客户机 SDK，用于捕获和记录应用程序运行时事件，第二个依赖项用于启用与应用程序用户进行交互时的应用程序内用户反馈。仅当启用应用程序内用户反馈时，才需要第二个依赖项。
  {: android}

4. 通过单击**工具 &gt; Android &gt; 使用 Gradle 文件同步项目**以使用 Gradle 来同步项目。
{: android}

5. 打开 Android 项目的 `AndroidManifest.xml` 文件。您可以在 **app > manifests** 中找到此文件。在 `<manifest>` 元素下添加因特网访问权和位置访问许可权：
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />
 
  ```
  {: codeblock}
  如果使用的 SDK 版本为 1.2 或更高版本，就需要将以下行置于 `AndroidManifest.xml` 文件的 `<application>` 元素内。
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

#### 步骤 2：根据要收集的分析数据的类型来检测 Android 应用程序
{: #instrument_app_based_on_data_android }
{: android}

1. 初始化应用程序，以捕获分析数据并将其发送到 Mobile Analytics 服务。首先，将以下 `import` 语句添加到 application 或 activity 类的开头：
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   接下来，使用 Mobile Analytics 客户机 SDK 来设置或初始化分析数据的捕获。  
   在 Android 应用程序的 main 活动的 `onCreate` 方法中，或在最适合应用程序项目运行的位置中，添加初始化代码。
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   在调用 init 方法之前，必须确保应用程序嵌入必需的代码，以便通过 Mobile Foundation 服务认证和授权设备。此步骤是所有 Mobile Foundation 服务应用程序都需要的通用步骤，而不是特定于分析数据捕获的步骤。<!--  Refer <need to link doc that talks about auth> -->
   {: android}

   初始化完成后，无需添加进一步的代码，应用程序现在就可以捕获设备信息和 Mobile Analytics SDK 日志。以下各部分中讨论的任何进一步 API 和代码都是可选的，可以根据要捕获的分析数据类型进行添加。
   {: android}

2. 要捕获应用程序生命周期事件（例如，应用程序会话和崩溃信息），请通过添加以下代码行来配置应用程序：
    
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. 要捕获用于捕获应用程序网络交互的网络事件，请通过添加以下代码行来配置应用程序：
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. 现在，您已初始化应用程序，可捕获分析数据。接下来，您必须将捕获到的数据发送到 Mobile Analytics 服务。
   使用以下 API 将分析数据发送到 Mobile Analytics 服务。
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  您可以自由决定何时在应用程序流中调用此 API，以将捕获到的分析数据发送到 Mobile Analytics 服务。在此发送操作之前，所有捕获到的分析数据都会本地存储在设备上。
  {: android}

5. 要捕获应用程序日志并将其发送到 Mobile Analytics 服务，请使用 Mobile Analytics 客户机 SDK 记录器 API。     
   Mobile Analytics 客户机 SDK 日志记录框架支持以下日志级别，这些级别按从最不详细到最详细的顺序列出，同时列出建议的使用准则：
    * FATAL - 用于不可恢复的崩溃或挂起。FATAL 级别保留用于记录不可恢复的错误，这些错误对于用户显示为应用程序崩溃
    * ERROR - 用于意外异常或意外网络协议错误
    * WARN - 用于记录未视为严重错误的使用情况警告，例如使用不推荐的 API 或网络响应慢
    * INFO - 用于报告初始化事件以及其他可能重要但不紧急的数据
    * DEBUG - 用于报告调试语句，以帮助开发者解决应用程序缺陷。
    记录器级别设置为 DEBUG 时，还可以获取 Mobile Analytics 客户机 SDK 日志，发送日志时这些日志也包含在内。
   {: note}
   {: android}

   以下代码片段显示了 Android 的样本记录器用法：
   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);
    
   //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
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

6.  要获取有关用户上线模式（新用户与老用户）的洞察，必须将用户身份与应用程序会话相关联。可以通过调用以下 API 来完成此步骤：
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    仅当调用了 WLAnalytics.send() API 时，才会将捕获到并本地存储的所有分析数据发送到 Mobile Analytics 服务。
    {: android}

7.  要获取应用程序 HTTP 网络交互模式的洞察，例如 HTTP API 调用数、请求数和平均响应时间，必须使用 Mobile Foundation 服务客户机 SDK 的 WLResourceRequest 类来发出 HTTP 调用。虽然您已初始化 Analytics 来捕获网络事件，但使用 `WLResourceRequest` 可使 Mobile Analytics 与网络调用挂钩并捕获相关数据。如以下代码中所示生成 HTTP 调用。
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

8.  要获取崩溃分析和日志，可以在应用程序启动期间调用以下 API，以检查先前的会话是否已崩溃，并相应地将捕获到的崩溃日志发送到 Mobile Analytics 服务。
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  要在客户机 SDK 中固有支持的内容的基础上定义定制分析并定义您自己的分析数据，可以使用定制日志记录 API
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

    可以在定制图表（可在 Mobile Analytics 控制台中定义）上绘制记录的定制数据，以获取定制洞察。
    {: android}

10. 使用“应用程序内用户反馈”可深化应用程序性能分析。您可以使应用程序**用户和测试者**能够向应用程序所有者提供丰富的上下文反馈。 **应用程序所有者**从其用户那里获取有关应用程序使用体验的实时反馈，然后**应用程序所有者**和**开发者**可基于这些反馈执行进一步操作。此功能部件为应用程序维护带来极高的敏捷性。使用以下 API 将应用程序切换到应用程序中任何操作处理程序中的“交互式反馈方式”，例如在处理按钮单击或菜单项选择时。
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### 检测 iOS 应用程序
{: #instrument_ios_app}
{: ios}

用于检测 iOS 应用程序的步骤：
{: ios}

#### 步骤 1：导入并安装用于 iOS 的 Mobile Analytics 客户机 SDK
{: #install_analytics_sdk_ios }
{: ios}

[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

Swift SDK 可用于 iOS 和 watchOS。
{: ios}

1. 将以下内容添加到 Xcode 项目的根目录中的现有 podfile
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

2. 在命令行窗口中，导航至 Xcode 项目的根目录，然后运行以下命令：
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### 步骤 2：根据要收集的分析数据的类型来检测 iOS 应用程序
{: #instrument_app_based_on_data_ios }
{: ios}

1. 初始化应用程序，以支持将日志发送到 Mobile Analytics。
   Swift SDK 可用于 iOS 和 watchOS。通过将以下 `import` 语句添加到 `AppDelegate.swift` 项目文件的开头，以导入 `BMSCore` 和 `BMSAnalytics` 框架：
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   接下来，使用 Mobile Analytics 客户机 SDK 来设置或初始化分析数据的捕获。在最适合应用程序项目运行的位置中，添加初始化代码。
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   在调用 init 方法之前，必须确保应用程序嵌入必需的代码，以便通过 Mobile Foundation 服务认证和授权设备。此步骤是所有 Mobile Foundation 服务应用程序都需要的通用步骤，而不是特定于分析数据捕获的步骤。<!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   初始化完成后，无需添加进一步的代码，应用程序现在就可以捕获设备信息和 Mobile Analytics SDK 日志。以下各部分中讨论的任何进一步 API 和代码都是可选的，可以根据要捕获的分析数据类型进行添加。
   {: ios}

2. 要捕获应用程序生命周期事件（例如，应用程序会话和崩溃信息），请通过添加以下代码行来配置应用程序：
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. 要捕获用于捕获应用程序网络交互的网络事件，请通过添加以下代码行来配置应用程序：
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. 现在，您已初始化应用程序，可捕获分析数据。接下来，您必须将捕获到的数据发送到 Mobile Analytics 服务。
   使用以下 API 将分析数据发送到 Mobile Analytics 服务。
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    您可以自由决定何时在应用程序流中调用此 API，以将捕获到的分析数据发送到 Mobile Analytics 服务。在此发送操作之前，所有捕获到的分析数据都会本地存储在设备上。
    {: ios}

5. 要捕获应用程序日志并将其发送到 Mobile Analytics 服务，请使用 Mobile Analytics 客户机 SDK 记录器 API。Mobile Analytics 客户机 SDK 日志记录框架支持以下日志级别，这些级别按从最不详细到最详细的顺序列出，同时列出建议的使用准则：
    * FATAL - 用于不可恢复的崩溃或挂起。FATAL 级别保留用于记录不可恢复的错误，这些错误对于用户显示为应用程序崩溃
    * ERROR - 用于意外异常或意外网络协议错误
    * WARN - 用于记录未视为严重错误的使用情况警告，例如使用不推荐的 API 或网络响应慢
    * INFO - 用于报告初始化事件以及其他可能重要但不紧急的数据
    * DEBUG - 用于报告调试语句，以帮助开发者解决应用程序缺陷。
    记录器级别设置为 DEBUG 时，还可以获取 Mobile Analytics 客户机 SDK 日志，发送日志时这些日志也包含在内。
   {: note}
   {: ios}

    遵循[教程 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) 来获取 Logger 的代码片段，以便在 iOS 应用程序中添加日志记录功能。

6.  要获取有关用户上线模式（新用户与老用户）的洞察，必须将用户身份与应用程序会话相关联。可以通过调用以下 API 来完成此关联：
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    仅当调用了 `WLAnalytics.sharedInstance().send()` API 时，才会将捕获到并本地存储的所有分析数据发送到 Mobile Analytics 服务。
    {: ios}

7.  要获取应用程序 HTTP 网络交互模式的洞察，例如 HTTP API 调用数、请求数和平均响应时间，必须使用 Mobile Foundation 服务客户机 SDK 的 WLResourceRequest 类来发出 HTTP 调用。虽然您已初始化 Analytics 来捕获网络事件，但使用 `WLResourceRequest` 可使 Mobile Analytics 与网络调用挂钩并捕获相关数据。如以下代码中所示生成 HTTP 调用。
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

8.  要获取崩溃分析和日志，可以在应用程序启动期间调用以下 API，以检查先前的会话是否已崩溃，并将捕获到的崩溃日志发送到 Mobile Analytics 服务。
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  要在客户机 SDK 中固有支持的内容的基础上定义定制分析并定义您自己的分析数据，可以使用定制日志记录 API
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

    可以在定制图表（可在 Mobile Analytics 控制台中定义）上绘制记录的定制数据，以获取定制洞察。
    {: ios}

10. 使用“应用程序内用户反馈”可深化应用程序性能分析。您可以使应用程序**用户和测试者**能够向应用程序所有者提供丰富的上下文反馈。 **应用程序所有者**从其用户那里获取有关应用程序使用体验的实时反馈，然后**应用程序所有者**和**开发者**可基于这些反馈执行进一步操作。此功能部件为应用程序维护带来极高的敏捷性。使用以下 API 将应用程序切换到应用程序中任何操作处理程序中的“交互式反馈方式”，例如在处理按钮单击或菜单项选择时。
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### 检测 Cordova 应用程序
{: #instrument_cordova_app}
{: cordova}
用于检测 Cordova 应用程序的步骤：
{: cordova}

#### 步骤 1：导入并安装用于 Cordova 的 Foundation 和 Analytics 客户机 SDK 插件
{: #install_analytics_sdk_cordova }
通过 Mobile Analytics Cordova 插件，可以检测移动应用程序。
{: cordova}

#### 安装 Cordova 客户机 SDK
{: #install_cordova_sdk}
{: cordova}

1. 创建 [Cordova ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://cordova.apache.org/#getstarted){: new_window} 项目或打开现有项目。
    {: cordova}

2. 将选择的 Android 或 iOS 平台添加到 Cordova 应用程序。从命令行运行以下一个或全部两个命令。<br/>
    **Android**：
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS**：
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. 添加 Foundation 客户机 SDK 插件 `cordova-plugin-mfp`。
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. 将适用于应用程序内反馈功能的可选 Analytics 客户机 SDK 插件 `cordova-plugin-mfp-analytics` 添加到应用程序：
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. 通过运行以下命令，验证插件是否已成功安装：
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### 步骤 2：根据要收集的分析数据的类型来检测 Cordova 应用程序
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. 在 Cordova 应用程序中，无需进行设置，并且初始化已内置。

   在调用以下任意分析方法之前，必须确保应用程序嵌入必需的代码，以便通过 Mobile Foundation 服务认证和授权设备。此步骤是所有 Mobile Foundation 服务应用程序都需要的通用步骤，而不是特定于分析数据捕获的步骤。<!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   初始化完成后，无需添加进一步的代码，应用程序现在就可以捕获设备信息和 Mobile Analytics SDK 日志。以下各部分中讨论的任何进一步 API 和代码都是可选的，可以根据要捕获的分析数据类型进行添加。
   {: cordova}

2. 要支持捕获生命周期事件，必须在 Cordova 应用程序的本机平台中对其进行初始化。
    * 对于 Android 平台：
      * 打开 **[Cordova 应用程序根文件夹] → platforms → android → src → com → sample → [app-name] → MainActivity.java** 文件。
      * 查找 `onCreate` 方法，并执行以下步骤来启用 `LIFECYCLE` 和 `NETWORK` 活动：
        * 要启用客户机生命周期事件日志记录，请添加以下行：
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * 要启用客户机网络事件日志记录，请添加以下行：
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * 通过运行以下命令来构建 Cordova 项目：`cordova build`。
    * 对于 iOS 平台：
      * 打开 **[Cordova 应用程序根文件夹] → platforms → ios → Classes** 文件夹，并找到 **AppDelegate.swift** (Swift) 文件。
      * 要启用 `LIFECYCLE` 和 `NETWORK` 活动，请执行以下步骤。
        * 要启用客户机生命周期事件日志记录，请添加以下行：
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * 要启用客户机网络事件日志记录，请添加以下行：
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * 通过运行以下命令来构建 Cordova 项目：`cordova build`。

3. 现在，您已初始化应用程序，可收集分析数据。接下来，可以将分析数据发送到 Mobile Analytics。
   使用以下 API 开始记录和发送使用情况分析：
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. 要捕获应用程序日志并将其发送到 Mobile Analytics 服务，请使用 Mobile Analytics 客户机 SDK 记录器 API。Mobile Analytics 客户机 SDK 日志记录框架支持以下日志级别，这些级别按从最不详细到最详细的顺序列出，同时列出建议的使用准则：
    * FATAL - 用于不可恢复的崩溃或挂起。FATAL 级别保留用于记录不可恢复的错误，这些错误对于用户显示为应用程序崩溃
    * ERROR - 用于意外异常或意外网络协议错误
    * WARN - 用于记录未视为严重错误的使用情况警告，例如使用不推荐的 API 或网络响应慢
    * INFO - 用于报告初始化事件以及其他可能重要但不紧急的数据
    * DEBUG - 用于报告调试语句，以帮助开发者解决应用程序缺陷。
    记录器级别设置为 DEBUG 时，还可以获取 Mobile Analytics 客户机 SDK 日志，发送日志时这些日志也包含在内。
    * TRACE - 用于方法入口和出口点
   {: note}
   {: cordova}

   以下代码片段显示了 Cordova 的样本记录器用法：
    ```Javascript
    // Set the logging level (optional)
    // The default setting is DEBUG
    WL.Logger.config({"level": "INFO"});

    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
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

5.  要获取有关用户上线模式（新用户与老用户）的洞察，必须将用户身份与应用程序会话相关联。JavaScript 级别中没有提供特定 API。但是，可以通过调用以下 API 在本机代码中完成此操作：

    **Android：**
      ```java
        WLAnalytics.setUserContext("userName or userIdentity");
      ```
      {: codeblock}
      {: cordova}

    **iOS：**
      ```Swift
        WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
      ```
      {: codeblock}
      {: cordova}

    仅当调用了 JavaScript 级别的 WL.Analytics.send() API [或] Android 本机中的 WWLAnalytics.send() API [或] iOS 本机中的 WLAnalytics.sharedInstance().send() API 时，才会将捕获到并本地存储的所有分析数据发送到 Mobile Analytics 服务。
    {: cordova}

6. 要获取应用程序 HTTP 网络交互模式的洞察，例如 HTTP API 调用数、请求数和平均响应时间，必须使用 Mobile Foundation 服务客户机 SDK 的 WLResourceRequest 类来发出 HTTP 调用。虽然您已初始化 Analytics 来捕获网络事件，但使用 `WLResourceRequest` 可使 Mobile Analytics 与网络调用挂钩并捕获相关数据。使用以下代码发出 HTTP 调用。
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

7. 要在客户机 SDK 中固有支持的内容的基础上定义定制分析并定义您自己的分析数据，可以使用定制日志记录 API
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    可以在定制图表（可在 Mobile Analytics 控制台中定义）上绘制记录的定制数据，以获取定制洞察。
    {: cordova}

8. 使用“应用程序内用户反馈”可深化应用程序性能分析。您可以使应用程序**用户和测试者**能够向应用程序所有者提供丰富的上下文反馈。 **应用程序所有者**从其用户那里获取有关应用程序使用体验的实时反馈，然后**应用程序所有者**和**开发者**可基于这些反馈执行进一步操作。此步骤为应用程序维护带来极高的敏捷性。使用以下 API 将应用程序切换到应用程序中任何操作处理程序中的“交互式反馈方式”，例如在处理按钮单击或菜单项选择时。
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### 检测 Web 应用程序
{: #instrument_web_app}
{: web}

用于检测 Web 应用程序的步骤：
{: web}

#### 步骤 1：导入并安装用于 Web 的 Mobile Analytics 客户机 SDK
{: #install_analytics_sdk_web }
{: web}

#### 安装 Web 客户机 SDK
{: #install_web_sdk}
通过 Mobile Analytics SDK，可以检测 Web 应用程序。
{: web}

1. 导航至 Web 应用程序的根文件夹，然后运行以下命令。使用 [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk) 将 SDK 添加到 Web 应用程序
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### 步骤 2：根据要收集的分析数据的类型来检测 Web 应用程序
{: #instrument_app_based_on_data_web }
{: web}

1. 引用主 HTML 页面的 `head` 元素中的 ibmmfpf.js 和 ibmmfpfanalytics.js 文件

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. 通过在 Web 应用程序的主 JavaScript 文件中指定上下文根和应用程序标识值，以初始化 Mobile Foundation Web SDK

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

   在调用以下任意分析方法之前，必须确保应用程序嵌入必需的代码，以便通过 Mobile Foundation 服务认证和授权设备。此步骤是所有 Mobile Foundation 服务应用程序都需要的通用步骤，而不是特定于分析数据捕获的步骤。<!--  Refer <need to link doc that talks about auth> -->
   {: web}

   初始化完成后，无需添加进一步的代码，应用程序现在就可以捕获设备信息和 Mobile Analytics SDK 日志。以下各部分中讨论的任何进一步 API 和代码都是可选的，可以根据要捕获的分析数据类型进行添加。
   {: web}

3. 将以下行添加到应用程序逻辑，以捕获应用程序生命周期事件和网络事件。

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. 现在，您已初始化应用程序，可捕获分析数据。接下来，您必须将捕获到的数据发送到 Mobile Analytics 服务。
   使用以下 API 将分析数据发送到 Mobile Analytics 服务。

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    您可以自由决定何时在应用程序流中调用此 API，以将捕获到的分析数据发送到 Mobile Analytics 服务。在此发送操作之前，所有捕获到的分析数据都会本地存储在设备上。
  {: web}

5. 要捕获应用程序日志并将其发送到 Mobile Analytics 服务，请使用 Mobile Analytics 客户机 SDK 记录器 API。Mobile Analytics 客户机 SDK 日志记录框架支持以下日志级别，这些级别按从最不详细到最详细的顺序列出，同时列出建议的使用准则：
    * FATAL - 用于不可恢复的崩溃或挂起。FATAL 级别保留用于记录不可恢复的错误，这些错误对于用户显示为应用程序崩溃
    * ERROR - 用于意外异常或意外网络协议错误
    * WARN - 用于记录未视为严重错误的使用情况警告，例如使用不推荐的 API 或网络响应慢
    * INFO - 用于报告初始化事件以及其他可能重要但不紧急的数据
    * DEBUG - 用于报告调试语句，以帮助开发者解决应用程序缺陷。
    记录器级别设置为 DEBUG 时，还可以获取 Mobile Analytics 客户机 SDK 日志，发送日志时这些日志也包含在内。
    * TRACE - 用于方法入口和出口点
   {: note}
   {: web}

   以下代码片段显示了 Web 应用程序的样本记录器用法：
   ```Javascript
    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    
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

   对于 Web SDK，客户机无法设置级别。所有日志记录都会发送到服务器，直到通过检索服务器配置概要文件来更改配置。
   {: note}
   {: web}

6. 要获取有关用户上线模式（新用户与老用户）的洞察，必须将用户身份与应用程序会话相关联。可以通过调用以下 API 来完成此操作：
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    仅当调用了 ibmmfpfanalytics.send() API 时，才会将捕获到并本地存储的所有分析数据发送到 Mobile Analytics 服务。
    {: web}

7.  要获取应用程序 HTTP 网络交互模式的洞察，例如 HTTP API 调用数、请求数和平均响应时间，必须使用 Mobile Foundation 服务客户机 SDK 的 WLResourceRequest 类来发出 HTTP 调用。虽然您已初始化 Analytics 来捕获网络事件，但使用 `WLResourceRequest` 可使 Mobile Analytics 与网络调用挂钩并捕获相关数据。使用以下代码发出 HTTP 调用。
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

8.  要在客户机 SDK 中固有支持的内容的基础上定义定制分析并定义您自己的分析数据，可以使用定制日志记录 API：

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    可以在定制图表（可在 Mobile Analytics 控制台中定义）上绘制记录的定制数据，以获取定制洞察。
    {: web}

## 下一步做什么？
{: #next_steps_analytics}

尝试执行[此处](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples)的简单样本。您能够在不到 5 分钟的时间内运行样本应用程序并了解 API 的功能。您还会发现分析在 Analytics 控制台上如何呈现为不同的洞察。  

Mobile Foundation Analytics 服务提供了 REST API，可帮助开发者导入 (POST) 和导出 (GET) 分析数据。

请试用[此处](https://ma-server.us-south.mobile-analytics-prod.cloud.ibm.com/analytics-service/)的 Swagger 文档上的分析 REST API。
