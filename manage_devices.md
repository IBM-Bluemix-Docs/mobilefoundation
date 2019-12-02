---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-29"

keywords: device management

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

# Manage devices
{: #manage_devices}

Device access and device state can be managed from the {{site.data.keyword.mobilefoundation_short}} Operations console. A device is uniquely identified with an identifier called device ID, assigned by {{site.data.keyword.mobilefoundation_short}} client SDK. You can also set a display name for your device. Both the device ID and the device display name fields are indexed for search.

Application developers can use the `setDeviceDisplayName` method of the `WLClient` class to set the device display name. See [here](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/) for the `WLClient` documentation. Java&trade; adapter developers can also set the device display name by using the `setDeviceDisplayName` method of the `com.ibm.mfp.server.registration.external.model MobileDeviceData` class.
{: tip}

{{site.data.keyword.mobilefoundation_short}} server maintains the status information for every device that accesses the server.
The possible status values are:
* Active
* Lost
* Stolen
* Expired
* Disabled

The default status for a device is **Active**, which indicates that the access from this device is not blocked. You can change the status to **Lost**, **Stolen**, or **Disabled** to block access to your application resources from the device. You can always restore the **Active** status to allow access again.

The **Expired** status is a special status that is set by {{site.data.keyword.mobilefoundation_short}} server after a device reached a preconfigured inactivity duration. This status is used for license tracking, and it does not affect the access rights of the device. When a device with an **Expired** status reconnects to the server, its status is restored to **Active**, and the device is granted access to the server.

## Block devices
{: #block_devices}

1. Go to the **Devices** page from the {{site.data.keyword.mobilefoundation_short}} Operations console.
1. Use the **Search** field to search for a device by providing the user ID that is associated with the device, or by providing the display name of the device (if it is set on a device).
1. For each of the device returned in the search result, you can see the device ID, display name, the device model, the operating system, and the list of users IDs that are associated with the device.
1. The device status column shows the status of the device. You can change the status of the device to **Lost**, **Stolen**, or **Disabled**, to block access from the device to protected application resources.

   Changing the status back to **Active** restores the original access rights.
   {: note}

## Unregister devices
{: #unregister_devices}

1. Go to the **Devices** page from the {{site.data.keyword.mobilefoundation_short}} Operations console.
1. Use the **Search** field to search for a device by providing the user ID that is associated with the device, or by providing the display name of the device (if it is set on a device).
1. For each of the device returned in the search result, you can see the device ID, display name, the device model, the operating system, and the list of users IDs that are associated with the device.
1. Select *Unregister* from the **Actions** column.

   Unregistering a device deletes the registration data of all the {{site.data.keyword.mobilefoundation_short}} applications that are installed on the device. In addition, the device display name, the list of users that are associated with the device, and the public attributes that the applications registered for this device are deleted.
   {: note}

Do not **Unregister** a device, if you intended to **Block** the device. Unregistering a device removes all device-related data. If a user attempts to access the {{site.data.keyword.mobilefoundation_short}} server by using the same device, then user is prompted to register the device and registering creates a new device ID for the device in the server. This means that the device regains access to the {{site.data.keyword.mobilefoundation_short}} server.
{: important}
