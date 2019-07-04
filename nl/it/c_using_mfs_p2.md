---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation, mobile analytics, professional plan, configure database

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Configurazione con il piano Professional 1 Application
{: #using_mobilefoundation_p2}

Con il piano Professional 1 Application, gli utenti possono creare 1 applicazione mobile con vari sistemi operativi mobili.
Dopo che hai creato l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, leggi la seguente procedura introduttiva al servizio.

## Prerequisiti per il piano Professional 1 Application
{: #prerequisites_p2}

Tieni conto di quanto segue, prima di configurare l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application
* {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application è supportato solo con i piani {{site.data.keyword.Db2_on_Cloud_short}} e {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}}.

* Devi avere accesso alle credenziali dell'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} o {{site.data.keyword.composeForPostgreSQL}} prima di poter configurare le impostazioni della tua istanza del servizio {{site.data.keyword.mobilefoundation_short}}.

L'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} può esistere in qualsiasi `Spazio` all'intero della tua `Organizzazione` {{site.data.keyword.Bluemix_notm}} o in qualsiasi altra `Organizzazione` a cui hai accesso. Assicurati di disporre delle autorizzazioni per accedere allo `Spazio` in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} o {{site.data.keyword.composeForPostgreSQL}}.
{: note}


## Configura la connessione al database
{: #configure_dashdb_p2}

###  Prime operazioni nella configurazione
{: #firststeps_p2}

Dopo che hai creato l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, completa la seguente procedura per iniziare a utilizzarla.

### Impostazione della connessione all'istanza del servizio Db2 on Cloud
{: #connect_dashdb_p2}

Una volta creata l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, vedrai la pagina *Panoramica*. Qui devi specificare le informazioni di connessione per l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} a cui deve connettersi l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}.

Se non hai un'istanza Db2 on Cloud esistente, puoi creare una nuova istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

Utilizza la seguente procedura per creare una nuova istanza del servizio Db2:

1. Nella pagina *Panoramica*, seleziona la sezione **Crea nuovo servizio**.

+ Seleziona `Sì` per l'opzione **Configurazione alta disponibilità **,
se desideri l'alta disponibilità per l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Controlla i dettagli del piano e fai clic su **Crea**.

Viene creata una nuova istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} che fornisce un'istanza
{{site.data.keyword.Db2_on_Cloud_short}} dedicata con 8 GB RAM e 2 vCPU e 500 GB di archiviazione.

Utilizza la seguente procedura per stabilire una connessione a un'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} esistente o all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} che hai creato:

1. Seleziona l'`Organizzazione` {{site.data.keyword.Bluemix_notm}} dove è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleziona lo `Spazio` {{site.data.keyword.Bluemix_notm}} in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}, dall'elenco di spazi disponibili nell'`Organizzazione` selezionata.   
Se non vedi elencati l'`Organizzazione` e lo `Spazio`, in cui è presente la tua istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}, verifica di essere un membro di tale `Organizzazione` e `Spazio`. Devi avere un ruolo di *Sviluppatore* per l'accesso all'organizzazione e allo spazio. Il servizio {{site.data.keyword.mobilefoundation_short}} accede alle credenziali dal servizio {{site.data.keyword.Db2_on_Cloud_short}}.
{: note}

+ Seleziona il `Nome servizio` e le `Credenziali` {{site.data.keyword.Db2_on_Cloud_short}} per connetterti all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} esistente.

+  Verifica la connessione all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} specificata.

+  Fai clic su **Aggiungi**. Questa azione crea le tabelle richieste nell'istanza del servizio database {{site.data.keyword.Db2_on_Cloud_short}} configurato.

Dopo pochi secondi, puoi accedere alla pagina `Panoramica` che ti fornisce le esercitazioni e i video per aiutarti a iniziare a lavorare con il servizio  {{site.data.keyword.mobilefoundation_short}}.

