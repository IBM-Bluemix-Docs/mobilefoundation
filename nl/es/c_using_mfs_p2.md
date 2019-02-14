---

copyright:
  years: 2016, 2019
lastupdated:  "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Configuración para utilizar el plan Professional 1 Application
{: #using_mobilefoundation_p2}

Con el plan Professional 1 Application, los usuarios pueden crear 1 aplicación móvil con varios sistemas operativos móviles.
Tras crear la instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, consulte el procedimiento siguiente para iniciarse en el uso del servicio.

## Requisitos previos
{: #prerequisites_p2}

Tenga en cuenta lo siguiente antes de configurar la instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application.
* {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application solo se admite con los planes de {{site.data.keyword.Db2_on_Cloud_short}} y {{site.data.keyword.composeForPostgreSQL}} de {{site.data.keyword.Bluemix_notm}}.

* Deberá tener acceso a las credenciales de la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} o {{site.data.keyword.composeForPostgreSQL}} antes de poder configurar los valores de su instancia de servicio {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: La instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} puede existir en cualquier `Espacio` dentro de la {{site.data.keyword.Bluemix_notm}} `Organización` o de cualquier otra `Organización` a la que tenga acceso. Asegúrese de que dispone de los permisos para acceder al `Espacio` donde existe la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} o {{site.data.keyword.composeForPostgreSQL}}.


## Adición de la conexión de base de datos
{: #configure_dashdb_p2}

###  Primeros pasos
{: #firststeps_p2}

Tras crear la instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, siga el procedimiento para iniciarse.

### Configuración de la conexión con la instancia de servicio Db2 on Cloud
{: #connect_dashdb_p2}

Después de crear la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, verá la página *Visión general*. Aquí deberá especificar la información de conexión de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o la instancia de servicio de {{site.data.keyword.composeForPostgreSQL}} a la que debería conectarse la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.

Si no tiene ninguna instancia de Db2 on Cloud existente, puede crear una nueva instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} (que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

Siga estos pasos para crear una nueva instancia de servicio de Db2:

1. En la página *Visión general*, seleccione la sección **Crear nuevo servicio**.

+ Seleccione `Sí` en la opción **Configuración de alta disponibilidad**, si desea una instancia de servicio altamente disponible de {{site.data.keyword.Db2_on_Cloud_short}}.

+ Revise los detalles del plan y pulse **Crear**.

Se ha creado una nueva instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}}, que proporciona una instancia dedicada de {{site.data.keyword.Db2_on_Cloud_short}} con RAM de 8 GB y 2 vCPU, y 500 GB de almacenamiento.

Siga estos pasos para conectarse a una instancia de servicio existente de {{site.data.keyword.Db2_on_Cloud_short}} o a la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} que acaba de crear:

1. Seleccione la `Organización` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleccione el `Espacio` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}}, en la lista de espacios disponibles en la `Organización` seleccionada.   
> **Nota:** Si no ve la `Organización` y el `Espacio` donde existe su instancia del servicio de {{site.data.keyword.Db2_on_Cloud_short}}, consulte si es miembro de la `Organización` y del `Espacio`. Se requiere el rol de *Desarrollador* para acceder a la organización y al espacio. El servicio {{site.data.keyword.mobilefoundation_short}} accede a las credenciales desde el servicio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleccione el `Nombre de servicio` y las `Credenciales` de {{site.data.keyword.Db2_on_Cloud_short}} para conectarse con la instancia  de servicio {{site.data.keyword.Db2_on_Cloud_short}} existente.

+  Pruebe la conexión a la instancia de servicio especificada de {{site.data.keyword.Db2_on_Cloud_short}}.

+  Pulse **Añadir**. Esta acción crea las tablas necesarias en la instancia de servicio de base de datos de {{site.data.keyword.Db2_on_Cloud_short}} configurada.

En varios segundos, puede acceder a la página `Visión general`, que le ofrece guías de aprendizaje y vídeos para ayudarle a aprender a utilizar el servicio {{site.data.keyword.mobilefoundation_short}}.

