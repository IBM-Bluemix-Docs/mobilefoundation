---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-24"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}

# Utilización de {{ site.data.keyword.cos_full_notm }} con {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) proporciona funciones de nivel empresarial diseñadas exclusivamente para dar soporte a la creación y el despliegue de la nueva generación de apps móviles cognitivas, contextuales y personalizadas. {{ site.data.keyword.cos_full_notm }} (COS) es un almacenamiento en la nube flexible, eficiente en coste y escalable para datos no estructurados. En esta guía se explica el modo en que una aplicación móvil que utiliza {{ site.data.keyword.mobilefoundation_short}} se puede conectar y captar o cargar datos en {{ site.data.keyword.cos_full_notm }} a través de una aplicación Ionic. La aplicación Ionic, el adaptador y los archivos relacionados para esta guía de aprendizaje de instrucciones están disponibles [aquí](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).
{: shortdesc}


## Requisitos previos
{: #cos-prerequisites}

1. Instale [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) ejecutando `npm install -g mfpdev-cli`. Esta cli se utiliza para registrar la app Ionic y desplegar el adaptador en el servidor de MF. Como alternativa, se pueden realizar estas actividades desde el panel de control del servidor de MF.

2. Instale la [CLI de {{ site.data.keyword.cloud_notm}}](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli) en su máquina.

3. Instale la cli ionic ejecutando `npm install -g ionic`

4. Instale cordova ejecutando `npm install -g cordova`


## Configuración del servidor de {{ site.data.keyword.mobilefoundation_short}}
{: #mobile-foundation-server-setup}

El servidor de {{ site.data.keyword.mobilefoundation_short}} está establecido en {{ site.data.keyword.cloud_notm}}. Configure una instancia de {{ site.data.keyword.cloud_notm}} del servidor de {{ site.data.keyword.mobilefoundation_short}} de la siguiente manera,

* En el catálogo de {{ site.data.keyword.cloud_notm}}, busque *{{ site.data.keyword.mobilefoundation_short}}*. Pulse sobre el mosaico **{{ site.data.keyword.mobilefoundation_short}}**.

    ![MFPCatalog](images/mfp_catalog.png)

* Proporcione un nombre adecuado para la instancia de servidor de {{ site.data.keyword.mobilefoundation_short}} y pulse **Crear**.

    ![BMMFPNew](images/bmmfpnew.png)

* Se crea una nueva instancia del servidor de MF y se solicitan las credenciales de inicio de sesión.

    ![MFPLogin](images/mfp_login.png)

* Las credenciales para iniciar sesión en el servidor de MF se pueden encontrar en el separador
**Credenciales** del menú.

    ![MFPcredentials](images/mfp_credentials.png)

* Proporcione estas credenciales e inicie sesión para entrar en el panel de control de MF.

    ![MFPDashboard](images/mfp_dashboard.png)


## Configuración de Cloud Object Storage
{: #cloud-object-storage-setup}

* En el catálogo de {{ site.data.keyword.cloud_notm}}, busque *Cloud Object Storage*. Pulse sobre el mosaico **Almacenamiento de objetos**.

    ![Catálogo](images/catalog.png)

* Proporcione un nombre para la instancia de COS y pulse **Crear**.

    ![Crear COS](images/cos_create.png)

* A continuación, pulse **Grupos** en las opciones del menú. Proporcione un nombre adecuado (en este ejemplo hemos elegido denominar el grupo como `sharedgallery`) para el grupo y pulse **Crear**.

    ![Crear grupo](images/bucketcreate.png)

* Una vez que se haya creado el grupo, la página de destino del grupo tendrá un aspecto similar a la imagen siguiente, proporcionándole opciones para cargar datos directamente.

    ![Página de destino de grupo](images/bucketlanding.png)

* Aquí, añada los tres archivos que se proporcionan en la carpeta **Short stories** del [ejemplo](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).


## App Ionic de MFP-COS y adaptador Java
{: #mfp-cos-ionic-app-and-java-adapter}

Descargue este [repositorio git](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) o clónelo. Este repositorio consta de dos componentes principales:

1. Un adaptador Java de MF
2. Una aplicación móvil Ionic

### Configuración de mfpdev-cli
{: #configuring-mfpdev-cli}

Añada los detalles del servidor a la cli. En el indicador de mandatos, ejecute `mfpdev server add`.

```
? Especifique el nombre del nuevo perfil de servidor:
```

Proporcione un nombre para el servidor y pulse Intro. En este ejemplo, el nombre que se proporciona es
`mfpserver`.

```
? Especifique el URL completo de este servidor:
```

Especifique el URL del servidor. Para el servidor de MF en {{ site.data.keyword.cloud_notm}}, el separador de credenciales de servicio contiene el URL. El puerto para el servidor de MF https en {{ site.data.keyword.cloud_notm}} es 443 y para la instancia del servidor de MF http es 80.

```
? Especifique el ID de inicio de sesión del servidor de MobileFirst: (admin)
```

Especifique `admin` y pulse **Intro**.

```
? Especifique la contraseña de administrador del servidor de MobileFirst:
```

Especifique la contraseña disponible en las credenciales de servicio.

```
? ¿Desea guardar la contraseña de administrador para este servidor?: (Y/n)
```

Según su preferencia, especifique *Y/N* (sí o no) e introduzca los detalles cuando se le soliciten.

```
? Especifique el tiempo de espera de conexión del servidor de MobileFirst en segundos: 30
```

Establezca 30 segundos como tiempo de espera predeterminado

```
? ¿Desea establecer este servidor como predeterminado?: (Y/n)
```

Especifique *Y* (sí) y pulse **Intro**.

***Salida esperada***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### Configuración del adaptador Java de MF
{: #configuring-the-mf-java-adapter}

Para conectarse a su instancia de COS, es necesario proporcionar algunos detalles de la instancia de COS en el archivo
`adapter.xml`. Suministre valores para los campos siguientes:

1. **endpointURL**: este campo es el URL de punto final público para su objeto de COS. Este URL se puede encontrar en el panel de control de COS, en **Grupos (en las opciones del menú) -> <nombre-grupo> (`sharedgallery` en este ejemplo) -> Configuración -> Puntos finales -> Público**
2. **AuthToken**: en esta guía de aprendizaje, utilizaremos la autenticación de IAM.

Para que el adaptador Java se conecte a su instancia de COS, se necesita autenticación que utilice IAM o HMAC. A continuación se indican los pasos para obtener la señal IAM. Para obtener información más detallada sobre los procesos de autenticación de IAM y HMAC, pulse
[aquí](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions).

#### Obtención de la señal IAM Oauth utilizando la CLI de {{ site.data.keyword.cloud_notm}}
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. En primer lugar, asegúrese de tener una clave de API. Obtenga la clave de API de [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users).
2. Inicie sesión en la plataforma {{ site.data.keyword.cloud_notm}} utilizando la CLI.

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  La salida será similar al fragmento de código siguiente,

	```text
	Authenticating...
	OK

	Targeted account <account-name> (<account-id>)

	Targeted resource group default

	API endpoint:     https://api.us-south.cf.cloud.ibm.com (API version: 	2.128.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. Para obtener todas las instancias del servicio de su cuenta de {{ site.data.keyword.cloud_notm}}, ejecute el mandato siguiente en la CLI.

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	Expected output:

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. Ejecute el mandato siguiente para obtener los detalles de su instancia de COS. <instance-name> es el nombre de su servicio de COS (`newObject` en esta guía de aprendizaje).

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	Expected output:

	```text
	Retrieving service instance <sinstance-name> in resource group 	Default under account <account-name> as<email-address>...
	OK

	Name:                  <resource-instance-name>
	ID:                    <resource-instance-id>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. Para obtener la señal IAM, ejecute el mandato siguiente:
  ```bash
	ibmcloud iam oauth-tokens
  ```

	Salida esperada:

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

Tras añadir los valores de *endpointURL* y *authToken*, cree el adaptador. Navegue a la carpeta raíz del adaptador en el indicador de mandatos y ejecute

```bash
mfpdev adapter build
```
{: codeblock}

El archivo `*.adapter` se crea en la carpeta `target`. Ejecute el mandato siguiente,

```bash
mfpdev adapter deploy
```
{: codeblock}

El adaptador se despliega en la instancia de MF.

Como alternativa, el adaptador se puede desplegar en el panel de control del servidor de MF. Abra el panel de control del servidor de MF, en el menú, y pulse **Adaptadores -> Nuevo** para abrir la página según se muestra en la imagen siguiente.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

A continuación, pulse **Desplegar adaptador** y cargue el archivo `.adapter` de la carpeta **target**.


### Configuración de la app Ionic
{: #configuring-the-ionic-app}

En la app, realice los pasos siguientes,

1. Navegue hasta la carpeta que contiene la aplicación Ionic.

2. Añada el plugin cordova de MF

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Añada la plataforma Android o iOS

  ```bash
	ionic cordova platform add android
  ```
	O bien

	```bash
	ionic cordova platform add ios
	```

4. Registre la app en el servidor de MF ejecutando

  ```bash
	mfpdev app register
	```

	Como alternativa, se puede registrar la app en el panel de control del servidor de MF. Abra el panel de control del servidor de MF y, en el menú,
pulse **Aplicaciones -> Nueva**.

	Se cargará la página tal como se muestra en la imagen siguiente.

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	Especifique los detalles solicitados. Proporcione un nombre para la aplicación en el recuadro de texto 'Nombre de aplicación'. Elija la plataforma necesaria.

	Para Android, el recuadro de texto **Paquete** acepta el *Identificador de aplicación*. Este parámetro se puede encontrar en el archivo `AndroidManifest.xml` como paquete de su aplicación Android. El recuadro de texto **Versión** debe rellenarse con el valor de *versionName* del archivo `AndroidManifest.xml`.

	Para iOS, el recuadro de texto **ID de paquete** acepta el *Identificador de aplicación* (sensible a mayúsculas y minúsculas). Este parámetro se puede encontrar en el archivo `mfpclient.plist` de la aplicación iOS. El recuadro de texto **Versión** debe rellenarse con el valor de *version* del archivo `mfpclient.plist` de su aplicación iOS.

5. Ejecute `ionic cordova prepare` para que los cambios se propaguen en los entornos añadidos.
6. Ejecute
  ```bash
	ionic cordova build android
  ```
	O bien
  ```bash
	ionic cordova build ios
  ```
	para asegurarse de que los cambios introducidos se añaden a los entornos.

7. Conecte un dispositivo o ejecute un emulador o simulador y ejecute el mandato siguiente.

	```bash
  ionic cordova run android
	```
	O bien
	```bash
	ionic cordova run ios
  ```

### Navegación por la aplicación Ionic
{: #navigating-the-ionic-application}

La aplicación Ionic muestra una lista de relatos breves que se han cargado con anterioridad (como último paso de la sección
[Configuración de Cloud Object Storage](#cloud-object-storage-setup)), desde la instancia de COS que ha creado. Al seleccionar una opción de relato concreta, el relato seleccionado se carga y se visualiza. También se proporciona una opción para añadir un relato, que se cargará luego en la instancia de COS.

La lista de objetos inicial de COS tiene un aspecto similar a la imagen siguiente.

![COS antes](images/cos_before.png)

La página de inicio de la aplicación proporciona una opción para *Obtener todos los relatos* o *Añadir relato*

![Pantalla de inicio de app](images/app-home-screen.png)

Al pulsar **Obtener todos los relatos**, aparecerán los relatos disponibles en COS.

![Seleccionar relato de app](images/app-select-story.png)

Seleccione un relato a cargar desde el menú desplegable y pulse **Cargar**

![Relato de app cargado](images/app-story-loaded.png)

A continuación, pulse el botón **Añadir relato** para añadir un relato propio. Especifique el título del relato y el contenido del relato en el área de texto y pulse **Añadir**.

![Añadir entrada de app](images/app-add-input.png)

Una vez que se haya añadido el relato, aparecerá un mensaje indicando que el relato se ha añadido correctamente.

![Relato de app añadido](images/app-story-added.png)

El panel de control de COS también tiene ahora el relato añadido desde la aplicación.

![COS añadido](images/cos_added.png)


La señal IAM oauth obtenida mediante `ibmcloud iam oauth-tokens` tiene una hora de caducidad y el adaptador falla con una excepción de `403 - Forbidden`. Por lo tanto, debe asegurarse de que la señal sea válida en el adaptador desplegado para que la app funcione según lo esperado.
{: note}
