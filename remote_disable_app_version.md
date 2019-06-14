---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: disable apps, remote disabling of apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Remotely disable an app version
{: #remotely_disable_an_app_version}

In this section, we discuss how-to to disable user access to a specific version of an application on a specific mobile operating system, and how-to provide a custom message to the user.

You can use the Mobile Foundation Operations Console to manage the app access.

1. Select your application version from the **Applications** section of the console’s navigation sidebar, and then select the application **Management** tab.
2. Change the status to **Access Disabled**.
3. Provide a URL for the new application version (usually, in the appropriate public or private app store), in the field **URL of latest version**.
   For some environments, the Application Center provides a URL to access the details view of an application version directly. See [Application properties](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties).
   {: tip}

4. In the **Default notification message** field, add the custom notification message to display when the user attempts to access the application. The following sample message directs users to upgrade to the latest version:
   `This version is no longer supported. Please upgrade to the latest version.`
5. In the **Supported locales** section, you can optionally, provide the notification message in other languages.
6. Select **Save** to apply your changes.

When a user runs an application that was remotely disabled, a dialog window with the configured custom message is displayed. The message is displayed on any application interaction that requires access to a protected resource, or when the application tries to obtain an access token. If you provided a version-upgrade URL, the dialog displays a **Get new version** button for upgrading to a newer version, in addition to the default **Close** button. <br/>
If the user closes the dialog window without upgrading the version, they can continue to work with the application resources that are not protected. However, any application interaction that requires access to a protected resource causes the dialog window to be displayed again and the application or user is not granted access to the resource.
