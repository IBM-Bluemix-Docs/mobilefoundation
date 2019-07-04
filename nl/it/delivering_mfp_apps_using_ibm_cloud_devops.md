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


# Fornitura delle applicazioni {{ site.data.keyword.mobilefoundation_short }} utilizzando {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }}
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

Questa esercitazione ti consente di automatizzare la fornitura delle applicazioni e degli adattatori a IBM  {{ site.data.keyword.mobilefoundation_short }} utilizzando {{ site.data.keyword.jazzhub_title }}
 in {{ site.data.keyword.cloud_notm }}.

La seguente immagine fornisce una panoramica della pipeline.

![overview_of_pipeline](images/p00_overview_of_pipeline.png "Sei fasi della pipeline DevOps")


## Prerequisiti
{: #mpfapps-devops-prerequisites }

* Un account [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration)
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* Un'applicazione di esempio e un [adattatore MFP](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* Un account [GitHub](http://github.com/)
* *Facoltativo:* l'istanza [Bitbar](https://bitbar.com/testing/) e la chiave API Bitbar (Puoi utilizzare qualsiasi servizio in base ai tuoi requisiti).


## Creazione del servizio Continuous Delivery e della toolchain
{: #creating-continuous-delivery-service-and-toolchain }

* Ricerca "Continuous Delivery" nel catalogo {{ site.data.keyword.cloud_notm }} (oppure [fai clic qui](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* Crea il servizio fornendo il nome servizio, la regione e così via.

    Nel seguente esempio, utilizziamo il nome servizio *MFP App/Adapter delivery Test*, la regione/località *London* e il gruppo di risorse *Default*.

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Pagina Catalog create per l'istanza del servizio Mobile Foundation")

* Dal menu di navigazione, seleziona **DevOps**, quindi fai clic su **Create a toolchain** e ricerca “Build your own toolchain” per creare una toolchain da zero.

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "Pagina Create your own toolchain con il risultato della ricerca per la creazione della tua toolchain")

* Specifica il nome toolchain, la regione e così via per eseguire la configurazione.


## Integrazione di GitHub con la toolchain per il controllo della versione e l'attivazione della pipeline
{: #integrating-github-with-the-toolchain}

* Dalla pagina **Overview** nella navigazione, fai clic su **Add a Tool** e ricerca GitHub.
* Configura lo strumento GitHub per i valori di **indirizzo server GitHub**, **tipo di repository** e **URL del repository**.
* Puoi creare un nuovo repository, una nuova biforcazione, un nuovo clone oppure utilizzare un repository esistente.

    Nel seguente esempio, utilizziamo il server GitHub "[https://github.com](http://github.com/)", il tipo di repository *Existing* e l'URL del repository *https://github.com/sagar20896/mfp-devops-20181210030116092*.

    ![configuring_toolchain](images/p03_configuring_toolchain.png "Schermata Configure the Integration che mostra i campi GitHub server, Repository type e Repository URL")

### Aggiunta della Delivery Pipeline alla toolchain
{: #adding-the-delivery-pipeline-to-the-toolchain}

Utilizza **Add Tool** e ricerca *Delivery Pipeline*. Configura la pipeline e fai clic su **Create integration**.

### Aggiunta di fasi alla pipeline
{: #adding-stages-to-the-pipeline}

#### Fase 1 - Configurazione di {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

In questa fase, effettueremo lo spin-up di un'istanza di {{ site.data.keyword.mobilefoundation_short }}. L'aggiunta di GitHub alla pipeline viene effettuata in questa fase e attiva la pipeline ogni volta che viene eseguito il push di una modifica nel repository git. I passi riportati di seguito mostrano la configurazione di GitHub. Puoi impostare l'attivazione della fase in base ai tuoi requisiti. Può essere manuale o automatica.

Nel seguente esempio, impostiamo Input Type come *Git repository*, Git repository come *mfp-devops-20181210030116092* e Git URL come *https://github.com/sagar20896/mfp-devops-20181210030116092* e il ramo come *master*.

![first_stage_git_input](images/p4_first_stage_git_input.png "Schermata Set up Mobile Foundation con la scheda Input selezionata")

- Fai clic su **Add stage** e configura la scheda **Input** in modo che punti al repository GitHub come mostrato nell'immagine.
- Nella scheda **Jobs**, fai clic su **ADD JOB** e seleziona *Deploy* come tipo di lavoro. Seleziona **Deployer type** come *Cloud Foundry*.
- Se non hai una chiave API, puoi crearne una per il tuo account {{ site.data.keyword.cloud_notm }} [qui](https://cloud.ibm.com/iam/#/apikeys).

Seleziona/riempi gli altri campi come richiesto. Aggiungi le seguenti righe nello **script di distribuzione**:

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

Nel precedente script, utilizziamo la CLI di Cloud Foundry per creare un'istanza del servizio {{ site.data.keyword.mobilefoundation_short }}.

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "Schermata Set up Mobile Foundation con la scheda Jobs selezionata")

Nella scheda **Environment properties**, aggiungi la proprietà *INSTANCE\_NAME* (come proprietà di testo) indicando quale desideri sia il nome dell'istanza MobileFoundation. Verrà utilizzato come identificativo in molte fasi.

![stage1_environment_properties](images/p06_stage1_environment_properties.png "Schermata Set up Mobile Foundation con la scheda Environment properties selezionata")

#### Fase 2 - Creazione di un adattatore
{: #stage2-building-an-adapter}

In questa fase, eseguiremo il pull del codice sorgente dell'adattatore e lo creeremo.

- Fai clic su **Add stage** e configura la fase in base alle tue preferenze per aggiungere il repository GitHub come input. **Stage trigger** deve essere impostato su *Run jobs when the previous stage is completed*.

- Passa alla scheda **Jobs** e aggiungi un lavoro di build. Seleziona il tipo di builder come *Maven* e aggiungi le seguenti righe negli script di build.

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

Nel precedente script, installiamo [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) per creare gli adattatori utilizzando il comando dell'adattatore nell'`adapters/JavaAdapter` del nostro repository.

Nel seguente esempio, utilizziamo **Builder type** come *npm* e utilizziamo lo script che viene fornito nello script di build. Lasciamo vuoti i parametri **Working directory** e **Build archive directory**.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "Schermata BuildAdapter con la scheda Jobs selezionata")

#### Fase 3 - Distribuzione di un adattatore
{: #stage3-deploying-an-adapter}

In questa fase, distribuiamo l'adattatore nell'istanza {{ site.data.keyword.mobilefoundation_short }} che abbiamo creato nella prima fase.

- Aggiungi una fase.
- Configura l'input.
- Imposta il tipo di input su **Build Artifacts**, **Stage** come il nome della fase in cui viene creato l'adattatore e il lavoro che crea l'adattatore nella fase di creazione dell'adattatore.
- **Stage trigger** deve essere impostato su *Run jobs when the previous stage is completed*.
- Aggiungi un lavoro di distribuzione dalla scheda **Jobs**.
- Seleziona **Deployer type** come *Cloud Foundry* e configura il resto in base alle tue preferenze.

Nel seguente esempio, utilizziamo **Deployer type** come *Cloud Foundry*, **{{ site.data.keyword.cloud_notm }} Region** come *Dallas*, la chiave API come la stessa chiave che abbiamo creato nella prima fase.

![deploy_adapter](images/p08_deploy_adapter.png "Schermata Deploy con la scheda Jobs selezionata")


Utilizza lo **script di distribuzione** riportato di seguito:

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

Lo script utilizza il comando di distribuzione dell'adattatore della mfpdev-cli per distribuire l'adattatore nell'istanza MFP.

- Imposta *INSTANCE_NAME* nella scheda **Environment properties** con lo stesso valore impostato nella prima fase quando è stata creata l'istanza {{ site.data.keyword.mobilefoundation_short }}. Utilizziamo lo stesso nome istanza in modo che l'adattatore venga distribuito nell'istanza che abbiamo creato nella prima fase.


#### Fase 4 - Verifica dell'adattatore
{: #stage4-test-adapter}

In questa fase, intendiamo verificare l'adattatore che è stato creato e distribuito nelle fasi precedenti. Nel nostro repository di adattatori di esempio, abbiamo gli script in ‘adapters/JavaAdapter/tests’ per verificare gli endpoint dell'adattatore.

- Imposta **Input** con lo stesso valore *GitHub repository* delle configurazioni degli adattatori di build.

- Aggiungi il *lavoro di distribuzione* nella scheda **Jobs**. Seleziona **Deployer type** come *Cloud Foundry*. Utilizziamo un lavoro di distribuzione invece che uno di test perché la distribuzione offre due opzioni per eseguire facilmente l'integrazione con cloud foundry.

Utilizza i seguenti script per verificare gli adattatori.

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

- Aggiungi *INSTANCE_NAME* nelle proprietà dell'ambiente, gli stessi valori che abbiamo aggiunto nelle fasi precedenti.

Puoi utilizzare il framework di test API per verificare gli adattatori. In questo esempio, abbiamo script shell che richiamano l'adattatore e verificano la correttezza.


#### Fase 5 - Creazione di applicazioni con Fastlane
{: #stage5-building-apps-with-fastlane}

L'input di questa fase deve essere il repository GitHub che abbiamo utilizzato nelle fasi precedenti. Questa fase deve essere attivata una volta eseguita la fase precedente (Verifica dell'adattatore).

Per creare l'applicazione, utilizziamo il template del lavoro di distribuzione nella scheda **Jobs**. Utilizza *Cloud Foundry* come **Deployer type**.

Utilizza il seguente script per creare l'applicazione:

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

Nel precedente script, utilizziamo `mfpdev-cli` per registrare l'applicazione in {{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) per creare e rilasciare l'applicazione. 

Le variabili di ambiente utilizzate nello script sono definite nella prossima scheda relativa alle **proprietà dell'ambiente**.

Nelle proprietà dell'ambiente, aggiungi le seguenti proprietà:

- *INSTANCE_NAME* come il **nome istanza mfp**
- *appPublishUrl* come **Repository** in cui deve essere pubblicata l'applicazione in modo che la fase di test possa eseguirne il pull.
- *gitPushUser* come il **nome utente GitHub**
- *gitPushEmail* come l'**email dell'utente GitHub**
- *gitPushToken* come il **token push git**
- *apkGitPushUrl* come **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `eg.: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### Fase 6 - Verifica dell'applicazione utilizzando Bitbar
{: #stage6-test-the-app-using-bitbar}

Anche questa fase è di tipo aperto. Puoi utilizzare qualsiasi framework di test basato sui tuoi requisiti. In questo esempio, utilizziamo [Bitbar](https://bitbar.com/testing/).

- Aggiungi i dettagli del repository GitHub relativi al tuo progetto di test (questo progetto è composto da tutti gli script di test e dalle risorse utente utilizzati per supportare il test).

- Per Bitbar, aggiungi un lavoro di build nella scheda **Jobs** e seleziona **Builder type** come *Maven*.

Utilizza uno script di build simile al seguente:

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

Per il precedente script, hai bisogno di alcune variabili di ambiente. 

- *screenshot\_dir* come **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* come il **percorso GitHub** all'applicazione che vuoi verificare
- *executionType* come il **tipo di esecuzione**
- *test* come il **Test** che desideri eseguire
- *bitbarApiKey* come la **chiave API Bitbar**
- *testdroid_project* come il **progetto**.


#### Fase 7 - Rimozione di {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

In questa fase, rimuoveremo l'istanza {{ site.data.keyword.mobilefoundation_short }} creata nella prima fase per la creazione, la distribuzione e la verifica nelle fasi precedenti.

Per questa fase non è previsto input.

- Crea un *lavoro di distribuzione* nella scheda **Jobs**.
- Utilizza il seguente script:

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

- Imposta *INSTANCE_NAME* nella scheda **Environment properties** con lo stesso valore impostato nella prima fase quando è stata creata l'istanza {{ site.data.keyword.mobilefoundation_short }}.
