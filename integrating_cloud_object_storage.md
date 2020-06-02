---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
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

# Using {{site.data.keyword.cos_full_notm}} with {{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}}
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} (MF) delivers enterprise-grade capabilities that are uniquely designed to support building and deploying of the next generation of cognitive, contextual, and personalized mobile apps. {{site.data.keyword.cos_full_notm}} (COS) is a flexible, cost-effective, and scalable cloud storage for unstructured data. This how-to guide explains how a mobile application that uses {{site.data.keyword.mobilefoundation_short}} can connect and fetch or upload data to {{site.data.keyword.cos_full_notm}} through an ionic application. The ionic application, adapter, and related files for this how-to tutorial are available [here](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App){: external}.
{: shortdesc}

## Prerequisites
{: #cos-prerequisites}

1. Install the [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) by running `npm install -g mfpdev-cli`. This cli is used to register the ionic app and deploy the adapter to the MF server. Alternatively these activities can be performed from the MF server dashboard.
1. Install [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli) on your machine.
1. Install ionic cli by running `npm install -g ionic`
1. Install cordova by running `npm install -g cordova`

## {{site.data.keyword.mobilefoundation_short}} Server setup
{: #mobile-foundation-server-setup}

The {{site.data.keyword.mobilefoundation_short}} server is set on {{site.data.keyword.cloud_notm}}. Set up an {{site.data.keyword.cloud_notm}} instance of the {{site.data.keyword.mobilefoundation_short}} server as follows:

* In the {{site.data.keyword.cloud_notm}} catalog, search for *{{site.data.keyword.mobilefoundation_short}}*. Click the **{{site.data.keyword.mobilefoundation_short}}** tile.

   ![MFPCatalog](images/mfp_catalog.png)

* Provide a suitable name for the {{site.data.keyword.mobilefoundation_short}} server instance and click **Create**.

   ![BMMFPNew](images/bmmfpnew.png)

* A new MF server instance gets created and asks for login credentials.

   ![MFPLogin](images/mfp_login.png)

* The credentials to log in to the MF server can be found in the **Credentials** tab in the menu.

   ![MFPcredentials](images/mfp_credentials.png)

* Provide these credentials and login to enter the MF dashboard.

   ![MFPDashboard](images/mfp_dashboard.png)

## Cloud Object Storage Setup
{: #cloud-object-storage-setup}

* In the {{site.data.keyword.cloud_notm}} catalog, search for *Cloud Object Storage*. Click the **Object Storage** tile.

   ![Catalog](images/catalog.png)

* Provide a name to your COS instance and click **Create**.

   ![Create COS](images/cos_create.png)

* Next, click **Buckets** in the menu options. Provide a suitable name (in this sample, `sharedgallery` represents the bucket name) for your bucket and click **Create**.

   ![Create Bucket](images/bucketcreate.png)

* When the bucket is created, the landing page of the bucket looks like the following image, providing you options to upload data directly.

   ![Bucket Landing Page](images/bucketlanding.png)

* Here, add the three files that are provided in the **Short stories** folder in the [sample](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).

## MFP-COS Ionic App and Java&trade; adapter
{: #mfp-cos-ionic-app-and-java-adapter}

Download this [Git repository](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App){: external} or clone it. This repo consists of two main components:

1. An MF Java&trade; adapter
1. An ionic mobile application

### Configuring mfpdev-cli
{: #configuring-mfpdev-cli}

Add the server details to the cli. On the command prompt, run: `mfpdev server add`.

```
? Enter the name of the new server profile:
```
{: screeen}

Provide a name for the server and press enter. In this sample, the name that is provided is `mfpserver`.

```
? Enter the fully qualified URL of this server:
```
{: screeen}

Enter the url of the server. For MF server on {{site.data.keyword.cloud_notm}}, the service credentials tab contains the url. The port for the https MF server on {{site.data.keyword.cloud_notm}} is 443 and for the http MF server instance is 80.

```
? Enter the MobileFirst Server administrator login ID: (admin)
```
{: screeen}

Enter `admin` and press **Enter**.

```
? Enter the MobileFirst Server administrator password:
```
{: screeen}

Enter the password available in the service credentials.

```
? Save the administrator password for this server?: (Y/n)
```
{: screeen}

Based on your preference, enter *Y/N* and enter details as asked by the prompts.

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```
{: screeen}

Set 30 sec as the default timeout

```
? Make this server the default?: (Y/n)
```
{: screeen}

Enter *Y* and press **Enter**.

***Expected output***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```
{: screeen}

### Configuring the MF Java&trade; adapter
{: #configuring-the-mf-java-adapter}

To connect to your COS instance, some details of your COS instance need to be provided in the `adapter.xml` file. Furnish values for the following fields:

1. **endpointURL**: This field is the public endpoint url for your COS object. This URL can be found on your COS's dashboard, under **Buckets (on the menu options) -> <your-bucket-name> (`sharedgallery` in this sample) -> Configuration -> Endpoints -> Public**
1. **AuthToken**: In this tutorial, IAM authentication is used.

For the Java&trade; adapter to connect to your instance of COS, authentication that uses IAM or HMAC is needed. Following are the steps to get the IAM token. For further details on IAM and HMAC authentication processes, click [here](https://cloud.ibm.com/docs/cloud-object-storage/api-reference?topic=cloud-object-storage-compatibility-api-bucket-operations#bucket-operations#AuthenticationOptions).

#### Obtaining IAM OAuth token by using {{site.data.keyword.cloud_notm}} CLI
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. First, make sure that you have an API key. Get the API key from [{{site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users).
1. Log in to the {{site.data.keyword.cloud_notm}} Platform by using the CLI.

   ```bash
   ibmcloud login --apikey <value>
   ```
   {: codeblock}

   Your output is similar to the following snippet,

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
   {: screeen}

1. To get all the service instances on your {{site.data.keyword.cloud_notm}} account, run the following command on the CLI.

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
   {: screeen}

1. Run the following command to get the details of your COS instance. `<instance-name>` is the name of your COS service (`newObject` for this tutorial).

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
   {: screeen}

1. To get IAM token, run the following command:

   ```bash
   ibmcloud iam oauth-tokens
   ```
   {: codeblock}

   Expected output:

   ```text
   IAM token:  Bearer <token>
   UAA token:  Bearer <refresh-token>
   ```
   {: screeen}

After you add the *endpointURL* and the *authToken* values, build the adapter. Go to the adapter's root folder in the command prompt and run.

```bash
mfpdev adapter build
```
{: codeblock}

The `*.adapter` file is created in the `target` folder. Run the following command:

```bash
mfpdev adapter deploy
```
{: codeblock}

The adapter is deployed to the MF instance.

Alternatively, the adapter can be deployed on the MF server dashboard. Open the MF server dashboard, on the menu, click **Adapters -> New** to open the page as shown in the following image.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

Then, click **Deploy Adapter** and upload the `.adapter` file from the **target** folder.

### Configuring the Ionic App
{: #configuring-the-ionic-app}

In the app, perform the following steps:

1. Go to the folder that contains the Ionic application.
1. Add the cordova MF plug-in

   ```bash
   ionic cordova plugin add cordova-plugin-mfp
   ```

1. Add the android or iOS platform

   ```bash
   ionic cordova platform add android
   ```

   Or

   ```bash
   ionic cordova platform add ios
   ```

1. Register your app to the MF server by running

   ```bash
   mfpdev app register
   ```

   Alternatively, the app can be registered on the MF server dashboard. Open the MF server dashboard and on the menu click **Applications -> New**.

   The page as shown in the following image gets loaded.

   ![MFPNewAppRegister](images/mfp_new_app_register.png)

   Enter the details requested. Give a name for your application in the textbox 'Application Name'. Choose the required platform.

   For Android, the **Package** textbox accepts the *Application identifier*. This parameter can be found in the `AndroidManifest.xml` as package of your Android application. The **Version** textbox field must be filled with the *versionName* value from the `AndroidManifest.xml`.

   For iOS, the **Bundle ID** textbox accepts *Application identifier* (case-sensitive). This parameter can be found in the `mfpclient.plist` of your iOS application. The **Version** textbox field must be filled with the *version* value from the `mfpclient.plist` file in your iOS application.

1. Run `ionic cordova prepare` for changes to percolate to the environments added.
1. Run

   ```bash
   ionic cordova build android
   ```

   Or

   ```bash
   ionic cordova build ios
   ```
   to ensure that the typescript changes are added to the environments.

1. Attach device or run an emulator or simulator and run the following command.

   ```bash
   ionic cordova run android
   ```

   Or

   ```bash
   ionic cordova run ios
   ```

### Navigating the Ionic application
{: #navigating-the-ionic-application}

The Ionic application displays a list of short stories that were uploaded previously (as the last step in the section [Cloud Object Storage Setup](#cloud-object-storage-setup)), from your created COS instance. On selecting a particular story option, the selected story is loaded and displayed. An option to add a story is also provided which is then uploaded to the COS instance.

The initial COS objects list looks like the following image.

![COS before](images/cos_before.png)

The home page of the application provides an option to either *Get all stories* or *Add story*

![App home screen](images/app-home-screen.png)

On clicking **Get all stories**, the stories available on COS are displayed.

![Apps select story](images/app-select-story.png)

Select a story to be loaded from the drop-down menu and click **Load**

![App story loaded](images/app-story-loaded.png)

Next, click **Add story** to add a story of your own. Enter a title for the story and the story's content in the text area and click **Add**.

![App adds input](images/app-add-input.png)

When the story is added, a message of successful addition of the story is displayed.

![App story added](images/app-story-added.png)

The COS dashboard now has the story is added from the application as well.

![COS added](images/cos_added.png)

The IAM OAuth token that is obtained by using `ibmcloud iam oauth-tokens` has an expiration time and the adapter fails with an exception of `403 - Forbidden`. It must be hence ensured that the token is valid on the deployed adapter for the app to function as expected.
{: note}
