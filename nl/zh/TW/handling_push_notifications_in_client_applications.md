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


# 處理用戶端應用程式中的推送通知
{: #handling_push_notifications_in_client_applications}

在 iOS、Android 及 Windows 原生型應用程式或 Cordova 型應用程式能夠接收及顯示送入的推送通知之前，必須先設定應用程式，而且必須實作 API。
{: shortdesc}

請參閱下列各節，以瞭解如何處理用戶端應用程式中送入的推送通知：

### 處理 Android 中的推送通知
{: #handling_push_notifications_in_android}
{: android}
Android 應用程式能夠處理任何收到的推送通知之前，需要先配置「Google Play 服務」的支援。配置應用程式之後，即可使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 來登錄和取消登錄裝置，以及訂閱和取消訂閱標籤。在本指導教學中，您將瞭解如何處理 Android 應用程式中的推送通知。
{: android}

#### 必要條件
{: #prereqs-andriod}
{: android}
* 要在本端執行的 {{ site.data.keyword.mfserver_short_notm }}，或遠端執行的 {{ site.data.keyword.mfserver_short_notm }}。
* 開發人員工作站上安裝的 {{ site.data.keyword.mobilefirst_notm  }} CLI
{: android}

#### 通知配置
{: #notifications-configuration }
{: android}

建立新的 Android Studio 專案，或使用現有的專案。  
如果 {{ site.data.keyword.mobilefirst_notm }} 原生 Android SDK 尚未存在於專案中，請遵循[將 {{ site.data.keyword.mobilefoundation_short }} SDK 新增至 Android 應用程式](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app)指導教學中的指示。
{: android}

##### 專案設定
{: #project-setup }
{: android}

1. 在 **Android → Gradle Script** 中，選取**build.gradle (Module: app)** 檔案，並將下列幾行新增至 `dependencies`：
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    有一個[已知的 Google 問題報告](https://code.google.com/p/android/issues/detail?id=212879)防止使用最新的「Play 服務」版本（目前是 9.2.0）。請使用低於 9.2.0 的版本。
    {: note}
    {: android}

    及：
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    或在單一行中：
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. 在 **Android → 應用程式 → 資訊清單**中，開啟 `AndroidManifest.xml` 檔案。
    * 將下列許可權新增至 `manifest` 標籤的頂端：

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

    * 將下列項目新增至 `application` 標籤：

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

      請務必將 `your.application.package.name` 取代為應用程式的實際套件名稱。
      {: note}
      {: android}

    * 將下列 `intent-filter` 新增至應用程式的活動。
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### 通知 API
{: #notifications-api }
{: android}

##### MFPPush 實例
{: #mfppush-instance }
{: android}

所有 API 呼叫都必須在 `MFPPush` 實例上進行呼叫。作法是建立類別層次欄位（例如 `private MFPPush push = MFPPush.getInstance();`），然後透過類別呼叫 `push.<api-call>`。
{: android}

或者，您可以針對需要存取推送 API 方法的每個實例呼叫 `MFPPush.getInstance().<api_call>`。
{: android}

##### 盤查處理程式
{: #challenge-handlers }
{: android}

如果 `push.mobileclient` 範圍對映至**安全檢查**，則您必須確定相符的**盤查處理程式**已存在，且在使用任何 Push API 之前登錄。
{: android}

在[認證驗證 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/) 指導教學中進一步瞭解盤查處理程式。
{: note}
{: android}

##### 用戶端
{: #client-side }
{: android}

| Java 方法 | 說明 |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | 起始設定所提供環境定義的 MFPPush。 |
| [`isPushSupported();`](#is-push-supported) | 裝置是否支援推送通知。 |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | 向「Push Notifications 服務」登錄裝置。 |
| [`getTags(MFPPushResponseListener)`](#get-tags) | 擷取推送通知服務實例中可用的標籤。 |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | 訂閱裝置至指定的標籤。 |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | 擷取裝置目前訂閱的所有標籤。 |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | 取消訂閱特定標籤。 |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | 從「Push Notifications 服務」取消登錄裝置 |
{: caption="表 1. Java 方法" caption-side="top"}
{: android}

###### 起始設定
{: #initialization}
{: android}

需要此項目，用戶端應用程式才能使用正確的應用程式環境定義連接至 MFPPush 服務。
{: android}

* 應該先呼叫 API 方法，再使用任何其他 MFPPush API。
* 登錄回呼函數以處理收到的推送通知。
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### 是否支援推送
{: #is-push-supported }
{: android}

檢查裝置是否支援推送通知。
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: android}

###### 登錄裝置
{: #register-device }
{: android}

將裝置登錄至推送通知服務。
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        // Successfully registered
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Registration failed with error
    }
});
```
{: codeblock}
{: android}

###### 取得標籤
{: #get-tags }
{: android}

從推送通知服務擷取所有可用的標籤。
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully retrieved tags as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to receive tags with error
    }
});
```
{: codeblock}
{: android}

###### 訂閱
{: #subscribe }
{: android}

訂閱所需的標籤。
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Subscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to subscribe
    }
});
```
{: codeblock}
{: android}

###### 取得訂閱
{: #get-subscriptions }
{: android}

擷取裝置目前訂閱的標籤。
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully received subscriptions as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to retrieve subscriptions with error
    }
});
```
{: codeblock}
{: android}

###### 取消訂閱
{: #unsubscribe }
{: android}

取消訂閱標籤。
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Unsubscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unsubscribe
    }
});
```
{: codeblock}
{: android}

###### 取消登錄
{: #unregister }
{: android}

從推送通知服務實例中取消登錄裝置。
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Unregistered successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unregister
    }
});
```
{: codeblock}
{: android}

#### 處理推送通知
{: #handling-a-push-notification }
{: android}

為了處理推送通知，您需要設定 `MFPPushNotificationListener`。實作下列其中一種方法即可達成此目的。
{: android}

##### 選項一
{: #option-one }
{: android}

在您想要處理推送通知的活動中。
{: android}

1. 將 `implements MFPPushNofiticationListener` 新增至類別宣告。
2. 透過在 `onCreate` 方法中呼叫 `MFPPush.getInstance().listen(this)`，設定類別使其成為接聽器。
2. 然後，您需要新增下列*必要* 方法：
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. 在此方法中，您將收到 `MFPSimplePushNotification`，而且可以處理通知來進行所需行為。
{: android}

##### 選項二
{: #option-two }
{: android}

透過對 `MFPPush` 實例呼叫 `listen(new MFPPushNofiticationListener())` 來建立接聽器，如下列範例所概述：
```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Handle push notification here
    }
});
```
{: codeblock}
{: android}

#### 將 Android 上的用戶端應用程式移轉至 FCM
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) 已[淘汰](https://developers.google.com/cloud-messaging/faq)並與 Firebase Cloud Messaging (FCM) 整合。Google 將在 2019 年 4 月前關閉大部分的 GCM 服務。
{: android}

如果您使用的是 GCM 專案，[則請將 Android 上的 GCM 用戶端應用程式移轉至 FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)。
{: android}

現在，使用 GCM 服務的現有應用程式會依現狀繼續運作。由於 Push Notifications 服務已更新為使用 FCM 端點，因此，未來所有新的應用程式都必須使用 FCM。
{: android}

移轉至 FCM 之後，請將您的專案更新為使用 FCM 認證，而不是舊的 GCM 認證。
{: note}
{: android}

##### FCM 專案設定
{: #fcm_project_setup}
{: android}

相較於舊的 GCM 模型，在 FCM 中設定應用程式有一些不同。
{: android}

1. 取得您的通知提供者認證，建立 FCM 專案，並將相同的專案新增至 Android 應用程式。將應用程式的套件名稱併入為 `com.ibm.mobilefirstplatform.clientsdk.android.push`。請參閱[這裡的文件](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android)，直到您已完成產生 `google-services.json` 檔案的步驟為止
   {: android}
2. 配置 Gradle 檔案。在應用程式的 `build.gradle` 檔案中新增下列內容：
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

    - 在根 build.gradle 的 `buildscript` 區段 `classpath 'com.google.gms:google-services:3.0.0'` 中新增下列相依關係
      {: codeblock}
      {: android}

    - 從 build.gradle 檔案中移除以下 GCM 外掛程式：`compile  com.google.android.gms:play-services-gcm:+`
     {: android}
 3. 配置 AndroidManifest 檔案。`AndroidManifest.xml` 需要進行下列變更
    {: android}

    移除下列項目：
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

    下列項目需要修改：
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

    將項目修改為：
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

    新增下列項目：
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

 4. 在 Android Studio 中開啟應用程式。複製應用程式目錄內您在**步驟 1** 中建立的 `google-services.json` 檔案。請注意，`google-service.json` 檔案包含您已新增的套件名稱。		
 5. 編譯 SDK。建置應用程式。
{: android}

### 處理 iOS 中的推送通知
{: #handling_push_notifications_in_ios }
{: ios}

可使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 來登錄和取消登錄裝置，以及訂閱和取消訂閱標籤。在本指導教學中，您將瞭解如何使用 Swift 處理 iOS 應用程式中的推送通知。
{: shortdesc}
{: ios}

如需無聲自動通知或互動式通知的相關資訊，請參閱：

* [無聲自動通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [互動式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### 必要條件
{: #prereqs-ios}
{: ios}

* 要在本端執行的 {{ site.data.keyword.mfserver_short }}，或遠端執行的 {{ site.data.keyword.mfserver_short }}。
* 開發人員工作站上安裝的 {{ site.data.keyword.mfp_cli_long_notm }}
{: ios}

#### 通知配置
{: #notifications-configuration_ios}
{: ios}

建立新的 Xcode 專案，或使用現有的 Xcode 專案。如果 {{ site.data.keyword.mobilefirst_notm }} 原生 iOS SDK 尚未存在於專案中，請遵循[將 {{ site.data.keyword.mobilefoundation_short }} SDK 新增至 iOS 應用程式](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/)指導教學中的指示。
{: ios}

#### 新增 Push SDK
{: #adding-the-push-sdk }
{: ios}

1. 開啟專案的現有 **podfile**，並新增下列各行：
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

    - 將 **Xcode-project-target** 取代為 Xcode 專案目標的名稱。
2. 儲存並關閉 **podfile**。
3. 從**指令行**視窗，導覽至專案的根資料夾。
4. 執行 `pod install` 指令
5. 使用 **.xcworkspace** 檔案來開啟專案。
{: ios}

#### 通知 API
{: #notifications-api-ios}
{: ios}

##### MFPPush 實例
{: #mfppush-instance-ios}
{: ios}

所有 API 呼叫都必須在 `MFPPush` 實例上進行呼叫。在視圖控制器中使用 `var`（例如 `var push = MFPPush.sharedInstance();`），然後在整個視圖控制器中呼叫 `push.methodName()`。
{: ios}

或者，您可以針對需要存取推送 API 方法的每個實例呼叫 `MFPPush.sharedInstance().methodName()`。
{: ios}

#### 盤查處理程式
{: #challenge-handlers-ios}
{: ios}

如果 `push.mobileclient` 範圍對映至**安全檢查**，則您必須確定相符的**盤查處理程式**已存在，且在使用任何 Push API 之前登錄。
{: ios}

在[認證驗證 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/) 指導教學中進一步瞭解盤查處理程式。
{: note}
{: ios}

#### 用戶端
{: #client-side-ios}
{: ios}

| Swift 方法 | 說明 |
|---------------|--------------|
| [`initialize()`](#initialization) | 起始設定所提供環境定義的 MFPPush。 |
| [`isPushSupported()`](#is-push-supported) | 裝置是否支援推送通知。 |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | 向「Push Notifications 服務」登錄裝置。 |
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | 將裝置記號傳送至伺服器 |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | 擷取推送通知服務實例中可用的標籤。 |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | 訂閱裝置至指定的標籤。 |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | 擷取裝置目前訂閱的所有標籤。 |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | 取消訂閱特定標籤。 |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | 從「Push Notifications 服務」取消登錄裝置 |
{: caption="表 2. Swift 方法" caption-side="top"}
{: ios}

##### 起始設定
{: #initialization-ios}
{: ios}

需要起始設定，用戶端應用程式才能連接至 MFPPush 服務。
{: ios}

* 應該先呼叫 `initialize` 方法，再使用任何其他 MFPPush API。
* 它會登錄回呼函數以處理收到的推送通知。
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### 是否支援推送
{: #is-push-supported-ios}
{: ios}

檢查裝置是否支援推送通知。
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: ios}

##### 登錄裝置並傳送裝置記號
{: #register-device--send-device-token-ios}
{: ios}

將裝置登錄至推送通知服務。
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

這通常是在 `didRegisterForRemoteNotificationsWithDeviceToken` 方法的 **AppDelegate** 中進行呼叫。
{: note}
{: ios}

##### 取得標籤
{: #get-tags-ios}
{: ios}

從推送通知服務擷取所有可用的標籤。
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

##### 訂閱
{: #subscribe-ios}
{: ios}

訂閱所需的標籤。
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

##### 取得訂閱
{: #get-subscriptions-ios}
{: ios}

擷取裝置目前訂閱的標籤。
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

##### 取消訂閱
{: #unsubscribe-ios}
{: ios}

取消訂閱標籤。
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

##### 取消登錄
{: #unregister-ios}
{: ios}

從推送通知服務實例中取消登錄裝置。
{: ios}

```swift
MFPPush.sharedInstance().unregisterDevice { (response, error)  -> Void in
   if error == nil {
       // Disable buttons
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

#### 處理推送通知
{: #handling-a-push-notification-ios}
{: ios}

推送通知由原生 iOS 架構直接處理。視您的應用程式生命週期而定，iOS 架構將會呼叫不同的方法。
{: ios}

比方說，如果在執行應用程式時收到簡式通知，則會觸發 **AppDelegate** 的 `didReceiveRemoteNotification`：
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // display the alert body
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

從 [Apple 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) 進一步瞭解如何處理 iOS 中的通知。
{: note}
{: ios}

### 處理 Cordova 中的推送通知
{: #handling_push_notifications_in_cordova }
{: cordova}

在 iOS、Android 及 Windows Cordova 應用程式能夠接收及顯示推送通知之前，必須將 **cordova-plugin-mfp-push** Cordova 外掛程式新增至 Cordova 專案。配置應用程式之後，即可使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 來登錄和取消登錄裝置、訂閱和取消訂閱標籤，以及處理修改。在本指導教學中，您將瞭解如何處理 Cordova 應用程式中的推送通知。
{: shortdesc}
{: cordova}

由於某個問題報告，Cordova 應用程式中目前**不支援**已鑑別通知。不過，會提供暫行解決方法：每一個 `MFPPush` API 呼叫都可以用 `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );` 包裝。
{: note}
{: cordova}

如需 iOS 中無聲自動通知或互動式通知的相關資訊，請參閱：
{: cordova}

* [無聲自動通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [互動式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### 必要條件
{: #prereqs-cordova}
{: cordova}

* 要在本端執行的 {{ site.data.keyword.mfserver_short }}，或遠端執行的 {{ site.data.keyword.mfserver_short }}
* 開發人員工作站上安裝的 {{ site.data.keyword.mfp_cli_long_notm }}
* 開發人員工作站上安裝的 Cordova CLI
{: cordova}

#### 通知配置
{: #notifications-configuration-cordova}
{: cordova}

建立新的 Cordova 專案或使用現有的專案，然後新增一個以上的支援平台：iOS、Android、Windows。
{: cordova}

如果 {{ site.data.keyword.mobilefirst_notm }} Cordova SDK 尚未存在於專案中，請遵循[將 {{ site.data.keyword.mobilefoundation_short }} SDK 新增至 Cordova 應用程式](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/)指導教學中的指示。
{: cordova}
{: note}

#### 新增 Push 外掛程式
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. 從**指令行**視窗中，導覽至 Cordova 專案的根目錄。  
2. 執行下列指令，將推送外掛程式新增至：
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. 執行下列指令，以建置 Cordova 專案：
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### iOS 平台
{: #ios-platform }
{: cordova}

iOS 平台需要一個額外步驟。  
{: cordova}

在 Xcode 中，於**功能**畫面中啟用應用程式的推送通知。
{: cordova}

為應用程式選取的組合 ID 必須符合您先前在 Apple Developer 網站中建立的 AppId。
{: important}
{: cordova}

![功能在 Xcode 中位於何處的影像](images/push-capability.png)
{: cordova}

#### Android 平台
{: #android-platform }
{: cordova}

Android 平台需要一個額外步驟。  
{: cordova}

在 Android Studio 中，將下列 `activity` 新增至 `application` 標籤：
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### 通知 API
{: #notifications-api-cordova}
{: cordova}

##### 用戶端
{: #client-side-cordova}
{: cordova}

| Javascript 函數 | 說明 |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | 起始設定 MFPPush 實例。 |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) | 裝置是否支援推送通知。 |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) | 向「Push Notifications 服務」登錄裝置。 |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) | 擷取推送通知服務實例中所有可用的標籤。 |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) | 訂閱特定標籤。 |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions-cordova) | 擷取裝置目前訂閱的標籤 |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) | 從特定標籤取消訂閱。 |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | 從「Push Notifications 服務」取消登錄裝置 |
{: caption="表 3. Javascript 函數" caption-side="top"}
{: cordova}

##### API 實作
{: #api-implementation }
{: cordova}

###### 起始設定
{: #initialization-cordova}
{: cordova}

起始設定 **MFPPush** 實例。
{: cordova}

- 需要起始設定，用戶端應用程式才能使用正確的應用程式環境定義連接至 MFPPush 服務。  
- 應該先呼叫 API 方法，再使用任何其他 MFPPush API。
- 登錄回呼函數以處理收到的推送通知。
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

###### 是否支援推送
{: #is-push-supported-cordova}
{: cordova}

檢查裝置是否支援推送通知。
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

###### 登錄裝置
{: #register-device-cordova}
{: cordova}

將裝置登錄至推送通知服務。如果不需要任何選項，則 options 可以設為 `null`。
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

###### 取得標籤
{: #get-tags-cordova}
{: cordova}

從推送通知服務擷取所有可用的標籤。
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

###### 訂閱
{: #subscribe-cordova}
{: cordova}

訂閱所需的標籤。
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

###### 取得訂閱
{: #get-subscriptions-cordova}
{: cordova}

擷取裝置目前訂閱的標籤。
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

###### 取消訂閱
{: #unsubscribe-cordova}
{: cordova}

取消訂閱標籤。
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

###### 取消登錄
{: #unregister-cordova}
{: cordova}

從推送通知服務實例中取消登錄裝置。
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

#### 處理推送通知
{: #handling-a-push-notification-cordova}
{: cordova}

您可以在已登錄的回呼函數中對其回應物件執行作業，以處理收到的推送通知。
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### 處理 Windows 8.1 Universal 及 Windows 10 UWP 中的推送通知
{: #handling_push_notifications_in_windows }
{: windows}

可使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 來登錄和取消登錄裝置，以及訂閱和取消訂閱標籤。在本指導教學中，您將瞭解如何使用 C# 處理原生 Windows 8.1 Universal 及 Windows 10 UWP 應用程式中的推送通知。
{: windows}

#### 必要條件
{: #prereqs-windows}
{: windows}

* 要在本端執行的 {{ site.data.keyword.mfserver_short_notm }}，或遠端執行的 {{ site.data.keyword.mfserver_short_notm }}。
* 開發人員工作站上安裝的 {{ site.data.keyword.mobilefirst_notm  }} CLI
{: windows}

#### 通知配置
{: #notifications-configuration-windows}
{: windows}

建立新的 Visual Studio 專案，或使用現有的專案。  
{: windows}

如果 {{ site.data.keyword.mobilefirst_notm }} 原生 Windows SDK 尚未存在於專案中，請遵循[將 {{ site.data.keyword.mobilefirst_notm }} SDK 新增至 Windows 應用程式](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/)指導教學中的指示。
{: windows}

#### 新增 Push SDK
{: #adding-the-push-sdk-windows}
{: windows}

1. 選取工具 → NuGet 套件管理程式 → 套件管理程式主控台。
2. 選擇您要安裝 {{ site.data.keyword.mobilefirst_notm }} Push 元件的專案。
3. 執行 **Install-Package IBM.MobileFirstPlatformFoundationPush** 指令來新增 {{ site.data.keyword.mobilefirst_notm }} Push SDK。
{: windows}

#### 必備的 WNS 配置
{: pre-requisite-wns-configuration }
{: windows}

1. 確保應用程式具有「快顯通知」通知功能。這可以在 Package.appxmanifest 中啟用。
2. 確保 `Package Identity Name` 及 `Publisher` 應該以向 WNS 登錄的值進行更新。
3. （選用項目）刪除 TemporaryKey.pfx 檔案。
{: windows}

#### 通知 API
{: #notifications-api-windows}
{: windows}

##### MFPPush 實例
{: #mfppush-instance-windows}
{: windows}

所有 API 呼叫都必須在 `MFPPush` 實例上進行呼叫。作法是建立變數（例如 `private MFPPush PushClient = MFPPush.GetInstance();`），然後透過類別呼叫 `PushClient.methodName()`。
{: windows}

或者，您可以針對需要存取推送 API 方法的每個實例呼叫 `MFPPush.GetInstance().methodName()`。
{: windows}

##### 盤查處理程式
{: #challenge-handlers-windows}
{: windows}

如果 `push.mobileclient` 範圍對映至**安全檢查**，則您必須確定相符的**盤查處理程式**已存在，且在使用任何 Push API 之前登錄。
{: windows}

在[認證驗證 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/) 指導教學中進一步瞭解盤查處理程式。
{: note}
{: windows}

#### 用戶端
{: #client-side-windows}
{: windows}

| C Sharp 方法                                                                                                | 說明 |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            | 起始設定所提供環境定義的 MFPPush。 |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    | 裝置是否支援推送通知。 |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  | 向「Push Notifications 服務」登錄裝置。 |
| [`GetTags()`](#get-tags-windows)                                | 擷取推送通知服務實例中可用的標籤。 |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     | 訂閱裝置至指定的標籤。 |
| [`GetSubscriptions()`](#get-subscriptions-windows)              | 擷取裝置目前訂閱的所有標籤。 |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) | 取消訂閱特定標籤。 |
| [`UnregisterDevice()`](#unregister-windows)                     | 從「Push Notifications 服務」取消登錄裝置 |
{: caption="表 4. C Sharp 方法" caption-side="top"}
{: windows}

##### 起始設定
{: #initialization-windows}
{: windows}

需要起始設定，用戶端應用程式才能連接至 MFPPush 服務。
{: windows}

* 應該先呼叫 `Initialize` 方法，再使用任何其他 MFPPush API。
* 它會登錄回呼函數以處理收到的推送通知。
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### 是否支援推送
{: #is-push-supported-windows}
{: windows}

檢查裝置是否支援推送通知。
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: windows}

##### 登錄裝置並傳送裝置記號
{: #register-device--send-device-token-windows}
{: windows}

將裝置登錄至推送通知服務。
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

##### 取得標籤
{: #get-tags-windows}
{: windows}

從推送通知服務擷取所有可用的標籤。
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

##### 訂閱
{: #subscribe-windows}
{: windows}

訂閱所需的標籤。
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Get subscription tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //successfully subscribed to push tag
}
else
{
    //failed to subscribe to push tags
}
```
{: codeblock}
{: windows}

##### 取得訂閱
{: #get-subscriptions-windows}
{: windows}

擷取裝置目前訂閱的標籤。
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

##### 取消訂閱
{: #unsubscribe-windows}
{: windows}

取消訂閱標籤。
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// unsubscribe tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //succes
}
else
{
    //failed to subscribe to tags
}
```
{: codeblock}
{: windows}

##### 取消登錄
{: #unregister-windows}
{: windows}

從推送通知服務實例中取消登錄裝置。
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

#### 處理推送通知
{: #handling-a-push-notification-windows}
{: windows}

為了處理推送通知，您需要設定 `MFPPushNotificationListener`。實作下列方法即可達成此目的。
{: windows}

1. 使用類型為 MFPPushNotificationListener 的介面來建立類別

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Handle push notification here      
    }
    }
    ```
    {: codeblock}
{: windows}
2. 透過呼叫 `MFPPush.GetInstance().listen(new NotificationListner())`，設定類別使其成為接聽器。
3. 在 onReceive 方法中，您將收到推送通知，而且可以處理通知來進行所需行為。
{: windows}

#### Windows Universal Push Notifications Service
{: #windows-universal-push-notifications-service }
{: windows}

在您的伺服器配置中，不需要開啟特定的埠。
{: windows}

WNS 使用一般 http 或 https 要求。
{: windows}
