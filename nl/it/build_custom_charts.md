---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, charts, app sessions, crashes, graph

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Crea grafici personalizzati
{: #build_custom_charts}

La vista Custom Charts nella console Mobile Analytics ti fornisce la flessibilità per creare le tue visualizzazioni dei dati di analisi acquisiti e archiviati.  I grafici personalizzati ti sono utili nell'approfondimento di ulteriori informazioni rispetto a quelle preconfigurate o anche estendendole senza problemi nelle analisi di business formulate sui dati del cliente.

In questa vista, puoi scegliere uno qualsiasi dei dataset di analisi supportati e poi selezionare uno dei tipi di grafico supportati per tracciare il dataset.  Puoi anche ridurre ulteriormente la visualizzazione definendo dei filtri da applicare sui dati che vengono tracciati.  

I dataset supportati sono:
 * Log applicazione
 * Sessioni applicazione
 * Dati personalizzati
 * Transazioni di rete

I tipi di grafici supportati sono:
 * Grafico a barre
 * Diagramma di flusso
 * Grafico a linee
 * Gruppo di metriche
 * Grafico a torta
 * Tabella

La selezione del dataset, il tipo di grafico da tracciare, la definizione delle caratteristiche grafico e i filtri dei dati da applicare possono essere trasmessi insieme in una definizione del grafico personalizzata e salvati.  Puoi creare e salvare quante definizioni personalizzate del grafico desideri. I grafici personalizzati salvati vengono mostrati nella vista Custom Charts con i dati di analisi importanti tracciati.

## Creazione di un grafico personalizzato
{: #creating_custom_chart}

Crea un grafico personalizzato utilizzando le seguenti istruzioni:

1.  Fai clic sul pulsante **Create Chart** nella scheda **Custom Charts** dal dashboard Mobile Analytics.
2.  Nella scheda **General Settings**, seleziona **Chart Title**, **Event Type** e **Chart Type**.
3.  Alla selezione di *Event Type* e *Chart Type*, viene visualizzata la scheda **Chart Definition**. Utilizza la scheda *Chart Definition* per definire il grafico per il tipo di grafico specificato che hai precedentemente selezionato. Dopo aver definito il grafico, puoi configurare i filtri e le proprietà del grafico.
4.  I filtri del grafico (**Chart Filters**) vengono utilizzati per ottimizzare il grafico personalizzato. Può essere definito più di un filtro per un grafico.
    Ad esempio, dopo che hai definito un grafico per visualizzare la durata della sessione dell'applicazione media, se desideri visualizzare questo grafico solo per un'applicazione specifica puoi creare un filtro nel seguente modo:
    * Seleziona **Application Name** per **Property**.
    * Seleziona **Equals** per **Operator**.
    * Seleziona il nome della tua applicazione per **Value**.
    * Fai clic su **Add Filter**.
    Il filtro del nome dell'applicazione viene aggiunto alla tabella dei filtri per il tuo grafico.
5.  Le proprietà del grafico sono disponibili per i tipi di grafico **Tabella**, **Grafico a barre** e **Grafico a linee**. L'obiettivo delle proprietà del grafico è di migliorare come vengono presentati i dati in modo che la visualizzazione sia più efficiente.
    Se hai creato un grafico **Tabella**, le proprietà del grafico vengono impostate per definire la dimensione della pagina della tabella, il campo con il quale ordinare e l'ordine di ordinamento del campo.
    Se hai creato un grafico **Grafico a barre** o **Grafico a linee**, le proprietà del grafico vengono impostate per etichettare le righe della soglia per aggiungere un intervallo di riferimento per chiunque stia monitorando il grafico.

## Ottenimento di informazioni approfondite personalizzate dai log di dati personalizzati
{: #creating_custom_chart_for_client_logs}    

Se desideri ottenere informazioni personalizzate più approfondite su come ad esempio gli utenti traccino l'applicazione, devi prima acquisire le informazioni di traccia dell'utente pertinenti come la pagina scelta o l'opzione selezionata o il pulsante su cui è stato fatto clic come dati personalizzati e registrarli.  Consulta l'argomento sulla [strumentazione della tua applicazione](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app), su come registrare i dati personalizzati.

Successivamente, crea una definizione di grafico personalizzata con dei dati personalizzati come il tipo di evento e scegli il tipo di grafico. Procedendo con la definizione delle proprietà del grafico (**Chart Properties**) o dei filtri del grafico (**Chart Filters**), noterai che i valori e i tipi di dati personalizzati verranno visualizzati nelle caselle a discesa.  Effettua le selezioni importanti per il tipo di informazione approfondita che stai cercando.  

La profondità e l'utilità delle informazioni approfondite personalizzate dipende interamente da quanto efficacemente e in modo pertinente vengono definiti e acquisiti i dati personalizzati nelle tue applicazioni.
{: note}

## Tipi di grafici
{: #types_of_charts}

### Grafico a barre
{:  #bar_graph}

Il grafico a barre consente la visualizzazione di dati numerici su un asse X. Quando definisci un grafico a barre, devi prima scegliere il valore per l'asse X. Puoi scegliere tra i seguenti valori possibili.

* **Timeline** - se vuoi visualizzare i tuoi dati come un andamento (ad esempio, la durata della sessione dell'applicazione media nel tempo), scegli *Timeline* per l'asse X.
* **Property** - scegli Property se vuoi visualizzare una scomposizione del numero totale per la proprietà specifica. Se scegli Property per l'asse X, viene implicitamente scelto Total per l'asse Y. Ad esempio, scegli *Property* per l'asse X e *Application Name* per *Property* per visualizzare un numero per un tipo di evento specificato, che viene suddiviso per nome applicazione.

Dopo aver definito un valore per l'asse X, puoi definire un valore per l'asse Y. Se scegli *Timeline* per l'asse X, puoi scegliere i seguenti valori possibili per l'asse Y.

* **Average** - calcola la media di una proprietà numerica nel tipo di evento fornito.
* **Total** - un conteggio totale di una proprietà nel tipo di evento fornito.
* **Unique** - un conteggio univoco di una proprietà nel tipo di evento fornito.
Dopo aver definito gli assi del grafico, devi scegliere un valore per Property.

### Grafico a linee
{:  #line_graph}

Il grafico a linee consente la visualizzazione di alcune metriche nel tempo. Questo tipo di grafico è utile quando vuoi visualizzare la tendenza dei dati nel tempo. Il primo valore da definire quando crei un grafico a linee è **Measure**, che ha i seguenti valori possibili.

* **Average** - calcola la media di una proprietà numerica nel tipo di evento fornito.
* **Total** - un conteggio totale di una proprietà nel tipo di evento fornito.
* **Unique** - un conteggio univoco di una proprietà nel tipo di evento fornito.
Dopo aver definito la misurazione, devi scegliere un valore per **Property**.

### Diagramma di flusso
{:  #flow_chart}

Il diagramma di flusso consente la visualizzazione della scomposizione del flusso di una proprietà ad un'altra. Per un diagramma di flusso, devono essere impostate le seguenti proprietà.

* **Source** - il valore di un nodo di origine nel diagramma.
* **Destination** - il valore del nodo di destinazione nel diagramma.
* **Property** - un valore della proprietà dal nodo di origine o di destinazione.
Con il diagramma di flusso, puoi visualizzare la scomposizione della densità di varie origini che fluiscono a una destinazione o viceversa. Ad esempio, se vuoi visualizzare la scomposizione delle severità del log per un'applicazione, puoi definire i seguenti valori.

* Seleziona *Application Name* per **Source**.
* Seleziona *Log Level* per **Destination**.
* Seleziona il nome della tua applicazione per **Property**.

### Gruppo di metriche
{:  #metric_group}

Il gruppo di metriche può essere utilizzato per visualizzare una sola metrica che viene misurata come un valore medio, un conteggio totale o un conteggio univoco. Per definire un gruppo di metriche, devi definire uno dei seguenti valori possibili per **Measure**.

* **Average** - calcola la media di una proprietà numerica nel tipo di evento fornito.
* **Total** - un conteggio totale di una proprietà nel tipo di evento fornito.
* **Unique** - un conteggio univoco di una proprietà nel tipo di evento fornito.
Dopo aver definito la misurazione, devi scegliere un valore per *Property*. Questa metrica viene visualizzata nel gruppo di metriche.

### Grafico a torta
{:  #pie_chart}

Il grafico a torta può essere utilizzato per visualizzare la scomposizione del numero di valori per una proprietà particolare. Ad esempio, se vuoi visualizzare una scomposizione degli arresti anomali, definisci i seguenti valori.

* Seleziona *App Session* per **Event Type**.
* Seleziona *Pie Chart* per **Chart Type**.
* Seleziona *Closed By* per **Property**.
Il grafico a torta risultante mostra la scomposizione delle sessioni dell'applicazione che sono state chiuse dall'utente rispetto a quelle chiuse per un arresto anomalo.

### Tabella
{:  #table}

La tabella è utile quando vuoi visualizzare i dati non elaborati. La creazione di una tabella è semplice tanto quanto l'aggiunta delle colonne per i dati non elaborati che vuoi visualizzare.
Poiché non tutte le proprietà sono obbligatorie per dei tipi di evento specifici, possono essere visualizzati dei valori null nella tua tabella. Se vuoi evitare che queste righe vengano visualizzate nella tua tabella, aggiungi un filtro *Exists* per una proprietà specifica nella scheda **Chart Filters**
