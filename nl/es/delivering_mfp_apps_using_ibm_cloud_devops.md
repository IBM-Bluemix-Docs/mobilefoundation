---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation, integration, devops, ibmcloud, pipeline

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


# Suministro de apps de {{ site.data.keyword.mobilefoundation_short }} utilizando {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }}
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

Esta guía de aprendizaje le ayudará a automatizar el suministro de apps y adaptadores a IBM  {{ site.data.keyword.mobilefoundation_short }} utilizando {{ site.data.keyword.jazzhub_title }}
en {{ site.data.keyword.cloud_notm }}.

En la imagen siguiente se proporciona una visión general del conducto.

![overview_of_pipeline](images/p00_overview_of_pipeline.png "Seis etapas del conducto de DevOps")


## Requisitos previos
{: #mpfapps-devops-prerequisites }

* [Cuenta de {{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration)
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* Una app y un [adaptador MFP](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/) de ejemplo
* Cuenta de [GitHub](http://github.com/)
* *Opcional:* instancia de [Bitbar](https://bitbar.com/testing/) y clave de API de Bitbar (puede utilizar cualquier servicio según sus requisitos).


## Creación del servicio Continuous Delivery y la cadena de herramientas
{: #creating-continuous-delivery-service-and-toolchain }

* Busque "Continuous Delivery" en el catálogo de {{ site.data.keyword.cloud_notm }} (o
[pulse aquí](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* Cree el servicio proporcionando el nombre de servicio, la región, etc.

    En el ejemplo siguiente, utilizaremos el nombre de servicio *MFP App/Adapter delivery Test*, la región/ubicación *London* y el grupo de recursos *Default*.

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Página de creación de catálogo para la instancia de servicio de Mobile Foundation")

* Desde el menú Navegación, seleccione **DevOps**, luego pulse **Crear una cadena de herramientas** y busque "Crear su propia cadena de herramientas" para crear una cadena de herramientas desde cero.

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "Página Crear su propia cadena de herramientas con el resultado de búsqueda para crear su propia cadena de herramientas")

* Proporcione el nombre de la cadena de herramientas, la región, etc., para configurarla.


## Integración de GitHub con la cadena de herramientas para el control de versiones y el desencadenante de conducto
{: #integrating-github-with-the-toolchain}

* En la página **Visión general** de la navegación, pulse **Añadir una herramienta** y busque GitHub.
* Configure la **dirección de servidor de GitHub**, **tipo de repositorio** y **URL de repositorio** para la herramienta GitHub.
* Puede crear un repositorio nuevo, hacer fork, clonar o utilizar un repositorio existente.

    En el ejemplo siguiente, utilizaremos el servidor de GitHub "[https://github.com](http://github.com/)", el tipo de repositorio *Existing* y el URL de repositorio *https://github.com/sagar20896/mfp-devops-20181210030116092*.

    ![configuring_toolchain](images/p03_configuring_toolchain.png "Pantalla Configurar la integración que muestra los campos Servidor de GitHub, Tipo de repositorio y URL de repositorio")

### Adición del conducto de entrega a la cadena de herramientas
{: #adding-the-delivery-pipeline-to-the-toolchain}

Utilice **Añadir herramienta** y busque *Delivery Pipeline*. Configure el conducto y pulse
**Crear integración**.

### Adición de etapas al conducto
{: #adding-stages-to-the-pipeline}

#### Etapa 1 - Configuración de {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

En esta etapa, daremos un giro a una instancia de {{ site.data.keyword.mobilefoundation_short }}. La adición de GitHub al conducto se realizaría en esta etapa y se desencadena el conducto siempre que se envíe un cambio al repositorio git. En los pasos siguientes se indica la configuración de GitHub. Puede establecer el desencadenante de etapa en función de sus requisitos. Puede ser manual o automático.

En el ejemplo siguiente, establecemos el Tipo de entrada como *Git repository*, el Repositorio Git como
*mfp-devops-20181210030116092*, el URL de Git como *https://github.com/sagar20896/mfp-devops-20181210030116092* y la rama como
*master*.

![first_stage_git_input](images/p4_first_stage_git_input.png "Pantalla Configurar Mobile Foundation con el separador Entrada seleccionado")

- Pulse **Añadir etapa** y configure el separador **Entrada** para que haga referencia al repositorio de GitHub como se muestra en la imagen.
- En el separador **Trabajos**, pulse **AÑADIR TRABAJO** y seleccione *Desplegar* como tipo de trabajo. Establezca el **Tipo de desplegador** en *Cloud Foundry*.
- Si no tiene una clave de API, puede crear una para su cuenta de
{{ site.data.keyword.cloud_notm }} [aquí](https://cloud.ibm.com/iam/#/apikeys).

Seleccione/rellene los demás campos según sea necesario. Añada las líneas siguientes en el **Script de despliegue**:

```
	#!/bin/bash
	token=$(cf oauth-token | awk '{print $2}')
	json={\"deploy\":\"true\",\"name\":\"$INSTANCE_NAME\",\"token\":\"$token\"}
	echo "Input: $json"
	cf cs "Mobile Foundation" Developer $INSTANCE_NAME -c $json
	echo "Created Service Instance"
	sleep 1m
	cf create-service-key $INSTANCE_NAME Credentials-1
	echo "Created Service Keys"
	credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	echo "Credentials: $credentials"
	password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "Password: $password"
	url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "URL: $url"
	user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "User: $user"
	until $(curl --output /dev/null --silent --head --fail $url/mfpconsole); do
	printf '.'
	sleep 5
	done
	# Deploying the confidential client
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients
	exit
```
{: codeblock}

En el script anterior, utilizamos la CLI de Cloud Foundry para crear una instancia del servicio {{ site.data.keyword.mobilefoundation_short }}.

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "Pantalla Configurar Mobile Foundation con el separador Trabajos seleccionado")

En el separador **Propiedades de entorno**, añada la propiedad *INSTANCE\_NAME* (como propiedad de texto) con el nombre que desea que tenga la instancia de MobileFoundation. Se utilizará como identificador en muchas etapas.

![stage1_environment_properties](images/p06_stage1_environment_properties.png "Pantalla Configurar Mobile Foundation con el separador Propiedades de entorno seleccionado")

#### Etapa 2 - Compilación de un adaptador
{: #stage2-building-an-adapter}

En esta etapa, extraeremos el código fuente del adaptador y lo compilaremos.

- Pulse **Añadir etapa** y configure la etapa según sus preferencias para añadir el repositorio de GitHub como entrada. El **Desencadenante de etapa** se debe establecer en *Ejecutar trabajos cuando la etapa anterior haya finalizado*.

- Cambie al separador **Trabajos** y añada un trabajo de compilación. Seleccione el tipo de compilador como
*Maven* y añada las líneas siguientes en los scripts de compilación.

```
	#!/bin/bash
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd adapters/JavaAdapter"
	cd adapters/JavaAdapter
	echo "#### Build the adapter : mfpdev adapter build"
	mfpdev adapter build
```
{: codeblock}

En el script anterior, instalamos [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) para compilar adaptadores utilizando el mandato adapter de `adapters/JavaAdapter` de nuestro repositorio.

En el ejemplo siguiente, utilizaremos el **Tipo de compilador** como *npm* y el script proporcionado en el script de compilación. Dejaremos los parámetros **Directorio de trabajo** y **Directorio de archivado de compilación** vacíos.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "Pantalla BuildAdapter con el separador Trabajos seleccionado")

#### Etapa 3 - Despliegue de un adaptador
{: #stage3-deploying-an-adapter}

En esta etapa, desplegaremos el adaptador en la instancia de {{ site.data.keyword.mobilefoundation_short }} que hemos creado en la primera etapa.

- Añada una etapa.
- Configure la entrada.
- Establezca el tipo de entrada en **Artefactos de compilación**, **Etapa** en el nombre de la etapa donde se ha compilado el adaptador, y el trabajo que compila el adaptador en la etapa de compilar el adaptador.
- El **Desencadenante de etapa** se debe establecer en *Ejecutar trabajos cuando la etapa anterior haya finalizado*.
- Añada un trabajo de despliegue desde el separador **Trabajos**.
- Establezca el **Tipo de desplegador** como *Cloud Foundry* y configure el resto según sus preferencias.

En el ejemplo siguiente, utilizaremos el **Tipo de desplegador** como *Cloud Foundry*, la
**Región de {{ site.data.keyword.cloud_notm }}** como *Dallas* y la clave de API como la misma que hemos creado en la primera etapa.

![deploy_adapter](images/p08_deploy_adapter.png "Pantalla Desplegar con el separador Trabajos seleccionado")


Utilice el **Script de despliegue** siguiente:

```
	#!/bin/bash
	set -x
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	      credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd adapters/JavaAdapter"
	cd adapters/JavaAdapter
	echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault
	echo "#### Deploy Adapter : mfpdev adapter deploy"
	mfpdev adapter deploy
```
{: codeblock}

El script utiliza el mandato adapter deploy de mfpdev-cli para desplegar el adaptador en la instancia de MFP.

- Establezca *INSTANCE_NAME* en el separador **Propiedades de entorno** con el mismo valor que el que se estableció en la primera etapa cuando se creó la instancia de {{ site.data.keyword.mobilefoundation_short }}. Utilizaremos el mismo nombre de instancia, por lo que el adaptador se desplegará en la instancia que hemos creado en la primera etapa.


#### Etapa 4 - Probar el adaptador
{: #stage4-test-adapter}

En esta etapa, tenemos intención de probar el adaptador que se ha compilado y desplegado en las etapas anteriores. En nuestro repositorio de adaptadores de ejemplo, tenemos scripts en ‘adapters/JavaAdapter/tests’ para probar los puntos finales de adaptador.

- Establezca la **Entrada** como *Repositorio de GitHub*, igual que en las configuraciones de adaptadores de compilación.

- Añada *trabajo de despliegue* en el separador **Trabajos**. Establezca el **Tipo de desplegador** como *Cloud Foundry*. Utilizaremos un trabajo de despliegue en lugar de un trabajo de prueba, porque deploy tiene más opciones para integrarse con Cloud Foundry fácilmente.

Utilice los scripts siguientes para probar adaptadores.

```
	#!/bin/bash
	set -x
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	 credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault

	# Make sure to have the test confidential client. Try another attempt to deploy test confidential client
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients

	echo "#### cd adapters/JavaAdapter/tests"
	cd adapters/JavaAdapter/tests
	echo "#### Testing Adapter Endpoints"
	./resource.sh
	./resource_greet.sh
	./resource_protected.sh
	./resource_testpost.sh
```
{: codeblock}

- Añada *INSTANCE_NAME* en las propiedades de entorno, con los mismos valores que hemos añadido en etapas anteriores.

Puede utilizar la infraestructura de pruebas de API para probar los adaptadores. En este ejemplo, tenemos scripts de shell que llaman al adaptador y lo prueban para ver si es correcto.


#### Etapa 5 - Compilación de apps con Fastlane
{: #stage5-building-apps-with-fastlane}

La entrada para esta etapa debe ser el repositorio de GitHub que hemos utilizado en las etapas anteriores. Esta etapa debe desencadenarse después de que se supere correctamente la etapa anterior (Probar el adaptador).

Para compilar la app, utilizaremos la plantilla de trabajo de despliegue del separador **Trabajos**. Utilice *Cloud Foundry* como **Tipo de desplegador**.

Utilice el script siguiente para compilar la app:

```
	# To use Node.js 6.7.0, uncomment the following line:
	export LANG=en_US.UTF-8

	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	 credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd apps/ResourceRequestAndroid"
	cd apps/ResourceRequestAndroid
	#echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault
	echo "#### Register App with MFP Server : mfpdev app register"
	mfpdev app register


	export JAVA_HOME=/opt/IBM/java8
	cd /home/pipeline

	# Android sdk
	wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
	sudo apt-get install unzip
	unzip /home/pipeline/sdk-tools-linux-3859397.zip
	echo 'y' | /home/pipeline/tools/bin/sdkmanager --licenses
	echo 'y' | /home/pipeline/tools/bin/sdkmanager "platform-tools" "platforms;android-26"

	# Prereq for installing Fastlane: Install RVM
	gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
	\curl -L https://get.rvm.io | bash -s stable --ruby
	source /home/pipeline/.rvm/scripts/rvm get stable --autolibs=enable
	gem -v

	# Install Fastlane
	gem install fastlane -NV

	# Build the apk file
	cd /home/pipeline/$BUILD_ID/apps/ResourceRequestAndroid
	fastlane beta

	mkdir /home/pipeline/temp
	cp ./app/build/outputs/apk/app-release.apk /home/pipeline/temp
	mkdir /home/pipeline/appupload

	# Push the generated apk for git hub for testing
	git config --global user.name $gitPushUser
	git config --global user.email $gitPushEmail
	git config --global push.default matching
	cd /home/pipeline/appupload

	git clone $appPublishUrl .
	ls
	rm -rf

	cp /home/pipeline/temp/app-release.apk .
	git add app-release.apk
	git commit -m "released a new version of apk - build : ($BUILD_ID)"
	echo $apkGitPushUrl
	git push $apkGitPushUrl
```
{: codeblock}

En el script anterior, utilizamos `mfpdev-cli` para registrar la app en
{{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) para compilar y publicar la app.

Las variables de entorno utilizadas en el script se definen en el siguiente separador **Propiedades de entorno**.

En las propiedades de entorno, añada las propiedades siguientes,

- *INSTANCE_NAME* como el **nombre de instancia de MFP**
- *appPublishUrl* como el **repositorio** donde es necesario publicar la app para que pueda extraerla la etapa de pruebas.
- *gitPushUser* como el **nombre de usuario de GitHub**
- *gitPushEmail* como el **correo electrónico del usuario de GitHub**
- *gitPushToken* como la **señal push de git**
- *apkGitPushUrl* como **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `por ejemplo: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### Etapa 6 - Probar la app utilizando Bitbar
{: #stage6-test-the-app-using-bitbar}

Esta etapa también se abre finalizada. Puede utilizar cualquier infraestructura de pruebas según sus requisitos. En este ejemplo, utilizaremos
[Bitbar](https://bitbar.com/testing/).

- Añada los detalles del repositorio de GitHub acerca del proyecto de prueba que tenga (este proyecto estará compuesto por todos los artefactos y scripts de prueba utilizados para dar soporte a las pruebas).

- Para Bitbar, añada un trabajo de compilación en el separador **Trabajos** y seleccione *Maven* como
**Tipo de compilador**.

Utilice un script de compilación similar al script siguiente,

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

Para el script anterior, necesita algunas variables de entorno.

- *screenshot\_dir* como **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* como la **vía de acceso de GitHub** a la aplicación que tiene pensado probar
- *executionType* como el **tipo de ejecución**
- *test* como la **prueba** que desea realizar
- *bitbarApiKey* como la **clave de API de Bitbar**
- *testdroid_project* como el **proyecto**.


#### Etapa 7 - Destrucción de {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

En esta etapa destruiremos la instancia de {{ site.data.keyword.mobilefoundation_short }} que se ha creado en la primera etapa para la compilación, el despliegue y las pruebas de etapas anteriores.

No habrá ninguna entrada en esta etapa.

- Cree un *Trabajo de despliegue* en el separador **Trabajos**.
- Utilice el script siguiente,

```
	#!/bin/bash
	sleep 60
	cf dsk -f $INSTANCE_NAME-Analytics Credentials-1
	cf ds -f $INSTANCE_NAME-Analytics
	cf dsk -f $INSTANCE_NAME Credentials-1
	cf ds -f $INSTANCE_NAME
	cf d -f $INSTANCE_NAME-Server
	exit
```
{: codeblock}

- Establezca *INSTANCE_NAME* en el separador **Propiedades de entorno** con el mismo valor que el que se estableció en la primera etapa cuando se creó la instancia de {{ site.data.keyword.mobilefoundation_short }}.
