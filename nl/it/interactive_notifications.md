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

# Notifiche interattive
{: #interactive_notifications}

Con la notifica interattiva, quando arriva una notifica, gli utenti possono agire sulla notifica senza aprire l'applicazione. Quando arriva una notifica interattiva, il dispositivo mostra i pulsanti di azione insieme al messaggio di notifica.
{: shortdesc}

Le notifiche interattive sono supportate sui dispositivi con iOS versione 8 e successive. Se una notifica interattiva viene inviata su un dispositivo iOS con una versione precedente alla 8, le azioni di notifica non verranno visualizzate.

## Invio di una notifica di push interattiva
{: #sending-interactive-push-notification }

Prepara la notifica e inviala. Per ulteriori informazioni, vedi [Invio delle notifiche di push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

Puoi impostare una stringa per indicare la categoria della notifica con l'oggetto di notifica, in **{{ site.data.keyword.mfp_oc_short_notm }} → [tua applicazione] → Push → Send Notifications → iOS custom settings**. In base al valore della categoria, vengono visualizzati i pulsanti di azione della notifica. Ad esempio,

![Impostazione delle categorie per le notifiche interattive iOS in {{ site.data.keyword.mfp_oc_short_notm }}](images/categories-for-interactive-notifications.png)

## Gestione delle notifiche di push interattive nelle applicazioni Cordova
{: #handling-interactive-push-notifications-in-cordova-applications }

Per ricevere le notifiche interattive, segui questi passi:

1. Nel JavaScript principale, definisci le categorie registrate per la notifica interattiva e passalo alla chiamata di registrazione del dispositivo `MFPPush.registerDevice`.

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

2. Passa l'oggetto `options` mentre registri il dispositivo per le notifiche di push.

   ```javascript
   MFPPush.registerDevice(options, function(successResponse) {
  		navigator.notification.alert("Successfully registered");
  		enableButtons();
   });  
   ```

## Gestione delle notifiche di push interattive nelle applicazioni iOS native
{: #handling-interactive-push-notifications-in-native-ios-applications }

Segui questi passi per ricevere le notifiche interattive:

1. Abilita la funzionalità dell'applicazione ad eseguire attività in background per la ricezione di notifiche remote. Questo passo è obbligatorio se alcune delle azioni sono abilitate in background.
2. Definisci le categorie registrate per le notifiche interattive e passale come opzioni a `MFPPush.registerDevice`.

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
