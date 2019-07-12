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

# Benachrichtigungen im Hintergrund
{: #silent_notifications}

Benachrichtigungen im Hintergrund erfolgen ohne Anzeige von Alerts oder andere Störungen des Benutzers. Wenn eine Benachrichtigung im Hintergrund eingeht, wird der Handling-Code der Anwendung im Hintergrund ausgeführt, ohne die Anwendung in den Vordergrund zu bringen. Zurzeit werden Benachrichtigungen im Hintergrund auf iOS-Geräten der Version 7 oder einer aktuelleren Version unterstützt. Wenn die Benachrichtigung im Hintergrund an iOS-Geräte mit einer älteren Version als Version 7 gesendet wird und die Anwendung im Hintergrund ausgeführt wird, wird die Benachrichtigung ignoriert. Wenn die Anwendung im Vordergrund ausgeführt wird, wird die Callback-Methode für Benachrichtigungen aufgerufen.
{: shortdesc}

## Push-Benachrichtigungen im Hintergrund senden
{: #sending-silent-push-notifications }

Bereiten Sie die Benachrichtigung vor und senden Sie sie. Weitere Informationen hierzu finden Sie im Abschnitt [Push-Benachrichtigungen senden](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

Die drei Typen von Benachrichtigungen, die für iOS unterstützt werden, werden durch die Konstanten `DEFAULT`, `SILENT` und `MIXED` dargestellt. Wenn der Typ nicht explizit angegeben wird, wird der Typ `DEFAULT` angenommen.

Bei Benachrichtigungen vom Typ `MIXED` wird auf dem Gerät eine Nachricht angezeigt, während die App im Hintergrund aktiviert wird und eine Benachrichtigung im Hintergrund verarbeitet. Die Callback-Methode für `MIXED` wird zweimal aufgerufen. Der erste Aufruf erfolgt, wenn die Benachrichtigung im Hintergrund das Gerät erreicht. Der zweite Aufruf erfolgt, wenn der Benutzer auf die Benachrichtigung tippt und so die Anwendung öffnet.

Wählen Sie ausgehend von der Anforderung unter **{{ site.data.keyword.mfp_oc_short_notm }} → [Ihre Anwendung] → Push → Benachrichtigungen senden → Angepasste iOS-Einstellungen** den entsprechenden Typ aus.

Wenn die Benachrichtigung im Hintergrund erfolgt, werden die Eigenschaften für **Alert**, **Sound** und **Badge** ignoriert.
{: note}

![Benachrichtigungstyp für Benachrichtigungen im Hintergrund unter iOS in der {{ site.data.keyword.mfp_oc_short_notm }}](images/notification-type-for-silent-notifications.png)

## Benachrichtigungen im Hintergrund in Cordova-Anwendungen
{: #handling-silent-push-notifications-in-cordova-applications }

In der Callback-Methode für JavaScript-Push-Benachrichtigungen müssen Sie die folgenden Schritte ausführen:

1. Überprüfen Sie den Benachrichtigungstyp. Beispiel:

   ```javascript
   if(props['content-available'] == 1) {
        //Benachrichtigung im Hintergrund oder gemischte Benachrichtigung. Nicht-GUI-Tasks hier ausführen.
   } else {
        //Normale Benachrichtigung
   }
   ```
   {: codeblock}

2. Rufen Sie für Benachrichtigungen im Hintergrund oder gemischte Benachrichtigungen nach Abschluss des Hintergrundjobs die API `WL.Client.Push.backgroundJobDone` auf.

## Benachrichtigungen im Hintergrund in nativen iOS-Anwendungen
{: #handling-silent-push-notifications-in-native-ios-applications }

Für den Empfang von Benachrichtigungen im Hintergrund müssen Sie die folgenden Schritte ausführen:

1. Aktivieren Sie die Anwendungsfunktion für die Ausführung von Hintergrundtasks beim Empfang der fernen Benachrichtigungen.
2. Überprüfen Sie, ob die Benachrichtigung im Hintergrund erfolgt. Prüfen Sie dazu, ob der Schlüssel `content-available` auf **1** gesetzt ist.
3. Nach der Verarbeitung der Benachrichtigung müssen Sie sofort den Block im handler-Parameter aufrufen, weil Ihre App andernfalls beendet wird. Die App hat maximal 30 Sekunden Zeit, die Benachrichtigung zu verarbeiten und den angegebenen Completion-Handler-Block aufzurufen.
