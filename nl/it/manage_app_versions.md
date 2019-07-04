---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestisci le versioni delle applicazioni
{: #manage_app_versions}

Le funzionalità di gestione delle applicazioni Mobile Foundation forniscono agli utenti e agli amministratori del server Mobile Foundation un controllo dettagliato sull'accesso di utenti e dispositivi alle loro applicazioni.

Il server Mobile Foundation tiene traccia di tutti i tentativi di accesso alla tua infrastruttura mobile e archivia le informazioni sull'applicazione, sull'utente e sul dispositivo su cui è installata l'applicazione. L'associazione tra l'applicazione, l'utente e il dispositivo forma la base per le funzionalità di gestione delle applicazioni mobili del server.

Utilizzando la Mobile Foundation Operations Console, puoi monitorare e gestire l'accesso alle tue risorse. Puoi anche gestire la tua versione dell'applicazione specifica.

1.  Vai alla Mobile Foundation Operations Console, fai clic su **Applicazioni**, scegli l'applicazione che vuoi gestire e seleziona la versione specifica dell'applicazione a cui sei interessato nell'elenco **Versioni** visualizzato.
    ![Gestisci la versione dell'applicazione](images/app_version_management.png)

2. Nella scheda **Gestione**, vedrai le opzioni per impostare lo stato dell'applicazione per la versione dell'applicazione selezionata. Gli stati supportati per una versione dell'applicazione sono:
   * Attivo
   * Attivo ed in fase di notifica
   * Accesso disabilitato
3. Una versione dell'applicazione può essere disabilitata selezionando l'opzione *Accesso disabilitato* da **Accesso all'applicazione > Stato**.
4. Puoi anche configurare il caricamento delle risorse web aggiornate per la tua applicazione Cordova nella sezione **Aggiornamento diretto**. Agli utenti che si connettono al server Mobile Foundation utilizzando questa specifica versione dell'applicazione viene presentata l'opzione di aggiornare la loro applicazione utilizzando l'aggiornamento diretto.
5. Puoi anche eseguire le seguenti azioni sulla versione dell'applicazione selezionata utilizzando le seguenti opzioni nel menu **Azioni**:
   *  Elimina versione
   *  Clona versione
   *  Esporta versione


Per ulteriori informazioni sulla gestione dei dispositivi, vedi [Gestione dei dispositivi](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices).
Per ulteriori informazioni sulla disabilitazione in remoto di una versione dell'applicazione, vedi [Disabilita in remoto una versione dell'applicazione](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version).
{: note}
