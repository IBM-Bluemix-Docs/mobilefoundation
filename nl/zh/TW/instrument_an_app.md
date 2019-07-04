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

# 檢測應用程式
{: #instrument_your_app}

Mobile Analytics 是 {{ site.data.keyword.mobilefoundation_short }} 服務內所內嵌的特性。Mobile Analytics 提供行動應用程式開發人員及應用程式擁有者的重要應用程式用法和應用程式效能洞察。

您需要檢測行動應用程式以便使用 Mobile Analytics 特性來監視應用程式用法和效能，以及取得其他統計資料。檢測應用程式具有下列步驟：

1.  匯入及安裝 Mobile Analytics Client SDK。
2.  根據您要收集的分析資料類型，來檢測應用程式。

下列各節將針對每個支援的平台，提供這些步驟的詳細資料。

### 檢測 Android 應用程式
{: #instrument_android_app}
{: android}
#### 步驟 1：匯入及安裝 Mobile Analytics Client SDK for Android
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

Mobile Analytics Client SDK 會與 Android 專案相依關係管理程式 Gradle 一起配送。Gradle 會從儲存庫中自動下載構件，並讓它們可供 Android 應用程式使用。
{: android}

1. 建立 [Android Studio ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://developer.android.com/sdk/index.html){: new_window} 專案或開啟現有專案。
{: android}

2. 開啟**應用程式模組**中的 `build.gradle` 檔案。
{: android}

  您的 Android 專案可能有兩個 `build.gradle` 檔案：一個適用於專案，而一個適用於應用程式模組。請務必使用**應用程式模組**檔案。
  {: tip}
  {: android}

3. 找到 `build.gradle` 檔案的 `Dependencies` 區段，並新增 {{site.data.keyword.mobileanalytics_short}} Client SDK 的編譯相依關係。您的儲存庫陳述式必須類似於下列程式碼範例：
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

  第一個相依關係適用於擷取及記載應用程式運行環境事件的 Mobile Analytics Client SDK，而第二個相依關係適用於啟用與應用程式使用者互動的應用程式內使用者意見。只有在您啟用應用程式內使用者意見時，才需要第二個相依關係
  {: android}

4. 按一下**工具 &gt; Android &gt; 將專案與 Gradle 檔案同步化**，以將專案與 Gradle 同步化。
{: android}

5. 開啟 Android 專案的 `AndroidManifest.xml` 檔案。您可以在**應用程式 > 資訊清單**中找到此檔案。在 `<manifest>` 元素下方，新增網際網路存取及位置存取許可權：
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />
 
  ```
  {: codeblock}
  如果您要使用 1.2 以上的 SDK 版本，則需要將下列各行放入 `AndroidManifest.xml` 檔案的 `<application>` 元素內。
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

#### 步驟 2：根據您要收集的分析資料類型，來檢測 Android 應用程式
{: #instrument_app_based_on_data_android }
{: android}

1. 起始設定應用程式，以擷取分析資料並將其傳送至 Mobile Analytics 服務。首先，將下列 `import` 陳述式新增至應用程式或活動類別開頭處：
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   接下來，使用 Mobile Analytics Client SDK 來設定或起始設定分析資料的擷取。  
   在 Android 應用程式之主要活動的 `onCreate` 方法中，或在最適合您應用程式專案的位置中，新增起始設定碼。
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   呼叫 init 方法之前，您必須確定應用程式內嵌必要程式碼，以使用 MobileFoundation 服務來鑑別及授權裝置。這個步驟是所有 Mobile Foundation 服務應用程式都需要的一般步驟，並不是 Analytics 資料擷取特有的。
   {: android}

   起始設定完成之後，即會立即啟用應用程式來擷取裝置資訊及 Mobile Analytics SDK 日誌，而不需要新增其他程式碼。下列各節中討論的其他任何 API 及程式碼都是選用的，而且可以根據您要擷取的分析資料種類進行新增。
   {: android}

2. 若要擷取應用程式階段作業及損毀資訊這類應用程式生命週期事件，請新增下行程式碼來配置應用程式：
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. 若要擷取可擷取應用程式網路互動的網路事件，請新增下行程式碼指令來配置應用程式：
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. 您現在已起始設定應用程式來擷取分析資料。接下來，您必須將擷取到的資料傳送至 Mobile Analytics 服務。使用下列 API，以將分析資料傳送至 Mobile Analytics 服務。
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  您可以自行決定何時在應用程式流程中呼叫此 API，以將擷取到的分析資料傳送至 Mobile Analytics 服務。在此傳送作業之前，所有擷取到的分析資料會儲存在裝置本端。
  {: android}

5. 若要擷取應用程式日誌並將其傳送至 Mobile Analytics 服務，請使用 Mobile Analytics Client SDK 日誌程式 API。     
   Mobile Analytics Client SDK 記載架構支援下列具有建議使用準則的記載層次（依最不詳細到最詳細程度列出）：
    * FATAL - 用於無法復原的損毀或當掉。FATAL 層次是保留用於記載無法復原的錯誤，這對使用者而言就像應用程式損毀
    * ERROR - 用於非預期的異常狀況或非預期的網路通訊協定錯誤
    * WARN - 用來記載未視為嚴重錯誤的用法警告（例如使用已淘汰的 API 或慢速網路回應）
    * INFO - 用於報告起始設定事件以及可能重要但不緊急的其他資料
    * DEBUG - 用於報告除錯陳述，以協助開發人員解決應用程式問題報告。日誌程式層次設為 DEBUG 時，您還會取得在傳送日誌時包含的 Mobile Analytics Client SDK 日誌。
   {: note}
   {: android}

   下列程式碼 Snippet 顯示 Android 的範例「日誌程式」用法：
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

6.  若要了解使用者上線模式（新使用者與返回的使用者），您必須建立使用者身分與應用程式階段作業的關聯。此步驟可以藉由呼叫下列 API 完成：
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    只有在呼叫 WLAnalytics.send() API 時，才會將所有擷取並儲存在本端的分析資料傳送至 Mobile Analytics 服務。
    {: android}

7.  若要了解已呼叫的 HTTP API、要求數目及平均回應時間這類應用程式 HTTP 網路互動模式，您必須使用 Mobile Foundation Service Client SDK 的 WLResourceRequest 類別來進行 HTTP 呼叫。雖然您已起始設定 Analytics 來擷取「網路」事件，使用 `WLResourceRequest` 可讓 Mobile Analytics 連結至網路呼叫並擷取相關資料。請依照下列程式碼來進行 HTTP 呼叫。
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

8.  若要取得「損毀分析」及日誌，您可以在啟動應用程式期間呼叫下列 API 以確認前一個階段作業是否已損毀，並相應地將擷取損毀日誌傳送至 Mobile Analytics 服務。
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  若要定義「自訂分析」，以及除了 Client SDK 中原本就支援的項目之外還要定義您自己的分析資料，您可以使用自訂記載 API：
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

    記載的自訂資料可以透過您可在 Mobile Analytics 主控台中定義以衍生自訂見解的自訂圖表予以繪製。
    {: android}

10. 使用「應用程式內使用者意見」以加深應用程式效能分析。您可以啟用應用程式**使用者及測試者**，以將豐富的環境定義意見提供給應用程式擁有者。**應用程式擁有者**會取得其使用者對**應用程式擁有者**及**開發人員**可進一步處理之應用程式使用經驗的即時意見。這項特性為應用程式維護帶來極大的靈活性。使用下列 API 以在應用程式的任何動作處理程式中將應用程式切換至「互動式意見模式」，例如，處理按一下按鈕或選取功能表項目時。
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### 檢測 iOS 應用程式
{: #instrument_ios_app}
{: ios}

檢測 iOS 應用程式的步驟：
{: ios}

#### 步驟 1：匯入及安裝 Mobile Analytics Client SDK for iOS
{: #install_analytics_sdk_ios }
{: ios}

[![CocoaPods 相容](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![CocoaPods 相容](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

Swift SDK 適用於 iOS 及 watchOS。
{: ios}

1. 將下列指令新增至位於 Xcode 專案根目錄的現有 podfile
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

2. 從指令行視窗中，導覽至 Xcode 專案根目錄，然後執行下列指令
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### 步驟 2：根據您要收集的分析資料類型，來檢測 iOS 應用程式
{: #instrument_app_based_on_data_ios }
{: ios}

1. 起始設定應用程式，以啟用將日誌傳送至 Mobile Analytics。Swift SDK 適用於 iOS 及 watchOS。將下列 `import` 陳述式新增至 `AppDelegate.swift` 專案檔開頭處，以匯入 `BMSCore` 和 `BMSAnalytics` 架構：
   	```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   接下來，使用 Mobile Analytics Client SDK 來設定或起始設定分析資料的擷取。在最適合您應用程式專案的位置中，新增起始設定碼。
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   呼叫 init 方法之前，您必須確定應用程式內嵌必要程式碼，以使用 MobileFoundation 服務來鑑別及授權裝置。這個步驟是所有 Mobile Foundation 服務應用程式都需要的一般步驟，並不是 Analytics 資料擷取特有的。
   {: ios}

   起始設定完成之後，即會立即啟用應用程式來擷取裝置資訊及 Mobile Analytics SDK 日誌，而不需要新增其他程式碼。下列各節中討論的其他任何 API 及程式碼都是選用的，而且可以根據您要擷取的分析資料種類進行新增。
   {: ios}

2. 若要擷取應用程式階段作業及損毀資訊這類應用程式生命週期事件，請新增下行程式碼來配置應用程式：
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. 若要擷取可擷取應用程式網路互動的網路事件，請新增下行程式碼指令來配置應用程式：
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. 您現在已起始設定應用程式來擷取分析資料。接下來，您必須將擷取到的資料傳送至 Mobile Analytics 服務。使用下列 API，以將分析資料傳送至 Mobile Analytics 服務。
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    您可以自行決定何時在應用程式流程中呼叫此 API，以將擷取到的分析資料傳送至 Mobile Analytics 服務。在這之前，所有擷取到的分析資料會儲存在裝置本端。
    {: ios}

5. 若要擷取應用程式日誌並將其傳送至 Mobile Analytics 服務，請使用 Mobile Analytics Client SDK 日誌程式 API。Mobile Analytics Client SDK 記載架構支援下列具有建議使用準則的記載層次（依最不詳細到最詳細程度列出）：
    * FATAL - 用於無法復原的損毀或當掉。FATAL 層次是保留用於記載無法復原的錯誤，這對使用者而言就像應用程式損毀
    * ERROR - 用於非預期的異常狀況或非預期的網路通訊協定錯誤
    * WARN - 用來記載未視為嚴重錯誤的用法警告（例如使用已淘汰的 API 或慢速網路回應）
    * INFO - 用於報告起始設定事件以及可能重要但不緊急的其他資料
    * DEBUG - 用於報告除錯陳述，以協助開發人員解決應用程式問題報告。日誌程式層次設為 DEBUG 時，您還會取得在傳送日誌時包含的 Mobile Analytics Client SDK 日誌。
   {: note}
   {: ios}

    遵循[指導教學 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) 中「日誌程式」的程式碼 Snippet，以在 iOS 應用程式中新增記載功能。

6.  若要了解使用者上線模式（新使用者與返回的使用者），您必須建立使用者身分與應用程式階段作業的關聯。此關聯可以藉由呼叫下列 API 完成：
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    只有在呼叫 `WLAnalytics.sharedInstance().send()` API 時，才會將所有擷取並儲存在本端的分析資料傳送至 Mobile Analytics 服務。
    {: ios}

7.  若要了解已呼叫的 HTTP API、要求數目及平均回應時間這類應用程式 HTTP 網路互動模式，您必須使用 Mobile Foundation Service Client SDK 的 WLResourceRequest 類別來進行 HTTP 呼叫。雖然您已起始設定 Analytics 來擷取「網路」事件，使用 `WLResourceRequest` 可讓 Mobile Analytics 連結至網路呼叫並擷取相關資料。請依照下列程式碼來進行 HTTP 呼叫。
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

8.  若要取得「損毀分析」及日誌，您可以在啟動應用程式期間呼叫下列 API 以確認前一個階段作業是否已損毀，並將擷取損毀日誌傳送至 Mobile Analytics 服務。
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  若要定義「自訂分析」，以及除了 Client SDK 中原本就支援的項目之外還要定義您自己的分析資料，您可以使用自訂記載 API：
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

    記載的自訂資料可以透過您可在 Mobile Analytics 主控台中定義以衍生自訂見解的自訂圖表予以繪製。
    {: ios}

10. 使用「應用程式內使用者意見」以加深應用程式效能分析。您可以啟用應用程式**使用者及測試者**，以將豐富的環境定義意見提供給應用程式擁有者。**應用程式擁有者**會取得其使用者對**應用程式擁有者**及**開發人員**可進一步處理之應用程式使用經驗的即時意見。這項特性為應用程式維護帶來極大的靈活性。使用下列 API 以在應用程式的任何動作處理程式中將應用程式切換至「互動式意見模式」，例如，處理按一下按鈕或選取功能表項目時。
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### 檢測 Cordova 應用程式
{: #instrument_cordova_app}
{: cordova}
檢測 Cordova 應用程式的步驟：
{: cordova}

#### 步驟 1：匯入及安裝 Foundation and Analytics Client SDK for Cordova 外掛程式
{: #install_analytics_sdk_cordova }
Mobile Analytics Cordova 外掛程式可讓您檢測行動應用程式。
{: cordova}

#### 安裝 Cordova Client SDK
{: #install_cordova_sdk}
{: cordova}

1. 建立 [Cordova ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://cordova.apache.org/#getstarted){: new_window} 專案或開啟現有專案。
    {: cordova}

2. 將您選擇的 Android 或 iOS 平台新增至 Cordova 應用程式。從指令行中執行下列其中一或兩個指令：<br/>
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

3. 新增 Foundation Client SDK 外掛程式 `cordova-plugin-mfp`。
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. 將適用於應用程式內意見功能的選用 Analytics Client SDK 外掛程式 `cordova-plugin-mfp-analytics` 新增至應用程式
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. 執行下列指令，驗證已順利安裝外掛程式：
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### 步驟 2：根據您要收集的分析資料類型，來檢測 Cordova 應用程式
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. 在 Cordova 應用程式中，不需要進行設定，起始設定已經內建。

   在呼叫下列任意分析方法之前，必須確保應用程式內嵌必要的程式碼，以便透過 Mobile Foundation 服務鑑別和授權裝置。這個步驟是所有 Mobile Foundation 服務應用程式都需要的一般步驟，並不是 Analytics 資料擷取特有的。
   {: cordova}

   起始設定完成之後，即會立即啟用應用程式來擷取裝置資訊及 Mobile Analytics SDK 日誌，而不需要新增其他程式碼。下列各節中討論的其他任何 API 及程式碼都是選用的，而且可以根據您要擷取的分析資料種類進行新增。
   {: cordova}

2. 若要啟用生命週期事件的擷取，則必須在 Cordova 應用程式的原生平台中予以起始設定。
    * 針對 Android 平台：
      * 開啟 **[Cordova 應用程式根資料夾] → platforms → android → src → com → sample → [應用程式名稱] → MainActivity.java** 檔案。
      * 尋找 `onCreate` 方法，並遵循下面的步驟來啟用 `LIFECYCLE` 及 `NETWORK` 活動：
        * 若要啟用用戶端生命週期事件記載，請新增下行：
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * 若要啟用用戶端網路事件記載，請新增下行：
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * 執行下列指令，以建置 Cordova 專案：`cordova build`。
    * 針對 iOS 平台：
      * 開啟 **[Cordova 應用程式根資料夾] → platforms → ios → Classes** 資料夾，並找到 **AppDelegate.swift** (Swift) 檔案。
      * 若要啟用 `LIFECYCLE` 及 `NETWORK` 活動，請執行下列步驟。
        * 若要啟用用戶端生命週期事件記載，請新增下行：
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * 若要啟用用戶端網路事件記載，請新增下行：
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * 執行下列指令，以建置 Cordova 專案：`cordova build`。

3. 您現在已起始設定應用程式來收集分析資料。接下來，您可以將分析資料傳送至 Mobile Analytics。使用下列 API 來開始記錄及傳送用量分析：
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. 若要擷取應用程式日誌並將其傳送至 Mobile Analytics 服務，請使用 Mobile Analytics Client SDK 日誌程式 API。Mobile Analytics Client SDK 記載架構支援下列具有建議使用準則的記載層次（依最不詳細到最詳細程度列出）：
    * FATAL - 用於無法復原的損毀或當掉。FATAL 層次是保留用於記載無法復原的錯誤，這對使用者而言就像應用程式損毀
    * ERROR - 用於非預期的異常狀況或非預期的網路通訊協定錯誤
    * WARN - 用來記載未視為嚴重錯誤的用法警告（例如使用已淘汰的 API 或慢速網路回應）
    * INFO - 用於報告起始設定事件以及可能重要但不緊急的其他資料
    * DEBUG - 用於報告除錯陳述，以協助開發人員解決應用程式問題報告。日誌程式層次設為 DEBUG 時，您還會取得在傳送日誌時包含的 Mobile Analytics Client SDK 日誌。
    * TRACE - 用於方法進入及結束點
   {: note}
   {: cordova}

   下列程式碼 Snippet 顯示 Cordova 的範例「日誌程式」用法：
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

5.  若要了解使用者上線模式（新使用者與返回的使用者），您必須建立使用者身分與應用程式階段作業的關聯。沒有來自 Javascript 層次的特定 API。但是，在原生程式碼的作法是呼叫下列 API：

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

    只有在 javascript 層次中呼叫 WL.Analytics.send() API [或] 在 android 原生中呼叫 WLAnalytics.send() API [或] 在 iOS 原生中呼叫 WLAnalytics.sharedInstance().send() API 時，才會將所有擷取並儲存在本端的分析資料傳送至 Mobile Analytics 服務。
    {: cordova}

6. 若要了解已呼叫的 HTTP API、要求數目及平均回應時間這類應用程式 HTTP 網路互動模式，您必須使用 Mobile Foundation Service Client SDK 的 WLResourceRequest 類別來進行 HTTP 呼叫。雖然您已起始設定 Analytics 來擷取「網路」事件，使用 `WLResourceRequest` 可讓 Mobile Analytics 連結至網路呼叫並擷取相關資料。請使用下列程式碼來進行 HTTP 呼叫。
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

7. 若要定義「自訂分析」，以及除了 Client SDK 中原本就支援的項目之外還要定義您自己的分析資料，您可以使用自訂記載 API：
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    記載的自訂資料可以透過您可在 Mobile Analytics 主控台中定義以衍生自訂見解的自訂圖表予以繪製。
    {: cordova}

8. 使用「應用程式內使用者意見」以加深應用程式效能分析。您可以啟用應用程式**使用者及測試者**，以將豐富的環境定義意見提供給應用程式擁有者。**應用程式擁有者**會取得其使用者對**應用程式擁有者**及**開發人員**可進一步處理之應用程式使用經驗的即時意見。此步驟為應用程式維護帶來極大的靈活性。使用下列 API 以在應用程式的任何動作處理程式中將應用程式切換至「互動式意見模式」，例如，處理按一下按鈕或選取功能表項目時。
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### 檢測 Web 應用程式
{: #instrument_web_app}
{: web}

檢測 Web 應用程式的步驟：
{: web}

#### 步驟 1：匯入及安裝 Mobile Analytics Client SDK for Web
{: #install_analytics_sdk_web }
{: web}

#### 安裝 Web Client SDK
{: #install_web_sdk}
Mobile Analytics SDK 可讓您檢測 Web 應用程式。
{: web}

1. 導覽至 Web 應用程式的根資料夾，以及執行下列指令。使用 [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)，將 SDK 新增至 Web 應用程式
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### 步驟 2：根據您要收集的分析資料類型，來檢測 Web 應用程式
{: #instrument_app_based_on_data_web }
{: web}

1. 在主要 html 頁面的 `head` 元素中參照 ibmmfpf.js 及 ibmmfpfanalytics.js 檔案

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. 在 Web 應用程式的主要 JavaScript 檔案中指定環境定義根目錄及應用程式 ID 值，以起始設定 Mobile Foundation Web SDK

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

   在呼叫下列任意分析方法之前，必須確保應用程式內嵌必要的程式碼，以便透過 Mobile Foundation 服務鑑別和授權裝置。這個步驟是所有 Mobile Foundation 服務應用程式都需要的一般步驟，並不是 Analytics 資料擷取特有的。
   {: web}

   起始設定完成之後，即會立即啟用應用程式來擷取裝置資訊及 Mobile Analytics SDK 日誌，而不需要新增其他程式碼。下列各節中討論的其他任何 API 及程式碼都是選用的，而且可以根據您要擷取的分析資料種類進行新增。
   {: web}

3. 將下行新增至應用程式邏輯，以擷取應用程式生命週期事件及網路事件。

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. 您現在已起始設定應用程式來擷取分析資料。接下來，您必須將擷取到的資料傳送至 Mobile Analytics 服務。使用下列 API，以將分析資料傳送至 Mobile Analytics 服務。

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    您可以自行決定何時在應用程式流程中呼叫此 API，以將擷取到的分析資料傳送至 Mobile Analytics 服務。在此傳送作業之前，所有擷取到的分析資料會儲存在裝置本端。
  {: web}

5. 若要擷取應用程式日誌並將其傳送至 Mobile Analytics 服務，請使用 Mobile Analytics Client SDK 日誌程式 API。Mobile Analytics Client SDK 記載架構支援下列具有建議使用準則的記載層次（依最不詳細到最詳細程度列出）：
    * FATAL - 用於無法復原的損毀或當掉。FATAL 層次是保留用於記載無法復原的錯誤，這對使用者而言就像應用程式損毀
    * ERROR - 用於非預期的異常狀況或非預期的網路通訊協定錯誤
    * WARN - 用來記載未視為嚴重錯誤的用法警告（例如使用已淘汰的 API 或慢速網路回應）
    * INFO - 用於報告起始設定事件以及可能重要但不緊急的其他資料
    * DEBUG - 用於報告除錯陳述，以協助開發人員解決應用程式問題報告。日誌程式層次設為 DEBUG 時，您還會取得在傳送日誌時包含的 Mobile Analytics Client SDK 日誌。
    * TRACE - 用於方法進入及結束點
   {: note}
   {: web}

   下列程式碼 Snippet 顯示 Web 應用程式的範例「日誌程式」用法：
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

   針對 Web SDK，無法由用戶端設定層次。除非擷取伺服器配置設定檔來變更配置，否則會將所有記載傳送至伺服器。
   {: note}
   {: web}

6. 若要了解使用者上線模式（新使用者與返回的使用者），您必須建立使用者身分與應用程式階段作業的關聯。作法是呼叫下列 API
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    只有在呼叫 ibmmfpfanalytics.send() API 時，才會將所有擷取並儲存在本端的分析資料傳送至 Mobile Analytics 服務。
    {: web}

7.  若要了解已呼叫的 HTTP API、要求數目及平均回應時間這類應用程式 HTTP 網路互動模式，您必須使用 Mobile Foundation Service Client SDK 的 WLResourceRequest 類別來進行 HTTP 呼叫。雖然您已起始設定 Analytics 來擷取「網路」事件，使用 `WLResourceRequest` 可讓 Mobile Analytics 連結至網路呼叫並擷取相關資料。請使用下列程式碼來進行 HTTP 呼叫。
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

8.  若要定義「自訂分析」，以及除了 Client SDK 中原本就支援的項目之外還要定義您自己的分析資料，您可以使用自訂記載 API：

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    記載的自訂資料可以透過您可在 Mobile Analytics 主控台中定義以衍生自訂見解的自訂圖表予以繪製。
    {: web}

## 下一步為何？
{: #next_steps_analytics}

試試[這個](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples)簡單的範例。不到 5 分鐘的時間，您就能夠執行這個範例應用程式，感受一下 API 功能。您也會注意到分析如何在「分析主控台」上顯示出不同的見解。  

Mobile Foundation Analytics Service 提供 REST API 協助開發人員匯入 (POST) 及匯出 (GET) 分析資料。

請在[這裡](https://ma-server.us-south.mobile-analytics-prod.cloud.ibm.com/analytics-service/)的 Swagger Docs 上嘗試分析 REST API。
