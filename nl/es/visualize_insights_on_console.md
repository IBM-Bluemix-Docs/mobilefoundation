---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, charts, visualize data, analytics console

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualización de conocimientos en la consola
{: #visualize_insights_on_console}

Para visualizar información de valor extraída de los datos de análisis capturados y enviados desde la aplicación, debe iniciar la consola de Mobile Analytics pulsando la opción **Consola de analíticas** en la navegación de la consola de Mobile Foundation Operations.

La consola de Mobile Analytics se puede ejecutar en dos modalidades:
  - **Demo Mode ON** (modalidad de demostración activada), que es puramente para fines de demostración y muestra las distintas vistas de análisis (gráficos y tablas) utilizando canales de información de datos simulados.
  - **Demo Mode OFF** (modalidad de demostración desactivada), que muestra las diversas vistas de análisis en función de los canales de información de datos en tiempo real que proceden de sus aplicaciones que están
[instrumentadas para Mobile Analytics](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

Todas las vistas de análisis se pueden recortar aplicando filtros al *nombre de aplicación*, *versión*, *SO de dispositivo* y *periodo de tiempo*, permitiendo así que pueda obtener información desde diferentes perspectivas.

Para visualizar la información de su aplicación, asegúrese de lo siguiente.
  - La aplicación está instrumentada para capturar y enviar los datos de análisis relevantes al servicio de Mobile Analytics.
  - Ha desactivado la modalidad de demostración en la consola de Analytics
  - Aplica los filtros adecuados. Por ejemplo, asegúrese de seleccionar un periodo de tiempo en el que la aplicación esté desplegada sobre el terreno y en el que esté activa con usuarios.

La consola de Mobile Analytics proporciona distintos tipos de análisis del uso y el rendimiento de las aplicaciones móviles, tal y como se ha categorizado en el panel de navegación de la consola de Analytics.  En las secciones siguientes se detallan las diferentes vistas de analíticas:


## Usuarios
{: #reports_visualized_users}
Esta vista le ayuda a obtener información sobre *Patrones de incorporación de usuario*, como el número de usuarios activos que han utilizado la app dentro de un intervalo de fechas especificado y una comparación del número de nuevos usuarios frente al número de usuarios existentes que vuelven a utilizar la app.
Los gráficos de esta vista se pueden filtrar en *nombre de app*, *sistema operativo* o *versión del sistema operativo*.

## Sesiones
{: #reports_visualized_sessions}
Esta vista le ayuda a obtener información sobre los *Patrones de uso* de su aplicación en términos de *Sesiones de app* durante el intervalo de fechas especificado. Una sesión se graba cuando una app se lleva al primer plano de un dispositivo.  Obtendrá información sobre las horas del día en que su aplicación se utiliza más y menos, lo que puede generar información útil para el negocio. Los gráficos de esta vista se pueden filtrar en *nombre de app*, *sistema operativo* o *versión del sistema operativo*.

## Solicitudes de red
{: #reports_visualized_network_requests}
Esta vista le ayuda a obtener información sobre la experiencia de la aplicación, ya que realiza llamadas de API a los sistemas de fondo.  Esta vista tiene tablas y gráficos que proporcionan una visión general sobre cuáles son las funciones más utilizadas de los sistemas de fondo y cuál ha sido su tiempo de respuesta y estabilidad, y si debería considerar el reequilibrio de los sistemas de soporte de fondo.

Esta vista contiene gráficos que trazan en un rango de datos seleccionado, el Tiempo promedio de ida y vuelta de las llamadas de API salientes de la aplicación, el número de solicitudes realizadas por llamada de API, el número de solicitudes con éxito frente a las que han fallado agrupadas por códigos de respuesta.  Los gráficos de esta vista se pueden filtrar según el nombre de la app, el sistema operativo o las versiones del sistema operativo.

## Bloqueos
{: #reports_visualized_crashes}
Esta vista le ayuda con información sobre la estabilidad de la aplicación en un periodo de tiempo seleccionado y le ayuda a decidir si es necesario corregir el diseño o la implementación de la aplicación.  Proporciona gráficos que contrastan el número de bloqueos contra el número total de usos y la frecuencia de bloqueo general.  Los gráficos de esta vista se pueden filtrar en *nombre de app*, *sistema operativo* o *versión del sistema operativo*.


## Resolución de problemas
{: #reports_visualized_troubleshooting}
Esta vista proporciona toda la información necesaria que un desarrollador de aplicaciones podría necesitar para resolver problemas en una aplicación.  Esta vista proporciona un análisis más detallado de los bloqueos de la aplicación en términos de los dispositivos afectados, el sistema operativo de host, el tiempo específico del bloqueo, el seguimiento de pila en el momento del bloqueo y también los registros de bloqueos que se pueden descargar para obtener un análisis más detallado.  

Los registros de bloqueos se recopilan buscando registros de apps que se han registrado en el nivel FATAL.  El SDK de cliente de Analytics para Android e iOS nativo maneja las excepciones y registra detalles sobre ellos, como mensajes de registro de nivel FATAL.  No obstante, con Cordova, los bloqueos en la capa de JavaScript debe manejarlos el desarrollador y los registros de bloqueo se envían al servicio Mobile Analytics para su visualización y análisis en la consola de Mobile Analytics.
{: note}


## Comentarios de usuarios
{: #reports_visualized_userfeedback}
Esta vista proporciona información acerca de la experiencia interactiva real que están viviendo los usuarios mientras utilizan la app y cómo se sienten al respecto.

* Los **propietarios de las apps** pueden obtener una vista detallada y rica en contexto de errores y otros comentarios enviados por **usuarios y verificadores** a medida que se registran mientras se ejecuta la aplicación
* Los **desarrolladores** reciben contextos precisos de aplicación para diagnosticar y solucionar errores o deficiencias en la funcionalidad.
* Los **Propietarios de apps** y los **Desarrolladores** pueden utilizar esta vista para gestionar también las acciones basadas en los comentarios recibidos como, por ejemplo, grabar comentarios o enlaces a problemas creados en los sistemas de seguimiento de errores.  También se puede establecer un estado de revisión general en cada comentario para ayudar a resumir las acciones realizadas a raíz de los comentarios del usuario.

## Gráficos personalizados
Esta vista amplía Mobile Analytics a los casos personalizados en los que los **Propietarios de apps** y los **Desarrolladores** desearían crear sus propios análisis específicos de aplicaciones.   Con este recurso puede crear sus propias vistas de analíticas (gráficos, tablas, etc.) a partir de datos de análisis estándar capturados por el SDK de cliente, así como datos personalizados o datos específicos de la aplicación que estén registrados.  Para obtener más información sobre este recurso de análisis ampliado, acceda
[aquí](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts).
