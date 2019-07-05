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
{{site.data.keyword.mobilefoundation_short}} **JSONStore** es una API del lado del cliente opcional que proporciona un sistema ligero de almacenamiento orientado a documentos. JSONStore ofrece un almacenamiento persistente de **documentos JSON**. Los documentos en una aplicación están disponibles en JSONStore incluso cuando el dispositivo que ejecuta la aplicación está fuera de línea. Este almacenamiento persistente que siempre está disponible puede ser útil para que los usuarios accedan a documentos cuando, por ejemplo, no hay ninguna conexión de red disponible en el dispositivo.
{: shortdesc}

Puesto que es familiar para los desarrolladores, a veces en esta documentación se utiliza terminología de bases de datos relacionales para explicar JSONStore. Sin embargo, hay muchas diferencias entre una base de datos relacional y JSONStore. Por ejemplo, el esquema estricto que se utiliza para almacenar datos en las bases de datos relacionales es distinto de la aproximación con JSONStore. Con JSONStore, se puede almacenar cualquier contenido JSON e indexar el contenido que se necesita buscar.

## Características principales
{: #key-features-jsonstore }
* Indexación de datos para una búsqueda eficiente
* Mecanismo de seguimiento cambios únicamente locales en los datos almacenados
* Soporte para muchos usuarios
* El cifrado AES 256 de los datos almacenados proporciona seguridad y confidencialidad. Existe la posibilidad de segmentar la protección por usuario con protección de contraseña si hay más de un usuario en un único dispositivo.

Un único almacén puede tener muchas recopilaciones y cada recopilación puede tener muchos documentos. También es posible tener una aplicación de MobileFirst con varios almacenes. Consulte el soporte de múltiples usuarios de JSONStore para obtener más información.

## Nivel de soporte
{: #support-level-jsonstore }
* JSONStore está soportado en aplicaciones Android e iOS nativas (no hay soporte para Windows nativo (Universal y UWP)).
* JSONStore está soportado en aplicaciones Cordova iOS, Android y Windows (Universal y UWP).


## Terminología general de JSONStore
{: #general-jsonstore-terminology }
### Documento
{: #document }
Un documento es el bloque constructivo básico de JSONStore.

Un documento JSONStore es un objeto JSON con un identificador que se genera de forma automática (`_id`) y con unos datos JSON. Es similar a un registro o a una fila en la terminología de base de datos. El valor de `_id` siempre es un entero exclusivo dentro de una recopilación específica. Algunas operaciones como, por ejemplo, `add`, `replace` y `remove` en la clase `JSONStoreInstance` utilizan una matriz de documentos u objetos. Estos métodos son útiles para realizar operaciones con diversos documentos u objetos al mismo tiempo.

Documento individual:

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

Matriz de documentos:

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Recopilación
{: #collection }
Una recopilación JSONStore es parecido a una tabla en la terminología de base de datos.  
El siguiente ejemplo de código no corresponde a la forma en la que se almacenan los documentos en el disco, pero son una manera fácil de visualizar una recopilación a un alto nivel.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Almacén
{: #store }
Un almacén es un archivo JSONStore persistente que contiene una o varias recopilaciones.  
Un almacén es similar a una base de datos relacional en la terminología de base de datos. También se hace referencia al almacén como JSONStore.

### Campos de búsqueda
{: #search-fields }
Un campo de búsqueda es una pareja de clave-valor.  
Los campos de búsqueda son claves que se indexan para disminuir los tiempos de búsqueda, de forma parecida a los atributos o campos de columna en la terminología de base de datos.

Los campos de búsqueda adicionales son claves indexadas que no son parte de los datos JSON almacenados. Estos campos definen la clave cuyos valores (en la recopilación JSON) están indexados y que se utilizan para realizar las búsquedas con mayor rapidez.

Los tipos de datos válidos son: String, Boolean, Number e Integer. Estos tipos son únicamente sugerencias de tipo, no hay una validación de tipo. Es más, estos tipos determinan cómo se almacenan los campos indexables. Por ejemplo, `{age: 'number'}` indexará 1 como 1,0 y `{age: 'integer'}` indexará 1 como 1.

Campos de búsqueda y campos de búsqueda adicionales:

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

Sólo es posible indexar claves dentro de un objeto, no el objeto en sí mismo. En las matrices no es posible indexar una matriz o un índice específico de la matriz (matriz[n]), pero puede indexar objetos dentro de una matriz.

Indexación de valores dentro de una matriz:

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

### Consultas
{: #queries }
Las consultas son objetos que utilizan campos de búsqueda o campos de búsqueda adicionales para buscar documentos.  
En estos ejemplos se presupone que el campo de búsqueda del nombre es de tipo serie y que el campo de búsqueda de edad es de tipo entero.

Encontrar documentos donde `name` coincida con `carlos`:

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

Encontrar documentos con `name` que coincida con `carlos` y `age` coincida con `99`:

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### Partes de consulta
{: #query-parts }
Las partes de consulta se utilizan para crear búsquedas más avanzadas. Algunas operaciones de JSONStore como, por ejemplo, algunas versiones de `find` o `count` utilizan partes de consulta. La información dentro de la parte de consulta está unida mediante sentencias `AND` y `OR`. El criterio de búsqueda solo devuelve una coincidencia si todos elementos de la parte de consulta se evalúan como **true**. Utilice una o varias partes de consulta para buscar coincidencias que satisfagan una o varias de las partes de consulta.

Las búsquedas con partes de consulta solo funcionan con campos de búsqueda de nivel superior. Por ejemplo: `name` y no `name.first`. Utilice varias recopilaciones donde todos los campos de búsqueda sean de nivel superior para resolver este comportamiento. Las operaciones de partes de consulta que funcionan con campos de búsqueda de nivel superior son:
`equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike` y `notLeftLike`. El comportamiento es indeterminado si utiliza campos de búsqueda que no sean de un nivel superior.

## Tabla de características
{: #features-table }
Compare las características de JSONStore con aquellas otras características de otros formatos y tecnologías de almacenamiento de datos.

JSONStore es una API de JavaScript para almacenar datos dentro de aplicaciones Cordova que utilizan el plugin de MobileFirst, una API Objective-C para aplicaciones iOS nativas, y una API Java para aplicaciones Android nativas. Como referencia, se proporciona una comparación entre las distintas tecnologías de almacenamiento JavaScript para ver como JSONStore se compara frente a las mismas.

JSONStore es similar a otras tecnologías como, por ejemplo, LocalStorage, Indexed DB, Cordova Storage API y Cordova File API. En la tabla se muestra como algunas de las características que JSONStore proporciona frente a otras tecnologías. La característica JSONStore solo está disponible en simuladores y dispositivos iOS y Android.

| Característica                                            | JSONStore      | LocalStorage | IndexedDB | Cordova storage API | Cordova file API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Soporte Android (aplicaciones nativas y Cordova)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Soporte iOS (aplicaciones nativas y Cordova)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal y Windows 10 UWP (aplicaciones Cordova)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| Cifrado de datos	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Máximo almacenamiento	                                 |Espacio disponible |    ~5 MB     |   ~5 MB 	 | Espacio disponible	   | Espacio disponible  |
| Almacenamiento fiable (ver nota)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| Mantener seguimiento de cambios locales	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Soporte a varios usuarios                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Indexación	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| Tipo de almacenamiento	                                 | Documentos JSON | Valores clave-valor | Documentos JSON | Relacional (SQL) | Series     |
{: caption="Tabla 1. Comparación de características" caption-side="top"}

Almacenamiento fiable significa que no se suprimen los datos a no ser que se produzca una de las siguientes situaciones:
* La aplicación se elimina del dispositivo.
* Se llama a uno de los métodos que eliminan los datos.
{: note}

## Soporte a múltiples usuarios
{: #multiple-user-support }
Con JSONStore, es posible crear muchos almacenes con varias recopilaciones en una única aplicación de MobileFirst.

La API init (JavaScript) u open (iOS nativo y Android nativo) pueden tomar un objeto de opciones con un nombre de usuario. Los distintos almacenes son archivos diferentes en el sistema de archivos. El nombre de usuario se utiliza como el nombre de archivo del almacén. Estos distintos almacenes se pueden cifrar con distintas contraseñas por razones de seguridad y privacidad. Al llamar a la API closeAll se elimina el acceso a todas las recopilaciones. También es posible cambiar la contraseña de un almacén de cifrado llamando a la API changePassword.

Un caso de uso de ejemplo sería el de varios usuarios que comparten un dispositivo físico (por ejemplo, una tableta iPad o Android) y una aplicación de MobileFirst. El soporte de múltiples usuarios es útil si los empleados trabajan en distintos turnos y manejan datos privados de distintos clientes al utilizar la aplicación de MobileFirst.

## Seguridad
{: #security-jsonstore }
Todas las recopilaciones se pueden cifrar en un almacén para protegerlas.

Para cifrar todas las recopilaciones en un almacén, pase una contraseña a la API `init` (JavaScript) u `open` (iOS nativo y Android nativo). Si no se pasa una contraseña, no se cifrará ningún documento en las recopilaciones del almacén.

Algunos artefactos de seguridad (por ejemplo los de sal) se almacenan en la cadena de claves (iOS), las preferencias compartidas (Android) y en la caja de seguridad de credenciales (Windows Universal 8.1 y Windows 10 UWP). El almacén se cifra con una clave AES (Advanced Encryption Standard) de 256 bits. Todas las claves están reforzadas mediante PBKDF2 (Password-Based Key Derivation Function 2). Puede decidir cifrar las recopilaciones de datos de una aplicación, sin embargo, no puede conmutar entre los formatos cifrados y sin cifrar ni utilizar ambos formatos dentro de un almacén.

La clave que protege los datos en el almacén se basa en la contraseña de usuario que proporcione. La clave no caduca y puede cambiarla llamando a la API changePassword.

La clave de protección de datos (DPK) es la clave que se utiliza para descifrar el contenido del almacén. La DPK se mantiene en la cadena de claves de iOS incluso si la aplicación está desinstalada. Para eliminar tanto la clave la cadena de herramientas como todo lo demás que JSONStore coloca en la aplicación, utilice la API destroy. Este proceso no se aplica a Android porque la DPK cifrada se almacena en las preferencias compartidas y se borra de las mismas al desinstalar la aplicación.

La primera vez que JSONStore abre una recopilación con una contraseña, lo que significa que el desarrollador desea cifrar los datos dentro del almacén, JSONStore necesita una señal aleatoria. Dicha señal se puede obtener desde el cliente o desde el servidor.

Cuando la clave localKeyGen está presente en la implementación JavaScript de la API JSONStore, y tiene un valor true, se genera localmente una señal criptográfica segura. De lo contrario, la señal se genera poniéndose en contacto con el servidor, con lo que se necesita conectividad al servidor de MobileFirst. Esta señal solamente es necesaria la primera vez que se abre un almacén con una contraseña. Con las implementaciones nativas (Objective-C y Java) se puede genera una señal criptográficamente segura de forma predeterminada, o el usuario puede pasar una mediante la opción secureRandom.

Deberá valorar si es más conveniente:
* abrir un almacén fuera de línea y confiar en el cliente para generar esta señal aleatoria (menos seguro) o
* abrir el almacén con acceso al servidor de MobileFirst (es necesaria conectividad) y confiar en el servidor (más seguro)

### Programa de utilidad de seguridad
{: #security-utilities }
La API del lado del cliente de MobileFirst proporciona algunos programas de utilidad de seguridad para proteger los datos del usuario. Las características como JSONStore son de gran utilidad si desea proteger objetos JSON. Sin embargo, no se recomienda almacenar blobs binarios en una recopilación de JSONStore.

En su lugar, almacene los datos binarios en el sistema de archivos, y almacene las vías de acceso de archivos y otros metadatos dentro de una recopilación JSONStore. Si desea proteger los archivos como, por ejemplo, imágenes, puede codificarlas como series base64, cifrarlas, y grabar la salida en el disco.

Para descifrar los datos, puede buscar en los metadatos en una recopilación JSONStore, leer los datos cifrados del disco y descifrarlos mediante los metadatos almacenados.

Estos metadatos pueden incluir la clave, la sal, el vector de inicialización (IV), el tipo de archivo o la vía de acceso al archivo, entre otros.

Aprenda más sobre los [Programas de utilidad de JSONStore](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities).
{: tip}

### Cifrado de Windows 8.1 Universal y Windows 10 UWP
{: #windows-81-universal-and-windows-10-uwp-encryption }
Todas las recopilaciones se pueden cifrar en un almacén para protegerlas.

JSONStore utiliza [SQLCipher ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://sqlcipher.net/){: new_window} como su nueva tecnología de base de datos subyacente. SQLCipher es una compilación de SQLite creada por Zetetic, LLC que añade una capa de cifrado a la base de datos.

JSONStore utiliza SQLCipher en todas las plataformas. En Android e iOS existe una versión de código abierto de SQLCipher, denominada Community Edition, que se incorpora a las versiones de JSONStore que se incluyen en Mobile Foundation. Las versiones de Windows de SQLCipher únicamente están disponibles a través de una licencia comercial y Mobile Foundation no las puede distribuir directamente.

En lugar de ello, JSONStore para Windows 8 Universal incluye SQLite como la base de datos subyacente. Para cifrar datos para alguna de estas plataformas, necesita adquirir su propia versión de SQLCipher y cambiar la versión de SQLite que se incluye en Mobile Foundation.

Si no necesita cifrado, JSONStore es funcionalmente completo (excepto por el cifrado) al utilizar la versión de SQLite en Mobile Foundation.

#### Sustitución de SQLite con SQLCipher para Windows Universal y Windows UWP
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. Ejecute SQLCipher para la extensión de Windows Runtime 8.1/10 que viene con SQLCipher for Windows Runtime Commercial Edition.
2. Una vez haya finalizado de la extensión, ubique la versión de SQLCipher del archivo **sqlite3.dll** que se acaba de crear. Hay uno para x86, uno para x64 y uno para ARM.

    ```bash
    C:\Archivos de programa (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
    ```
    {: codeblock}

3. Copie y sustituya este archivo con su aplicación de MobileFirst.

    ```bash
    <nombre del proyecto Worklight>\apps\<application name>\windows8\native\buildtarget\<platform>
    ```
    {: codeblock}

## Rendimiento
{: #performance-jsonstore }
Los siguientes factores pueden afectar al rendimiento de JSONStore.

### Red
{: #network-jsonstore }
* Compruebe la conectividad de red antes de realizar operaciones como, por ejemplo, enviar todos los documentos "sucios" a un adaptador.
* La cantidad de datos que se envían a través de la red a un cliente afectan en gran medida al rendimiento. Envíe solo los datos que sean necesarios para la aplicación, en lugar de copiar todo dentro de su base de datos de fondo.
* Si está utilizando un adaptador, considere establecer el distintivo compressResponse en true. De este modo, las respuestas se comprimen, lo que generalmente supone utilizar menos ancho de banda y tiene un tiempo de transferencia más bajo que sin compresión.

### Memoria
{: #memory-jsonstore }
* Cuando se utiliza la API de JavaScript, los documentos JSONStore se serializan y deserializan como series entre la capa nativa (Objective-C, Java o C#) y la capa de JavaScript. Una forma para mitigar posibles problemas de memoria es utilizar limit y offset al utilizar la API find. De esta forma, limita la cantidad de memoria asignada a los resultados y puede implementar funcionalidades como la paginación (mostrar x números de resultados por página).
* En lugar de utilizar nombres de clave largos que al final se serializan y deserializan como series, considere el correlacionar estos nombres de clave largos con otros más cortos (por ejemplo: `myVeryVeryVerLongKeyName` en `k` o `key`). Lo ideal sería correlacionarlos con nombres de clave cortos al enviarlos desde el adaptador al cliente, y correlacionarlos con los nombres de clave largos al enviar datos de nuevo a los sistemas de fondo.
* Considere dividir los datos dentro de un almacén en varias recopilaciones. Es mejor utilizar varios documentos pequeños en distintas recopilaciones en lugar de utilizar documentos monolíticos en una única recopilación. Este aspecto depende de cómo están relacionados los datos y los casos de uso para dichos datos.
* Cuando utiliza la API add con una matriz de objetos, es posible que aparezcan problemas de memoria. Para mitigar este problema, llame a estos métodos con menos objetos JSON al mismo tiempo.
* JavaScript y Java™ tienen recopiladores de basura, mientras que Objective-C utiliza ARC (Automatic Reference Counting). Deje que funcionen, pero no dependa de ellos en su totalidad. Intente asignar a null las referencias que no utilice y utilice la herramienta de creación de perfiles para comprobar que la utilización de memoria disminuye cuando espere que así ocurra.

### CPU
{: #cpu-jsonstore }
* La cantidad de campos de búsqueda y campos de búsqueda adicionales que se utilizan afectan al rendimiento cuando se llama al método add, que realiza la indexación. Indexe únicamente los valores que utilice en las consultas para el método find.
* De forma predeterminada, JSONStore realiza un seguimiento de los cambios locales a sus documentos. Este comportamiento se puede inhabilitar, ahorrando por lo tanto ciclos, estableciendo el distintivo `markDirty` en **false** al utilizar las API add, remove y replace.
* La habilitación de la seguridad añade algo de sobrecarga a las API `init` u `open`, así como a otras operaciones, que funcionan con documentos dentro de la recopilación. Considere si realmente necesita la seguridad. Por ejemplo, la API open es mucho más lenta con cifrado puesto que debe generar las claves de cifrado que se utilizarán para el cifrado y el descifrado.
* Las API `replace` y `remove` dependen del tamaño de la recopilación puesto que deben recorrerla en su totalidad para sustituir o eliminar todas las apariciones. Puesto que debe recorrer cada registro, desde descifrarlos uno a uno, lo que hace que sea más lento cuando se utiliza el cifrado. Este coste en el rendimiento es más evidente en recopilaciones grandes.
* La API `count` es relativamente costosa. Sin embargo, puede mantener una variable que mantenga el recuento de dicha recopilación. Actualícela cada vez que almacene o elimine elementos en la recopilación.
* Las API `find` (`find`, `findAll` y `findById`) se ven afectadas por el cifrado, puesto que deben descifrar cada documento para ver si hay una coincidencia. Para una búsqueda mediante una consulta, si se pasa un límite, podría ser más rápido al detenerse la consulta al alcanzar el límite de los resultados. JSONStore no necesita descifrar el resto de los documentos para averiguar si quedan otros resultados de búsqueda.

## Simultaneidad
{: #concurrency-jsonstore }
### Simultaneidad en JavaScript
{: #javascript-jsonstore }
La mayoría de las operaciones que se pueden realizar en una recopilación, como son las de añadir o buscar, son asíncronas. Estas operaciones devuelven una promesa de jQuery que se resuelve cuando la operación se completa de forma satisfactoria y se rechaza cuando se da una anomalía. Estas promesas son similares a las devoluciones de llamada de éxito y anomalías.

Un deferred de jQuery es una promesa que se puede resolver o rechazar. Los siguientes ejemplos no son específicos para JSONStore, están pensados para ayudar a entender su uso en general.

En lugar de promesas y devoluciones de llamadas, también se pueden escuchar los sucesos `success` y `failure` de JSONStore. Ejecute acciones basadas en los argumentos que se pasan al escucha de sucesos.

Definición de Promise de ejemplo:

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

Ejemplo de utilización de Promise:

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Definición de devolución de llamada de ejemplo:

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

Ejemplo de uso de devolución de llamada:

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Sucesos de ejemplo:

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```
{: codeblock}

### Simultaneidad en Objective-C
{: #objective-c-jsonstore }
Cuando se utiliza la API iOS nativa para JSONStore, todas las operaciones se añaden a la cola de asignación asíncrona. Este comportamiento asegura que las operaciones que afectan al almacén se ejecutan en orden en una hebra que no es la hebra principal. Para obtener más información, consulte la documentación de Apple en [Grand Central Dispatch (GCD) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}.

### Simultaneidad en Java
{: #java-jsonstore }
Cuando se utiliza la API Android nativa para JSONStore, todas las operaciones se ejecutan en la hebra principal. Debe crear hebras o utilizar agrupaciones de hebras para obtener un comportamiento asíncrono. Todas las operaciones de almacenamiento son de proceso múltiple.

## Analíticas
{: #analytics-jsonstore }
Puede recopilar elementos clave de información analítica relacionados con JSONStore

### Información de archivo
{: #file-information }
Si se llama a la API JSONStore con el distintivo de analíticas establecido en **true**, la información de archivo se recopila una vez por sesión de aplicación. Una sesión de aplicación se define al cargar la aplicación en memoria hasta que se elimina de la memoria. Utilice esta información para determinar el espacio que el contenido de JSONStore está utilizando en la aplicación.

### Métricas de rendimiento
{: #performance-metrics }
Las métricas de rendimiento se recopilan cada vez que se llama a una API JSONStore con información sobre los tiempos de inicio y finalización de una operación. Utilice esta información para determinar lo que pueden tardar las distintas operaciones en milisegundos.

### Ejemplo de JSONStore para iOS
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

### Ejemplo de JSONStore para Android
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

### Ejemplo de JSONStore para JavaScript
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## Cómo trabajar con datos externos
{: #working-with-external-data }
Se puede trabajar con los datos externos en diferentes conceptos: **Pull** y **Push**.

### Pull
{: #pull }
Muchos sistemas utilizan el término pull para hacer referencia a la obtención de datos de una origen externo.  
Los tres elementos más importantes son:

#### Extracción desde el origen de datos externo
{: #external-data-source-pull }
Este origen puede ser una base de datos, una API SOAP o REST, o muchos otros. El único requisito es que debe ser accesible desde el servidor de MobileFirst o directamente desde la aplicación de cliente. Lo más conveniente es que este origen de datos devuelva datos en formato JSON.

#### Capa de transporte para la extracción
{: #transport-layer-pull }
Este origen es cómo obtiene datos desde el origen externo en su origen interno, una recopilación de JSONStore dentro del almacén. Una alternativa es un adaptador.

#### API de origen de datos interno para la extracción
{: #internal-data-source-api-pull }
Este origen son las API de JSONStore que se pueden utilizar para añadir datos JSON a una recopilación.

El almacén interno se puede cumplimentar con datos que se leen desde un archivo, un campo de entrada o datos codificados mediante programación para una variable. No tienen que venir de una forma exclusiva desde un origen externo que precisa comunicación de red.
{: note}

Todos los siguientes ejemplos de código están escritos en un pseudocódigo similar a JavaScript.

Utilice adaptadores para la capa de transporte. Algunas de las ventajas de utilizar adaptadores son conversión XML a JSON, seguridad, filtrado, desacoplamiento de código del lado del cliente y de código del lado del servidor.
{: note}
##### Origen de datos externo: punto final REST de fondo
{: #external-data-source-backend-rest-endpoint-head}
Imagine que tiene un punto final REST que lee datos de una base de datos y devuelve una matriz de objetos JSON.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

Los datos que se devuelven pueden ser similares a los del siguiente ejemplo:

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

##### Capa de transporte: adaptador
{: #transport-layer-adapter}
Imagine que ha creado un adaptador que se denomina people y que ha definido un procedimiento que se denomina getPeople. El procedimiento llama al punto final REST y devuelve la matriz de objetos JSON al cliente. Es posible que aquí desee realizar más tareas, por ejemplo, devolver un subconjunto de datos al cliente.

```javascript
function getPeople() {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

En el cliente, puede utilizar la API WLResourceRequest para obtener datos. Además, podría desear pasar parámetros desde el cliente al adaptador. Un ejemplo es una fecha con la última vez que el cliente obtuvo nuevos datos del origen externo a través del adaptador.

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

Podría desear aprovecharse de `compressResponse`, `timeout` y de otros parámetros que se pueden pasar a la API `WLResourceRequest`.  
{: note}

Opcionalmente, puede omitir el adaptador y utilizar algo como jQuery.ajax para contactar directamente al punto final REST con los datos almacenados.

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

##### API de origen de datos interno: JSONStore
{: #internal-data-source-api-jsonstore}
Después de obtener una respuesta del sistema de fondo, trabaje con los datos mediante JSONStore.
JSONStore proporciona una forma de realizar un seguimiento de los cambios locales. Habilita algunas API para marcar documentos como "sucios". La API graba la última operación que se realizó en el documento, y cuándo el documento se marcó como "sucio". Puede utilizar esta información para implementar características como la sincronización de datos.

La API change toma los datos y algunas opciones:

###### replaceCriteria
{: #replacecriteria}
  
Estos campos de búsqueda son parte de los datos de entrada. Se utilizan para encontrar documentos que ya existen dentro de una recopilación. Por ejemplo, si selecciona:

```javascript
['id', 'ssn']
```
{: codeblock}

Como criterio de sustitución, pase la siguiente matriz como los datos de entrada:

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

Y la recopilación `people` ya contiene el siguiente documento:

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

La operación `change` busca un documento que coincida exactamente con la siguiente consulta:

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

A continuación, la operación `change` realiza una sustitución con los datos de entrada y la recopilación contendrá:

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

El nombre se ha cambiado de `Carlitos` a `Carlos`. Si más de un documento coincide con el criterio de sustitución, todos los documentos que coincidan son sustituidos con los respectivos datos de entrada.

###### addNew 
{: #addnew}
 
Cuando no hay documentos que coincidan con el criterio de sustitución, la API change busca los valores de este distintivo. Si el distintivo se establece en **true**, la API change crea un nuevo documento y lo añade al almacén. De lo contrario, no se realiza ninguna acción.

###### markDirty 
{: #markdirty}
 
Determina si la API change marca documentos sustituidos o añadidos como "sucios".

Se devuelve una matriz de datos desde el adaptador:

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

.then(function () {
  // ...
})
```
{: codeblock}

Puede utilizar otras API para realizar cambios en los documentos locales que se almacenan. Obtenga siempre un descriptor de acceso para la recopilación en la que desea realizar operaciones.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

A continuación, puede añadir datos (una matriz de objetos JSON) y decidir si desea marcarlos como "sucios". Normalmente, establecerá el distintivo markDirty en false cuando quiera obtener cambios del origen externo. Establezca el distintivo en true cuando quiera añadir datos de forma local.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

También puede sustituir un documento, y opcionalmente marcar el documento con las sustituciones como "sucio".

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

De forma parecida, puede eliminar un documento y opcionalmente marcar la eliminación como "sucia". Los documentos que se eliminan y marcan como sucios no aparecerán al utilizar la API find. Sin embargo, continuarán dentro de la recopilación a no ser que utilice la API `markClean`, que físicamente elimina los documentos de la recopilación. Si el documento no se marca como sucio, se elimina físicamente de la recopilación.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### Push
{: #push }
Muchos sistemas utilizan el término push para hacer referencia al envío de datos a un origen externo.

Los tres elementos más importantes son:

#### API de origen de datos interno para Push
{: #internal-data-source-api-push }
Este origen de datos es la API JSONStore que devuelve documentos únicamente con cambios locales ("sucios").

#### Capa de transporte para Push
{: #transport-layer-push }
Este origen permitirá contactar con el origen de datos externo para enviar los cambios.

#### Envío push a origen de datos externo
{: #external-data-source-push }
Este origen de datos es habitualmente una base de datos, un punto final REST o SOAP, entre otros, que recibe las actualizaciones que el cliente realiza a los datos.

Todos los siguientes ejemplos de código están escritos en un pseudocódigo similar a JavaScript.

Utilice adaptadores para la capa de transporte. Algunas de las ventajas de utilizar adaptadores son conversión XML a JSON, seguridad, filtrado, desacoplamiento de código del lado del cliente y de código del lado del servidor.
{: note}

##### API de origen de datos interno: JSONStore  
{: #internal-data-source-api-jsonstore-head}
Después de obtener un descriptor de acceso para la recopilación, llame a la API `getAllDirty` para obtener todos los elementos marcados como "sucios". Estos documentos tienen cambios únicamente locales que desea enviar al origen de datos externo a través de una capa de transporte.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

El argumento `dirtyDocs` es similar a:

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

Los campos son:
* `_id`: Campo interno que utiliza JSONStore. A cada documento se le asigna un identificador único.
* `json`: Los datos que se almacenan.
* `_operation`: La última operación realizada en el documento. Los valores posibles son add, store, replace y remove.
* `_dirty`: Indicación de fecha y hora que se almacena como un valor numérico que representa el momento en que el documento se marcó como "sucio".

##### Capa de transporte: adaptador de MobileFirst  
{: #transport-layr-mobilefirst-adapter }
Puede elegir enviar documentos "sucios" a un adaptador. Se presupone que tiene el adaptador `people` definido con un procedimiento `updatePeople`.

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

Podría desear aprovecharse de `compressResponse`, `timeout` y de otros parámetros que se pueden pasar a la API `WLResourceRequest`.
{: note}

En el servidor de MobileFirst, el adaptador tiene el procedimiento `updatePeople`, que podría ser como el del siguiente ejemplo:

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

En lugar de basarse en la salida de la API `getAllDirty` en el cliente, podría tener que actualizar la carga útil para encontrar un formato que fuese el esperado por el proceso de fondo. Podría tener que dividir las sustituciones, las eliminaciones e inclusiones en llamadas de API de fondo diferentes.

Opcionalmente, puede iterar a través de la matriz `dirtyDocs` y comprobar el campo `_operation`. A continuación, enviar sustituciones a un procedimiento, las eliminaciones a otro procedimiento y las inclusiones a otro procedimiento. El ejemplo anterior envía todos los documentos "sucios" de forma general al adaptador.

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

Opcionalmente, puede omitir el adaptador y ponerse directamente en contacto con el punto final REST.

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

##### Origen de datos externo: punto final REST de fondo
{: #external-data-source-backend-rest-endpoint }
El sistema de fondo acepta o rechaza los cambios y, a continuación, retransmite de vuelta una respuesta al cliente. Después de que el cliente inspeccione la respuesta, puede pasar a la API markClean documentos que se actualizaron.

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

Después de que los documentos se marquen como "limpios", no aparecerán en la salida de la API `getAllDirty`.

## Resolución de problemas de JSONStore
{: #troubleshooting-jsonstore }

## Proporciona información cuando solicite ayuda
{: #provide-information-when-you-ask-for-help }
Se recomienda proporcionar más información en lugar de correr riesgos y no proporcionar la suficiente. La lista siguiente es un buen punto de partida para la información que es necesaria para ayudar con los problemas de JSONStore.

* Sistema operativo y versión. Por ejemplo, Windows XP SP3 Virtual Machine o Mac OSX 10.8.3.
* Versión de Eclipse. Por ejemplo, Eclipse Indigo 3.7 Java EE.
* Versión de JDK. Por ejemplo, Java SE Runtime Environment (compilación 1.7).
* Versión de {{ site.data.keys.product }}. Por ejemplo, IBM Worklight V5.0.6 Developer Edition.
* Versión de iOS. Por ejemplo, iOS Simulator 6.1 o iPhone 4S iOS 6.0 (en desuso, consulte las características en desuso y los elementos de API).
* Versión de Android. Por ejemplo, el nivel 14 de API de Android Emulator 4.1.1 o Samsung Galaxy Android 4.0.
* Versión de Windows. Por ejemplo, Windows 8, Windows 8.1 o Windows Phone 8.1.
* Versión de adb. Por ejemplo, Android Debug Bridge versión 1.0.31.
* Los registros, como la salida de Xcode en iOS o la salida de logcat en Android.

## Intentar aislar el problema
{: #try-to-isolate-the-issue }
Siga estos pasos para aislar el problema y notificarlo de forma más precisa.

1. Restablezca el emulador (Android) o simulador (iOS) y llame a la API destroy para empezar con un sistema limpio.
2. Asegúrese de que se está ejecutando en un entorno de producción soportado.
    * Android >= emulador o dispositivo 2.3 ARM v7/ARM v8/x86
    * iOS >= simulador o dispositivo 6.0 (en desuso)
    * Simulador o dispositivo Windows 8.1/10 ARM/x86/x64
3. Intente desactivar el cifrado sin pasar una contraseña a las API init u open.
4. Busque en el archivo de base de datos SQLite que ha generado JSONStore. El cifrado debe estar desactivado.

   * Emulador de Android:

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * Simulador de iOS:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Simulador Windows 8.1 Universal / Windows 10 UWP

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

    La implementación única de JavaScript que se ejecuta en un navegador web (Firefox, Chrome, Internet Explorer) no utiliza una base de datos SQLite. El archivo se almacena en HTML5 LocalStorage.
    {: note}
   * Busque en `searchFields` con `.schema` y seleccione los datos con `SELECT * FROM <collection-name>;`. Para salir de sqlite3, escriba `.exit`. Si pasa un nombre de usuario al método init, el archivo de denomina **the-username.sqlite**. Si no pasa un nombre de usuario, el archivo se llama **jsonstore.sqlite** de forma predeterminada.
5. (solo en Android) Habilite el JSONStore detallado.

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```
    {: codeblock}

6. Utilice el depurador.

## Problemas habituales
{: #common-issues-jsonstore }
La comprensión de las siguientes características de JSONSTore puede ayudarle a resolver algunos de los problemas comunes con los que se puede encontrar.  

* La única forma de almacenar datos binarios en JSONStore es codificarlo primero en base64. Almacene los nombres de archivo o vías en lugar de los archivos reales en JSONStore.
* El acceso a los datos de JSONStore desde el código nativo solo es posible en {{ site.data.keys.v62_product_full }} V6.2.0.
* No hay límite en los datos que puede almacenar en JSONStore más allá de los límites que impone el sistema operativo móvil.
* JSONStore proporciona almacenamiento de datos persistente. No solo se almacena en la memoria.
* La API init falla cuando el nombre de la recopilación empieza con un dígito o un símbolo. IBM Worklight V5.0.6.1 y posterior devuelve un error adecuado: `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* Hay una diferencia entre un número y un entero en los campos de búsqueda. Los valores numéricos como `1` y `2` se almacenan como `1.0` y `2.0` cuando el tipo es `number`. Se almacenan como `1` y `2` cuando el tipo es `integer`.
* Si una aplicación se ve forzada a detenerse o se cuelga, siempre falla con el código de error -1 cuando la aplicación se inicia de nuevo y se llama la API `init` u `open`. Si se produce este problema, llame primero a la API `closeAll`.
* La implementación de JavaScript de JSONStore espera que el código se llame en serie. Espere a que finalice una operación antes de llamar a la siguiente.
* Las transacciones no están soportadas en aplicaciones Android 2.3.x for Cordova.
* Cuando utiliza JSONStore en un dispositivo de 64 bits, es posible que vea el siguiente error: `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* Este error significa que tiene bibliotecas nativas de 64 bits en el proyecto de Android y JSONStore no funciona actualmente cuando utiliza dichas bibliotecas. Para confirmar, vaya a **src/main/libs** o **src/main/jniLibs** en el proyecto Android y compruebe si tiene las carpetas x86_64 o arm64-v8a. Si lo hace, suprima dichas carpetas y JSONStore podrá volver a trabajar.
* En algunos casos (o entornos), el flujo entra en `wlCommonInit()` antes de que el plugin de JSONStore se inicialice. Esto provoca que las llamadas de API relacionadas con JSONStore fallen. El programa de arranque `cordova-plugin-mfp` llama a `WL.Client.init` automáticamente, lo que desencadena la función `wlCommonInit` cuando se completa. Este proceso de inicialización es distinto para el plugin de JSONStore. El plugin de JSONStore no tiene una forma de _parar_ la llamada `WL.Client.init`. En distintos entornos, puede suceder que el flujo entre en `wlCommonInit()` antes de que `mfpjsonjslloaded` se complete.
Para garantizar la solicitud de los sucesos `mfpjsonjsloaded` y `mfpjsloaded`, el desarrollador tiene la opción de llamar a `WL.CLient.init`
manualmente. Esto eliminará la necesidad de tener código específico de plataforma.

  Utilice los pasos siguientes para configurar la llamada de `WL.CLient.init` de forma manual:                             

  1. En `config.xml`, cambie la propiedad `clientCustomInit` a **true**.

  + En el archivo `index.js`:                                   
    * añada la línea siguiente al principio del archivo:                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * deje la llamada `WL.JSONStore.init` en `wlCommonInit()`                    

    * añada la función siguiente:  
    ```javascript                                         
    function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }
    ```                                                                     

Esperará al suceso `mfpjsonjsloaded` (fuera de `wlCommonInit`), lo que garantizará que el script se ha cargado y que llama posteriormente a `WL.Client.init`, que iniciará `wlCommonInit`; este llamará después a `WL.JSONStore.init`.

## Almacenar internos
{: #store-internals }
Visualice un ejemplo sobre cómo se almacenan los datos de JSONStore.

Los elementos clave de este ejemplo simplificado son:

* `_id` es el identificador único (por ejemplo, AUTO INCREMENT PRIMARY KEY).
* `json` contiene una representación exacta del objeto JSON que se almacena.
* `name` y age son campos de búsqueda.
* `key` es un campo de búsqueda adicional.

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |
{: caption="Tabla 2. Ejemplo de elementos clave de los datos de JSONStore" caption-side="top"}

Cuando realiza una búsqueda utilizando una de las consultas siguientes o una combinación de ambas: `{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}`, el documento que se devuelve es `{_id: 1, json: {name: 'carlos', age: 99} }`.

Los otros campos de JSONStore internos son:

* `_dirty`: Determina si el documento se ha marcado como modificado o no. Este campo es útil para realizar un seguimiento de los cambios en los documentos.
* `_deleted`: Marca un documento como suprimido o no suprimido. Este campo es útil para eliminar objetos de la recopilación, utilizarlos posteriormente para realizar un seguimiento de los cambios con el programa de fondo y decidir si desea eliminarlos o no.
* `_operation`: Una serie que refleja la última operación que se debe realizar en el documento (por ejemplo, sustituir).

## Errores de JSONStore
{: #jsonstore-errors }
### Errores de JavaScript
{: #javascript-errors }
JSONStore utiliza un objeto de error para devolver mensajes sobre la causa de los errores.

Cuando se produce un error durante una operación de JSONStore (por ejemplo, los métodos `find` y `add` en la clase `JSONStoreInstance`), se devuelve un objeto de error. Proporciona información sobre la causa de la anomalía.

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
No todos los pares de clave/valor forman parte de cada objeto de error. Por ejemplo, el valor de doc solo está disponible cuando la operación ha fallado debido a que un documento (por ejemplo, el método `remove` en la clase `JSONStoreInstance`) no ha podido eliminar otro documento.

### Errores de Objective-C
{: #objective-c-errors }
Todas las API que pueden fallar toman un parámetro de error que lleva una dirección a un objeto NSError. Si no desea que se le notifiquen los errores, puede pasar en `nil`. Cuando una operación falla, la dirección se llena con un NSError, que tiene un error y posible `userInfo`. La `userInfo` puede contener detalles adicionales (por ejemplo, el documento que ha provocado el error).

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Errores de Java
{: #java-errors }
Todas las llamadas de API de Java generan una excepción determinada, en función del error que se haya producido. Puede manejar cada excepción por separado o puede detectar `JSONStoreException` como término para todas las excepciones de JSONStore.

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### Lista de códigos de error
{: #list-of-error-codes }
Lista de códigos de error comunes y su descripción:

|Código de error      | Descripción |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Error no reconocido. |
| -75 OS\_SECURITY\_FAILURE | Este código de error está relacionado con el distintivo requireOperatingSystemSecurity. Se puede producir si la API destroy no puede eliminar los metadatos de seguridad protegidos por la seguridad del sistema operativo (Touch ID con recuperación tras error del código de acceso) o las API init u open no pueden localizar los metadatos de seguridad. También puede fallar si el dispositivo no ofrece soporte a la seguridad del sistema operativo, aunque haya solicitado el uso de la seguridad del sistema operativo. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore se cierra. Intente llamar al método open en la clase de JSONStore primero para habilitar el acceso al almacén. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | Ha habido un problema al retrotraer la transacción. |
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |No se puede llamar a removeCollection mientras hay una transacción en curso. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | No se puede llamar a destroy mientras hay transacciones en curso. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | No se puede llamar a closeAll mientras hay transacciones en curso. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | No se puede inicializar un almacén mientras hay transacciones en curso. |
| -43 TRANSACTION_FAILURE | Ha habido un problema con las transacciones. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | No se puede confirmar la retrotracción de una transacción cuando no hay ninguna transacción en curso. |
| -41 TRANSACTION\_IN\_POGRESS | No se puede iniciar una transacción nueva mientras haya otra transacción en curso. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Ha habido un problema con FIPS. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Se ha producido un problema al obtener información de archivo del sistema de archivos. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Problema al sustituir documentos de una recopilación. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Problema al eliminar documentos de una recopilación. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Problema al almacenar la clave de protección de datos (DPK). |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Problema al indexar datos de entrada. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Compruebe que los tipos que se pasan a searchFields sean string, integer, number o boolean. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | Una operación en una matriz de documentos, por ejemplo, el método replace, puede fallar mientras trabaja con un documento específico. El documento que ha fallado se devuelve y la transacción se retrotrae. En Android, este error se produce también al intentar utilizar JSONStore en arquitecturas no soportadas. |
| -10 ACCEPT\_CONDITION\_FAILED | La función de aceptación que el usuario ha proporcionado ha devuelto false. |
| -9 OFFSET\_WITHOUT\_LIMIT | Para utilizar un desplazamiento, también debe especificar un límite. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Error de validación, debe ser un entero positivo. |
| -7 INVALID_USERNAME | Error de validación (debe ser solo [A-Z], [a-z] o [0-9]). |
| -6 USERNAME\_MISMATCH\_DETECTED | Para cerrar sesión, un usuario de JSONStore debe llamar primero al método closeAll. Solo puede haber un usuario al mismo tiempo. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |Se ha producido un problema con el método destroy mientras intentaba suprimir el archivo que contiene el contenido del almacén. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Se ha producido un problema con el método destroy mientras intentaba borrar la cadena de claves (iOS) o las preferencias de usuario compartido (Android). |
| -3 INVALID\_KEY\_ON\_PROVISION | Se ha enviado la contraseña incorrecta a un almacén cifrado. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | Los campos de búsqueda no son dinámicos. No es posible cambiar los campos de búsqueda sin llamar los métodos destroy o removeCollection antes de llamar los métodos init u open con los nuevos campos de búsqueda. Este error se puede producir si cambia el nombre o el tipo del campo de búsqueda. Por ejemplo: {key: 'string'} a {key: 'number'} o {myKey: 'string'} a {theKey: 'string'}. |
| -1 PERSISTENT\_STORE\_FAILURE | Error genérico. Un funcionamiento incorrecto en el código nativo; lo más probable es que se haya producido en la llamada del método init. |
| 0 SUCCESS | En algunos casos, el código nativo de JSONStore devuelve 0 para indicar se ha realizado correctamente. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | Error de validación. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | Error de validación. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | Error de validación. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | Error de validación. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | Error de validación. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | Error de validación. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | Error de validación. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |La consulta que selecciona todos los documentos marcados como modificados ha fallado. Un ejemplo en SQL de la consulta sería: SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | Para utilizar las funciones como los métodos push y load en la clase JSONStoreCollection, debe pasarse un adaptador al método init. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | Error de validación |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | Error de validación |
| 12 ADAPTER_FAILURE | Se ha producido un problema en la llamada de WL.Client.invokeProcedure, específicamente en la conexión con el adaptador. Este error es distinto al error en el adaptador que intenta llamar a un programa de fondo. |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | Error de validación |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | La llamada del método de mejora en la clase JSONStoreCollection para sustituir una función existente (buscar y añadir) no está permitida. |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Push envía el documento a un adaptador, pero JSONStore no puede marcar el documento como no modificado. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | Para iniciar una recopilación con una contraseña, debe haber conectividad con el {{ site.data.keys.mf_server }}, puesto que devuelve una 'señal aleatoria segura'. IBM  Worklight V5.0.6 permite a los desarrolladores generar la señal aleatoria segura pasando de manera local {localKeyGen: true} al método init mediante el objeto de opciones. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | No se han podido cargar los datos porque WL.Client.invokeProcedure ha llamado a la devolución de llamadas de error. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | El objeto de carga que se ha pasado al método init no ha superado la validación. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | Se ha producido un problema con la clave utilizada en el objeto de carga al llamar al método de adición. |
| 20 UNDEFINED\_PUSH\_OPERATION | No se ha definido ningún procedimiento para enviar documentos modificados al servidor. Por ejemplo: el método init (nuevo documento modificado, operación = 'add') y el método push (busca el nuevo documento con la operación = 'add') se han llamado, pero no se ha encontrado ninguna clave de adición con el procedimiento de adición en el adaptador enlazado a la recopilación. El enlace de un adaptador se realiza en el método init. |
| 21 INVALID\_ADD\_INDEX\_KEY | Se ha producido un problema con los campos de búsqueda adicionales. |
| 22 INVALID\_SEARCH\_FIELD | Uno de los campos de búsqueda no es válido. Compruebe que ninguno de los campos de búsqueda que se han pasado sean _id,json,_deleted, u _operation. |
| 23 ERROR\_CLOSING\_ALL | Error genérico. Se ha producido un error al llamar el código nativo al método closeAll. |
| 24 ERROR\_CHANGING\_PASSWORD | No se puede cambiar la contraseña. La contraseña antigua era incorrecta, por ejemplo. |
| 25 ERROR\_DURING\_DESTROY | Error genérico. Se ha producido un error al llamar el código nativo al método destroy. |
| 26 ERROR\_CLEARING\_COLLECTION | Error genérico. Se ha producido un error en la llamada del código nativo al método removeCollection. |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | Error de validación. |
| 28 INVALID\_SORT\_OBJECT | La matriz proporcionada para la ordenación no es válida porque uno de los objetos JSON no es válido. La sintaxis correcta es una matriz de objetos JSON, donde cada objeto contiene únicamente una propiedad. Esta propiedad busca en el campo con el que se debe ordenar, y si es ascendente o descendente. Por ejemplo: {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | La matriz proporcionada para filtrar los resultados no es válida. La sintaxis correcta para esta matriz es una matriz de series, en la que cada serie es un campo de búsqueda o un campo JSONStore interno. Para obtener más información, consulte Almacenar internos. |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | Se produce un error de validación cuando la matriz no es únicamente una matriz de objetos JSON. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | Error de validación. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | Error de validación. |
{: caption="Tabla 3. Códigos de error comunes" caption-side="top"}
