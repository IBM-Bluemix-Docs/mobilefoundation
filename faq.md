---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

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
{:faq: data-hd-content-type='faq'}

# FAQs
{: #mfp-faq}

This FAQ provides answers to common questions about the {{site.data.keyword.mobilefoundation_long}} service.
{: shortdesc}

## Deleting or modifying {{site.data.keyword.mobilefoundation_short}} server application can cause {{site.data.keyword.mobilefoundation_short}} server downtime
{: #cfapp-downtime}
{: faq}

The {{site.data.keyword.mobilefoundation_short}} service creates the {{site.data.keyword.mobilefoundation_short}} server as a Liberty Buildpack Cloud Foundary application. This Application will be listed in your {{site.data.keyword.cloud_notm}} console in the Cloud Foundry Application section. It will be with the `<name of instance>-Server`.

This application is created and managed by {{site.data.keyword.mobilefoundation_short}}. Do not delete or modify this Cloud Foundry Application in any way, as it may affect the normal working of the {{site.data.keyword.mobilefoundation_short}} server.

## How do I know when updates to {{site.data.keyword.mobilefoundation_short}} service is made available?
{: #maintupdates_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} creates a {{site.data.keyword.mfserver_short_notm}}. The updates to the {{site.data.keyword.mobilefoundation_short}} server are notified to the users. A notification is shown in the service instance dashboard. The user can choose to apply the update to {{site.data.keyword.mobilefoundation_short}} during a maintenance window that is decided by the user. You can choose to update the {{site.data.keyword.mobilefoundation_short}} server when it's convenient for you.

{{site.data.keyword.mobilefoundation_short}} service update is made available when one of the following components is updated.

* {{site.data.keyword.mfserver_short_notm}}.
* Underlying Liberty version.
* Underlying Java&trade; Developer Kit version.

## How do I apply the updates to {{site.data.keyword.mobilefoundation_short}} service?
{: #apply_update_mf}
{: faq}

The update to {{site.data.keyword.mobilefoundation_short}} can be applied by clicking **Recreate**.
On applying the update, the version of the server, as seen in the {{site.data.keyword.mfp_oc_short_notm}}, is modified to indicate the server update version.

* Users cannot apply their own fixes and updates to their {{site.data.keyword.mobilefoundation_short}} service instance.

See [Re-creating server in Professional Per Device plan](/docs/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5) and [Re-creating server in Professional 1 Application plan](/docs/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2) to understand the difference in behavior across the plans when **Recreate** is clicked.
{: note}

## How do I configure custom domain for my {{site.data.keyword.mobilefoundation_short}} server instance?
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} provisions a {{site.data.keyword.mfserver_short_notm}}, which is accessible by using a URL with the domain names based on the {{site.data.keyword.cloud_notm}} **Region**. You can also configure your own custom domain.

The URL or route is created with the default domain names based on the {{site.data.keyword.cloud_notm}} `Region`.

|  Domain  |  Region  |    
|:-------- |:-------- |    
|`mybluemix.net` | US South |    
|`eu-gb.mybluemix.net` | United Kingdom  |
|`au-syd.mybluemix.net` | Sydney  |   
|`eu-de.mybluemix.net` | Frankfurt |   
{: caption="Table 1. Application domain names based on Region in {{site.data.keyword.cloud_notm}}" caption-side="top"}

To use your own domain, you need to configure custom domain by performing the following steps:

1. Create a {{site.data.keyword.mfserver_short_notm}} instance by creating the {{site.data.keyword.mobilefoundation_short}} service instance by choosing one of the supported plans.
1. Add the custom domain that you would want to use, to your {{site.data.keyword.cloud_notm}} `Organization`. Go to **Manage Organizations > DOMAINS > ADD DOMAIN** to add your own domain.
1. Set up a route for the server to use your custom domain.
1. Go to the DNS provider for your domain, and add a CNAME entry, which routes the traffic from your domain to the default {{site.data.keyword.cloud_notm}} route, where the server is running.
1. If you would like to configure `https` for your custom domain, then upload the SSL certificate for your domain in {{site.data.keyword.cloud_notm}}. To upload the SSL certificate, go to **Manage Organizations > DOMAINS**, select the custom domain that you want to configure SSL certificate for, click the **Upload Certificate** to upload SSL certificate for your domain. For more information, see [this post](https://www.ibm.com/cloud/blog/ssl-certificates-bluemix-custom-domains){: external}.

## How do I back up my configuration and quickly onboard when there is no persistent database (Developer Plan)?
{: #persistentdatabase}
{: faq}

The Developer plan does not offer a persistent database, which might cause at times loss of data. To quickly onboard in such cases, be sure to follow these best practices:

* Every time that you make any of the following server-side actions:
   * Deploy an adapter or update any adapter configuration or property value
   * Perform any security configuration such scope-mapping and alike

   Run the following from the command line to download your configuration to a .zip file:

   ```bash
   $curl -X GET -u admin:admin -o export.zip http://<App Name>.mybluemix.net/mfpadmin/management-apis/2.0/runtimes/mfp/export/all
   ```

* In case you re-create your server or lose your configuration, run the following from the command line to import the configuration to the server:

   ```bash
   $curl -X POST -u admin:admin -F file=@./export.zip http://<App Name>.mybluemix.net/mfpadmin/management-apis/2.0/runtimes/mfp/deploy/multi
   ```
