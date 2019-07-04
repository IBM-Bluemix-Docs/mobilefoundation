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


# 在客户机应用程序中处理推送通知
{: #handling_push_notifications_in_client_applications}

要使 iOS、Android 和 Windows 基于本机或基于 Cordova 的应用程序能够接收和显示入局推送通知，必须先设置该应用程序并且必须实现 API。
{: shortdesc}

请参阅以下部分以了解如何在客户机应用程序中处理入局推送通知：

### 在 Android 中处理推送通知
{: #handling_push_notifications_in_android}
{: android}
要使 Android 应用程序能够处理已收到的任何推送通知，需要配置 Google Play 服务支持。在配置应用程序后，可以使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 来注册和注销设备以及预订和取消预订标记。在本教程中，您将了解如何在 Android 应用程序中处理推送通知。
{: android}

#### 先决条件
{: #prereqs-andriod}
{: android}
* 本地运行的 {{ site.data.keyword.mfserver_short_notm }} 或远程运行的 {{ site.data.keyword.mfserver_short_notm }}。
* 安装在开发者工作站上的 {{ site.data.keyword.mobilefirst_notm  }} CLI
{: android}

#### 通知配置
{: #notifications-configuration }
{: android}

创建新的 Android Studio 项目或使用现有项目。  
如果该项目中还没有 {{ site.data.keyword.mobilefirst_notm }} 本机 Android SDK，请遵循[将 {{ site.data.keyword.mobilefoundation_short }} SDK 添加到 Android 应用程序](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app)教程中的指示信息。
{: android}

##### 项目设置
{: #project-setup }
{: android}

1. 在 **Android → Gradle Scripts** 中，选择 **build.gradle (Module: app)** 文件，并将以下行添加到 `dependencies` 中：
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    因为有一个[已知 Google 缺陷](https://code.google.com/p/android/issues/detail?id=212879)，无法使用最新版本（当前为 9.2.0）的 Play 服务。请使用低于 9.2.0 的版本。
    {: note}
    {: android}

    以及：
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    或在单独一行中：
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. 在 **Android → app → manifests** 中，打开 `AndroidManifest.xml` 文件。
    * 向顶部的 `manifest` 标记中添加以下许可权：

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

    * 向 `application` 标记添加以下内容：

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

      确保将 `your.application.package.name` 替换为应用程序的实际程序包名。
      {: note}
      {: android}

    * 将以下 `intent-filter` 添加到应用程序的活动。
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

##### MFPPush 实例
{: #mfppush-instance }
{: android}

必须在一个 `MFPPush` 实例上发出所有 API 调用。为此，可以创建一个类级别字段，例如，`private MFPPush push = MFPPush.getInstance();`，然后在这整个类中调用 `push.<api-call>`。
{: android}

或者，也可以针对您需要在其中访问推送 API 方法的每个实例都调用 `MFPPush.getInstance().<api_call>`。
{: android}

##### 质询处理程序
{: #challenge-handlers }
{: android}

如果 `push.mobileclient` 作用域映射到**安全性检查**，那么需要确保在使用任何推送 API 之前，存在已注册的匹配**质询处理程序**。
{: android}

在[凭证验证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/) 教程中了解有关质询处理程序的更多信息。
{: note}
{: android}

##### 客户机端
{: #client-side }
{: android}

|Java 方法 | 描述|
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
|[`initialize(Context context);`](#initialization) |针对提供的上下文，初始化 MFPPush。                               |
|[`isPushSupported();`](#is-push-supported) |设备是否支持推送通知。 |
|[`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) |向推送通知服务注册设备。               |
|[`getTags(MFPPushResponseListener)`](#get-tags) |在推送通知服务实例中检索可用的标记。 |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) |使设备预订指定的标记。                          |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) |检索设备当前预订的所有标记。               |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) |取消对特定标记的预订。 |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | 从推送通知服务注销设备。 |
{: caption="表 1. Java 方法" caption-side="top"}
{: android}

###### 初始化
{: #initialization}
{: android}

在客户机应用程序使用正确的应用程序上下文连接到 MFPPush 服务时为必需项。
{: android}

* 应先调用此 API 方法，然后再使用任何其他 MFPPush API。
* 注册回调函数以处理已收到的推送通知。
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### 是否支持推送
{: #is-push-supported }
{: android}

检查设备是否支持推送通知。
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

###### 注册设备
{: #register-device }
{: android}

向推送通知服务注册设备。
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

###### 获取标记
{: #get-tags }
{: android}

从推送通知服务检索所有可用标记。
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

###### 预订
{: #subscribe }
{: android}

预订所需的标记。
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

###### 获取预订
{: #get-subscriptions }
{: android}

检索设备当前预订的标记。
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

###### 取消预订
{: #unsubscribe }
{: android}

取消对标记的预订。
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

###### 注销
{: #unregister }
{: android}

从推送通知服务实例注销设备。
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

#### 处理推送通知
{: #handling-a-push-notification }
{: android}

要处理推送通知，需要设置 `MFPPushNotificationListener`。可实现以下某种方法来执行此操作。
{: android}

##### 选项 1
{: #option-one }
{: android}

在要处理推送通知的活动中。
{: android}

1. 向类声明中添加 `implements MFPPushNofiticationListener`。
2. 通过在 `onCreate` 方法中调用 `MFPPush.getInstance().listen(this)`，将该类设置为侦听器。
2. 然后，需要添加以下*必需*方法：
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. 在此方法中，您将收到 `MFPSimplePushNotification`，并可以处理关于期望行为的通知。
{: android}

##### 选项 2
{: #option-two }
{: android}

通过对 `MFPPush` 实例调用 `listen(new MFPPushNofiticationListener())` 来创建侦听器，如以下示例所概述：
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

#### 将 Android 上的客户机应用程序迁移到 FCM
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) 已[不推荐使用](https://developers.google.com/cloud-messaging/faq)，它已经与 Firebase Cloud Messaging (FCM) 集成。Google 将在 2019 年 4 月之前关闭大多数 GCM 服务。
{: android}

如果您正在使用 GCM 项目，请[将 Android 上的 GCM 客户机应用程序迁移到 FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)。
{: android}

目前，使用 GCM 服务的现有应用程序将继续按原样运行。由于推送通知服务已更新为使用 FCM 端点，因此以后所有新的应用程序都必须使用 FCM。
{: android}

迁移到 FCM 后，更新项目以使用 FCM 凭证而不是旧的 GCM 凭证。
{: note}
{: android}

##### FCM 项目设置
{: #fcm_project_setup}
{: android}

与旧的 GCM 模型相比，在 FCM 中设置应用程序稍有不同。
{: android}

1. 获取通知提供程序凭证，创建一个 FCM 项目并将相同内容添加到 Android 应用程序。包含应用程序包名称 `com.ibm.mobilefirstplatform.clientsdk.android.push`。请参阅[此处的文档](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android)，直至您已完成生成 `google-services.json` 文件的步骤
   {: android}
2. 配置您的 Gradle 文件。在应用程序的 `build.gradle` 文件中添加以下内容
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

    - 将以下依赖项添加到根 build.gradle 的 `buildscript` 部分：
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - 从 build.gradle 文件中除去以下 GCM 插件：`compile  com.google.android.gms:play-services-gcm:+`
     {: android}
 3. 配置 AndroidManifest 文件。`AndroidManifest.xml` 中需要进行以下更改
    {: android}

    除去以下条目：
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

    以下条目需要修改：
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

    将条目修改为：
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

    添加以下条目：
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

 4. 在 Android Studio 中打开应用程序。复制您在**步骤 1** 中在应用程序目录内创建的 `google-services.json` 文件。请注意，`google-service.json` 文件包含您已添加的程序包名称。		
 5. 编译 SDK。构建应用程序。
{: android}

### 在 iOS 中处理推送通知
{: #handling_push_notifications_in_ios }
{: ios}

可以使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 来注册和注销设备以及预订和取消预订标记。在本教程中，您将了解如何在使用 Swift 的 iOS 应用程序中处理推送通知。
{: shortdesc}
{: ios}

有关静默通知或交互式通知的信息，请参阅：

* [静默通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [交互式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### 先决条件
{: #prereqs-ios}
{: ios}

* 本地运行的 {{ site.data.keyword.mfserver_short }} 或远程运行的 {{ site.data.keyword.mfserver_short }}。
* 安装在开发者工作站上的 {{ site.data.keyword.mfp_cli_long_notm }}
{: ios}

#### 通知配置
{: #notifications-configuration_ios}
{: ios}

创建新的 Xcode 项目或使用现有项目。
如果该项目中还没有 {{ site.data.keyword.mobilefirst_notm }} 本机 iOS SDK，请遵循[将 {{ site.data.keyword.mobilefoundation_short }} SDK 添加到 iOS 应用程序](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/)教程中的指示信息。
{: ios}

#### 添加推送 SDK
{: #adding-the-push-sdk }
{: ios}

1. 打开该项目的现有 **podfile**，然后添加以下行：
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

    - 将 **Xcode-project-target** 替换为 Xcode 项目目标的名称。
2. 保存并关闭该 **podfile**。
3. 从**命令行**窗口中，导航至该项目的根文件夹。
4. 运行 `pod install` 命令。
5. 通过 **.xcworkspace** 文件打开项目。
{: ios}

#### 通知 API
{: #notifications-api-ios}
{: ios}

##### MFPPush 实例
{: #mfppush-instance-ios}
{: ios}

必须在一个 `MFPPush` 实例上发出所有 API 调用。为此，需要在视图控制器中使用 `var`（例如，`var push = MFPPush.sharedInstance();`），然后在视图控制器中调用 `push.methodName()`。
{: ios}

或者，也可以针对您需要在其中访问推送 API 方法的每个实例都调用 `MFPPush.sharedInstance().methodName()`。
{: ios}

#### 质询处理程序
{: #challenge-handlers-ios}
{: ios}

如果 `push.mobileclient` 作用域映射到**安全性检查**，那么需要确保在使用任何推送 API 之前，存在已注册的匹配**质询处理程序**。
{: ios}

在[凭证验证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/) 教程中了解有关质询处理程序的更多信息。
{: note}
{: ios}

#### 客户机端
{: #client-side-ios}
{: ios}

|Swift 方法 | 描述|
|---------------|--------------|
| [`initialize()`](#initialization) |针对提供的上下文，初始化 MFPPush。                               |
| [`isPushSupported()`](#is-push-supported) |设备是否支持推送通知。 |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) |向推送通知服务注册设备。               |
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) |将设备令牌发送到服务器。|
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) |在推送通知服务实例中检索可用的标记。 |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) |使设备预订指定的标记。                          |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  |检索设备当前预订的所有标记。               |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) |取消对特定标记的预订。 |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | 从推送通知服务注销设备。 |
{: caption="表 2. Swift 方法" caption-side="top"}
{: ios}

##### 初始化
{: #initialization-ios}
{: ios}

客户机应用程序连接到 MFPPush 服务时，需要执行初始化。
{: ios}

* 应先调用 `initialize` 方法，然后再使用任何其他 MFPPush API。
* 它会注册回调函数以处理已收到的推送通知。
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### 是否支持推送
{: #is-push-supported-ios}
{: ios}

检查设备是否支持推送通知。
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

##### 注册设备并发送设备令牌
{: #register-device--send-device-token-ios}
{: ios}

向推送通知服务注册设备。
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

这通常在 **AppDelegate** 的 `didRegisterForRemoteNotificationsWithDeviceToken` 方法中调用。
{: note}
{: ios}

##### 获取标记
{: #get-tags-ios}
{: ios}

从推送通知服务检索所有可用标记。
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

##### 预订
{: #subscribe-ios}
{: ios}

预订所需的标记。
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

##### 获取预订
{: #get-subscriptions-ios}
{: ios}

检索设备当前预订的标记。
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

##### 取消预订
{: #unsubscribe-ios}
{: ios}

取消对标记的预订。
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

##### 注销
{: #unregister-ios}
{: ios}

从推送通知服务实例注销设备。
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

#### 处理推送通知
{: #handling-a-push-notification-ios}
{: ios}

由本机 iOS 框架直接处理推送通知。根据应用程序生命周期，iOS 框架将调用不同的方法。
{: ios}

例如，如果在运行应用程序时收到简单通知，那么将触发 **AppDelegate** 的 `didReceiveRemoteNotification`：
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

从 [Apple 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) 了解有关在 iOS 中处理通知的更多信息。
{: note}
{: ios}

### 在 Cordova 中处理推送通知
{: #handling_push_notifications_in_cordova }
{: cordova}

要使 iOS、Android 和 Windows Cordova 应用程序能够接收和显示推送通知，需要先将 **cordova-plugin-mfp-push** Cordova 插件添加到 Cordova 项目中。在配置应用程序后，可以使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 来注册和注销设备、预订和取消预订标记以及处理通知。在本教程中，您将了解如何在 Cordova 应用程序中处理推送通知。
{: shortdesc}
{: cordova}

由于缺陷，在 Cordova 应用程序中当前**不支持**已认证的通知。但是，提供了以下变通方法：可通过 `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );` 来包装每个 `MFPPush` API 调用。
{: note}
{: cordova}

有关 iOS 中的静默通知或交互式通知的信息，请参阅：
{: cordova}

* [静默通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [交互式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### 先决条件
{: #prereqs-cordova}
{: cordova}

* 本地运行的 {{ site.data.keyword.mfserver_short }} 或远程运行的 {{ site.data.keyword.mfserver_short }}
* 安装在开发者工作站上的 {{ site.data.keyword.mfp_cli_long_notm }}
* 安装在开发者工作站上的 Cordova CLI
{: cordova}

#### 通知配置
{: #notifications-configuration-cordova}
{: cordova}

创建新的 Cordova 项目或使用现有项目，并添加一个或多个受支持的平台：iOS、Android 或 Windows。
{: cordova}

如果该项目中还没有 {{ site.data.keyword.mobilefirst_notm }} Cordova SDK，请遵循[将 {{ site.data.keyword.mobilefirst_notm }} SDK 添加到 Cordova 应用程序](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/)教程中的指示信息。

{: cordova}
{: note}

#### 添加推送插件
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. 从**命令行**窗口中，导航至 Cordova 项目的根目录。  
2. 运行以下命令以添加推送插件：
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. 运行以下命令以构建 Cordova 项目：
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### iOS 平台
{: #ios-platform }
{: cordova}

iOS 平台需要一个额外步骤。  
{: cordova}

在 Xcode 的**功能**屏幕中，为您的应用程序启用推送通知。
{: cordova}

为应用程序选择的 bundleId 必须与先前在 Apple Developer 站点中创建的 AppId 相匹配。
{: important}
{: cordova}

![Xcode 中功能位置的图像](images/push-capability.png)
{: cordova}

#### Android 平台
{: #android-platform }
{: cordova}

Android 平台需要一个额外步骤。  
{: cordova}

在 Android Studio 中，将以下 `activity` 添加到 `application` 标记：
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### 通知 API
{: #notifications-api-cordova}
{: cordova}

##### 客户机端
{: #client-side-cordova}
{: cordova}

|Javascript 函数 | 描述|
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | 初始化 MFPPush 实例。 |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) |设备是否支持推送通知。 |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) |向推送通知服务注册设备。               |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) |在推送通知服务实例中检索所有可用的标记。 |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) |预订特定标记。 |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions-cordova) |检索设备当前预订的标记。 |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) |取消对特定标记的预订。 |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | 从推送通知服务注销设备。 |
{: caption="表 3. Javascript 函数" caption-side="top"}
{: cordova}

##### API 实现
{: #api-implementation }
{: cordova}

###### 初始化
{: #initialization-cordova}
{: cordova}

初始化 **MFPPush** 实例。
{: cordova}

- 在客户机应用程序使用适当的应用程序上下文连接到 MFPPush 服务时为必需项。  
- 应先调用此 API 方法，然后再使用任何其他 MFPPush API。
- 注册回调函数以处理已收到的推送通知。
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

###### 是否支持推送
{: #is-push-supported-cordova}
{: cordova}

检查设备是否支持推送通知。
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

###### 注册设备
{: #register-device-cordova}
{: cordova}

向推送通知服务注册设备。如果不需要任何选项，可将 options 设置为 `null`。
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

###### 获取标记
{: #get-tags-cordova}
{: cordova}

从推送通知服务检索所有可用标记。
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

###### 预订
{: #subscribe-cordova}
{: cordova}

预订所需的标记。
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

###### 获取预订
{: #get-subscriptions-cordova}
{: cordova}

检索设备当前预订的标记。
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

###### 取消预订
{: #unsubscribe-cordova}
{: cordova}

取消对标记的预订。
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

###### 注销
{: #unregister-cordova}
{: cordova}

从推送通知服务实例注销设备。
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

#### 处理推送通知
{: #handling-a-push-notification-cordova}
{: cordova}

通过在已注册的回调函数中对响应对象执行操作，可以处理已收到的推送通知。
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### 在 Windows 8.1 Universal 和 Windows 10 UWP 中处理推送通知
{: #handling_push_notifications_in_windows }
{: windows}

可以使用 {{ site.data.keyword.mobilefirst_notm }} 提供的通知 API 来注册和注销设备以及预订和取消预订标记。在本教程中，您将了解如何在使用 C# 的本机 Windows 8.1 Universal 和 Windows 10 UWP 应用程序中处理推送通知。
{: windows}

#### 先决条件
{: #prereqs-windows}
{: windows}

* 本地运行的 {{ site.data.keyword.mfserver_short_notm }} 或远程运行的 {{ site.data.keyword.mfserver_short_notm }}。
* 安装在开发者工作站上的 {{ site.data.keyword.mobilefirst_notm  }} CLI
{: windows}

#### 通知配置
{: #notifications-configuration-windows}
{: windows}

创建新的 Visual Studio 项目或使用现有项目。  
{: windows}

如果该项目中还没有 {{ site.data.keyword.mobilefirst_notm }} 本机 Windows SDK，请遵循[将 {{ site.data.keyword.mobilefirst_notm }} SDK 添加到 Windows 应用程序](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/)教程中的指示信息。
{: windows}

#### 添加推送 SDK
{: #adding-the-push-sdk-windows}
{: windows}

1. 选择“工具 → NuGet 程序包管理器 → 程序包管理器控制台”。
2. 选择要安装 {{ site.data.keyword.mobilefirst_notm }} 推送组件的项目。
3. 运行 **Install-Package IBM.MobileFirstPlatformFoundationPush** 命令来添加 {{ site.data.keyword.mobilefirst_notm }} 推送 SDK。
{: windows}

#### 必备的 WNS 配置
{: pre-requisite-wns-configuration }
{: windows}

1. 确保应用程序具备 Toast 通知功能。可在 Package.appxmanifest 中启用此功能。
2. 确保使用向 WNS 注册的值来更新 `Package Identity Name` 和 `Publisher`。
3. （可选）删除 TemporaryKey.pfx 文件。
{: windows}

#### 通知 API
{: #notifications-api-windows}
{: windows}

##### MFPPush 实例
{: #mfppush-instance-windows}
{: windows}

必须在一个 `MFPPush` 实例上发出所有 API 调用。为此，可以创建一个变量（例如，`private MFPPush PushClient = MFPPush.GetInstance();`），然后在这整个类中调用 `PushClient.methodName()`。
{: windows}

或者，也可以针对您需要在其中访问推送 API 方法的每个实例都调用 `MFPPush.GetInstance().methodName()`。
{: windows}

##### 质询处理程序
{: #challenge-handlers-windows}
{: windows}

如果 `push.mobileclient` 作用域映射到**安全性检查**，那么需要确保在使用任何推送 API 之前，存在已注册的匹配**质询处理程序**。
{: windows}

在[凭证验证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/) 教程中了解有关质询处理程序的更多信息。
{: note}
{: windows}

#### 客户机端
{: #client-side-windows}
{: windows}

|C Sharp 方法                                                                                                | 描述|
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            |针对提供的上下文，初始化 MFPPush。                               |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    |设备是否支持推送通知。 |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  |向推送通知服务注册设备。               |
| [`GetTags()`](#get-tags-windows)                                |在推送通知服务实例中检索可用的标记。 |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     |使设备预订指定的标记。                          |
| [`GetSubscriptions()`](#get-subscriptions-windows)              |检索设备当前预订的所有标记。               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) |取消对特定标记的预订。 |
| [`UnregisterDevice()`](#unregister-windows)                     | 从推送通知服务注销设备。 |
{: caption="表 4. C Sharp 方法" caption-side="top"}
{: windows}

##### 初始化
{: #initialization-windows}
{: windows}

客户机应用程序连接到 MFPPush 服务时，需要执行初始化。
{: windows}

* 应先调用 `Initialize` 方法，然后再使用任何其他 MFPPush API。
* 它会注册回调函数以处理已收到的推送通知。
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### 是否支持推送
{: #is-push-supported-windows}
{: windows}

检查设备是否支持推送通知。
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

##### 注册设备并发送设备令牌
{: #register-device--send-device-token-windows}
{: windows}

向推送通知服务注册设备。
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

##### 获取标记
{: #get-tags-windows}
{: windows}

从推送通知服务检索所有可用标记。
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

##### 预订
{: #subscribe-windows}
{: windows}

预订所需的标记。
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

##### 获取预订
{: #get-subscriptions-windows}
{: windows}

检索设备当前预订的标记。
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

##### 取消预订
{: #unsubscribe-windows}
{: windows}

取消对标记的预订。
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

##### 注销
{: #unregister-windows}
{: windows}

从推送通知服务实例注销设备。
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

#### 处理推送通知
{: #handling-a-push-notification-windows}
{: windows}

要处理推送通知，需要设置 `MFPPushNotificationListener`。可实现以下方法来执行此操作。
{: windows}

1. 使用 MFPPushNotificationListener 类型的接口创建类

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
2. 通过调用 `MFPPush.GetInstance().listen(new NotificationListner())` 将该类设置为侦听器
3. 在 onReceive 方法中，您将收到推送通知，并可以处理关于期望行为的通知。
{: windows}

#### Windows Universal 推送通知服务
{: #windows-universal-push-notifications-service }
{: windows}

无需在服务器配置中打开特定端口。
{: windows}

WNS 使用常规的 http 或 https 请求。
{: windows}
