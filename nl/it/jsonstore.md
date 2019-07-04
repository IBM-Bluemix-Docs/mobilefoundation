---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-14"

keywords: JSONStore, offline storage, jsonstore error codes

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:pre: .pre}


# JSONStore
{: #jsonstore}
Il **JSONStore** {{site.data.keyword.mobilefoundation_short}} è un'API lato client facoltativa che fornisce un semplice sistema di archiviazione orientato ai documenti. JSONStore abilita l'archiviazione persistente dei **documenti JSON**. I documenti presenti in un'applicazione sono disponibili in JSONStore anche quando il dispositivo su cui è in esecuzione l'applicazione è offline. Questa archiviazione persistente e sempre disponibile può essere utile per consentire agli utenti di accedere ai documenti quando, ad esempio, non è disponibile alcuna connessione di rete nel dispositivo.
{: shortdesc}

Poiché è familiare per gli sviluppatori, in questa documentazione a volte viene utilizzata la terminologia dei database relazionali per aiutare a spiegare JSONStore. Esistono tuttavia molte differenze tra un database relazionale e JSONStore. Ad esempio, lo schema rigoroso utilizzato per archiviare i dati nei database relazionali è diverso dall'approccio di JSONStore. Con JSONStore, puoi archiviare qualsiasi contenuto JSON e indicizzare il contenuto che devi ricercare.

## Funzioni chiave
{: #key-features-jsonstore }
* Indicizzazione dei dati per una ricerca efficiente
* Meccanismo per tracciare le modifiche solo locali ai dati archiviati
* Supporto per molti utenti
* La crittografia AES 256 dei dati archiviati garantisce sicurezza e riservatezza. Puoi segmentare la protezione per utente con la protezione tramite password nel caso in cui ci sia più di un utente su un singolo dispositivo.

Un singolo archivio può avere molte raccolte e ogni raccolta può avere molti documenti. È anche possibile avere un'applicazione MobileFirst composta da più archivi. Per informazioni, vedi Supporto di più utenti di JSONStore.

## Livello di supporto
{: #support-level-jsonstore }
* JSONStore è supportato nelle applicazioni native iOS e Android (nessun supporto per Windows nativo (Universal e UWP)).
* JSONStore è supportato nelle applicazioni Cordova iOS, Android e Windows (Universal e UWP).


## Terminologia generale di JSONStore
{: #general-jsonstore-terminology }
### Documento
{: #document }
Un documento è l'elemento costitutivo di base di JSONStore.

Un documento JSONStore è un oggetto JSON con un identificativo generato automaticamente (`_id`) e con dei dati JSON. È simile a un record o una riga nella terminologia dei database. Il valore di `_id` è sempre un numero intero univoco all'interno di una specifica raccolta. Alcune funzioni come `add`, `replace` e `remove` nella classe `JSONStoreInstance` prendono un array di documenti o di oggetti. Questi metodi sono utili per eseguire operazioni su vari documenti o oggetti contemporaneamente.

Singolo documento:

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

Array di documenti:

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Raccolta
{: #collection }
Una raccolta JSONStore è simile a una tabella nella terminologia dei database.  
Il seguente esempio di codice non rappresenta il modo in cui i documenti sono archiviati su disco, ma è un buon modo per visualizzare l'aspetto di una raccolta ad alto livello.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Archivio
{: #store }
Un archivio è il file persistente di JSONStore costituito da una o più raccolte.  
Un archivio è simile a un database relazionale nella terminologia dei database. Un archivio è indicato anche come JSONStore.

### Campi di ricerca
{: #search-fields }
Un campo di ricerca è una coppia chiave-valore.  
I campi di ricerca sono chiavi che vengono indicizzate per ridurre i tempi di ricerca, in modo simile ai campi o agli attributi di colonna nella terminologia dei database.

I campi di ricerca aggiuntivi sono chiavi indicizzate ma che non fanno parte dei dati JSON archiviati. Questi campi definiscono la chiave i cui valori (nella raccolta JSON) sono indicizzati e possono essere utilizzati per effettuare ricerche in modo più rapido.

I tipi di dati validi sono: stringa, booleano, numero e intero. Questi tipi sono solo suggerimenti di tipo, non c'è una convalida del tipo. Inoltre, questi tipi determinano come vengono archiviati i campi indicizzabili. Ad esempio, `{age: 'number'}` indicizzerà 1 come 1,0 e `{age: 'integer'}` indicizzerà 1 come 1.

Campi di ricerca e campi di ricerca aggiuntivi:

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

È possibile indicizzare solo le chiavi all'interno di un oggetto, non l'oggetto stesso. Gli array sono gestiti in modalità pass-through, il che significa che non puoi indicizzare un array o un indice specifico dell'array (arr[n]), ma puoi indicizzare gli oggetti all'interno di un array.

Indicizzazione dei valori all'interno di un array:

```javascript

var searchFields = {
    'people.name' : 'string', // matches carlos and tim on myObject
    'people.age' : 'integer' // matches 99 and 100 on myObject
};

var myObject = {
    people : [
        {name: 'carlos', age: 99},
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### Query
{: #queries }
Le query sono oggetti che utilizzano campi di ricerca o campi di ricerca aggiuntivi per cercare i documenti.  
In questi esempi si suppone che il campo di ricerca del nome sia di tipo stringa e che il campo di ricerca dell'età sia di tipo intero.

Trova documenti dove `name` corrisponde a `carlos`:

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

Trova documenti dove `name` corrisponde a `carlos` e `age` corrisponde a `99`:

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### Parti di query
{: #query-parts }
Le parti di query vengono utilizzate per creare ricerche più avanzate. Alcune operazioni di JSONStore, come ad esempio alcune versioni di `find` o `count` prendono parti di query. Tutto ciò che è all'interno di una parte di query viene unito da istruzioni `AND`, mentre le parti di query stesse vengono unite da istruzioni `OR`. I criteri di ricerca restituiscono una corrispondenza solo se tutti gli elementi della parte di query sono **true**. Puoi utilizzare più di una parte di query per cercare corrispondenze che soddisfino una o più delle parti di query.

La ricerca con parti di query funziona solo nei campi di ricerca di primo livello. Ad esempio: `name` e non `name.first`. Per ovviare a questo comportamento, utilizza più raccolte in cui tutti i campi di ricerca siano di primo livello. Le operazioni delle parti di query che funzionano con campi di ricerca non di primo livello sono: `equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike` e `notLeftLike`. Il comportamento è indeterminato se utilizzi campi di ricerca non di primo livello.

## Tabella delle funzioni
{: #features-table }
Confronta le funzioni di JSONStore con quelle di altre tecnologie e formati di archiviazione dei dati.

JSONStore è un'API JavaScript per l'archiviazione dei dati all'interno delle applicazioni Cordova che utilizzano il plug-in MobileFirst, un'API Objective-C per applicazioni iOS native e un'API Java per applicazioni Android native. Come riferimento, di seguito viene fornito un confronto tra le diverse tecnologie di archiviazione JavaScript per vedere il paragone con JSONStore.

JSONStore è simile a tecnologie come LocalStorage, Indexed DB, Cordova Storage API e Cordova File API. La tabella mostra come alcune funzioni fornite da JSONStore vengono confrontate con altre tecnologie. La funzione di JSONStore è disponibile solo su dispositivi e simulatori iOS e Android.

| Funzione                                            | JSONStore      | LocalStorage | IndexedDB | Cordova storage API | Cordova file API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Supporto Android (applicazioni Cordova e native)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Supporto iOS (applicazioni Cordova e native)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal e Windows 10 UWP (applicazioni Cordova)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| Crittografia dei dati	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Archiviazione massima	                                 |Spazio disponibile |    ~5 MB     |   ~5 MB 	 | Spazio disponibile	   | Spazio disponibile  |
| Archiviazione affidabile (vedi nota)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| Tenere traccia delle modifiche locali	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Supporto multiutente                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Indicizzazione	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| Tipo di archiviazione	                                 | Documenti JSON | Coppie chiave-valore | Documenti JSON | Relazionale (SQL) | Stringhe     |
{: caption="Tabella 1. Confronto delle funzioni" caption-side="top"}

Per archiviazione affidabile si intende che i tuoi dati non vengono eliminati a meno che non si verifichi uno dei seguenti eventi:
* L'applicazione viene rimossa dal dispositivo.
* Viene richiamato uno dei metodi che rimuove i dati.
{: note}

## Supporto di più utenti
{: #multiple-user-support }
Con JSONStore, puoi creare vari archivi costituiti da diverse raccolte in un'unica applicazione MobileFirst.

L'API init (JavaScript) od open (iOS nativo e Android nativo) può prendere un oggetto options con un nome utente. I diversi archivi sono file separati nel file system. Il nome utente viene utilizzato come nome file dell'archivio. Questi archivi separati possono essere crittografati con password diverse per motivi di sicurezza e privacy. La chiamata all'API closeAll rimuove l'accesso a tutte le raccolte. È anche possibile modificare la password di un archivio crittografato chiamando l'API changePassword.

Un caso di utilizzo di esempio potrebbe essere rappresentato da vari dipendenti che condividono un dispositivo fisico (ad esempio, un iPad o un tablet Android) e un'applicazione MobileFirst. Il supporto di più utenti è utile quando i dipendenti lavorano in turni diversi e gestiscono dati privati di diversi clienti, mentre utilizzano l'applicazione MobileFirst.

## Sicurezza
{: #security-jsonstore }
Puoi proteggere tutte le raccolte in un archivio crittografandole.

Per crittografare tutte le raccolte in un archivio, passa una password all'API `init` (JavaScript) o `open` (iOS nativo e Android nativo). Se non viene passata alcuna password, nessuno dei documenti presenti nelle raccolte dell'archivio viene crittografato.

Alcune risorse di sicurezza (ad esempio, salt) vengono memorizzate nel keychain (iOS), nelle preferenze condivise (Android) e nella casella di sicurezza della credenziali (Windows Universal 8.1 e Windows 10 UWP). L'archivio è crittografato con una chiave AES (Advanced Encryption Standard) a 256 bit. Tutte le chiavi sono rafforzate con PBKDF2 (Password-Based Key Derivation Function 2). Puoi scegliere di crittografare raccolte di dati per un'applicazione, ma non puoi passare da formati crittografati a quelli in testo semplice o combinare i formati all'interno di un archivio.

La chiave che protegge i dati nell'archivio si basa sulla password utente che fornisci. La chiave non scade, ma puoi modificarla chiamando l'API changePassword.

La chiave di protezione dei dati (DPK) è la chiave utilizzata per decrittografare il contenuto dell'archivio. La DPK viene mantenuta nel keychain iOS anche se l'applicazione viene disinstallata. Per rimuovere sia la chiave nel keychain che tutto ciò che JSONStore inserisce nell'applicazione, utilizza l'API di eliminazione permanente (destroy). Questo processo non è applicabile ad Android perché la DPK crittografata è archiviata nelle preferenze condivise e viene cancellata quando si disinstalla l'applicazione.

La prima volta che JSONStore apre una raccolta con una password, il che significa che lo sviluppatore vuole crittografare i dati all'interno dell'archivio, JSONStore necessita di un token casuale. Tale token casuale può essere ottenuto dal client o dal server.

Quando la chiave localKeyGen è presente nell'implementazione JavaScript dell'API JSONStore e ha un valore true, viene generato localmente un token crittograficamente sicuro. In caso contrario, il token viene generato contattando il server, il che richiede la connettività al server MobileFirst. Questo token è richiesto solo la prima volta che un archivio viene aperto con una password. Le implementazioni native (Objective-C e Java) generano un token crittograficamente sicuro in locale per impostazione predefinita oppure puoi passarne uno attraverso l'opzione secureRandom.

Il compromesso è tra:
* aprire un archivio offline e fare affidamento al client per generare quel token casuale (meno sicuro) oppure
* aprire l'archivio con accesso al server MobileFirst (richiede connettività) e fare affidamento al server (più sicuro)

### Programmi di utilità di sicurezza
{: #security-utilities }
L'API lato client di MobileFirst fornisce alcuni programmi di utilità di sicurezza per proteggere i dati dell'utente. Funzioni come JSONStore sono ottime se vuoi proteggere gli oggetti JSON. Tuttavia, non si consiglia di archiviare blob binari in una raccolta JSONStore.

Invece, archivia i dati binari sul file system e archivia i percorsi di file e altri metadati all'interno di una raccolta JSONStore. Se vuoi proteggere file come, ad esempio, le immagini, puoi codificarli come stringhe in base64, crittografarli e scrivere l'output sul disco.

Per decrittografare i dati, puoi cercare i metadati in una raccolta JSONStore, leggere i dati crittografati dal disco e decrittografarli utilizzando i metadati archiviati.

Questi metadati possono includere la chiave, salt, il vettore di inizializzazione (IV), il tipo di file, il percorso del file e altro.

Scopri di più sui [Programmi di utilità di sicurezza JSONStore](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities).
{: tip}

### Crittografia di Windows 8.1 Universal e Windows 10 UWP
{: #windows-81-universal-and-windows-10-uwp-encryption }
Puoi proteggere tutte le raccolte in un archivio crittografandole.

JSONStore utilizza [SQLCipher ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://sqlcipher.net/){: new_window} come propria tecnologia di database sottostante. SQLCipher è una build di SQLite prodotta da Zetetic, LLC che aggiunge un livello di crittografia al database.

JSONStore utilizza SQLCipher su tutte le piattaforme. Su Android e iOS è disponibile una versione open source gratuita di SQLCipher, nota come Community Edition, che viene incorporata nelle versioni di JSONStore incluse in Mobile Foundation. Le versioni Windows di SQLCipher sono disponibili solo con una licenza commerciale e non possono essere ridistribuite direttamente da Mobile Foundation.

Invece, JSONStore per Windows 8 Universal include SQLite come database sottostante. Per crittografare i dati per una di queste piattaforme, devi acquisire la tua propria versione di SQLCipher e sostituire la versione di SQLite inclusa in Mobile Foundation.

Se non hai bisogno di crittografia, JSONStore è completamente funzionale (ad eccezione della crittografia) utilizzando la versione SQLite in Mobile Foundation.

#### Sostituzione di SQLite con SQLCipher per Windows Universal e Windows UWP
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. Esegui l'estensione SQLCipher per Windows Runtime 8.1/10 fornita con SQLCipher per Windows Runtime Commercial Edition.
2. Dopo che l'estensione ha terminato l'installazione, individua la versione SQLCipher del file **sqlite3.dll** appena creato. Ce n'è una per x86, una per x64 e una per ARM.

    ```bash
    C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
    ```
    {: codeblock}

3. Copia e sostituisci questo file nella tua applicazione MobileFirst.

    ```bash
    <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
    ```
    {: codeblock}

## Prestazioni
{: #performance-jsonstore }
Di seguito sono riportati i fattori che possono influire sulle prestazioni di JSONStore.

### Rete
{: #network-jsonstore }
* Controlla la connettività di rete prima di eseguire operazioni come, ad esempio, l'invio di tutti i documenti dirty a un adattatore.
* La quantità di dati inviati sulla rete a un client influisce pesantemente sulle prestazioni. Invia solo i dati richiesti dall'applicazione, invece di copiare tutto ciò che è presente nel database di backend.
* Se stai utilizzando un adattatore, considera la possibilità di impostare l'indicatore compressResponse su true. In questo modo, le risposte vengono compresse, che generalmente utilizzano meno larghezza di banda e hanno un tempo di trasferimento più veloce rispetto a quelle senza compressione.

### Memoria
{: #memory-jsonstore }
* Quando utilizzi l'API JavaScript, i documenti JSONStore vengono serializzati e deserializzati come stringhe tra il livello nativo (Objective-C, Java o C#) e il livello JavaScript. Un modo per mitigare possibili problemi di memoria consiste nell'utilizzare limite e offset quando utilizzi l'API di ricerca (find). In questo modo, limiti la quantità di memoria assegnata per i risultati e puoi implementare funzionalità come la paginazione (mostra un numero X di risultati per pagina).
* Invece di utilizzare nomi di chiave lunghi che alla fine vengono serializzati e deserializzati come stringhe, puoi associare quei nomi di chiave lunghi ad altri più brevi (ad esempio: `myVeryVeryVerLongKeyName` a `k` o `key`). Idealmente, li associ ai nomi di chiave brevi quando li invii dall'adattatore al client e li associ ai nomi di chiave lunghi originali quando invii di nuovo i dati al backend.
* Considera la possibilità di suddividere i dati all'interno di un archivio in varie raccolte. Disponi piccoli documenti su varie raccolte anziché documenti monolitici in una singola raccolta. Questa considerazione dipende da quanto siano correlati i dati e dai casi di utilizzo per questi dati.
* Quando utilizzi l'API di aggiunta (add) con un array di oggetti, è possibile che si verifichino problemi di memoria. Per mitigare questo problema, chiama questi metodi con meno oggetti JSON alla volta.
* JavaScript e Java™ hanno i raccoglitori dei dati inutilizzati, mentre Objective-C utilizza l'ARC (Automatic Reference Counting). Utilizzalo tranquillamente, ma non dipendere interamente da questa funzione. Prova ad annullare i riferimenti che non usi e utilizza lo strumento di profilazione per verificare che l'utilizzo della memoria diminuisca quando ti aspetti che ciò avvenga.

### CPU
{: #cpu-jsonstore }
* La quantità di campi di ricerca e campi di ricerca aggiuntivi utilizzati influiscono sulle prestazioni quando chiami il metodo add, che esegue l'indicizzazione. Indicizza solo i valori utilizzati nelle query per il metodo find.
* Per impostazione predefinita, JSONStore tiene traccia delle modifiche locali ai suoi documenti. Questo comportamento può essere disabilitato, risparmiando quindi alcuni cicli, impostando l'indicatore `markDirty` su **false** quando utilizzi le API di aggiunta (add), rimozione (remove) e sostituzione (replace).
* L'abilitazione della sicurezza aggiunge un sovraccarico alle API `init` o `open` e ad altre operazioni che funzionano con i documenti all'interno della raccolta. Considera se la sicurezza è realmente necessaria. Ad esempio, l'API open è molto più lenta con la crittografia poiché deve generare le chiavi di crittografia utilizzate per la crittografia e la decrittografia.
* Le API `replace` e `remove` dipendono dalla dimensione della raccolta in quanto devono passare attraverso l'intera raccolta per sostituire o rimuovere tutte le ricorrenze. Poiché deve passare attraverso ogni record, deve decrittografarli uno a uno, il che la rende molto più lenta quando viene utilizzata la crittografia. Questo calo di prestazioni è più evidente nelle raccolte di grandi dimensioni.
* L'API `count` è relativamente costosa. Tuttavia, puoi mantenere una variabile che mantenga il conteggio per tale raccolta. Aggiornala ogni volta che archivi o rimuovi elementi dalla raccolta.
* Le API `find` (`find`, `findAll` e `findById`) sono influenzate dalla crittografia, in quanto devono decrittografare ogni documento per vedere se si tratta di una corrispondenza o meno. Per una ricerca tramite query, se viene superato un limite, è potenzialmente più veloce in quanto si arresta quando raggiunge il limite dei risultati. JSONStore non ha bisogno di decrittografare il resto dei documenti per scoprire se rimangono altri risultati della ricerca.

## Simultaneità
{: #concurrency-jsonstore }
### Simultaneità in JavaScript
{: #javascript-jsonstore }
La maggior parte delle operazioni che possono essere eseguite su una raccolta, come ad esempio l'aggiunta e la ricerca, sono asincrone. Queste operazioni restituiscono una promessa jQuery che viene risolta quando l'operazione viene completata correttamente e rifiutata quando si verifica un errore. Queste promesse sono simili ai callback di esito positivo e malfunzionamento.

Un valore Deferred di jQuery è una promessa che può essere risolta o rifiutata. I seguenti esempi non sono specifici di JSONStore, ma hanno lo scopo di aiutarti a capire il loro utilizzo in generale.

Invece di promesse e callback, puoi anche ascoltare gli eventi `success` e `failure` di JSONStore. Esegui le azioni che si basano sugli argomenti passati al listener di eventi.

Esempio di definizione della promessa:

```javascript
var asyncOperation = function () {
  // Assumes that you have jQuery defined via $ in the environment
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```
{: codeblock}

Esempio di utilizzo della promessa:

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Esempio di definizione del callback:

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

Esempio di utilizzo del callback:

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Eventi di esempio:

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```
{: codeblock}

### Simultaneità in Objective-C
{: #objective-c-jsonstore }
Quando utilizzi l'API iOS nativa per JSONStore, tutte le operazioni vengono aggiunte a una coda di assegnazione sincrona. Questo comportamento garantisce che le operazioni che interessano l'archivio vengano eseguite in ordine su un thread che non è il thread principale. Per ulteriori informazioni, consulta la documentazione di Apple in [Grand Central Dispatch (GCD) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}.

### Simultaneità in Java
{: #java-jsonstore }
Quando utilizzi l'API Android nativa per JSONStore, tutte le operazioni vengono eseguite sul thread principale. Devi creare thread o utilizzare pool di thread per ottenere un comportamento asincrono. Tutte le operazioni di archivio sono con protezione di thread.

## Analisi
{: #analytics-jsonstore }
Puoi raccogliere elementi chiave delle informazioni di analisi relative a JSONStore

### Informazioni sul file
{: #file-information }
Se l'API JSONStore viene chiamata con l'indicatore di analisi impostato su **true**, le informazioni sul file vengono raccolte una volta per ogni sessione dell'applicazione. Una sessione dell'applicazione è definita come il caricamento dell'applicazione nella memoria e la sua rimozione dalla memoria. Puoi utilizzare queste informazioni per determinare la quantità di spazio utilizzato dal contenuto JSONStore nell'applicazione.

### Metriche delle prestazioni
{: #performance-metrics }
Le metriche delle prestazioni vengono raccolte ogni volta che viene chiamata un'API JSONStore con informazioni sulle ore di inizio e di fine di un'operazione. Puoi utilizzare queste informazioni per determinare il tempo impiegato dalle varie operazioni in millisecondi.

### Esempio JSONStore per iOS
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

### Esempio JSONStore per Android
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

### Esempio JSONStore per JavaScript
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## Utilizzo dei dati esterni
{: #working-with-external-data }
Puoi lavorare con i dati esterni in diversi concetti: **Pull** e **Push**.

### Pull
{: #pull }
Molti sistemi utilizzano il termine pull per fare riferimento al richiamo di dati da un'origine esterna.  
Esistono tre elementi importanti:

#### Pull dall'origine dati esterna
{: #external-data-source-pull }
Questa origine può essere un database, un'API REST o SOAP o molto altro. L'unico requisito è che deve essere accessibile dal server MobileFirst o direttamente dall'applicazione client. Idealmente, vuoi che questa origine restituisca i dati in formato JSON.

#### Livello di trasporto per il pull
{: #transport-layer-pull }
Questa origine rappresenta come ottieni i dati dall'origine esterna alla tua origine interna, una raccolta JSONStore all'interno dell'archivio. Un'alternativa è un adattatore.

#### API origine dati interna per il pull
{: #internal-data-source-api-pull }
Questa origine rappresenta le API JSONStore che puoi utilizzare per aggiungere i dati JSON a una raccolta.

Puoi popolare l'archivio interno con i dati letti da un file, un campo di input o dati hardcoded in una variabile. Non devono provenire esclusivamente da un'origine esterna che richiede la comunicazione di rete.
{: note}

Tutti i seguenti esempi di codice sono scritti in uno pseudocodice simile a JavaScript.

Utilizza gli adattatori per il livello di trasporto. Alcuni dei vantaggi dell'utilizzo degli adattatori sono conversione da XML a JSON, sicurezza, filtraggio e disaccoppiamento del codice lato server e del codice lato client.
{: note}
##### Origine dati esterna: endpoint REST di backend 
{: #external-data-source-backend-rest-endpoint-head}
Immagina di avere un endpoint REST che legge i dati da un database e li restituisce come un array di oggetti JSON.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

I dati restituiti possono essere simili al seguente esempio:

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

##### Livello di trasporto: adattatore 
{: #transport-layer-adapter}
Immagina di aver creato un adattatore chiamato people e di aver definito una procedura chiamata getPeople. La procedura richiama l'endpoint REST e restituisce l'array di oggetti JSON al client. Qui potresti voler svolgere più attività, ad esempio, restituire solo un sottoinsieme dei dati al client.

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Sul client, puoi utilizzare l'API WLResourceRequest per ottenere i dati. Inoltre, potresti voler passare alcuni parametri dal client all'adattatore. Un esempio è una data con l'ultima volta in cui il client ha ottenuto nuovi dati dall'origine esterna tramite l'adattatore.

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

Potresti voler utilizzare `compressResponse`, `timeout` e altri parametri che possono essere passati all'API `WLResourceRequest`.  
{: note}

Facoltativamente, puoi ignorare l'adattatore e utilizzare qualcosa come jQuery.ajax per contattare direttamente l'endpoint REST con i dati che vuoi archiviare.

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### API origine dati interna: JSONStore 
{: #internal-data-source-api-jsonstore}
Dopo aver ottenuto la risposta dal backend, lavora con i dati utilizzando JSONStore.
JSONStore fornisce un modo per tracciare le modifiche locali. Abilita alcune API a contrassegnare i documenti come dirty. L'API registra l'ultima operazione che è stata eseguita sul documento e quando il documento è stato contrassegnato come dirty. Puoi quindi utilizzare queste informazioni per implementare funzioni come la sincronizzazione dei dati.

L'API di modifica (change) prende i dati e alcune opzioni:

###### replaceCriteria
{: #replacecriteria}
  
Questi campi di ricerca fanno parte dei dati di input. Sono utilizzati per individuare i documenti che sono già all'interno di una raccolta. Ad esempio, se selezioni:

```javascript
['id', 'ssn']
```
{: codeblock}

Come criterio di sostituzione, passa il seguente array come dati di input:

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

E la raccolta `people` è già composta dal seguente documento:

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

L'operazione `change` individua un documento che corrisponde esattamente alla seguente query:

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

Quindi, l'operazione `change` esegue una sostituzione con i dati di input e la raccolta e composta da:

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

Il nome è stato modificato da `Carlitos` a `Carlos`. Se più di un documento corrisponde al criterio di sostituzione, tutti i documenti corrispondenti vengono sostituiti con i rispettivi dati di input.

###### addNew 
{: #addnew}
 
Quando nessun documento corrisponde al criterio di sostituzione, l'API di modifica (change) controlla il valore di questo indicatore. Se l'indicatore è impostato su **true**, l'API di modifica crea un nuovo documento e lo aggiunge all'archivio. Altrimenti, non vengono eseguite ulteriori azioni.

###### markDirty 
{: #markdirty}
 
Determina se l'API di modifica (change) contrassegna come dirty i documenti che vengono sostituiti o aggiunti.

Viene restituito un array di dati dall'adattatore:

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

Puoi utilizzare altre API per tracciare le modifiche ai documenti locali archiviati. Ottieni sempre un accessor alla raccolta su cui esegui le operazioni.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

Quindi, puoi aggiungere i dati (array di oggetti JSON) e decidere se vuoi che vengano contrassegnati come dirty o meno. In genere, devi impostare l'indicatore markDirty su false quando ottieni modifiche dall'origine esterna. Imposta l'indicatore su true quando aggiungi i dati localmente.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

Puoi anche sostituire un documento e scegliere di contrassegnare il documento con le sostituzioni come dirty o meno.

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

Allo stesso modo, puoi rimuovere un documento e scegliere di contrassegnare la rimozione come dirty o meno. I documenti rimossi e contrassegnati come dirty non vengono visualizzati quando utilizzi l'API di ricerca (find). Tuttavia, sono ancora all'interno della raccolta finché non utilizzi l'API `markClean`, che rimuove fisicamente i documenti dalla raccolta. Se il documento non è contrassegnato come dirty, viene fisicamente rimosso dalla raccolta.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### Push
{: #push }
Molti sistemi utilizzano il termine push per fare riferimento all'invio di dati a un'origine esterna.

Esistono tre elementi importanti:

#### API origine dati interna per il push
{: #internal-data-source-api-push }
Questa origine è l'API JSONStore che restituisce i documenti con modifiche solo locali (dirty).

#### Livello di trasporto per il push
{: #transport-layer-push }
Questa origine rappresenta come vuoi contattare l'origine dati esterna per inviare le modifiche.

#### Push all'origine dati esterna
{: #external-data-source-push }
Questa origine è tipicamente un endpoint di database, REST o SOAP, tra gli altri, che riceve gli aggiornamenti apportati ai dati dal client.

Tutti i seguenti esempi di codice sono scritti in uno pseudocodice simile a JavaScript.

Utilizza gli adattatori per il livello di trasporto. Alcuni dei vantaggi dell'utilizzo degli adattatori sono conversione da XML a JSON, sicurezza, filtraggio e disaccoppiamento del codice lato server e del codice lato client.
{: note}

##### API origine dati interna: JSONStore   
{: #internal-data-source-api-jsonstore-head}
Dopo aver ottenuto un accessor alla raccolta, chiama l'API `getAllDirty` per richiamare tutti i documenti contrassegnati come dirty. Questi documenti hanno modifiche solo locali che desideri inviare all'origine dati esterna attraverso un livello di trasporto.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

L'argomento `dirtyDocs` è simile al seguente esempio:

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

I campi sono:
* `_id`: campo interno utilizzato da JSONStore. A ogni documento ne viene assegnato uno univoco.
* `json`: i dati che sono stati archiviati.
* `_operation`: l'ultima operazione che è stata eseguita sul documento. I valori possibili sono add, store, replace e remove.
* `_dirty`: una data/ora che viene memorizzata come numero per indicare quando il documento è stato contrassegnato come dirty.

##### Livello di trasporto: adattatore MobileFirst   
{: #transport-layr-mobilefirst-adapter }
Puoi scegliere di inviare i documenti dirty a un adattatore. Supponiamo che tu abbia un adattatore `people` definito con una procedura `updatePeople`.

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

Potresti voler utilizzare `compressResponse`, `timeout` e altri parametri che possono essere passati all'API `WLResourceRequest`.
{: note}

Sul server MobileFirst, l'adattatore ha la procedura `updatePeople`, che potrebbe essere simile al seguente esempio:

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Invece di trasmettere l'output dall'API `getAllDirty` sul client, è possibile che tu debba aggiornare il payload in modo che corrisponda a un formato previsto dal backend. Potresti dover suddividere le sostituzioni, rimozioni e inclusioni in chiamate API di backend separate.

Facoltativamente, puoi scorrere l'array `dirtyDocs` e controllare il campo `_operation`. Quindi, invia le sostituzioni a una procedura, le rimozioni a un'altra procedura e le inclusioni a un'altra procedura ancora. L'esempio precedente invia tutti i documenti dirty in blocco all'adattatore.

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

Facoltativamente, puoi ignorare l'adattatore e contattare direttamente l'endpoint REST.

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### Origine dati esterna: endpoint REST di backend 
{: #external-data-source-backend-rest-endpoint }
Il backend accetta o rifiuta le modifiche, quindi ritrasmette una risposta al client. Dopo che il client ha esaminato la risposta, può passare i documenti che sono stati aggiornati all'API markClean.

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

Dopo che i documenti sono stati contrassegnati come puliti, non vengono visualizzati nell'output dall'API `getAllDirty`.

## Risoluzione dei problemi JSONStore
{: #troubleshooting-jsonstore }

## Fornisci informazioni quando richiedi assistenza
{: #provide-information-when-you-ask-for-help }
È meglio fornire più informazioni che rischiare di non fornire quelle sufficienti. Il seguente elenco è un buon punto di partenza per le informazioni necessarie per la risoluzione dei problemi di JSONStore.

* Sistema operativo e versione. Ad esempio, Windows XP SP3 Virtual Machine o Mac OSX 10.8.3.
* Versione Eclipse. Ad esempio, Eclipse Indigo 3.7 Java EE.
* Versione JDK. Ad esempio, Java SE Runtime Environment (build 1.7).
* Versione {{ site.data.keys.product }}. Ad esempio, IBM Worklight V5.0.6 Developer Edition.
* Versione iOS. Ad esempio, iOS Simulator 6.1 o iPhone 4S iOS 6.0 (obsoleto, vedi Funzioni ed elementi API obsoleti).
* Versione Android. Ad esempio, Android Emulator 4.1.1 o Samsung Galaxy Android 4.0, livello API 14.
* Versione Windows. Ad esempio, Windows 8, Windows 8.1 o Windows Phone 8.1.
* Versione adb. Ad esempio, Android Debug Bridge versione 1.0.31.
* Log, come l'output Xcode su iOS o l'output logcat su Android.

## Prova a isolare il problema
{: #try-to-isolate-the-issue }
Segui questa procedura per isolare il problema per poterlo segnalare in modo più accurato.

1. Reimposta l'emulatore (Android) o il simulatore (iOS) e chiama l'API di eliminazione permanente (destroy) per iniziare con un sistema pulito.
2. Assicurati di essere in esecuzione su un ambiente di produzione supportato.
    * Android >= simulatore o dispositivo 2.3 ARM v7/ARM v8/x86
    * iOS >= simulatore o dispositivo 6.0 (obsoleto)
    * Simulatore o dispositivo Windows 8.1/10 ARM/x86/x64
3. Prova a disattivare la crittografia non passando una password alle API init od open.
4. Controlla il file di database SQLite generato da JSONStore. La crittografia deve essere disattivata.

   * Emulatore Android:

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * Simulatore iOS:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Simulatore Windows 8.1 Universal / Windows 10 UWP

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

    L'implementazione solo JavaScript eseguita su un browser web (Firefox, Chrome, Safari, Internet Explorer) non utilizza un database SQLite. Il file è archiviato in HTML5 LocalStorage.
    {: note}
   * Esamina i `searchFields` con `.schema` e seleziona i dati con `SELECT * FROM <collection-name>;`. Per uscire da sqlite3, immetti `.exit`. Se passi un nome utente al metodo init, il file viene denominato **the-username.sqlite**. Se non passi un nome utente, il file è denominato **jsonstore.sqlite** per impostazione predefinita.
5. (Solo Android) Abilita i dettagli di JSONStore.

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```
    {: codeblock}

6. Utilizza il programma di debug.

## Problemi comuni
{: #common-issues-jsonstore }
Comprendere le seguenti caratteristiche di JSONStore può aiutare a risolvere alcuni dei problemi comuni che potresti incontrare.  

* L'unico modo per archiviare i dati binari in JSONStore è codificarli prima in base64. Archivia i nomi o i percorsi dei file invece dei file effettivi in JSONStore.
* L'accesso ai dati JSONStore dal codice nativo è possibile solo in {{ site.data.keys.v62_product_full }} V6.2.0.
* Non c'è limite alla quantità di dati che puoi archiviare all'interno di JSONStore, oltre i limiti imposti dal sistema operativo mobile.
* JSONStore fornisce l'archiviazione dei dati persistente. Non vengono archiviati solo in memoria.
* L'API init on riesce quando il nome della raccolta inizia con una cifra o un simbolo. IBM Worklight V5.0.6.1 e successive restituisce un errore appropriato: `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* C'è una differenza tra un numero e un intero nei campi di ricerca. I valori numerici come `1` e `2` vengono archiviati come `1.0` e `2.0` quando il tipo è `number`. Vengono archiviati come `1` e `2` quando il tipo è `integer`.
* Se un'applicazione si interrompe o ne viene forzato l'arresto, si blocca sempre con il codice di errore -1 quando l'applicazione viene riavviata e viene chiamata l'API `init` o `open`. Se si verifica questo problema, chiama prima l'API `closeAll`.
* L'implementazione JavaScript di JSONStore prevede che il codice venga chiamato in serie. Attendi il completamento di un'operazione prima di chiamare quella successiva.
* Le transazioni non sono supportate in Android 2.3.x per le applicazioni Cordova.
* Se utilizzi JSONStore su un dispositivo a 64 bit, potresti vedere il seguente errore: `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* Questo errore significa che hai librerie native a 64 bit nel tuo progetto Android e JSONStore non funziona attualmente quando usi queste librerie. Per confermare, vai in **src/main/libs** o **src/main/jniLibs** sotto il tuo progetto Android e controlla se hai le cartelle x86_64 o arm64-v8a. Se le hai, elimina queste cartelle in modo che JSONStore possa funzionare di nuovo.
* In alcuni casi (o ambienti), il flusso immette `wlCommonInit()` prima che il plug-in JSONStore venga inizializzato. Ciò provoca l'esito negativo delle chiamate API relative a JSONStore. Il bootstrap `cordova-plugin-mfp` chiama automaticamente `WL.Client.init` che attiva la funzione `wlCommonInit` quando viene completata. Questo processo di inizializzazione è diverso per il plug-in JSONStore. Il plug-in JSONStore non ha un modo per _arrestare_ la chiamata `WL.Client.init`. Nei diversi ambienti, può accadere che il flusso immetta `wlCommonInit()` prima del completamento di `mfpjsonjslloaded`.
Per garantire l'ordine degli eventi `mfpjsonjsloaded` e `mfpjsloaded`, lo sviluppatore ha la possibilità di chiamare `WL.CLient.init`
manualmente. Ciò eliminerà la necessità di avere un codice specifico per la piattaforma.

  Utilizza la seguente procedura per configurare manualmente la chiamata di `WL.CLient.init`:                             

  1. In `config.xml`, modifica la proprietà `clientCustomInit` su **true**.

  + Nel file `index.js`:                                   
    * aggiungi la seguente riga all'inizio del file:                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * lascia la chiamata `WL.JSONStore.init` in `wlCommonInit()`                    

    * aggiungi la seguente funzione:  
    ```javascript                                         
    function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }
    ```                                                                     

Questo attenderà l'evento `mfpjsonjsloaded` (al di fuori di `wlCommonInit`), che garantirà che lo script sia stato caricato e successivamente richiamerà `WL.Client.init` che attiverà `wlCommonInit` e che quindi richiamerà `WL.JSONStore.init`.

## Risorse interne di archiviazione
{: #store-internals }
Vedi un esempio di come vengono archiviati i dati JSONStore.

Gli elementi chiave in questo esempio semplificato sono:

* `_id` è l'identificativo univoco (ad esempio, AUTO INCREMENT PRIMARY KEY).
* `json` contiene una rappresentazione esatta dell'oggetto JSON archiviato.
* `name` ed age sono campi di ricerca.
* `key` è un campo di ricerca aggiuntivo.

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |
{: caption="Tabella 2. Esempio di elementi chiave di dati JSONStore" caption-side="top"}

Quando effettui una ricerca utilizzando una delle seguenti query o una combinazione di esse: `{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}`, il documento restituito è `{_id: 1, json: {name: 'carlos', age: 99} }`.

Gli altri campi interni di JSONStore sono:

* `_dirty`: determina se il documento è stato contrassegnato come dirty o meno. Questo campo è utile per tenere traccia delle modifiche ai documenti.
* `_deleted`: contrassegna un documento come eliminato o meno. Questo campo è utile per rimuovere oggetti dalla raccolta, per utilizzarli in seguito per tenere traccia delle modifiche con il tuo backend e decidere se rimuoverli o meno.
* `_operation`: una stringa che riflette l'ultima operazione da eseguire sul documento (ad esempio, replace).

## Errori JSONStore
{: #jsonstore-errors }
### Errori JavaScript
{: #javascript-errors }
JSONStore utilizza un oggetto errore per restituire messaggi sulla causa degli errori.

Quando si verifica un errore durante un'operazione di JSONStore (ad esempio, i metodi `find` e `add` nella classe `JSONStoreInstance`), viene restituito un oggetto errore. Questo fornisce informazioni sulla causa dell'errore.

```javascript
var errorObject = {
  src: 'find', // Operation that failed.
  err: -50, // Error code.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Error message.
  col: 'people', // Collection name.
  usr: 'jsonstore', // User name.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Document that is related to the failure.
  res: {...} // Response from the server.
}
```
{: codeblock}
Non tutte le coppie chiave/valore fanno parte di ogni oggetto errore. Ad esempio, il valore doc è disponibile solo quando l'operazione non è riuscita a causa di un documento (ad esempio, il metodo `remove` nella classe `JSONStoreInstance` non è riuscito a rimuovere un documento).

### Errori Objective-C
{: #objective-c-errors }
Tutte le API che potrebbero non riuscire prendono un parametro di errore che prende un indirizzo a un oggetto NSError. Se non vuoi essere avvisato sugli errori, puoi passare `nil`. Quando un'operazione non riesce, l'indirizzo viene popolato con un NSError, che ha un errore e delle potenziali `userInfo`. Le `userInfo` potrebbero contenere ulteriori dettagli (ad esempio, il documento che ha causato l'errore).

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Errori Java
{: #java-errors }
Tutte le chiamate API Java generano una determinata eccezione, a seconda dell'errore che si è verificato. Puoi gestire ogni eccezione separatamente o puoi rilevare `JSONStoreException` come un modo per rilevare tutte le eccezioni di JSONStore.

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### Elenco dei codici di errore
{: #list-of-error-codes }
Elenco dei codici di errore comuni e loro descrizione:

|Codice di errore      | Descrizione |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Errore non riconosciuto. |
| -75 OS\_SECURITY\_FAILURE | Questo codice di errore è correlato all'indicatore requireOperatingSystemSecurity. Si può verificare se l'API di eliminazione permanente (destroy) non può rimuovere i metadati di sicurezza protetti dalla protezione del sistema operativo (Touch ID con fallback del passcode) o le API init e open non possono individuare i metadati di sicurezza. Può avere esito negativo anche se il dispositivo non supporta la protezione del sistema operativo, ma ne è stato richiesto l'utilizzo. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore è chiuso. Tenta di richiamare il metodo open nella prima classe JSONStore per abilitare l'accesso all'archivio. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | Si è verificato un problema con il rollback della transazione. |
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |Impossibile richiamare removeCollection mentre una transazione è in corso. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | Impossibile richiamare destroy mentre ci sono delle transazioni in corso. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | Impossibile richiamare closeAll mentre ci sono delle transazioni attive. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | Impossibile inizializzare un archivio mentre ci sono delle transazioni in corso. |
| -43 TRANSACTION_FAILURE | Si è verificato un problema con le transazioni. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | Impossibile eseguire il commit per eseguire il rollback di una transazione quando non ci sono delle transazioni in corso |
| -41 TRANSACTION\_IN\_POGRESS | Impossibile avviare una nuova transazione mentre ne è in corso un'altra. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Qualcosa non ha funzionato con FIPS. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Problema durante l'ottenimento delle informazioni sul file dal file system. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Problema durante la sostituzione dei documenti da una raccolta. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Problema durante la rimozione dei documenti da una raccolta. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Problema durante l'archiviazione della chiave di protezione dei dati (DPK). |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Problema durante l'indicizzazione dei dati di input. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Controlla che i tipi che stai trasmettendo a searchFields siano string, integer, number o boolean. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | Un'operazione su un array di documenti, ad esempio il metodo replace può avere esito negativo mentre funziona con un documento specifico. Il documento con il malfunzionamento viene restituito e viene eseguito il rollback della transazione. Su Android, questo errore si verifica anche quando tenti di utilizzare JSONStore su architetture non supportate. |
| -10 ACCEPT\_CONDITION\_FAILED | La funzione accept che l'utente ha fornito ha restituito false. |
| -9 OFFSET\_WITHOUT\_LIMIT | Per utilizzare offset, devi specificare anche un limite. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Errore di convalida, deve essere un numero intero positivo. |
| -7 INVALID_USERNAME | Errore di convalida (deve essere solo [A-Z] o [a-z] o [0-9]). |
| -6 USERNAME\_MISMATCH\_DETECTED | Per scollegarsi, un utente JSONStore deve prima richiamare il metodo closeAll. Può esserci soltanto un utente alla volta. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |Un problema con il metodo destroy mentre si tentava di eliminare il file che ospita i contenuti dell'archivio. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Problema con il metodo destroy mentre si tentava di eliminare il keychain (iOS) o le preferenze utente condivise (Android). |
| -3 INVALID\_KEY\_ON\_PROVISION | È stata trasmessa una password non corretta a un archivio crittografato. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | I campi di ricerca non sono dinamici. Non è possibile modificare i campi di ricerca senza richiamare il metodo destroy o removeCollection prima di richiamare init o openmethod con i nuovi campi di ricerca. Questo errore si verifica se modifichi il nome o il tipo del campo di ricerca. Ad esempio: {key: 'string'} con {key: 'number'} o {myKey: 'string'} con {theKey: 'string'}. |
| -1 PERSISTENT\_STORE\_FAILURE | Errore generico. Un malfunzionamento nel codice nativo, molto probabilmente richiamando il metodo init. |
| 0 SUCCESS | In alcuni casi, il codice nativo di JSONStore restituisce 0 per indicare il corretto esito dell'operazione. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | Errore di convalida. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | Errore di convalida. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | Errore di convalida. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | Errore di convalida. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | Errore di convalida. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | Errore di convalida. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | Errore di convalida. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |La query che seleziona tutti i documenti contrassegnati come dirty non è riuscita. Un esempio in SQL della query sarebbe: SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | Per utilizzare funzioni come i metodi push e load nella classe JSONStoreCollection, è necessario passare un adattatore al metodo init. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | Errore di convalida |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | Errore di convalida |
| 12 ADAPTER_FAILURE | Problema durante la chiamata di WL.Client.invokeProcedure, nello specifico un problema di connessione all'adattatore. Questo errore è diverso da un errore nell'adattatore che tenta di chiamare un back-end. | |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | Errore di convalida |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | Non è consentita la chiamata del metodo enhance nella classe JSONStoreCollection per sostituire una funzione esistente (find e add). |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Push invia il documento a un adattatore ma JSONStore non riesce a contrassegnare il documento come non dirty. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | Per avviare una raccolta con una password deve esserci connettività a {{ site.data.keys.mf_server }} perché restituisce un 'token casuale sicuro'. IBM  Worklight  V5.0.6 e versioni successive consente agli sviluppatori di generare il token casuale sicuro localmente passando {localKeyGen: true} al metodo init tramite l'oggetto di opzioni. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | Non è stato possibile caricare i dati perché WL.Client.invokeProcedure ha chiamato il callback di malfunzionamento. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | L'oggetto di caricamento passato al metodo init non ha superato la convalida. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | C'è un problema con la chiave utilizzata nell'oggetto di caricamento quando chiami il metodo add. |
| 20 UNDEFINED\_PUSH\_OPERATION | Nessuna procedura definita per eseguire il push di documenti dirty al server. Ad esempio: sono stati chiamati il metodo init (il nuovo documento è dirty, operazione = 'add') e il metodo push (trova il nuovo documento con l'operazione = 'add'), ma non è stata trovata alcuna chiave di aggiunta con la procedura di aggiunta nell'adattatore collegato alla raccolta. Il collegamento di un adattatore avviene all'interno del metodo init. |
| 21 INVALID\_ADD\_INDEX\_KEY | Problema con i campi di ricerca aggiuntivi. |
| 22 INVALID\_SEARCH\_FIELD | Uno dei tuoi campi di ricerca non è valido. Verifica che nessuno dei campi di ricerca passati sia _id,json,_deleted o _operation. |
| 23 ERROR\_CLOSING\_ALL | Errore generico. Si è verificato un errore quando il codice nativo ha chiamato il metodo closeAll. |
| 24 ERROR\_CHANGING\_PASSWORD | Impossibile modificare la password. Ad esempio, la vecchia password passata era sbagliata. |
| 25 ERROR\_DURING\_DESTROY | Errore generico. Si è verificato un errore quando il codice nativo ha chiamato il metodo destroy. |
| 26 ERROR\_CLEARING\_COLLECTION | Errore generico. Si è verificato un errore quando il codice nativo ha chiamato il metodo removeCollection . |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | Errore di convalida. |
| 28 INVALID\_SORT\_OBJECT | L'array fornito per l'ordinamento non è valido perché uno degli oggetti JSON non è valido. La sintassi corretta è un array di oggetti JSON, in cui ogni oggetto contiene solo una singola proprietà. Questa proprietà cerca il campo con il quale ordinare e se è ascendente o discendente. Ad esempio: {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | L'array fornito per filtrare i risultati non è valido. La sintassi corretta per questo array è un array di stringhe, in cui ogni stringa è un campo di ricerca o un campo interno di JSONStore. Per ulteriori informazioni, vedi Risorse interne di archiviazione. |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | Errore di convalida quando l'array non è un array di soli oggetti JSON. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | Errore di convalida. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | Errore di convalida. |
{: caption="Tabella 3. Codici di errore comuni" caption-side="top"}
