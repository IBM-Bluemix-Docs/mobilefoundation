---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, set up alerts, alert definitions

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configurar alertas
{: #set_up_alerts}

Mobile Analytics proporciona la característica Alertas, que le permite definir alertas para supervisar las aplicaciones. Puede configurar umbrales, que si se sobrepasan, desencadenan alertas para notificar al supervisor de la consola de Mobile Analytics. Las alertas desencadenadas se pueden visualizar en la consola o se pueden configurar para que se retransmitan a un webhook personalizado para su manejo adecuado.

Esta característica proporciona los medios para detectar y alertar infracciones de umbral observadas en los registros de errores de la aplicación, bloqueos de aplicación y transacciones de red. Las alertas y los umbrales reactivos aportan agilidad en las respuestas a las condiciones de rendimiento de las aplicaciones sin tener que supervisar continuamente las vistas/gráficos correspondientes.

Esta característica está en versión BETA.
{: note}

En las secciones siguientes se describe la creación, la gestión de alertas y su visualización en la consola de Mobile Analytics.

## Creación de una definición de alertas para los registros de aplicaciones (App Logs)
{: #creating_alert_def}

Puede crear una definición de alerta basada en los registros de apps.  Por ejemplo, si desea supervisar los registros de su aplicación cada 5 minutos para comprobar si la aplicación de una versión específica ha registrado errores más de tres veces, a continuación se indica cómo puede configurar esto.

1.  En la consola de Mobile Analytics, pulse **Definiciones** para ir a la página de definiciones de alertas.
2.  Pulse **Crear alerta**.
3.  Indique los siguientes valores:
    * **Nombre de alerta**: *Alerta para registros de apps*
    * **Mensaje**: *Alerta de mensaje de error*
    * **Frecuencia de consulta**: *5 minutos*
    * **Tipo de suceso**: *Registros de apps*
        * **Propiedad**: *Nivel de registro*
            * **Valor**: *Error*
            * **Umbral**: *Total para la instancia de aplicación*<br/>
Si selecciona la opción *Promedio para aplicación*, el promedio de los registros de apps se realiza por el número de dispositivos. Por ejemplo, si tiene dos dispositivos y un dispositivo envía seis registros de apps mientras que el otro dispositivo envía tres registros de apps, el promedio es de 4,5 registros de apps.
              {: note}
            * **Operador**: *es mayor que o igual a*
            * Valor de umbral: *3*
4.  Pulse **Siguiente** y proporcione el siguiente valor:
    * **Método**: *Solo para la consola de Analytics*<br/>
Ademas, elija la opción *Consola de Analytics y POST de red*, si también desea enviar un mensaje POST con una carga útil JSON a su URL personalizado. Los campos siguientes están disponibles si selecciona esta opción.
      * **URL POST de red**
      * **Cabeceras**
      * **Tipo de autenticación**
      * **Cuerpo de la solicitud POST**
5. Pulse **Guardar**.  

Ahora ha creado una definición de alerta para desencadenar una alerta al final de cada intervalo de 5 minutos cuando el número de registros de apps alcance el umbral de 3 o más registros de errores.

Esta alerta se mantiene en directo y activa, supervisando por la frecuencia de configuración hasta que la definición de alerta se inhabilita o se suprime.
{: note}

## Creación de una definición de alerta para bloqueos de aplicaciones
{: #creating_alert_crashes}

A continuación se muestra un ejemplo de configuración de alertas sobre bloqueos de aplicaciones.  Esta alerta supervisa cada 2 minutos todos los bloqueos de aplicaciones y desencadena una alerta si el número de bloqueos observados supera un recuento de 5.

1.  En la consola de Mobile Analytics, pulse **Definiciones** para ir a la página de definiciones de alertas.
2.  Pulse **Crear alerta**.
3.  Indique los siguientes valores:
    * **Nombre de alerta**: *Alerta para bloqueos de aplicaciones*
    * **Mensaje**: *Alerta de bloqueo de app*
    * **Frecuencia de consulta**: *2 minutos*
    * **Tipo de suceso**: *Bloqueos de aplicaciones*
        * **Nombre de la aplicación**: *Cualquier aplicación*
        * **Cualquier aplicación**: *Cualquier versión*
    * **Umbral**: *Recuento de bloqueos*
    * **Operador**: *es mayor que o igual a*
    * Valor de umbral: *5*
4.  Pulse **Método de distribución** o **Siguiente** y proporcione el siguiente valor:
    * **Método**: *Solo para la consola de Analytics*<br/>
Ademas, elija la opción *Consola de Analytics y POST de red*, si también desea enviar un mensaje POST con una carga útil JSON a su URL personalizado. Los campos siguientes están disponibles si selecciona esta opción.
      * **URL POST de red**
      * **Cabeceras**
      * **Tipo de autenticación**
      * **Cuerpo de la solicitud POST**
5. Pulse **Guardar**.  

Esta alerta se mantiene en directo y activa, supervisando por la frecuencia de configuración hasta que la definición de alerta se inhabilita o se suprime.
{: note}

## Gestión de definiciones de alertas
{: #managing_alert_def}

Puede gestionar las definiciones de alertas desde la página **Definiciones** de alertas.

Para cada definición de alerta, puede realizar las operaciones siguientes,
* Alternar el recuadro de selección de la columna **Habilitado** para habilitar o inhabilitar una definición de alerta específica.
* Si desea crear una copia de una definición de alerta y cambiar algunos valores, pulse el icono de duplicado.
* Pulse el icono del lápiz, si desea editar una definición de alerta.
* Pulse el icono de la papelera, si desea suprimir una definición de alerta.

## Visualización de alertas
{: #viewing_alerts}

Puede visualizar las alertas, que se han desencadenado desde la página **Registros** de alertas.

1.  En la consola de Mobile Analytics, pulse **Registros**.
2.  Pulse el icono **+** de cualquiera de las alertas. Esta acción muestra la definición de alerta y la sección **Instancias de alerta**.
    Si la definición de alerta correspondiente no se ha suprimido o modificado, puede editarla pulsando **Editar alerta**. De lo contrario, el botón **Editar alerta** no está disponible y se muestra el mensaje siguiente: `Esta definición de alerta se ha modificado o suprimido.`
3.  Opcionalmente, puede seleccionar una alerta y pulsar el icono de la papelera para suprimirla.
