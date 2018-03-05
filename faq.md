---

copyright:
  years: 2018
lastupdated:  "2018-02-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}


# FAQ

This FAQ provides answers to common questions about the {{site.data.keyword.mobilefoundation_long}} service.
{: shortdesc}

## How do I know when updates to {{site.data.keyword.mobilefoundation_short}} service is made available?
{: #maintupdates_mf}

{{site.data.keyword.mobilefoundation_short}} provisions a {{site.data.keyword.mfserver_short_notm}}. The updates to the {{site.data.keyword.mobilefoundation_short}} server are notified to the users. A notification will be shown in the service instance dashboard. The user can choose to apply the update to {{site.data.keyword.mobilefoundation_short}} during a maintenance window that is decided by him. You can choose to update the {{site.data.keyword.mobilefoundation_short}} server when it is convenient for you.

{{site.data.keyword.mobilefoundation_short}} service update will be made available when one of the following components are updated.

* {{site.data.keyword.mfserver_short_notm}}.
* Underlying Liberty version.
* Underlying Java Developer Kit version.

## How do I apply the updates to {{site.data.keyword.mobilefoundation_short}} service?
{: #apply_update_mf}

The update to {{site.data.keyword.mobilefoundation_short}} can be applied by clicking **Recreate**.
On applying the update, the version of the server, as seen in the {{site.data.keyword.mfp_oc_short_notm}}, will be modified to indicate the server update version.

> **Note:**
>  * Users will not be able to apply their own fixes and updates to their {{site.data.keyword.mobilefoundation_short}} service instance.
>  * See [Re-creating server in Developer plan](c_using_mfs_p1.html#recreate_mobilefoundation_p1), [Re-creating server in DeveloperPro plan](c_using_mfs_p3.html#recreate_mobilefoundation_p3), [Re-creating server in Professional Per Capacity plan](c_using_mfs_p4.html#recreate_mobilefoundation_p4) and [Re-creating server in Professional 1 Application plan](c_using_mfs_p2.html#recreate_mobilefoundation_p2) to understand the difference in behavior across the plans  when **Recreate** is clicked.
>

## How do I configure custom domain for my {{site.data.keyword.mobilefoundation_short}} server instance?
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} provisions a {{site.data.keyword.mfserver_short_notm}}, which is accessible using a URL having the  domain names based on the {{site.data.keyword.Bluemix_notm}} **Region**. You can also configure your own custom domain.

The URL or route is created with the default domain names based on the {{site.data.keyword.Bluemix_notm}} `Region`.

  |Domain |  Region  |    
  |:----- | :----- |    
  |`mybluemix.net` | US South |    
  |`eu-gb.mybluemix.net` | United Kingdom  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Table 1. Application domain names based on Region in {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

To be able to use your own domain you will need to configure custom domain by performing the following steps:

1.	Create a {{site.data.keyword.mfserver_short_notm}} instance  by creating the {{site.data.keyword.mobilefoundation_short}} service instance by choosing one of the supported plans.

+ Add the custom domain that you would want to use, to your {{site.data.keyword.Bluemix_notm}} `Organization`. Go to **Manage Organizations > DOMAINS > ADD DOMAIN** to add your own domain.

+ Set up a route for the <!--container group--> server to use your custom domain.

+ Go to the DNS provider for your domain, and add a CNAME entry, which will route the traffic from your domain to the default {{site.data.keyword.Bluemix_notm}} route, where the <!--container group--> server is running.

+ If you would like to configure `https` for your custom domain then upload the SSL certificate for your domain in {{site.data.keyword.Bluemix_notm}}. To do this go to **Manage Organizations > DOMAINS**, select the custom domain you want to configure SSL certificate for, click the **Upload Certificate** to upload SSL certificate for your domain. Refer to [this post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}, for more information.