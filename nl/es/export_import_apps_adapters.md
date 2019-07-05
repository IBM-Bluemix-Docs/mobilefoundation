---

copyright:
  years: 2018
lastupdated: "2019-06-06"

keywords: export apps, adapters export

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Exportar e importar aplicaciones y adaptadores
{: #export_import_apps_adapters}

En {{site.data.keyword.mfp_oc_short_notm}}, bajo determinadas condiciones, puede exportar una aplicación o una de sus versiones y luego importarla a un servidor distinto. También puede exportar y reimportar adaptadores. Utilice esta función para reutilizar o realizar una copia de seguridad, o para migrar a través de instancias de {{site.data.keyword.mfserver_short_notm}}.

Si se le otorga el rol de administrador **mfpadmin** y el rol de desplegador **mfpdeployer**, podrá exportar una versión o todas las versiones de una aplicación. La aplicación o la versión se exporta como un archivo comprimido de `.zip`, que guarda el *ID de aplicación*, los *descriptores*, los *datos de autenticidad* y los *recursos web*. Más adelante, puede importar el archivo para volver a desplegar la aplicación o versión al mismo servidor o a uno distinto.

Tenga cuidado con los casos siguientes:
* El archivo de exportación incluye datos de autenticidad de la aplicación. Estos datos son específicos de la compilación de una app para móvil. La app para móvil incluye el URL del servidor. Por lo tanto, si desea utilizar otro servidor, deberá volver a crear la app. Transferir solo los archivos de la app exportados no funcionaría.
* Algunos artefactos pueden variar de un servidor a otro. Las credenciales push son diferentes dependiendo de si trabaja en un entorno de desarrollo o un entorno de producción.
* La configuración del tiempo de ejecución de la aplicación (que contiene el estado activo o inhabilitado y los perfiles de registro) se puede transferir en algunos casos, pero no en todos.
* Es posible que la transferencia de recursos web no tenga sentido en algunos casos como, por ejemplo, cuando vuelve a crear la app para utilizar un servidor nuevo.
{: tip}

##  Requisitos previos
{: #prereq}

Inicie el {{site.data.keyword.mfp_oc_short_notm}}.

##  Procedimiento
{: #procedure}

1.  Desde la barra lateral de navegación, seleccione una aplicación o una versión de aplicación o un adaptador.

2.  Seleccione **Acciones > Exportar aplicación** o **Exportar versión** o **Exportar adaptador**.
     Se le solicitará que guarde el archivo de archivado `.zip` que encapsula los recursos exportados. El aspecto del recuadro de diálogo depende del navegador y la carpeta de destino depende de los valores del navegador.

3.   Guarde el archivo archivador.
      El nombre del archivo archivador incluye el nombre y la versión de la aplicación o el adaptador, por ejemplo, `export_applications_com.sample.zip`.

4.   Para volver a utilizar un archivo de exportación existente, seleccione **Acciones > Importar aplicación** o **Importar versión** o **Importar adaptador**, vaya al archivo y pulse en **Desplegar**.
      El marco de consola principal visualiza los detalles del adaptador o la aplicación importada.

##    Resultados
{: #results}

Si realiza una importación en el mismo servidor, la aplicación o versión no se restauran necesariamente como se exportaron. Es decir, el despliegue nuevo en el momento de la importación no elimina las modificaciones posteriores. Es más, si algún recurso de aplicación se modifica entre la exportación y el redespliegue en el momento de la importación, únicamente los recursos que se incluyen en el archivador exportado se redespliegan en su estado original.
<br/>
Por ejemplo, si exporta una aplicación sin datos de autenticidad, carga datos de autenticidad y después importa el archivo inicial, los datos de autenticidad no se borrarán.
