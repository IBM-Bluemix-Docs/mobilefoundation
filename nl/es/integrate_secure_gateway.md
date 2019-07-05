---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Establecimiento de una conexión segura con un sistema local utilizando el servicio Secure Gateway
{: #integrate_secure_gateway}

Cuando crea apps móviles de empresa, normalmente desea integrar las apps con los sistemas existentes de registros. Para acceder y utilizar los datos almacenados en el centro de datos local desde una app móvil, puede utilizar el [servicio de Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) con el [servicio Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway) en [IBM Cloud](https://cloud.ibm.com/). El servicio Secure Gateway proporciona una conectividad rápida, fácil y segura y establece un túnel entre IBM Cloud y el sistema remoto al que desea conectarse.

En esta guía de aprendizaje, se explica cómo acceder a los puntos finales HTTP en el centro de datos local desde los adaptadores de Mobile Foundation que se ejecutan en IBM Cloud, utilizando el servicio Secure Gateway.

## Requisitos previos
{: #prereq_int_sec_gw}

Para completar esta guía de aprendizaje, necesitará un punto final HTTP dentro del cortafuegos de empresa que exponga los sistemas de datos de registro. De forma alternativa, cree un punto final de prueba en el entorno local utilizando [este proyecto ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) `Node.js` de ejemplo.

Vaya a la carpeta del proyecto y ejecute el servidor HTTP.

```bash
npm install
node app.js
```
{: codeblock}

## Escenario de integración con el servicio Secure Gateway
{: #secure_gateway}

La imagen siguiente ilustra la arquitectura que se utiliza en el escenario de integración explicado en esta guía de aprendizaje.

![Diagrama de arquitectura](images/SecureGatewayArchi.png "Diagrama de arquitectura de dispositivo, servicio de nube y red local")

## Implementación de la integración de Secure Gateway
{: #implementing_sg_integration}

### Crear una instancia de servicio de Secure Gateway
Inicie sesión en IBM Cloud y cree una instancia del [servicio Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway/).

![IBM Cloud](images/SecureGatewayInst.gif "Creación de una instancia de Secure Gateway del catálogo de IBM Cloud")

Después de crear la instancia de servicio de Secure Gateway, siga los pasos siguientes para configurar el servicio Secure Gateway entre IBM Cloud y el entorno local.

### Añadir una pasarela
{: #add_gateway}

En el panel de control del servicio Secure Gateway, pulse **Añadir pasarela** para crear una nueva pasarela proporcionando cualquier nombre de pasarela que desee.

![Añadir pasarela](images/AcmeAddGateway.gif "Pasos de la interfaz de usuario de Añadir pasarela")


### Añadir un cliente de Secure Gateway
{: #add_sg_client}

![Añadir cliente](images/AcmeAddClient.gif "Pasos de la interfaz de usuario de Añadir cliente")

Desde la nueva pasarela en el separador **Clientes**, pulse **Conectar cliente**.

Puede utilizar cualquiera de los clientes de su elección y ejecutar el cliente de Secure Gateway en el entorno local. Los pasos para configurar el cliente de Secure Gateway están disponibles en la consola de Secure Gateway.

En esta guía de aprendizaje, utilizaremos la opción de contenedor Docker para ejecutar el cliente de Secure Gateway.
Siga los siguientes pasos:
*   Instale Docker en la máquina local, si aún no está instalado.
*   Inicie un terminal y ejecute el cliente de Secure Gateway en un contenedor utilizando el mandato que se muestra en la consola de servicio.
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId` se puede encontrar en la consola tal como se muestra en la imagen anterior.

### Añadir un destino
{: #add_destination}

![Añadir destino](images/AcmeAddDest.gif "Pasos de la interfaz de usuario de Añadir destino")

Desde la nueva pasarela en el separador **Destinos**, pulse **Añadir destino**.

El servicio Secure Gateway le permite conectarse a un punto final local desde un entorno de IBM Cloud o conectarse desde un entorno local a un recurso de nube. En este caso de ejemplo, seleccione local como la ubicación de recursos y proporcione el nombre de host/IP y el puerto del recurso local. Además, proporcione un nombre de destino de su elección.

Para permitir que la solicitud se reenvíe desde el cliente de Secure Gateway al punto final de recurso, tendrá que añadir el recurso a la lista de acceso.
Ejecute el mandato siguiente en el terminal del cliente de Secure Gateway (en ejecución como contenedor en el tiempo de ejecución de Docker, en este caso de ejemplo) para el recurso a la lista de acceso.

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` y `resourcePort` hacen referencia a los detalles del punto final de recursos local.

El destino ya está configurado. El servicio Secure Gateway llenará los detalles de host y puerto de la nube, que se pueden utilizar para acceder al recurso local desde el entorno de nube.

![Separador Destino](images/AcmeCloudPopulate.gif "Pantalla Detalles de host y puerto")

### Configuración del servicio Secure Gateway con Mobile Foundation y adaptador de Mobile Foundation
{: #configuration_sg_mfp}

En esta guía de aprendizaje, utilizaremos la instancia de servicio de Mobile Foundation en IBM Cloud para configurar el servidor de Mobile Foundation. El servicio de Mobile Foundation en IBM Cloud ayuda a suministrar el servidor de Mobile Foundation en el tiempo de ejecución de Liberty como una aplicación de Cloud Foundry. El servicio de Mobile Foundation le permite realizar ejecutar en IBM Cloud cualquier proyecto de Mobile Foundation desarrollado en un entorno local.

### Configuración del servidor de Mobile Foundation en IBM Cloud
{: #mf_server_setup}

Cree una instancia del [servicio de Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) desde la consola de IBM Cloud.

Desde la consola de servicio de Mobile Foundation, cree el [servidor de Mobile Foundation ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/).


### Creación y despliegue del adaptador de Mobile Foundation
{: #deploying_mf_adapter}

En esta guía de aprendizaje, nos vamos a conectar al punto final de Secure Gateway mediante un adaptador de Mobile Foundation. [Descargue ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) el adaptador JavaHTTP de Mobile Foundation.

Cree y despliegue el adaptador en la consola de operaciones de Mobile Foundation utilizando mandatos [mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli).
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

Obtenga información sobre la creación y el despliegue de adaptadores desde [aquí ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/).
{: tip}

Proporcione los detalles de host y el puerto de la nube para el punto final de recurso en el adaptador JavaHTTP, obtenido de la sección anterior.

![AdapterConfiguration ](images/AdapterConfiguration.png "Página de configuraciones HTTP de Java")

donde `cap-sg-prd-5.securegateway.appdomain.cloud` y `18946` son el host y el puerto de Secure Gateway, respectivamente.

El adaptador de Mobile Foundation está ahora configurado y el servicio de Mobile Foundation está ahora habilitado para trabajar con un sistema local dentro de la empresa utilizando el servicio Secure Gateway.

### Creación y registro de la app de ejemplo de Mobile Foundation
{: #registering_sample_app}

Descargue la app de ejemplo de Mobile Foundation desde [aquí](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/), siga las instrucciones bajo `Readme` y registre la app en la consola de operaciones de Mobile Foundation.

Ejecute la app, proporcione las credenciales para iniciar la sesión y pulse el botón *Iniciar sesión*. Pulse el botón *Captar escritores Acme* para llamar a su punto final local a través de Secure Gateway utilizando el adaptador JavaHTTP desplegado en la consola de operaciones de Mobile Foundation. Reciba los datos deseados del entorno local.

![App que recibe datos locales](images/AcmePublishersApp.gif "App de ejemplo que recibe datos")

Puede conectarse a varios puntos finales locales configurando varios destinos en el servicio Secure Gateway y desplegando los adaptadores de Mobile Foundation para conectarse al host de nube respectivo del punto final. También puede configurar el servicio Secure Gateway con seguridad adicional para asegurarse de que la comunicación con el punto final se realice a través de HTTPS y de seguridad del lado de la aplicación. Encontrará los [detalles aquí](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg).


## Resumen
{: #summary_int_sec_gw}

Al utilizar esta guía de aprendizaje, debe poder establecer una conexión segura entre los adaptadores de Mobile Foundation que se ejecutan en IBM Cloud y un punto final HTTP local, mediante el servicio Secure Gateway.
