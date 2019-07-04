---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, sending interactive notification

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

# 互動式通知
{: #interactive_notifications}

使用互動式通知，當通知到達時，使用者可以對通知採取動作而不必開啟應用程式。當互動式通知到達時，裝置會顯示動作按鈕以及通知訊息。
{: shortdesc}

在具有 iOS 第 8 版及更高版本的裝置上支援互動式通知。如果互動式通知傳送至版本低於 8 的 iOS 裝置，則不會顯示通知動作。

## 傳送互動式推送通知
{: #sending-interactive-push-notification }

準備通知並傳送通知。如需相關資訊，請參閱[傳送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)。

您可以在 **{{ site.data.keyword.mfp_oc_short_notm }} → [您的應用程式] → 推送 → 傳送通知 → iOS 自訂設定**下，設定字串以便用通知物件指出通知的種類。根據種類值，會顯示通知動作按鈕。例如，

![在 {{ site.data.keyword.mfp_oc_short_notm }} 設定 iOS 互動式通知的種類](images/categories-for-interactive-notifications.png)

## 在 Cordova 應用程式中處理互動式推送通知
{: #handling-interactive-push-notifications-in-cordova-applications }

若要收到互動式通知，請遵循下列步驟：

1. 在主要 JavaScript 中，定義互動式通知的已登錄種類，並將它傳遞給裝置登錄呼叫 `MFPPush.registerDevice`。

   ```javascript
   var options = {
        ios: {
            alert: true,
            badge: true,
            sound: true,     
            categories: [{
                //Category identifier, this is used while sending the notification.
                id : "poll",

                //Optional array of actions to show the action buttons along with the message.    
                actions: [{
                    //Action identifier
                    id: "poll_ok",

                    //Action title to be displayed as part of the notification button.
                    title: "OK",

                    //Optional mode to run the action in foreground or background. 1-foreground. 0-background. Default is foreground.
                    mode: 1,  

                    //Optional property to mark the action button in red color. Default is false.
                    destructive: false,

                    //Optional property to set if authentication is required or not before running the action.(Screen lock).
                    //For foreground, this property is always true.
                    authenticationRequired: true
                },
                {
                    id: "poll_nok",
                    title: "NOK",
                    mode: 1,
                    destructive: false,
                    authenticationRequired: true
                }],

                //Optional list of actions that is needed to show in the case alert.
                //If it is not specified, then the first four actions will be shown.
                defaultContextActions: ['poll_ok','poll_nok'],

                //Optional list of actions that is needed to show in the notification center, lock screen.
                //If it is not specified, then the first two actions will be shown.
                minimalContextActions: ['poll_ok','poll_nok']
            }]     
        }
   }
   ```

2. 針對推送通知登錄裝置時，請傳遞 `options` 物件。

   ```javascript
   MFPPush.registerDevice(options, function(successResponse) {
  		navigator.notification.alert("Successfully registered");
  		enableButtons();
   });  
   ```

## 在原生 iOS 應用程式中處理互動式推送通知
{: #handling-interactive-push-notifications-in-native-ios-applications }

請遵循下列步驟，接收互動式通知：

1. 啟用應用程式功能，以執行背景作業來接收遠端通知。如果部分動作是在背景啟用，則需要執行此步驟。
2. 定義互動式通知的已登錄種類，並將它們傳遞為 `MFPPush.registerDevice` 的選項。

   ```swift
   //define categories for Interactive Push
   let acceptAction = UIMutableUserNotificationAction()
   acceptAction.identifier = "OK"
   acceptAction.title = "OK"
   acceptAction.activationMode = .Foreground

   let rejetAction = UIMutableUserNotificationAction()
   rejetAction.identifier = "Cancel"
   rejetAction.title = "Cancel"
   rejetAction.activationMode = .Foreground

   let category = UIMutableUserNotificationCategory()
   category.identifier = "poll"
   category.setActions([acceptAction, rejetAction], forContext: .Default)

   let categories:Set<UIUserNotificationCategory> = [category]

   let options = ["alert":true, "badge":true, "sound":true, "categories": categories]

   // Register device
    MFPPush.sharedInstance().registerDevice(options as [NSObject : AnyObject], completionHandler: {(response: WLResponse!, error: NSError!) -> Void in
   ```
