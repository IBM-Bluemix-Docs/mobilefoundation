---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Push Notifications, notifications, unicast notifications, tag notifications, interactive notifications, silent notifications, configure DataPower

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

# Notificaciones push
{: #push_notifications}

Las notificaciones permiten que un dispositivo móvil reciba mensajes de tipo "push" que se envían desde un servidor. Se reciben notificaciones independientemente de si la aplicación se está ejecutando en primer plano o en segundo plano.  
{: shortdesc}

{{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} proporciona un conjunto unificado de métodos de API para enviar notificaciones push a aplicaciones iOS, Android, Windows 8.1 Universal, Windows 10 UWP y Cordova (iOS, Android). Las notificaciones se envían desde {{ site.data.keyword.mfserver_short_notm }} a la infraestructura de proveedor (Apple, Google, Microsoft, pasarelas SMS) y de allí a los dispositivos pertinentes. El mecanismo de notificaciones unificadas hace que todo el proceso de comunicación con usuarios y dispositivos sea transparente para el desarrollador.

## Soporte de dispositivos
{: #device-support}
Se admite la notificación push para las plataformas siguientes en {{ site.data.keyword.mobilefoundation_short }}:

* iOS 8.x o posterior
* Android 4.x o posterior
* Windows 8.1, Windows 10

## Formas de notificación push
{: #push-notifications-forms}
Las notificaciones pueden ser de distintos tipos:

* **Alerta** (iOS, Android, Windows) - mensaje de texto emergente
* **Sonido** (iOS, Android, Windows) - archivo de sonido que se reproduce cuando se recibe una notificación
* **Identificador** (iOS), Mosaico (Windows) - representación gráfica que permite un breve texto o una imagen
* **Banner** (iOS), Toast (Windows) - un mensaje de texto emergente que desaparece en la pantalla del dispositivo
* **Interactiva** (iOS 8 y superior) - botones de acción dentro del banner de una notificación recibida
* **Silenciosa** (iOS 8 y superior) - envío de notificaciones sin molestar al usuario

## Tipos de notificación push
{: #push-notification-types }

### Notificaciones de etiqueta
{: #tag-notifications-overview}

Las notificaciones de etiqueta son mensajes destinados a todos los dispositivos suscritos a una etiqueta concreta.  

Las notificaciones basadas en código permiten la segmentación de notificaciones en función de los temas o de las áreas de temas. Los destinatarios de las notificaciones pueden elegir recibirlas únicamente si tratan sobre un tema o asunto de su interés. Por lo tanto, las notificaciones basadas en etiquetas proporcionan un medio de segmentar los destinatarios. Con esta característica, puede definir etiquetas y enviar o recibir mensajes por etiquetas. El mensaje únicamente se dirige a los dispositivos que se han suscrito a una etiqueta.

### Notificaciones de difusión
{: #broadcast-notifications-overview}

Las notificaciones de difusión son una forma de notificaciones push dirigidas a todos los dispositivos suscritos. De forma predeterminada están dirigidas a cualquier aplicación de {{ site.data.keyword.mobilefirst_notm }} habilitada para push mediante una suscripción a la etiqueta reservada `Push.all` (que se crea de forma automática para cada dispositivo). Las notificaciones de difusión se pueden inhabilitar anulando la suscripción desde la etiqueta reservada `Push.all`.

### Notificaciones de difusión única
{: #unicast-notifications-overview}

Las notificaciones de difusión única, o notificaciones autenticadas de usuario, se protegen con OAuth. Estos mensajes de notificación están dirigidos a un dispositivo o un ID de usuario concreto. El ID de usuario en la suscripción de usuarios puede provenir del contexto de seguridad subyacente.

### Notificaciones interactivas
{: #interactive-notifications-overview}

Con una notificación interactiva, cuando llega una notificación, los usuarios pueden actuar sin abrir la aplicación. Cuando llega una notificación interactiva, el dispositivo muestra los botones de la acción junto con el mensaje de notificación. Actualmente, las notificaciones interactivas están soportadas en dispositivos en iOS versión 8 y posterior. Si se envía una notificación interactiva a un dispositivo iOS con una versión anterior a la versión 8, las acciones de la notificación no se visualizarán.

Aprenda a manejar [Notificaciones interactivas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications).

### Notificaciones silenciosas
{: #silent-notifications-overview}

Las notificaciones silenciosas son notificaciones que no visualizan alertas ni interrumpen al usuario. Cuando llega una notificación silenciosa, la aplicación que se encarga del código lo ejecuta en un segundo plano sin llevar la aplicación a un primer plano. Actualmente, se da soporte a las instalaciones silenciosas en dispositivos iOS a partir de la versión 7. Si se envía una notificación silenciosa a dispositivos iOS con una versión anterior a la 7, si la aplicación se ejecuta en un segundo plano, se omite la notificación. Si la aplicación se está ejecutando en un primer plano, se invoca al método de llamada de retorno de notificación.

Aprenda a gestionar [Notificaciones silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications).

Las notificaciones de difusión única no contienen ninguna etiqueta en la carga útil. El mensaje de notificación puede dirigirse a varios dispositivos o usuarios especificando varios deviceID o userID respectivamente, en el bloque de destino de la API del mensaje POST.
{: note}

## Valores de proxy
{: #proxy-settings}

Utilice los valores de proxy para establecer el proxy opcional a través de la cual se enviarán notificaciones a APNS y FCM. El proxy se configura mediante las propiedades de configuración **push.apns.proxy.* ** y **push.gcm.proxy.***. Para obtener más información, consulte [Lista de propiedades JNDI para el servicio push de {{ site.data.keyword.mfserver_short_notm }}](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

WNS no tiene soporte de proxy.
{: note}

### Utilización del DataPower de WebSphere como punto final de notificación de push
{: #proxy-settings-datapower-endpoint}

Puede configurar DataPower para aceptar solicitudes de notificación de {{ site.data.keyword.mfserver_short_notm }} y redirigirlas a FCM y WNS.

No hay soporte para APN.
{: note}

#### Configuración de {{ site.data.keyword.mfserver_short_notm }}
{: #proxy-settings-config-mfp-server}

En `server.xml`, configure la propiedad JNDI siguiente.
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

donde `host` es el nombre de host de DataPower y `port` es el número de puerto en el que se configura HTTPS Front Side Handler para FCM y WNS.

#### Configuración de DataPower
{: #proxy-settings-config-datapower}

1. Inicie sesión en el dispositivo de DataPower.
2. Vaya a **Servicios > Pasarela multiprotocolo > Nueva pasarela multiprotocolo**.
3. Proporcione un nombre con el que pueda identificar la configuración.
4. Seleccione la política de pasarela multiprotocolo del gestor XML como valor predeterminado y la política de reescritura de URL a ninguna.
5. Seleccione el botón de selección **programa de fondo estático** y seleccione cualquiera de las siguientes opciones para **establecer el URL del programa de fondo predeterminado**:
    - Para FCM,	`https://gcm-http.googleapis.com`
    - Para WNS,	`https://hk2.notify.windows.com`
6. Seleccione el tipo de respuesta y el tipo de solicitud como paso a través.

### Generación de un certificado
{: #proxy-settings-generate-cert}

Para generar un certificado, elija una de las opciones siguientes:

* Para FCM:
    1. Desde la línea de mandatos, emita `Openssl` para obtener los certificados de FCM.
    2. Ejecute el mandato siguiente:
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
        {: codeblock}
    3. Copie el contenido de *-----BEGIN CERTIFICATE-----  a -----END CERTIFICATE-----* y guárdelo en un archivo con la extensión
`.pem`.

* Para WNS:
    1. Desde la línea de mandatos, utilice `Openssl` para obtener los certificados de WNS.
    2. Ejecute el mandato siguiente:
        ```
        openssl s_client -connect https://hk2.notify.windows.com:443
        ```
        {: codeblock}
    3. Copie el contenido de *-----BEGIN CERTIFICATE-----  a -----END CERTIFICATE-----* y guárdelo en un archivo con la extensión `.pem`.

### Valores de backside
{: #proxy-settings-backside-settings}

* Para FCM y WNS:
    1. Cree un certificado criptográfico.
        a. Vaya a **Objetos > Configuración criptográfica y pulse Certificado criptográfico**.
        b. Proporcione un nombre con el que pueda identificar el certificado criptográfico.
        c. Pulse **Cargar** para cargar el certificado FCM generado.
        d. Defina el **Alias de contraseña** en ninguno.
        e. Pulse **Generar clave**.
        ![Configurar certificado criptográfico](images/bck_1.gif "Configurar certificado criptográfico")

    2. Cree una credencial de validación criptográfica.
        a. Vaya a Objetos > Configuración criptográfica** y pulse **Credencial de validación criptográfica**.
        b. Proporcione un nombre exclusivo.
        c. En Certificados, seleccione el certificado criptográfico que haya creado en el paso anterior (paso 1).
        d. Para **Modalidad de validación de certificado**, seleccione Coincidir con el certificado exacto o emisor inmediato.
        e. Pulse **Aplicar**.

        ![Configurar credenciales de validación criptográfica](images/bck_2.gif "Configurar credenciales de validación criptográfica")

    3. Cree una credencial de validación criptográfica:
        a. Vaya a **Objetos > Configuración criptográfica** y pulse **Perfil criptográfico**.
        b. Pulse **Añadir**.
        c. Proporcione un nombre exclusivo.
        d. En **Credenciales de validación**, seleccione la credencial de validación creada en el paso anterior (paso 2 del menú desplegable) y establezca las credenciales de identificación en **ninguna**.
        e. Pulse **Aplicar**.

        ![Configurar perfil criptográfico](images/bck_3.gif "Configurar perfil criptográfico")

    4. Crear un perfil de proxy SSL:

        a. Vaya a **Objetos** > **Configuración criptográfica** >, **Perfil de proxy SSL**.
        b. Elija una de las opciones siguientes:

            - Para SMS, seleccione ninguno en **Perfil de proxy SSL**.
            - Para FCM y WNS con un URL de programa de fondo seguro (HTTPS), complete los siguientes pasos:
                i. Pulse **Añadir**.
                ii. Proporcione un nombre con el que pueda identificar el perfil de proxy SSL más tarde.
                iii. Seleccione **Dirección SSL** como **Reenvío** en el desplegable.
                iv. Para el perfil criptográfico de reenvío (Cliente), seleccione el perfil criptográfico creado en el paso 3.
                v. Pulse **Aplicar**.

        ![Configurar perfil de proxy SSL](images/bck_4.gif "Configurar perfil de proxy SSL (en desuso)")

    5. En la ventana Pasarela multiprotocolo, en **Valores de backside**, seleccione **Perfil de proxy** como **Tipo de cliente SSL** y seleccione el perfil de proxy SSL creado en el paso 4.

        ![Configurar perfil de proxy SSL](images/bck_5.gif "Ventana Pasarela multiprotocolo")

### Valores de frontside
{: #proxy-settings-frontside-settings}

* Para FCM y WNS:

    1. Cree un par clave-certificado con el nombre de host de DataPower como valor de nombre común:
        a. Vaya a **Administración** > **Varios** y pulse **Herramientas criptográficas**.
        b. Especifique el nombre de host de DataPower como el valor del nombre común (CN).
        c. Seleccione **Exportar clave privada** si tiene pensado exportar la clave privada más adelante, y pulse **Generar clave**.

        ![Creación de un par de clave-certificado](images/frnt_1.gif "Creación de un par de clave-certificado")

    2. Cree una credencial de identificación criptográfica:
        a. Vaya a **Objetos** > **Configuración criptográfica** y pulse **Credencial de identificación criptográfica**.
        b. Pulse **Añadir**.
        c. Proporcione un nombre exclusivo.
        d. Para el certificado y la clave criptográfica, seleccione el certificado y la clave que se han generado en el paso anterior (paso 1) en el recuadro de lista.
        e. Pulse **Aplicar**.

        ![Creación de una credencial de identificación criptográfica](images/frnt_2.gif "Creación de una credencial de identificación criptográfica")

    3. Cree un perfil criptográfico:

        a. Vaya a **Objetos** > **Configuración criptográfica** y pulse **Perfil criptográfico**.
        b. Pulse **Añadir**.
        c. Proporcione un nombre exclusivo.
        d. Para las credenciales de identificación, seleccione la credencial de identificación creada en el paso anterior (paso 2) en el recuadro de lista. Establezca las credenciales de validación en ninguna.
        e. Pulse **Aplicar**.

        ![Configurar perfil criptográfico](images/frnt_3.gif "Configurar perfil criptográfico")

    4. Crear un perfil de proxy SSL:

        a. Vaya a **Objetos** > **Configuración criptográfica** >, **Perfil de proxy SSL**.
        b. Pulse **Añadir**.
        c. Proporcione un nombre exclusivo.
        d. Seleccione la dirección SSL como **Inversa** en el recuadro de lista.
        e. Para el perfil criptográfico inverso (servidor), seleccione el perfil criptográfico que se ha creado en el paso anterior (paso 3).  
        f. Pulse **Aplicar**.

        ![Configurar perfil de proxy SSL](images/frnt_4.gif "Configurar perfil de proxy SSL")

    5. Cree un controlador de frontside de HTTPS:

        a. Vaya a **Objetos** > **Controladores de protocolo** > **controlador de frontside de HTTPS**.
        b. Pulse **Añadir**.
        c. Proporcione un nombre exclusivo.
        d. Para **Dirección IP local**, seleccione el alias correcto o déjelo en el valor predeterminado (0.0.0.0).
        e. Proporcione un puerto disponible.
        f. Para **Versiones y métodos permitidos**, seleccione HTTP 1.0, HTTP 1.1, método POST, método GET, URL con ?, URL con #, URL con ..
        g. Seleccione **Perfil proxy** como tipo de servidor SSL.
        h. Para el perfil de proxy SSL (en desuso), seleccione el perfil de proxy SSL creado en el paso anterior (paso 4).
        i. Pulse **Aplicar**.

        ![Configurar controlador de frontside de HTTPS](images/frnt_5.gif "Configurar controlador de frontside de HTTPS")

    6. En la página Configurar la pasarela multiprotocolo en **Valores de frontside**, seleccione el controlador de Frontside de https como **Protocolo FrontSide**, creado en el paso 5, y pulse **Aplicar**.

        ![Configuración general](images/frnt_6.gif "Configuración general")

    El certificado que está utilizando el DataPower en los valores de frontside, es uno autofirmado. Las conexiones con DataPower fallarán a menos que se añada el certificado en el almacén de claves JRE utilizado por {{ site.data.keyword.mobilefirst_notm }}.

## Siguientes pasos
{: #next-steps }

Siga los pasos de la configuración necesaria de lado del servidor y el lado del cliente para poder enviar y recibir notificaciones push:

* [Configurar notificaciones push](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [Enviar notificaciones push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [Manejo de las notificaciones push en aplicaciones cliente](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [Notificaciones silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notificaciones interactivas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
