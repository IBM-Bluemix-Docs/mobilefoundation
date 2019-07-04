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

# Notifiche silenziose
{: #silent_notifications}

Le notifiche silenziose sono notifiche che non visualizzano avvisi altrimenti disturberebbero l'utente. Quando arriva una notifica silenziosa, il codice che gestisce l'applicazione viene eseguito in background senza portare l'applicazione in primo piano. Al momento, le notifiche silenziose sono supportate sui dispositivi iOS con versione 7 e successive. Se la notifica silenziosa viene inviata ai dispositivi iOS con una versione precedente alla 7, la notifica viene ignorata se l'applicazione è in esecuzione in background. Se l'applicazione è in esecuzione in primo piano, viene richiamato il metodo di callback della notifica.
{: shortdesc}

## Invio di notifiche di push silenziose
{: #sending-silent-push-notifications }

Prepara la notifica e inviala. Per ulteriori informazioni, vedi [Invio delle notifiche di push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

I tre tipi di notifiche supportati per iOS sono rappresentati dalle costanti `DEFAULT`, `SILENT` e `MIXED`. Quando il tipo non viene specificato esplicitamente, si presuppone il tipo `DEFAULT`.

Per le notifiche di tipo `MIXED`, viene visualizzato un messaggio sul dispositivo mentre, in background, l'applicazione si attiva ed elabora la notifica silenziosa. Il metodo di callback per le notifiche di tipo `MIXED` viene richiamato due volte - una quando la notifica silenziosa raggiunge il dispositivo e un'altra quando l'applicazione viene aperta toccando la notifica.

In base al requisito, scegli il tipo appropriato in **{{ site.data.keyword.mfp_oc_short_notm }} → [tua applicazione] → Push → Send Notifications → iOS custom settings**.

Se la notifica è silenziosa, le proprietà **alert**, **sound** e **badge** vengono ignorate.
{: note}

![Impostazione del tipo di notifica per le notifiche silenziose iOS in {{ site.data.keyword.mfp_oc_short_notm }}](images/notification-type-for-silent-notifications.png)

## Gestione delle notifiche di push silenziose nelle applicazioni Cordova
{: #handling-silent-push-notifications-in-cordova-applications }

Nel metodo di callback della notifica di push JavaScript, devi eseguire questi passi:

1. Controlla il tipo di notifica. Ad esempio,

   ```javascript
   if(props['content-available'] == 1) {
        //Silent Notification or Mixed Notification. Perform non-GUI tasks here.
   } else {
        //Normal notification
   }
   ```
   {: codeblock}

2. Se la notifica è silenziosa o mista, una volta completato il lavoro in background, richiama l'API `WL.Client.Push.backgroundJobDone`.

## Gestione delle notifiche di push silenziose nelle applicazioni iOS native
{: #handling-silent-push-notifications-in-native-ios-applications }

Devi seguire questi passi per ricevere le notifiche silenziose:

1. Abilita la funzionalità dell'applicazione ad eseguire attività in background per la ricezione di notifiche remote.
2. Controlla se la notifica è silenziosa oppure no verificando che la chiave `content-available` sia impostata su **1**.
3. Una volta completata l'elaborazione della notifica, devi richiamare immediatamente il blocco nel parametro del gestore altrimenti la tua applicazione verrà terminata. La tua applicazione ha fino a 30 secondi per elaborare la notifica e richiamare il blocco del gestore di completamento specificato.
