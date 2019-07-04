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

# 静默通知
{: #silent_notifications}

静默通知是既不显示警报也不以其他方式打扰用户的通知。当静默通知到达时，应用程序处理代码将在后台运行，而不是将应用程序转到前台。目前，在装有 iOS V7 及更高版本的设备上支持静默通知。如果将静默通知发送到装有 iOS V7 之前版本的设备，那么当应用程序在后台运行时，将忽略该通知。如果应用程序在前台运行，那么将调用通知回调方法。
{: shortdesc}

## 发送静默推送通知
{: #sending-silent-push-notifications }

准备通知并发送通知。有关更多信息，请参阅[发送推送通知](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)。

iOS 支持的三种通知类型分别由常量 `DEFAULT`、`SILENT` 和 `MIXED` 表示。如果未明确指定类型，那么将使用 `DEFAULT` 类型。

对于 `MIXED` 类型的通知，在设备上显示消息的同时，在后台会唤醒应用程序并让其处理静默通知。`MIXED` 类型通知的回调方法会调用两次：一次是在静默通知到达设备时，另一次是在通过点击通知打开应用程序时。

根据具体的需求，在 **{{ site.data.keyword.mfp_oc_short_notm }} → [您的应用程序] → 推送 → 发送通知 → iOS 定制设置**下选择相应的类型。

如果通知为静默，那么将忽略**警报**、**声音**和**角标**属性。
{: note}

![在 {{ site.data.keyword.mfp_oc_short_notm }} 中设置 iOS 静默通知的通知类型](images/notification-type-for-silent-notifications.png)

## 在 Cordova 应用程序中处理静默推送通知
{: #handling-silent-push-notifications-in-cordova-applications }

在 JavaScript 推送通知回调方法中，必须执行以下步骤：

1. 检查通知类型。例如，

   ```javascript
   if(props['content-available'] == 1) {
        //Silent Notification or Mixed Notification. Perform non-GUI tasks here.
   } else {
        //Normal notification
   }
   ```
   {: codeblock}

2. 如果通知为静默或混合类型，那么在完成后台作业后调用 `WL.Client.Push.backgroundJobDone` API。

## 在本机 iOS 应用程序中处理静默推送通知
{: #handling-silent-push-notifications-in-native-ios-applications }

必须执行以下步骤以接收静默通知：

1. 启用在接收远程通知时执行后台任务的应用程序功能。
2. 通过检查 `content-available` 键是否设置为 **1** 来检查通知是否为静默类型。
3. 在处理完通知后，必须在 handler 参数中立即调用该块，否则将终止您的应用程序。您的应用程序最多有 30 秒时间来处理通知并调用指定的完成处理程序块。