Non puoi modificare l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} configurata per essere utilizzata dalla tua istanza del servizio {{site.data.keyword.mobilefoundation_short}}. Tuttavia, puoi utilizzare la stessa istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} tra più istanze del servizio {{site.data.keyword.mobilefoundation_short}}, poiché ogni istanza di {{site.data.keyword.mobilefoundation_short}} crea il proprio schema nell'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} selezionata.
{: note}

## Avvio del server MobileFirst creato utilizzando il piano Professional 1 Application
{: #start_mobilefoundation_p2}

* Per avviare {{site.data.keyword.mfserver_short_notm}}, con le impostazioni predefinite, fai clic su **Avvia server di base**.

* Questa selezione crea un {{site.data.keyword.mfserver_long_notm}} con le seguenti impostazioni:
    -  1 GB di memoria. Questa dimensione è sufficiente per lo sviluppo, per delle attività moderate di test e i carichi di lavoro di produzione su piccola scala.

    -	Il `nome utente` e la `password` ti vengono generati
automaticamente. Disporrai dell'accesso ad essi quando il server è avviato e in esecuzione.

    Inizia il processo di creazione del tuo server. Questo processo impiega circa 10 minuti e una finestra di messaggio
indica l'avanzamento di questa operazione. Al suo completamento, viene visualizzato un dashboard
dove puoi vedere:

      -	Lo stato del tuo server in esecuzione (stato, dimensione).

      -	La rotta del server viene creata per te. Utilizza questa rotta nella tua applicazione mobile per collegarti a {{site.data.keyword.mfserver_short_notm}}.

      -	I tuoi `nomeutente` e `password` personali per accedere a {{site.data.keyword.mfp_oc_short_notm}}. La `password` è nascosta. Fai clic sull'icona **Mostra password** per visualizzarla.

*	Fai clic su **Avvia console** per aprire {{site.data.keyword.mfp_oc_short_notm}}.

Con la console, puoi gestire le tue applicazioni mobili, gli adattatori e i tuoi dispositivi mobili, utilizzare il tuo server come un backend mobile, inviare notifiche di push e altro ancora.

## Ricreazione del server MobileFirst quando utilizzi il piano Professional 1 Application
{: #recreate_mobilefoundation_p2}

*	Fai clic su **Ricrea** per ricreare il server.

* Questa azione arresta il tuo server esistente e elimina i dati. Viene creata una nuova istanza del server con una versione aggiornata, se disponibile. Il completamento di questa azione richiede
alcuni minuti.

I dati della tua istanza del server precedente che includono le informazioni su applicazioni e adattatori vengono mantenuti nell'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} configurata. Questi dati saranno utilizzati per ricreare il tuo server.
{: note}

##	Impostazione della configurazione avanzata nel piano Professional 1 Application
{: #using_mfs_advanced_p2}

Utilizza **Avvia server con la configurazione avanzata** dalla pagina `Panoramica` per creare il server con le impostazioni avanzate o personalizzate. Puoi inoltre
aggiornare le impostazioni del server per personalizzare la tua configurazione server facendo clic sulla scheda
**Configurazione**. {{site.data.keyword.mobilefoundation_short}} ti dà l'accesso ad alcune impostazioni avanzate.

*	Dalla scheda **Topologia**, puoi selezionare la dimensione del server e il numero di istanze del server di cui hai bisogno. Il server da 1 GB predefinito è sufficiente per lo sviluppo e test semplici.
  - Seleziona la dimensione corretta per il tuo server sulla base delle tue necessità.

  - **Istanze** visualizza il numero di nodi creati.

## Mobile Analytics nel piano Professional 1 Application
{: #mobile_analytics_p2}

Il server Mobile Analytics è incluso e preconfigurato con l'istanza del servizio del piano Mobile Foundation: Developer.

* Avvia la console Mobile Analytics da {{site.data.keyword.mfp_oc_short_notm}}.

Per ulteriori informazioni su Mobile Analytics, vedi [qui](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}.
