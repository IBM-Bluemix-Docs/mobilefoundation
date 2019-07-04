---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, charts, visualize data, analytics console

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualizza le informazioni approfondite sulla console
{: #visualize_insights_on_console}

Per visualizzare le informazioni approfondite dai dati di analisi acquisiti e inviati dalla tua applicazione, devi avviare la console Mobile Analytics facendo clic sull'opzione **Analytics Console** dal menu di navigazione della console Mobile Foundation Operations. 

La console Mobile Analytics può essere eseguita in due modalità:
  - **Demo Mode ON**, che è pensata semplicemente per scopi dimostrativi e mostra le diverse viste di analisi (grafici e tabelle) utilizzando feed di dati simulati.
  - **Demo Mode OFF**, che mostra le varie viste di analisi basate sui feed di dati in tempo reale provenienti dalle tue applicazioni [strumentate per Mobile Analytics](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

Tutte le viste di analisi possono essere eliminate applicando i filtri per *nome applicazione*, *versione*, *SO del dispositivo* e *periodo di tempo*, consentendoti pertanto di ottenere informazioni approfondite da diverse prospettive.

Per visualizzare le informazioni approfondite per la tua applicazione, assicurati di quanto segue.
  - La tua applicazione è strumentata per acquisire ed inviare i dati di analisi rilevanti al servizio Mobile Analytics.
  - Hai DISATTIVATO la modalità demo nella console Analytics
  - Applica i filtri corretti. Ad esempio, assicurati di selezionare un periodo di tempo quando la tua applicazione viene distribuita nel campo e che sia attiva con gli utenti.

La console Mobile Analytics fornisce diversi tipi di analisi delle prestazioni e dell'utilizzo della tua applicazione mobile categorizzati nel pannello di navigazione della console Analytics.  Le seguenti sezioni illustrano le diverse viste di analisi:


## Users
{: #reports_visualized_users}
Questa vista ti aiuta ad ottenere le informazioni approfondite in *Modelli di onboarding degli utenti* come ad esempio il numero di utenti attivi che hanno utilizzato l'applicazione in un intervallo di date specificato e un confronto del numero di nuovi utenti rispetto a quelli esistenti che hanno riutilizzato la tua applicazione.
I grafici in questa vista possono essere filtrati in base a *nome applicazione*, *sistema operativo* o *versione sistema operativo*.

## Sessions
{: #reports_visualized_sessions}
Questa vista ti aiuta ad ottenere le informazioni approfondite in *Modelli di utilizzo* della tua applicazione in termini di sessioni dell'applicazione (*App Sessions*) per l'intervallo di date specificato. Viene registrata una sessione quando un'applicazione viene portata in primo piano in un dispositivo.  Otterrai delle informazioni approfondite su quali ore del giorno la tua applicazione è stata più o meno utilizzata e ciò può portare a informazioni aziendali utili. I grafici in questa vista possono essere filtrati in base a *nome applicazione*, *sistema operativo* o *versione sistema operativo*.

## Network Requests
{: #reports_visualized_network_requests}
Questa vista ti aiuta ad ottenere le informazioni approfondite sull'esperienza della tua applicazione perché effettua delle chiamate API ai sistemi di backend.  Questa vista ha delle tabelle e dei grafici che forniscono uno sguardo su quali sono le funzioni più usate dei tuoi sistemi di backend, qual è il loro tempo di risposta e com'è la loro stabilità e se devi considerare di ribilanciare i tuoi sistemi di supporto di backend.

Questa vista contiene dei grafici che tracciano per un intervallo di date selezionato, il tempo di andata e ritorno medio delle chiamate API in uscita della tua applicazione, il numero di richieste effettuate per chiamata API, il numero di richieste riuscite rispetto a quelle fallite raggruppate per codici di risposta.  I grafici in questa vista possono essere filtrati in base al nome dell'applicazione, al sistema operativo o alle versioni del sistema operativo.

## Crashes
{: #reports_visualized_crashes}
Questa vista ti aiuta con informazioni approfondite su quanto è stata stabile la tua applicazione per un periodo di tempo selezionato e ti aiuta a decidere se l'implementazione o la progettazione della tua applicazione deve essere corretta.  Fornisce dei grafici che mostrano le differenze sul numero di arresti anomali rispetto al numero totale di utilizzi e alla frequenza di arresto anomalo generale.  I grafici in questa vista possono essere filtrati in base a *nome applicazione*, *sistema operativo* o *versione sistema operativo*.


## Troubleshooting
{: #reports_visualized_troubleshooting}
Questa vista fornisce tutte le informazioni necessarie di cui potrebbe avere bisogno uno sviluppatore dell'applicazione per risolvere i problemi con un'applicazione.  Questa vista fornisce un'analisi più dettagliata degli arresti anomali della tua applicazione in termini di dispositivi interessati, SO host, ora specifica dell'arresto anomalo, traccia di stack al momento dell'arresto anomalo e inoltre i log che possono essere scaricati per un'analisi più dettagliata.  

I log dell'arresto anomalo vengono raccolti cercando i log dell'applicazione che sono registrati al livello FATAL.  L'SDK client Analytics per Android e iOS gestisce in modo nativo le eccezioni non rilevate e ne registra i dettagli come messaggi di log di livello FATAL.  Tuttavia, nel caso di Cordova, tutti gli arresti anomali al livello JavaScript devono essere gestiti dagli sviluppatori e i log di arresto anomalo inviati al servizio Mobile Analytics devono essere esaminati ed analizzati nella console Mobile Analytics.
{: note}


## User Feedback
{: #reports_visualized_userfeedback}
Questa vista fornisce le informazioni approfondite su qual è l'effettiva esperienza interattiva dei tuoi utenti quando usano l'applicazione e cosa ne pensano.

* **I proprietari dell'applicazione** possono ottenere una vista ricca di contesto e dettagliata dei bug e di altri feedback inviati da **utenti e tester** come registrato mentre esegui l'applicazione
* **Gli sviluppatori** possono ricevere dei contesti dell'applicazione accurati per diagnosticare e correggere i bug o le lacune delle funzioni.
* **I proprietari dell'applicazione** e **Gli sviluppatori** possono utilizzare questa vista anche per gestire le azioni sui feedback ricevuti come ad esempio la registrazione dei commenti o dei link ai problemi creati nei sistemi di traccia del bug.  Può inoltre essere configurato uno stato di revisione globale per ogni feedback per aiutarti nel riepilogo delle azioni effettuate sul feedback utente.

## Custom Charts
Questa vista estende Mobile Analytics a casi personalizzati in cui i **proprietari dell'applicazione** e gli **sviluppatori** vorranno creare le proprie analisi specifiche per l'applicazione.   Utilizzando questa struttura, puoi creare le tue viste di analisi (grafici, tabelle ecc.) su dati di analisi standard acquisiti dall'SDK client e anche sui dati personalizzati o specifici per l'applicazione che sono stati registrati.  Per ulteriori informazioni su questa struttura di analisi estesa, vedi [qui](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts).
