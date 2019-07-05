---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, set up android app for notification, set up iOS app for notification, set up cordova app for notification, set up windows app for notification

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
{:windows: .ph data-hd-programlang='Windows'}


# Manejo de las notificaciones push en aplicaciones cliente
{: #handling_push_notifications_in_client_applications}

Antes de que aplicaciones basadas en Cordova o basadas en iOS, Android y Windows nativo puedan recibir y visualizar notificaciones push entrantes, primero se debe configurar la aplicación y se deben implementar las API.
{: shortdesc}

Consulte las secciones siguientes para aprender a manejar las notificaciones push entrantes en aplicaciones cliente:

### Manejo de las notificaciones push en Android
{: #handling_push_notifications_in_android}
{: android}
Antes que las aplicaciones Android puedan manejar las notificaciones push que reciban, es necesario configurar el soporte para Google Play Services. Una vez se haya configurado la aplicación, se puede utilizar la API de notificaciones que {{ site.data.keyword.mobilefirst_notm }} proporciona con el propósito de registrar y anular el registro de dispositivos y suscribir y anular la suscripción a etiquetas. En esta guía de aprendizaje, aprenderá a manejar el envío de notificaciones en aplicaciones Android.
{: android}

#### Requisitos previos
{: #prereqs-andriod}
{: android}
* {{ site.data.keyword.mfserver_short_notm }} para ejecutar localmente o {{ site.data.keyword.mfserver_short_notm }} ejecutándose de forma remota.
* CLI de {{ site.data.keyword.mobilefirst_notm  }} instalada en la estación de trabajo del desarrollador
{: android}

#### Configuración de notificaciones
{: #notifications-configuration }
{: android}

Cree un nuevo proyecto de Android Studio o utilice uno que ya exista.  
Si {{ site.data.keyword.mobilefirst_notm }} Native Android SDK todavía no está presente en el proyecto, siga las instrucciones en la guía de aprendizaje [Adición de {{ site.data.keyword.mobilefoundation_short }} SDK para aplicaciones Android](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app).
{: android}

##### Configurar el proyecto
{: #project-setup }
{: android}

1. En **Android → Scripts de Gradle**, seleccione el archivo **build.gradle (Módulo: app)** y añada las siguientes líneas a `dependencies`:
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    Hay un [defecto conocido de Google](https://code.google.com/p/android/issues/detail?id=212879) que impide el uso de la versión más reciente de Play Services (actualmente la 9.2.0). Utilice una versión menor que la 9.2.0.
    {: note}
    {: android}

    Y:
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    O en una sola línea:
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. En **Android → app → manifiestos**, abra el archivo `AndroidManifest.xml`.
    * Añada los siguientes permisos en la parte superior de la etiqueta `manifest`:

        ```xml
        <!-- Permissions -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

        <!-- GCM Permissions -->
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <permission
    	     android:name="your.application.package.name.permission.C2D_MESSAGE"
    	     android:protectionLevel="signature" />
        ```
        {: codeblock}
        {: android}

    * Añada lo siguiente a la etiqueta `application`:

        ```xml
        <!-- GCM Receiver -->
        <receiver
             android:name="com.google.android.gms.gcm.GcmReceiver"
             android:exported="true"
             android:permission="com.google.android.c2dm.permission.SEND">
             <intent-filter>
                 <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                 <category android:name="your.application.package.name" />
             </intent-filter>
        </receiver>

        <!-- MFPPush Intent Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.c2dm.intent.RECEIVE" />
              </intent-filter>
        </service>

        <!-- MFPPush Instance ID Listener Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.gms.iid.InstanceID" />
              </intent-filter>
        </service>

        <activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler"
             android:theme="@android:style/Theme.NoDisplay"/>
 	    ```
      {: codeblock}
      {: android}

      Asegúrese de sustituir `your.application.package.name` por el nombre real del paquete de la aplicación.
      {: note}
      {: android}

    * Añada el siguiente `intent-filter` a la actividad de la aplicación.
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### API de notificaciones
{: #notifications-api }
{: android}

##### Instancia de MFPPush
{: #mfppush-instance }
{: android}

Todas las llamadas de API se deben realizar en una instancia de `MFPPush`.  Esto se puede realizar creando un campo de nivel de clase como, por ejemplo, `private MFPPush push = MFPPush.getInstance();` y a continuación, llamando a `push.<api-call>` a través de la clase.
{: android}

Como alternativa, puede llamar a `MFPPush.getInstance().<api_call>` para cada instancia en la que necesita acceder a los métodos de API de push.
{: android}

##### Manejadores de desafíos
{: #challenge-handlers }
{: android}

Si el ámbito de `push.mobileclient` está correlacionado con la **comprobación de seguridad**, debe asegurarse de que existen **manejadores de desafíos** coincidentes registrados antes de utilizar las API de push.
{: android}

Obtenga más información sobre los manejadores de desafíos en la guía de aprendizaje de
[validación de credenciales ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/).
{: note}
{: android}

##### Lado del cliente
{: #client-side }
{: android}

| Métodos Java | Descripción |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | Inicia MFPPush con el contexto proporcionado. |
| [`isPushSupported();`](#is-push-supported) | Indica si el dispositivo da soporte a notificaciones push. |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | Registra el dispositivo con el servicio de notificaciones push. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | Recupera las etiquetas disponibles en una instancia del servicio de notificaciones push. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | Suscribe el dispositivo para las etiquetas especificadas. |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | Recupera todas las etiquetas a las que el dispositivo está actualmente suscrito. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | Anula la suscripción de una o varias etiquetas. |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | Anula el registro del dispositivo del servicio notificaciones push. |
{: caption="Tabla 1. Métodos Java" caption-side="top"}
{: android}

###### Inicialización
{: #initialization}
{: android}

Requerido para la aplicación de cliente para conectarse al servicio MFPPush con el contexto de aplicación correcto.
{: android}

* Primero se debe llamar al método de la API antes de utilizar cualquier otra API MFPPush.
* Registra la función de retorno de llamada para manejar las notificaciones push recibidas.
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### Está push soportado
{: #is-push-supported }
{: android}

Comprueba si el dispositivo da soporte a las notificaciones push.
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: android}

###### Registrar el dispositivo
{: #register-device }
{: android}

Registre el dispositivo para el servicio de notificaciones push.
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        // Successfully registered
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Registration failed with error
    }
});
```
{: codeblock}
{: android}

###### Obtener etiquetas
{: #get-tags }
{: android}

Recupere todas las etiquetas disponibles desde el servicio de notificaciones push.
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully retrieved tags as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to receive tags with error
    }
});
```
{: codeblock}
{: android}

###### Suscribir
{: #subscribe }
{: android}

Suscríbase a las etiquetas deseadas.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Subscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to subscribe
    }
});
```
{: codeblock}
{: android}

###### Obtener suscripciones
{: #get-subscriptions }
{: android}

Recupere las etiquetas a las que el dispositivo está actualmente suscrito.
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully received subscriptions as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to retrieve subscriptions with error
    }
});
```
{: codeblock}
{: android}

###### Eliminar suscripción
{: #unsubscribe }
{: android}

Anule la suscripción de etiquetas.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Unsubscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unsubscribe
    }
});
```
{: codeblock}
{: android}

###### Anular el registro
{: #unregister }
{: android}

Anule el registro del dispositivo de una instancia de servicio de notificaciones push.
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Unregistered successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unregister
    }
});
```
{: codeblock}
{: android}

#### Manejo de una notificación push
{: #handling-a-push-notification }
{: android}

Con el propósito de manejar una notificación push será necesario configurar un `MFPPushNotificationListener`.  Esto se puede conseguir implementando uno de los métodos siguientes.
{: android}

##### Primera opción
{: #option-one }
{: android}

En la actividad en que desea manejar las notificaciones push.
{: android}

1. Añada `implements MFPPushNofiticationListener` a la declaración de la clase.
2. Establezca la clase para ser el escucha llamando a `MFPPush.getInstance().listen(this)` en el método `onCreate`.
2. Necesitará añadir el siguiente método *required*:
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. En este método recibirá `MFPSimplePushNotification` y podrá manejar la notificación para el comportamiento deseado.
{: android}

##### Segunda opción
{: #option-two }
{: android}

Cree un escucha llamando a `listen(new MFPPushNofiticationListener())` en una instancia de `MFPPush` tal como se indica en el ejemplo siguiente:
```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Handle push notification here
    }
});
```
{: codeblock}
{: android}

#### Migrar las apps cliente en Android a FCM
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) está [en desuso](https://developers.google.com/cloud-messaging/faq) y se ha integrado con Firebase Cloud Messaging (FCM). Google desactivará la mayoría de los servicios de GCM en abril de 2019.
{: android}

Si utiliza un proyecto de GCM, [a continuación migre las apps cliente de GCM en Android a FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: android}

Por ahora, las aplicaciones existentes que utilizan servicios de GCM seguirán funcionando tal cual. Dado que el servicio de Notificaciones push se ha actualizado para utilizar los puntos finales de FCM, en el futuro todas las nuevas aplicaciones deben utilizar FCM.
{: android}

Tras la migración a FCM, actualice el proyecto para que utilice las credenciales de FCM en lugar de las antiguas credenciales de GCM.
{: note}
{: android}

##### Configuración del proyecto de FCM
{: #fcm_project_setup}
{: android}

La configuración de una aplicación en FCM es algo distinta en comparación con el antiguo modelo de GCM.
{: android}

1. Obtenga sus credenciales del proveedor de notificaciones, cree un proyecto de FCM y añada el mismo a la aplicación de Android. Incluya el nombre del paquete de la aplicación como `com.ibm.mobilefirstplatform.clientsdk.android.push`. Consulte la [documentación aquí](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android), hasta el paso donde haya terminado de generar el archivo `google-services.json`
   {: android}
2. Configure el archivo de Gradle. Añada lo siguiente en el archivo `build.gradle` de la app
    ```xml
    dependencies {
       ......
       compile 'com.google.firebase:firebase-messaging:10.2.6'
       .....
    }

    apply plugin: 'com.google.gms.google-services'
    ```
    {: codeblock}
    {: android}

    - Añada la dependencia siguiente en la sección `buildscript` de build.gradle raíz
`classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - Elimine la parte posterior al plugin GCM del archivo build.gradle `compile com.google.android.gms:play-services-gcm:+`
     {: android}
 3. Configure el archivo AndroidManifest. Son necesarios los cambios siguientes en el `AndroidManifest.xml`
    {: android}

    Elimine las entradas siguientes:
    {: android}
    ```xml
        <receiver android:exported="true" android:name="com.google.android.gms.gcm.GcmReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="your.application.package.name" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <category android:name="your.application.package.name" />
            </intent-filter>
        </receiver>  

        <service android:exported="false" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID" />
            </intent-filter>
        </service>

        <uses-permission android:name="your.application.package.name.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```
    {: codeblock}
    {: android}

    Es necesario modificar las entradas siguientes:
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    Modifique las entradas a:
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    Añada la entrada siguiente:
    {: android}

    ```xml
        <service android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPush"
                android:exported="true">
                <intent-filter>
                    <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
                </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

 4. Abra la app en Android Studio. Copie el archivo `google-services.json` que ha creado en el **step-1** dentro del directorio de la app. Tenga en cuenta que el archivo `google-service.json` incluye el nombre del paquete que ha añadido.		
 5. Compile el SDK. Compile la aplicación.
{: android}

### Manejo de las notificaciones push en iOS
{: #handling_push_notifications_in_ios }
{: ios}

La API de notificaciones que {{ site.data.keyword.mobilefirst_notm }} proporciona sirve para registrar y anular el registro de dispositivos y suscribir y anular la suscripción a etiquetas. En esta guía de aprendizaje, aprenderá a manejar el envío de notificaciones en aplicaciones iOS utilizando Swift.
{: shortdesc}
{: ios}

Para obtener información sobre las notificaciones interactivas o silenciosas, consulte:

* [Notificaciones silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notificaciones interactivas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### Requisitos previos
{: #prereqs-ios}
{: ios}

* {{ site.data.keyword.mfserver_short }} para ejecutar localmente o {{ site.data.keyword.mfserver_short }} ejecutándose de forma remota.
* {{ site.data.keyword.mfp_cli_long_notm }} instalado en la estación de trabajo del desarrollador
{: ios}

#### Configuración de notificaciones
{: #notifications-configuration_ios}
{: ios}

Cree un nuevo proyecto Xcode o utilice uno existente.
Si {{ site.data.keyword.mobilefirst_notm }} Native iOS SDK todavía no está presente en el proyecto, siga las instrucciones en la guía de aprendizaje [Adición de {{ site.data.keyword.mobilefoundation_short }} SDK para aplicaciones iOS](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: ios}

#### Añadir el SDK de push
{: #adding-the-push-sdk }
{: ios}

1. Abra el **podfile** existente del proyecto y añada las siguientes líneas:
    ```xml
    use_frameworks!

    platform :ios, 8.0
    target "Xcode-project-target" do
         pod 'IBMMobileFirstPlatformFoundation'
         pod 'IBMMobileFirstPlatformFoundationPush'
    end

    post_install do |installer|
         workDir = Dir.pwd

         installer.pods_project.targets.each do |target|
             debugXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.debug.xcconfig"
             xcconfig = File.read(debugXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(debugXcconfigFilename, "w") { |file| file << newXcconfig }

             releaseXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.release.xcconfig"
             xcconfig = File.read(releaseXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(releaseXcconfigFilename, "w") { |file| file << newXcconfig }
         end
    end
    ```
    {: codeblock}
    {: ios}

    - Sustituya **Xcode-project-target** por el nombre de su proyecto Xcode de destino.
2. Guarde y cierre el **podfile**.
3. Desde una ventana de **línea de mandatos**, vaya a la carpeta raíz del proyecto.
4. Ejecute el mandato `pod install`
5. Abra proyecto utilizando el archivo **.xcworkspace**.
{: ios}

#### API de notificaciones
{: #notifications-api-ios}
{: ios}

##### Instancia de MFPPush
{: #mfppush-instance-ios}
{: ios}

Todas las llamadas de API se deben realizar en una instancia de `MFPPush`. Esto se puede realizar utilizando una `var` en un controlador de vista, por ejemplo, `var push = MFPPush.sharedInstance();` y, a continuación, llamando a `push.methodName()` a través del controlador de vista.
{: ios}

Otra posibilidad es llamar a `MFPPush.sharedInstance().methodName()` para cada instancia en la que necesita acceder a los métodos de API de push.
{: ios}

#### Manejadores de desafíos
{: #challenge-handlers-ios}
{: ios}

Si el ámbito de `push.mobileclient` está correlacionado con la **comprobación de seguridad**, debe asegurarse de que existen **manejadores de desafíos** coincidentes registrados antes de utilizar las API de push.
{: ios}

Obtenga más información sobre los manejadores de desafíos en la guía de aprendizaje de
[validación de credenciales ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/).
{: note}
{: ios}

#### Lado del cliente
{: #client-side-ios}
{: ios}

| Métodos Swift | Descripción  |
|---------------|--------------|
| [`initialize()`](#initialization) | Inicia MFPPush con el contexto proporcionado. |
| [`isPushSupported()`](#is-push-supported) | Indica si el dispositivo da soporte a notificaciones push. |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | Registra el dispositivo con el servicio de notificaciones push.|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | Envía la señal de dispositivo al servidor |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | Recupera las etiquetas disponibles en una instancia del servicio de notificaciones push. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | Suscribe el dispositivo para las etiquetas especificadas. |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | Recupera todas las etiquetas a las que el dispositivo está actualmente suscrito. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | Anula la suscripción de una o varias etiquetas. |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | Anula el registro del dispositivo del servicio notificaciones push.              |
{: caption="Tabla 2. Métodos Swift" caption-side="top"}
{: ios}

##### Inicialización
{: #initialization-ios}
{: ios}

La inicialización es necesaria para que la aplicación de cliente se conecte al servicio MFPPush.
{: ios}

* Primero se debe llamar al método `initialize` antes de utilizar cualquier otra API MFPPush.
* Registra la función de retorno de llamada para manejar las notificaciones push recibidas.
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### Está push soportado
{: #is-push-supported-ios}
{: ios}

Comprueba si el dispositivo da soporte a las notificaciones push.
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: ios}

##### Registrar el dispositivo y enviar una señal de dispositivo
{: #register-device--send-device-token-ios}
{: ios}

Registre el dispositivo para el servicio de notificaciones push.
{: ios}

```swift
MFPPush.sharedInstance().registerDevice(nil) { (response, error) -> Void in
    if error == nil {
        self.enableButtons()
        self.showAlert("Registered successfully")
        print(response?.description ?? "")
    } else {
        self.showAlert("Registrations failed.  Error \(error?.localizedDescription)")
        print(error?.localizedDescription ?? "")
    }
}
```
{: codeblock}
{: ios}

<!--`options` = `[NSObject : AnyObject]` which is an optional parameter that is a dictionary of options to be passed with your register request, sends the device token to the server to register the device with its unique identifier.-->

```swift
MFPPush.sharedInstance().sendDeviceToken(deviceToken)
```
{: codeblock}
{: ios}

Normalmente, se llama a esto en **AppDelegate** en el método `didRegisterForRemoteNotificationsWithDeviceToken`.
{: note}
{: ios}

##### Obtener etiquetas
{: #get-tags-ios}
{: ios}

Recupere todas las etiquetas disponibles desde el servicio de notificaciones push.
{: ios}

```swift
MFPPush.sharedInstance().getTags { (response, error) -> Void in
    if error == nil {
        print("The response is: \(response)")
        print("The response text is \(response?.responseText)")
        if response?.availableTags().isEmpty == true {
            self.tagsArray = []
            self.showAlert("There are no available tags")
        } else {
            self.tagsArray = response!.availableTags() as! [String]
            self.showAlert(String(describing: self.tagsArray))
            print("Tags response: \(response)")
        }
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Suscribir
{: #subscribe-ios}
{: ios}

Suscríbase a las etiquetas deseadas.
{: ios}

```swift
var tagsArray: [String] = ["Tag 1", "Tag 2"]

MFPPush.sharedInstance().subscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Subscribed successfully")
        print("Subscribed successfully response: \(response)")
    } else {
        self.showAlert("Failed to subscribe")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Obtener suscripciones
{: #get-subscriptions-ios}
{: ios}

Recupere las etiquetas a las que el dispositivo está actualmente suscrito.
{: ios}

```swift
MFPPush.sharedInstance().getSubscriptions { (response, error) -> Void in
   if error == nil {
       var tags = [String]()
       let json = (response?.responseJSON)! as [AnyHashable: Any]
       let subscriptions = json["subscriptions"] as? [[String: AnyObject]]
       for tag in subscriptions! {
           if let tagName = tag["tagName"] as? String {
               print("tagName: \(tagName)")
               tags.append(tagName)
           }
       }
       self.showAlert(String(describing: tags))
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

##### Eliminar suscripción
{: #unsubscribe-ios}
{: ios}

Anule la suscripción de etiquetas.
{: ios}

```swift
var tags: [String] = {"Tag 1", "Tag 2"};

// Unsubscribe from tags
MFPPush.sharedInstance().unsubscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Unsubscribed successfully")
        print(String(describing: response?.description))
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Anular el registro
{: #unregister-ios}
{: ios}

Anule el registro del dispositivo de una instancia de servicio de notificaciones push.
{: ios}

```swift
MFPPush.sharedInstance().unregisterDevice { (response, error)  -> Void in
   if error == nil {
       // Disable buttons
       self.disableButtons()
       self.showAlert("Unregistered successfully")
       print("Subscribed successfully response: \(response)")
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

#### Manejo de una notificación push
{: #handling-a-push-notification-ios}
{: ios}

La infraestructura iOS nativa maneja directamente las notificaciones push. En función del ciclo de vida de su aplicación, la infraestructura de iOS llamará a métodos distintos.
{: ios}

Por ejemplo si se recibe una notificación simple mientras se ejecuta la aplicación, se desencadenará el `didReceiveRemoteNotification` de **AppDelegate**:
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // display the alert body
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

Obtenga más información sobre el manejo de notificaciones en iOS en la
[Documentación de Apple ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).
{: note}
{: ios}

### Manejo de las notificaciones push en Cordova
{: #handling_push_notifications_in_cordova }
{: cordova}

Antes de que las aplicaciones iOS, Android y Windows Cordova reciban y visualicen notificaciones push, es necesario añadir el plugin Cordova **cordova-plugin-mfp-push** al proyecto Cordova. Una vez se haya configurado la aplicación, se pueden utilizar la API de notificaciones que {{ site.data.keyword.mobilefirst_notm }} proporciona con el propósito de registrar y anular el registro de dispositivos, suscribir y anular la suscripción de etiquetas y manejar aplicaciones. En esta guía de aprendizaje, aprenderá a manejar el envío de notificaciones en aplicaciones Cordova.
{: shortdesc}
{: cordova}

Actualmente **no hay soporte** para notificaciones autenticadas en las aplicaciones Cordova debido a un defecto. Sin embargo se proporciona un método alternativo: cada llamada de API de `MFPPush` puede ser acomodada mediante `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );`.
{: note}
{: cordova}

Para obtener información sobre las notificaciones interactivas o silenciosas en iOS, consulte:
{: cordova}

* [Notificaciones silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notificaciones interactivas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### Requisitos previos
{: #prereqs-cordova}
{: cordova}

* {{ site.data.keyword.mfserver_short }} para ejecutar localmente, o un remotamente ejecutando {{ site.data.keyword.mfserver_short }}
* {{ site.data.keyword.mfp_cli_long_notm }} instalado en la estación de trabajo del desarrollador
* Interfaz de línea de mandatos (CLI) de Cordova instalada en la estación de trabajo del desarrollador
{: cordova}

#### Configuración de notificaciones
{: #notifications-configuration-cordova}
{: cordova}

Cree un nuevo proyecto Cordova o utilice uno ya existente, y añada una o varias de las plataformas soportadas: iOS, Android, Windows.
{: cordova}

Si {{ site.data.keyword.mobilefirst_notm }} Cordova SDK todavía no está presente en el proyecto, siga las instrucciones en la guía de aprendizaje [Adición de {{ site.data.keyword.mobilefirst_notm }} SDK para aplicaciones Cordova](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: cordova}
{: note}

#### Añadir el plugin de push
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. Desde una ventana de **línea de mandatos**, vaya a la raíz del proyecto Cordova.  
2. Añada el plugin ejecutando el mandato:
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. Compile el proyecto Cordova ejecutando el mandato:
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### Plataforma iOS
{: #ios-platform }
{: cordova}

La plataforma iOS precisa de un paso adicional.  
{: cordova}

En Xcode, habilite el envío de notificaciones para la aplicación en la pantalla **Funcionalidades**.
{: cordova}

El bundleId (ID de paquete) seleccionado para la aplicación debe coincidir con el AppId (ID de app) que haya creado anteriormente en el sitio de desarrollador de Apple.
{: important}
{: cordova}

![Imagen del lugar donde está la función en Xcode](images/push-capability.png)
{: cordova}

#### Plataforma Android
{: #android-platform }
{: cordova}

La plataforma Android precisa de un paso adicional.  
{: cordova}

En Android Studio, añada la siguiente `actividad` a la etiqueta `application`:
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### API de notificaciones
{: #notifications-api-cordova}
{: cordova}

##### Lado del cliente
{: #client-side-cordova}
{: cordova}

| Función Javascript | Descripción |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | Inicializa la instancia MFPPush. |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) | Indica si el dispositivo da soporte a notificaciones push. |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) | Registra el dispositivo con el servicio de notificaciones push. |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) | Recupera todas las etiquetas disponibles en una instancia de servicio de notificaciones push. |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) | Suscribe a una etiqueta concreta. |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions-cordova) | Recupera las etiquetas de servicio a las que actualmente está suscrito. |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) | Anula la suscripción a una etiqueta concreta. |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | Anula el registro del dispositivo del servicio notificaciones push. |
{: caption="Tabla 3. Funciones Javascript" caption-side="top"}
{: cordova}

##### Implementación de API
{: #api-implementation }
{: cordova}

###### Inicialización
{: #initialization-cordova}
{: cordova}

Inicializa la instancia **MFPPush**.
{: cordova}

- Requerido para la aplicación de cliente para conectarse al servicio MFPPush con el contexto de aplicación correcto.  
- Primero se debe llamar al método de la API antes de utilizar cualquier otra API MFPPush.
- Registra la función de retorno de llamada para manejar las notificaciones push recibidas.
{: cordova}

```javascript
MFPPush.initialize (
    function(successResponse) {
        alert("Successfully intialized");
        MFPPush.registerNotificationsCallback(notificationReceived);
    },
    function(failureResponse) {
        alert("Failed to initialize");
    }
);
```
{: codeblock}
{: cordova}

###### Está push soportado
{: #is-push-supported-cordova}
{: cordova}

Comprueba si el dispositivo da soporte a las notificaciones push.
{: cordova}

```javascript
MFPPush.isPushSupported (
    function(successResponse) {
        alert("Push Supported: " + successResponse);
    },
    function(failureResponse) {
        alert("Failed to get push support status");
    }
);
```
{: codeblock}
{: cordova}

###### Registrar el dispositivo
{: #register-device-cordova}
{: cordova}

Registre el dispositivo para el servicio de notificaciones push. Si no se proporcionan opciones, se pueden establecer en `null`.
{: cordova}

```javascript
var options = { };
MFPPush.registerDevice(
    options,
    function(successResponse) {
        alert("Successfully registered");
    },
    function(failureResponse) {
        alert("Failed to register");
    }
);
```
{: codeblock}
{: cordova}

###### Obtener etiquetas
{: #get-tags-cordova}
{: cordova}

Recupere todas las etiquetas disponibles desde el servicio de notificaciones push.
{: cordova}

```javascript
MFPPush.getTags (
    function(tags) {
        alert(JSON.stringify(tags));
},
    function() {
        alert("Failed to get tags");
    }
);
```
{: codeblock}
{: cordova}

###### Suscribir
{: #subscribe-cordova}
{: cordova}

Suscríbase a las etiquetas deseadas.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.subscribe(
    tags,
    function(tags) {
        alert("Subscribed successfully");
    },
    function() {
        alert("Failed to subscribe");
    }
);
```
{: codeblock}
{: cordova}

###### Obtener suscripciones
{: #get-subscriptions-cordova}
{: cordova}

Recupere las etiquetas a las que el dispositivo está actualmente suscrito.
{: cordova}

```javascript
MFPPush.getSubscriptions (
    function(subscriptions) {
        alert(JSON.stringify(subscriptions));
    },
    function() {
        alert("Failed to get subscriptions");
    }
);
```
{: codeblock}
{: cordova}

###### Eliminar suscripción
{: #unsubscribe-cordova}
{: cordova}

Anule la suscripción de etiquetas.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.unsubscribe(
    tags,
    function(tags) {
        alert("Unsubscribed successfully");
    },
    function() {
        alert("Failed to unsubscribe");
    }
);
```
{: codeblock}
{: cordova}

###### Anular el registro
{: #unregister-cordova}
{: cordova}

Anule el registro del dispositivo de una instancia de servicio de notificaciones push.
{: cordova}

```javascript
MFPPush.unregisterDevice(
    function(successResponse) {
        alert("Unregistered successfully");
    },
    function() {
        alert("Failed to unregister");
    }
);
```
{: codeblock}
{: cordova}

#### Manejo de una notificación push
{: #handling-a-push-notification-cordova}
{: cordova}

Las notificaciones push recibidas se pueden manejar trabajando con el objeto de respuesta en la función de retorno registrada.
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Manejo de las notificaciones push en Windows 8.1 Universal y Windows 10 UWP
{: #handling_push_notifications_in_windows }
{: windows}

La API de notificaciones que {{ site.data.keyword.mobilefirst_notm }} proporciona sirve para registrar y anular el registro de dispositivos y suscribir y anular la suscripción a etiquetas. En esta guía de aprendizaje, aprenderá a manejar el envío de notificaciones en aplicaciones nativas Windows 8.1 Universal y Windows 10 UWP mediante C#.
{: windows}

#### Requisitos previos
{: #prereqs-windows}
{: windows}

* {{ site.data.keyword.mfserver_short_notm }} para ejecutar localmente o {{ site.data.keyword.mfserver_short_notm }} ejecutándose de forma remota.
* CLI de {{ site.data.keyword.mobilefirst_notm  }} instalada en la estación de trabajo del desarrollador
{: windows}

#### Configuración de notificaciones
{: #notifications-configuration-windows}
{: windows}

Cree un nuevo proyecto de Visual Studio o utilice uno que ya exista.  
{: windows}

Si {{ site.data.keyword.mobilefirst_notm }} Native Windows SDK todavía no está presente en el proyecto, siga las instrucciones en la guía de aprendizaje [Adición de {{ site.data.keyword.mobilefirst_notm }} SDK para aplicaciones Windows](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/).
{: windows}

#### Añadir el SDK de push
{: #adding-the-push-sdk-windows}
{: windows}

1. Seleccione Herramientas → NuGet Package Manager → Package Manager Console.
2. Elija el proyecto en el que desea instalar el componente push de {{ site.data.keyword.mobilefirst_notm }}.
3. Añada el SDK de push de {{ site.data.keyword.mobilefirst_notm }} ejecutando el mandato **Install-Package IBM.MobileFirstPlatformFoundationPush**.
{: windows}

#### Requisito previo de configuración de WNS
{: pre-requisite-wns-configuration }
{: windows}

1. Asegúrese de que la aplicación tiene la funcionalidad de notificación Toast. Esta funcionalidad se habilita en Package.appxmanifest.
2. Asegúrese de que `Package Identity Name` y `Publisher` se actualicen con los valores registrados con WNS.
3. (Opcional) Suprima el archivo TemporaryKey.pfx.
{: windows}

#### API de notificaciones
{: #notifications-api-windows}
{: windows}

##### Instancia de MFPPush
{: #mfppush-instance-windows}
{: windows}

Todas las llamadas de API se deben realizar en una instancia de `MFPPush`.  Esto se puede realizar creando una variable como, por ejemplo, `private MFPPush PushClient = MFPPush.GetInstance();` y, a continuación, llamando a `PushClient.methodName()` a lo largo de la clase.
{: windows}

Otra posibilidad es llamar a `MFPPush.GetInstance().methodName()` para cada instancia en la que necesita acceder a los métodos de API de push.
{: windows}

##### Manejadores de desafíos
{: #challenge-handlers-windows}
{: windows}

Si el ámbito de `push.mobileclient` está correlacionado con la **comprobación de seguridad**, debe asegurarse de que existen **manejadores de desafíos** coincidentes registrados antes de utilizar las API de push.
{: windows}

Obtenga más información sobre los manejadores de desafíos en la guía de aprendizaje de
[validación de credenciales ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/).
{: note}
{: windows}

#### Lado del cliente
{: #client-side-windows}
{: windows}

| Métodos de C Sharp                                                                                                | Descripción                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            | Inicia MFPPush con el contexto proporcionado.                               |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    | Indica si el dispositivo da soporte a notificaciones push.                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  | Registra el dispositivo con el servicio de notificaciones push.               |
| [`GetTags()`](#get-tags-windows)                                | Recupera las etiquetas disponibles en una instancia del servicio de notificaciones push. |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     | Suscribe el dispositivo para las etiquetas especificadas.                          |
| [`GetSubscriptions()`](#get-subscriptions-windows)              | Recupera todas las etiquetas a las que el dispositivo está actualmente suscrito.               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) | Anula la suscripción de una o varias etiquetas.                                  |
| [`UnregisterDevice()`](#unregister-windows)                     | Anula el registro del dispositivo del servicio notificaciones push.              |
{: caption="Tabla 4. Métodos C Sharp" caption-side="top"}
{: windows}

##### Inicialización
{: #initialization-windows}
{: windows}

La inicialización es necesaria para que la aplicación de cliente se conecte al servicio MFPPush.
{: windows}

* Primero se debe llamar al método `Initialize` antes de utilizar cualquier otra API MFPPush.
* Registra la función de retorno de llamada para manejar las notificaciones push recibidas.
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### Está push soportado
{: #is-push-supported-windows}
{: windows}

Comprueba si el dispositivo da soporte a las notificaciones push.
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: windows}

##### Registrar el dispositivo y enviar una señal de dispositivo
{: #register-device--send-device-token-windows}
{: windows}

Registre el dispositivo para el servicio de notificaciones push.
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

##### Obtener etiquetas
{: #get-tags-windows}
{: windows}

Recupere todas las etiquetas disponibles desde el servicio de notificaciones push.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetTags();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
} else{
    Message = new MessageDialog("Failed to get Tags list");
}
```
{: codeblock}
{: windows}

##### Suscribir
{: #subscribe-windows}
{: windows}

Suscríbase a las etiquetas deseadas.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Get subscription tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //successfully subscribed to push tag
}
else
{
    //failed to subscribe to push tags
}
```
{: codeblock}
{: windows}

##### Obtener suscripciones
{: #get-subscriptions-windows}
{: windows}

Recupere las etiquetas a las que el dispositivo está actualmente suscrito.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetSubscriptions();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
}
else
{
    Message = new MessageDialog("Failed to get subcription list...");
}
```
{: codeblock}
{: windows}

##### Eliminar suscripción
{: #unsubscribe-windows}
{: windows}

Anule la suscripción de etiquetas.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// unsubscribe tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //succes
}
else
{
    //failed to subscribe to tags
}
```
{: codeblock}
{: windows}

##### Anular el registro
{: #unregister-windows}
{: windows}

Anule el registro del dispositivo de una instancia de servicio de notificaciones push.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

#### Manejo de una notificación push
{: #handling-a-push-notification-windows}
{: windows}

Con el propósito de manejar una notificación push será necesario configurar un `MFPPushNotificationListener`.  Esto se puede lograr implementando el siguiente método.
{: windows}

1. Cree una clase utilizando la interfaz de tipo MFPPushNotificationListener

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Handle push notification here      
    }
    }
    ```
    {: codeblock}
{: windows}
2. Establezca la clase que debe hacer de escucha llamando a `MFPPush.GetInstance().listen(new NotificationListner())`
3. En el método onReceive recibirá la notificación push y podrá manejarla para el comportamiento que desee.
{: windows}

#### Servicio de notificaciones push de Windows Universal
{: #windows-universal-push-notifications-service }
{: windows}

No es necesario abrir un puerto específico en su configuración de servidor.
{: windows}

WNS utiliza solicitudes http o https estándar.
{: windows}
