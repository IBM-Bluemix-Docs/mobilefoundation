---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: obfuscating resources, security

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

# Enmascarar recursos
{: #obfuscate_resources}

La ofuscación es el proceso de modificar un ejecutable de modo que ya no sea útil para un pirata informático, pero siga siendo plenamente funcional. La ofuscación de código automatizado hace que sea difícil la ingeniería inversa en un programa.

Por medio de la ofuscación, es mucho más difícil aplicar ingeniería inversa en una aplicación, y queda protegida ante el robo de secreto comercial (propiedad intelectual), el acceso no autorizado, la omisión de las licencias u otros controles y la vulnerabilidad.

La ofuscación de código consta de muchas técnicas diferentes y técnicas de seguridad de aplicaciones:

* Ofuscación de redenominación: la redenominación altera el nombre de los métodos y las variables y hace que el origen descompilado sea más difícil de entender para el ser humano, pero no altera la ejecución de la aplicación. Es el método preferido de ofuscación de .NET, iOS, Java y Android.
* Cifrado de serie
* Ofuscación de flujo de control
* Transformación de patrón de instrucción
* Inserción de código ficticio
* Evitar la manipulación, etc.

La ofuscación hace que sea mucho más difícil que los atacantes revisen el código y analicen la aplicación. Para la ofuscación, hay diversas herramientas de terceros disponibles.

* Para la ofuscación de una aplicación android, consulte este [blog](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/).

  Es necesario crear el archivo de configuración (proguard-project.txt) para la ofuscación de Android ProGuard con una aplicación Android de MobileFirst.
  {: note}

* Para la ofuscación básica de la aplicación de Cordova, consulte [Cifrado de aplicación de Cordova](#encryptingcordovapackage).

## Cifrado de recursos web de sus paquetes Cordova
{: #encryptingcordovapackage}

Para minimizar el riesgo de que alguien visualice y modifique los recursos web, mientras está en el paquete
`.apk` o `.ipa`, puede utilizar el mandato de CLI de MobileFirst siguiente:
```bash
mfpdev app webencrypt
```
{: codeblock}
o el distintivo `mfpwebencrypt` para cifrar la información. Este procedimiento no proporciona un cifrado inviolable, pero sí proporciona un nivel básico de enmascaramiento.

### Requisitos previos

* Debe instalar las herramientas de desarrollo de Cordova. Este ejemplo utiliza la interfaz de línea de mandatos (CLI) de Cordova. Si utiliza otras herramientas de desarrollo de Cordova, algunos de los pasos serán distintos. Consulte la documentación de su herramienta de Cordova para obtener instrucciones.
* Debe instalar la CLI de MobileFirst.
* Debe instalar el plugin Cordova de MobileFirst.

El mejor momento para completar este procedimiento es al finalizar el desarrollo de su app y antes de desplegarla. Si ejecuta cualquiera de los siguientes mandatos después de completar el procedimiento de cifrado de los recursos web, el contenido que estaba cifrado pasará a estar descifrado.

* `cordova prepare`
* `cordova build`
* `cordova run`
* `cordova emulate`
* `mfpdev app webupdate`
* `mfpdev app preview`

Si ejecuta alguno de los mandatos de la lista después de cifrar los recursos web, debe completar este procedimiento de nuevo para cifrar los recursos web.

1. Abra una ventana de terminal y vaya al directorio raíz de la app Cordova que desea cifrar.
2. Prepare la app especificando uno de los siguientes mandatos:
    ```bash
      cordova prepare
    ```
    {: codeblock}
    ```bash
      mfpdev app webupdate
    ```
    {: codeblock}
3. Complete uno de los siguientes procedimientos para cifrar el contenido,
    * Especifique el mandato siguiente:
      ```bash
      mfpdev app webencrypt
      ```
      {: codeblock}

      Puede ver información acerca del mandato `mfpdev app webencrypt` especificando 
      ```bash
      mfpdev help app webencrypt
      ```
      {: codeblock}
      {: tip}

    * Los recursos web de sus paquetes de Cordova también se pueden cifrar añadiendo el distintivo `mfpwebencrypt` al mandato `cordova compile` o `cordova build` al compilar sus paquetes.
        ```bash
         cordova compile -- --mfpwebencrypt
         ```
         {: codeblock}
         o bien
         ```bash
         cordova build -- --mfpwebencrypt
         ```
         {: codeblock}
         La información sobre el sistema operativo en la carpeta **www**
se sustituye por un archivo **resources.zip** que contiene el contenido cifrado.
         Si su app está dirigida al sistema operativo Android y el archivo **resources.zip** es mayor de 1 MB, el archivo **resources.zip** se divide en archivos .zip de 768 KB más pequeños denominados **resources.zip.nnn**. La variable nnn es un número de 001 a 999.
4. Pruebe la aplicación con los recursos cifrados mediante el emulador que se proporciona con las herramientas específicas de la plataforma. Por ejemplo, se puede utilizar el emulador en Android Studio para Android o Xcode para iOS.

No utilice los siguientes mandatos Cordova para probar la aplicación después de cifrarla,
```bash
cordova run
```
{: codeblock}
```bash
cordova emulate
```
{: codeblock}
Estos mandatos renuevan el contenido cifrado en la carpeta www y lo guardan de nuevo como contenido descifrado. Si utiliza estos mandatos, recuerde completar el procedimiento de nuevo para cifrar antes de publicar la app.
{: note}
