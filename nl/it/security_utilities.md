---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: security

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Programmi di utilità di sicurezza
{: #security_utilities}

L'API lato client di Mobile Foundation fornisce alcuni programmi di utilità di sicurezza per proteggere i dati dell'utente. Funzioni come JSONStore sono ottime se vuoi proteggere gli oggetti JSON. Ti consigliamo di non archiviare blob binari in una raccolta JSONStore.

Invece, archivia i dati binari sul file system e archivia i percorsi di file e altri metadati all'interno di una raccolta JSONStore. Se vuoi proteggere file come, ad esempio, le immagini, puoi codificarli come stringhe in base64, crittografarli e scrivere l'output sul disco. Per decrittografare i dati, puoi cercare i metadati in una raccolta JSONStore. Leggi quindi i dati crittografati dal disco e decrittografali utilizzando i metadati archiviati. Questi metadati possono includere la chiave, salt, il vettore di inizializzazione (IV), il tipo di file, il percorso del file e altro.

A un alto livello, l'API SecurityUtils fornisce le seguenti API:

* Generazione chiavi - Invece di passare direttamente una password alla funzione di crittografia, questa funzione di generazione chiavi utilizza PBKDF2 (Password-Based Key Derivation Function v2) per generare una chiave sicura a 256 bit per l'API di crittografia. Questa prende un parametro per il numero di iterazioni. Più alto è il numero, più tempo impiega un aggressore a forzare la tua chiave. Utilizza un valore di almeno 10.000. Il salt deve essere univoco, ciò garantisce che gli aggressori avranno più difficoltà a utilizzare le informazioni hash esistenti per attaccare la tua password. Utilizza una lunghezza di 32 byte.
* Crittografia - L'input viene crittografato tramite AES (Advanced Encryption Standard). L'API prende una chiave che viene generata con l'API di generazione chiavi. Internamente, genera un vettore di inizializzazione (IV) sicuro, che viene utilizzato per aggiungere la randomizzazione alla prima cifratura a blocchi. Il testo viene crittografato. Se vuoi crittografare un'immagine o un altro formato binario, converti il tuo binario in testo in base64 utilizzando queste API. Questa funzione di crittografia restituisce un oggetto con le seguenti parti:
    * ct (testo cifrato, chiamato anche testo crittografato)
    * IV (vettore di inizializzazione)
    * v (versione, che consente all'API di evolversi pur rimanendo compatibile con una versione precedente)
* Decrittografia - Prende l'output dall'API di crittografia come input e decrittografa il testo cifrato o crittografato in testo normale.
* Stringa casuale remota - Ottiene una stringa esadecimale casuale contattando un generatore casuale sul server MobileFirst. Il valore predefinito è 20 byte, ma puoi modificare il numero fino a 64 byte.
* Stringa casuale locale - Ottiene una stringa esadecimale casuale generandone una localmente, a differenza dell'API di stringa casuale remota, che richiede l'accesso alla rete. Il valore predefinito è 32 byte e non c'è un valore massimo. Il tempo di operazione è proporzionale al numero di byte.
* Codifica in base64 - Prende una stringa e applica la codifica base64. Incorrere in una codifica base64 in base alla natura dell'algoritmo significa che la dimensione dei dati è aumentata di circa 1,37 volte la dimensione originale.
* Decodifica in base64 - Prende una stringa codificata in base64 e applica la decodifica base64.

## Configurazione
Per utilizzare le API dei programma di utilità di sicurezza JSONStore, assicurati di importare i seguenti file.

### Configura iOS

```objc
#import "WLSecurityUtils.h"
```

### Configura Android

```java
import com.worklight.wlclient.api.SecurityUtils
```

### Configura JavaScript
Non è richiesta alcuna configurazione.

## Esempi per iOS
### Crittografia e decrittografia in iOS

```objc
// User provided password, hardcoded only for simplicity.
NSString* password = @"HelloPassword";

// Random salt with recommended length.
NSString* salt = [WLSecurityUtils generateRandomStringWithBytes:32];

// Recomended number of iterations.
int iterations = 10000;

// Populated with an error if one occurs.
NSError* error = nil;

// Call that generates the key.
NSString* key = [WLSecurityUtils generateKeyWithPassword:password
                                 andSalt:salt
                                 andIterations:iterations
                                 error:&error];

// Text that is encrypted.
NSString* originalString = @"My secret text";
NSDictionary* dict = [WLSecurityUtils encryptText:originalString
                                      withKey:key
                                      error:&error];

// Should return: 'My secret text'.
NSString* decryptedString = [WLSecurityUtils decryptWithKey:key
                                             andDictionary:dict
                                             error:&error];
```
{: codeblock}

### Codifica e decodifica in base64 in iOS

```objc
// Input string.
NSString* originalString = @"Hello world!";

// Encode to base64.
NSData* originalStringData = [originalString dataUsingEncoding:NSUTF8StringEncoding];
NSString* encodedString = [WLSecurityUtils base64StringFromData:originalStringData length:originalString.length];

// Should return: 'Hello world!'.
NSString* decodedString = [[NSString alloc] initWithData:[WLSecurityUtils base64DataFromString:encodedString] encoding:NSUTF8StringEncoding];
```
{: codeblock}

### Ottieni stringa casuale remota in iOS

```objc
[WLSecurityUtils getRandomStringFromServerWithBytes:32
                 timeout:1000
                 completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {

  // You might want to see the response and the connection error before moving forward.

  // Get the secure random string.
  NSString* secureRandom = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
}];
```
{: codeblock}

## Esempi per Android
### Crittografia e decrittografia in Android

```java
String password = "HelloPassword";
String salt = SecurityUtils.getRandomString(32);
int iterations = 10000;

String key = SecurityUtils.generateKey(password, salt, iterations);

String originalText = "Hello World!";

JSONObject encryptedObject = SecurityUtils.encrypt(key, originalText);

// Deciphered text will be the same as the original text.
String decipheredText = SecurityUtils.decrypt(key, encryptedObject);
```
{: codeblock}

### Codifica e decodifica in base64 in Android

```java
import android.util.Base64;

String originalText = "Hello World";
byte[] base64Encoded = Base64.encode(text.getBytes("UTF-8"), Base64.DEFAULT);

String encodedText = new String(base64Encoded, "UTF-8");

byte[] base64Decoded = Base64.decode(text.getBytes("UTF-8"), Base64.DEFAULT);

// Decoded text will be the same as the original text.
String decodedText = new String(base64Decoded, "UTF-8");
```
{: codeblock}

### Ottieni stringa casuale remota in Android

```java
Context context; // This is the current Activity's context.
int byteLength = 32;

// Listener calls the callback functions after it gets the response from the server.
WLRequestListener listener = new WLRequestListener(){
  @Override
  public void onSuccess(WLResponse wlResponse) {
    // Implement the success handler.
  }

  @Override
  public void onFailure(WLFailResponse wlFailResponse) {
    // Implement the failure handler.
    }
};

SecurityUtils.getRandomStringFromServer(byteLength, context, listener);
```
{: codeblock}

### Ottieni stringa casuale locale in Android

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

## Esempi per JavaScript
### Crittografia e decrittografia in JavaScript

```javascript
// Keep the key in a variable so that it can be passed to the encrypt and decrypt API.
var key;

// Generate a key.
WL.SecurityUtils.keygen({
  password: 'HelloPassword',
  salt: Math.random().toString(),
  iterations: 10000
})

.then(function (res) {

  // Update the key variable.
  key = res;

  // Encrypt text.
  return WL.SecurityUtils.encrypt({
    key: key,
    text: 'My secret text'
  });
})

.then(function (res) {

  // Append the key to the result object from encrypt.
  res.key = key;

  // Decrypt.
  return WL.SecurityUtils.decrypt(res);
})

.then(function (res) {

  // Remove the key from memory.
  key = null;

  //res => 'My secret text'
})

.fail(function (err) {
  // Handle failure in any of the previously called APIs.
});
```
{: codeblock}

### Codifica e decodifica in base64 in JavaScript

```javascript
WL.SecurityUtils.base64Encode('Hello World!')
.then(function (res) {
  return WL.SecurityUtils.base64Decode(res);
})
.then(function (res) {
  //res => 'Hello World!'
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

### Ottieni stringa casuale remota in JavaScript

```javascript
WL.SecurityUtils.remoteRandomString(32)
.then(function (res) {
  // res => deba58e9601d24380dce7dda85534c43f0b52c342ceb860390e15a638baecc7b
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

### Ottieni stringa casuale locale in JavaScript

```javascript
WL.SecurityUtils.localRandomString(32)
.then(function (res) {
  // res => 40617812588cf3ddc1d1ad0320a907a7b62ec0abee0cc8c0dc2de0e24392843c
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}
