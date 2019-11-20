---

copyright:
  years: 2018
lastupdated: "2019-11-15"

keywords: export apps, adapters export

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

# Export and import applications and adapters
{: #export_import_apps_adapters}

From the {{site.data.keyword.mfp_oc_short_notm}}, under certain conditions, you can export an application or one of its versions, and later import it to a different server. You can also export and reimport adapters. Use this capability for reuse or back up purposes or to migrate across {{site.data.keyword.mfserver_short_notm}} instances.

If you're granted the **mfpadmin** administrator role and the **mfpdeployer** deployer role, you can export one version or all versions of an application. The application or version is exported as a `.zip` compressed file, which saves the *application ID*, *descriptors*, *authenticity data*, and *web resources*. You can later import the archive to redeploy the application or version to the same or a different server.

Carefully consider your use case:
* The export file includes the application authenticity data. That data is specific to the build of a mobile app. The mobile app includes the URL of the server. Hence, if you want to use another server, you must rebuild the app. Transferring only the exported app files wouldn't work.
* Some artifacts might vary from one server to another. Push credentials are different depending on whether you work in a development or production environment.
* The application runtime configuration (that contains the active or disabled state and the log profiles) can be transferred in some cases but not all.
* Transferring web resources might not make sense in some cases, for example when you rebuild the app to use a new server.
{: tip}

## Prerequisite
{: #prereq}

Launch the {{site.data.keyword.mfp_oc_short_notm}}.

## Procedure
{: #procedure}

1. From the navigation sidebar, select an application or application version, or an adapter.
1. Select **Actions > Export Application** or **Export Version** or **Export Adapter**.
   You're prompted to save the `.zip` archive file that encapsulates the exported resources. The aspect of the dialog box depends on your browser and the target folder depends on your browser settings.
1. Save the archive file.
   The archive file name includes the name and version of the application or adapter, for example `export_applications_com.sample.zip`.
1. To reuse an existing export archive, select **Actions > Import Application** or **Import Version** or **Import Adapter**, browse to the archive, and click **Deploy**.
   The main console frame displays the details of the imported application or adapter.

## Results
{: #results}

If you import to the same server, the application or version isn't necessarily restored as it was exported. That is, the redeployment at import time doesn't remove subsequent modifications. Rather, if some application resources are modified between export time and redeployment at import time, only the resources that are included in the exported archive are redeployed in their original state.

For example, if you export an application with no authenticity data, then you upload authenticity data, and then you import the initial archive, the authenticity data isn't erased.
