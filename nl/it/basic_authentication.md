---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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


# Autenticazione e sicurezza
{: #basic_authentication}

Il framework di sicurezza MobileFirst è basato sul protocollo [OAuth 2.0](http://oauth.net/). In base a questo protocollo, una risorsa può essere protetta da un **ambito** che definisce le autorizzazioni richieste per accedere alla risorsa. Per accedere a una risorsa protetta, il client deve fornire un **token di accesso** corrispondente che racchiude l'ambito dell'autenticazione concessa al client.

Il protocollo OAuth separa i ruoli del server di autorizzazione e del server di risorse su cui è ospitata la risorsa.

* Il server di autorizzazione gestisce l'autorizzazione dei client e la generazione dei token.
* Il server di risorse utilizza il server di autorizzazione per convalidare il token di accesso fornito dal client e garantisce che corrisponda all'ambito di protezione della risorsa richiesta.

Il framework di sicurezza è sviluppato intorno a un server di autorizzazione che implementa il protocollo OAuth ed espone gli endpoint OAuth con cui il client interagisce per ottenere i token di accesso. Il framework di sicurezza fornisce i blocchi di creazione per l'implementazione di una logica di autorizzazione personalizzata in aggiunta al server di autorizzazione e al sottostante protocollo OAuth. Per impostazione predefinita, MobileFirst Server funge anche da **server di autorizzazione**. Puoi tuttavia configurare un'applicazione IBM WebSphere DataPower perché funga da server di autorizzazione e interagisca con MobileFirst Server.

L'applicazione client può quindi utilizzare questi token per accedere alle risorse su un **server di risorse**, che può essere MobileFirst Server stesso oppure un server esterno. Il server di risorse controlla la validità del token per accertarsi che al client possa essere concesso l'accesso alla risorsa richiesta. La separazione tra server di risorse e server di autorizzazione implementa la sicurezza sulle risorse in esecuzione al di fuori di MobileFirst Server.

Gli sviluppatori di applicazioni proteggono l'accesso alle loro risorse definendo l'ambito richiesto per ogni risorsa protetta e implementando i controlli di sicurezza e i gestori delle verifiche. Il framework di sicurezza lato server e l'API lato client gestiscono lo scambio di messaggi OAuth e l'interazione con il server di autorizzazione in modo trasparente, consentendo agli sviluppatori di concentrarsi solo sulla logica di autorizzazione.

## Entità di autorizzazione
{: #acs_authorization_entitiesty}

### Token di accesso
{: #acs_access_tokens}

Un token di accesso MobileFirst è un'entità firmata digitalmente che descrive i permessi di autorizzazione di un client. Dopo che la richiesta di autorizzazione del client per un ambito specifico è stata concessa e il client è stato autenticato, l'endpoint del token del server di autorizzazione invia al client una risposta HTTP che contiene il token di accesso richiesto.

#### Struttura del token di accesso

Il token di accesso MobileFirst contiene le seguenti informazioni:

* **ID client**: un identificativo univoco del client.
* **Ambito**: l'ambito per il quale il token è stato concesso (vedi gli ambiti OAuth). Questo ambito non include l'ambito dell'applicazione obbligatorio.
* **Tempo di scadenza del token**: il tempo dopo il quale il token diventa non valido (scade), in secondi.

#### Scadenza del token
{: #token-expiration}

Il token di accesso concesso rimane valido finché non trascorre il suo tempo di scadenza. Il tempo di scadenza del token di accesso è impostato sul tempo di scadenza più breve tra i tempi di scadenza di tutti i controlli di sicurezza nell'ambito. Se però il periodo fino al tempo di scadenza più breve è più lungo del periodo di scadenza del token massimo dell'applicazione, il tempo di scadenza del token viene impostato sul tempo corrente più il periodo di scadenza massimo. Il periodo di scadenza del token massimo predefinito (durata di validità) è 3.600 secondi (1 ora) ma può essere configurato impostando il valore della proprietà ``maxTokenExpiration``.

##### Configurazione del periodo di scadenza del token di accesso massimo 
{: #acs_config-max-access-tokens}

Configura il periodo di scadenza del token di accesso massimo dell'applicazione utilizzando uno dei seguenti metodi alternativi:

* Utilizzando la MobileFirst Operations Console
    1. Seleziona la scheda **[la tua applicazione]** → **Security**.
    2. Nella sezione **Token Configuration**, imposta il valore del campo Maximum **Token-Expiration Period (seconds)** sul tuo valore preferito e fai clic su **Save**. Puoi ripetere questa procedura in qualsiasi momento per modificare il periodo di scadenza del token massimo o selezionare **Restore Default Values** per ripristinare il valore predefinito.

* Modifica del file di configurazione dell'applicazione

    1. Da una CLI, vai alla cartella root del progetto ed esegui il seguente comando.
      ```bash
      mfpdev app pull
      ```
    2. Apri il file di configurazione, che si trova nella cartella `[cartella-progetto]\mobilefirst`.
    3. Modifica il file definendo una proprietà `maxTokenExpiration` e impostane il valore sul periodo di scadenza del token di accesso massimo, in secondi:
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. Distribuisci il file JSON di configurazione aggiornato eseguendo il comando: ``mfpdev app push``.

##### Struttura della risposta del token di accesso 
{: #acs_access-tokens-structure}

Una risposta HTTP con esito positivo a una richiesta di token di accesso contiene un oggetto JSON con il token di accesso e i dati supplementari. Il seguente è un esempio di una risposta del token di accesso valida dal server di autorizzazione:

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

L'oggetto JSON di risposta del token ha questi oggetti di proprietà.

* **token_type**: il tipo di token è sempre "*Bearer*", nel rispetto della [specifica OAuth 2.0 Bearer Token Usage](https://tools.ietf.org/html/rfc6750).
* **expires_in**: il tempo di scadenza del token di accesso, in secondi.
* **access_token**: il token di accesso generato (i token di accesso effettivi sono più lunghi rispetto a quelli mostrati nell'esempio).
* **scope**: l'ambito richiesto.

Le informazioni **expires_in** e **scope** sono contenute anche nel token stesso (**access_token**).

**Nota**: la struttura di una risposta del token di accesso valida è pertinente se usi la classe `WLAuthorizationManager` di basso livello e gestisci l'interazione OAuth tra il client e i server di autorizzazione e di risorse personalmente oppure se utilizzi un client riservato. Se stai utilizzando la classe `WLResourceRequest` di alto livello, che racchiude il flusso OAuth per l'accesso alle risorse protette, il framework di sicurezza gestisce l'elaborazione delle risposte del token di accesso per tuo conto. Vedi [Client security APIs](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) e [Confidential Clients](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).

### Token di aggiornamento
{: #acs_refresh_tokens}

Un token di aggiornamento è un tipo di token speciale che può essere utilizzato per ottenere un nuovo token di accesso quando il token di accesso scade. Per richiedere un nuovo token di accesso, può essere presentato un token di aggiornamento valido. I token di aggiornamento sono token a lunga durata e rimangono validi per un periodo di tempo più lungo rispetto ai token di accesso.

Il token di aggiornamento deve essere utilizzato con cautela da un'applicazione perché può consentire a un utente di rimanere autenticato per sempre. Le applicazioni social media, le applicazioni di e-commerce, l'esplorazione del catalogo del prodotto e le applicazioni di programma di utilità di questo tipo, dove il provider dell'applicazione non autentica gli utenti regolarmente, possono utilizzare i token di aggiornamento. Le applicazioni che impongono frequentemente l'autenticazione degli utenti devono evitare l'uso di token di aggiornamento.
Token di aggiornamento MobileFirst

Un token di aggiornamento MobileFirst è un'entità firmata digitalmente come un token di accesso che descrive i permessi di autorizzazione di un client. Il token di aggiornamento può essere utilizzato per ottenere un nuovo token di accesso dello stesso ambito. Dopo che la richiesta di autorizzazione del client per un ambito specifico è stata concessa e il client è stato autenticato, l'endpoint del token del server di autorizzazione invia al client una risposta HTTP che contiene il token di accesso e quello di aggiornamento richiesti. Quando il token di accesso scade, il client invia il token di aggiornamento all'endpoint del token del server di autorizzazione per ottenere una nuova serie di token di accesso e token di aggiornamento.

#### Struttura del token di aggiornamento
{: #structure_refresh_tokens}

Simile al token di accesso MobileFirst, il token di aggiornamento MobileFirst contiene le seguenti informazioni:

* **ID client**: un identificativo univoco del client.
* **Ambito**: l'ambito per il quale il token è stato concesso (vedi gli ambiti OAuth). Questo ambito non include l'ambito dell'applicazione obbligatorio.
* **Tempo di scadenza del token**: il tempo dopo il quale il token diventa non valido (scade), in secondi.

##### Scadenza del token
{: #str-token-expiration}

Il periodo di scadenza del token per il token di aggiornamento è più lungo del tipico periodo di scadenza del token di accesso. Il token di aggiornamento, una volta concesso, rimane valido finché non trascorre il suo tempo di scadenza. Entro questo periodo di validità, un client può utilizzare il token di aggiornamento per ottenere una nuova serie di token di accesso e token di aggiornamento. Il token di aggiornamento ha un periodo di scadenza fisso di 30 giorni. Ogni volta che il client riceve correttamente una nuova serie di token di accesso e token di aggiornamento, la scadenza del token di aggiornamento viene reimpostata, dando così al client un'esperienza di un token che non scade mai. Le regole di scadenza del token di accesso rimangono identiche a quanto spiegato nella sezione **Token di accesso**.

##### Abilitazione della funzione di token di aggiornamento 
{: #acs_enable-refresh-token}

La funzione di token di aggiornamento può essere abilitata utilizzando le seguenti proprietà rispettivamente sul lato client e sul lato server.

Proprietà lato client (Android):
*Nome file*: mfpclient.properties
*Nome proprietà*: wlEnableRefreshToken
*Valore proprietà*: true
Ad esempio,
*wlEnableRefreshToken*=true

Proprietà lato client (iOS):
*Nome file*: mfpclient.plist
*Nome proprietà*: wlEnableRefreshToken
*Valore proprietà*: true
Ad esempio,
*wlEnableRefreshToken*=true

Proprietà lato server:
*Nome file*: server.xml
*Nome proprietà*: mfp.security.refreshtoken.enabled.apps
*Valore proprietà*: application bundle id separated by ‘;’

Ad esempio,

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
Utilizza ID bundle differenti per le differenti piattaforme.

##### Struttura della risposta del token di aggiornamento 
{: #refresh-token-response-structure}

Il seguente è un esempio di una risposta del token di aggiornamento valida dal server di autorizzazione:

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

La risposta del token di aggiornamento ha un oggetto di proprietà aggiuntivo `refresh_token` oltre agli altri oggetti di proprietà spiegati come parte della struttura della risposta del token di accesso.

**Nota**: i token di aggiornamento sono di lunga durata rispetto ai token di accesso. Pertanto, la funzione di token di aggiornamento deve essere utilizzata con cautela. Le applicazioni in cui l'autenticazione utente periodica non è necessaria sono i candidati ideali per l'utilizzo della funzione di token di aggiornamento.

MobileFirst supporta la funzione di token di aggiornamento su iOS a partire da CD Update 3.

#### Controlli di sicurezza
{: #acs_securitychecks}

Un controllo di sicurezza è un'entità lato server che implementa la logica di sicurezza per proteggere le risorse dell'applicazione lato server. Un semplice esempio di un controllo di sicurezza è un controllo di sicurezza dell'accesso utente che riceve le credenziali di un utente e verifica le credenziali rispetto a un registro utenti. Un altro esempio è il controllo di sicurezza di autenticità dell'applicazione MobileFirst predefinito, che convalida l'autenticità dell'applicazione mobile e protegge pertanto da tentativi illegittimi di accesso alle risorse dell'applicazione. Lo stesso controllo di sicurezza può anche essere utilizzato per proteggere diverse risorse.

Un controllo di sicurezza di norma emette verifiche di sicurezza che richiedono che il client risponda in un modo specifico per superare il controllo. Questo handshake si verifica come parte del flusso di acquisizione del token di accesso OAuth. Il client utilizza i **gestori delle verifiche** per gestire le verifiche dai controlli di sicurezza.

##### Controlli di sicurezza integrati 
{: #builtin-sec-checks}

Sono disponibili i seguenti controlli di sicurezza predefiniti:

* Autenticità dell'applicazione
* SSO (single sign-on) basato su LTPA
* Aggiornamento diretto

#### Entità gestore delle verifiche
{: #challengehandler_entity}

Quando provi ad accedere a una risorsa protetta, al client potrebbe essere presentata una verifica. Una verifica è una domanda, un test di sicurezza, una richiesta da parte del server per garantire che al client sia consentito l'accesso a questa risorsa. Più comunemente, questa verifica è una richiesta di credenziali, come ad esempio un nome utente e una password.

Un gestore delle verifiche è un'entità lato client che implementa la logica di sicurezza lato client e l'interazione utente correlata.

**Importante**: dopo essere stata ricevuta, una verifica non può essere ignorata. Devi rispondere o annullarla. Ignorare una verifica può causare una modalità di funzionamento imprevista.

### Ambiti
{: #scopes}

Puoi proteggere le risorse, come ad esempio gli adattatori, da un accesso non autorizzato assegnando un ambito alla risorsa.

Un ambito è definito come una stringa di uno o più elementi di ambito separati da spazi (“scopeElement1 scopeElement2 …”) o null per applicare l'ambito predefinito (RegisteredClient). Il framework di sicurezza MobileFirst richiede un token di accesso per qualsiasi risorsa dell'adattatore, anche se alla risorsa non è assegnato un ambito, a meno che non disabiliti la protezione della risorsa per la risorsa.

#### Elementi di ambito
{: #scopeelements}

Un elemento di ambito può essere uno dei seguenti:

* Il nome di un controllo di sicurezza.
* Una parola chiave arbitraria, come ad esempio `access-restricted` o `deletePrivilege`, che definisce il livello di sicurezza necessario per questa risorsa. Questa parola chiave viene successivamente associata a un controllo di sicurezza.

#### Associazione dell'ambito
{: #scopemapping}

Per impostazione predefinita, gli **elementi di ambito** che scrivi nel tuo **ambito** sono associati a un **controllo di sicurezza** con lo stesso nome. Ad esempio, se scrivi un controllo di sicurezza denominato `PinCodeAttempts`, puoi utilizzare un elemento di ambito con lo stesso nome all'interno del tuo ambito.

L'associazione dell'ambito consente l'associazione di elementi di ambito ai controlli di sicurezza. Quando il client chiede un elemento di ambito, questa configurazione definisce quali controlli di sicurezza devono essere applicati. Ad esempio, puoi associare l'elemento di ambito `access-restricted` al tuo controllo di sicurezza `PinCodeAttempts`.

L'associazione dell'ambito è utile quando desideri proteggere una risorsa differentemente, a seconda di quale sia l'applicazione che sta provando ad accedere a essa. Puoi anche associare un ambito a un elenco di zero o più controlli di sicurezza.

Ad esempio: scope = `access-restricted deletePrivilege`

* Nell'applicazione A
    * `access-restricted` è associato a `PinCodeAttempts`.
    * `deletePrivilege` è associato a una stringa vuota.
* Nell'applicazione B
    * `access-restricted` è associato a `PinCodeAttempts`.
    * `deletePrivilege` è associato a `UserLogin`.

Per associare il tuo elemento di ambito a una stringa vuota, non selezionare alcun controllo di sicurezza nel menu a comparsa **Add New Scope Element Mapping**.

![Associazione ambito](/images/scope_mapping.png "Schermata Add new scope element mapping")

Puoi anche modificare manualmente il file JSON di configurazione dell'applicazione con la configurazione richiesta ed eseguire nuovamente il push delle modifiche a un MobileFirst Server.

1. Da una **finestra di riga di comando**, vai alla cartella root del progetto ed esegui il comando `mfpdev app pull`.
2. Apri il file di configurazione, che si trova nella cartella `[cartella-progetto]\mobilefirst`.
3. Modifica il file definendo una proprietà `scopeElementMapping`. In questa proprietà, definisci le coppie di dati, ciascuna delle quali è formata dal nome del tuo elemento di ambito selezionato e da una stringa di zero o più controlli di sicurezza separati da spazi a cui viene associato l'elemento. Ad esempio:

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. Distribuisci il file JSON di configurazione aggiornato eseguendo il seguente comando:
  ```bash
  mfpdev app push
  ```

Puoi anche eseguire il push delle configurazioni aggiornate ai server remoti. Fai riferimento all'esercitazione Using MobileFirst CLI to Manage MobileFirst artifacts.
{: note}

### Protezione delle risorse
{: #protecting-resources}

Nel modello OAuth, una risorsa protetta è una risorsa che richiede un token di accesso. Puoi utilizzare il framework di sicurezza MobileFirst per proteggere sia le risorse ospitate su un'istanza di MobileFirst Server che le risorse su un server esterno. Proteggi una risorsa assegnando ad essa un ambito che definisce le autorizzazioni richieste per acquisire un token di accesso per la risorsa.

Puoi proteggere le tue risorse in vari modi:

#### Ambito dell'applicazione obbligatorio
{: #mandatoryappscope}

A livello dell'applicazione, puoi definire un ambito che si applica a tutte le risorse utilizzate dall'applicazione. Il framework di sicurezza esegue questi controlli (se esistono) in aggiunta ai controlli di sicurezza dell'ambito di risorsa richiesto.

**Nota**:
   * L'ambito dell'applicazione obbligatorio non viene applicato quando accedi a una risorsa non protetta.
   * Il token di accesso concesso per l'ambito della risorsa non contiene l'ambito dell'applicazione obbligatorio.

Nella MobileFirst Operations Console, seleziona la tua applicazione dalla sezione **Applications** della barra laterale di navigazione e seleziona quindi la scheda **Security**. In **Mandatory Application Scope**, seleziona **Add to Scope**.

![Ambito dell'applicazione obbligatorio](/images/mandatory-application-scope.png "Schermata Configure mandatory application scope")

Puoi anche modificare manualmente il file JSON di configurazione dell'applicazione con la configurazione richiesta ed eseguire nuovamente il push delle modifiche a un MobileFirst Server.

1. Da una **finestra di riga di comando**, vai alla cartella root del progetto ed esegui il comando `mfpdev app pull`.
2. Apri il file di configurazione, che si trova nella cartella **project-folder\mobilefirst**.
3. Modifica il file definendo una proprietà `mandatoryScope` e impostando il valore della proprietà su una stringa di ambito che contiene un elenco separato da spazi dei tuoi elementi di ambito selezionati. Ad esempio,

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. Distribuisci il file JSON di configurazione aggiornato eseguendo il comando: mfpdev app push.

Puoi anche eseguire il push delle configurazioni aggiornate ai server remoti.

#### Protezione delle risorse dell'adattatore
{: #protectadapterres}

Nel tuo adattatore, puoi specificare l'ambito di protezione per un metodo Java o una procedura di risorsa JavaScript oppure per un'intera classe di risorse Java. Un ambito è definito come una stringa di uno o più elementi di ambito separati da spazi (“scopeElement1 scopeElement2 …”) oppure null per applicare l'ambito predefinito. Per ulteriori informazioni sulla protezione di risorse dell'adattatore, vedi [Protezione degli adattatori](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters).

### Disabilitazione della protezione delle risorse
{: #disablingresprotection}

Puoi disabilitare la protezione delle risorse MobileFirst predefinita per una specifica risorsa dell'adattatore Java o JavaScript o per un'intera classe Java, come descritto nelle seguenti sezioni Java e JavaScript. Quando la protezione delle risorse è disabilitata, il framework di sicurezza MobileFirst non richiede un token per accedere alla risorsa.

#### Disabilitazione della protezione OAuth delle risorse Java
{: #disablejavaresoauthprotection}

Per disabilitare completamente la protezione OAuth per un metodo di risorsa o una classe di risorse Java, aggiungi l'annotazione `@OAuthSecurity` alla dichiarazione di risorsa o classe e imposta il valore dell'elemento `enabled` su `false`:

```
@OAuthSecurity(enabled = false)
```

Il valore predefinito dell'elemento `enabled` dell'annotazione è `true`. Quando l'elemento `enabled` è impostato su `false`, l'elemento `scope` viene ignorato e la risorsa o la classe di risorse non sono protette.

**Nota**: quando assegni un ambito a un metodo di una classe non protetta, il metodo viene protetto indipendentemente dall'annotazione della classe a meno che tu non imposti l'elemento `enabled` dell'annotazione della risorsa su `false`.

Controlla i seguenti esempi: 

Il seguente codice disabilita la protezione delle risorse per un metodo `helloUser`:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

Il seguente codice disabilita la protezione delle risorse per una classe `MyUnprotectedResources`:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### Disabilitazione della protezione OAuth delle risorse JavaScript
{: #disablejavascriptresoauthprotection}

Per disabilitare completamente la protezione OAuth per una risorsa dell'adattatore JavaScript (procedura), nel file **adapter.xml** imposta l'attributo `secured` dell'elemento <procedure> su `false`:

```javascript
<procedure name="procedureName" secured="false">
```

Quando l'attributo `secured` è impostato su `false`, l'attributo `scope` viene ignorato e la risorsa non è protetta.

Controlla il seguente esempio:

Il seguente codice disabilita la protezione delle risorse per una procedura `userName`:

```javascript
<procedure name="userName" secured="false">
```

### Definizione di risorse non protette
{: #defunprotectedresources}

Una risorsa non protetta è una risorsa che non richiede un token di accesso. Il framework di sicurezza MobileFirst non gestisce l'accesso alle risorse non protette e non convalida o controlla l'identità dei client che accedono a tali risorse. Pertanto, funzioni quali Direct Update, il blocco dell'accesso ai dispositivi o la disabilitazione in remoto di un'applicazione non sono supportate per le risorse non protette.

### Protezione di risorse esterne
{: #protecextresources}

Per proteggere le risorse esterne, aggiungi un filtro risorsa con un modulo di convalida del token di accesso al server di risorse esterno. Il modulo di convalida del token utilizza l'endpoint di introspezione del server di autorizzazione del framework di sicurezza per convalidare i token di accesso MobileFirst prima di concedere al client OAuth l'accesso alle risorse. Puoi utilizzare l'API REST MobileFirst per il runtime MobileFirst per creare un tuo modulo di convalida del token di accesso per qualsiasi server esterno. In alternativa, utilizza una delle estensioni MobileFirst fornite per proteggere le risorse Java esterne.
