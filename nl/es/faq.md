---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:note: .note}

# Preguntas más frecuentes
{: #mfp-faq}

Estas preguntas más frecuentes proporcionan respuestas a preguntas comunes sobre el servicio {{site.data.keyword.mobilefoundation_long}}.
{: shortdesc}

## ¿Cómo puedo saber cuándo están disponibles las actualizaciones del servicio {{site.data.keyword.mobilefoundation_short}}?
{: #maintupdates_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} crea un {{site.data.keyword.mfserver_short_notm}}. Las actualizaciones del servidor de {{site.data.keyword.mobilefoundation_short}} se notifican a los usuarios. Se mostrará una notificación en el panel de control de la instancia del servicio. El usuario puede decidir aplicar la actualización a {{site.data.keyword.mobilefoundation_short}} durante la ventana de mantenimiento que considere el usuario. Puede actualizar el servidor de {{site.data.keyword.mobilefoundation_short}} cuando le convenga.

La actualización del servicio {{site.data.keyword.mobilefoundation_short}} estará disponible cuando se actualice uno de los componentes siguientes.

* {{site.data.keyword.mfserver_short_notm}}.
* La versión subyacente de Liberty.
* La versión subyacente de Java Developer Kit.

## ¿Cómo aplico las actualizaciones al servicio {{site.data.keyword.mobilefoundation_short}}?
{: #apply_update_mf}
{: faq}

La actualización de {{site.data.keyword.mobilefoundation_short}} se puede aplicar pulsando **Recrear**.
Al aplicar la actualización, la versión del servidor, que se muestra en {{site.data.keyword.mfp_oc_short_notm}}, se modificará y pasará a indicarse la versión de la actualización del servidor.

* Los usuarios no pueden aplicar sus propios arreglos y actualizaciones a su instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.
* Consulte [Recreación del servidor en el plan Professional Per Device](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5) y [Recreación del servidor en el plan Professional 1 Application](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2) para conocer la diferencia de comportamiento en los planes cuando se pulsa **Recrear**.
{: note}

## ¿Cómo configuro el dominio personalizado para mi instancia de servidor de {{site.data.keyword.mobilefoundation_short}}?
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} suministra un {{site.data.keyword.mfserver_short_notm}}, que es accesible mediante un URL con los nombres de dominio basados en la **Región** de {{site.data.keyword.Bluemix_notm}}. También puede configurar su propio dominio personalizado.

El URL o la ruta se crean con los nombres de dominio predeterminados basados en la `Región` de {{site.data.keyword.Bluemix_notm}}.

  |Dominio |  Región  |    
  |:----- | :----- |    
  |`mybluemix.net` | EE.UU. sur |    
  |`eu-gb.mybluemix.net` | Reino Unido  |
  |`au-syd.mybluemix.net` | Sídney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Tabla 1. Nombres de dominio de aplicación basados en Región en {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Para utilizar su propio dominio será necesario configurar el dominio personalizado realizando los siguientes pasos:

1.	Cree una instancia de {{site.data.keyword.mfserver_short_notm}} creando la instancia de servicio de {{site.data.keyword.mobilefoundation_short}} seleccionando uno de los planes soportados.

+ Añada el dominio personalizado que desea utilizar a la `Organización` de {{site.data.keyword.Bluemix_notm}}. Vaya a **Gestionar organizaciones > DOMINIOS > AÑADIR DOMINIO** para añadir su propio dominio.

+ Configure una ruta para que el servidor utilice el dominio personalizado.

+ Vaya al proveedor DNS para su dominio y añada una entrada CNAME, que dirigirá el tráfico de su dominio a la ruta de {{site.data.keyword.Bluemix_notm}} predeterminada, donde se está ejecutando el servidor.

+ Si desea configurar `https` para el dominio personalizado, cargue el certificado SSL para su dominio en {{site.data.keyword.Bluemix_notm}}. Para cargar el certificado SSL, vaya a **Gestionar organizaciones > DOMINIOS**, seleccione el dominio personalizado para el que desea configurar el certificado SSL, pulse **Cargar certificado** para cargar el certificado SSL para su dominio. Para obtener más información, consulte
[esta publicación
![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}.
