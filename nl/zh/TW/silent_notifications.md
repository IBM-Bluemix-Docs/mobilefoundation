---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notification, sending silent notifications

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

# 無聲自動通知
{: #silent_notifications}

無聲自動通知是指不會顯示警示或打擾使用者的通知。當無聲自動通知到達時，應用程式處理程式碼會在背景執行，而不會將應用程式帶到前景。目前，在具有第 7 版以上版本的 iOS 裝置上支援無聲自動通知。如果無聲自動通知傳送至版本低於 7 的 iOS 裝置，則應用程式在背景中執行時會忽略通知。如果應用程式在前景中執行，則會呼叫通知回呼方法。
{: shortdesc}

## 傳送無聲自動推送通知
{: #sending-silent-push-notifications }

準備通知並傳送通知。如需相關資訊，請參閱[傳送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)。

iOS 所支援的三種通知類型由常數 `DEFAULT`、`SILENT` 及 `MIXED` 來代表。未明確指定類型時，會假設 `DEFAULT` 類型。

對於 `MIXED` 類型通知，裝置上會顯示一則訊息，同時在背景中，應用程式會啟動並處理無聲自動通知。`MIXED` 類型通知的回呼方法會呼叫兩次 - 一次是在無聲自動通知送達裝置時，另一次則是點選通知來開啟應用程式時。

根據需求，在 **{{ site.data.keyword.mfp_oc_short_notm }} → [您的應用程式] → 推送 → 傳送通知 → iOS 自訂設定**下選擇適當的類型。

如果通知是無聲自動，則會忽略 **alert**、**sound** 和 **badge** 內容。
{: note}

![在 {{ site.data.keyword.mfp_oc_short_notm }} 中設定 iOS 無聲自動通知的通知類型](images/notification-type-for-silent-notifications.png)

## 在 Cordova 應用程式中處理無聲自動推送通知
{: #handling-silent-push-notifications-in-cordova-applications }

在 JavaScript 推送通知回呼方法中，您必須執行下列步驟：

1. 檢查通知類型。例如，

   ```javascript
   if(props['content-available'] == 1) {
        //Silent Notification or Mixed Notification. Perform non-GUI tasks here.
   } else {
        //Normal notification
   }
   ```
   {: codeblock}

2. 如果通知是無聲自動或混合，在您完成背景工作之後，請呼叫 `WL.Client.Push.backgroundJobDone` API。

## 在原生 iOS 應用程式中處理無聲自動推送通知
{: #handling-silent-push-notifications-in-native-ios-applications }

您必須遵循下列步驟來接收無聲自動通知：

1. 啟用應用程式功能，以執行背景作業來接收遠端通知。
2. 檢查 `content-available` 索引鍵是否設為 **1**，來檢查通知是否為無聲自動通知。
3. 完成通知處理之後，您必須立即在處理程式參數中呼叫區塊，否則您的應用程式將會終止。您的應用程式有最多 30 秒的時間可處理通知，並呼叫指定的完成處理程式區塊。
