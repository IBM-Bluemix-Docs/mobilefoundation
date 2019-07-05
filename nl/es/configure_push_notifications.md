---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

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


# Configurar notificaciones push
{: #configure_push_notifications}

Para poder enviar notificaciones push a dispositivos iOS, Android o Windows,
{{ site.data.keyword.mfserver_short_notm }} debe configurarse en primer lugar con los detalles de FCM para Android, un certificado APNS para iOS o credenciales WNS para Windows 8.1 Universal / Windows 10 UWP.
{: shortdesc}

A continuación, se pueden enviar notificaciones utilizando las opciones siguientes:
* todos los dispositivos (difusión)
* dispositivos que se han registrado en etiquetas específicas
* un único ID de dispositivo
* ID de usuario
* solo dispositivos iOS
* solo dispositivos Android
* solo dispositivos Windows
* en función del usuario autenticado.

## Configuración de notificaciones
{: #setting-up-notifications }

La habilitación del soporte de notificaciones implica seguir varios pasos de configuración tanto en la aplicación de cliente como en {{ site.data.keyword.mfserver_short_notm }}. Continúe la lectura para configurar el lado del servidor, o vaya a [Configuración del lado del cliente](#tutorials-to-follow-next).

En el lado del servidor, la configuración necesaria incluye: la configuración del proveedor que se necesita (APNS, FCM o WNS) y la correlación del ámbito "push.mobileclient".

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

Google [ha dejado en desuso GCM](https://developers.google.com/cloud-messaging/faq) y ha integrado Cloud Messaging con Firebase. Si utiliza un proyecto GCM, asegúrese de
[migrar las apps cliente de GCM en Android a FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: note}

Los dispositivos Android utilizan el servicio Firebase Cloud Messaging (FCM) para las notificaciones push.

Para configurar FCM:

1. Visite la [Consola de Firebase](https://console.firebase.google.com/?pli=1).
2. Cree un proyecto y proporcione un nombre de proyecto.
3. Pule el icono de la rueda dentada de valores y seleccione **Valores de proyecto**.
4. Pulse el separador **Cloud Messaging** para generar una **clave de API de servidor** y un **ID de remitente** y pulse **Guardar**.

También puede configurar FCM utilizando la
[API REST para el servicio Push de {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) o la
[API REST para el servicio de administración de {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi).
{: note}

Si su organización dispone de un cortafuegos que restringe el tráfico a o desde Internet, debe seguir los pasos siguientes:  
* Configurar el cortafuegos para permitir la conectividad con FCM a fin de que las apps de cliente de FCM reciban mensajes.
* Es necesario abrir los puertos 5228, 5229 y 5230. FCM normalmente solo utiliza el puerto 5228 pero, a veces, también utiliza los puertos 5229 y 5230.
* FCM no proporciona una dirección IP específica, por lo que debe permitir que el cortafuegos acepte conexiones salientes para todas las direcciones IP contenidas en los bloques de IP que se listan en la ASN 15169 de Google.
* Asegúrese de que el cortafuegos acepte conexiones salientes de {{ site.data.keyword.mfserver_short_notm }} a fcm.googleapis.com en el puerto 443.
{: note }

<img class="gifplayer" alt="Imagen de la adición de credenciales de GCM" src="images/gcm-setup.png"/>

### Servicio de notificaciones push de Apple
{: #apple-push-notifications-service }

Los dispositivos iOS utilizan APNS (Push Notification Service) de Apple para las notificaciones push.  

Para configurar APNS:

1. Genere un certificado de notificación push para el desarrollo o la producción. Para ver los pasos detallados, consulte la sección
`Para iOS` [aquí](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1).
2. En {{ site.data.keyword.mfp_oc_short_notm }} → **[su aplicación] → Push → Valores de push**, seleccione el tipo de certificado y proporcione el archivo y la contraseña del certificado. A continuación, pulse **Guardar**.

  * Para que se envíen las notificaciones push, los servidores siguientes deben ser accesibles desde una instancia de
{{ site.data.keyword.mfserver_short_notm }},
    * Servidores de recinto de pruebas:
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * Servidores de producción:
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * Durante la fase de desarrollo, utilice el archivo de certificado de recinto de pruebas apns-certificate-sandbox.p12.
  * Durante la fase de producción, utilice el archivo de certificado de producción apns-certificate-production.p12.
    * El certificado de producción de APNS solo se puede probar cuando la aplicación que lo utiliza se envía correctamente a la App Store de Apple.
    {: note }

MobileFirst no admite certificados universales.
{: note }

También puede configurar APNS utilizando la [API REST para el servicio Push de {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) o la
[API REST para el servicio de administración de {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc).
{: note}

<img class="gifplayer" alt="Imagen de la adición de credenciales de APNS" src="images/apns-setup.png"/>

### Servicio de notificaciones push de Windows
{: #windows-push-notifications-service }

Los dispositivos Windows utilizan WNS (Windows Push Notifications Service) para las notificaciones push.  

Para configurar WNS:

1. Siga las [instrucciones que Microsoft proporciona](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx) para generar los valores de **SID (Package Security Identifier)** y **Secreto de cliente**.
2. En {{ site.data.keyword.mfp_oc_short_notm }} → **[su aplicación] → Push → Valores de push**, añada estos valores y pulse **Guardar**.

También puede configurar WNS utilizando la
[API REST para el servicio Push de {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) o la
[API REST para el servicio de administración de {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc)

<img class="gifplayer" alt="Imagen de la adición de credenciales de WNS" src="images/wns-setup.png"/>

### Correlación de ámbito
{: #scope-mapping }

Correlacione el elemento de ámbito **push.mobileclient** con la aplicación.

1. Cargue {{ site.data.keyword.mfp_oc_short_notm }}, vaya a **[su aplicación] → Seguridad → Correlaciones de elementos de ámbito** y pulse en **Nueva**.
2. Especifique "push.mobileclient" en el campo **Elemento de ámbito**. A continuación, pulse **Añadir**.

Lista de ámbitos adicionales disponibles:

|**Ámbitos** | **Descripción**|
|---|---|
|apps.read | Permiso para leer recurso de aplicación. |
|apps.write | Permiso para crear, actualizar, suprimir recurso de aplicación. |
|gcmConf.read | Permiso para leer valores de configuración GCM (SenderID y clave de API). |
|gcmConf.write | Permiso para actualizar, suprimir valores de configuración GCM. |
|apnsConf.read | Permiso para leer valores de configuración de APN. |
|apnsConf.write | Permiso para actualizar, suprimir valores de configuración APN. |
|devices.read | Permiso para leer dispositivo. |
|devices.write | Permiso para crear, actualizar y suprimir dispositivo. |
|subscriptions.read | Permiso para leer suscripciones. |
|subscriptions.write | Permiso para crear, actualizar, suprimir suscripciones. |
|messages.write | Permiso para enviar notificaciones push. |
|webhooks.read | Permiso para leer notificaciones de suceso. |
|webhooks.write | Permiso para enviar notificaciones de suceso.|
|smsConf.read | Permiso para leer valores de configuración SMS.|
|smsConf.write | Permiso para actualizar, suprimir valores de configuración SMS.|
|wnsConf.read | Permiso para leer valores de configuración WNS.|
|wnsConf.write | Permiso para actualizar, suprimir valores de configuración WNS.|
{: caption="Tabla 1. Descripciones de ámbito" caption-side="top"}

<img class="gifplayer" alt="Correlación de ámbito" src="images/scope-mapping.png"/>

### Notificaciones autenticadas
{: #authenticated-notifications }

Las notificaciones autenticadas son las que se envían a uno o varios `userIds`.  

Correlacione el elemento de ámbito **push.mobileclient** para la comprobación de seguridad utilizada para la aplicación.  

1. Cargue {{ site.data.keyword.mfp_oc_short_notm }} y vaya a **[su aplicación] → Seguridad → Correlación de elementos de ámbito**, pulse en **Nuevo** o edite una entrada de correlación de ámbito existente.
2. Seleccione una comprobación de seguridad. A continuación, pulse **Añadir**.

  <img class="gifplayer" alt="Notificaciones autenticadas" src="images/authenticated-notifications.png"/>

## Definición de etiquetas
{: #defining-tags }
En {{ site.data.keyword.mfp_oc_short_notm }} → **[su aplicación] → Push → Etiquetas**, pulse **Nueva**.  
Proporcione el correspondiente `Nombre de etiqueta` y `Descripción` y pulse **Guardar**.

<img class="gifplayer" alt="Adición de etiquetas" src="images/adding-tags.png"/>

Las suscripciones se vinculan con una etiqueta y un registro de dispositivo. Cuando se anula el registro de un dispositivo desde una etiqueta, automáticamente se anula el registro de todas las suscripciones asociadas desde el propio dispositivo. En un caso de ejemplo en el que existan varios usuarios de un dispositivo, se deben implementar suscripciones en aplicaciones móviles según los criterios de inicio de sesión de usuario. Por ejemplo, realizar la llamada de suscripción después de que el usuario haya iniciado la sesión de forma satisfactoria en una aplicación y realizar de forma explícita la llamada para anular la suscripción como parte del manejo de la acción de finalización de sesión.

## Guías de aprendizaje que se han de seguir a continuación
{: #tutorials-to-follow-next }

* [Enviar notificaciones push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

Con el lado del servidor ya configurado, configure el lado del cliente y gestione las notificaciones recibidas.

* [Manejo de las notificaciones push en aplicaciones cliente](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
