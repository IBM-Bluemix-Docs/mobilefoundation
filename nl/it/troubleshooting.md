---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: troubleshooting techniques

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Risoluzione dei problemi generali
{: #general-troubleshooting}

Per isolare e risolvere i problemi con i tuoi prodotti {{site.data.keyword.IBM_notm}}, puoi utilizzare le informazioni sulla risoluzione dei problemi. Queste informazioni contengono le istruzioni per utilizzare le risorse di determinazione del problema che vengono fornite con i tuoi prodotti {{site.data.keyword.IBM_notm}}, incluso {{site.data.keyword.mobilefoundation_short}}.
{: shortdesc}

## Tecniche per la risoluzione dei problemi
{: #ts_overview}

La *risoluzione dei problemi* è un approccio sistematico per la risoluzione di un problema. L'obiettivo della risoluzione dei problemi è determinare perché qualcosa non funziona come previsto e come risolvere il problema. Alcune tecniche comuni possono rendere più semplice l'attività di risoluzione dei problemi.

Il primo passo del processo di risoluzione dei problemi è la descrizione completa del problema. Le descrizioni dei problemi aiutano l'utente e il rappresentante del supporto tecnico {{site.data.keyword.IBM_notm}} a capire da dove iniziare per trovare le cause del problema. In questa fase, è necessario porsi alcune domande di base:

- Quali sono i sintomi del problema?
- Dove si presenta il problema?
- Quando si presenta il problema?
- In quali condizioni si verifica il problema?
- Il problema può essere riprodotto?

Rispondendo a queste domande si ottiene una descrizione chiara del problema che può agevolarne la risoluzione.

### Quali sono i sintomi del problema?

Quando si inizia a descrivere un problema, la domanda più ovvia è **Qual è il problema?** Questa domanda potrebbe sembrare diretta, tuttavia è possibile suddividerla in alcune domande più mirate che creano un quadro più descrittivo del problema. Queste domande includono quanto segue:

- Chi o cosa sta notificando il problema?
- Quali sono i messaggi e i codici di errore?
- Qual è il malfunzionamento del sistema? Ad esempio, si tratta di un loop, un blocco, un arresto anomalo, una riduzione di prestazioni o di risultati non corretti?

### Dove si presenta il problema?

Determinare l'origine del problema non è sempre facile, tuttavia è una delle fasi più importanti nella risoluzione di un problema. Tra un componente di segnalazione e un componente in cui si verifica un errore possono esistere molti livelli di tecnologia. Reti, dischi e driver sono solo alcuni dei componenti da considerate quando si esaminano i problemi.

Le domande seguenti aiutano a individuare il punto esatto in cui si verifica il problema e a isolare il livello del problema:

- Il problema è specifico di una piattaforma o sistema operativo o si verifica su più piattaforme o sistemi operativi?
- L'ambiente e la configurazione attuali sono supportati?
- Tutti gli utenti riscontrano questo problema?
- (Per le installazioni su più siti). Il problema si verifica su tutti i siti?

Se il problema viene segnalato da un livello, non necessariamente il problema ha origine in quel livello. Parte dell'identificazione del punto in cui ha origine un problema è capire l'ambiente in cui esiste. È sempre utile dedicare tempo alla descrizione completa dell'ambiente del problema indicando sistema operativo e versione, tutti i software corrispondenti e relative versioni e le informazioni sull'hardware. Accertarsi che l'applicazione venga eseguita in un ambiente costituito da una configurazione supportata, molti problemi sono riconducibili a livelli di software incompatibili, non concepiti per essere eseguiti insieme o non collaudati completamente insieme.

### Quando si presenta il problema?

Sviluppare una sequenza temporale dettagliata degli eventi che portano al malfunzionamento, soprattutto in quei casi in cui il problema si verifica una sola volta. È possibile sviluppare una sequenza temporale molto facilmente andando all'indietro: iniziare dal momento in cui è stato riportato l'errore (il più precisamente possibile, fino al millisecondo), e andare a ritroso tra i log e le informazioni disponibili. Di solito, è necessario ripercorrere gli eventi solo fino al primo a dimostrarsi sospetto riscontrato nel log diagnostico.

Per sviluppare una linea temporale di eventi, rispondere alle seguenti domande:

- Il problema si verifica solo ad una certa ora del giorno o della notte?
- Con quale frequenza di verifica il problema?
- Quale sequenza di eventi porta al momento in cui viene riportato il problema?
- Il problema si verifica dopo una modifica all'ambiente, ad esempio l'aggiornamento o l'installazione di software o hardware?

Le risposte a questi tipi di domande forniscono un quadro generale che è possibile utilizzare come riferimento nel determinare la risoluzione del problema.

### In quali condizioni si verifica il problema?

Per risolvere un problema, è importante sapere quali sistemi e applicazioni sono in esecuzione nel momento in cui si verifica il problema. Le seguenti domande sull'ambiente possono facilitare l'identificazione della causa principale del problema:

- Il problema si verifica sempre durante l'esecuzione della stessa attività?
- È necessario che si verifichi una certa sequenza di eventi perché si riscontri il problema?
- Si verifica contemporaneamente un malfunzionamento di altre applicazioni?

Rispondere a questo tipo di domande può facilitare l'illustrazione dell'ambiente in cui si verifica il problema e a correlare eventuali dipendenze. Solo perché più problemi si verificano contemporaneamente, non vuol dire che sono necessariamente correlati.

### Il problema può essere riprodotto?

Da un punto di vista della risoluzione dei problemi, il problema ideale è quello che può essere riprodotto. Di solito, potendo riprodurre un problema, si può fare ricorso ad un insieme più vasto di strumenti e procedure per svolgere le indagini. Di conseguenza, i problemi che possono essere riprodotti sono spesso più facili da risolvere.

Tuttavia, i problemi che è possibile riprodurre possono presentare uno svantaggio: se il problema impatta l'attività di business in modo significativo, non si desidera che si verifichi. Se possibile, ricreare il problema in un ambiente di test o di sviluppo che offre in genere una maggiore flessibilità e un maggiore controllo durante l'analisi.

- È possibile riprodurre il problema su un sistema di test?
- Più utenti o applicazioni stanno riscontrando lo stesso tipo di problema?
- È possibile ricreare il problema eseguendo un unico comando, un insieme di comandi o una particolare applicazione?


##  Limitazioni note
{: #knownlimitations_mfp}

* La IU del servizio {{site.data.keyword.mobilefoundation_short}} non utilizza il modello specifico della locale selezionato dall'utente per visualizzare i numeri.

## Come ottenere aiuto e supporto per Mobile Foundation
{: #getting_help_mobilefoundation}

Se hai dei problemi o delle domande quando utilizzi {{site.data.keyword.mobilefoundation_short}}, puoi ottenere aiuto ricercando le informazioni o facendo delle domande in un forum. Puoi inoltre aprire un ticket di supporto.

Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che venga individuata dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.

Se hai domande tecniche sullo sviluppo o sulla distribuzione di un'applicazione con {{site.data.keyword.mobilefoundation_short}}, inserisci la tua domanda in [Stack Overflow ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://stackoverflow.com/search?q=ibm-mobilefirst+bluemix){:new_window} e contrassegnala con le tag `bluemix` e `ibm-mobilefirst`.

Per domande sul servizio e sulle istruzioni per l'utilizzo iniziale, utilizza il forum [IBM developerWorks dW Answers ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/mobilefirst/?smartspace=bluemix){:new_window}. Includi le tag `bluemix` e  `mobilefirst`.

Per ulteriori informazioni sull'apertura di un ticket di supporto IBM o sui livelli di supporto e sulle gravità del ticket, vedi [Come contattare il supporto](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support){: new_window}.
