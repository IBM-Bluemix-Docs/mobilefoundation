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
{{site.data.keyword.mobilefoundation_short}} **JSONStore** 是一个可选的客户机端 API，提供了面向文档的轻量级存储系统。JSONStore 支持持久存储 **JSON 文档**。即使在运行应用程序的设备脱机时，该应用程序中的文档在 JSONStore 中仍可用。这种始终可用的持久性存储器非常有用，例如可在设备中没有网络连接可用时，支持用户访问文档。
{: shortdesc}

因为开发者十分熟悉关系数据库术语，所以在本文档中有时会使用关系数据库术语来帮助说明 JSONStore。但是关系数据库和 JSONStore 之间有很多不同。例如，用于在关系数据库中存储数据的严格模式与 JSONStore 方法不同。使用 JSONStore，您可以存储任何 JSON 内容，并可对您需要搜索的内容建立索引。

## 主要功能
{: #key-features-jsonstore }
* 用于高效搜索的数据索引
* 用于跟踪对已存储数据的仅本地更改的机制
* 支持多个用户
* 对已存储数据进行 AES 256 加密以提供安全性和机密性。如果单个设备上有多个用户，那么可以使用密码保护按用户进行分段保护。

单个存储器中可以有多个集合，每个集合可以有多个文档。还可以使一个 MobileFirst 应用程序包含多个存储器。有关信息，请参阅 JSONStore 多用户支持。

## 支持级别
{: #support-level-jsonstore }
* 在本机 iOS 和 Android 应用程序中支持 JSONStore（本机 Windows（Universal 和 UWP）不支持）。
* 在 Cordova iOS、Android 和 Windows（Universal 和 UWP）应用程序中支持 JSONStore。


## 常规 JSONStore 术语
{: #general-jsonstore-terminology }
### 文档 (Document)
{: #document }
文档是 JSONStore 的基本构建块。

JSONStore 文档是具有自动生成的标识 (`_id`) 和 JSON 数据的 JSON 对象，类似于数据库术语中的记录或行。`_id` 的值始终是特定集合中的唯一整数。`JSONStoreInstance` 类中的某些函数（如 `add`、`replace` 和 `remove`）采用“文档数组”或“对象数组”。这些方法对于同时针对各种文档或对象执行操作非常有用。

单个文档：

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

文档数组：

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### 集合 (Collection)
{: #collection }
JSONStore 集合类似于数据库术语中的表。  
以下代码示例并不是在磁盘上存储文档的方式，但很适合用于在高级别直观显示集合的样子。

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### 存储器 (Store)
{: #store }
存储器是包含一个或多个集合的持久性 JSONStore 文件。  
存储器类似于数据库术语中的关系数据库。存储器也称为 JSONStore。

### 搜索字段 (Search fields)
{: #search-fields }
搜索字段是键/值对。  
搜索字段是为了快速查找而建立索引的键，类似于数据库术语中的列字段或属性。

额外搜索字段是已建立索引但不属于所存储 JSON 数据的键。这些字段定义其值（在 JSON 集合中）已建立索引的键，并且可以用于更快地进行搜索。

有效数据类型为：String、Boolean、Number 和 Integer。这些类型只是类型提示，没有类型验证。此外，这些类型决定了如何存储可以建立索引的字段。例如，`{age: 'number'}` 将 1 作为 1.0 建立索引，`{age: 'integer'}` 将 1 作为 1 建立索引。

搜索字段和额外搜索字段：

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

只能对对象中的键（而不是对象本身）建立索引。数组以传递方式进行处理，这意味着无法对数组或数组的特定索引 (arr[n]) 建立索引，但可以对数组中的对象建立索引。

对数组中的值建立索引：

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

### 查询 (Queries)
{: #queries }
查询是使用搜索字段或额外搜索字段来查找文档的对象。  
这些示例假定 name 搜索字段的类型为 string，age 搜索字段的类型为 integer。

查找其 `name` 与 `carlos` 匹配的文档：

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

查找其 `name` 与 `carlos` 匹配并且 `age` 与 `99` 匹配的文档：

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### 查询部分 (Query parts)
{: #query-parts }
查询部分用于构建更高级的搜索。某些 JSONStore 操作（例如，某些版本的 `find` 或 `count`）会采用查询部分。查询部分中的所有项都由 `AND` 语句进行连接，而查询部分本身由 `OR` 语句进行连接。仅在查询部分中的所有项均为 **true** 时，搜索条件才会返回匹配项。您可以使用多个查询部分来搜索满足一个或多个查询部分的匹配项。

使用查询部分进行查找仅作用于顶级搜索字段。例如：作用于 `name`，而不会作用于 `name.first`。使用包含的所有搜索字段均为顶级字段的多个集合，可规避此行为。处理非顶级搜索字段的查询部分操作为：`equal`、`notEqual`、`like`、`notLike`、`rightLike`、`notRightLike`、`leftLike` 和 `notLeftLike`。 如果使用非顶级搜索字段，那么行为不确定。

## 功能表
{: #features-table }
比较 JSONStore 功能与其他数据存储技术和格式的功能。

JSONStore 既是用于存储使用 MobileFirst 插件的 Cordova 应用程序中数据的 JavaScript API，也是用于本机 iOS 应用程序的 Objective-C API 以及用于本机 Android 应用程序的 Java API。请参考以下不同 JavaScript 存储技术的比较，以了解 JSONStore 与它们相比有哪些不同。

JSONStore 类似于 LocalStorage、Indexed DB、Cordova Storage API 和 Cordova File API 等技术。下表显示了 JSONStore 提供的某些功能与其他技术进行对比的结果。JSONStore 功能仅供 iOS 和 Android 设备以及模拟器使用。

|功能|JSONStore|LocalStorage|IndexedDB|Cordova Storage API|Cordova File API|
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
|Android 支持（Cordova 和本机应用程序）|	     ✔|✔|✔|✔|✔|
|iOS 支持（Cordova 和本机应用程序）|	     ✔|✔|✔|✔|✔|
|Windows 8.1 Universal 和 Windows 10 UWP（Cordova 应用程序）|	     ✔|✔|✔|        -	           |✔|
|数据加密|	     ✔|      -	    |     -	     |        -	           |         -	      |
|最大存储量|可用空间|约 5 MB|约 5 MB|可用空间|可用空间|
|可靠存储（请参阅注释）|	     ✔|      -	    |     -	     |✔|✔|
|跟踪本地更改|	     ✔|      -	    |     -	     |        -	           |         -	      |
|多用户支持|	     ✔|      -	    |     -	     |        -	           |         -	      |
|建立索引|	     ✔|      -	    |✔|✔|         -	      |
|存储类型|JSON 文档|键/值对|JSON 文档|关系数据库 (SQL)|字符串|
{: caption="表 1. 功能比较" caption-side="top"}

“可靠存储”表示除非发生以下其中一个事件，否则不会删除您的数据：
* 从设备上除去应用程序。
* 调用除去数据的某种方法。
{: note}

## 多用户支持
{: #multiple-user-support }
使用 JSONStore，可以在单个 MobileFirst 应用程序中创建包含不同集合的多个存储器。

init (JavaScript) 或 open（本机 iOS 和本机 Android）API 可使用包含用户名的 options 对象。不同的存储器是文件系统中的单独文件。用户名用作存储器的文件名。出于安全性和隐私的原因，这些单独存储器可以通过不同的密码进行加密。调用 closeAll API 将除去对所有集合的访问权。也可以通过调用 changePassword API 来更改已加密存储器的密码。

示例用例是共享物理设备（例如，iPad 或 Android 平板电脑）和 MobileFirst 应用程序的各种员工。员工使用 MobileFirst 应用程序按不同轮班工作并处理来自不同客户的私有数据时，多用户支持非常有用。

## 安全性
{: #security-jsonstore }
您可以加密存储器中的所有集合以确保其安全性。

要加密存储器中的所有集合，请将密码传递到 `init` (JavaScript) 或 `open`（本机 iOS 和本机 Android）API。如果未传递密码，那么不会加密存储器集合中的任何文档。

某些安全工件（例如，加密盐 (Salt)）存储在密钥链 (iOS)、共享首选项 (Android) 和凭据保险箱（Windows Universal 8.1 和 Windows 10 UWP）中。存储器利用 256 位高级加密标准 (AES) 密钥进行加密。所有密钥通过基于密码的密钥派生函数 2 (PBKDF2) 进行增强。您可以选择对应用程序的数据集合进行加密，但不能在加密格式和纯文本格式之间进行切换，也不能在一个存储器中混合使用不同格式。

用于保护存储器中数据的密钥基于您提供的用户密码。密钥不会到期，但您可以通过调用 changePassword API 来更改密钥。

数据保护密钥 (DPK) 是用于解密存储器内容的密钥。DPK 保存在 iOS 密钥链中，即使应用程序已卸载也是如此。要除去密钥链中的密钥以及 JSONStore 放入应用程序中的其他任何项，请使用 destroy API。 此过程不适用于 Android，因为加密的 DPK 会存储在共享首选项中，并会在卸载应用程序时擦除。

JSONStore 第一次使用密码打开集合时（这意味着开发者想要加密存储器中的数据），JSONStore 需要随机令牌。该随机令牌可以从客户机或服务器获取。

JSONStore API 的 JavaScript 实现中提供 localKeyGen 密钥并且其值为 true 时，将在本地生成使用密码保护的安全令牌。否则，将通过联系服务器来生成此种令牌，这需要连接到 MobileFirst 服务器。仅在第一次使用密码打开存储器时需要此令牌。缺省情况下，本机实现（Objective-C 和 Java）会在本地生成使用密码保护的安全令牌，或者您可以通过 secureRandom 选项来传递此种令牌。

以下各项之间的权衡如下：
* 以脱机方式打开存储器并信任客户机，以生成该随机令牌（安全性较低），或者
* 通过访问 MobileFirst 服务器（需要连接）并信任该服务器来打开存储器（安全性更高）

### 安全实用程序
{: #security-utilities }
MobileFirst 客户机端 API 提供了一些安全实用程序来帮助保护用户的数据。如果要保护 JSON 对象，那么 JSONStore 等功能非常有用。但是，建议不要在 JSONStore 集合中存储二进制 Blob。

应改为在文件系统上存储二进制数据，并将文件路径和其他元数据存储在 JSONStore 集合中。如果要保护图像等文件，可以将其编码为 Base64 字符串，对其加密，然后将输出写入磁盘。

要对数据进行解密，可以在 JSONStore 集合中查找元数据，从磁盘读取加密的数据，然后使用存储的元数据对其进行解密。

此元数据可包括密钥、加密盐 (Salt)、初始化向量 (IV)、文件类型、文件路径等。

了解有关 [JSONStore 安全实用程序](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities)的更多信息。
{: tip}

### Windows 8.1 Universal 和 Windows 10 UWP 加密
{: #windows-81-universal-and-windows-10-uwp-encryption }
您可以加密存储器中的所有集合以确保其安全性。

JSONStore 将 [SQLCipher ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://sqlcipher.net/){: new_window} 用作其底层数据库技术。SQLCipher 是 Zetetic, LLC 生成的 SQLite 构建，用于向数据库添加一个加密层。

JSONStore 在所有平台上使用 SQLCipher。在 Android 和 iOS 上，可使用免费的开放式源代码版本的 SQLCipher，即 Community Edition，此版本已合并到 Mobile Foundation 中包含的各 JSONStore 版本中。Windows 版 SQLCipher 仅在商业许可下可用，不能由 Mobile Foundation 直接再分发。

JSONStore for Windows 8 Universal 改为将 SQLite 作为底层数据库包含在内。要对其中任何一个平台进行数据加密，您需要获取自己版本的 SQLCipher，并换出 Mobile Foundation 中包含的 SQLite 版本。

如果您不需要加密，那么 JSONStore 可使用 Mobile Foundation 中的 SQLite 版本实现全功能运行（不包括加密）。

#### 针对 Windows Universal 和 Windows UWP，将 SQLite 替换为 SQLCipher
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. 运行 SQLCipher for Windows Runtime Commercial Edition 随附的 SQLCipher for Windows Runtime 8.1/10 扩展。
2. 安装完扩展后，查找刚创建的 **sqlite3.dll** 文件的 SQLCipher 版本。 有一个用于 x86 的版本，一个用于 x64 的版本，一个用于 ARM 的版本。

    ```bash
    C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
    ```
    {: codeblock}

3. 将此文件复制到 MobileFirst 应用程序进行替换。

    ```bash
    <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
    ```
    {: codeblock}

## 性能
{: #performance-jsonstore }
下面是可能会影响 JSONStore 性能的因素。

### 网络
{: #network-jsonstore }
* 在执行操作（例如，将所有脏文档发送到适配器）之前，请先检查网络连接。
* 通过网络发送到客户机的数据量会严重影响性能。因此，请仅发送应用程序所需的数据，而不要复制后端数据库中的所有内容。
* 如果在使用适配器，请考虑将 compressResponse 标志设置为 true。这将压缩响应，与无压缩相比，通常压缩后的响应所用带宽较少，并且传输速度更快。

### 内存
{: #memory-jsonstore }
* 在使用 JavaScript API 时，JSONStore 文档将序列化和反序列化为本机（Objective-C、Java 或 C#）层和 JavaScript 层之间的字符串。缓解可能的内存问题的一种方法是在使用 find API 时应用限制和偏移量。这样，您可限制针对结果分配的内存量，并且可实现分页（每页显示 X 个结果）等功能。
* 请不要使用最终序列化和反序列化为字符串的长密钥名称，而是考虑将这些长密钥名称映射到较短的名称（例如：将 `myVeryVeryVerLongKeyName` 映射到 `k` 或 `key`）。理想情况下，您在将长密钥名称从适配器发送到客户机时，会将其映射到短密钥名称；在将数据发送回后端时，又会将其映射到原始长密钥名称。
* 考虑将存储器中的数据拆分为不同的集合。让小型文档分布在各个集合中，而不是在单个集合中包含整个文档。此注意事项取决于数据间的相关程度以及这些数据的用例。
* 将 add API 用于对象数组时，可能会遇到内存问题。要缓解此问题，请同时对较少的 JSON 对象调用这些方法。
* JavaScript 和 Java™ 具有垃圾收集器，而 Objective-C 具有自动引用计数。可以使用这些工具，但不要完全依赖这些工具。尝试使不再使用的引用失效，并且在预计内存使用量会下降时，使用概要分析工具来检查内存使用量是否在下降。

### CPU
{: #cpu-jsonstore }
* 在调用建立索引的 add 方法时，使用的搜索字段和额外搜索字段的数量会影响性能。仅对 find 方法的查询中使用的值建立索引。
* 缺省情况下，JSONStore 会跟踪对其文档的本地更改。通过在使用 add、remove 和 replace API 时，将 `markDirty` 标志设置为 **false**，可以禁用此行为，从而节省若干周期。
* 启用安全性会为 `init` 或 `open` API 以及使用集合中文档的其他操作增加一些开销。请考虑是否真正需要安全性。例如，open API 在使用加密时的速度要慢得多，因为它必须生成用于加密和解密的加密密钥。
* `replace` 和 `remove` API 取决于集合大小，因为这两个 API 必须遍历整个集合来替换或除去所有符合条件项。由于必须遍历每个记录，也就必须解密每个记录，所以使用加密时的速度就会慢得多。在大型集合上，此性能下降更加明显。
* `count` API 的开销相对较大。但是，您可以保留一个变量来保持对该集合的计数。每次在集合中存储或除去内容时，请更新该变量。
* `find` API（`find`、`findAll` 和 `findById`）受加密影响，因为这些 API 必须解密每个文档，以确定该文档是否匹配。对于按查询查找，如果超过了限制，查找速度可能会更快，这是因为在达到结果限制时会停止查找。JSONStore 不需要解密其余文档，以弄清楚是否还有其他任何搜索结果。

## 并行
{: #concurrency-jsonstore }
### JavaScript 中的并行操作
{: #javascript-jsonstore }
可以对集合执行的大部分操作都是异步的，例如 add 和 find。这些操作会在操作成功完成时返回解析的 jQuery Promise，在发生失败时返回拒绝的 jQuery Promise。这些 Promise 类似于成功和失败回调。

jQuery Deferred 是可以解析或拒绝的 Promise。以下示例虽然不是特定于 JSONStore 的示例，但旨在帮助您了解其一般用法。

您还可以侦听 JSONStore `success` 和 `failure` 事件，而不使用 Promise 和回调。执行基于传递到事件侦听器的参数的操作。

示例 Promise 定义：

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

示例 Promise 用法：

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

示例回调定义：

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

示例回调用法：

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

示例事件：

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```
{: codeblock}

### Objective-C 中的并行操作
{: #objective-c-jsonstore }
在将本机 iOS API 用于 JSONStore 时，所有操作都将添加到同步分派队列。此行为可确保涉及存储器的操作按顺序在非主线程上运行。有关更多信息，请参阅 Apple 文档：[Grand Central Dispatch (GCD) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}。

### Java 中的并行操作
{: #java-jsonstore }
在将本机 Android API 用于 JSONStore 时，所有操作都将在主线程上运行。您必须创建线程或者使用线程池以使用异步行为。所有存储器操作都是线程安全的。

## 分析
{: #analytics-jsonstore }
您可以收集与 JSONStore 相关的关键分析信息片段。

### 文件信息
{: #file-information }
如果调用 JSONStore API 并将 analytics 标志设置为 **true**，那么会针对每个应用程序会话收集一次文件信息。应用程序会话定义为从将应用程序装入内存起，到从内存中除去应用程序止。您可以使用这些信息来确定应用程序中的 JSONStore 内容使用的空间量。

### 性能度量值
{: #performance-metrics }
每次使用有关操作开始和结束时间的信息调用 JSONStore API 时，都将收集性能度量值。您可以使用这些信息来确定各种操作所用的时间（毫秒）。

### iOS 的 JSONStore 示例
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

### Android 的 JSONStore 示例
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

### JavaScript 的 JSONStore 示例
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## 使用外部数据
{: #working-with-external-data }
您可以在多个不同的概念中使用外部数据：**拉取**和**推送**。

### 拉取
{: #pull }
许多系统使用术语“拉取”来指从外部源获取数据。  
有三个重要部分：

#### 从外部数据源拉取
{: #external-data-source-pull }
此源可以是数据库、REST 或 SOAP API 等等。唯一的要求是源必须可从 MobileFirst 服务器或直接从客户机应用程序进行访问。理想情况下，您希望此源以 JSON 格式返回数据。

#### 用于拉取的传输层
{: #transport-layer-pull }
此源表示您如何将数据从外部源传送到内部源（存储器中的 JSONStore 集合）。一个备选方案是适配器。

#### 用于拉取的内部数据源 API
{: #internal-data-source-api-pull }
此源是可用于将 JSON 数据添加到集合的 JSONStore API。

可以使用从文件读取的数据、输入字段或变量中的硬编码数据来填充内部存储器。填充内容不必专门来自需要网络通信的外部源。
{: note}

以下所有代码示例都是以类似于 JavaScript 的伪码编写的。

针对传输层使用适配器。使用适配器的一些优点包括服务器端代码和客户机端代码的 XML 到 JSON 转换、安全性、过滤和解耦。
{: note}
##### 外部数据源：后端 REST 端点
{: #external-data-source-backend-rest-endpoint-head}
假设您具有一个 REST 端点，用于从数据库读取数据并将其作为 JSON 对象数组返回。

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

返回的数据可能类似于以下示例：

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

##### 传输层：适配器
{: #transport-layer-adapter}
假设创建名为 people 的适配器，并定义名为 getPeople 的过程。该过程将调用 REST 端点并将 JSON 对象数组返回给客户机。您可能希望在此执行更多操作，例如仅将数据的子集返回给客户机。

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

在客户机上，可以使用 WLResourceRequest API 来获取数据。此外，您可能希望将某些参数从客户机传递到适配器。例如，客户机上次通过适配器从外部源获取新数据的日期。

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

您可能希望利用可传递到 `WLResourceRequest` API 的 `compressResponse`、`timeout` 以及其他参数。  
{: note}

（可选）您可以跳过适配器，而使用类似于 jQuery.ajax 的内容来直接通过要存储的数据联系 REST 端点。

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

##### 内部数据源 API：JSONStore
{: #internal-data-source-api-jsonstore}
从后端获取响应后，使用 JSONStore 来处理这些数据。
JSONStore 提供了跟踪本地更改的方法。该方法支持某些 API 将文档标记为“脏”。API 将记录对文档执行的最后一个操作，以及将文档标记为“脏”的时间。然后可以使用这些信息来实现数据同步等功能。

change API 将接受数据和某些选项：

###### replaceCriteria
{: #replacecriteria}
  
这些搜索字段是输入数据的一部分，用于查找集合中已存在的文档。例如，如果选择：

```javascript
['id', 'ssn']
```
{: codeblock}

作为替换条件，将以下数组作为输入数据传递：

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

并且 `people` 集合已包含以下文档：

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

`change` 操作将查找与以下查询完全匹配的文档：

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

然后，`change` 操作会使用输入数据执行替换，因此集合包含：

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

名称已从 `Carlitos` 更改为 `Carlos`。如果多个文档与替换标准相匹配，那么匹配的所有文档都会使用相应的输入数据进行替换。

###### addNew 
{: #addnew}
 
当没有任何文档与替换标准相匹配时，change API 将查看此标志的值。如果此标志设置为 **true**，那么 change API 将创建新文档，并将其添加到存储器中。如果未设置为 true，那么不执行进一步的操作。

###### markDirty 
{: #markdirty}
 
确定 change API 是否将替换或添加的文档标记为“脏”。

将从适配器返回数据数组：

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

可以使用其他 API 来跟踪对存储的本地文档进行的更改。请始终获取您对其执行操作的集合的存取器。

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

然后，可以添加数据（JSON 对象的数组）并决定是否要将其标记为“脏”。通常情况下，从外部源获取更改时，您会希望将 markDirty 标志设置为 false。在本地添加数据时，请将此标志设置为 true。

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

还可以替换文档，并选择是否将包含替换项的文档标记为“脏”。

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

同样，可以除去文档，并选择是否将除去项标记为“脏”。使用 find API 时，不会显示已除去并标记为“脏”的文档。但是，这些文档会一直保留在集合中，直到使用 `markClean` API（从而将其从集合中实际除去）为止。如果文档未标记为“脏”，那么该文档会从集合中实际除去。

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### 推送
{: #push }
许多系统使用术语“推送”来指将数据发送到外部源。

有三个重要部分：

#### 用于推送的内部数据源 API
{: #internal-data-source-api-push }
此源是 JSONStore API，用于返回包含仅本地更改（脏）的文档。

#### 用于推送的传输层
{: #transport-layer-push }
此源是您希望联系外部数据源以发送更改的方式。

#### 推送到外部数据源
{: #external-data-source-push }
此源通常是数据库、REST 或 SOAP 端点等等，用于接收客户机对数据进行的更新。

以下所有代码示例都是以类似于 JavaScript 的伪码编写的。

针对传输层使用适配器。使用适配器的一些优点包括服务器端代码和客户机端代码的 XML 到 JSON 转换、安全性、过滤和解耦。
{: note}

##### 内部数据源 API：JSONStore  
{: #internal-data-source-api-jsonstore-head}
获取集合的存取器后，调用 `getAllDirty` API 以获取标记为“脏”的所有文档。这些文档中包含您要通过传输层发送到外部数据源的仅本地更改。

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

`dirtyDocs` 自变量类似于以下示例：

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

字段如下：
* `_id`：JSONStore 使用的内部字段。每个文档都会分配有唯一的相应值。
* `json`：存储的数据。
* `_operation`：对文档执行的最后一个操作。可能的值为 add、store、replace 和 remove。
* `_dirty`：存储为数字的时间戳记，表示文档被标记为“脏”的时间。

##### 传输层：MobileFirst 适配器  
{: #transport-layr-mobilefirst-adapter }
您可以选择将脏文档发送到适配器。假设您具有定义有 `updatePeople` 过程的 `people` 适配器。

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

您可能希望利用可传递到 `WLResourceRequest` API 的 `compressResponse`、`timeout` 以及其他参数。
{: note}

在 MobileFirst 服务器上，适配器具有 `updatePeople` 过程，该过程可能类似于以下示例：

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

您可能必须更新有效内容以与后端预期的格式相匹配，而不是中继客户机上 `getAllDirty` API 的输出。可能必须将替换项、除去项以及包含项拆分成不同的后端 API 调用。

（可选）可以迭代 `dirtyDocs` 数组，并检查 `_operation` 字段。然后，将替换项发送到一个过程，将移除项发送到另一过程，将包含项发送到另一过程。先前的示例将所有脏文档批量发送到适配器。

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

（可选）您可以跳过适配器，而直接联系 REST 端点。

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

##### 外部数据源：后端 REST 端点
{: #external-data-source-backend-rest-endpoint }
后端将接受或拒绝更改，然后将响应中继回客户机。客户机看到响应后，可以将更新的文档传递给 markClean API。

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

将文档标记为“干净”后，这些文档就不会显示在 `getAllDirty` API 的输出中。

## 对 JSONStore 进行故障诊断
{: #troubleshooting-jsonstore }

## 在寻求帮助时提供信息
{: #provide-information-when-you-ask-for-help }
请尽量提供更多信息，以免因提供的信息不足而造成风险。要获取帮助解决 JSONStore 问题所需的信息，以下列表是不错的起点。

* 操作系统和版本。例如，Windows XP SP3 虚拟机或 Mac OSX 10.8.3。
* Eclipse 版本。例如，Eclipse Indigo 3.7 Java EE。
* JDK 版本。例如，Java SE 运行时环境（构建 1.7）。
* {{ site.data.keys.product }} 版本。例如，IBM Worklight V5.0.6 Developer Edition。
* iOS 版本。例如，iOS Simulator 6.1 或 iPhone 4S iOS 6.0（不推荐，请参阅“不推荐使用的功能和 API 元素”）。
* Android 版本。例如，Android Emulator 4.1.1 或 Samsung Galaxy Android 4.0 API Level 14。
* Windows 版本。例如，Windows 8、Windows 8.1 或 Windows Phone 8.1。
* adb 版本。例如，Android Debug Bridge V1.0.31。
* 日志，例如 iOS 上的 Xcode 输出或 Android 上的 logcat 输出。

## 尝试隔离问题
{: #try-to-isolate-the-issue }
执行以下步骤来隔离问题，以便更准确地报告问题。

1. 重置仿真器 (Android) 或模拟器 (iOS) 并调用 destroy API 以从干净的系统开始。
2. 确保在受支持的生产环境中运行。
    * Android >= 2.3 ARM v7/ARM v8/x86 仿真器或设备
    * iOS >= 6.0 模拟器或设备（不推荐）
    * Windows 8.1/10 ARM/x86/x64 模拟器或设备
3. 尝试通过不将密码传递给 init 或 open API 来关闭加密。
4. 查看 JSONStore 生成的 SQLite 数据库文件。必须关闭加密。

   * Android 仿真器：

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * iOS 模拟器：

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Windows 8.1 Universal / Windows 10 UWP 模拟器

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   在 Web 浏览器（Firefox、Chrome、Safari、Internet Explorer）上运行的“仅 JavaScript”实现不使用 SQLite 数据库。该文件存储在 HTML5 LocalStorage 中。
    {: note}
   * 查看带有 `.schema` 的 `searchFields`，并通过 `SELECT * FROM <collection-name>;` 来选择数据。要退出 sqlite3，请输入 `.exit`。如果将用户名传递到 init 方法，那么该文件名为 **the-username.sqlite**。如果未传递用户名，那么缺省情况下该文件名为 **jsonstore.sqlite**。
5. （仅限 Android）启用详细 JSONStore。

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```
    {: codeblock}

6. 使用调试器。

## 常见问题
{: #common-issues-jsonstore }
了解以下 JSONStore 特征可帮助解决您可能遇到的一些常见问题。  

* 在 JSONStore 中存储二进制数据的唯一方法是首先以 Base64 格式对该数据进行编码。将文件名或路径（而不是实际文件）存储在 JSONStore 中。
* 只能在 {{ site.data.keys.v62_product_full }} V6.2.0 中从本机代码访问 JSONStore 数据。
* 除了移动操作系统施加的限制之外，对于可在 JSONStore 中存储的数据量没有限制。
* JSONStore 提供持久数据存储。这不只是存储在内存中。
* 如果集合名称以数字或符号开头，那么 init API 将失败。IBM Worklight V5.0.6.1 和更高版本会返回相应的错误：`4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* 在搜索字段中，数字与整数是有区别的。如果类型为 `number`，那么数字值 `1` 和 `2` 将存储为 `1.0` 和 `2.0`。如果类型为 `integer`，它们将存储为 `1` 和 `2`。
* 如果应用程序被强制停止或崩溃，那么在该应用程序重新启动并调用 `init` 或 `open` API 时，该应用程序始终会失败，并返回错误代码 -1。如果发生此问题，请首先调用 `closeAll` API。
* JSONStore 的 JavaScript 实现期望串行调用代码。请等待操作完成，然后再调用下一个操作。
* 在 Android 2.3.x 中，Cordova 应用程序不支持事务。
* 在 64 位设备上使用 JSONStore 时，您可能会看到以下错误：`java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* 此错误意味着您的 Android 项目中有 64 位本机库，但 JSONStore 当前无法在使用这些库的情况下运行。要进行确认，请转至 Android 项目下的 **src/main/libs** 或 **src/main/jniLibs**，并检查是否存在 x86_64 或 arm64-v8a 文件夹。如果存在，请删除这些文件夹，这样 JSONStore 便可以恢复运行。
* 在某些情况（或环境）下，此流程会在初始化 JSONStore 插件前进入 `wlCommonInit()`。这会导致 JSONStore 相关 API 调用失败。`cordova-plugin-mfp` 引导程序会自动调用 `WL.Client.init`，这样会在调用完成时触发 `wlCommonInit` 函数。对于 JSONStore 插件，此初始化过程会有所不同。JSONStore 插件无法_停止_ `WL.Client.init` 调用。根据环境不同，此流程可能在 `mfpjsonjslloaded` 完成之前进入 `wlCommonInit()`。
为确保 `mfpjsonjsloaded` 和 `mfpjsloaded` 事件的顺序正确，开发者可以选择手动调用 `WL.CLient.init`。这样可避免使用特定于平台的代码。

  使用以下步骤以配置为手动调用 `WL.CLient.init`：                             

  1. 在 `config.xml` 中，将 `clientCustomInit` 属性更改为 **true**。

  + 在 `index.js` 文件中：                                   
    * 在该文件开头添加以下行：                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * 将 `WL.JSONStore.init` 调用保留在 `wlCommonInit()` 中                    

    * 添加以下函数：  
    ```javascript                                         
function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }
    ```                                                                     

这会等待 `mfpjsonjsloaded` 事件（在 `wlCommonInit` 外部），确保已装入脚本，随后调用 `WL.Client.init` 以触发 `wlCommonInit`，然后调用 `WL.JSONStore.init`。

## 存储内部资源
{: #store-internals }
请参阅有关如何存储 JSONStore 数据的示例。

此简化示例中包含以下关键元素：



* `_id` 是唯一标识（例如，AUTO INCREMENT PRIMARY KEY）。
* `json` 包含已存储的 JSON 对象的确切表示法。
* `name` 和 age 是搜索字段。
* `key` 是额外搜索字段。

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |
{: caption="表 2. JSONStore 数据键元素示例" caption-side="top"}

使用 `{_id : 1}、{name: 'carlos'}`、`{age: 99}、{key: 'c'}` 查询之一或者这些查询的组合进行搜索时，返回的文档为 `{_id: 1,json: {name: 'carlos', age: 99} }`。

其他内部 JSONStore 字段包括：



* `_dirty`：确定文档是否标记为“脏”。此字段用于跟踪文档更改。

* `_deleted`：是否将文档标记为“已删除”。此字段用于从集合中除去对象，以后使用这些对象跟踪后端更改以及确定是否除去这些对象。

* `_operation`：用于反映要对文档执行的最后一个操作（例如，replace）的字符串。

## JSONStore 错误
{: #jsonstore-errors }
### JavaScript 错误
{: #javascript-errors }
JSONStore 使用 error 对象返回有关故障原因的消息。

在执行 JSONStore 操作（例如，`JSONStoreInstance` 类中的 `find` 和 `add` 方法）期间发生错误时，会返回一个 error 对象。该对象提供与故障原因有关的信息。



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
并非所有键/值对都属于每个 error 对象。例如，仅当操作因未能除去某个文档而失败（例如，`JSONStoreInstance` 类中的 `remove` 方法）时，doc 值才可用。

### Objective-C 错误
{: #objective-c-errors }
所有可能失败的 API 都会接受 error 参数，此参数采用 NSError 对象的地址。如果您不想收到错误通知，可以传入 `nil`。操作失败时，将使用 NSError（包含一个错误和一些可能的 `userInfo`）填充此地址。`userInfo` 可能包含额外的详细信息（例如，导致失败的文档）。

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java 错误
{: #java-errors }
根据发生的错误，所有 Java API 调用都会抛出某个特定异常。您可以分别处理每个异常，也可以捕获 `JSONStoreException` 以囊括所有 JSONStore 异常。

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### 错误代码列表
{: #list-of-error-codes }
常见错误代码及其描述的列表：

|错误代码| 描述|
|----------------|-------------|
| -100 UNKNOWN_FAILURE |无法识别的错误。|
| -75 OS\_SECURITY\_FAILURE |此错误代码与 requireOperatingSystemSecurity 标志相关。如果 destroy API 未能除去受操作系统安全性（带有密码备选项的 Touch ID）保护的安全性元数据，或者如果 init 或 open API 找不到安全性元数据，那么可能发生此错误。如果设备不支持操作系统安全性，但请求了使用操作系统安全性，那么也可能失败。|
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore 已关闭。请首先尝试调用 JSONStore 类中的 open 方法，以启用对存储器的访问。|
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK |回滚事务时发生问题。|
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |有事务正在处理时，无法调用 removeCollection。|
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY |有事务正在处理时，无法调用 destroy。|
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL |有事务存在时，无法调用 closeAll。|
| -44 TRANSACTION\_FAILURE\_DURING\_INIT |有事务正在处理时，无法初始化存储器。|
| -43 TRANSACTION_FAILURE |事务发生问题。|
| -42 NO\_TRANSACTION\_IN\_PROGRESS |没有事务正在处理时，无法落实回滚事务|
| -41 TRANSACTION\_IN\_POGRESS |正在处理某个事务时，无法启动另一个新事务。|
| -40 FIPS\_ENABLEMENT\_FAILURE |FIPS 有问题。|
| -24 JSON\_STORE\_FILE\_INFO\_ERROR |从文件系统中获取文件信息时发生问题。|
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE |替换集合中的文档时发生问题。|
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE |从集合中除去文档时发生问题。|
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE |存储数据保护密钥 (DPK) 时发生问题。|
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE |对输入数据建立索引时发生问题。|
| -12 INVALID\_SEARCH\_FIELD\_TYPES |检查要传递到 searchFields 的类型是 string、integer、number 还是 boolean。|
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT |对文档数组执行的操作（例如，replace 方法）在处理特定文档时可能失败。将返回失败的文档并回滚该事务。在 Android 上，尝试在不支持的体系结构上使用 JSONStore 时也会发生此错误。|
| -10 ACCEPT\_CONDITION\_FAILED |用户提供的 accept 函数返回 false。|
| -9 OFFSET\_WITHOUT\_LIMIT |要使用偏移量，还必须指定限制。|
| -8 INVALID\_LIMIT\_OR\_OFFSET |验证错误，必须是正整数。|
| -7 INVALID_USERNAME |验证错误（只能为 [A-Z]、[a-z] 或 [0-9]）。|
| -6 USERNAME\_MISMATCH\_DETECTED |要注销，JSONStore 用户必须首先调用 closeAll 方法。一次只能有一个用户。|
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |destroy 方法在尝试删除保存存储器内容的文件时发生问题。|
| -4 DESTROY\_REMOVE\_KEYS\_FAILED |destroy 方法在尝试清除钥匙串 (iOS) 或共享用户首选项 (Android) 时发生问题。|
| -3 INVALID\_KEY\_ON\_PROVISION |传递到已加密存储器的密码不正确。|
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH |搜索字段不是动态的。如果不在对新搜索字段调用 init 或 open 方法之前调用 destroy 方法或 removeCollection 方法，那么无法更改搜索字段。如果更改搜索字段的名称或类型，可能会发生此错误。例如：将 {key: 'string'} 更改为 {key: 'number'}，或者将 {myKey: 'string'} 更改为 {theKey: 'string'}。|
| -1 PERSISTENT\_STORE\_FAILURE |一般错误。本机代码中发生故障，最可能是因为调用 init 方法。|
| 0 SUCCESS |在某些情况下，JSONStore 本机代码会返回 0 以指示成功。|
| 1 BAD\_PARAMETER\_EXPECTED\_INT |验证错误。|
| 2 BAD\_PARAMETER\_EXPECTED\_STRING |验证错误。|
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION |验证错误。|
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING |验证错误。|
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT |验证错误。|
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT |验证错误。|
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT |验证错误。|
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |用于选择标记为“脏”的所有文档的查询失败。查询的 SQL 示例为：SELECT * FROM [collection] WHERE _dirty > 0。|
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION |要使用 JSONStoreCollection 类中的 push 和 load 方法之类的函数，必须将适配器传递到 init 方法。|
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS |验证错误|
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO |验证错误|
| 12 ADAPTER_FAILURE |调用 WL.Client.invokeProcedure 时发生问题，具体是连接到适配器时发生问题。此错误与尝试调用后端的适配器中发生的故障不同。|
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID |验证错误|
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS |不允许调用 JSONStoreCollection 类中的enhance 方法来替换现有函数（find 和 add）。|
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED |推送操作将文档发送到适配器，但 JSONStore 无法将文档标记为不脏。|
| 16 COULD\_NOT\_GET\_SECURE\_KEY |要使用密码启动集合，必须连接到 {{ site.data.keys.mf_server }}，因为它会返回“安全随机令牌”。IBM Worklight V5.0.6 和更高版本允许开发者本地生成安全随机令牌，并通过 options 对象将 {localKeyGen: true} 传递到 init 方法。|
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER |无法加载数据，因为 WL.Client.invokeProcedure 调用了失败回调。|
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ |传递到 init 方法的装入对象未通过验证。|
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT |调用 add 方法时，装入对象中使用的密钥发生问题。|
| 20 UNDEFINED\_PUSH\_OPERATION |未定义用于将脏文档推送到服务器的过程。例如：调用了 init 方法（新文档为脏文档，操作为“add”）和 push 方法（发现了新文档，操作为“add”），但未在链接到集合的适配器中找到添加密钥（含添加过程）。链接适配器是在 init 方法中执行的。 |
| 21 INVALID\_ADD\_INDEX\_KEY | 额外搜索字段发生问题。 |
| 22 INVALID\_SEARCH\_FIELD | 某一个搜索字段无效。请验证传入的搜索字段中是否不包括 _id、json_、deleted 或 _operation。|
| 23 ERROR\_CLOSING\_ALL |一般错误。本机代码调用 closeAll 方法时发生错误。 |
| 24 ERROR\_CHANGING\_PASSWORD | 无法更改密码。例如，传递了错误的旧密码。|
| 25 ERROR\_DURING\_DESTROY |一般错误。本机代码调用 destroy 方法时发生错误。|
| 26 ERROR\_CLEARING\_COLLECTION |一般错误。本机代码调用 removeCollection 方法时发生错误。|
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID |验证错误。|
| 28 INVALID\_SORT\_OBJECT |提供的用于排序的数组无效，因为其中一个 JSON 对象无效。正确的语法是 JSON 对象数组，其中每个对象仅包含单个属性。此属性将搜索作为排序依据的字段，并指定是升序还是降序。例如：{searchField1 : "ASC"}。 |
| 29 INVALID\_FILTER\_ARRAY | 提供的用于过滤结果的数组无效。此数组的正确语法是字符串数组，其中每个字符串是搜索字段或内部 JSONStore 字段。有关更多信息，请参阅“存储内部资源”。|
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS |数组不是仅包含 JSON 对象的数组时发生验证错误。|
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS |验证错误。|
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA |验证错误。|
{: caption="表 3. 常见错误代码" caption-side="top"}
