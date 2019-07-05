---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: troubleshooting techniques

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Resolución general de problemas
{: #general-troubleshooting}

Para aislar y resolver problemas con los productos de {{site.data.keyword.IBM_notm}}, puede utilizar la información de resolución de problemas. Esta información contiene instrucciones para utilizar los recursos de determinación de problemas que se proporcionan con los productos de {{site.data.keyword.IBM_notm}}, incluido {{site.data.keyword.mobilefoundation_short}}.
{: shortdesc}

## Técnicas para la resolución de problemas
{: #ts_overview}

La *resolución de problemas* es un procedimiento sistemático para resolver un problema. El objetivo de la resolución de problemas es determinar por qué algo no funciona del modo esperado y cómo resolverlo. Algunas técnicas comunes pueden ayudar con la tarea de resolución de problemas.

El primer paso del proceso de resolución de problemas consiste en describir por completo el problema. Las descripciones de problemas le ayudan a usted y al representante del servicio de asistencia técnica de {{site.data.keyword.IBM_notm}} a saber por dónde empezar para encontrar la causa del problema. En este paso, deben plantearse algunas cuestiones básicas:

- ¿Cuáles son los síntomas del problema?
- ¿Dónde se produce el problema?
- ¿Cuándo se produce el problema?
- ¿En qué condiciones se produce el problema?
- ¿Puede reproducirse el problema?

Las respuestas a estas preguntas normalmente conducen a una buena descripción del problema, que a su vez puede llevarle a una resolución.

### ¿Cuáles son los síntomas del problema?

Cuando se empieza a describir un problema, la pregunta más obvia es:
**¿Cuál es el problema?** Esta pregunta puede parecer sencilla; sin embargo, puede dividirla en varias más precisas que creen una imagen más descriptiva del problema. Estas preguntas pueden incluir las siguientes,

- ¿Quién o qué está informando del problema?
- ¿Cuáles son los códigos y los mensajes de error?
- ¿Cómo falla el sistema? Por ejemplo, ¿es un bucle, un cuelgue, un bloqueo, una degradación del rendimiento o un resultado incorrecto?

### ¿Dónde se produce el problema?

Determinar dónde se origina el problema no es siempre fácil, pero es uno de los pasos más importantes para resolver un problema. Muchas capas de tecnología pueden existir entre los componentes de informe y los componentes anómalos. Las redes, discos y controladores son sólo algunos de los componentes que deben tenerse en cuenta al investigar un problema.

Las siguientes preguntas le ayudarán a centrarse en dónde se produce el problema para aislar la capa del problema:

- ¿El problema es específico de una plataforma o sistema operativo, o por el contrario es común en varias plataformas o sistemas operativos?
- ¿Están soportados el entorno y la configuración actuales?
- ¿Tienen el problema todos los usuarios?
- (Para instalaciones multisitio). ¿Tienen todos los sitios el problema?

Si una capa notifica el problema, el problema no necesariamente se ha creado en esa capa. Parte de la identificación de dónde se origina un problema es comprender el entorno en el que existe. Dedique tiempo a describir completamente el entorno del problema, incluido el sistema operativo y la versión, todo el software y las versiones correspondientes, y la información de hardware. Confirme que está ejecutando dentro de un entorno que es una configuración admitida, muchos problemas pueden deberse a niveles incompatibles de software que no se han planeado para que se ejecuten juntos, o cuyo funcionamiento conjunto no está totalmente comprobado.

### ¿Cuándo se produce el problema?

Desarrolle una línea temporal detallada de sucesos que lleven hasta el error, especialmente para los casos en los que solo ocurra una vez. Puede desarrollar fácilmente una línea temporal realizando un retroceso: empiece en el momento en que se informó del error (tan detalladamente como sea posible, incluso hasta el milisegundo) y retroceda por los registros y la información disponibles. Normalmente sólo deberá llegar hasta el primer suceso sospechoso que encuentre en un registro de diagnóstico.

Para desarrollar una línea temporal detallada de sucesos, responda a estas preguntas:

- ¿El problema sucede una sola vez en una determinada hora del día o de la noche?
- ¿Con qué frecuencia ocurre el problema?
- ¿Qué secuencia de sucesos antecede al momento en que se informa del problema?
- ¿Sucede el problema después de un cambio de entorno como, por ejemplo, al actualizar o instalar software o hardware?

La respuesta a este tipo de preguntas puede proporcionar un marco de referencia en el que investigar el problema.

### ¿En qué condiciones se produce el problema?

Saber qué sistemas y aplicaciones se están ejecutando en el momento en que se produce un problema es una parte importante de la resolución de problemas. Estas preguntas sobre el entorno pueden ayudarle a identificar la causa raíz del problema:

- ¿Se produce siempre el problema cuando se está ejecutando la misma tarea?
- ¿Debe producirse una determinada secuencia de sucesos para que ocurra el problema?
- ¿Hay otras aplicaciones que den error al mismo tiempo?

La respuesta a este tipo de preguntas le ayudará a conocer el entorno en el que se produce el problema y establecer correlaciones de dependencias. Solo porque varios problemas se hayan producido más o menos al mismo tiempo, no significa que necesariamente tengan que estar relacionados.

### ¿Puede reproducirse el problema?

Desde el punto de vista de la resolución de problemas, el problema ideal es el que se puede reproducir. Normalmente, cuando un problema se puede reproducir, dispone de un conjunto de herramientas o procedimientos más grande para ayudarle en la investigación. Por lo tanto, los problemas que puede reproducir suelen ser más fáciles de depurar y resolver.

No obstante, los problemas que puede reproducir pueden tener una desventaja: si el problema tiene un impacto empresarial importante, no desea que se repita. Si es posible, vuelva a crear el problema en un entorno de prueba o de desarrollo, que habitualmente ofrecen más flexibilidad y control durante la investigación.

- ¿Se puede volver a crear el problema en un sistema de prueba?
- ¿Hay varios usuarios o aplicaciones que encuentren el mismo tipo de problema?
- ¿Se puede recrear el problema mediante la ejecución de un único mandato, un conjunto de mandatos, una aplicación concreta?


##  Limitaciones conocidas
{: #knownlimitations_mfp}

* La IU del servicio {{site.data.keyword.mobilefoundation_short}} no utiliza el patrón específico del entorno local seleccionado por el usuario para mostrar números.

## Obtención de ayuda y soporte para Mobile Foundation
{: #getting_help_mobilefoundation}

Si tiene problemas o preguntas sobre el uso de {{site.data.keyword.mobilefoundation_short}}, puede obtener ayuda buscando información o formulando preguntas en un foro. También puede abrir una incidencia de soporte.

Cuando utilice los foros para hacer una pregunta, etiquete la pregunta para que puedan verla los equipos de desarrollo de
{{site.data.keyword.Bluemix_notm}}.

Si tiene preguntas técnicas sobre el desarrollo o el despliegue de una app con {{site.data.keyword.mobilefoundation_short}}, publíquelas en [Stack Overflow ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](http://stackoverflow.com/search?q=ibm-mobilefirst+bluemix){:new_window} y etiquete las preguntas con `bluemix` e `ibm-mobilefirst`.

Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM developerWorks dW Answers ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://developer.ibm.com/answers/topics/mobilefirst/?smartspace=bluemix){:new_window}. Incluya las etiquetas `bluemix` y `mobilefirst`.

Para obtener más información sobre cómo abrir una incidencia de soporte de IBM, o sobre los niveles y la gravedad de la incidencia, consulte
[Cómo obtener soporte](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support){: new_window}.
