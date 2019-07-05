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

#	Programa de utilidad de seguridad
{: #security_utilities}

La API del lado del cliente de Mobile Foundation proporciona algunos programas de utilidad de seguridad para proteger los datos del usuario. Las características como JSONStore son de gran utilidad si desea proteger objetos JSON. Recomendamos no almacenar blobs binarios en una recopilación JSONStore.

En su lugar, almacene los datos binarios en el sistema de archivos, y almacene las vías de acceso de archivos y otros metadatos dentro de una recopilación JSONStore. Si desea proteger los archivos como, por ejemplo, imágenes, puede codificarlas como series base64, cifrarlas, y grabar la salida en el disco. Para descifrar los datos, podrá buscar en los metadatos en una recopilación JSONStore. Lea los datos cifrados del disco y descífrelos mediante los metadatos almacenados. Estos metadatos pueden incluir la clave, la sal, el vector de inicialización (IV), el tipo de archivo o la vía de acceso al archivo, entre otros.

A un alto nivel, las SecurityUtils API proporciona las siguientes API:

* Generación de claves - En lugar de pasar directamente una contraseña a la función de cifrado, esta función de generación de claves utiliza PBKDF2 (Password-Based Key Derivation Function v2) para generar una clave de 256 bits para la API de cifrado. Toma un parámetro para el número de iteraciones. Cuanto mayor sea el número, más tiempo necesitará un atacante para averiguar su clave mediante fuerza bruta. Utilice un valor de al menos 10.000. La sal debe ser única, lo que dificulta mucho la labor de los atacantes que utilicen información hash existente para atacar su contraseña. Utilice una longitud de 32 bytes.
* Cifrado - La entrada se cifra utilizando AES (Advanced Encryption Standard). La API toma una clave que se genera con la API de generación de claves. Internamente, genera un vector de inicialización (IV) seguro, que se utiliza para añadir aleatorización al primer cifrado de bloque. Se cifra el texto. Si desea cifrar una imagen u otro formato binario, convierta su código binario en texto base64 utilizando estas API. Esta función de cifrado devuelve un objeto con las siguientes partes:
    * ct (texto cifrado, también denominado texto encriptado)
    * IV (vector de inicialización)
    * v (versión, que permite que la API evolucione mientras es todavía compatible con una versión anterior)
* Descifrado - Toma la salida de la API de cifrado como entrada y descifra el texto cifrado o encriptado en texto sin formato.
* Serie aleatoria remota - Obtiene una serie hexadecimal aleatoria al ponerse en contacto con un generador aleatorio en el servidor de MobileFirst. El valor predeterminado es de 20 bytes, pero este valor se puede incrementar hasta los 64 bytes.
* Serie aleatoria local - Obtiene una serie hexadecimal aleatoria generando una de forma local, a diferencia de la API de la serie aleatoria remota que precisa de acceso de red. El valor predeterminado es de 32 bytes y no hay un valor máximo. El tiempo de la operación es proporcional al número de bytes.
* Codificación base64 - Toma una serie y aplica la codificación base64. La utilización de la codificación base64 supone por la naturaleza del algoritmo que el tamaño de los datos se incrementa en aproximadamente 1.37 veces su tamaño original.
* Decodificar base64 - Toma una serie codificada en base64 y aplica la descodificación base64.

## Configuración
Asegúrese de importar los siguientes archivos para utilizar las API de los programas de utilidad de seguridad JSONStore.

### Configurar iOS

```objc
#import "WLSecurityUtils.h"
```

### Configurar Android

```java
import com.worklight.wlclient.api.SecurityUtils
```

### Configurar JavaScript
No es necesaria configuración alguna.

## Ejemplos para iOS
### Cifrado y descifrado en iOS

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

### Codificar y decodificar base64 en iOS

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

### Obtener serie aleatoria remota en iOS

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

## Ejemplos para Android
### Cifrado y descifrado en Android

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

### Codificar y decodificar base64 en Android

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

### Obtener serie aleatoria remota en Android

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

### Obtener serie aleatoria local en Android

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

## Ejemplos para JavaScript
### Cifrado y descifrado en JavaScript

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

### Codificar y decodificar base64 en JavaScript

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

### Obtener serie aleatoria remota en JavaScript

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

### Obtener serie aleatoria local en JavaScript

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
