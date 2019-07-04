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

# 遠端停用應用程式版本
{: #remotely_disable_an_app_version}

在此章節中，我們討論如何停用特定行動作業系統上特定應用程式版本的使用者存取權，以及如何向使用者提供自訂訊息。

您可以使用「Mobile Foundation 作業主控台」來管理應用程式存取。

1. 從主控台導覽資訊看板的**應用程式**區段中選取您的應用程式版本，然後選取應用程式**管理**標籤。
2. 將狀態變更為**已停用存取權**。
3. 在**最新版本的 URL** 欄位中，提供新應用程式版本的 URL（通常，是在適當的公用或專用應用程式儲存庫中）。在部分環境中，「應用程式中心」會提供 URL 來直接存取應用程式版本的詳細資料視圖。請參閱[應用程式內容](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties)。
   {: tip}

4. 在**預設通知訊息**欄位中，新增要在使用者嘗試存取應用程式時顯示的自訂通知訊息。下列範例訊息會指示使用者升級至最新版本：
   `This version is no longer supported. Please upgrade to the latest version.`
5. 在**支援的語言環境**區段中，您可以選擇提供其他語言的通知訊息。
6. 選取**儲存**，以套用變更。

當使用者執行已遠端停用的應用程式時，會顯示一個內含已配置自訂訊息的對話視窗。任何需要存取受保護資源的應用程式互動，或當應用程式嘗試取得存取記號時，都會顯示該訊息。如果您已提供版本升級 URL，則除了預設**關閉**按鈕之外，此對話框還會顯示**取得新版本**按鈕，以升級至較新版本。<br/>
如果使用者關閉對話視窗而不升級版本，則可以繼續使用未受保護的應用程式資源。不過，任何需要存取受保護資源的應用程式互動都會再次顯示對話框視窗，而且不會將資源的存取權授與應用程式或使用者。
