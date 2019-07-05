---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: getting started, mobile foundation, plans, configure mobile foundation server, sample app, setup

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Guía de aprendizaje de iniciación
{: #getting-started}

{{site.data.keyword.mobilefoundation_long}} acelera la configuración de un entorno de {{site.data.keyword.mfp_full}} que puede utilizar para desarrollar, probar y ejecutar apps móviles de empresa. {{site.data.keyword.mobilefoundation_short}} ofrece los diferentes planes de servicio siguientes:
* **Developer**: proporciona una instancia del servidor de Foundation en la cuenta del usuario. Permite cualquier número de aplicaciones con el número total de dispositivos conectados en todas las plataformas limitado a 10. Es gratuito y solo se utiliza para fines de desarrollo y pruebas.
* **Professional Per Device**: proporciona una instancia del servidor de Foundation en la cuenta del usuario y se factura según el número de dispositivos conectados de manera activa
* **Professional 1 Application**: proporciona una instancia del servidor de Foundation en la cuenta del usuario y permite que cualquier número de usuarios y dispositivos se conecten activamente solo a una aplicación individual.    
{: shortdesc}

Puede revisar todos los planes disponibles [aquí](https://cloud.ibm.com/catalog/services/mobile-foundation).
{: note}

Cree una instancia de servicio de {{site.data.keyword.mobilefoundation_short}} que utilice uno de los planes con soporte siguiendo esta guía de aprendizaje de iniciación. Después podrá registrar una aplicación. Descargue y edite la aplicación registrada, despliegue un adaptador y, por último, pruebe la aplicación.

## Antes de empezar
{: #prereqs-gs}

Necesitará una cuenta de {{site.data.keyword.Bluemix}} y una instancia del servicio de {{site.data.keyword.mobilefoundation_short}}.

## Paso 1: Cree una instancia del servicio de {{site.data.keyword.mobilefoundation_short}}
{: #step1create}

1. En el **catálogo** de {{site.data.keyword.Bluemix_notm}}, seleccione [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation). Se abre la pantalla de configuración del servicio.
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Seleccione la región, la organización y el espacio en los que desea crear la instancia de servicio.
4. Seleccione el **Plan de precios** y pulse **Crear**.

## Paso 2: Cree el canal móvil
{: #buildmobilechannel}


### Para el plan {{site.data.keyword.mobilefoundation_short}}: Developer
{: #buildchanneldevplan}

Después de crear una instancia de {{site.data.keyword.mobilefoundation_short}}: Developer, puede empezar a crear el canal móvil realizando los pasos siguientes.

* Puede acceder instantáneamente al servidor de Mobile Foundation y trabajar con él.

  Esta selección crea un {{site.data.keyword.mfserver_long_notm}} con la configuración siguiente:
  *	1 GB de memoria. Este tamaño es suficiente para realizar actividades de desarrollo y de pruebas no muy exigentes.

  * Para acceder al servidor de Mobile Foundation mediante la CLI, necesitará las credenciales, que están disponibles al pulsar **Credenciales de servicio** en el panel de navegación de la consola de IBM Cloud.

### Para el plan {{site.data.keyword.mobilefoundation_short}}: Professional Per Device
{: #buildchannelprofdeviceplan}

Después de crear una instancia del servicio de {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, puede empezar a crear el canal móvil realizando los pasos siguientes.

  1.  Conéctese a un servicio {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} existente en {{site.data.keyword.Bluemix_notm}}.

      1.  Seleccione la `Organización` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

      + Seleccione el `Espacio` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}, en la lista de espacios disponibles en la `Organización` seleccionada.

      + Seleccione el `Nombre de servicio` y `Credenciales` de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} para conectarse a la instancia de servicio existente de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

      + Pruebe la conexión a la instancia de servicio seleccionada de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} pulsando **Probar conexión**.

      + Pulse **Añadir** seguido de **Continuar** en la ventana emergente que le solicita información sobre el servicio {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} seleccionado. Esta acción crea las tablas necesarias en la instancia de servicio de base de datos de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} configurada.

      Después de añadir una conexión {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} a la instancia {{site.data.keyword.mobilefoundation_short}} no podrá cambiarla.
      {: note}
  2.  Cree e inicie el servidor.

      1. Cree una instancia de servidor de {{site.data.keyword.mobilefirst_notm}} con la configuración predeterminada, pulse **Iniciar servidor básico**.

      + Esta selección suministra un {{site.data.keyword.mfserver_long_notm}} con la configuración siguiente:
          - Dos nodos con 1 GB de memoria cada uno. Este tamaño es correcto para realizar actividades de desarrollo, de pruebas no muy exigentes y cargas de trabajo de producción a pequeña escala.

          -	El `nombre de usuario` y la `contraseña` se generan de forma automática. Tiene acceso a ellos cuando el servidor está en ejecución.

          Se inicia el proceso de creación del servidor. Este proceso dura unos 10 minutos y un mensaje indica el progreso de la operación. Una vez finalizado, se muestra un panel de control en el que se puede ver lo siguiente:
            -	El estado del servidor que se ejecuta (estado, tamaño).
            -	Se ha creado la ruta para el servidor. Utilice esta ruta en su aplicación móvil para conectarse a {{site.data.keyword.mfserver_short_notm}}.
            -	Su `nombre de usuario` y `contraseña` personal para acceder a la {{site.data.keyword.mfp_oc_short_notm}}. La `contraseña` está oculta. Pulse el icono **Mostrar contraseña** para visualizarla.

      +	Pulse **Iniciar consola** para abrir la {{site.data.keyword.mfp_oc_short_notm}}.      

      Para crear una instancia de servidor de {{site.data.keyword.mobilefirst_notm}} con configuración avanzada para la topología, seguridad y otros tipos de configuración del servidor, pulse **Iniciar servidor con configuración avanzada**. Para obtener más información, consulte [Ajuste de la configuración avanzada](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#using_mfs_advanced_p5).
      {: tip}

### Para el plan {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application
{: #buildchannelprof1appplan}

Después de crear una instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, puede empezar a crear el canal móvil realizando los pasos siguientes.

  1.  Conéctese a un servicio {{site.data.keyword.Db2_on_Cloud_long}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL_full}} existente en {{site.data.keyword.Bluemix_notm}}.

      1.  Seleccione la `Organización` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

      + Seleccione el `Espacio` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}, en la lista de espacios disponibles en la `Organización` seleccionada.

      + Seleccione el `Nombre de servicio` y `Credenciales` de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} para conectarse a la instancia de servicio existente de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

      + Pruebe la conexión a la instancia de servicio seleccionada de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} pulsando **Probar conexión**.

      + Pulse **Añadir**, seguido de **Continuar** en la ventana emergente que le solicita información sobre el servicio {{site.data.keyword.Db2_on_Cloud_short}} o {{site.data.keyword.composeForPostgreSQL}} seleccionado. Esta acción crea las tablas necesarias en la instancia de servicio de base de datos de {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} configurada.

      Después de añadir una conexión {{site.data.keyword.Db2_on_Cloud_short}} (cualquier plan que no sea el plan **Lite**) o {{site.data.keyword.composeForPostgreSQL}} a la instancia {{site.data.keyword.mobilefoundation_short}} no podrá cambiarla.
      {: note}

  2.  Cree e inicie el servidor.

      1. Cree una instancia de servidor de {{site.data.keyword.mobilefirst_notm}} con la configuración predeterminada, pulse **Iniciar servidor básico**.

        `La instancia de servidor básico incluye un nodo y 1 GB de memoria.`

      + El `nombre de usuario` y la `contraseña` se generan de forma automática. Tiene acceso a ellos cuando el servidor está en ejecución.  

        Se inicia el proceso de creación del servidor. Este proceso dura unos 10 minutos y un mensaje indica el progreso de la operación. Una vez finalizado, se muestra un panel de control en el que se puede ver lo siguiente:
          -	El estado del servidor que se ejecuta (estado, tamaño).
          -	Se ha creado la ruta para el servidor. Utilice esta ruta en su aplicación móvil para conectarse a {{site.data.keyword.mfserver_short_notm}}.
          -	Su `nombre de usuario` y `contraseña` personal para acceder a la {{site.data.keyword.mfp_oc_short_notm}}. La `contraseña` está oculta. Pulse el icono **Mostrar contraseña** para visualizarla.

      +  Pulse **Iniciar consola** para abrir la {{site.data.keyword.mfp_oc_short_notm}}.  

      Para crear una instancia de servidor de {{site.data.keyword.mobilefirst_notm}} con configuración avanzada para la topología, seguridad y otros tipos de configuración del servidor, pulse **Iniciar servidor con configuración avanzada**. Para obtener más información, consulte [Ajuste de la configuración avanzada](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#using_mfs_advanced_p2).
      {: tip}

Vaya a [Utilización del servicio Mobile Foundation para configurar el servidor de MobileFirst ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/){: new_window} para obtener más información sobre la iniciación a {{site.data.keyword.mobilefoundation_short}}.
{: note}

## Paso 3: Registre la aplicación en {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

Después de crear e iniciar la instancia de servidor de Mobile Foundation, siga estos pasos para registrar una aplicación de Android.

  1.  Invoque la {{site.data.keyword.mfp_oc_short_notm}} cargando el URL:
`http://<your-server-host>:<server-port>/mfpconsole`. Utilice el `nombre de usuario` y la `contraseña` generados en el momento del suministro.

  + En el **Panel de control** de la {{site.data.keyword.mfp_oc_short_notm}}, pulse **Nueva** junto a **Aplicaciones**.

  + Escriba *MFPStarterAndroid* como **nombre de aplicación**.

  + Seleccione *Android* en **Elegir plataforma**.

  + Indique *com.ibm.mfpstarterandroid* como **Identificador de aplicación**.

  + Indique *1.0* como **Versión**.

  + Pulse **Registrar aplicación**.

## Paso 4: Descargue la aplicación de ejemplo
{: #downloadapp}

  1.  Desde el **Panel de control** de {{site.data.keyword.mfp_oc_short_notm}}, seleccione **MFPStarterAndroid** en **Aplicaciones**.

  + Pulse **Obtener código de inicio** y seleccione descargar la aplicación de ejemplo de Android.

## Paso 5: Edite la aplicación de ejemplo
{: #editapp}

  1. Importe en Android Studio la app de ejemplo de Android que ha descargado en el paso anterior.

  + En el menú de la barra lateral **Proyecto** en Android Studio, seleccione el archivo **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java**.

    * Añada las importaciones siguientes
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * Pegue el fragmento de código siguiente y sustituya la llamada a `WLAuthorizationManager.getInstance().obtainAccessToken`

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Will print "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## Paso 6: Despliegue un adaptador
{: #deployadapter}

  1. Descargue este [artefacto de adaptador](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} y despliéguelo desde la {{site.data.keyword.mfp_oc_short_notm}} mediante **Acciones → Desplegar adaptador**.

## Paso 7: Pruebe la aplicación
{: #testapp}

  1. En Android Studio, en el menú de la barra lateral de **Proyecto**, seleccione el archivo **app → src → main →assets → mfpclient.properties** y edite las propiedades de `protocol`, `host` y `port` con los valores correctos del servidor de MobileFirst.

   Los valores suelen ser https, *su-dirección-de-servidor* y 443.
   {: tip}

  2. Pulse **Ejecutar App** en Android Studio.
     * Verá que la app se ha iniciado en un emulador de dispositivo.
     * Pulse **Hacer ping a servidor de MobileFirst** en la aplicación y se mostrará `Conectado al servidor de MobileFirst`.
     * Si la aplicación se ha podido conectar al servidor de MobileFirst, el adaptador Java desplegado realizará una llamada de solicitud de recursos.
     * La respuesta del adaptador aparecerá en la vista de LogCat de Android Studio.


## Siguientes pasos
{: #nextsteps-gs}

Siga las [Guías de aprendizaje rápido ![Icono de enlace externo](../../icons/launch-glyph.svg "Guías de aprendizaje rápido")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} para trabajar con más aplicaciones de ejemplo y explorar el funcionamiento de {{site.data.keyword.mobilefoundation_short}}.

Hay guías de aprendizaje rápido en las que se describe el funcionamiento de {{site.data.keyword.mobilefoundation_short}} para apps de iOS, Android, Web, Cordova, Windows React Native, Ionic y Xamarin.
