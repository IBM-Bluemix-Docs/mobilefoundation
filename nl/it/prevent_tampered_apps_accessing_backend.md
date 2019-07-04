---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, restrict backend access, tampered apps

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

# Evita che le applicazioni manomesse accedano al backend
{: #prevent_tampered_apps_accessing_backend}

L'autenticità dell'applicazione aiuta a controllarne la validità prima di abilitare eventuali servizi, evitando così che un'applicazione manomessa acceda ai servizi di backend.
{: shortdesc}

Per proteggere adeguatamente la tua applicazione, abilita il controllo di sicurezza dell'autenticità dell'applicazione di MobileFirst predefinito (``appAuthenticity``). Quando è abilitato, questo controllo convalida l'autenticità dell'applicazione prima di fornirle qualsiasi servizio. Le applicazioni nell'ambiente di produzione devono avere questa funzione abilitata.

Per abilitare l'autenticità dell'applicazione, puoi attenerti alle istruzioni a schermo in **MobileFirst Operations Console → [la-tua-applicazione] → Authenticity** o riesaminare le seguenti informazioni.

<dl>
  <dt>Disponibilità</dt>
  <dd>L'autenticità dell'applicazione è disponibile in tutte le piattaforme supportate (iOS, Android, Windows 8.1 Universal, Windows 10 UWP) sia nelle applicazioni Cordova sia in quelle native.</dd>
  <dt>Limitazioni</dt>
  <dd>L'autenticità dell'applicazione non supporta **Bitcode** in iOS. Pertanto, prima di utilizzare l'autenticità dell'applicazione, disabilita Bitcode nelle proprietà del progetto Xcode.</dd>
</dl>

## Flusso di autenticità dell'applicazione
{: #appauthenticityflow}

Il controllo di sicurezza dell'autenticità dell'applicazione viene eseguito durante la registrazione dell'applicazione a MobileFirst Server, che si verifica la prima volta che un'istanza dell'applicazione prova a stabilire una connessione al server. Per impostazione predefinita, il controllo dell'autenticità non viene eseguito nuovamente.

Dopo che l'autenticità dell'applicazione è stata abilitata, e se il cliente deve introdurre eventuali modifiche nella sua applicazione, è necessario eseguire un upgrade della versione dell'applicazione.

Vedi [Configurazione dell'autenticità dell'applicazione](#configappauthenticity) per informazioni su come personalizzare questa modalità di funzionamento.

### Abilitazione dell'autenticità dell'applicazione
{: #enableappauthenticity}

Per fare in modo che l'autenticità dell'applicazione sia abilitata nella tua applicazione:

1. Apri la MobileFirst Operations Console nel tuo browser preferito.
2. Seleziona la tua applicazione dalla barra laterale di navigazione e fai clic sulla voce di menu **Authenticity**.
3. Usa il pulsante **On/Off** nella casella **Status** per impostarla su On.

![Abilitazione dell'autenticità dell'applicazione](/images/enable_application_authenticity.png)

MobileFirst Server convalida l'autenticità dell'applicazione al primo tentativo di connessione al server. Per applicare questa convalida anche alle risorse protette, aggiungi il controllo di sicurezza appAuthenticity all'ambito di protezione.

### Disabilitazione dell'autenticità dell'applicazione
{: #disableappauthenticity}

Alcune modifiche all'applicazione durante lo sviluppo potrebbero causarne la mancata riuscita della convalida dell'autenticità. Di conseguenza, si consiglia di disabilitare l'autenticità dell'applicazione durante il processo di sviluppo. Le applicazioni nell'ambiente di produzione devono avere questa funzione abilitata.

Per disabilitare l'autenticità dell'applicazione, usa il pulsante **On/Off** nella casella **Status** per impostarla su Off.

## Configurazione dell'autenticità dell'applicazione
{: #configappauthenticity}

Per impostazione predefinita, l'autenticità dell'applicazione viene controllata solo durante la registrazione del client. Tuttavia, proprio come qualsiasi altro controllo di sicurezza, puoi decidere di proteggere la tua applicazione o le tue risorse con il controllo di sicurezza ``appAuthenticity`` dalla console, attenendoti alle istruzioni che trovi in Protezione delle risorse.

Puoi configurare il controllo di sicurezza di autenticità dell'applicazione predefinito con la seguente proprietà:

* ``expirationSec``: ha come valore predefinito 3600 secondi / 1 ora. Definisce la durata fino alla scadenza del token di autenticità.

Dopo che un controllo di autenticità è stato completato, non si verifica di nuovo fino a quando il token non è scaduto in base al valore impostato.

Per configurare la proprietà ``expirationSec``:

1. Carica la MobileFirst Operations Console, vai a **[la tua applicazione] → Security → Security-Check Configurations** e fai clic su **New**.
2. Cerca l'elemento di ambito ``appAuthenticity``.
3. Imposta un nuovo valore in secondi.

    ![Configurazione della scadenza in numero di secondi](/images/configuring_expirationSec.png)

## BTS (Build Time Secret)
{: #buildtimesecret}

Il BTS (Build Time Secret) è uno **strumento facoltativo per migliorare la convalida dell'autenticità**, solo per le applicazioni iOS. Lo strumento inserisce l'applicazione con un segreto determinato al momento della creazione, che viene successivamente utilizzato nel processo di convalida dell'autenticità.

Lo strumento BTS può essere scaricato da **MobileFirst Operations Console → Download Center**.

Per utilizzare lo strumento BTS in Xcode:

1. Nella scheda **Build Phases**, fai clic sul pulsante **+** e crea una nuova **Run Script Phase**.
2. Copia il percorso dello strumento BTS e incollalo nella nuova “Run Script Phase” che hai creato.
3. Trascina la fase di script di esecuzione prima di **Compile sources phase**.

Lo strumento deve essere utilizzato quando si crea una versione di produzione dell'applicazione.

## Risoluzione dei problemi con l'autenticità dell'applicazione
{: #troubleshooting-app-authenticity}

### Reimpostazione
{: #trblreset}

L'algoritmo di autenticità dell'applicazione utilizza i dati e i metadati dell'applicazione nella sua convalida. Il primo dispositivo che stabilisce una connessione al server dopo l'abilitazione dell'autenticità dell'applicazione fornisce una “impronta digitale” dell'applicazione, che contiene alcuni di questi dati.

È possibile reimpostare questa impronta digitale, fornendo all'algoritmo dei nuovi dati. Ciò potrebbe essere utile durante lo sviluppo (ad esempio dopo la modifica dell'applicazione in Xcode). Per reimpostare l'impronta digitale, usa il comando **reset** dalla CLI mfpadm.

Dopo la reimpostazione dell'impronta digitale, il controllo di sicurezza appAuthenticity continua a funzionare come prima (ciò sarà trasparente all'utente).

### Tipi di convalida
{: #trblvalidationtypes}

Mobile First Platform Foundation fornisce l'autenticità dell'applicazione statica e dinamica per le applicazioni. Questi tipi di convalida differiscono per l'algoritmo e gli attributi utilizzati per generare i valori di inizializzazione dell'autenticità dell'applicazione. Per impostazione predefinita, quando è abilitata, l'autenticità dell'applicazione utilizza l'algoritmo di convalida dinamico. Entrambi i tipi di convalida garantiscono la sicurezza dell'applicazione. L'autenticità dell'applicazione dinamica utilizza delle rigide convalide e controlla l'autenticità dell'applicazione. Per l'autenticità dell'applicazione statica, utilizziamo un algoritmo leggermente rilassato, che non utilizzerà tutti i controlli di convalida utilizzati nell'autenticità dell'applicazione dinamica.

L'autenticità dell'applicazione dinamica è configurabile da MobileFirst Console. L'algoritmo interno si occupa della generazione dei dati di autenticità dell'applicazione sulla base delle opzioni scelte nella console. Per l'autenticità dell'applicazione statica, bisogna utilizzare la CLI mfpadm.

Per abilitare l'autenticità dell'applicazione statica e per passare dall'uno all'altro tipo di convalida, utilizza la CLI mfpadm:

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE può essere dynamic o static.
