---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, instrumenting cordova app, instrumenting iOS app, instrumenting android app

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
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
{:web: .ph data-hd-programlang='Web'}

# Instrumentar la app
{: #instrument_your_app}

Mobile Analytics es una característica que está incluida en el servicio {{ site.data.keyword.mobilefoundation_short }}. Mobile Analytics proporciona el uso de aplicaciones clave y los conocimientos de rendimiento de aplicaciones para desarrolladores de aplicaciones móviles y propietarios de aplicaciones.

Necesita instrumentar la aplicación móvil para utilizar la característica Mobile Analytics para supervisar el uso de la aplicación, el rendimiento y obtener otras estadísticas. La instrumentación de la aplicación tiene los pasos siguientes:

1.  Importe e instale el SDK de cliente de Mobile Analytics.
2.  Instrumente la aplicación basándose en el tipo de datos de análisis que desea recopilar.

En las secciones siguientes se indican los detalles de estos pasos, para cada una de las plataformas admitidas.

### Instrumentar una aplicación Android
{: #instrument_android_app}
{: android}
#### Paso 1: Importe e instale el SDK de cliente de Mobile Analytics para Android
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

El SDK de cliente de Mobile Analytics se distribuye con Gradle, un gestor de dependencias para proyectos Android. Gradle descarga automáticamente artefactos desde los repositorios y los deja a disposición de su aplicación Android.
{: android}

1. Cree un proyecto [Android Studio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://developer.android.com/sdk/index.html){: new_window} o abra uno existente.
{: android}

2. Abra el archivo `build.gradle` que se encuentra en el **módulo de la app**.
{: android}

  Es posible que el proyecto de Android tenga dos archivos `build.gradle`: uno para el proyecto y otro para el módulo de app. Asegúrese de utilizar el archivo del **módulo de la app**.
  {: tip}
  {: android}

3. Busque la sección `Dependencias` del archivo `build.gradle` y añada una dependencia de compilación para el SDK de cliente de {{site.data.keyword.mobileanalytics_short}}. La sentencia de los repositorios debe parecerse al siguiente código de ejemplo:
{: android}

  ```
  dependencies {
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationanalytics:8.0.+@aar'
    // other dependencies
  }
  ```
  {: codeblock}
  {: android}

  La primera dependencia es para el sdk de cliente de Mobile Analytics, para capturar y registrar sucesos de tiempo de ejecución de aplicaciones, y la segunda dependencia es para permitir que los comentarios de usuario integrados en la app interactúen con el usuario de la aplicación. La segunda dependencia sólo es necesaria si se habilitan los comentarios de usuario integrados en la app
  {: android}

4. Sincronice el proyecto con Gradle; para ello, pulse **Tools &gt; Android &gt; Sync Project with Gradle Files**.
{: android}

5. Abra el archivo `AndroidManifest.xml` del proyecto de Android. Puede encontrar este archivo en **app > manifests**. Añada permisos de acceso a Internet y de acceso a ubicación bajo el elemento `<manifest>`:
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />

  ```
  {: codeblock}
  Si utiliza una versión de SDK mayor que >= 1.2, necesitará incluir las líneas siguientes dentro del elemento
`<application>` del archivo `AndroidManifest.xml`.
  {: android}

  ```
    <activity
  android:name="com.ibm.mobilefirstplatform.clientsdk.android.ui.UIActivity"
  android:label="@string/app_name"
  android:launchMode="singleTask">
  <intent-filter>
  <action android:name="android.intent.action.MAIN" />
  <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  </activity>
  ```
  {: codeblock}
  {: android}

#### Paso 2: Instrumente la aplicación de Android basándose en el tipo de datos de análisis que desea recopilar
{: #instrument_app_based_on_data_android }
{: android}

1. Inicialice la aplicación para recopilar y enviar datos de análisis al servicio de Mobile Analytics.  En primer lugar, añada las siguientes sentencias `import` al principio de la clase de actividad o aplicación:
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   A continuación, utilice el SDK de cliente de Mobile Analytics para configurar o inicializar la captura de datos de análisis.  
   Añada el código de inicialización en el método `onCreate` de la actividad principal en la aplicación de Android o en la ubicación que más convenga a su proyecto de aplicación.
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   Antes de llamar al método init, debe asegurarse de que la aplicación incorpora el código necesario para autenticar y autorizar el dispositivo con el servicio de MobileFoundation.  Este paso es un paso común que es necesario para todas las aplicaciones de servicios de Mobile Foundation y no es específico para la captura de datos de Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   Con la inicialización completa, la aplicación estará habilitada para capturar la información de dispositivos y los registros de SDK de Mobile Analytics sin añadir código adicional.  Las API y los códigos adicionales que se describen en las secciones siguientes son opcionales y se pueden añadir en función del tipo de datos de análisis que desee capturar.
   {: android}

2. Para capturar los sucesos del ciclo de vida de la aplicación, como la sesión de aplicación y la información de bloqueo, configure la aplicación añadiendo la línea de código siguiente:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. Para capturar los sucesos de red que capturan las interacciones de red de aplicaciones, configure la aplicación añadiendo la línea de código siguiente:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. Ha inicializado la aplicación para capturar los datos de análisis.  A continuación, debe enviar los datos capturados al servicio de Mobile Analytics.
   Utilice la API siguiente para enviar datos de análisis al servicio de Mobile Analytics.
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  Es libre de decidir cuándo llamar a esta API en el flujo de aplicaciones para enviar los datos de análisis capturados al servicio de Mobile Analytics.  Hasta entonces, todos los datos de análisis capturados se almacenan localmente en el dispositivo.
  {: android}

5. Para capturar y enviar registros de aplicaciones al servicio de Mobile Analytics, utilice las API de registrador de SDK de cliente de Mobile Analytics.     
   La infraestructura de registro del SDK de cliente de Mobile Analytics admite los niveles de registro siguientes, listados de menor a mayor detalle, con las directrices de uso recomendadas:
    * FATAL: se usa para bloqueos irrecuperables. El nivel FATAL está reservado para errores de registro irrecuperables que ven los usuarios como un bloqueo de aplicación
    * ERROR: uso para excepciones inesperadas o errores de protocolo de red inesperados
    * WARN: se usa para registrar advertencias de uso que no se consideran errores críticos, como el uso de API en desuso o en caso de una respuesta de red lenta
    * INFO: se usa para registrar sucesos de inicialización y otros datos que podrían ser importantes, pero no urgentes
    * DEBUG: se usa para notificar sentencias de depuración para que los desarrolladores puedan resolver defectos de la aplicación.
    Cuando el nivel de registro es DEBUG, también obtiene registros del SDK de cliente de Mobile Analytics, que se incluye cuando se envían los registros.
   {: note}
   {: android}

   Los siguientes fragmentos de código muestran el ejemplo de uso del registrador para Android:
   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);

   //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
   Logger logger = Logger.getInstance("loggerName");

   // Log messages with different levels
   // Debug message for feature 1
   // Info message for feature 2
   logger.debug("debug message");
   //the logger.debug message is not logged because the logLevelFilter is set to Info
   logger.info("info message");

   // Send logs to the Mobile Analytics
   Logger.send();
   ```
   {: codeblock}
   {: android}

6.  Para obtener información sobre los patrones de incorporación de usuario (nuevos usuarios frente a usuarios que vuelven), debe asociar una identidad de usuario con la sesión de la aplicación.  Este paso se puede realizar llamando a la API siguiente
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    Todos los datos de análisis capturados y almacenados localmente se envían al servicio de Mobile Analytics sólo cuando se llama a la API WLAnalytics.send().
    {: android}

7.  Para obtener información sobre los patrones de interacción de la red HTTP de las aplicaciones, como por ejemplo las API HTTP llamadas, el número de solicitudes y los tiempos medios de respuesta, debe utilizar la clase WLResourceRequest del SDK de cliente de servicio de Mobile Foundation para realizar las llamadas HTTP.  Aunque haya inicializado Analytics para capturar sucesos de red, el uso de `WLResourceRequest` permite a Mobile Analytics acceder a las llamadas de red y capturar los datos relevantes.  Realice las llamadas de HTTP como en el código siguiente.
    ```java
      WLResourceRequest request = new WLResourceRequest(new URI(url), WLResourceRequest.GET);
            request.send(new WLResponseListener() {

                @Override
                    public void onSuccess(WLResponse wlResponse) {
                    //handle success of HTTP call and use wlResponse 
                }

                @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                    //handle failure of HTTP call and use wlFailResponse
                }
            });
    ```
    {: codeblock}
    {: android}

8.  Para obtener analíticas de bloqueo y registros, puede llamar a la siguiente API durante el inicio de la aplicación para comprobar si la sesión anterior se ha bloqueado y, en consecuencia, enviar los registros del bloqueo de captura al servicio de Mobile Analytics.
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  Para definir análisis personalizados y definir sus propios datos de análisis adicionales a los admitidos de forma inherente en el SDK de cliente, puede utilizar la API de registro personalizado
    ```java
        //create a JSON to capture the custom data 
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("FromPage", "LoginPage");

        //log the custom data with a message
        WLAnalytics.log("log message", jsonObject);

        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.send();
    ```
    {: codeblock}
    {: android}

    Los datos personalizados registrados se pueden trazar sobre gráficos personalizados que puede definir en la consola de Mobile Analytics para obtener información personalizada.
    {: android}

10. Utilice comentarios de usuario integrados en la app para realizar un análisis más detallado del rendimiento de la aplicación. Puede permitir que los **usuarios y probadores** de su aplicación proporcionen comentarios contextuales enriquecidos a los propietarios de las apps. Los **propietarios de las apps** reciben comentarios en tiempo real de sus usuarios sobre la experiencia de uso de la aplicación, y los **propietarios de las apps** y los **desarrolladores** pueden tomar medidas basadas en dichos comentarios.  Esta característica agiliza de forma significativa el mantenimiento de la aplicación.  Utilice la API siguiente para cambiar la aplicación a la modalidad de comentarios interactivos en cualquier manejador de acciones de la aplicación, por ejemplo, al manejar la pulsación de un botón o la selección de un elemento de menú.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### Instrumentar una aplicación iOS
{: #instrument_ios_app}
{: ios}

Pasos para instrumentar la aplicación de iOS:
{: ios}

#### Paso 1: Importe e instale el SDK de cliente de Mobile Analytics para iOS
{: #install_analytics_sdk_ios }
{: ios}

[![Compatible con CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![Compatible con CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

El SDK de Swift está disponible para iOS y watchOS.
{: ios}

1. Añada lo siguiente al podfile existente, ubicado en la raíz del proyecto Xcode
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
       pod 'IBMMobileFirstPlatformFoundationAnalytics'
   end
   ```
   {: codeblock}
   {: ios}

2. Desde una ventana de línea de mandatos, vaya a la raíz del proyecto Xcode y ejecute el mandato siguiente:
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### Paso 2: Instrumente la aplicación de iOS basándose en el tipo de datos de análisis que desea recopilar
{: #instrument_app_based_on_data_ios }
{: ios}

1. Inicialice la aplicación para habilitar el envío de registros a Mobile Analytics.
   El SDK de Swift está disponible para iOS y watchOS. Importe las infraestructuras `BMSCore` y `BMSAnalytics`; para ello, añada las siguientes sentencias `import` al inicio del archivo del proyecto `AppDelegate.swift`:
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   A continuación, utilice el SDK de cliente de Mobile Analytics para configurar o inicializar la captura de datos de análisis.
   Añada el código de inicialización en la ubicación que más convenga a su proyecto de aplicación.
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   Antes de llamar al método init, debe asegurarse de que la aplicación incorpora el código necesario para autenticar y autorizar el dispositivo con el servicio de MobileFoundation.  Este paso es un paso común que es necesario para todas las aplicaciones de servicios de Mobile Foundation y no es específico para la captura de datos de Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   Con la inicialización completa, la aplicación estará habilitada para capturar la información de dispositivos y los registros de SDK de Mobile Analytics sin añadir código adicional.  Las API y los códigos adicionales que se describen en las secciones siguientes son opcionales y se pueden añadir en función del tipo de datos de análisis que desee capturar.
   {: ios}

2. Para capturar los sucesos del ciclo de vida de la aplicación, como la sesión de aplicación y la información de bloqueo, configure la aplicación añadiendo la línea de código siguiente:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. Para capturar los sucesos de red que capturan las interacciones de red de aplicaciones, configure la aplicación añadiendo la línea de código siguiente:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. Ha inicializado la aplicación para capturar los datos de análisis.  A continuación, debe enviar los datos capturados al servicio de Mobile Analytics.
   Utilice la API siguiente para enviar datos de análisis al servicio de Mobile Analytics.
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    Es libre de decidir cuándo llamar a esta API en el flujo de aplicaciones para enviar los datos de análisis capturados al servicio de Mobile Analytics.  Hasta entonces, todos los datos de análisis capturados se almacenan localmente en el dispositivo.
    {: ios}

5. Para capturar y enviar registros de aplicaciones al servicio de Mobile Analytics, utilice las API de registrador de SDK de cliente de Mobile Analytics.
   La infraestructura de registro del SDK de cliente de Mobile Analytics admite los niveles de registro siguientes, listados de menor a mayor detalle, con las directrices de uso recomendadas:
    * FATAL: se usa para bloqueos irrecuperables. El nivel FATAL está reservado para errores de registro irrecuperables que ven los usuarios como un bloqueo de aplicación
    * ERROR: uso para excepciones inesperadas o errores de protocolo de red inesperados
    * WARN: se usa para registrar advertencias de uso que no se consideran errores críticos, como el uso de API en desuso o en caso de una respuesta de red lenta
    * INFO: se usa para registrar sucesos de inicialización y otros datos que podrían ser importantes, pero no urgentes
    * DEBUG: se usa para notificar sentencias de depuración para que los desarrolladores puedan resolver defectos de la aplicación.
    Cuando el nivel de registro es DEBUG, también obtiene registros del SDK de cliente de Mobile Analytics, que se incluye cuando se envían los registros.
   {: note}
   {: ios}

    Siga [la guía de aprendizaje ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) para detectar fragmentos de código para el registrador con el fin de añadir funciones de registro en aplicaciones iOS.

6.  Para obtener información sobre los patrones de incorporación de usuario (nuevos usuarios frente a usuarios que vuelven), debe asociar una identidad de usuario con la sesión de la aplicación.  Esta asociación se puede realizar llamando a la API siguiente
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    Todos los datos de análisis capturados y almacenados localmente se envían al servicio de Mobile Analytics sólo cuando se llama a la API `WLAnalytics.sharedInstance().send()`.
    {: ios}

7.  Para obtener información sobre los patrones de interacción de la red HTTP de las aplicaciones, como por ejemplo las API HTTP llamadas, el número de solicitudes y los tiempos medios de respuesta, debe utilizar la clase WLResourceRequest del SDK de cliente de servicio de Mobile Foundation para realizar las llamadas HTTP.  Aunque haya inicializado Analytics para capturar sucesos de red, el uso de `WLResourceRequest` permite a Mobile Analytics acceder a las llamadas de red y capturar los datos relevantes.  Realice las llamadas de HTTP como en el código siguiente.
    ```Swift
      let request = WLResourceRequest( url: URL(string: "/adapters/JavaAdapter/users"), method: WLHttpMethodGet )

      request!.send { (response, error) -> Void in
          if(error == nil){
              //handle failure of HTTP call and use wlFailResponse
          }
          else{
              //handle success of HTTP call and use wlResponse
          }
      }
    ```
    {: codeblock}
    {: ios}

8.  Para obtener analíticas de bloqueo y registros, puede llamar a la siguiente API durante el inicio de la aplicación para comprobar si la sesión anterior se ha bloqueado y enviar los registros del bloqueo de captura al servicio de Mobile Analytics.
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  Para definir análisis personalizados y definir sus propios datos de análisis adicionales a los admitidos de forma inherente en el SDK de cliente, puede utilizar la API de registro personalizado
    ```Swift
        //create an array object with key value pair as custom data
        let eventObject = ["FromPage": "LoginPage"]

        //log the custom data with a message
        WLAnalytics.sharedInstance().log("log message", withMetadata: eventObject);

        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.sharedInstance().send();
    ```
    {: codeblock}
    {: ios}

    Los datos personalizados registrados se pueden trazar sobre gráficos personalizados que puede definir en la consola de Mobile Analytics para obtener información personalizada.
    {: ios}

10. Utilice comentarios de usuario integrados en la app para realizar un análisis más detallado del rendimiento de la aplicación.   Puede permitir que los **usuarios y probadores** de su aplicación proporcionen comentarios contextuales enriquecidos a los propietarios de las apps. Los **propietarios de las apps** reciben comentarios en tiempo real de sus usuarios sobre la experiencia de uso de la aplicación, y los **propietarios de las apps** y los **desarrolladores** pueden tomar medidas basadas en dichos comentarios.  Esta característica agiliza de forma significativa el mantenimiento de la aplicación.  Utilice la API siguiente para cambiar la aplicación a la modalidad de comentarios interactivos en cualquier manejador de acciones de la aplicación, por ejemplo, al manejar la pulsación de un botón o la selección de un elemento de menú.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Instrumentar una aplicación de Cordova
{: #instrument_cordova_app}
{: cordova}
Pasos para instrumentar la aplicación de Cordova:
{: cordova}

#### Paso 1: Importe e instale el plugin del SDK de cliente de Foundation y Analytics para Cordova
{: #install_analytics_sdk_cordova }
El plugin de Cordova de Mobile Analytics le permite instrumentar la aplicación móvil.
{: cordova}

#### Instalación del SDK de cliente de Cordova
{: #install_cordova_sdk}
{: cordova}

1. Cree un proyecto de [Cordova ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://cordova.apache.org/#getstarted){: new_window} o abra uno existente.
    {: cordova}

2. Añada las plataformas Android o iOS que prefiera a la aplicación de Cordova. Ejecute uno, o ambos, de los siguientes mandatos desde la línea de mandatos. <br/>
    **Android**:
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS**:
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. Añada el plugin de sdk de cliente de Foundation `cordova-plugin-mfp`.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. Añada el plugin de sdk de cliente de Analytics `cordova-plugin-mfp-analytics`, que es para la función de comentarios integrados en la app
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. Verifique que el plug-in se ha instalado correctamente ejecutando el siguiente mandato:
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### Paso 2: Instrumente la aplicación de Cordova basándose en el tipo de datos de análisis que desea recopilar
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. En aplicaciones Cordova no es necesaria ninguna configuración inicial y la inicialización ya viene incorporada.

   Antes de llamar a alguno de los métodos de análisis siguientes, debe asegurarse de que la aplicación incorpora el código necesario para autenticar y autorizar el dispositivo con el servicio de MobileFoundation.  Este paso es un paso común que es necesario para todas las aplicaciones de servicios de Mobile Foundation y no es específico para la captura de datos de Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   Con la inicialización completa, la aplicación estará habilitada para capturar la información de dispositivos y los registros de SDK de Mobile Analytics sin añadir código adicional.  Las API y los códigos adicionales que se describen en las secciones siguientes son opcionales y se pueden añadir en función del tipo de datos de análisis que desee capturar.
   {: cordova}

2. Para habilitar la captura de los sucesos del ciclo de vida, se debe inicializar en la plataforma nativa de la app Cordova.
    * Para la plataforma Android:
      * Abra el archivo **[carpeta raíz aplicación Cordova] → plataformas → android → src → com → sample → [nombre-app] → MainActivity.java**.
      * Busque el método `onCreate` y habilitar las actividades `LIFECYCLE` y `NETWORK`:
        * Para habilitar el registro de sucesos del ciclo de vida del cliente, añada la línea siguiente:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Para habilitar el registro de sucesos de red del cliente, añada la línea siguiente:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Compile el proyecto Cordova ejecutando el mandato: `cordova build`.
    * Para la plataforma iOS:
      * Abra la carpeta **[carpeta raíz aplicación Cordova] → plataformas → ios → Clases** y busque el archivo **AppDelegate.swift** (Swift).
      * Para habilitar las actividades `LIFECYCLE` y `NETWORK`, realice los pasos siguientes.
        * Para habilitar el registro de sucesos del ciclo de vida del cliente, añada la línea siguiente:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Para habilitar el registro de sucesos de red del cliente, añada la línea siguiente:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Compile el proyecto Cordova ejecutando el mandato: `cordova build`.

3. Ha inicializado la aplicación para recopilar los datos de análisis. A continuación, puede enviar datos de análisis a Mobile Analytics.
   Utilice las API siguientes para empezar el registro y envío de analíticas de uso:
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. Para capturar y enviar registros de aplicaciones al servicio de Mobile Analytics, utilice las API de registrador de SDK de cliente de Mobile Analytics.
   La infraestructura de registro del SDK de cliente de Mobile Analytics admite los niveles de registro siguientes, listados de menor a mayor detalle, con las directrices de uso recomendadas:
    * FATAL: se usa para bloqueos irrecuperables. El nivel FATAL está reservado para errores de registro irrecuperables que ven los usuarios como un bloqueo de aplicación
    * ERROR: uso para excepciones inesperadas o errores de protocolo de red inesperados
    * WARN: se usa para registrar advertencias de uso que no se consideran errores críticos, como el uso de API en desuso o en caso de una respuesta de red lenta
    * INFO: se usa para registrar sucesos de inicialización y otros datos que podrían ser importantes, pero no urgentes
    * DEBUG: se usa para notificar sentencias de depuración para que los desarrolladores puedan resolver defectos de la aplicación.
    Cuando el nivel de registro es DEBUG, también obtiene registros del SDK de cliente de Mobile Analytics, que se incluye cuando se envían los registros.
    * TRACE - utilizado para puntos de entrada y salida de método
   {: note}
   {: cordova}

   Los siguientes fragmentos de código muestran el ejemplo de uso del registrador para Cordova:
    ```Javascript
    // Set the logging level (optional)
    // The default setting is DEBUG
    WL.Logger.config({"level": "INFO"});

    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    var logger = WL.Logger.create({ pkg: 'loggerName' });

    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug","debug message");
    //the logger.debug message is not logged because the logLevelFilter is set to Info
    logger.info("info","info message");

    // Send logs to the Mobile Analytics
    logger.send();
    ```
    {: codeblock}
    {: cordova}

5.  Para obtener información sobre los patrones de incorporación de usuario (nuevos usuarios frente a usuarios que vuelven), debe asociar una identidad de usuario con la sesión de la aplicación.  No hay ninguna API específica disponible en el nivel de Javascript. Pero esta acción se puede realizar en el código nativo llamando a la API siguiente:

    **Android:**
      ```java
        WLAnalytics.setUserContext("userName or userIdentity");
      ```
      {: codeblock}
      {: cordova}

    **iOS:**
      ```Swift
        WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
      ```
      {: codeblock}
      {: cordova}

    Todos los datos de análisis capturados y almacenados localmente se envían al servicio de Mobile Analytics solo cuando se llama a la API WL.Analytics.send() en nivel javascript [o] API WLAnalytics.send() en android nativo [o] API WLAnalytics.sharedInstance().send() en iOS nativo.
    {: cordova}

6. Para obtener información sobre los patrones de interacción de la red HTTP de las aplicaciones, como por ejemplo las API HTTP llamadas, el número de solicitudes y los tiempos medios de respuesta, debe utilizar la clase WLResourceRequest del SDK de cliente de servicio de Mobile Foundation para realizar las llamadas HTTP.  Aunque haya inicializado Analytics para capturar sucesos de red, el uso de `WLResourceRequest` permite a Mobile Analytics acceder a las llamadas de red y capturar los datos relevantes.  Realice las llamadas de HTTP utilizando el código siguiente.
    ```Javascript
      var resourceRequest = new WLResourceRequest("url-path", WLResourceRequest.GET);
      resourceRequest.send().then(
        onSuccess: function(response) {
            resultText = "Successfully called the resource: " + response.responseText;
        },

        onFailure: function(response) {
            resultText = "Failed to call the resource:" + response.errorMsg;
        }
      );
    ```
    {: codeblock}
    {: cordova}

7. Para definir análisis personalizados y definir sus propios datos de análisis adicionales a los admitidos de forma inherente en el SDK de cliente, puede utilizar la API de registro personalizado
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    Los datos personalizados registrados se pueden trazar sobre gráficos personalizados que puede definir en la consola de Mobile Analytics para obtener información personalizada.
    {: cordova}

8. Utilice comentarios de usuario integrados en la app para realizar un análisis más detallado del rendimiento de la aplicación.   Puede permitir que los **usuarios y probadores** de su aplicación proporcionen comentarios contextuales enriquecidos a los propietarios de las apps. Los **propietarios de las apps** reciben comentarios en tiempo real de sus usuarios sobre la experiencia de uso de la aplicación, y los **propietarios de las apps** y los **desarrolladores** pueden tomar medidas basadas en dichos comentarios.  Este paso agiliza de forma significativa el mantenimiento de la aplicación.  Utilice la API siguiente para cambiar la aplicación a la modalidad de comentarios interactivos en cualquier manejador de acciones de la aplicación, por ejemplo, al manejar la pulsación de un botón o la selección de un elemento de menú.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Instrumentar una aplicación web
{: #instrument_web_app}
{: web}

Pasos para instrumentar la aplicación web:
{: web}

#### Paso 1: Importe e instale el SDK de cliente de Mobile Analytics para web
{: #install_analytics_sdk_web }
{: web}

#### Instalación del SDK de cliente web
{: #install_web_sdk}
El SDK de Mobile Analytics le permite instrumentar la aplicación web.
{: web}

1. Vaya a la carpeta raíz de la aplicación web y ejecute el mandato siguiente. Añada el SDK a la aplicación web utilizando [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### Paso 2: Instrumente la aplicación web basándose en el tipo de datos de análisis que desea recopilar
{: #instrument_app_based_on_data_web }
{: web}

1. Haga referencia a los archivos ibmmfpf.js e ibmmfpfanalytics.js en el elemento `head` de la página html principal

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. Inicialice el SDK web de Mobile Foundation especificando los valores de raíz de contexto y de ID de aplicación en el archivo JavaScript principal de la aplicación web

    ```Javascript
      var wlInitOptions = {
        mfpContextRoot : '/mfp', // "mfp" is the default context root in the Mobile Foundation
        applicationId : 'com.sample.mywebapp' // Replace with your own value.
      };

      WL.Client.init(wlInitOptions).then (
        function() {
          // Application logic.
        }
      );
    ```
    {: codeblock}
    {: web}

   Antes de llamar a alguno de los métodos de análisis siguientes, debe asegurarse de que la aplicación incorpora el código necesario para autenticar y autorizar el dispositivo con el servicio de MobileFoundation.  Este paso es un paso común que es necesario para todas las aplicaciones de servicios de Mobile Foundation y no es específico para la captura de datos de Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   Con la inicialización completa, la aplicación estará habilitada para capturar la información de dispositivos y los registros de SDK de Mobile Analytics sin añadir código adicional.  Las API y los códigos adicionales que se describen en las secciones siguientes son opcionales y se pueden añadir en función del tipo de datos de análisis que desee capturar.
   {: web}

3. Añada la línea siguiente a la lógica de la aplicación para capturar sucesos de ciclo de vida de aplicación y sucesos de red.

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. Ha inicializado la aplicación para capturar los datos de análisis.  A continuación, debe enviar los datos capturados al servicio de Mobile Analytics. Utilice la API siguiente para enviar datos de análisis al servicio de Mobile Analytics.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    Es libre de decidir cuándo llamar a esta API en el flujo de aplicaciones para enviar los datos de análisis capturados al servicio de Mobile Analytics.  Hasta entonces, todos los datos de análisis capturados se almacenan localmente en el dispositivo.
    {: web}

5. Para capturar y enviar registros de aplicaciones al servicio de Mobile Analytics, utilice las API de registrador de SDK de cliente de Mobile Analytics.
   La infraestructura de registro del SDK de cliente de Mobile Analytics admite los niveles de registro siguientes, listados de menor a mayor detalle, con las directrices de uso recomendadas:
    * FATAL: se usa para bloqueos irrecuperables. El nivel FATAL está reservado para errores de registro irrecuperables que ven los usuarios como un bloqueo de aplicación
    * ERROR: uso para excepciones inesperadas o errores de protocolo de red inesperados
    * WARN: se usa para registrar advertencias de uso que no se consideran errores críticos, como el uso de API en desuso o en caso de una respuesta de red lenta
    * INFO: se usa para registrar sucesos de inicialización y otros datos que podrían ser importantes, pero no urgentes
    * DEBUG: se usa para notificar sentencias de depuración para que los desarrolladores puedan resolver defectos de la aplicación.
    Cuando el nivel de registro es DEBUG, también obtiene registros del SDK de cliente de Mobile Analytics, que se incluye cuando se envían los registros.
    * TRACE - utilizado para puntos de entrada y salida de método
   {: note}
   {: web}

   Los siguientes fragmentos de código muestran el ejemplo de uso del registrador para la app web:
   ```Javascript
    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish

    var logger = ibmmfpfanalytics.logger.pkg("loggerName");
    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug message");
    logger.info("info message");

    // Send logs to the Mobile Analytics
    ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

   Con el SDK web, el cliente no puede establecer el nivel. Se envían todos los registros al servidor hasta que se cambia la configuración al recuperar el perfil de configuración del servidor.
   {: note}
   {: web}

6. Para obtener información sobre los patrones de incorporación de usuario (nuevos usuarios frente a usuarios que vuelven), debe asociar una identidad de usuario con la sesión de la aplicación.  Esto se puede realizar llamando a la API siguiente
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    Todos los datos de análisis capturados y almacenados localmente se envían al servicio de Mobile Analytics sólo cuando se llama a la API ibmmfpfanalytics.send().
    {: web}

7.  Para obtener información sobre los patrones de interacción de la red HTTP de las aplicaciones, como por ejemplo las API HTTP llamadas, el número de solicitudes y los tiempos medios de respuesta, debe utilizar la clase WLResourceRequest del SDK de cliente de servicio de Mobile Foundation para realizar las llamadas HTTP.  Aunque haya inicializado Analytics para capturar sucesos de red, el uso de `WLResourceRequest` permite a Mobile Analytics acceder a las llamadas de red y capturar los datos relevantes.  Realice las llamadas de HTTP utilizando el código siguiente.
    ```Javascript
      var resourceRequest = new WL.ResourceRequest("url_path", WLResourceRequest.GET);
      resourceRequest.send().then(function(response) {
            console.log('handle success' + JSON.stringify(data));
        },function(error){
            console.log('handle failure: ' + error);
        });
    ```
    {: codeblock}
    {: web}

8.  Para definir análisis personalizados y definir sus propios datos de análisis adicionales a los admitidos de forma inherente en el SDK de cliente, puede utilizar la API de registro personalizado:

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    Los datos personalizados registrados se pueden trazar sobre gráficos personalizados que puede definir en la consola de Mobile Analytics para obtener información personalizada.
    {: web}

## ¿Cuál es el siguiente paso?
{: #next_steps_analytics}

Pruebe un ejemplo sencillo desde [aquí](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples).  En menos de 5 minutos podrá ejecutar la aplicación de ejemplo y familiarizarse con lo que hace la API. También podrá ver cómo se muestra el análisis en la consola de Analytics como diferente información de valor.  

Mobile Foundation Analytics Service proporciona API REST para ayudar a los desarrolladores a importar (POST) y a exportar (GET) datos de análisis.

Pruebe la API REST de análisis en Swagger Docs desde [aquí](https://ma-server.us-south.mobile-analytics-prod.cloud.ibm.com/analytics-service/).
