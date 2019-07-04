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


# 使用 {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }} 提供 {{ site.data.keyword.mobilefoundation_short }} 應用程式
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

本指導教學可使用 {{ site.data.keyword.cloud_notm }} 上的 {{ site.data.keyword.jazzhub_title }}，協助您將應用程式及配接器自動提供給 IBM {{ site.data.keyword.mobilefoundation_short }}。

下列影像提供管線的概觀。

![overview_of_pipeline](images/p00_overview_of_pipeline.png "DevOps 管道的六個階段")


## 必要條件
{: #mpfapps-devops-prerequisites }

* [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration) 帳戶
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* 一個範例應用程式及 [MFP 配接器](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* [GitHub](http://github.com/) 帳戶
* *選用項目：* [Bitbar](https://bitbar.com/testing/) 實例及 Bitbar API 金鑰（您可以根據需求使用任何服務）。


## 建立 Continuous Delivery 服務及工具鏈
{: #creating-continuous-delivery-service-and-toolchain }

* 在「{{ site.data.keyword.cloud_notm }} 型錄」中搜尋 "Continuous Delivery"（或[按一下這裡](https://cloud.ibm.com/catalog/services/continuous-delivery)）。
* 提供服務名稱、地區等等來建立服務。

    在下列範例中，對於服務名稱使用 *MFP App/Adapter delivery Test*、對於地區/位置使用 *London*、對於資源群組使用 *Default*。

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Mobile Foundation 服務實例的型錄建立頁面")

* 從導覽功能表中，選取 **DevOps**，然後按一下**建立工具鏈**，並搜尋「建置您自己的工具鏈」以從頭開始建立工具鏈。

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "建立您自己的工具鏈頁面，其中顯示了「建置您自己的工具鏈」的搜尋結果")

* 提供要配置的工具鏈名稱、地區等等。


## 將 GitHub 與版本控制及管線觸發程式的工具鏈整合
{: #integrating-github-with-the-toolchain}

* 從導覽的**概觀**頁面中，按一下**新增工具**並搜尋 GitHub。
* 配置 GitHub 工具的 **GitHub 伺服器位址**、**儲存庫類型**及**儲存庫 URL**。
* 您可以建立新儲存庫；分出、複製或使用現有的儲存庫。

    在下列範例中，對於 GitHub 伺服器使用 "[https://github.com](http://github.com/)"、對於儲存庫類型使用 *Existing*、對於儲存庫 URL 使用 *https://github.com/sagar20896/mfp-devops-20181210030116092*。

    ![configuring_toolchain](images/p03_configuring_toolchain.png "「配置整合」畫面，顯示了「GitHub 伺服器」、「儲存庫類型」和「儲存庫 URL」欄位")

### 將交付管線新增至工具鏈
{: #adding-the-delivery-pipeline-to-the-toolchain}

使用**新增工具**，並搜尋 *Delivery Pipeline*。請配置管線，然後按一下**建立整合**。

### 將階段新增至管線
{: #adding-stages-to-the-pipeline}

#### 階段 1 - 設定 {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

在此階段中，我們將啟動 {{ site.data.keyword.mobilefoundation_short }} 的實例。將 GitHub 新增至管線的作業處於這個階段，只要將變更推送至 Git 儲存庫時，它就會觸發管線。下列步驟會顯示 GitHub 配置。您可以根據需求設定階段觸發程式。它可以是手動或自動的。

在下列範例中，我們將「輸入類型」設為 *Git repository*、將 Git 儲存庫設為 *mfp-devops-20181210030116092* 並將 Git URL 設為 *https://github.com/sagar20896/mfp-devops-20181210030116092*，以及將分支設為 *master*。

![first_stage_git_input](images/p4_first_stage_git_input.png "「設定 Mobile Foundation」畫面，其中選取了「輸入」標籤")

- 按一下**新增階段**，然後配置**輸入**標籤，以指向影像中顯示的 GitHub 儲存庫。
- 在**工作**標籤中，按一下**新增工作**，然後選取*部署* 作為工作類型。將**部署者類型**選取為 *Cloud Foundry*。
- 如果您沒有 API 金鑰，則可以在[這裡](https://cloud.ibm.com/iam/#/apikeys)為 {{ site.data.keyword.cloud_notm }} 帳戶建立 API 金鑰。

視需要選取/填入其他欄位。在**部署 Script** 中新增下列幾行：

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

在上述 Script 中，我們使用 Cloud Foundry CLI 來建立 {{ site.data.keyword.mobilefoundation_short }} 服務實例。

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "「設定 Mobile Foundation」畫面，其中選取了「工作」標籤")

在**環境內容**標籤中，將內容 *INSTANCE\_NAME*（作為文字內容）新增為要作為 MobileFoundation 實例名稱的內容。它將作為許多階段中的 ID 使用。

![stage1_environment_properties](images/p06_stage1_environment_properties.png "「設定 Mobile Foundation」畫面，其中選取了「環境內容」標籤")

#### 階段 2 - 建置配接器
{: #stage2-building-an-adapter}

在此階段中，我們會取回配接器原始碼並進行建置。

- 按一下**新增階段**，然後根據您的喜好設定來配置階段，以將 GitHub 儲存庫新增為輸入。必須將**階段觸發程式**設為*在完成前一個階段時執行工作*。

- 切換至**工作**標籤，並新增建置工作。將建置器類型選取為 *Maven*，並在建置 Script 中新增下列幾行。

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

在上述 Script 中，我們安裝 [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) 以在儲存庫的 `adapters/JavaAdapter` 中使用配接器指令建置配接器。

在下列範例中，我們使用 *npm* 作為**建置器類型**，並使用建置 Script 中提供的 Script。我們會將**工作目錄**及**建置保存目錄**參數保留為空白。

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "「建置配接器」畫面，其中選取了「工作」標籤")

#### 階段 3 - 部署配接器
{: #stage3-deploying-an-adapter}

在此階段中，我們將配接卡部署至我們在第一個階段中建立的 {{ site.data.keyword.mobilefoundation_short }} 實例。

- 新增階段。
- 配置輸入。
- 將輸入類型設為**建置構件**、將**階段**設為建置配接器的階段名稱，以及設定在建置配接器階段中建置配接器的工作。
- 必須將**階段觸發程式**設為*在完成前一個階段時執行工作*。
- 從**工作**標籤中新增部署工作。
- 將**部署者類型**選取為 *Cloud Foundry*，並根據您的喜好設定來配置其餘項目。

在下列範例中，我們使用 *Cloud Foundry* 作為**部署者類型**、使用 *Dallas* 作為 **{{ site.data.keyword.cloud_notm }} 地區**，API 金鑰與我們在第一個階段中建立的 API 金鑰相同。

![deploy_adapter](images/p08_deploy_adapter.png "「部署」畫面，其中選取了「工作」標籤")


使用下列**部署 Script**：

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

此 Script 使用 mfpdev-cli 的配接器部署指令，將配接卡部署至 MFP 實例。

- 在**環境內容**標籤中，以建立 {{ site.data.keyword.mobilefoundation_short }} 實例時在第一個階段中設定的相同值來設定 *INSTANCE_NAME<*。我們使用相同的實例名稱，讓配接器部署至我們在第一個階段中建立的實例。


#### 階段 4 - 測試配接器
{: #stage4-test-adapter}

在此階段中，我們打算測試先前階段中所建置及部署的配接器。在我們的範例配接器儲存庫中，我們在 'adapters/JavaAdapter/tests' 上有 Script 用來測試配接器端點。

- 將**輸入**設為與建置配接器配置相同的 *GitHub repository*。

- 在**工作**標籤中新增 *deploy job*。將**部署者類型**選取為 *Cloud Foundry*。我們使用部署工作而非測試工作，是因為部署可以輕鬆地與 Cloud Foundry 整合。

請使用下列 Script 來測試配接器。

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

- 在環境內容中新增 *INSTANCE_NAME*，這是我們在前一個階段所新增的相同值。

您可以使用 API 測試架構來測試配接器。在此範例中，我們有 Shell Script 可用來呼叫配接器，並測試其正確性。


#### 階段 5 - 使用 Fastlane 建置應用程式
{: #stage5-building-apps-with-fastlane}

此階段的輸入應該是我們在先前階段中使用的 GitHub 儲存庫。在前一個階段（測試配接器）通過之後，必須觸發此階段。

為了建置應用程式，我們會在**工作**標籤中使用部署工作範本。請使用 *Cloud Foundry* 作為**部署者類型**。

請使用下列 Script 來建置應用程式：

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

在上述 Script 中，我們使用 `mfpdev-cli` 以將應用程式登錄到 {{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) 來建置和發佈應用程式。

用於 Script 的環境變數定義在下一個**環境內容**標籤中。

在環境內容中新增下列內容：

- 將 *INSTANCE_NAME* 設為 **MFP 實例名稱**
- 將 *appPublishUrl* 設為應用程式必須發佈而讓測試階段能夠將其取回的**儲存庫**。
- 將 *gitPushUser* 設為 **GitHub 使用者名稱**
- 將 *gitPushEmail* 設為 **GitHub 使用者的電子郵件**
- 將 *gitPushToken* 設為 **Git 推送記號**
- 將 *apkGitPushUrl* 設為 **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    例如：`https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### 階段 6 - 使用 Bitbar 來測試應用程式
{: #stage6-test-the-app-using-bitbar}

此階段也是開放式階段。您可以根據需求來使用任何測試架構。在此範例中，我們會使用 [Bitbar](https://bitbar.com/testing/)。

- 新增與您所擁有測試專案（此專案包含用來支援測試的所有測試 Script 及構件）相關的 GitHub 儲存庫詳細資料。

- 針對 Bitbar，在**工作**標籤下新增建置工作，並將**建置器類型**選取為 *Maven*。

使用類似下列 Script 的建置 Script：

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

對於上述 Script，您需要一些環境變數。

- 將 *screenshot\_dir* 設為 **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- 將 *applicationPath* 設為您打算要測試之應用程式的 **GitHub 路徑**
- 將 *executionType* 設為**執行類型**
- 將 *test* 設為您想要執行的**測試**
- 將 *bitbarApiKey* 設為 **Bitbar API 金鑰**
- 將 *testdroid_project* 設為**專案**。


#### 階段 7 - 關閉 {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

在此階段中，我們將關閉第一個階段所建立，以便在先前階段中進行建置、部署及測試的 {{ site.data.keyword.mobilefoundation_short }} 實例。

此階段不會有任何輸入。

- 在**工作**標籤中建立 *Deploy job*。
- 使用下列 Script：

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

- 將**環境內容**中的 *INSTANCE_NAME* 設為建立 {{ site.data.keyword.mobilefoundation_short }} 實例時在第一個階段中設定的值。
