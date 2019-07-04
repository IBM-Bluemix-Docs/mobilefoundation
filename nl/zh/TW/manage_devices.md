---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 管理裝置
{: #manage_devices}

裝置存取及裝置狀態可以從「Mobile Foundation 作業」主控台進行管理。使用稱為裝置 ID、且由 Mobile Foundation 用戶端 SDK 所指派的 ID，可唯一識別裝置。您也可以設定裝置的顯示名稱。會對裝置 ID 及裝置顯示名稱欄位檢索，以進行搜尋。

應用程式開發人員可以使用 `WLClient` 類別的 `setDeviceDisplayName` 方法，來設定裝置顯示名稱。如需 `WLClient` 文件，請參閱[這裡](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/)。Java 配接器開發人員也可以使用 `com.ibm.mfp.server.registration.external.model MobileDeviceData` 類別的 `setDeviceDisplayName` 方法來設定裝置顯示名稱。
{: tip}

Mobile Foundation Server 會維護存取伺服器之每個裝置的狀態資訊。可能的狀態值如下：
* 作用中
* 遺失
* 遭竊
* 過期
* 已停用

裝置的預設狀態是**作用中**，指出不會封鎖來自此裝置的存取。您可以將狀態變更為**遺失**、**遭竊**或**已停用**，以封鎖從裝置存取應用程式資源。您一律可以還原**作用中**狀態，以容許重新存取。

**過期**狀態是 Mobile Foundation Server 在裝置達到預先配置的閒置期間之後所設定的特殊狀態。此狀態用於授權追蹤，但不會影響裝置的存取權。具有**過期**狀態的裝置重新連接至伺服器時，其狀態會還原為**作用中**，而且會將伺服器的存取權授與該裝置。

## 區塊裝置
{: #block_devices}

1. 從「Mobile Foundation 作業」主控台移至**裝置**頁面。
2. 使用**搜尋**欄位，藉由提供與裝置相關聯的使用者 ID 或提供裝置的顯示名稱（如果已在裝置上設定）來搜尋裝置。
3. 針對搜尋結果中傳回的每個裝置，您可以查看裝置 ID、顯示名稱、裝置機型、作業系統，以及與裝置相關聯的使用者 ID 清單。
4. 裝置狀態直欄會顯示裝置的狀態。您可以將裝置的狀態變更為**遺失**、**遭竊**或**已停用**，以封鎖從裝置存取受保護的應用程式資源。

   將狀態變更回**作用中**，會還原原始存取權。
   {: note}


## 取消登錄裝置
{: #unregister_devices}

1. 從「Mobile Foundation 作業」主控台移至**裝置**頁面。
2. 使用**搜尋**欄位，藉由提供與裝置相關聯的使用者 ID 或提供裝置的顯示名稱（如果已在裝置上設定）來搜尋裝置。
3. 針對搜尋結果中傳回的每個裝置，您可以查看裝置 ID、顯示名稱、裝置機型、作業系統，以及與裝置相關聯的使用者 ID 清單。
4. 從**動作**直欄中，選取*取消登錄*。

   取消登錄裝置，會刪除裝置上安裝的所有 Mobile Foundation 應用程式的登錄資料。此外，還會刪除裝置顯示名稱、與裝置相關聯的使用者清單，以及應用程式針對此裝置所登錄的公用屬性。
   {: note}


如果您要**封鎖**裝置，請不要**取消登錄**裝置。取消登錄裝置會移除所有裝置相關資料。如果使用者嘗試使用相同的裝置來存取 Mobile Foundation Server，則系統會提示該使用者登錄裝置，而登錄將為伺服器中的裝置建立新的裝置 ID。這表示裝置會重新取得對 Mobile Foundation Server 的存取。
{: important}
