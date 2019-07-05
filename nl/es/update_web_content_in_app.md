---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Direct Update
{: #direct_update}

Los recursos web (JavaScript, HTML, CSS o archivos de imagen) de las aplicaciones Cordova o Ionic pueden actualizarse instantáneamente (over-the-air, OTA) con **Direct Update** sin que los usuarios tengan que pasar por la actualización de App Store o Play Store. Mediante la característica Direct Update, las empresas pueden asegurarse de que los usuarios finales utilicen la versión más reciente de sus apps. Para actualizar una aplicación, los recursos web actualizados de la aplicación necesitan empaquetarse y cargarse al servidor de Mobile, utilizando la CLI de MobileFirst o desplegando un archivo de archivado generado. Direct Update se activará entonces automáticamente. A continuación, se aplicará en cada solicitud de usuario a un recurso protegido.

![Diagrama de cómo funciona la actualización directa](images/internal_function.jpg)

Se da soporte a Direct Update en las plataformas Cordova iOS y Cordova Android.
{: note}

Para la fase de pruebas de preproducción o de producción, se recomienda implementar Direct Update seguro antes de publicar la aplicación en la tienda de apps. Una versión de Direct Update segura necesita una pareja de claves RSA extraídas de un certificado de servidor firmado por una autoridad de certificación (CA) real.

1. Tenga cuidado de no modificar la configuración del almacén de claves una vez que se publique la aplicación. Las actualizaciones descargadas no pueden autenticarse antes de volver a configurar la aplicación con una nueva clave pública y de volver a publicar la aplicación. Si no realiza los dos pasos anteriores, Direct Update fallará en el cliente.
2. Direct Update actualiza solo los recursos web de la aplicación. Si desea actualizar recursos nativos, se debe enviar una nueva versión de la aplicación a las respectivas tiendas de apps.
3. Cuando se utiliza la característica Direct Update y está habilitada la característica de suma de comprobación de recursos web, se establece una nueva base de suma de comprobación con cada Direct Update.
4. Si el servidor de Mobile Foundation se ha actualizado, seguirá sirviendo a las actualizaciones directas correctamente. Sin embargo, si se sube un archivador de Direct Update compilado recientemente (archivo .zip), puede detener las actualizaciones en los clientes antiguos. La razón es que el archivado contiene la versión del plug-in de `cordova-plugin-mfp`. Antes de servir dicho archivador a un cliente móvil, el servidor compara la versión del cliente con la versión del plugin. Si ambas versiones son lo suficientemente cercanas (los tres dígitos más significativos son los mismos), Direct Update funcionará de forma normal. De lo contrario, el servidor de Mobile Foundation omitirá la actualización de forma silenciosa. Una solución para la discordancia de versiones es descargar el `cordova-plugin-mfp` con la misma versión que la del proyecto de Cordova original y volver a generar el archivado de Direct Update.
{: tip}

Después de una actualización de Direct Update, la aplicación deja de utilizar los recursos web empaquetados de forma previa. En su lugar, utiliza los recursos web descargados desde el recinto de pruebas de la aplicación. Si la memoria caché de aplicaciones del dispositivo se borra, se utilizarán de nuevo los recursos web empaquetados originales.

Direct Update de los recursos web se aplica solo a una versión específica de la app. Por ejemplo, las actualizaciones generadas para la versión 2.0 de la aplicación no se entregarán a la versión 2.1 de la misma.
{: note}

## Creación y despliegue de recursos web actualizados
{: #creating_deploying_updates}

Una vez haya realizado los cambios en los recursos web, puede empaquetarlos y cargarlos a la instancia de Mobile Foundation utilizando la CLI de Mobile Foundation.

1.  Direct Update actualiza solo los recursos web de la aplicación. Si desea actualizar recursos nativos, se debe enviar una nueva versión de la aplicación a las respectivas tiendas de apps.
2. El servicio Mobile Foundation detendrá la actualización directa a los clientes si la parte nativa de la app es significativamente distinta a la que se ha cargado en el servicio. Esto se puede producir cuando *cordova-plugin-mfp* se actualiza con la versión más reciente del plugin. Antes de servir el archivador a un cliente móvil, el servidor compara la versión del cliente con la versión del plugin. Si ambas versiones son lo suficientemente cercanas (los tres dígitos más significativos del identificador de versión son los mismos), Direct Update funcionará de forma normal. De lo contrario, el servidor de Mobile Foundation omitirá la actualización de forma silenciosa. Una solución para la discordancia de versiones es descargar el *cordova-plugin-mfp* con la misma versión que la del proyecto de Cordova original y volver a generar el archivado de Direct Update.
{: tip}

1. Abra una ventana de línea de mandatos y vaya a la raíz del proyecto de Cordova.
2. Ejecute el mandato:
  ```bash
  mfpdev app webupdate
  ```
  El mandato `mfpdev app webupdate` empaqueta los recursos web actualizados en un archivo `.zip` y los cargará al servidor de Mobile Foundation predeterminado ejecutándose en la estación de trabajo del desarrollador. Los recursos web empaquetados se pueden encontrar en la carpeta `[cordova-project-root-folder]/mobilefirst/`.

Para ver los pasos alternativos a la actualización del contenido web en la app, diríjase [aquí](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app).

## Configuración avanzada de Direct Update
{: #direct_update_advanced_config}

Para ver temas avanzados sobre la configuración de Direct Update, diríjase [aquí](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration).
