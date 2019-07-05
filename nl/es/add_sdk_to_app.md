---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Mobile Foundation SDK, android sdk, iOS sdk, cordova sdk, react native sdk

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
{:reactnative: .ph data-hd-programlang='reactnative'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}

#	Añadir SDK de Mobile Foundation a una app
{: #add_sdk_to_app}

### Adición del SDK de Android a la app
{: android}

Abra Android Studio, elija la vista de Android y elija **Scripts de Gradle**. Seleccione el archivo `build.gradle (Module: app)` y, a continuación, realice los pasos siguientes para añadir el SDK de Android a la aplicación android.
{: android}

1. Añada la línea siguiente a la sección `dependencies`.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. Añada la línea siguiente a la sección `android`.
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. En la vista Android, abra el archivo **app → manifests → AndroidManifest.xml**. Añada los permisos siguientes antes del elemento `application`.
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. Añada la actividad de interfaz de usuario de MobileFirst junto al elemento `activity` existente.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### Adición del SDK de iOS a la app
{: ios}

Este es el SDK principal para IBM Mobile Foundation, que consta de API para implementar la seguridad, la autorización, el registro, los adaptadores de llamadas y otras funciones principales de Mobile Foundation. Realice los pasos siguientes para añadir el SDK de iOS a su aplicación iOS.
{: ios}

1. Vaya a la carpeta raíz de la app iOS, ejecute el mandato siguiente para crear un Podfile.
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. Con la ayuda de su editor preferido, abra el Podfile.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. Actualice el pod utilizando el mandato siguiente.
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. Instale el pod utilizando el mandato siguiente.
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. Abra [ProjectName].xcworkspace para abrir el proyecto en Xcode.
6. Añada el archivo `mfpclient.plist` al espacio de trabajo Xcode.
7. Desde un indicador de mandatos, navegue hasta la carpeta del proyecto y registre la app con la instancia de Mobile Foundation.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: ios}

### Adición del SDK de Cordova a la app
{: cordova}

Este SDK es el SDK principal para IBM Mobile Foundation, que consta de API para implementar la seguridad, la autorización, el registro, los adaptadores de llamadas y otras funciones principales de Mobile Foundation. Realice los pasos siguientes para añadir el SDK de Cordova a su aplicación.
{: cordova}

1. Cree un proyecto de Cordova.
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. Cambie el directorio a la raíz del proyecto de Cordova.
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Añada una o más plataformas soportadas al proyecto de Cordova utilizando la CLI de Cordova.
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Añada el plugin de Cordova principal de Mobile Foundation.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. Prepare los recursos de la aplicación ejecutando el mandato siguiente.
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Registre la aplicación en el servidor de Mobile Foundation.
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### Adición del plugin de SDK de React Native SDK a la app
{: reactnative}

Para añadir las prestaciones de Mobile Foundation a una app de React Native existente, debe añadir el plugin `react-native-ibm-mobilefirst` a la app. El plugin `react-native-ibm-mobilefirst` contiene el SDK de Mobile Foundation. Realice los pasos siguientes para añadir el plugin React Native a su aplicación React Native.
{: reactnative}

1. Añada este plugin de la misma forma que añade cualquier otro plugin `npm` a la app.
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. El primer paso consiste en crear un proyecto de React Native, por ejemplo, *MobileFirstApp*. Utilice la CLI de React Native para crear un nuevo proyecto.
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. A continuación, añada el plugin de React Native a la app.
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. Enlace el proyecto de modo que todas las dependencias nativas se añadan al proyecto de React Native.
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. Para Android, realice los campos siguientes en `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`).
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. Añada **tools:replace='android:allowBackup'** a la etiqueta `application`.
   ```xml
   <application
            android:name=".MainApplication"
            android:label="@string/app_name"
            android:icon="@mipmap/ic_launcher"
            android:allowBackup="false"
            android:theme="@style/AppTheme"
            tools:replace="android:allowBackup">
   ```
   {: codeblock}
   {: reactnative}
7. Para iOS, abra XCode, en el navegador del proyecto, arrastre y suelte `mfpclient.plist` de la carpeta `ios`. Este paso sólo es aplicable para la plataforma iOS.
{: reactnative}
