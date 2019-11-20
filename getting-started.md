---

copyright:
  years: 2016, 2019
lastupdated: "2019-11-15"

keywords: getting started, mobile foundation, plans, configure mobile foundation server, sample app, setup

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

# Getting started tutorial
{: #getting-started}

{{site.data.keyword.mobilefoundation_long}} expedites the setting up of an {{site.data.keyword.mfp_full}} environment using which you can develop, test, and run enterprise mobile apps. {{site.data.keyword.mobilefoundation_short}} offers the following different service plans:
* **Developer**: Provisions an instance of Foundation Server in user's account. Permits any number of applications with the total number of connected devices across all applications limited to 10, free of charge and to be used for development and testing purposes only.
* **Professional Per Device**: Provisions an instance of Foundation Server in user's account and charged by the number of actively connected devices.
* **Professional 1 Application**: Provisions an instance of Foundation Server in user's account and permits any number of users and devices to be actively connected for a single application only. 
{: shortdesc}

You can review all the available plans [here](https://cloud.ibm.com/catalog/services/mobile-foundation).
{: note}

Create a {{site.data.keyword.mobilefoundation_short}} service instance that uses one of the supported plans by following this getting started tutorial. You can then register an application. Download and edit the registered application, deploy an adapter, and finally test the application.

## Before you begin
{: #prereqs-gs}

You need an {{site.data.keyword.cloud}} account to create an instance of the {{site.data.keyword.mobilefoundation_short}} service.

## Step 1: Create an instance of {{site.data.keyword.mobilefoundation_short}} service
{: #step1create}

1. In the {{site.data.keyword.cloud_notm}} **catalog**, select [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation). The service configuration screen opens.
1. Give your service instance a name, or use the preset name.
1. Choose the region, organization, and space where you would want to create the service instance.
1. Select your **Pricing Plan** and click **Create**.

## Step 2: Build your mobile channel
{: #buildmobilechannel}

### For {{site.data.keyword.mobilefoundation_short}}: Developer plan
{: #buildchanneldevplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}} service with Developer plan, you can start building your mobile channel.

* You can instantly access and work with the Mobile Foundation Server.

  This selection creates an {{site.data.keyword.mfserver_long_notm}} with the following settings:
  * 1 GB of memory. This size is enough for development and light testing activities.
  * To access the Mobile Foundation Server by using CLI you need the credentials, which are available when you click **Service credentials** from the navigation pane of the {{site.data.keyword.cloud_notm}} console.

The Developer plan does not offer a persistent database, as such be sure to backup your configuration as explained in the [FAQ section](/docs/services/mobilefoundation?topic=mobilefoundation-mfp-faq#persistentdatabase).
{: important}

{{site.data.keyword.mobilefoundation_short}} Server might be stopped by default. It is important to make sure that the server is running at the time of application testing.
{: note}

### For {{site.data.keyword.mobilefoundation_short}}: Professional Per Device plan
{: #buildchannelprofdeviceplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}} Professional Per Device service, you can start building your mobile channel by completing the following steps.

1. Specify the **Database Settings**.

   You can create or connect to an existing **{{site.data.keyword.Db2_on_Cloud_short}}** or connect to an existing **{{site.data.keyword.composeForPostgreSQL}}** instance or connect to an existing **Databases for {{site.data.keyword.postgresql}}** service instance on {{site.data.keyword.cloud_notm}}.
   {: note}

   * For **{{site.data.keyword.Db2_on_Cloud_short}}** database service:

      + Select the {{site.data.keyword.cloud_notm}} `Organization`, `Space`, `Service Name`, and `Credentials`. where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exits.
      + Test the connection to the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance by clicking **Test Connection**.
      + Click **Connect**.
      + Click **Save** on the pop-up window that asks for confirmation on the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} service instance.

      Make sure you have the permissions to access the `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} instance exits. After you add a {{site.data.keyword.Db2_on_Cloud_short}} instance, you will not be able to change it.
      {: note}

   * For **{{site.data.keyword.composeForPostgreSQL}}** database service:

      You cannot create a new {{site.data.keyword.composeForPostgreSQL}} database now, but if you have already created one, then you can connect to that {{site.data.keyword.composeForPostgreSQL}} database instance. Use Databases for {{site.data.keyword.postgresql}} service instance going forward. Also, create a Mobile Foundation instance at the Organization level for providing access to others in your Organization.
      {: note}

      + Select the {{site.data.keyword.cloud_notm}} `Organization`, `Space`, `Service Name`, and `Credentials`. where the {{site.data.keyword.composeForPostgreSQL}} service instance exits.
      + Test the connection to the selected {{site.data.keyword.composeForPostgreSQL}} service instance by clicking **Test Connection**.
      + Click **Connect**.
      + Click **Save** on the pop-up window that asks for confirmation on the selected {{site.data.keyword.composeForPostgreSQL}} service instance. This action creates the required tables in the configured {{site.data.keyword.composeForPostgreSQL}} service instance.

      Make sure you have the permissions to access the `Space` where the {{site.data.keyword.composeForPostgreSQL}} instance exits. After you add a {{site.data.keyword.composeForPostgreSQL}} instance, you'll not be able to change it.
      {: note}

   * For **Databases for {{site.data.keyword.postgresql}}** database service:

      + Select the `Resource Group`, `Service Name`, and `Credentials`. where the Databases for {{site.data.keyword.postgresql}} service instance exits.
      + Test the connection to the selected Databases for {{site.data.keyword.postgresql}} service instance by clicking **Test Connection**.
      + Click **Connect**.
      + Click **Save** on the pop-up window that asks for confirmation on the selected Databases for {{site.data.keyword.postgresql}} service instance. This action creates the required tables in the configured Databases for {{site.data.keyword.postgresql}} service instance.
        
      Make sure you have the permissions to access the `Resource Group` where the Databases for {{site.data.keyword.postgresql}} instance exits. After you add a Databases for {{site.data.keyword.postgresql}} instance, you'll not be able to change it.
      {: note}

1. Create and start the server.

   1. Create a {{site.data.keyword.mobilefoundation_short}} server instance with the default configuration, click **Configure Server**. This will display the **Manage** tab.

      + This selection provisions an {{site.data.keyword.mfserver_long_notm}} with the following settings:
         - Two nodes with 1 GB memory each. This size is good for development, moderate testing activities, and small scale production workloads.
         - Enter the `Admin console password`. 
         - `Re-enter Admin console password` to confirm. You can use this password to login to the admin console when the server is up and running.
         - Optionally, you can add license keys like `LTPA keys`, `Keystore` and `Truststore`.
         - Click **Create Advanced Server**. 

         The process of creating your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
         - The status of your server that is running (state, size).

      + Click **Launch Operation console** to open the {{site.data.keyword.mfp_oc_short_notm}}.      

      To create a {{site.data.keyword.mobilefirst_notm}} server instance with advanced configuration for topology, security, and other server configuration, click **Start Server with Advanced Configuration**. For more information, see [Setting up advanced configuration](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#using_mfs_advanced_p5).
      {: tip}

### For {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application plan
{: #buildchannelprof1appplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application service, you can start building your mobile channel by completing the following steps.

1. Specify the **Database Settings**.

   You can create or connect to an existing **{{site.data.keyword.Db2_on_Cloud_short}}** or connect to an existing **{{site.data.keyword.composeForPostgreSQL}}** instance or connect to an existing **Databases for {{site.data.keyword.postgresql}}** service instance on {{site.data.keyword.cloud_notm}}.
   {: note}

   * For **{{site.data.keyword.Db2_on_Cloud_short}}** database service:

      + Select the {{site.data.keyword.cloud_notm}} `Organization`, `Space`, `Service Name`, and `Credentials`. where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exits.
      + Test the connection to the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance by clicking **Test Connection**.
      + Click **Connect**.
      + Click **Save** on the pop-up window that asks for confirmation on the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} service instance.

      Make sure you have the permissions to access the `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} instance exits. After you add a {{site.data.keyword.Db2_on_Cloud_short}} instance, you'll not be able to change it.
      {: note}

   * For **{{site.data.keyword.composeForPostgreSQL}}** database service:

      You cannot create a new {{site.data.keyword.composeForPostgreSQL}} database now, but if you have already created one, then you can connect to that {{site.data.keyword.composeForPostgreSQL}} database instance. Use Databases for {{site.data.keyword.postgresql}} service instance going forward. Also, create a Mobile Foundation instance at the Organization level for providing access to others in your Organization.
      {: note}

      + Select the {{site.data.keyword.cloud_notm}} `Organization`, `Space`, `Service Name`, and `Credentials`. where the {{site.data.keyword.composeForPostgreSQL}} service instance exits.
      + Test the connection to the selected {{site.data.keyword.composeForPostgreSQL}} service instance by clicking **Test Connection**.
      + Click **Connect**.
      + Click **Save** on the pop-up window that asks for confirmation on the selected {{site.data.keyword.composeForPostgreSQL}} service instance. This action creates the required tables in the configured {{site.data.keyword.composeForPostgreSQL}} service instance.

      Make sure you have the permissions to access the `Space` where the {{site.data.keyword.composeForPostgreSQL}} instance exits. After you add a {{site.data.keyword.composeForPostgreSQL}} instance, you'll not be able to change it.
      {: note}

   * For **Databases for {{site.data.keyword.postgresql}}** database service:

      + Select the `Resource Group`, `Service Name`, and `Credentials`. where the Databases for {{site.data.keyword.postgresql}} service instance exits.
      + Test the connection to the selected Databases for {{site.data.keyword.postgresql}} service instance by clicking **Test Connection**.
      + Click **Connect**.
      + Click **Save** on the pop-up window that asks for confirmation on the selected Databases for {{site.data.keyword.postgresql}} service instance. This action creates the required tables in the configured Databases for {{site.data.keyword.postgresql}} service instance.

      Make sure you have the permissions to access the `Resource Group` where the Databases for {{site.data.keyword.postgresql}} instance exits. After you add a Databases for {{site.data.keyword.postgresql}} instance, you'll not be able to change it.
      {: note}

1. Create and start the server.

   1. Create a {{site.data.keyword.mobilefoundation_short}} server instance with the default configuration, click **Configure Server**. This will display the **Manage** tab.

      + This selection provisions an {{site.data.keyword.mfserver_long_notm}} with the following settings:
         - Two nodes with 1 GB memory each. This size is good for development, moderate testing activities, and small scale production workloads.
         - Enter the `Admin console password`. 
         - `Re-enter Admin console password` to confirm. You can use this password to login to the admin console when the server is up and running.
         - Optionally, you can add license keys like `LTPA keys`, `Keystore` and `Truststore`.
         - Click **Create Advanced Server**. 

         The process of creating your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
         - The status of your server that is running (state, size).

      + Click **Launch Operation console** to open the {{site.data.keyword.mfp_oc_short_notm}}.      

      To create a {{site.data.keyword.mobilefirst_notm}} server instance with advanced configuration for topology, security, and other server configuration, click **Start Server with Advanced Configuration**. For more information, see [Setting up advanced configuration](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#using_mfs_advanced_p2).
      {: tip}

Go to [Using the Mobile Foundation service to set up Mobile Foundation Server](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/){: external} to learn more about getting started with {{site.data.keyword.mobilefoundation_short}}.
{: note}

## Step 3: Register your application in {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

After you create and start your {{site.data.keyword.mfserver_short_notm}} instance, you can carry out the following steps to register an Android application.

1. Invoke the {{site.data.keyword.mfp_oc_short_notm}} by loading the URL: `http://<your-server-host>:<server-port>/mfpconsole`. Use the `username` and `password` generated at the time of provisioning.

   + In the  {{site.data.keyword.mfp_oc_short_notm}} **Dashboard**, click **New** next to **Applications**.
   + Provide *MFPStarterAndroid* as the **Application Name**.
   + **Choose Platform** to be *Android*.
   + Provide *com.ibm.mfpstarterandroid* as the **Application Identifier**.
   + Enter *1.0* as the **Version**.
   + Click **Register application**.

## Step 4: Download the sample Android application
{: #downloadapp}

1. From the {{site.data.keyword.mfp_oc_short_notm}} **Dashboard**, select **MFPStarterAndroid** under **Applications**.
1. Click **Get Starter Code** and select to download the Android application sample.

## Step 5: Edit the sample application
{: #editapp}

1. Import the downloaded sample Android app, from the earlier step, to an Android Studio.
1. From the **Project** sidebar menu in Android Studio, select the **app → src → main → java → com → ibm → mfpstarterandroid → ServerConnectActivity.java** file.
   * Add the following imports

      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

   * Paste the following code snippet, replacing the call to `WLAuthorizationManager.getInstance().obtainAccessToken`

      ```java
      WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
          @Override
          public void onSuccess(AccessToken token) {
              System.out.println("Received the following access token value: " + token);
              runOnUiThread(new Runnable() {
                  @Override
                  public void run() {
                      titleLabel.setText("Yay!");
                      connectionStatusLabel.setText("Connected to MobileFirst Server");
                  }
              });

              URI adapterPath = null;
              try {
                  adapterPath = new URI("/adapters/javaAdapter/resource/greet");
              } catch (URISyntaxException e) {
                  e.printStackTrace();
              }

              WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

              request.setQueryParameter("name","world");
              request.send(new WLResponseListener() {
                  @Override
                  public void onSuccess(WLResponse wlResponse) {
                      // Will print "Hello world" in LogCat.
                      Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                  }

                  @Override
                  public void onFailure(WLFailResponse wlFailResponse) {
                      Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                  }
              });
          }

          @Override
          public void onFailure(WLFailResponse wlFailResponse) {
              System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
              runOnUiThread(new Runnable() {
                  @Override
                  public void run() {
                      titleLabel.setText("Bummer...");
                      connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                  }
              });
          }
      });
      ```
      {: codeblock}  

## Step 6: Deploy an adapter
{: #deployadapter}

Adapters contains server-side code snippets and configuration. After making modifications or configuration changes, you build and deploy the artifacts on the {{site.data.keyword.mobilefoundation_short}} server.

Download this [adapter artifact](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} and deploy it from {{site.data.keyword.mfp_oc_short_notm}} by using **Actions → Deploy adapter**.

## Step 7: Test the application
{: #testapp}

1. In Android Studio, from the **Project** sidebar menu, select the **app → src → main →assets → mfpclient.properties** file and edit the `protocol`, `host` and `port` properties with the correct values for your MobileFirst Server.

   The values are typically https, *your-server-address* and 443.
   {: tip}

1. Click **Run App** in Android Studio.
   * You'll see that the app started on a device emulator.
   * Click **Ping MobileFirst Server** in your application, this displays `Connected to MobileFirst Server`.
   * If the application was able to connect to the MobileFirst Server, the Java adapter that is deployed will make a resource request call.
   * The adapter response is then printed in Android Studio’s LogCat view.

## Next steps
{: #nextsteps-gs}

You can follow the [Quick Start tutorials](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: external} to work with more sample applications and to explore the working of {{site.data.keyword.mobilefoundation_short}}.

Quick Start has tutorials that explains the working of {{site.data.keyword.mobilefoundation_short}} for iOS, Android, Web, Cordova, Windows, React Native, Ionic, and Xamarin apps.
