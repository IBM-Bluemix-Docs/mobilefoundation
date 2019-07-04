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


# 使用 {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }} 交付 {{ site.data.keyword.mobilefoundation_short }} 应用程序
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

本教程帮助您通过使用 {{ site.data.keyword.cloud_notm }} 上的 {{ site.data.keyword.jazzhub_title }}，自动向 IBM {{ site.data.keyword.mobilefoundation_short }} 交付应用程序和适配器。

下图提供了管道的概述。

![overview_of_pipeline](images/p00_overview_of_pipeline.png "DevOps 管道的六个阶段")


## 先决条件
{: #mpfapps-devops-prerequisites }

* [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration) 帐户
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* 样本应用程序和 [MFP 适配器](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* [GitHub](http://github.com/) 帐户
* *可选：* [Bitbar](https://bitbar.com/testing/) 实例和 Bitbar API 密钥（您可以根据需求使用任何服务）。


## 创建 Continuous Delivery 服务和工具链
{: #creating-continuous-delivery-service-and-toolchain }

* 在 {{ site.data.keyword.cloud_notm }} 目录中搜索“Continuous Delivery”（或者[单击此处](https://cloud.ibm.com/catalog/services/continuous-delivery)）。
* 通过提供服务名称和区域等来创建服务。

    在以下示例中，对于服务名称使用 *MFP App/Adapter delivery Test*，对于区域/位置使用*伦敦*，对于资源组使用*缺省值*。

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Mobile Foundation 服务实例的目录创建页面")

* 从导航菜单中，选择 **DevOps**，然后单击**创建工具链**，并搜索“构建您自己的工具链”以从头开始创建工具链。

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "创建您自己的工具链页面，其中显示了“构建您自己的工具链”的搜索结果")

* 提供要配置的工具链名称和区域等。


## 将 GitHub 与工具链相集成以用于版本控制和管道触发器
{: #integrating-github-with-the-toolchain}

* 从导航的**概述**页面中，单击**添加工具**并搜索 GitHub。
* 配置 GitHub 工具的 **GitHub 服务器地址**、**存储库类型**和**存储库 URL**。
* 您可以创建新存储库，派生、克隆或使用现有存储库。

    在以下示例中，对于 GitHub 服务器使用“[https://github.com](http://github.com/)”，对于存储库类型使用*现有*，对于存储库 URL 使用 *https://github.com/sagar20896/mfp-devops-20181210030116092*。

    ![configuring_toolchain](images/p03_configuring_toolchain.png "“配置集成”屏幕，显示了“GitHub 服务器”、“存储库类型”和“存储库 URL”字段")

### 将 Delivery Pipeline 添加到工具链
{: #adding-the-delivery-pipeline-to-the-toolchain}

使用**添加工具**并搜索 *Delivery Pipeline*。配置管道并单击**创建集成**。

### 向管道添加阶段
{: #adding-stages-to-the-pipeline}

#### 阶段 1 - 设置 {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

在此阶段中，我们将启动 {{ site.data.keyword.mobilefoundation_short }} 的实例。将 GitHub 添加到管道处于此阶段中，每次将更改推送到 Git 存储库时将触发管道。以下步骤显示 GitHub 配置。您可以根据需求设置阶段触发器。可以是手动的，也可以是自动的。

在以下示例中，我们将“输入类型”设置为 *Git 存储库*，将 Git 存储库设置为 *mfp-devops-20181210030116092*，将 Git URL 设置为 *https://github.com/sagar20896/mfp-devops-20181210030116092*，并将分支设置为 *master*。

![first_stage_git_input](images/p4_first_stage_git_input.png "“设置 Mobile Foundation”屏幕，其中选择了“输入”选项卡")

- 单击**添加阶段**并配置**输入**选项卡以指向 GitHub 存储库，如图中所示。
- 在**作业**选项卡中，单击**添加作业**，然后选择*部署*作为作业类型。对于**部署程序类型**选择 *Cloud Foundry*。
- 如果您没有 API 密钥，那么可以在[此处](https://cloud.ibm.com/iam/#/apikeys)为您的 {{ site.data.keyword.cloud_notm }} 帐户创建一个密钥。

根据需要选择/填写其他字段。在**部署脚本**中添加以下行：

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

在上述脚本中，我们使用 Cloud Foundry CLI 来创建 {{ site.data.keyword.mobilefoundation_short }} 服务实例。

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "“设置 Mobile Foundation”屏幕，其中选择了“作业”选项卡")

在**环境属性**选项卡中，添加属性 *INSTANCE\_NAME*（作为文本属性）作为想要的 MobileFoundation 实例名称。它将在多个阶段中用作标识。

![stage1_environment_properties](images/p06_stage1_environment_properties.png "“设置 Mobile Foundation”屏幕，其中选择了“环境属性”选项卡")

#### 阶段 2 - 构建适配器
{: #stage2-building-an-adapter}

在此阶段中，我们将拉取适配器源代码并进行构建。

- 单击**添加阶段**并根据添加 GitHub 存储库作为输入的首选项配置阶段。**阶段触发器**必须设置为*在前一个阶段完成时运行作业*。

- 切换到**作业**选项卡并添加构建作业。对于构建器类型选择 *Maven*，并在构建脚本中添加以下行。

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

在上述脚本中，我们安装 [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) 以在存储库的 `adapters/JavaAdapter` 中使用适配器命令构建适配器。

在以下示例中，对于**构建器类型**使用 *npm*，并使用构建脚本中提供的脚本。将**工作目录**和**构建归档目录**参数保留为空。

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "“构建适配器”屏幕，其中选择了“作业”选项卡")

#### 阶段 3 - 部署适配器
{: #stage3-deploying-an-adapter}

在此阶段中，我们将适配器部署到在第一个阶段中创建的 {{ site.data.keyword.mobilefoundation_short }} 实例。

- 添加阶段。
- 配置输入。
- 将输入类型设置为**构建工件**，将**阶段**设置为构建适配器所在的阶段的名称，并设置在构建适配器阶段中构建适配器的作业。
- **阶段触发器**必须设置为*在前一个阶段完成时运行作业*。
- 从**作业**选项卡添加部署作业。
- 对于**部署程序类型**选择 *Cloud Foundry*，并根据首选项配置其余项。

在以下示例中，对于**部署程序类型**使用 *Cloud Foundry*，对于 **{{ site.data.keyword.cloud_notm }} 区域**使用*达拉斯*，对于 API 密钥使用在第一阶段中创建的密钥。

![deploy_adapter](images/p08_deploy_adapter.png "“部署”屏幕，其中选择了“作业”选项卡")


使用以下**部署脚本**：

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

脚本使用 mfpdev-cli 的适配器部署命令以将适配器部署到 MFP 实例。

- 在**环境属性**选项卡中设置 *INSTANCE_NAME*，值与创建 {{ site.data.keyword.mobilefoundation_short }} 实例时在第一个阶段中设置的值相同。我们使用相同实例名称，以便将适配器部署到在第一个阶段中创建的实例。


#### 阶段 4 - 测试适配器
{: #stage4-test-adapter}

在此阶段中，我们打算测试在先前阶段中构建和部署的适配器。在样本适配器存储库中，我们在“adapters/JavaAdapter/tests”中包含用于测试适配器端点的脚本。

- 将**输入**设置为与构建适配器的配置相同的 *GitHub 存储库*。

- 在**作业**选项卡中添加*部署作业*。对于**部署程序类型**选择 *Cloud Foundry*。我们使用部署作业而不是测试作业，因为部署具有选项可轻松与 Cloud Foundry 集成。

使用以下脚本来测试适配器。

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

- 在环境属性中添加 *INSTANCE_NAME*，这些值与先前阶段中添加的值相同。

您可以使用 API 测试框架来测试适配器。在此示例中，我们使用 shell 脚本来调用适配器并测试正确性。


#### 阶段 5 - 使用 Fastlane 构建应用程序
{: #stage5-building-apps-with-fastlane}

此阶段的输入应该是在先前阶段中使用的 GitHub 存储库。此阶段必须在上一阶段（测试适配器）通过后触发。

为构建应用程序，我们在**作业**选项卡中使用部署作业模板。使用 *Cloud Foundry* 作为**部署程序类型**。

使用以下脚本来构建应用程序：

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

在上述脚本中，我们使用 `mfpdev-cli` 以将应用程序注册到 {{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) 来构建和发布应用程序。

在下一个**环境属性**选项卡中定义在脚本中使用的环境变量。

在环境属性中，添加以下属性：

- *INSTANCE_NAME* 为 **mfp instance name**
- *appPublishUrl* 为**存储库**，需要在其中发布应用程序，以便测试阶段可拉取该应用程序。
- *gitPushUser* 为 **GitHub 用户名**
- *gitPushEmail* 为 **GitHub 用户的电子邮件**
- *gitPushToken* 为 **Git 推送令牌**
- *apkGitPushUrl* 为 **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `例如：https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### 阶段 6 - 使用 Bitbar 测试应用程序
{: #stage6-test-the-app-using-bitbar}

此阶段也是开放式的。您可以根据需求使用任何测试框架。在此示例中，我们使用 [Bitbar](https://bitbar.com/testing/)。

- 添加有关您拥有的测试项目的 GitHub 存储库详细信息（此项目将由用于支持测试的所有测试脚本和工件组成）。

- 对于 Bitbar，在**作业**选项卡下添加构建作业，然后对于**构建器类型**选择 *Maven*。

使用类似于以下脚本的构建脚本：

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

对于上述脚本，您需要一些环境变量。

- *screenshot\_dir* 为 **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* 为您打算测试的应用程序的 **GitHub 路径**
- *executionType* 为**执行类型**
- *test* 为您想要执行的**测试**
- *bitbarApiKey* 为 **Bitbar API 密钥**
- *testdroid_project* 为**项目**。


#### 阶段 7 - 除去 {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

在前面的阶段使用了在第一个阶段中创建的 {{ site.data.keyword.mobilefoundation_short }} 实例进行构建、部署和测试。在此阶段中，我们将除去该实例。

此阶段不会有任何输入。

- 在**作业**选项卡中创建*部署作业*。
- 使用以下脚本：

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

- 在**环境属性**选项卡中将 *INSTANCE_NAME* 设置为创建 {{ site.data.keyword.mobilefoundation_short }} 实例时在第一个阶段中设置的值。