No puede cambiar la instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} que está configurada para que la utilice la instancia del servicio {{site.data.keyword.mobilefoundation_short}}. No obstante, puede utilizar la misma instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} en varias instancias de servicio {{site.data.keyword.mobilefoundation_short}}, ya que cada instancia de servicio {{site.data.keyword.mobilefoundation_short}} crea su propio esquema en la instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} seleccionada.
{: note}

## Inicio del servidor de MobileFirst
{: #start_mobilefoundation_p2}

* Para iniciar {{site.data.keyword.mfserver_short_notm}} con los valores predeterminados, pulse **Iniciar servidor básico**.

* Esta selección crea un {{site.data.keyword.mfserver_long_notm}} con la configuración siguiente:
    -  1 GB de memoria. Este tamaño es suficiente para realizar actividades de desarrollo, de pruebas no muy exigentes y cargas de trabajo de producción a pequeña escala.

    -	El `nombre de usuario` y la `contraseña` se generan de forma automática. Tiene acceso a ellos cuando el servidor está en ejecución.

    Se inicia el proceso de creación del servidor. Este proceso dura unos 10 minutos y un mensaje indica el progreso de la operación. Una vez finalizado, se muestra un panel de control en el que se puede ver lo siguiente:

      -	El estado del servidor que se ejecuta (estado, tamaño).

      -	Se ha creado la ruta para el servidor. Utilice esta ruta en su aplicación móvil para conectarse a {{site.data.keyword.mfserver_short_notm}}.

      -	Su `nombre de usuario` y `contraseña` personal para acceder a la {{site.data.keyword.mfp_oc_short_notm}}. La `contraseña` está oculta. Pulse el icono **Mostrar contraseña** para visualizarla.

*	Pulse **Iniciar consola** para abrir la {{site.data.keyword.mfp_oc_short_notm}}.

Con la consola, puede gestionar sus apps móviles, adaptadores y dispositivos móviles, utilizar su servidor como programa de fondo móvil, enviar notificaciones push, etc.

## Recreación del servidor de MobileFirst
{: #recreate_mobilefoundation_p2}

*	Pulse **Recrear** para volver a crear el servidor.

* Esta acción detiene el servidor existente y suprime los datos. Se crea una nueva instancia con una versión actualizada, si está disponible. Esta acción tarda unos minutos en completarse.

Los datos de su instancia de servidor anterior, incluida la información sobre apps y adaptadores, se conservan en la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} configurada. Estos datos se utilizan para volver a crear el servidor.
{: note}

##	Ajuste de la configuración avanzada
{: #using_mfs_advanced_p2}

Utilice la opción **Iniciar servidor con configuración avanzada** en la página `Visión general` para crear el servidor con valores avanzados o personalizados. También puede actualizar los valores del servidor para personalizar su configuración desde el separador **Configuración**. {{site.data.keyword.mobilefoundation_short}} le ofrece acceso a algunos valores avanzados.

*	Desde el separador **Topología**, puede seleccionar el tamaño del servidor y el número de instancias de servidor que necesita. El valor predeterminado del servidor es de 1 GB, que es suficiente para el desarrollo y la realización de pruebas no muy exigentes.
  - Seleccione el tamaño correcto para su servidor en función de sus necesidades.

  - **Instancias** muestra el número de nodos que se han creado.

## Mobile Analytics
{: #mobile_analytics}

El servidor de Mobile Analytics está incluido y preconfigurado con la instancia de servicio del plan Mobile Foundation: Developer.

* Inicie la consola de Mobile Analytics desde {{site.data.keyword.mfp_oc_short_notm}}.

Para obtener más información sobre Mobile Analytics, puede consultar [MobileFirst Foundation Operational Analytics](https://cloud.ibm.com/docs/services/mobileanalytics/mobileanalytics_overview.html#about-mobile-analytics){: new_window}.

Consulte la [documentación de {{site.data.keyword.mobilefoundation_long}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/){: new_window}, para obtener más detalles.
