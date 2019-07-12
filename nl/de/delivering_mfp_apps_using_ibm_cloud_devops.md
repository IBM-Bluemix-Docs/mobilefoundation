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


# {{ site.data.keyword.mobilefoundation_short }}-Apps mithilfe von {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }} bereitstellen
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

Dieses Lernprogramm unterstützt Sie bei der automatisierten Bereitstellung von Apps und Adaptern für IBM  {{ site.data.keyword.mobilefoundation_short }} mithilfe des {{ site.data.keyword.jazzhub_title }}
 unter {{ site.data.keyword.cloud_notm }}.

Die folgende Abbildung zeigt eine Übersicht der Pipeline.

![overview_of_pipeline](images/p00_overview_of_pipeline.png "Sechs Phasen der DevOps-Pipeline")


## Voraussetzungen
{: #mpfapps-devops-prerequisites }

* Ein [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration)-Konto
* Die [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) (IBM MobileFirst Platform Foundation Development Command Line Interface)
* Eine Beispielapp und einen [MFP-Adapter](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* Ein [GitHub](http://github.com/)-Konto
* *Optional:* [Bitbar](https://bitbar.com/testing/)-Instanz und Bitbar-API-Schlüssel (Sie können einen beliebigen Service gemäß Ihren Anforderungen verwenden).


## Continuous Delivery-Service und Toolchain erstellen
{: #creating-continuous-delivery-service-and-toolchain }

* Suchen Sie nach 'Continuous Delivery' im {{ site.data.keyword.cloud_notm }}-Katalog (oder [klicken Sie hier](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* Erstellen Sie den Service, indem Sie den Servicenamen, die Region usw. bereitstellen.

    Im folgenden Beispiel werden der Servicename als *Bereitstellungstest für MFP-App/Adapter*, die Region/der Standort als *London* und die Ressourcengruppe als *Standard* angegeben.

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Katalogerstellungsseite für Mobile Foundation-Serviceinstanzen")

* Wählen Sie im Navigationsmenü **DevOps** aus und klicken Sie anschließend auf **Toolchain erstellen**, suchen Sie dann nach “Eigene Toolchain erstellen”, um eine völlig neue Toolchain zu erstellen. 

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "Seite zum Erstellen einer eigenen Toolchain mit Suchergebnis zum Erstellen einer eigenen Toolchain")

* Geben Sie den Toolchainnamen, die Region usw. für die Konfiguration an.


## Integration von GitHub mit der Toolchain für Versionssteuerung und für Pipelineauslöser
{: #integrating-github-with-the-toolchain}

* Klicken Sie auf der Seite **Übersicht** in der Navigation auf **Tool hinzufügen** und suchen Sie nach einem GitHub.
* Konfigurieren Sie das GitHub-Tool für die **GitHub-Server-Adresse**, den **Repository-Typ** und die **Repository-URL**.
* Sie können ein neues Repository erstellen oder ein vorhandenes Repository aufspalten, klonen oder verwenden.

    Im folgenden Beispiel werden der GitHub-Server als '[https://github.com](http://github.com/)', der Repository-Typ als '*Vorhanden*' und die Repository-URL als '*https://github.com/sagar20896/mfp-devops-20181210030116092*' angegeben.

    ![configuring_toolchain](images/p03_configuring_toolchain.png "Anzeige zur Konfiguration der Integration mit den Feldern für GitHub-Server, Repositorytyp und Repository-URL")

### Delivery Pipeline zur Toolchain hinzufügen
{: #adding-the-delivery-pipeline-to-the-toolchain}

Klicken Sie auf **Tool hinzufügen** und suchen Sie nach *Delivery Pipeline*. Konfigurieren Sie die Pipeline und klicken Sie auf **Integration erstellen**.

### Stufen zur Pipeline hinzufügen
{: #adding-stages-to-the-pipeline}

#### Stufe 1 - {{ site.data.keyword.mobilefoundation_short }} einrichten

{: #stage1-setting-up-mobile-foundation}

In dieser Stufe richten Sie eine Instanz von {{ site.data.keyword.mobilefoundation_short }} ein. Das Hinzufügen von GitHub zur Pipeline würde in dieser Stufe erfolgen und die Pipeline würde jedes Mal ausgelöst werden, wenn eine Änderung mit Push-Operation an das Git-Repository übertragen wird. Die nachfolgenden Schritte zeigen die GitHub-Konfiguration. Sie können den Stufenauslöser in Abhängigkeit von Ihren Anforderungen festlegen. Dies kann manuell oder automatisch erfolgen.

Im folgenden Beispiel werden der Eingabetyp als *Git-Repository*, das Git-Repository als *mfp-devops-20181210030116092* und die Git-URL als *https://github.com/sagar20896/mfp-devops-20181210030116092* und der Zweig als *Master* angegeben.

![first_stage_git_input](images/p4_first_stage_git_input.png "Anzeige zum Einrichten der Mobile Foundation mit der ausgewählten Registerkarte für die Eingabe")

- Klicken Sie auf **Stufe hinzufügen** und konfigurieren Sie die Registerkarte **Eingabe** so, dass sie auf das GitHub-Repository verweist, wie in der Abbildung dargestellt.
- Klicken Sie auf der Registerkarte **Jobs** auf **Job hinzufügen** und wählen Sie *Bereitstellen* als Jobtyp aus. Wählen Sie **Cloud Foundry** als *Bereitstellertyp* aus.
- Wenn Sie über keinen API-Schlüssel verfügen, können Sie für Ihr {{ site.data.keyword.cloud_notm }}-Konto [hier](https://cloud.ibm.com/iam/#/apikeys) einen Schlüssel erstellen.

Wählen Sie die anderen Felder nach Bedarf aus bzw. füllen Sie sie aus. Fügen Sie die folgenden Zeilen im **Bereitstellungsscript** hinzu:

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
	# Bereitstellung des vertraulichen Clients
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients
	exit
```
{: codeblock}

Im vorherigen Script wird die Cloud Foundry-CLI zum Erstellen einer {{ site.data.keyword.mobilefoundation_short }}-Serviceinstanz verwendet.

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "Anzeige zum Einrichten der Mobile Foundation mit der ausgewählten Registerkarte für Jobs")

Fügen Sie auf der Registerkarte **Umgebungseigenschaften** die Eigenschaft *INSTANCE\_NAME* (als Texteigenschaft) als den Namen der MobileFoundation-Instanz hinzu. Dieser wird als Kennung über viele Stufen hinweg verwendet.

![stage1_environment_properties](images/p06_stage1_environment_properties.png "Anzeige zum Einrichten der Mobile Foundation mit der ausgewählten Registerkarte für die Umgebung")

#### Stufe 2 - Adapter erstellen
{: #stage2-building-an-adapter}

In dieser Stufe extrahieren Sie den Quellcode des Adapters und erstellen den Adapter.

- Klicken Sie auf **Stufe hinzufügen** und konfigurieren Sie die Stufe basierend auf Ihren Vorgaben zum Hinzufügen eines GitHub-Repositorys als Eingabe. Der **Stufenauslöser** muss auf *Jobs ausführen, wenn die vorherige Stufe abgeschlossen ist* festgelegt werden.

- Wechseln Sie zur Registerkarte **Jobs** und fügen Sie einen Buildjob hinzu. Wählen Sie als Builder-Typ *Maven* aus und fügen Sie die folgenden Zeilen zu den Build-Scripts hinzu.

```
	#!/bin/bash
	# Zur Verwendung von Node.js 6.7.0 entfernen Sie die Kommentarzeichen aus folgender Zeile:
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

Im vorherigen Script installieren Sie die [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) (IBM MobileFirst Platform Foundation Development Command Line Interface) zum Erstellen von Adaptern, indem Sie einen Adapterbefehl im Verzeichnis `adapters/JavaAdapter` des Repositorys verwenden.

Im folgenden Beispiel werden *npm* als **Builder-Typ** und das Script verwendet, das im Build-Script angegeben ist. Lassen Sie die Parameter für **Arbeitsverzeichnis** und **Archivverzeichnis erstellen** leer.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "Anzeige des Buildadapters mit der ausgewählten Registerkarte für Jobs")

#### Stufe 3 - Adapter bereitstellen
{: #stage3-deploying-an-adapter}

In dieser Stufe stellen Sie den Adapter in der {{ site.data.keyword.mobilefoundation_short }}-Instanz bereit, die Sie in der ersten Stufe erstellt haben.

- Fügen Sie eine Stufe hinzu.
- Konfigurieren Sie die Eingabe.
- Setzen Sie den Eingabetyp auf **Buildartefakt**, geben Sie **Stufe** als den Namen der Stufe, in der der Adapter erstellt wird, und als den Job an, mit dem der Adapter in der Stufe zur Adaptererstellung erstellt wird.
- Der **Stufenauslöser** muss auf *Jobs ausführen, wenn die vorherige Stufe abgeschlossen ist* festgelegt werden.
- Fügen Sie von der Registerkarte **Jobs** einen Bereitstellungsjob hinzu.
- Wählen Sie *Cloud Foundry* als **Bereitstellertypy** aus und konfigurieren Sie den Rest gemäß Ihren Vorgaben.

Im folgenden Beispiel wird *Cloud Foundry* als **Bereitstellertyp** verwendet und als **{{ site.data.keyword.cloud_notm }}-Region** ist *Dallas* angegeben. Der API-Schlüssel entspricht dem in Stufe 1 erstellten Schlüssel.

![deploy_adapter](images/p08_deploy_adapter.png "Anzeige für die Bereitstellung mit der ausgewählten Registerkarte für Jobs")


Verwenden Sie das nachfolgende **Bereitstellungsscript**:

```
	#!/bin/bash
	set -x
	# Zur Verwendung von Node.js 6.7.0 entfernen Sie die Kommentarzeichen aus folgender Zeile:
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

Das Script verwendet den Befehl 'adapter deploy' der IBM MobileFirst Platform Foundation Development Command Line Interface (mfpdev-cli) zum Bereitstellen des Adapters für die MFP-Instanz.

- Legen Sie für *INSTANCE_NAME* auf der Registerkarte **Umgebungseigenschaften** den gleichen Wert fest, den Sie auch beim Erstellen der {{ site.data.keyword.mobilefoundation_short }}-Instanz in der Stufe festgelegt haben. Es wird der gleiche Instanzname verwendet, damit der Adapter für die Instanz bereitgestellt wird, die Sie in Stufe 1 erstellt haben.


#### Stufe 4 - Adapter testen
{: #stage4-test-adapter}

In dieser Stufe testen Sie den Adapter, den Sie in den vorherigen Stufen erstellt und getestet haben. Im vorliegenden beispielhaften Adapter-Repository befinden sich Scripts im Verzeichnis 'adapters/JavaAdapter/tests', die zum Testen der Adapterendpunkte verwendet werden können.

- Legen Sie die **Eingabe** als *GitHub-Repository* entsprechend den Konfigurationen beim Erstellen von Adaptern fest.

- Fügen Sie *Bereitstellungsjob* auf der Registerkarte **Jobs** hinzu. Wählen Sie *Cloud Foundry* als **Bereitstellertyp** aus. Der Bereitstellungsjob wird anstelle von Testjobs verwendet, da im Rahmen der Bereitstellung Optionen verfügbar sind, die problemlos in Cloud Foundry integriert werden können.

Verwenden Sie die folgenden Scripts zum Testen von Adaptern.

```
	#!/bin/bash
	set -x
	# Zur Verwendung von Node.js 6.7.0 entfernen Sie die Kommentarzeichen aus folgender Zeile:
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

	# Stellen Sie sicher, dass Sie über den vertraulichen Testclient verfügen. Versuchen Sie erneut, den vertraulichen Testclient bereitzustellen
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

- Fügen Sie *INSTANCE_NAME* in den Umgebungseigenschaften hinzu. Verwenden Sie dabei die gleichen Werte wie aus den vorherigen Stufen.

Sie können API-Testframeworks zum Testen der Adapter verwenden. In diesem Beispiel verfügen Sie über Shell-Scripts, mit denen der Adapter aufgerufen und auf seine Richtigkeit überprüft werden kann.


#### Stufe 5 - Apps mit Fastlane erstellen
{: #stage5-building-apps-with-fastlane}

Die Eingabe in dieser Stufe sollte das GitHub-Repository sein, das Sie in den vorherigen Stufen verwendet haben. Diese Stufe muss ausgelöst werden, nachdem die vorherige Stufe (Adapter testen) ausgeführt wurde.

Zum Erstellen der App verwenden Sie die Vorlage zum Bereitstellen des Jobs auf der Registerkarte **Jobs**. Verwenden Sie *Cloud Foundry* als **Bereitstellertyp**.

Verwenden Sie das folgende Script, um die App zu erstellen:

```
	# Zur Verwendung von Node.js 6.7.0 entfernen Sie die Kommentarzeichen aus folgender Zeile:
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

	# Android-SDK
	wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
	sudo apt-get install unzip
	unzip /home/pipeline/sdk-tools-linux-3859397.zip
	echo 'y' | /home/pipeline/tools/bin/sdkmanager --licenses
	echo 'y' | /home/pipeline/tools/bin/sdkmanager "platform-tools" "platforms;android-26"

	# Voraussetzungen zur Installation von Fastlane: RVM installieren
	gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
	\curl -L https://get.rvm.io | bash -s stable --ruby
	source /home/pipeline/.rvm/scripts/rvm get stable --autolibs=enable
	gem -v

	# Installieren Sie Fastlane
	gem install fastlane -NV

	# Erstellen Sie die APK-Datei
	cd /home/pipeline/$BUILD_ID/apps/ResourceRequestAndroid
	fastlane beta

	mkdir /home/pipeline/temp
	cp ./app/build/outputs/apk/app-release.apk /home/pipeline/temp
	mkdir /home/pipeline/appupload

	# Übertragen Sie die generierte APK-Datei für GitHub mit Push zum Testen
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

Im vorherigen Script wird `mfpdev-cli` verwendet, um die App bei {{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) zu registrieren, um die App zu erstellen und freizugeben. 

Die Umgebungsvariablen, die im Script verwendet werden, werden auf der nächsten Registerkarte **Umgebungseigenschaften** definiert.

Fügen Sie in den Umgebungseigenschaften die folgenden Eigenschaften hinzu:

- *INSTANCE_NAME* als **MPF-Instanzname**
- *appPublishUrl* als **Repository**, in dem die App veröffentlicht werden muss, damit sie in der Teststufe extrahiert werden kann.
- *gitPushUser* als **GitHub-Benutzername**
- *gitPushEmail* als **E-Mail-Adresse des GitHub-Benutzers**
- *gitPushToken* als **Git-Push-Token**
- *apkGitPushUrl* als **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `z. B.: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### Stufe 6 - App mit Bitbar testen
{: #stage6-test-the-app-using-bitbar}

Diese Stufe ist auch offen ohne Ende konzipiert. Sie können jedes Testframework basierend auf Ihren Anforderungen verwenden. In diesem Beispiel wird [Bitbar](https://bitbar.com/testing/) verwendet.

- Fügen Sie die GitHub-Repositorydetails zu Ihrem Testprojekt hinzu (dieses Projekt umfasst alle Testscripts und Artefakte, die unterstützend zum Testen verwendet werden).

- Fügen Sie für Bitbar einen Buildjob auf der Registerkarte **Jobs** hinzu und wählen Sie als **Builder-Typ** die Option *Maven* aus.

Verwenden Sie ein Build-Script ähnlich dem folgenden Script:

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

Für das vorherige Script benötigen Sie einige Umgebungsvariablen.

- *screenshot\_dir* als **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* als **GitHub-Pfad** zu der Anwendung, die Sie testen möchten
- *executionType* als **Ausführungstyp**
- *Test* als **Test**, den Sie ausführen möchten
- *bitbarApiKey* als **Bitbar-API-Schlüssel**
- *testdroid_project* als **Projekt**.


#### Stufe 7 - {{ site.data.keyword.mobilefoundation_short }} abrüsten
{: #stage7-tearing-down-mobile-foundation}

In dieser Stufe rüsten Sie die {{ site.data.keyword.mobilefoundation_short }}-Instanz ab, die in Stufe 1 zum Erstellen, Bereitstellen und Testen in den vorherigen Stufen erstellt wurde.

In dieser Stufe gibt es keine Eingaben.

- Erstellen Sie auf der Registerkarte **Jobs** einen *Bereitstellungsjob*.
- Verwenden Sie das folgende Script:

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

- Legen Sie für *INSTANCE_NAME* auf der Registerkarte **Umgebungseigenschaften** den gleichen Wert fest, den Sie auch beim Erstellen der {{ site.data.keyword.mobilefoundation_short }}-Instanz in der Stufe festgelegt haben.
