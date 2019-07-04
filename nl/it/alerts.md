---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, set up alerts, alert definitions

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configura gli avvisi
{: #set_up_alerts}

Mobile Analytics fornisce la funzione Avvisi, che ti consente di definire gli avvisi per monitorare le tue applicazioni. Puoi configurare delle soglie, che se superate attivano degli avvisi per avvisare il monitoraggio della console Mobile Analytics. Gli avvisi attivati possono essere visualizzati sulla console oppure possono essere configurati per essere ritrasmessi a un webhook personalizzato per una gestione appropriata.

Questa funzione fornisce i mezzi per rilevare e segnalare le violazioni delle soglie rilevate tramite i log di errore dell'applicazione, gli arresti anomali dell'applicazione e le transazioni di rete. Delle soglie e degli avvisi reattivi ti forniscono agilità nelle risposte alle condizioni delle prestazioni dell'applicazione senza dover monitorare continuamente le viste / grafici corrispondenti.

Questa funzione è in fase BETA.
{: note}

Le seguenti sezioni descrivono la creazione, la gestione degli avvisi e la loro visualizzazione sulla console Mobile Analytics.

## Creazione di una definizione di avviso per i log dell'applicazione (Log applicazione)
{: #creating_alert_def}

Puoi creare una definizione di avviso basata sui log applicazione.  Ad esempio, se vuoi monitorare i tuoi log dell'applicazione ogni 5 minuti per controllare se la tua applicazione di una versione specifica ha registrato degli errori più di tre volte, questo è il modo con cui puoi configurare questa impostazione.

1.  Nella console Mobile Analytics, fai clic su **Definizioni** per passare alla pagina delle definizioni di avviso.
2.  Fai clic su **Crea avviso**.
3.  Fornisci i seguenti valori:
    * **Nome avviso**: *Avviso per i log applicazione*
    * **Messaggio**: *Avviso di messaggio di errore*
    * **Frequenza query**: *5 minuti*
    * **Tipo di evento**: *Log applicazione*
        * **Proprietà**: *Livello di log*
            * **Valore**: *Errore*
            * **Soglia**: *Totale per l'istanza dell'applicazione*<br/>
              Se scegli l'opzione *Media per applicazione*, per i log applicazione viene calcolata la media in base al numero di dispositivi. Ad esempio, se hai due dispositivi e uno di essi invia sei log applicazione mentre l'altro ne invia tre, la media è di 4,5 log applicazione.
              {: note}
            * **Operatore**: *è maggiore di o uguale a*
            * Valore soglia: *3*
4.  Fai clic su **Avanti** e fornisci il seguente valore:
    * **Metodo**: *Solo console Analytics*<br/>
      Inoltre, scegli l'opzione *Console Analytics e POST di rete*, se desideri inviare un messaggio POST con un payload JSON al tuo URL personalizzato. I seguenti campi sono disponibili se scegli questa opzione.
      * **URL POST di rete**
      * **Intestazioni**
      * **Tipo di autenticazione**
      * **Corpo richiesta POST**
5. Fai clic su **Salva**.  

Hai appena creato una definizione di avviso per attivare un avviso al termine di ogni intervallo di 5 minuti quando il numero di log applicazione supera la soglia di 3 o più log di errori.

Questo avviso rimane attivo, monitorato dalla frequenza di configurazione finché la definizione di avviso non viene disabilitata o eliminata.
{: note}

## Creazione di una definizione di avviso per gli arresti anomali delle applicazioni
{: #creating_alert_crashes}

Quello che segue è un esempio di configurazione degli avvisi sugli arresti anomali dell'applicazione.  Questo avviso esegue il monitoraggio ogni 2 minuti, tutti gli arresti anomali dell'applicazione attivano un avviso se il numero di arresti anomali riscontrati raggiunge i 5.

1.  Nella console Mobile Analytics, fai clic su **Definizioni** per passare alla pagina delle definizioni di avviso.
2.  Fai clic su **Crea avviso**.
3.  Fornisci i seguenti valori:
    * **Nome avviso**: *Avviso per arresti anomali delle applicazioni*
    * **Messaggio**: *Avviso di arresto anomalo dell'applicazione*
    * **Frequenza query**: *2 minuti*
    * **Tipo di evento**: *Arresti anomali delle applicazioni*
        * **Nome applicazione**: *Qualsiasi applicazione*
        * **Qualsiasi applicazione**: *Qualsiasi versione*
    * **Soglia**: *Conteggio arresti anomali*
    * **Operatore**: *è maggiore di o uguale a*
    * Valore soglia: *5*
4.  Fai clic su **Metodo di distribuzione** o su **Avanti** e fornisci il seguente valore:
    * **Metodo**: *Solo console Analytics*<br/>
      Inoltre, scegli l'opzione *Console Analytics e POST di rete*, se desideri inviare un messaggio POST con un payload JSON al tuo URL personalizzato. I seguenti campi sono disponibili se scegli questa opzione.
      * **URL POST di rete**
      * **Intestazioni**
      * **Tipo di autenticazione**
      * **Corpo richiesta POST**
5. Fai clic su **Salva**.  

Questo avviso rimane attivo, monitorato dalla frequenza di configurazione finché la definizione di avviso non viene disabilitata o eliminata.
{: note}

## Gestione delle definizioni di avviso
{: #managing_alert_def}

Gestisci le tue definizioni di avviso dalla pagina **Definizioni** dell'avviso.

Per ogni definizione di avviso puoi effettuare le seguenti operazioni:
* Attiva/disattiva la casella di spunta sotto la colonna **Abilitato** per abilitare o disabilitare una specifica definizione di avviso.
* Se vuoi creare una copia di una definizione di avviso e modificare alcuni valore, fai clic sull'icona duplicato.
* Fai clic sull'icona matita, se vuoi modificare una definizione di avviso.
* Fai clic sull'icona cestino, se vuoi eliminare una definizione di avviso.

## Visualizzazione degli avvisi
{: #viewing_alerts}

Puoi visualizzare gli avvisi, che sono stati attivati dalla pagina **Log** dell'avviso.

1.  Nella console Mobile Analytics, fai clic su **Log**.
2.  Fai clic sull'icona **+** per uno qualsiasi degli avvisi. Questa azione visualizza la definizione di avviso e la sezione **Istanze avviso**.
    Se la definizione di avviso corrispondente non era stata eliminata o modificata, puoi modificare la definizione di avviso facendo clic su **Modifica avviso**. Altrimenti, il pulsante **Modifica avviso** non è disponibile e viene visualizzato il seguente messaggio:
    `This alert definition has since been modified or deleted.`
3.  Puoi facoltativamente selezionare un avviso e fare clic sull'icona cestino per eliminarlo.
