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

# Interaktive Benachrichtigungen
{: #interactive_notifications}

Wenn eine interaktive Benachrichtigung eingeht, können Benutzer entsprechende Aktionen ausführen, ohne die Anwendung zu öffnen. Beim Eintreffen einer interaktiven Benachrichtigung zeigt das Gerät die Benachrichtigung und Aktionsschaltflächen an.
{: shortdesc}

Interaktive Benachrichtigungen werden auf Geräten mit iOS-Version 8 und höher unterstützt. Wenn eine interaktive Benachrichtigung an ein iOS-Gerät mit einer älteren Version als 8 gesendet wird, werden die Benachrichtigungsaktionen nicht angezeigt.

## Interaktive Push-Benachrichtigungen senden
{: #sending-interactive-push-notification }

Bereiten Sie die Benachrichtigung vor und senden Sie sie. Weitere Informationen hierzu finden Sie im Abschnitt [Push-Benachrichtigungen senden](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

Unter **{{ site.data.keyword.mfp_oc_short_notm }} → [Ihre Anwendung] → Push → Benachrichtigungen senden → Angepasste iOS-Einstellungen** können Sie eine Zeichenfolge festlegen, um die Kategorie der Benachrichtigung mit dem Benachrichtigungsobjekt anzugeben. Die Aktionsschaltflächen für die Benachrichtigung werden ausgehend vom Kategoriewert angezeigt. Beispiel:

![Kategorien für interaktive Benachrichtigungen unter iOS in der {{ site.data.keyword.mfp_oc_short_notm }}](images/categories-for-interactive-notifications.png)

## Interaktive Benachrichtigungen in Cordova-Anwendungen
{: #handling-interactive-push-notifications-in-cordova-applications }

Gehen Sie wie folgt vor, um interaktive Benachrichtigungen zu empfangen:

1. Definieren Sie im Haupt-JavaScript die registrierten Kategorien für interaktive Benachrichtigungen und übergeben Sie sie an den Aufruf `MFPPush.registerDevice` für die Geräteregistrierung.

   ```javascript
   var options = {
        ios: {
            alert: true,
            badge: true,
            sound: true,     
            categories: [{
                //Beim Senden der Benachrichtigung verwendete Kategorie-ID.
                id : "poll",

                //Optionales Array mit Aktionen, um zusammen mit der Nachricht die Aktionsschaltflächen anzuzeigen.    
                actions: [{
                    //Aktions-ID
                    id: "poll_ok",

                    //Aktionstitel, der als Teil der Benachrichtigungsschaltfläche angezeigt wird.
                    title: "OK",

                    //Optionaler Modus für die Ausführung der Aktion im Vorder- oder Hintergrund (1 für Vordergrund, 0 für Hintergrund). Standard ist Vordergrund.
                    mode: 1,  

                    //Optionale Eigenschaft für die Darstellung der Aktionsschaltfläche in rot. Standardwert ist 'false'.
                    destructive: false,

                    //Optionale Eigenschaft, um festzulegen, ob vor Ausführung der Aktion eine Authentifizierung erforderlich ist (Bildschirmsperre).
                    //Beim Vordergrundmodus hat diese Eigenschaft immer den Wert 'true'.
                    authenticationRequired: true
                },
                {
                    id: "poll_nok",
                    title: "NOK",
                    mode: 1,
                    destructive: false,
                    authenticationRequired: true
                }],

                //Optionale Liste mit Aktionen, die im Falle eines Alerts angezeigt werden muss.
                //Fehlt die Liste, werden die ersten vier Aktionen angezeigt.
                defaultContextActions: ['poll_ok','poll_nok'],

                //Optionale Liste mit Aktionen, die im Lockscreen mit der Benachrichtigungszentrale angezeigt werden muss.
                //Fehlt die Liste, werden die ersten beiden Aktionen angezeigt.
                minimalContextActions: ['poll_ok','poll_nok']
            }]     
        }
   }
   ```

2. Übergeben Sie das Objekt `options` beim Registrieren des Geräts für Push-Benachrichtigungen.

   ```javascript
   MFPPush.registerDevice(options, function(successResponse) {
  		navigator.notification.alert("Successfully registered");
  		enableButtons();
   });  
   ```

## Interaktive Benachrichtigungen in nativen iOS-Anwendungen
{: #handling-interactive-push-notifications-in-native-ios-applications }

Führen Sie für den Empfang interaktiver Benachrichtigungen die folgenden Schritte aus:

1. Aktivieren Sie die Anwendungsfunktion für die Ausführung von Hintergrundtasks beim Empfang der fernen Benachrichtigungen. Dieser Schritt ist erforderlich, wenn einige der Aktionen im Hintergrund aktiviert sind.
2. Definieren Sie registrierte Kategorien für interaktive Benachrichtigungen und übergeben Sie sie als Optionen an `MFPPush.registerDevice`.

   ```swift
   //Kategorien für interaktive Push-Operationen definieren
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

   // Gerät registrieren
    MFPPush.sharedInstance().registerDevice(options as [NSObject : AnyObject], completionHandler: {(response: WLResponse!, error: NSError!) -> Void in
   ```
