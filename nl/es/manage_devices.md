---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestionar dispositivos
{: #manage_devices}

El acceso de dispositivos y el estado del dispositivo se pueden gestionar desde la consola de operaciones de Mobile Foundation. Un dispositivo se identifica de forma exclusiva con un identificador denominado ID de dispositivo, asignado por el SDK del cliente de Mobile Foundation. También puede establecer un nombre de visualización para el dispositivo. Los campos ID de dispositivo y Nombre de visualización del dispositivo se indexan para la búsqueda.

Los desarrolladores de aplicaciones pueden utilizar el método `setDeviceDisplayName` de la clase `WLClient` para establecer el nombre de dispositivo para mostrar. Pulse [aquí](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/) para obtener la documentación de `WLClient`. Los desarrolladores del adaptador de Java también pueden establecer el nombre de visualización del dispositivo utilizando el método `setDeviceDisplayName` de la clase `com.ibm.mfp.server.registration.external.model MobileDeviceData`.
{: tip}

El servidor de Mobile Foundation mantiene la información de estado de cada dispositivo que acceda al servidor.
Los valores de estado posibles son:
* Activo
* Perdido
* Robado
* Caducado
* Inhabilitado

El estado por defecto de un dispositivo es **Activo**, lo que indica que el acceso desde dicho dispositivo no está bloqueado. Puede cambiar el estado a **Perdido**, **Robado** o **Inhabilitado** para bloquear el acceso desde el dispositivo a los recursos de su aplicación. Siempre puede restaurar el estado **Activo** para permitir de nuevo el acceso.

El estado **Caducado** es un estado especial que establece el servidor de Mobile Foundation después de que un dispositivo haya alcanzado una duración de inactividad preconfigurada. Este estado se utiliza para el seguimiento de la gestión de licencias, y no afecta a los derechos de acceso del dispositivo. Cuando un dispositivo con el estado **Caducado** se vuelve a conectar al servidor, el estado se restaura a **Activo** y al dispositivo se le otorga acceso al servidor.

## Dispositivos de bloque
{: #block_devices}

1. Vaya a la página **Dispositivos** desde la consola de operaciones de Mobile Foundation.
2. Utilice el campo **Búsqueda** para buscar un dispositivo especificando el ID de usuario que está asociado con el dispositivo o proporcionando el nombre de visualización del dispositivo (si se ha establecido en un dispositivo).
3. Para cada uno de los dispositivos devueltos en el resultado de búsqueda, podrá ver el ID de dispositivo, el nombre de visualización, el sistema operativo y la lista de ID de usuarios asociados con dicho dispositivo.
4. La columna de estado del dispositivo muestra el estado del mismo. Puede cambiar el estado del dispositivo a **Perdido**, **Robado** o **Inhabilitado** para bloquear el acceso desde el dispositivo a los recursos de aplicación protegidos.

   Al cambiar el estado de nuevo a **Activo** restaura los derechos de acceso originales.
   {: note}


## Anular registro de dispositivos
{: #unregister_devices}

1. Vaya a la página **Dispositivos** desde la consola de operaciones de Mobile Foundation.
2. Utilice el campo **Búsqueda** para buscar un dispositivo especificando el ID de usuario que está asociado con el dispositivo o proporcionando el nombre de visualización del dispositivo (si se ha establecido en un dispositivo).
3. Para cada uno de los dispositivos devueltos en el resultado de búsqueda, podrá ver el ID de dispositivo, el nombre de visualización, el sistema operativo y la lista de ID de usuarios asociados con dicho dispositivo.
4. Seleccione *Anular registro* desde la columna **Acciones**.

   Al anular el registro de un dispositivo se suprimen los datos de registro de todas las aplicaciones de Mobile Foundation instaladas en el dispositivo. Además, el nombre de visualización del dispositivo, la lista de usuarios que están asociados con el dispositivo y los atributos públicos que la aplicación ha registrado para dicho dispositivo se suprimen.
   {: note}


No **anule el registro** de un dispositivo si tiene intención de **bloquear** el mismo. Al anular el registro de un dispositivo se eliminan todos los datos relacionados con el dispositivo. Si un usuario intenta acceder al servidor de Mobile Foundation utilizando el mismo dispositivo, se le solicitará que lo registre y el registro creará un nuevo ID de dispositivo para el mismo en el servidor. Esto significa que el dispositivo recuperará el acceso al servidor de Mobile Foundation.
{: important}
