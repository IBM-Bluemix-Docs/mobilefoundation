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

#	Sicherheitsdienstprogramme
{: #security_utilities}

Die clientseitige Mobile Foundation-API stellt einige Sicherheitsdienstprogramme bereit, mit denen Sie die Daten Ihrer Benutzer schützen können. Funktionen wie JSONStore sind ausgezeichnet für den Schutz Ihrer JSON-Objekte geeignet. Es wird empfohlen, binäre Blobs nicht in einer JSONStore-Sammlung zu speichern.

Speichern Sie Binärdaten stattdessen im Dateisystem und speichern Sie die Dateipfade und andere Metadaten in einer JSONStore-Sammlung. Wenn Sie Dateien wie Bilder schützen möchten, können Sie sie als Base64-Zeichenfolgen codieren, sie verschlüsseln und die Ausgabe auf Platte schreiben. Um die Daten zu entschlüsseln, können Sie die Metadaten in einer JSONStore-Sammlung suchen. Lesen Sie die verschlüsselten Daten von der Platte und entschlüsseln Sie sie mithilfe der gespeicherten Metadaten. Diese Metadaten können den Schlüssel, Initialisierungsvektor (IV), Dateityp, Dateipfad, das Salt und andere Attribute enthalten.

Als übergeordnete API stellt SecurityUtils die folgenden APIs bereit:

* Schlüsselgenerierung: Die Funktion für Schlüsselgenerierung übergibt ein Kennwort nicht direkt an die Verschlüsselungsfunktion, sondern generiert mit PBKDF2 (Password-Based Key Derivation Function v2) einen 256-Bit-Schlüssel für die Verschlüsselungs-API. Die Funktion akzeptiert einen Parameter für die Anzahl der Iterationen. Je größer diese Anzahl ist, desto länger würde ein Angreifer benötigen, um Ihren Schlüssel zu knacken. Verwenden Sie mindestens einen Wert von 10.000. Das Salt muss eindeutig sein und macht es Angreifern schwer, Ihr Kennwort mit vorhandenen Hashinformationen zu entschlüsseln. Verwenden Sie eine Länge von 32 Byte.
* Verschlüsselung: Die Eingabe wird nach AES (Advanced Encryption Standard) verschlüsselt. Die API verwendet einen Schlüssel, der von der API für Schlüsselgenerierung generiert wurde. Intern generiert sie einen sicheren IV, der die Randomisierung der ersten Blockverschlüsselung verstärkt. Text wird verschlüsselt. Wenn Sie ein Bild oder ein anderes Binärformat verschlüsseln möchten, wandeln Sie Ihre Binärdaten mit diesen APIs in Base64-Text um. Diese Verschlüsselungsfunktion gibt ein aus folgenden Teilen bestehendes Objekt zurück:
    * ct (Chiffriertext, der auch als verschlüsselter Text bezeichnet wird)
    * IV
    * v (Version, um die Weiterentwicklung der API bei gleichzeitig weiter bestehender Kompatibilität mit einer früheren Version zu ermöglichen)
* Entschlüsselung: Die Ausgabe der Verschlüsselungs-API wird als Eingabe verwendet. Der Chiffriertext oder der verschlüsselte Text wird als Klartext entschlüsselt.
* Ferne zufällige Zeichenfolge: Über den Kontakt zu einem Zufallsgenerator auf dem MobileFirst-Server wird eine zufällige hexadezimale Zeichenfolge abgerufen. Die Standardlänge liegt bei 20 Byte. Sie können diesen Wert aber auf bis zu 64 Byte erhöhen.
* Lokale zufällige Zeichenfolge: Im Gegensatz zur API für die ferne Zeichenfolge, die Netzzugriff erfordert, wird hier lokal eine zufällige hexadezimale Zeichenfolge generiert und abgerufen. Die Standardlänge liegt bei 32 Byte. Es gibt keinen Maximalwert. Die Operationszeit steigt proportional mit der Bytezahl.
* Base64-Codierung: Diese API wendet auf eine Zeichenfolge die Base64-Codierung an. Aufgrund des verwendeten Algorithmus bedeutet eine Base64-Codierung, dass sich die Größe der ursprünglichen Daten um etwa das 1,37-Fache erhöht.
* Base64-Decodierung: Diese API wendet auf eine Base64-codierte Zeichenfolge die Base64-Decodierung an.

## Einrichtung
Sie müssen die folgenden Dateien importieren, um die APIs der JSONStore-Sicherheitsdienstprogramme verwenden zu können.

### iOS einrichten

```objc
#import "WLSecurityUtils.h"
```

### Android einrichten

```java
import com.worklight.wlclient.api.SecurityUtils
```

### JavaScript einrichten
Es ist keine Einrichtung erforderlich.

## Beispiele für iOS
### Verschlüsselung und Entschlüsselung in iOS

```objc
// Vom Benutzer angegebenes Kennwort, aus Gründen der Einfachheit im Klartext.
NSString* password = @"HelloPassword";

// Zufälliges Salt mit empfohlener Länge.
NSString* salt = [WLSecurityUtils generateRandomStringWithBytes:32];

// Empfohlene Anzahl Iterationen.
int iterations = 10000;

// Eintrag eines Fehlers, sofern einer auftritt.
NSError* error = nil;

// Aufruf, der den Schlüssel generiert.
NSString* key = [WLSecurityUtils generateKeyWithPassword:password
                                 andSalt:salt
                                 andIterations:iterations
                                 error:&error];

// Text, der verschlüsselt wird.
NSString* originalString = @"My secret text";
NSDictionary* dict = [WLSecurityUtils encryptText:originalString
                                      withKey:key
                                      error:&error];

// Zurückgegeben werden sollte: 'My secret text'.
NSString* decryptedString = [WLSecurityUtils decryptWithKey:key
                                             andDictionary:dict
                                             error:&error];
```
{: codeblock}

### Base64 codieren und decodieren (in iOS)

```objc
// Eingabezeichenfolge.
NSString* originalString = @"Hello world!";

// In Base64 codieren.
NSData* originalStringData = [originalString dataUsingEncoding:NSUTF8StringEncoding];
NSString* encodedString = [WLSecurityUtils base64StringFromData:originalStringData length:originalString.length];

// Zurückgegeben werden sollte: 'Hello world!'.
NSString* decodedString = [[NSString alloc] initWithData:[WLSecurityUtils base64DataFromString:encodedString] encoding:NSUTF8StringEncoding];
```
{: codeblock}

### Ferne zufällige Zeichenfolge abrufen (in iOS)

```objc
[WLSecurityUtils getRandomStringFromServerWithBytes:32
                 timeout:1000
                 completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {

  // Bevor Sie fortfahren, möchten Sie vielleicht die Antwort und den Verbindungsfehler sehen.

  // Sichere zufällige Zeichenfolge abrufen.
  NSString* secureRandom = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
}];
```
{: codeblock}

## Beispiele für Android
### Verschlüsselung und Entschlüsselung in Android

```java
String password = "HelloPassword";
String salt = SecurityUtils.getRandomString(32);
int iterations = 10000;

String key = SecurityUtils.generateKey(password, salt, iterations);

String originalText = "Hello World!";

JSONObject encryptedObject = SecurityUtils.encrypt(key, originalText);

// Der entschlüsselte Text stimmt mit dem ursprünglichen Text überein.
String decipheredText = SecurityUtils.decrypt(key, encryptedObject);
```
{: codeblock}

### Base64 codieren und decodieren (in Android)

```java
import android.util.Base64;

String originalText = "Hello World";
byte[] base64Encoded = Base64.encode(text.getBytes("UTF-8"), Base64.DEFAULT);

String encodedText = new String(base64Encoded, "UTF-8");

byte[] base64Decoded = Base64.decode(text.getBytes("UTF-8"), Base64.DEFAULT);

// Der decodierte Text stimmt mit dem ursprünglichen Text überein.
String decodedText = new String(base64Decoded, "UTF-8");
```
{: codeblock}

### Ferne zufällige Zeichenfolge abrufen (in Android)

```java
Context context; // Dies ist der Kontext der aktuellen Aktivität.
int byteLength = 32;

// Wenn der Listener die Antwort vom Server erhalten hat, ruft er die Callback-Funktionen auf.
WLRequestListener listener = new WLRequestListener(){
  @Override
  public void onSuccess(WLResponse wlResponse) {
    // Erfolgs-Handler implementieren.
  }

  @Override
  public void onFailure(WLFailResponse wlFailResponse) {
    // Fehler-Handler implementieren.
    }
};

SecurityUtils.getRandomStringFromServer(byteLength, context, listener);
```
{: codeblock}

### Lokale zufällige Zeichenfolge abrufen (in Android)

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

## Beispiele für JavaScript
### Verschlüsselung und Entschlüsselung in JavaScript

```javascript
// Den Schlüssel in einer Variablen belassen, damit er an die Ver- und Entschlüsselungs-API übergeben werden kann.
var key;

// Schlüssel generieren.
WL.SecurityUtils.keygen({
  password: 'HelloPassword',
  salt: Math.random().toString(),
  iterations: 10000
})

.then(function (res) {

  // Schlüsselvariable aktualisieren.
  key = res;

  // Text verschlüsseln.
  return WL.SecurityUtils.encrypt({
    key: key,
    text: 'My secret text'
  });
})

.then(function (res) {

  // Schlüssel zum Ergebnisobjekt der Verschlüsselung hinzufügen.
  res.key = key;

  // Entschlüsseln.
  return WL.SecurityUtils.decrypt(res);
})

.then(function (res) {

  // Schlüssel aus dem Speicher entfernen.
  key = null;

  //res => 'My secret text'
})

.fail(function (err) {
  // Fehler in den zuvor aufgerufenen APIs behandeln.
});
```
{: codeblock}

### Base64 codieren und decodieren (in JavaScript)

```javascript
WL.SecurityUtils.base64Encode('Hello World!')
.then(function (res) {
  return WL.SecurityUtils.base64Decode(res);
})
.then(function (res) {
  //res => 'Hello World!'
})
.fail(function (err) {
  // Fehler behandeln.
});
```
{: codeblock}

### Ferne zufällige Zeichenfolge abrufen (in JavaScript)

```javascript
WL.SecurityUtils.remoteRandomString(32)
.then(function (res) {
  // res => deba58e9601d24380dce7dda85534c43f0b52c342ceb860390e15a638baecc7b
})
.fail(function (err) {
  // Fehler behandeln.
});
```
{: codeblock}

### Lokale zufällige Zeichenfolge abrufen (in JavaScript)

```javascript
WL.SecurityUtils.localRandomString(32)
.then(function (res) {
  // res => 40617812588cf3ddc1d1ad0320a907a7b62ec0abee0cc8c0dc2de0e24392843c
})
.fail(function (err) {
  // Fehler behandeln.
});
```
{: codeblock}
