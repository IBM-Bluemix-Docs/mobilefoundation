---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestionar versiones de apps
{: #manage_app_versions}

Las funciones de gestión de aplicaciones de Mobile Foundation proporcionan a los administradores y usuarios del servidor de Mobile Foundation un control detallado del acceso de usuarios y dispositivos a sus aplicaciones.

El servidor de Mobile Foundation realiza un seguimiento de todos los intentos de acceder a la infraestructura móvil y almacena información sobre la aplicación, el usuario y el dispositivo en el que se ha instalado la aplicación. La correlación entre la aplicación, el usuario y el dispositivo forma la base de las funciones de gestión de aplicaciones móviles del servidor.

Mediante la consola de operaciones de Mobile Foundation puede supervisar y gestionar el acceso a sus recursos. También puede gestionar la versión específica de la aplicación.

1.  Vaya a la consola de operaciones de Mobile Foundation, pulse en **Aplicaciones**, seleccione la aplicación que desea gestionar y seleccione la versión de la aplicación específica en la que está interesada de la lista de **Versiones** que se muestra.
    ![Manage application version](images/app_version_management.png)

2. En el separador **Gestión**, verá las opciones para establecer el estado de la aplicación de la versión de aplicación seleccionada. Los estados soportados de una versión de aplicación son,
   * Activo
   * Activo y notificando
   * Acceso inhabilitado
3. Se puede inhabilitar una versión de aplicación seleccionando la opción *Acceso inhabilitado* en **Acceso de aplicación > Estado**.
4. También puede establecer una configuración para cargar los recursos web actualizados de la aplicación Cordova en la sección **Direct Update**. A los usuarios que se conecten al servidor de Mobile Foundation utilizando esta versión de aplicación específica se les presenta la opción de actualizar su aplicación mediante Direct Update.
5. También puede llevar a cabo las acciones siguientes en la versión de aplicación seleccionada, utilizando las opciones siguientes en el menú **Acciones**:
   *  Suprimir versión
   *  Clonar versión
   *  Exportar versión


Para obtener más información sobre cómo gestionar los dispositivos, consulte
[Gestión de dispositivos](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices).
Para obtener más información sobre cómo inhabilitar de forma remota una versión de app, consulte
[Inhabilitar de forma remota una versión de app](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version).
{: note}
