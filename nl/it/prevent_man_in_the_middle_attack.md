---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, MIM attack, certificate pinning

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Evita attacchi man-in-the-middle
{: #prevent_man_in_the_middle_attack}


Quando si comunica su reti pubbliche, è essenziale inviare e ricevere informazioni in modo protetto. Il protocollo ampiamente utilizzato per proteggere queste comunicazioni è SSL/TLS. SSL/TLS utilizza certificati digitali per fornire l'autenticazione e la crittografia. Questi protocolli si basano sulla verifica della catena di certificati e sono vulnerabili a un certo numero di attacchi pericolosi, compresi gli attacchi man-in-the-middle, che si verificano quando una parte non autorizzata è in grado di visualizzare e modificare tutto il traffico che passa tra il dispositivo mobile e i sistemi di backend.
{: shortdesc}

Riduci gli attacchi alla sicurezza tramite l'[associazione di certificati](#cert_pinning) ed evita attacchi MitM (man-in-the-middle).

Questa funzione è facoltativa ed è disponibile solo per i piani Professional.
{: note}

Il processo di associazione di certificati consiste nei passi di seguito indicati:

1. Esegui la [Configurazione del certificato](#certsetup).
2. Configura il tuo codice client per richiamare il metodo [API di associazione di certificato](#certpinapi) corretto prima di effettuare una richiesta protetta.

Durante l'handshake SSL (prima richiesta al server), l'SDK client Mobile Foundation verifica che la chiave pubblica del certificato server corrisponda alla chiave pubblica del certificato archiviato nell'applicazione.

Devi utilizzare un certificato acquistato da un'autorità di certificazione (CA, Certificate Authority). I certificati autofirmati **non sono supportati**.
{: note}

## Associazione di certificato
{: #cert_pinning}

I protocolli che si basano sulla verifica della catena di certificati, come ad esempio SSL/TLS, sono vulnerabili a un certo numero di attacchi pericolosi, compresi gli attacchi man-in-the-middle, che si verificano quando una parte non autorizzata è in grado di visualizzare e modificare tutto il traffico che passa tra il dispositivo mobile e i sistemi di backend. Perché venga ritenuto genuino e valido, un certificato viene firmato digitalmente da un certificato root che appartiene a un'autorità di certificazione (CA, Certificate Authority) ritenuta attendibile. I sistemi operativi e i browser mantengono degli elenchi di certificati root di CA ritenute attendibili in modo da poter verificare facilmente i certificati che le CA hanno emesso e firmato.

IBM Mobile Foundation fornisce una API per abilitare l'**associazione di certificato**. È supportata in applicazioni MobileFirst Cordova multipiattaforma, Android native e iOS native.

## Processo di associazione di certificato
{: #certpinprocess}

L'associazione di certificato è il processo di associazione di un host alla sua chiave pubblica prevista. Puoi configurare il tuo codice client per accettare solo uno specifico certificato per il tuo nome dominio invece di qualsiasi certificato che corrisponde a un certificato root di una CA attendibile riconosciuto dal sistema operativo o dal browser. Una copia del certificato viene inserita nella tua applicazione client Durante l'handshake SSL (prima richiesta al server), l'SDK client MobileFirst verifica che la chiave pubblica del certificato server corrisponda alla chiave pubblica del certificato archiviato nell'applicazione.

Puoi anche associare più certificati alla tua applicazione client. Inserisci una copia di tutti i certificati nella tua applicazione client. Durante l'handshake SSL (prima richiesta al server), l'SDK client MobileFirst verifica che la chiave pubblica del certificato server corrisponda alla chiave pubblica di uno dei certificati archiviati nell'applicazione.
{: note}

* **Importante**
    * Alcuni sistemi operativi mobili potrebbero memorizzare in cache il risultato del controllo della convalida del certificato. Pertanto, il tuo codice richiama il metodo API di associazione del certificato **prima** che venga effettuata una richiesta protetta. In caso contrario, eventuali richieste successive potrebbero tralasciare il controllo della convalida e dell'associazione del certificato.
    * Assicurati di utilizzare solo le API Mobile Foundation per tutte le comunicazioni con l'host correlato, anche dopo l'associazione di certificato. L'utilizzo di API di terze parti per interagire con lo stesso host potrebbe portare a una modalità di funzionamento imprevista, come ad esempio la memorizzazione in cache di un certificato non verificato da parte del sistema operativo mobile.
    * Il richiamo del metodo API di associazione di certificato una seconda volta sovrascrive la precedente operazione di associazione.

Se il processo di associazione viene eseguito con esito positivo, la chiave pubblica all'interno del certificato fornito viene utilizzata per verificare l'integrità del certificato MobileFirst Server durante l'handshake SSL/TLS della richiesta protetta. Se il processo di associazione non riesce, tutte le richieste SSL/TLS al server vengono rifiutate dall'applicazione client.

## Configurazione del certificato
{: #certsetup}

Devi utilizzare un certificato acquistato da un'autorità di certificazione (CA, Certificate Authority). I certificati autofirmati **non sono supportati**. Per compatibilità con gli ambienti supportati, assicurati di utilizzare un certificato codificato in formato **DER** (Distinguished Encoding Rules, come definito dallo standard International Telecommunications Union X.690).

Il certificato deve essere inserito sia in MobileFirst Server sia nella tua applicazione Inserisci il certificato nel seguente modo:

* In MobileFirst Server (WebSphere Application Server, WebSphere Application Server Liberty o Apache Tomcat), consulta la documentazione per il tuo server delle applicazioni specifico per informazioni su come configurare SSL/TLS e i certificati.
* Nella tua applicazione:
    * Per iOS nativa, aggiungi il certificato al **bundle** dell'applicazione
    * Per Android nativa, inserisci il certificato nella cartella **assets**
    * Per Cordova, inserisci il certificato nella cartella **app-name\www\certificates** (se la cartella non è già presente, procedi a crearla).

## API di associazione di certificato
{: #certpinapi}

L'associazione di certificato consiste nel seguente metodo di API sovraccaricato, in cui un metodo ha un parametro ``certificateFilename``, dove ``certificateFilename`` è il nome del file certificato, e il secondo metodo ha un parametro ``certificateFilenames``, dove ``certificateFilenames`` è un array di nomi dei file certificato.

### Android
{: #certpinapiandroid}

* Singolo certificato:

    Sintassi: pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

    Esempio:

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* Più certificati:

    Sintassi: pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    Esempio:

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

Il metodo di associazione di certificato genera un'eccezione in due casi:

* Il file non esiste
* Il file è nel formato errato

### iOS
{: #certpinapiios}

* Associazione di certificato singolo

    Sintassi: pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    Il metodo di associazione di certificato genera un'eccezione in due casi:

    * Il file non esiste
    * Il file è nel formato errato

* Associazione di più certificati

    Sintassi: pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    Il metodo di associazione di certificato genera un'eccezione in due casi:

    * Non esiste alcun file di certificato
    * Nessuno del file di certificato è in un formato corretto

**In Objective-C**:

Esempio; certificato singolo:

```objectc
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

Esempio: più certificati:

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**In Swift**:

Esempio; certificato singolo:

```swift
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

Esempio: più certificati:

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

Il metodo di associazione di certificato genererà un'eccezione in due casi:

* Il file non esiste
* Il file è nel formato errato

### Cordova
{: #certpinapicordova}

* Associazione di certificato singolo:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* Associazione di più certificati:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

Il metodo di associazione di certificato restituisce una promessa:

* Il metodo di associazione di certificato richiama il metodo `onSuccess` se l'associazione ha esito positivo.
* Il metodo di associazione di certificato attiva il callback `onFailure` nei seguenti due casi:
  * Il file non esiste.
  * Il file è nel formato errato.

Successivamente, se viene effettuata una richiesta protetta a un server il cui certificato non è associato, viene richiamato il callback `onFailure` della richiesta specifica (ad esempio `obtainAccessToken` o `WLResourceRequest`).

Ulteriori informazioni sul metodo API di associazione di certificato sono disponibili nella [Guida di riferimento per l'API](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks).
{: note}
