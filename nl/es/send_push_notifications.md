---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, sending notification, HTTP/2

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}


# Enviar notificaciones push
{: #send_push_notifications}

Las notificaciones push se pueden enviar desde {{ site.data.keyword.mfp_oc_short_notm }} o a través de API REST.
{: shortdesc}

* Con {{ site.data.keyword.mfp_oc_short_notm }}, se pueden enviar dos tipos de notificaciones: etiqueta y difusión.
* Con las API REST, se pueden enviar todas las formas de notificaciones: etiqueta, difusión y autenticada.

## Envío de notificaciones push desde {{ site.data.keyword.mfp_oc_short_notm }}
{: #sending-push-notification-from-mobilefirst-operations-console }

Las notificaciones se pueden enviar a un único ID de dispositivo, a uno o varios ID de usuario, solo a dispositivos iOS o solo dispositivos Android, o a dispositivos suscritos a etiquetas.

### Notificaciones de etiqueta
{: #tag-notifications }

Las notificaciones de etiqueta son mensajes destinados a todos los dispositivos suscritos a una etiqueta concreta. Las etiquetas representan temas de interés para el usuario y proporcionan la posibilidad de recibir notificaciones en función del interés que haya elegido.

En {{ site.data.keyword.mfp_oc_short_notm }} → **[su aplicación] → Push → separador Enviar notificaciones**, seleccione **Dispositivos por etiquetas** desde el separador **Enviar a** y proporcione el **Texto de notificación**. A continuación, pulse **Enviar**.

<img class="gifplayer" alt="Envío por etiqueta" src="images/sending-by-tag.png"/>

### Notificaciones de difusión
{: #broadcast-notifications }

Las notificaciones de difusión son una forma de notificaciones push dirigidas a todos los dispositivos suscritos. De forma predeterminada las notificaciones están habilitadas para las aplicaciones de {{ site.data.keyword.mobilefirst_notm }} habilitadas para push mediante una suscripción a la etiqueta reservada `Push.all` (que se crea de forma automática para cada dispositivo). Es posible anular mediante programación la suscripción a la etiqueta `Push.all`.

En {{ site.data.keyword.mfp_oc_short_notm }} → **[su aplicación] → Push → separador Enviar notificaciones**, seleccione **Todo** desde el separador **Enviar a** y proporcione el **Texto de notificación**. A continuación, pulse **Enviar**.

<img class="gifplayer" alt="Envío a todos" src="images/sending-to-all.png"/>

## Envío de notificaciones push utilizando API REST
{: #sending-push-notifications-using-rest-apis }

Al utilizar las API REST para enviar notificaciones, se pueden enviar todas las formas de notificación: notificaciones de etiqueta y difusión, y notificaciones autenticadas.

Para enviar una notificación, se realiza una solicitud utilizando POST al punto final REST: `imfpush/v1/apps/<application-identifier>/messages`.  
A continuación se muestra un URL de ejemplo,

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

Para revisar todas las API REST de notificaciones push, consulte el tema [API REST de servicios de tiempo de ejecución](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html) en la documentación de usuario.

### Carga útil de notificación
{: #notification-payload }

La solicitud puede contener las siguientes propiedades de carga útil:

|Propiedades de carga útil| Definición
|--- | ---
|message | Mensaje de alerta a enviar. |
|settings | Los valores son los distintos atributos de la notificación. |
|target | Conjunto de destinos: etiquetas, plataformas, dispositivos o ID de consumidor. Solo se puede especificar uno de los destinos. |
|deviceIds | Matriz de los dispositivos representados por los identificadores de dispositivo. Los dispositivos con estos identificadores recibirán la notificación. Se trata de una notificación de difusión única. |
|notificationType | Valor entero que indica el canal (push o SMS) utilizado para enviar el mensaje. Los valores permitidos son 1 (solo push), 2 (solo SMS) y 3 (ambos). |
|platforms | Matriz de plataformas de dispositivo. Los dispositivos que se ejecuten en estas plataformas recibirán la notificación. Los valores soportados son (Apple/iOS), G (Google/Android) y M (Microsoft/Windows). |
|tagNames | Matriz de etiquetas que se especifican como tagNames. Los dispositivos suscritos a estas etiquetas recibirán la notificación. Utilice este tipo de destino para notificaciones basadas en etiquetas. |
|userIds | Matriz de usuarios representados por sus userIds para enviar la notificación. Se trata de una notificación de difusión única. |
|phoneNumber | Número de teléfono utilizado para registrar el dispositivo y recibir notificaciones. Se trata de una notificación de difusión única. |
{: caption="Tabla 1. Propiedades de la carga útil" caption-side="top"}

**Ejemplo de JSON de carga útil de notificaciones push**

```json
{
    "message" : {
    "alert" : "Test message",
  },
  "settings" : {
    "apns" : {
      "badge" : 1,
      "iosActionKey" : "Ok",
      "payload" : "",
      "sound" : "song.mp3",
      "type" : "SILENT",
    },
    "gcm" : {
      "delayWhileIdle" : ,
      "payload" : "",
      "sound" : "song.mp3",
      "timeToLive" : ,
    },
  },
  "target" : {
    // The following list is for demonstration purposes only - per the documentation only 1 target is allowed to be used at a time.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**Ejemplo de JSON de carga útil de notificación SMS**

```json
{
  "message": {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds" : ["38cc1c62-03bb-36d8-be8e-af165e671cf4"]
   }
}
```

## Envío de la notificación
{: #sending-the-notification }

La notificación se puede enviar utilizando distintas herramientas. A efectos de prueba, se utilizará **Postman**; en los pasos siguientes se describe la configuración,

1. [Configuración de un cliente confidencial](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
  El envío de una notificación push a través de la API REST utiliza elementos de ámbito separados por espacios `messages.write` y `push.application.<applicationId>`.
  <img class="gifplayer" alt="Configurar un cliente confidencial" src="images/push-confidential-client.png"/>
2. [Creación de una señal de acceso](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token).  
3. Realice una solicitud **POST** a **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages**
  - Si está utilizando una instancia remota de {{ site.data.keyword.mobilefirst_notm }}, sustituya los valores de `hostname` y `port` con los suyos propios.
  - Actualice el valor del identificador de aplicación con el suyo propio.
4. Establezca una cabecera:
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - Sustituya el valor que hay tras **Bearer** por el valor de su señal de acceso del paso (1).
    ![Cabecera Authorization](images/postman_authorization_header.png "Cabecera Authorization")
5. Establezca un cuerpo:
  - Actualice sus propiedades según se describe en [Carga útil de notificación](#notification-payload).
  - Por ejemplo, añadiendo la propiedad **target** con el atributo **userIds**, podrá enviar una notificación a usuarios específicamente registrados.
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![Cuerpo de Authorization](images/postman_json.png "Cuerpo de Authorization")

    Tras pulsar el botón **Enviar**, el dispositivo recibirá una notificación:

    ![Imagen de la aplicación de ejemplo](images/notifications-app.png "Notificación push en un dispositivo móvil")

## Personalización de notificaciones
{: #customizing-notifications }

Antes de enviar el mensaje de notificación, también puede personalizar los atributos de notificación siguientes.  

En {{ site.data.keyword.mfp_oc_short_notm }} → **[su aplicación] → Push → Etiquetas → separador Enviar notificaciones**, amplíe la sección **Valores personalizados de iOS/Android** para cambiar los atributos de notificación.

### Android
{: #android }

* Entre otras, sonido de notificación, tiempo que permanece almacenada una notificación en el almacenamiento FCM o carga útil personalizada.
* Si desea cambiar el título de la notificación, añada `push_notification_tile` en el archivo **strings.xml** del proyecto.

### iOS
{: #ios }

* Sonido de notificación, carga útil personalizada, título de clave de acción, tipo de notificación y número de identificador.

  ![Personalización de notificaciones push](images/customizing-push-notifications.png "Página de push de la consola de operaciones de Mobile First con el separador Enviar push seleccionado")

## Soporte de HTTP/2 para notificaciones push de APN
{: #http2-support-for-apns-push-notifications}

El servicio Apple Push Notification (APNs) admite una nueva API basada en el protocolo de red HTTP/2. El soporte para HTTP/2 proporciona muchas ventajas, incluyendo las siguientes,

* Longitud de mensaje aumentada de 2 KB a 4 KB, que permite añadir contenido adicional a las notificaciones.
* Se elimina la necesidad de establecer varias conexiones entre cliente y servidor, lo que mejora el rendimiento.
* Soporte para certificados SSL de cliente de notificación push universal.

>Las notificaciones push en {{ site.data.keyword.mobilefirst_notm }} ahora admiten notificaciones push APN basadas en HTTP/2 junto con las notificaciones basadas en socket TCP heredadas.

### Habilitación de HTTP/2
{: #enabling-http2}

Las notificaciones basadas en HTTP/2 se pueden habilitar utilizando una propiedad JNDI.
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

Si se añade la propiedad JNDI, no se utilizarán las notificaciones heredadas basadas en socket TCP, y solo estarán habilitadas las notificaciones basadas en HTTP/2.
{: note}

### Soporte de proxy para HTTP/2
{: #proxy-support-for-http2}

Las notificaciones basadas en HTTP/2 se pueden enviar a través de un proxy HTTP. Para habilitar el direccionamiento de las notificaciones a través de un proxy, acceda [aquí](#proxy-support).

## Soporte de proxy
{: #proxy-support }
Utilice los valores proxy para establecer el proxy opcional a través de las notificaciones que se envían a dispositivos Android e iOS. El proxy se configura mediante las propiedades de configuración `push.apns.proxy.*` y `push.gcm.proxy.*`. Para obtener más información, consulte
[Lista de propiedades JNDI para el servicio push de {{ site.data.keyword.mfserver_short_notm }}](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

## Siguientes pasos
{: #next-tutorial-to-follow }
Con el lado del servidor ya configurado, configure el lado del cliente y gestione las notificaciones recibidas utilizando la guía de aprendizaje siguiente.

* [Manejo de las notificaciones push en aplicaciones cliente](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
