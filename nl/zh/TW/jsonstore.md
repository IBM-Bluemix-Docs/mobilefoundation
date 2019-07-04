---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-14"

keywords: JSONStore, offline storage, jsonstore error codes

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:pre: .pre}


# JSONStore
{: #jsonstore}
{{site.data.keyword.mobilefoundation_short}} **JSONStore** 是選用的用戶端 API，可提供輕量型文件導向的儲存空間系統。JSONStore 會啟用 **JSON 文件**的持續性儲存空間。應用程式中的文件會在 JSONStore 中提供，即使執行應用程式的裝置已離線。例如，裝置中沒有可用的網路連線時，這個持續性且一律可用的儲存空間可協助使用者存取文件。
{: shortdesc}

因為 JSONStore 是開發人員所熟悉的，所以本文件有時會使用關聯式資料庫術語來協助說明 JSONStore。不過，關聯式資料庫與 JSONStore 之間有許多差異。例如，用來將資料儲存至關聯式資料庫的嚴格綱目與 JSONStore 的方法不同。使用 JSONStore，您可以儲存任何 JSON 內容，並檢索需要搜尋的內容。

## 主要特性
{: #key-features-jsonstore }
* 資料檢索，以進行有效率地搜尋
* 適用於追蹤對已儲存資料所做之僅限本端變更的機制
* 支援多位使用者
* 已儲存資料的 AES 256 加密提供安全及機密性。如果單一裝置上有多位使用者，您可以使用密碼保護來劃分使用者的保護。

單一儲存庫可以有多個集合，而每個集合都可以有多份文件。也可能有由多個儲存庫組成的 MobileFirst 應用程式。如需相關資訊，請參閱 JSONStore 多重使用者支援。

## 支援層次
{: #support-level-jsonstore }
* 原生 iOS 及 Android 應用程式中支援 JSONStore（原生 Windows（Universal 及 UWP）不予支援）。
* Cordova iOS、Android 及 Windows（Universal 及 UWP）應用程式中支援 JSONStore。


## 一般 JSONStore 術語
{: #general-jsonstore-terminology }
### 文件
{: #document }
文件是 JSONStore 的基本構成要素。

JSONStore 文件是具有自動產生 ID (`_id`) 及 JSON 資料的 JSON 物件。這類似於資料庫術語中的記錄或列。`_id` 的值一律是特定集合內的唯一整數。部分函數（例如 `JSONStoreInstance` 類別中的 `add`、`replace` 及 `remove`）採用「文件」或「物件」的「陣列」。這些方法適用於一次對各種「文件」或「物件」執行多項作業。

單一文件：

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

文件陣列：

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### 集合
{: #collection }
JSONStore 集合類似於資料庫術語中的表格。  
下列程式碼範例不是在磁碟上儲存文件的方式，但是是用來視覺化集合的高層次外觀的好方式。

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### 儲存庫
{: #store }
儲存庫是由一個以上集合所組成的持續性 JSONStore 檔案。  
儲存庫類似於資料庫術語中的關聯式資料庫。儲存庫也稱為 JSONStore。

### 搜尋欄位
{: #search-fields }
搜尋欄位是一個鍵值組。  
搜尋欄位是為了快速查閱時間而檢索的索引鍵，類似於資料庫術語中的直欄欄位或屬性。

額外搜尋欄位是已檢索的索引鍵，但不是已儲存 JSON 資料的一部分。這些欄位定義檢索其值（在 JSON 集合中）且可用來更快速搜尋的索引鍵。

有效的資料類型包含：「字串」、「布林」、「數字」及「整數」。這些類型只是類型提示，並未進行類型驗證。此外，這些類型決定如何儲存可檢索的欄位。例如，`{age: 'number'}` 會將 1 檢索為 1.0，而 `{age: 'integer'}` 會將 1 檢索為 1。

搜尋欄位和額外搜尋欄位：

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

只能檢索物件內的索引鍵，而不是物件本身。陣列是以透通方式處理，這表示您無法檢索陣列或陣列的特定索引 (arr[n])，但您可以檢索陣列內的物件。

對陣列中的值編製索引：

```javascript

var searchFields = {
    'people.name' : 'string', // matches carlos and tim on myObject
    'people.age' : 'integer' // matches 99 and 100 on myObject
};

var myObject = {
    people : [
        {name: 'carlos', age: 99},
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### 查詢
{: #queries }
查詢是使用搜尋欄位或額外搜尋欄位來尋找文件的物件。  
這些範例假設 name 搜尋欄位的類型是「字串」，而 age 搜尋欄位的類型是「整數」。

尋找其 `name` 與 `carlos` 符合的文件：

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

尋找其 `name` 與 `carlos` 符合並且 `age` 與 `99` 符合的文件：

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### 查詢組件
{: #query-parts }
查詢組件用來建置更進階的搜尋。部分 JSONStore 作業（例如 `find` 或 `count` 的部分版本）採用查詢組件。查詢組件內的所有項目都是依據 `AND` 陳述式結合的，而查詢組件本身則是依據 `OR` 陳述式結合的。只有在查詢組件內的所有項目都是 **true** 時，搜尋準則才會傳回相符項。您可以使用多個查詢組件來搜尋滿足一個以上查詢組件的相符項。

使用查詢組件尋找，只能在最上層的搜尋欄位上運作。例如：使用 `name`，而不是 `name.first`。使用包含的所有搜尋欄位均為最上層欄位的多個集合，可規避此行為。使用非最上層搜尋欄位的查詢組件作業包含：`equal`、`notEqual`、`like`、`notLike`、`rightLike`、`notRightLike`、`leftLike` 及 `notLeftLike`。如果您使用非最上層搜尋欄位，則行為不確定。

## 特性表格
{: #features-table }
比較 JSONStore 特性與其他資料儲存技術及格式的特性。

JSONStore 是適用於將資料儲存於 Cordova 應用程式內的 JavaScript API，而 Cordova 應用程式使用 MobileFirst 外掛程式、適用於原生 iOS 應用程式的 Objective-C API，以及適用於原生 Android 應用程式的 Java API。如需參照，以下是不同 JavaScript 儲存技術的比較，可查看 JSONStore 與它們的比較情況。

JSONStore 類似於 LocalStorage、Indexed DB、Cordova Storage API 及 Cordova File API 這類技術。此表格顯示 JSONStore 所提供的部分特性與其他技術的比較情況。JSONStore 特性只適用於 iOS 及 Android 裝置和模擬器。

| 特性                                               | JSONStore      | LocalStorage | IndexedDB | Cordova Storage API | Cordova File API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Android 支援（Cordova 及原生應用程式）             |	     ✔ 	      |        ✔	           |         ✔	      |         ✔	      |         ✔	      |
| iOS 支援（Cordova 及原生應用程式）                 |	     ✔ 	      |        ✔	           |         ✔	      |         ✔	      |         ✔	      |
| Windows 8.1 Universal 及 Windows 10 UWP（Cordova 應用程式）          |	     ✔ 	      |     ✔	     |         ✔	      |        -	           |         ✔	      |
| 資料加密       	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| 儲存空間上限   	                                 |可用空間       |    ~5 MB     |   ~5 MB 	 | 可用空間  | 可用空間  |
| 可靠的儲存空間（請參閱附註）                     |	     ✔ 	      |      -	    |     -	     |         ✔	      |         ✔	      |
| 追蹤本端變更                                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| 多使用者支援                                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| 檢索   	                                         |	     ✔ 	      |      -	    |        ✔	           |         ✔	      |         -	      |
| 儲存空間類型                                     | JSON 文件      | 鍵值組          | JSON 文件      | 關聯式 (SQL)     | 字串        |
{: caption="表 1. 特性比較" caption-side="top"}

「可靠的儲存空間」表示除非發生下列其中一個事件，否則不會刪除您的資料：
* 從裝置中移除應用程式。
* 呼叫用於移除資料的其中一種方法。
{: note}

## 多使用者支援
{: #multiple-user-support }
使用 JSONStore，您可以建立許多由單一 MobileFirst 應用程式中的不同集合所組成的儲存庫。

init (JavaScript) 或 open（原生 iOS 及原生 Android）API 可以採用具有使用者名稱的 options 物件。不同的儲存庫是檔案系統中的個別檔案。使用者名稱用來作為儲存庫的檔名。基於安全及隱私權理由，這些個別儲存庫可以使用不同的密碼進行加密。呼叫 closeAll API 將移除對所有集合的存取。也可以藉由呼叫 changePassword API 來變更已加密儲存庫的密碼。

使用案例範例：如共用實體裝置（例如 iPad 或 Android 平板電腦）及 MobileFirst 應用程式的多位員工。若員工在不同班次工作，並處理來自不同客戶的專用資料，同時使用 MobileFirst 應用程式，則多使用者支援非常有用。

## 安全
{: #security-jsonstore }
您可以加密儲存庫中的所有集合，來保護它們的安全。

若要加密儲存庫中的所有集合，請將密碼傳遞至 `init` (JavaScript) 或 `open`（原生 iOS 及原生 Android）API。如果未傳遞任何密碼，則不會加密儲存庫集合中的任何文件。

部分安全構件（例如 salt）儲存在金鑰鏈 (iOS)、共用喜好設定 (Android) 及認證鎖定器（Windows Universal 8.1 及 Windows 10 UWP）中。儲存庫會以 256 位元的「進階加密標準 (AES)」金鑰進行加密。所有金鑰都是使用密碼型金鑰鍵衍生函數 2 (PBKDF2) 來增強。您可以選擇為應用程式加密資料收集，但無法切換已加密與純文字格式，或混合使用儲存庫內的格式。

用於保護儲存庫中資料的金鑰係根據您所提供的使用者密碼。金鑰不會到期，但您可以藉由呼叫 changePassword API 來進行變更。

資料保護金鑰 (DPK) 是用來解密儲存庫內容的金鑰。DPK 會保存在 iOS 金鑰鏈中，即使應用程式已解除安裝。若要移除金鑰鏈中的金鑰，以及 JSONStore 放在應用程式中的所有其他項目，請使用 destroy API。此程序不適用於 Android，因為已加密 DPK 儲存在共用喜好設定中，並且在應用程式解除安裝時予以清除。

JSONStore 第一次使用密碼來開啟集合時（這表示開發人員想要加密儲存庫內的資料），JSONStore 需要一個隨機記號。該隨機記號可以從用戶端或伺服器中取得。

若 localKeyGen 索引鍵出現在 JSONStore API 的 JavaScript 實作中，而且其值為 true，則會在本端產生一個加密的安全記號。否則，會藉由聯絡伺服器（需要與 MobileFirst Server 的連線）來產生記號。只有在第一次使用密碼開啟儲存庫時，才需要此記號。依預設，原生實作（Objective-C 及 Java）會在本端產生一個加密的安全記號，或者，您可以透過 secureRandom 選項來傳遞一個加密的安全記號。

下列作業之間的取捨：
* 離線開啟儲存庫，並信任用戶端來產生該隨機記號（較不安全），或
* 使用對 MobileFirst Server（需要連線功能）的存取權來開啟儲存庫，並信任伺服器（較安全）

### 安全公用程式
{: #security-utilities }
MobileFirst 用戶端 API 提供一些安全公用程式，以協助保護使用者的資料。如果您要保護 JSON 物件，優先推薦 JSONStore 這類特性。不過，建議您不要在 JSONStore 集合中儲存二進位 Blob。

而是將二進位資料儲存在檔案系統上，並將檔案路徑和其他 meta 資料儲存在 JSONStore 集合內。如果您要保護影像這類檔案，可以將它們編碼為 base64 字串，並進行加密，然後將輸出寫入磁碟。

若要解密資料，您可以查閱 JSONStore 集合中的 meta 資料，讀取磁碟中的已加密資料，然後使用所儲存的 meta 資料予以解密。

此 meta 資料可以包括索引鍵、salt、起始設定向量 (IV)、檔案類型、檔案路徑及其他項目。

進一步瞭解 [JSONStore 安全公用程式](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities)。
{: tip}

### Windows 8.1 Universal 及 Windows 10 UWP 加密
{: #windows-81-universal-and-windows-10-uwp-encryption }
您可以加密儲存庫中的所有集合，來保護它們的安全。

JSONStore 使用 [SQLCipher ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://sqlcipher.net/){: new_window} 作為其基礎資料庫技術。SQLCipher 是 Zetetic 所產生的 SQLite 建置，而 LLC 會新增資料庫的加密層。

JSONStore 在所有平台上使用 SQLCipher。在 Android 及 iOS 上，提供一個稱為 Community Edition 的 SQLCipher 免費開放程式碼版本，而 Community Edition 會被納入 Mobile Foundation 所內含的 JSONStore 版本。Windows 版本的 SQLCipher 只有透過商業授權才能提供，而且無法由 Mobile Foundation 直接重新配送。

相反地，適用於 Windows 8 Universal 的 JSONStore 將包括 SQLite 作為基礎資料庫。若要加密上述任一平台的資料，您需要獲得自己的 SQLCipher 版本，並交換 Mobile Foundation 中所內含的 SQLite 版本。

如果您不需要加密，則 JSONStore 會使用 Mobile Foundation 中的 SQLite 版本完整運作（不加密）。

#### 將 SQLite 取代為適用於 Windows Universal 及 Windows UWP 的 SQLCipher
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. 執行隨附 SQLCipher for Windows Runtime Commercial Edition 的 SQLCipher for Windows Runtime 8.1/10 延伸。
2. 安裝延伸完成之後，找出剛才建立的 **sqlite3.dll** 檔案的 SQLCipher 版本。其中一個適用於 x86、一個適用於 x64，一個適用於 ARM。

    ```bash
    C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
    ```
    {: codeblock}

3. 複製這個檔案，並取代 MobileFirst 應用程式。

    ```bash
    <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
    ```
    {: codeblock}

## 效能
{: #performance-jsonstore }
下列因素會影響 JSONStore 效能。

### 網路
{: #network-jsonstore }
* 執行將所有變動過的文件傳送給配接器這類作業之前，請先檢查網路連線功能。
* 透過網路傳送至用戶端的資料量會嚴重影響效能。僅傳送應用程式所需要的資料，而不是複製後端資料庫內的所有資料。
* 如果您使用配接器，請考慮將 compressResponse 旗標設為 true。如此一來，即會壓縮回應，這通常會使用較少的頻寬，而且傳送時間比不使用壓縮的速度更快。

### 記憶體
{: #memory-jsonstore }
* 當您使用 JavaScript API 時，會將 JSONStore 文件序列化並解除序列化為原生（Objective-C、Java 或 C#）層與 JavaScript 層之間的字串。降低可能記憶體問題的其中一種方法，是在使用 find API 時使用限制及偏移。如此一來，您會限制針對結果所配置的記憶體數量，而且可以實作分頁（每頁顯示 X 個結果）這類事項。
* 考慮將最終序列化及解除序列化為「字串」的長金鑰名稱對映到較短的金鑰名稱（例如：`myVeryVeryVerLongKeyName` 到 `k` 或 `key`），而不要使用那些長金鑰名稱。理想狀況下，當您將它們從配接器傳送至用戶端時，可以將它們對映至短金鑰名稱，而在您將資料送回後端時，會將它們對映至原始長金鑰名稱。
* 考慮將儲存庫內的資料分割為各種集合。具有各種集合的小型文件，而不是單一集合的龐大文件。此考量取決於資料的緊密相關程度與所指定資料的使用案例。
* 當您搭配使用 add API 與物件陣列時，可能會發生記憶體問題。若要降低此問題，請一次使用較少的 JSON 物件來呼叫這些方法。
* JavaScript 及 Java™ 具有記憶體回收器，而 Objective-C 具有「自動參照計數」。容許它運作，但不完全依賴。嘗試將不再使用的參照設為空值，並使用側寫工具來確認記憶體用量在您預期它下降時下降。

### CPU
{: #cpu-jsonstore }
* 當您呼叫執行檢索的 add 方法時，所使用的搜尋欄位及額外搜尋欄位數量會影響效能。只會檢索用於 find 方法之查詢中的值。
* 依預設，JSONStore 會追蹤其文件的本端變更。此行為可以予以停用，因此節省一些循環，方法是在您使用 add、remove 及 replace API 時將 `markDirty` 旗標設為 **false**。
* 啟用安全會將一些額外負擔新增至 `init` 或 `open` API，以及其他使用集合內文件的作業。請考量是否真的需要安全。例如，open API 的加密速度較慢，因為它必須產生用於加密及解密的加密金鑰。
* `replace` 及 `remove` API 取決於集合大小，因為它們必須透過整個集合才能取代或移除所有出現項目。因為它必須透過每筆記錄，所以必須將每筆記錄解密，因此在使用加密時會變慢。此效能命中對大型集合更為顯著。
* `count` API 相當昂貴。不過，您可以保留可保留該集合計數的變數。每次您儲存或移除集合中的項目時，都要更新它。
* `find` API（`find`、`findAll` 及 `findById`）受到加密的影響，因為它們必須解密每份文件才能看到它是否符合。對於依查詢尋找，如果達到限制，則可能較為快速，因為當它達到結果限制時就會停止。JSONStore 不需要將其餘的文件解密，即可瞭解是否還有任何其他搜尋結果。

## 並行性
{: #concurrency-jsonstore }
### JavaScript 中的並行性
{: #javascript-jsonstore }
可對集合執行的大部分作業（例如新增及尋找）都是非同步作業。這些作業會傳回一個 jQuery 承諾，在作業順利完成時會解決此承諾，而在作業失敗時拒絕此承諾。這些承諾與成功及失敗回呼類似。

「jQuery 延遲」是可解決或拒絕的承諾。下列範例並非 JSONStore 特有的，但一般是要協助您瞭解其使用情形。

您也可以接聽 JSONStore `success` 及 `failure` 事件，而非承諾及回呼。執行以傳遞給事件接聽器之引數為基礎的動作。

範例 Promise 定義：

```javascript
var asyncOperation = function () {
  // Assumes that you have jQuery defined via $ in the environment
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```
{: codeblock}

範例 Promise 用法：

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

範例回呼定義：

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

範例回呼用法：

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

範例事件：

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```
{: codeblock}

### Objective-C 中的並行性
{: #objective-c-jsonstore }
當您使用適用於 JSONStore 的 Native iOS API 時，會將所有作業新增至同步分派佇列。此行為確保觸及儲存庫的作業依序在不是主要執行緒的執行緒上執行。如需相關資訊，請參閱 Apple 文件，網址為 [Grand Central Dispatch (GCD) ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}。

### Java 中的並行性
{: #java-jsonstore }
當您使用適用於 JSONStore 的 Native Android API 時，所有作業都會在主要執行緒上執行。您必須建立執行緒，或使用執行緒儲存區，才能具有非同步行為。所有儲存作業都是安全執行緒。

## 分析
{: #analytics-jsonstore }
您可以收集與 JSONStore 相關的重要分析資訊部分

### 檔案資訊
{: #file-information }
如果在 analytics 旗標設為 **true** 的情況下呼叫 JSONStore API，則會針對每個應用程式階段作業收集一次檔案資訊。應用程式階段作業定義為將應用程式載入至記憶體，並將其從記憶體中移除。您可以使用此資訊來判斷 JSONStore 內容在應用程式中使用了多少空間。

### 效能度量值
{: #performance-metrics }
每次使用作業的開始及結束時間相關資訊來呼叫 JSONStore API 時，就會收集效能度量值。您可以使用此資訊來判斷各項作業所花費的時間（毫秒）。

### 適用於 iOS 的 JSONStore 範例
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

### 適用於 Android 的 JSONStore 範例
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

### 適用於 JavaScript 的 JSONStore 範例
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## 使用外部資料
{: #working-with-external-data }
您可以在數個不同概念中使用外部資料：**取回**及**推送**。

### 取回
{: #pull }
許多系統都使用「取回」術語來表示從外部來源取得資料。  
有三個重要部分：

#### 從外部資料來源取回
{: #external-data-source-pull }
此來源可以是資料庫、REST 或 SOAP API，或是許多其他項目。唯一的需求是必須可從 MobileFirst Server 或直接從用戶端應用程式進行存取。理想狀況下，您希望此來源以 JSON 格式傳回資料。

#### 取回的傳輸層
{: #transport-layer-pull }
此來源是如何將外部來源中的資料放入內部來源（即儲存庫內的 JSONStore 集合）。替代方案是使用配接器。

#### 取回的內部資料來源 API
{: #internal-data-source-api-pull }
此來源是 JSONStore API，可用來將 JSON 資料新增至集合。

您可以將從檔案、輸入欄位或寫在變數中之資料所讀取的資料移入內部儲存庫中。它不一定只來自需要網路通訊的外部來源。
{: note}

下列所有程式碼範例都是以虛擬碼撰寫，其與 JavaScript 類似。

請將配接器用於「傳輸層」。使用配接器的一些優點包含：XML 至 JSON、安全、過濾以及取消伺服器端程式碼與用戶端程式碼的連結。
{: note}
##### 外部資料來源：後端 REST 端點
{: #external-data-source-backend-rest-endpoint-head}
假設您有一個 REST 端點，該端點會從資料庫讀取資料，並將其作為 JSON 物件陣列傳回。

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

傳回的資料可能類似下列範例：

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

##### 傳輸層：配接器
{: #transport-layer-adapter}
假設您已建立一個稱為 people 的配接器，並已定義一個稱為 getPeople 的程序。此程序會呼叫 REST 端點，並將 JSON 物件的陣列傳回給用戶端。建議您在這裡執行更多工作，例如，只將一部分的資料傳回給用戶端。

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

在用戶端上，您可以使用 WLResourceRequest API 來取得資料。此外，建議您將部分參數從用戶端傳遞至配接器。範例是：具有用戶端前次透過配接器從外部來源取得新資料之時間的日期。

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

建議您充分運用 `compressResponse`、`timeout` 以及可傳遞給 `WLResourceRequest` API 的其他參數。  
{: note}

您可以選擇跳過配接器，並使用 jQuery.ajax 這類項目直接聯絡 REST 端點與您要儲存的資料。

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### 內部資料來源 API：JSONStore
{: #internal-data-source-api-jsonstore}
從後端取得回應後，使用 JSONStore 來處理這些資料。
JSONStore 提供一種追蹤本端變更的方法。它可讓某些 API 將文件標示為「變動過」。API 會記錄前次對文件所執行的作業，以及將文件標示為「變動過」的時間。然後，您可以使用此資訊來實作資料同步化這類特性。

change API 會取得資料及部分選項：

###### replaceCriteria
{: #replacecriteria}
  
這些搜尋欄位是輸入資料的一部分。它們用來找出已在集合內的文件。例如，如果您選取：

```javascript
['id', 'ssn']
```
{: codeblock}

作為取代準則，傳遞下列陣列作為輸入資料：

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

而 `people` 集合已包含下列文件：

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

`change` 作業會找出完全符合下列查詢的文件：

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

然後，`change` 作業會使用輸入資料來執行取代，而集合包含下列各項：

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

名稱已從 `Carlitos` 變更為 `Carlos`。如果有多份文件符合取代準則，則所有符合的文件都會取代為個別的輸入資料。

###### addNew 
{: #addnew}
 
沒有任何文件符合取代準則時，change API 會查看此旗標的值。如果此旗標設為 **true**，則 change API 會建立新的文件，並將它新增至儲存庫。否則，不採取其他任何動作。

###### markDirty 
{: #markdirty}
 
決定 change API 是否將已取代或新增的文件標示為「變動過」。

從配接器傳回資料陣列：

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

您可以使用其他 API 來追蹤對所儲存之本端文件的變更。一律會取得您對其執行作業之集合的存取元。

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

然後，您可以新增資料（JSON 物件的陣列），並決定是否要將它標示為「變動過」。一般而言，當您從外部來源取得變更時，會想要將 markDirty 旗標設為 false。當您在本端新增資料時，會將此旗標設為 true。

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

您也可以取代文件，並選擇是否將具有取代的文件標示為「變動過」。

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

同樣地，您可以移除文件，並選擇是否將移除文件標示為「變動過」。當您使用 find API 時，不會顯示已移除及標示為「變動過」的文件。不過，它們仍位於集合內，直到您使用 `markClean` API，這會從集合中實際移除文件。如果文件未標示為「變動過」，則它實際上已從集合中移除。

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### 推送
{: #push }
許多系統都使用「推送」術語來表示將資料傳送至外部來源。

有三個重要部分：

#### 推送的內部資料來源 API
{: #internal-data-source-api-push }
此來源是指傳回具有僅限本端變更（變動過）之文件的 JSONStore API。

#### 推送的傳輸層
{: #transport-layer-push }
此來源說明您要如何聯絡外部資料來源以傳送變更。

#### 推送至外部資料來源
{: #external-data-source-push }
此來源一般是指資料庫、REST 或 SOAP 端點及其他項目，可收到用戶端對資料所做的更新。

下列所有程式碼範例都是以虛擬碼撰寫，其與 JavaScript 類似。

請將配接器用於「傳輸層」。使用配接器的一些優點包含：XML 至 JSON、安全、過濾以及取消伺服器端程式碼與用戶端程式碼的連結。
{: note}

##### 內部資料來源 API：JSONStore  
{: #internal-data-source-api-jsonstore-head}
在您取得集合的存取元之後，請呼叫 `getAllDirty` API 來取得所有標示為「變動過」的文件。這些文件具有僅限本端變更，而您想要透過傳輸層將其傳送至外部資料來源。

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

`dirtyDocs` 引數類似下列範例：

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

這些欄位如下：
* `_id`：JSONStore 使用的內部欄位。每份文件都會獲指派一個唯一的 ID。
* `json`：已儲存的資料。
* `_operation`：前次對文件所執行的作業。可能的值為 add、store、replace 及 remove。
* `_dirty`：儲存為數字的時間戳記，以表示將文件標示為「變動過」的時間。

##### 傳輸層：MobileFirst 配接器  
{: #transport-layr-mobilefirst-adapter }
您可以選擇將變動過的文件傳送至配接器。假設您具有使用 `updatePeople` 程序所定義的 `people` 配接器。

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

建議您充分運用 `compressResponse`、`timeout` 以及可傳遞給 `WLResourceRequest` API 的其他參數。
{: note}

在 MobileFirst Server 上，配接器具有 `updatePeople` 程序，其可能類似下列範例：

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

您可能需要更新有效負載以符合後端所預期的格式，而非轉遞來自用戶端之 `getAllDirty` API 的輸出。您可能需要將取代、移除及併入分割為個別的後端 API 呼叫。

您可以選擇反覆運算 `dirtyDocs` 陣列，並檢查 `_operation` 欄位。然後，將取代傳送至某個程序、將移除傳送至另一個程序，並將併入傳送至其他程序。前一個範例會將所有變動過的文件大量傳送至配接器。

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

您可以選擇跳過配接器，並直接聯絡 REST 端點。

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### 外部資料來源：後端 REST 端點
{: #external-data-source-backend-rest-endpoint }
後端會接受或拒絕變更，然後將回應轉遞回用戶端。在用戶端查看回應之後，可以將已更新的文件傳遞至 markClean API。

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

文件在標示為全新之後，就不會顯示在 `getAllDirty` API 的輸出中。

## 疑難排解 JSONStore
{: #troubleshooting-jsonstore }

## 在您尋求協助時提供資訊
{: #provide-information-when-you-ask-for-help }
最好提供更多的資訊，這總比冒著未提供足夠資訊的風險要好。下列清單是協助 JSONStore 問題所需資訊的不錯起點。

* 作業系統及版本。例如，Windows XP SP3 Virtual Machine 或 Mac OSX 10.8.3。
* Eclipse 版本。例如，Eclipse Indigo 3.7 Java EE。
* JDK 版本。例如，Java SE Runtime Environment（建置 1.7）。
* {{ site.data.keys.product }} 版本。例如，IBM Worklight 5.0.6 版 Developer Edition。
* iOS 版本。例如，iOS Simulator 6.1 或 iPhone 4S iOS 6.0（已淘汰，請參閱「已淘汰的特性及 API 元素」）。
* Android 版本。例如，Android Emulator 4.1.1 或 Samsung Galaxy Android 4.0 API Level 14。
* Windows 版本。例如，Windows 8、Windows 8.1 或 Windows Phone 8.1。
* adb 版本。例如，Android Debug Bridge 1.0.31 版。
* 日誌，例如 iOS 上的 Xcode 輸出或 Android 上的 logcat 輸出。

## 嘗試隔離問題
{: #try-to-isolate-the-issue }
請遵循下列步驟來隔離問題，以更精確地報告問題。

1. 重設 Android 模擬器或 iOS 模擬器，並呼叫 destroy API 以使用乾淨的系統開始。
2. 確定您是在支援的正式作業環境上執行。
    * Android >= 2.3 ARM 第 7 版/ARM 第 8 版/x86 模擬器或裝置
    * iOS >= 6.0 模擬器或裝置（已淘汰）
    * Windows 8.1/10 ARM/x86/x64 模擬器或裝置
3. 嘗試藉由不將密碼傳遞給 init 或 open API 來關閉加密。
4. 查看 JSONStore 所產生的 SQLite 資料庫檔案。必須關閉加密。

   * Android 模擬器：

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * iOS 模擬器：

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Windows 8.1 Universal/Windows 10 UWP 模擬器

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   在 Web 瀏覽器（Firefox、Chrome、Safari、Internet Explorer）上執行的「僅 JavaScript」實作不使用 SQLite 資料庫。檔案儲存在 HTML5 LocalStorage 中。
    {: note}
   * 查看含 `.schema` 的 `searchFields`，並使用 `SELECT * FROM <collection-name>;` 來選取資料。若要結束 sqlite3，請鍵入 `.exit`。如果您將使用者名稱傳遞至 init 方法，則檔案稱為 **the-username.sqlite**。如果您未傳遞使用者名稱，則依預設，檔案稱為 **jsonstore.sqlite**。
5. （僅限 Android）啟用詳細 JSONStore。

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```
    {: codeblock}

6. 使用除錯器。

## 常見問題
{: #common-issues-jsonstore }
瞭解下列 JSONStore 特徵有助於解決您可能遇到的一些常見問題。  

* 在 JSONStore 中儲存二進位資料的唯一方法是先以 base64 進行編碼。儲存檔名或路徑，而不是 JSONStore 中的實際檔案。
* 只有在 {{ site.data.keys.v62_product_full }} 6.2.0 版中，才可能從原生程式碼存取 JSONStore 資料。
* 不限制您可以儲存在 JSONStore 內的資料量，這會超出行動作業系統所強制的限制。
* JSONStore 提供持續資料儲存空間。它不僅儲存在記憶體中。
* 集合名稱以數字或符號開頭時，init API 會失敗。IBM Worklight 5.0.6.1 版及更新版本會傳回適當的錯誤：`4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* 搜尋欄位中的數字與整數之間存在差異。類型是 `number` 時，`1` 及 `2` 這類數值會儲存為 `1.0` 及 `2.0`。類型為 `integer` 時，它們會儲存為 `1` 及 `2`。
* 如果強制停止或損毀應用程式，則在應用程式再次啟動並呼叫 `init` 或 `open` API 時，一律會失敗，錯誤碼為 -1。如果發生此問題，請先呼叫 `closeAll` API。
* JSONStore 的 JavaScript 實作預期會循序呼叫程式碼。請先等待作業完成，再呼叫下一個作業。
* 適用於 Cordova 應用程式的 Android 2.3.x 不支援交易。
* 在 64 位元裝置上使用 JSONStore 時，您可能會看到下列錯誤：`java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* 此錯誤表示您的 Android 專案中有 64 位元原生程式庫，而在您使用這些程式庫時，JSONStore 目前未作用。若要確認，請移至 Android 專案下的 **src/main/libs** 或 **src/main/jniLibs**，並檢查您是否具有 x86_64 或 arm64-v8a 資料夾。如果有的話，請刪除這些資料夾，JSONStore 即可再次運作。
* 在某些情況（環境）中，流程會先進入 `wlCommonInit()`，再起始設定 JSONStore 外掛程式。這會導致 JSONStore 相關 API 呼叫失敗。`cordova-plugin-mfp` 引導會在完成時自動呼叫 `WL.Client.init` 以觸發 `wlCommonInit` 函數。JSONStore 外掛程式的這個起始設定程序是不同的。JSONStore 外掛程式沒有方法可以_中止_ `WL.Client.init` 呼叫。在不同的環境之間，流程可能會在 `mfpjsonjslloaded` 完成之前先進入 `wlCommonInit()`。若要確定 `mfpjsonjsloaded` 及 `mfpjsloaded` 事件的順序，開發人員可以選擇手動呼叫 `WL.CLient.init`。這不需要具有平台特定程式碼。

  使用下列步驟以配置為手動呼叫 `WL.CLient.init`：                             

  1. 在 `config.xml` 中，將 `clientCustomInit` 內容變更為 **true**。

  + 在 `index.js` 檔案中：                                   
    * 在檔案開頭新增下列這一行：                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * 在 `wlCommonInit()` 中保留 `WL.JSONStore.init` 呼叫                    

    * 新增下列函數：  
    ```javascript                                         
    function initWL(){                                                     
            var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
            : {};                                                                
            WL.Client.init(options);                                           
    }
    ```                                                                     

這將等待 `mfpjsonjsloaded` 事件（在 `wlCommonInit` 外部），這會確定已載入 Script 並且隨後會呼叫 `WL.Client.init` 以觸發 `wlCommonInit`，接著這會呼叫 `WL.JSONStore.init`。

## 儲存庫內容
{: #store-internals }
請參閱如何儲存 JSONStore 資料的範例。

這個簡化範例中的重要元素：

* `_id` 是唯一 ID（例如，AUTO INCREMENT PRIMARY KEY）。
* `json` 包含所儲存 JSON 物件的確切呈現。
* `name` 及 age 是搜尋欄位。
* `key` 是額外搜尋欄位。

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |
{: caption="表 2. JSONStore 資料鍵元素範例" caption-side="top"}

當您使用下列其中一個查詢或下列查詢組合：`{_id : 1}, {name: 'carlos'}`、`{age: 99}, {key: 'c'}` 進行搜尋時，傳回的文件是 `{_id: 1, json: {name: 'carlos', age: 99} }`。

其他內部 JSONStore 欄位如下：

* `_dirty`：決定是否將文件標示為「變動過」。此欄位適用於追蹤對文件的變更。
* `_deleted`：是否將文件標示為已刪除。此欄位適用於從集合中移除物件，並於稍後使用它們來追蹤後端的變更，然後決定是否移除它們。
* `_operation`：該字串可反映要對文件執行的最後一個作業（例如，replace）。

## JSONStore 錯誤
{: #jsonstore-errors }
### JavaScript 錯誤
{: #javascript-errors }
JSONStore 使用 error 物件來傳回失敗原因的訊息。

在 JSONStore 作業（例如，`JSONStoreInstance` 類別中的 `find` 及 `add` 方法）期間發生錯誤時，會傳回 error 物件。它會提供失敗原因的相關資訊。

```javascript
var errorObject = {
  src: 'find', // Operation that failed.
  err: -50, // Error code.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Error message.
  col: 'people', // Collection name.
  usr: 'jsonstore', // User name.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Document that is related to the failure.
  res: {...} // Response from the server.
}
```
{: codeblock}
並非所有鍵值組都是每個 error 物件的一部分。例如，只有在作業失敗時，才能使用 doc 值，因為文件（例如，`JSONStoreInstance` 類別中的 `remove` 方法）無法移除文件。

### Objective-C 錯誤
{: #objective-c-errors }
所有可能會失敗的 API 都會採用使用 NSError 物件位址的 error 參數。如果您不要收到錯誤通知，可以傳入 `nil`。作業失敗時，會將 NSError 移入位址，而 NSError 具有錯誤及某些可能的 `userInfo`。`userInfo` 可能會包含額外詳細資料（例如，導致失敗的文件）。

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java 錯誤
{: #java-errors }
所有 Java API 呼叫都會擲出特定異常狀況（視發生的錯誤而定）。您可以個別處理每個異常狀況，也可以捕捉 `JSONStoreException` 作為所有 JSONStore 異常狀況的保護傘。

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### 錯誤碼清單
{: #list-of-error-codes }
一般錯誤碼及其說明的清單：

|錯誤碼          | 說明 |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | 無法辨識的錯誤。|
| -75 OS\_SECURITY\_FAILURE | 此錯誤碼與 requireOperatingSystemSecurity 旗標有關。如果 destroy API 無法移除受作業系統安全保護的安全 meta 資料（具有密碼撤回的 Touch ID），或是 init 或 open API 找不到安全 meta 資料，則可能發生此錯誤。如果裝置不支援作業系統安全，但要求使用作業系統安全，則也可能失敗。|
| -50 PERSISTENT\_STORE\_NOT\_OPEN | 關閉 JSONStore。請先嘗試在 JSONStore 類別中呼叫 open 方法，以啟用儲存庫的存取。|
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | 回復交易時發生問題。|
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION | 無法在進行一個交易的同時呼叫 removeCollection。|
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | 無法在進行多個交易的同時呼叫 destroy。|
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | 無法在有多個交易的同時呼叫 closeAll。|
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | 無法在進行多個交易的同時起始設定儲存庫。|
| -43 TRANSACTION_FAILURE | 交易發生問題。|
| -42 NO\_TRANSACTION\_IN\_PROGRESS | 無法在沒有進行中交易時確定回復交易 |
| -41 TRANSACTION\_IN\_POGRESS | 無法在進行另一個交易的同時啟動新的交易。|
| -40 FIPS\_ENABLEMENT\_FAILURE | FIPS 發生錯誤。|
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | 從檔案系統取得檔案資訊時發生問題。|
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | 取代集合中的文件時發生問題。|
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | 移除集合中的文件時發生問題。|
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | 儲存「資料保護金鑰 (DPK)」時發生問題。|
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | 檢索輸入資料時發生問題。|
| -12 INVALID\_SEARCH\_FIELD\_TYPES |確認您要傳遞給 searchFields 的類型是 string、integer、number 或 boolean。|
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | 對文件陣列的作業，例如 replace 方法在與特定文件搭配使用時可能會失敗。會傳回失敗的文件，並回復交易。在 Android 上，嘗試在不受支援的架構上使用 JSONStore 時，也會發生此錯誤。|
| -10 ACCEPT\_CONDITION\_FAILED | 使用者提供的 accept 函數已傳回 false。|
| -9 OFFSET\_WITHOUT\_LIMIT | 若要使用偏移，您也必須指定限制。|
| -8 INVALID\_LIMIT\_OR\_OFFSET | 驗證錯誤，必須是正整數。|
| -7 INVALID_USERNAME | 驗證錯誤（只能是 [A-Z] 或 [a-z] 或 [0-9]）。|
| -6 USERNAME\_MISMATCH\_DETECTED | 若要登出，JSONStore 使用者必須先呼叫 closeAll 方法。一次只能有一位使用者。|
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED | destroy 方法在嘗試刪除保留儲存庫內容的檔案時發生問題。|
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | destroy 方法在嘗試清除金鑰鏈 (iOS) 或共用使用者喜好設定 (Android) 時發生問題。|
| -3 INVALID\_KEY\_ON\_PROVISION | 已將錯誤的密碼傳遞至已加密的儲存庫。|
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | 搜尋欄位不是動態的。對新的搜尋欄位呼叫 init 或 openmethod 之前，需要呼叫 destroy 方法或 removeCollection 方法，才能變更搜尋欄位。如果您變更搜尋欄位的名稱或類型，則可能發生此錯誤。例如：{key: 'string'} 到 {key: 'number'}，或 {myKey: 'string'} 到 {theKey: 'string'}。|
| -1 PERSISTENT\_STORE\_FAILURE | 一般錯誤。原生程式碼異常，很可能是呼叫 init 方法。|
| 0 SUCCESS | 在某些情況下，JSONStore 原生程式碼會傳回 0 以指出成功。|
| 1 BAD\_PARAMETER\_EXPECTED\_INT | 驗證錯誤。|
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | 驗證錯誤。|
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | 驗證錯誤。|
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | 驗證錯誤。|
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | 驗證錯誤。|
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | 驗證錯誤。|
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | 驗證錯誤。|
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB | 選取所有標示為變動過之文件的查詢失敗。查詢 SQL 中的範例會是：SELECT * FROM [collection] WHERE _dirty > 0。|
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | 若要使用 JSONStoreCollection 類別中 push 及 load 方法這類的函數，必須將配接器傳遞至 init 方法。|
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | 驗證錯誤 |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | 驗證錯誤 |
| 12 ADAPTER_FAILURE | 呼叫 WL.Client.invokeProcedure 時發生問題，特別是連接至配接器時發生的問題。此錯誤與嘗試呼叫後端之配接器中的失敗不同。|
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | 驗證錯誤 |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | 不容許在 JSONStoreCollection 類別中呼叫 enhance 方法來取代現有函數（find 及 add）。|
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Push 會將文件傳送至配接器，但 JSONStore 無法將文件標示為未變動過。|
| 16 COULD\_NOT\_GET\_SECURE\_KEY | 若要起始具有密碼的集合，則必須連接至 {{ site.data.keys.mf_server }}，因為它會傳回「安全隨機記號」。IBM Worklight 5.0.6 版及更新版本容許開發人員產生安全隨機記號，而此記號透過 options 物件在本端將 {localKeyGen: true} 傳遞至 init 方法。|
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | 無法載入資料，因為 WL.Client.invokeProcedure 已呼叫失敗回呼。|
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | 已傳遞至 init 方法的 load 物件未通過驗證。|
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | 當您呼叫 add 方法時，load 物件中使用的索引鍵發生問題。|
| 20 UNDEFINED\_PUSH\_OPERATION | 未定義將變動過文件推送至伺服器的程序。例如：已呼叫 init 方法（新文件變動過，作業 = 'add'）及 push 方法（尋找作業 = 'add' 的新文件），但在鏈結至集合的配接器中找不到任何具有 add 程序的 add 索引鍵。鏈結配接器是在 init 方法內完成。|
| 21 INVALID\_ADD\_INDEX\_KEY | 額外搜尋欄位時發生問題。|
| 22 INVALID\_SEARCH\_FIELD | 其中一個搜尋欄位無效。請驗證所傳入的搜尋欄位都不是 _id、json、_deleted 或 _operation。|
| 23 ERROR\_CLOSING\_ALL | 一般錯誤。原生程式碼呼叫 closeAll 方法時發生錯誤。|
| 24 ERROR\_CHANGING\_PASSWORD | 無法變更密碼。例如，傳遞的舊密碼錯誤。|
| 25 ERROR\_DURING\_DESTROY | 一般錯誤。原生程式碼呼叫 destroy 方法時發生錯誤。|
| 26 ERROR\_CLEARING\_COLLECTION | 一般錯誤。原生程式碼呼叫 removeCollection 方法時發生錯誤。|
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | 驗證錯誤。|
| 28 INVALID\_SORT\_OBJECT | 提供的排序陣列無效，因為其中一個 JSON 物件無效。正確語法是 JSON 物件的陣列，其中，每個物件都只包含單一內容。此內容會搜尋用來排序的欄位，以及其為遞增還是遞減。例如：{searchField1 : "ASC"}。|
| 29 INVALID\_FILTER\_ARRAY | 提供的過濾結果陣列無效。此陣列的正確語法是字串陣列，其中，每個字串都是搜尋欄位或內部 JSONStore 欄位。如需相關資訊，請參閱「儲存庫內容」。|
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | 陣列不是只有 JSON 物件的陣列時發生驗證錯誤。|
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | 驗證錯誤。|
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | 驗證錯誤。|
{: caption="表 3. 常見錯誤碼" caption-side="top"}
