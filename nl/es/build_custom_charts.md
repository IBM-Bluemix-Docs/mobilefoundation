---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, charts, app sessions, crashes, graph

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

# Crear gráficos personalizados
{: #build_custom_charts}

La vista Gráficos personalizados en la consola de Mobile Analytics le proporciona la flexibilidad necesaria para crear sus propias visualizaciones sobre los datos de análisis capturados y almacenados.  Los gráficos personalizados le ayudan a extraer conocimientos de lo que se proporciona inicialmente o incluso ampliarlos fácilmente a la analítica de negocio formulada en torno a los datos personalizados.

En esta vista, puede elegir cualquiera de los conjuntos de datos de análisis soportados y, a continuación, seleccionar uno de los tipos de gráficos soportados para trazar el conjunto de datos.  Puedo incluso recortar aún más la visualización definiendo filtros que se aplicarán sobre los datos que se están trazando.  

Los conjuntos de datos soportados son,
 * Registros de apps
 * Sesiones de apps
 * Datos personalizados
 * Transacciones de red

Los tipos de gráficos soportados son,
 * Gráfico de barras
 * Diagrama de flujo
 * Gráfico de líneas
 * Grupo de métricas
 * Gráfico circular
 * Tabla

La selección del conjunto de datos, el tipo de gráfico a trazar, la definición de las características del gráfico y los filtros de datos que se van a aplicar se pueden compilar en una definición de gráfico personalizado y guardarse.  Puede crear y guardar todas las definiciones de gráficos personalizados que necesite. Los gráficos personalizados guardados se muestran en la vista Gráficos personalizados con los datos de análisis relevantes trazados.

## Creación de un gráfico personalizado
{: #creating_custom_chart}

Cree un gráfico personalizado siguiendo estos pasos:

1.  Pulse el botón **Crear gráfico** en el separador **Gráficos personalizados** del panel de control de Mobile Analytics.
2.  En el separador **Valores generales**, seleccione el **Título de gráfico**, el **Tipo de suceso** y el **Tipo de gráfico**.
3.  Al seleccionar el *Tipo de suceso* y el *Tipo de gráfico*, aparece el separador **Definición de gráfico**. Utilice el separador *Definición de gráfico* para definir el gráfico especificado para el tipo de gráfico que se ha seleccionado anteriormente. Después de definir el gráfico, puede establecer filtros y propiedades del gráfico.
4.  Los **Filtros de gráfico** permite realizar ajustes más precisos al gráfico personalizado. Se puede definir más de un filtro para un gráfico.
    Por ejemplo, después de definir un gráfico para visualizar el promedio de duración de la sesión de app, si desea visualizar este gráfico solo para una app específica, puede crear un filtro de la forma siguiente:
    * Seleccione **Nombre de aplicación** como **Propiedad**.
    * Seleccione **Igual a** como **Operador**.
    * Seleccione el nombre de su app como **Valor**.
    * Pulse **Añadir filtro**.
    El filtro del nombre de la app se añade a la tabla de filtros de su gráfico.
5.  Las propiedades del gráfico están disponibles en los tipos de gráfico **Tabla**, **Gráfico de barras** y **Gráfico de líneas**. El objetivo de las propiedades del gráfico es mejorar la presentación de los datos para que la visualización sea más efectiva.
    Si ha creado un gráfico de **Tabla**, las propiedades del gráfico se establecen para definir el tamaño de página de la tabla, el campo en el que se debe ordenar y el orden de clasificación del campo.
    Si ha creado un gráfico de **Gráfico de barras** o **Gráfico de líneas**, las propiedades del diagrama se establecen para etiquetar las líneas de umbral para añadir un marco de referencia para cualquiera que esté supervisando el gráfico.

## Obtención de información personalizada a partir de registros de datos personalizados
{: #creating_custom_chart_for_client_logs}    

Si desea obtener información detallada personalizada, como seguir el rastro de los usuarios en la aplicación, primero necesita capturar la información de rastreo del usuario relevante, como la página elegida, la opción seleccionada o el botón pulsado como datos personalizados y registrarlos.  Consulte el tema sobre [instrumentar su app](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app), sobre cómo registrar datos personalizados.

A continuación, cree una definición de gráfico personalizado con datos personalizados como Tipo de suceso y elija un Tipo de gráfico. A medida que proceda a definir las **Propiedades de gráfico** o **Filtros de gráfico**, observará que aparecen los valores y los tipos de datos personalizados en los recuadros desplegables.  Realice las selecciones correspondientes para el tipo de información que está buscando.  

La profundidad y la utilidad de la información personalizada depende exclusivamente de la eficacia o relevancia con la que haya definido y capturado los datos personalizados de sus aplicaciones.
{: note}

## Tipos de gráficos
{: #types_of_charts}

### Gráfico de barras
{:  #bar_graph}

El gráfico de barras permite la visualización de datos numéricos a lo largo de un eje X. Al definir un gráfico de barras, primero se debe elegir el valor para el eje X. Puede elegir entre los siguientes posibles valores.

* **Línea temporal**: si desea visualizar los datos como una tendencia (por ejemplo, promedio de duración de la sesión de app a lo largo del tiempo), elija *Línea temporal* para Eje X.
* **Propiedad**: elija Propiedad si desea ver un desglose del recuento para la propiedad especificada. Si elige Propiedad para el eje X, de forma implícita se elige el Total para el eje Y. Por ejemplo, elija *Propiedad* para el eje X y *Nombre de aplicación* para *Propiedad* para visualizar un recuento para un tipo de suceso especificado, que se desglosa por nombre de app.

Después de definir un valor para el eje X, puede definir un valor para el eje Y. Si elige *Línea temporal* para eje X, puede elegir los siguientes valores para el eje Y.

* **Promedio**: calcula el promedio de una propiedad numérica en el tipo de suceso especificado.
* **Total**: un recuento total de una propiedad en el tipo de suceso especificado.
* **Exclusivo**: un recuento exclusivo de una propiedad en el tipo de suceso especificado.
Después de definir los ejes del gráfico, debe elegir un valor para la propiedad.

### Gráfico de líneas
{:  #line_graph}

El gráfico de líneas permite la visualización de algunas métricas a lo largo del tiempo. Este tipo de gráfico es valioso cuando quiere visualizar la tendencia de los datos a lo largo del tiempo. El primer valor que se define al crear un gráfico de líneas es **Medida**, que tiene los siguientes posibles valores.

* **Promedio**: calcula el promedio de una propiedad numérica en el tipo de suceso especificado.
* **Total**: un recuento total de una propiedad en el tipo de suceso especificado.
* **Exclusivo**: un recuento exclusivo de una propiedad en el tipo de suceso especificado.
Después de definir la medida, debe elegir un valor para **Propiedad**.

### Diagrama de flujo
{:  #flow_chart}

El diagrama de flujo permite la visualización del desglose de flujo de una propiedad a otra. Con un diagrama de flujo, se deben establecer las siguientes propiedades.

* **Origen**: el valor de un nodo de origen en el diagrama.
* **Destino**: valor del nodo de destino en el diagrama.
* **Propiedad**: un valor de propiedad del nodo de origen o del nodo de destino.
Con el diagrama de flujo, puede ver el desglose de densidad de varios orígenes que fluyen hacia un destino, o al revés. Por ejemplo, si desea ver el desglose de las gravedades de registro de una app, puede definir los valores siguientes.

* Seleccione *Nombre de aplicación* como **Origen**.
* Seleccione *Nivel de registro* como **Destino**.
* Seleccione el nombre de su app como **Propiedad**.

### Grupo de métricas
{:  #metric_group}

El grupo de métricas permite visualizar una métrica individual que se mide como un valor promedio, un recuento total o un valor exclusivo. Para definir un grupo de métricas, debe definir uno de los siguientes valores posibles para **Medida**.

* **Promedio**: calcula el promedio de una propiedad numérica en el tipo de suceso especificado.
* **Total**: un recuento total de una propiedad en el tipo de suceso especificado.
* **Exclusivo**: un recuento exclusivo de una propiedad en el tipo de suceso especificado.
Después de definir la medida, debe elegir un valor para *Propiedad*. Esta métrica se visualiza en el grupo de métricas.

### Gráfico circular
{:  #pie_chart}

El gráfico circular se puede utilizar para visualizar el desglose del recuento de valores de una propiedad concreta. Por ejemplo, si desea visualizar un desglose de bloqueos, defina los siguientes valores.

* Seleccione *Sesión de app* como **Tipo de suceso**.
* Seleccione *Gráfico circular* como **Tipo de gráfico**.
* Seleccione *Cerrado por* como **Propiedad**.
El gráfico circular resultante muestra el desglose de las sesiones de app que fueron cerradas por el usuario en lugar de las sesiones de app, que se cerraron por un bloqueo.

### Tabla
{:  #table}

La tabla es útil cuando desea ver los datos sin procesar. La generación de una tabla es tan simple como añadir columnas de los datos sin procesar que desea ver.
Como no todas las propiedades son necesarias para tipos de sucesos específicos, pueden aparecer valores nulos en la tabla. Si desea evitar que estas filas aparezcan en la tabla, añada un filtro *Existe* para una propiedad específica en el separador **Filtros de gráficos**
