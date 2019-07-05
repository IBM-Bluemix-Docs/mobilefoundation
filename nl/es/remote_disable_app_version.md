---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: disable apps, remote disabling of apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Inhabilitar de forma remota una versión de app
{: #remotely_disable_an_app_version}

En esta sección, trataremos cómo inhabilitar el acceso de usuarios a una versión concreta de una aplicación en un sistema operativo móvil específico y cómo proporcionar un mensaje personalizado al usuario.

Puede utilizar la consola de operaciones de Mobile Foundation para gestionar el acceso a la app.

1. Seleccione la versión de la aplicación desde la sección **Aplicaciones** de la barra lateral de navegación de la consola y seleccione el separador **Gestión** de la aplicación.
2. Cambie el estado a **Acceso inhabilitado**.
3. Proporcione un URL para la nueva versión de la aplicación (normalmente, en el almacén de apps público o privado adecuado) en el campo **URL de la última versión**.
   Para algunos entornos, el centro de aplicaciones proporciona un URL para poder acceder directamente a la vista de detalles de una versión de la aplicación. Consulte [Propiedades de aplicación](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties).
   {: tip}

4. En el **Mensaje de notificación predeterminado**, añada el mensaje de notificación personalizado a visualizar cuando el usuario intente acceder a la aplicación. El mensaje de ejemplo siguiente lleva a los usuarios a realizar una actualización de la versión más reciente: `Ya no se ofrece soporte a esta versión. Actualice a la versión más reciente.`
5. En la sección **Entornos locales soportados** puede proporcionar el mensaje de notificación en otros idiomas.
6. Seleccione **Guardar** para aplicar los cambios.

Si un usuario ejecuta una aplicación que se ha inhabilitado de forma remota, aparecerá una ventana de diálogo con el mensaje personalizado configurado. El mensaje se muestra con cualquier interacción de la aplicación que precise acceso a un recurso protegido, o cuando la aplicación intente acceder a una señal de acceso. Si ha proporcionado un URL de actualización de versión, el diálogo mostrará el botón **Obtener nueva versión** para actualizar a una versión más reciente, además del botón predeterminado **Cerrar**. <br/>
Si el usuario cierra la ventana de diálogo sin actualizar la versión, podrá continuar trabajando con los recursos de aplicación que no están protegidos. Sin embargo, cualquier interacción de la aplicación que requiera acceso a un recurso protegido, provoca que el diálogo se vuelva a mostrar y que la aplicación o el usuario no obtengan acceso al recurso.
