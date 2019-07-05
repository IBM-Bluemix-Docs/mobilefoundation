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

# Impedir ataques de suplantación de identidad
{: #prevent_man_in_the_middle_attack}


Al comunicar con redes públicas, es esencial enviar y recibir información de forma segura. El protocolo más usado para asegurar estas comunicaciones es SSL/TLS. SSL/TLS utiliza certificados digitales para proporcionar autenticación y cifrado. Estos protocolos se basan en la verificación de cadenas de certificado y son vulnerables a un número de ataques peligrosos, incluidos los ataques de suplantación de identidad, que se producen cuando una parte no autorizada tiene la capacidad de visualizar y modificar todo el tráfico que pasa entre el dispositivo móvil y los sistemas de fondo.
{: shortdesc}

Reduzca los ataques de seguridad mediante la [Fijación de certificados](#cert_pinning) e impida los ataques de suplantación de identidad (MitM).

Esta característica es opcional y solo está disponible en los planes profesionales.
{: note}

El proceso de fijación de certificados consta de los pasos siguientes:

1. Realice la [Configuración del certificado](#certsetup).
2. Configure el código de cliente para llamar al método correcto de [API de fijación de certificados](#certpinapi) antes de realizar una solicitud protegida.

Durante el reconocimiento SSL (primera solicitud al servidor), el SDK de cliente de Mobile Foundation verifica que la clave pública del certificado de servidor coincide con la clave pública del certificado que se almacena en la app.

Debe utilizar un certificado obtenido de una entidad emisora de certificados. **No se da soporte** a los certificados autofirmados.
{: note}

## Fijación de certificados
{: #cert_pinning}

Los protocolos que se basan en la verificación de cadenas de certificado como, por ejemplo, SSL/TLS, son vulnerables a un número de ataques peligrosos, incluidos los ataques de suplantación de identidad, que se producen cuando una parte no autorizada puede visualizar y modificar todo el tráfico que pasa entre el dispositivo móvil y los sistemas de fondo. Para asegurar que un certificado es genuino o válido, un certificado raíz perteneciente a la autoridad emisora de certificados (CA) de confianza lo firma digitalmente. Los sistemas operativos y navegadores mantienen listas de certificados raíz CA de confianza para que puedan verificar fácilmente los certificados que los CA han emitido y firmado.

IBM Mobile Foundation proporciona una API para habilitar la **fijación de certificados**. Está soportada en aplicaciones de iOS nativas, de Android nativas y de Cordova MobileFirst de varias plataformas.

## Proceso de fijación de certificados
{: #certpinprocess}

La fijación de certificados es el proceso de asociar un host con la clave pública esperada que le corresponde. Puede configurar el código de cliente para que acepte sólo un certificado específico para el nombre de dominio, en lugar de cualquier certificado que corresponda a un certificado raíz de CA de confianza reconocido por el sistema operativo o el navegador. Una copia del certificado se coloca en la aplicación cliente. Durante el reconocimiento SSL (primera solicitud al servidor), el SDK de cliente de MobileFirst verifica que la clave pública del certificado de servidor coincide con la clave pública del certificado que se almacena en la app.

También puede fijar varios certificados con su aplicación cliente. Coloque una copia de todos los certificados en la aplicación cliente. Durante el reconocimiento SSL (primera solicitud al servidor), el SDK de cliente de MobileFirst verifica que la clave pública del certificado de servidor coincide con la clave pública de uno de los certificados que se almacenan en la app.
{: note}

* **Importante**
    * Es posible que algunos sistemas operativos móviles guarden en caché el resultado de la comprobación de la validación de certificado. Por lo tanto, el código llama al método de API de fijación de certificados **antes** de realizar una solicitud protegida. De lo contrario, es posible que cualquier solicitud posterior omita la validación de certificado y la comprobación de fijación.
    * Asegúrese de utilizar sólo las API de Mobile Foundation para todas las comunicaciones con el host relacionado, incluso después de la fijación de certificados. Es posible que la utilización de API de terceros para interaccionar con el mismo host provoque comportamientos inesperados como, por ejemplo, que un sistema operativo móvil almacene en memoria caché un certificado no verificado.
    * Si se vuelve a llamar el método API de fijación de certificados, se sustituye la operación de fijación previa.

Si el proceso de fijación se realiza correctamente, la clave pública incluida en el certificado indicado se utiliza para verificar la integridad del certificado del servidor de MobileFirst durante el reconocimiento SSL/TLS de solicitud asegurada. Si el proceso de fijación falla, la aplicación de cliente rechaza todas las solicitudes SSL/TLS que se hagan al servidor.

## Configuración del certificado
{: #certsetup}

Debe utilizar un certificado obtenido de una entidad emisora de certificados. **No se da soporte** a los certificados autofirmados. Para obtener compatibilidad con los entornos soportados, asegúrese de que el certificado está codificado en formato **DER** (Reglas de codificación distinguida, tal y como se define en la Unión Internacional de Telecomunicaciones X.690 estándar).

El certificado se debe colocar tanto en el servidor de MobileFirst como en la aplicación. Coloque el certificado de la siguiente manera,

* En el servidor de MobileFirst (WebSphere Application Server, WebSphere Application Server Liberty o Apache Tomcat), consulte la documentación del servidor de aplicaciones específico para obtener información sobre cómo configurar SSL/TLS y certificados.
* En la aplicación,
    * Para iOS nativo, añada el certificado en el **paquete** de aplicación
    * Para Android nativo, coloque el certificado en la carpeta de **activos**
    * Para Cordova, coloque el certificado en la carpeta **app-name\www\certificates** (si la carpeta no existe, créela)

## API de fijación de certificados
{: #certpinapi}

La fijación de certificados consiste en el siguiente método API de sobrecarga, en el que un método tiene un parámetro ``certificateFilename``, en el que ``certificateFilename`` es el nombre del archivo de certificado, y el segundo método tiene un parámetro ``certificateFilenames``, en el que ``certificateFilenames`` es una matriz de los nombres de los archivos de certificado.

### Android
{: #certpinapiandroid}

* Certificado único:

    Sintaxis: pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

    Ejemplo:

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* Varios certificados:

    Sintaxis: pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    Ejemplo:

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

El método de fijación de certificados provocará una excepción en dos casos:

* El archivo no existe
* El formato del archivo no es correcto

### iOS
{: #certpinapiios}

* Fijación de certificado único:

    Sintaxis: pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    El método de fijación de certificados provocará una excepción en dos casos:

    * El archivo no existe
    * El formato del archivo no es correcto

* Fijación de varios certificados

    Sintaxis: pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    El método de fijación de certificados provocará una excepción en dos casos:

    * No existe ninguno de los archivos de certificado
    * El formato de los archivos de certificado no es correcto

**En Objective-C**:

Ejemplo: Certificado único:

```objectc
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

Ejemplo de varios certificados:

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**En Swift**:

Ejemplo: Certificado único:

```swift
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

Ejemplo de varios certificados:

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

El método de fijación de certificados provocará una excepción en dos casos:

* El archivo no existe
* El formato del archivo no es correcto

### Cordova
{: #certpinapicordova}

* Fijación de certificado único:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* Fijación de varios certificados:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

El método de fijación de certificados devuelve una promesa:

* El método de fijación de certificados llama al método `onSuccess` si la fijación se realiza correctamente.
* El método de fijación de certificados desencadena la devolución de llamada `onFailure` en los dos casos siguientes,
  * El archivo no existe.
  * El formato del archivo no es correcto.

A continuación, si se realiza una solicitud asegurada al servidor cuyo certificado no está fijado, se llama a la devolución de llamada `onFailure` de la solicitud específica (por ejemplo, `obtainAccessToken` o `WLResourceRequest`).

Obtenga más información sobre el método de API de fijación de certificados en la [Referencia de API](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks).
{: note}
