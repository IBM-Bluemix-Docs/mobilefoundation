---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Cordova 应用程序中的 Direct Update
{: #direct_update_cordova_apps}

可以通过 Direct Update 以*无线 (OTA)* 方式更新 Cordova 应用程序中的 Web 资源（JavaScript、HTML、CSS 或图像文件）。通过 Direct Update 功能，企业可以确保最终用户使用最新版本的应用程序。
要更新应用程序，需要使用 MobileFirst CLI 或通过部署生成的归档文件，将更新后的应用程序 Web 资源打包并上传到 MobileFirst 服务器。随后，系统将自动激活 Direct Update。此后，每次用户请求受保护资源时，都会强制执行 Direct Update。

Cordova iOS 和 Cordova Android 平台中支持 Direct Update。

对于开发和测试目的，开发者通常只需将归档上传到开发服务器就可以使用 Direct Update。此过程易于实施，但是并不安全。对于此阶段，会使用从嵌入的 MobileFirst 自签名证书中抽取的内部 RSA 密钥对。

但是，对于实时生产或预生产测试阶段，建议在将应用程序发布到应用程序商店之前实施安全 Direct Update。安全 Direct Update 需要从实际 CA 签名服务器证书中抽取的 RSA 密钥对。

* 请注意，在应用程序发布之后，不要修改密钥库配置。使用新的公用密钥重新配置应用程序并且重新发布应用程序后，才能对下载的更新进行认证。如果不执行上述这两个步骤，在客户机上执行 Direct Update 会失败。
*  Direct Update 仅更新应用程序的 Web 资源。要更新本机资源，必须将新的应用程序版本提交到相应的应用程序商店。
* 使用 Direct Update 功能并且启用了 Web 资源校验和功能后，每次 Direct Update 都会建立新的校验和库。
* 如果是使用修订包升级的 MobileFirst 服务器，那么该服务器将继续正常处理 Direct Update。但是，如果上传了最新构建的 Direct Update 归档（.zip 文件），那么可能会停止对较旧版本客户机的更新。原因是该归档包含 `cordova-plugin-mfp` 插件的版本。在将该归档提供给移动式客户机之前，服务器会将客户机版本与插件版本进行比较。如果两个版本足够接近（即三个最重要的数字相同），那么 Direct Update 会正常执行。否则，MobileFirst 服务器会以静默方式跳过更新。如果版本不匹配，一个解决方案是下载与原始 Cordova 项目中的 `cordova-plugin-mfp` 版本相同的 cordova-plugin-mfp，并重新生成 Direct Update 归档。
* 在最佳状况下，单个 MobileFirst 服务器能以 250 MB/秒的速度将数据推送到客户机。如果需要更高速度，请考虑使用集群或 CDN 服务。
{: tip}

## Direct Update 的工作原理
{: #working_direct_update}

应用程序 Web 资源初始与应用程序打包在一起，以首先确保脱机可用性。在此之后，应用程序将对向 MobileFirst 服务器发出的每一个请求检查更新。

>**注：**执行 Direct Update 后，会过 60 分钟再次检查更新。

![Direct Update 工作原理图](images/internal_function.jpg)

执行 Direct Update 后，应用程序不会再使用预打包的 Web 资源，而是改为使用从应用程序沙箱下载的 Web 资源。如果清除了设备上的应用程序高速缓存，将再次使用原始打包的 Web 资源。

>Direct Update 仅应用于特定版本。换言之，针对应用程序 V2.0 生成的更新不能应用到同一应用程序的其他版本中。

## 创建和部署更新后的 Web 资源
{: #creating_deploying_updates}

更新后的 Web 资源需要打包并上传到 MobileFirst 服务器。

1. 打开命令行窗口，并导航至 Cordova 项目的根目录。
2. 运行以下命令：
  ```
  mfpdev app webupdate
  ```
  {: pre}
`mfpdev app webupdate` 命令将更新后的 Web 资源打包成 `.zip` 文件，并将其上传到开发者工作站中运行的缺省 MobileFirst 服务器。可以在 `[cordova-project-root-folder]/mobilefirst/` 文件夹中找到打包的 Web 资源。

**替代步骤：**

* 构建 `.zip` 文件，并将其上传到其他 MobileFirst 服务器中：`mfpdev app webupdate [server-name] [runtime-name]`。例如：

  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* 上传先前生成的 `.zip` 文件：`mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`。
例如：
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* 将打包的 Web 资源手动上传到 MobileFirst 服务器：
  1. 构建 .zip 文件，但不上传：
```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. 装入 MobileFirst Operations Console，然后单击应用程序条目。
  3. 单击**上传 Web 资源文件**，以上传打包的 Web 资源。    
      ![通过控制台上传 Direct Update .zip 文件](images/upload-direct-update-package.png)

运行 `mfpdev help app webupdate` 命令以了解更多信息。
{: tip}

## 用户体验
{: #user_experience}

收到 Direct Update，缺省情况下会显示一个对话框，并且系统会要求用户具有启动更新过程的许可权。用户核准进度条之后，将显示一个对话框并且会下载 Web 资源。更新完成后，将自动重新装入应用程序。

![Direct Update 示例](images/direct-update-flow.png)

## 定制 Direct Update UI
{: #customize_du_ui}

可以定制向最终用户显示的 Direct Update UI。
在 **index.js** 的 `wlCommonInit()` 函数中添加以下内容：
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData* 是包含 downloadSize 属性的 JSON 对象，该属性表示要从 MobileFirst 服务器下载的更新包的文件大小（以字节为单位）。
*directUpdateContext* 是公开 .start() 和 .stop() 函数的 JavaScript 对象，这两个函数分别用于启动和停止 Direct Update 流程。

如果 MobileFirst 服务器上的 Web 资源比应用程序中的 Web 资源更新，那么会将 Direct Update 质询数据添加到服务器响应中。MobileFirst 客户机端框架每次检测到此 Direct Update 质询时，都将调用 `wl_directUpdateChallengeHandler.handleDirectUpdate` 函数。

该函数提供缺省的 Direct Update 设计：在 Direct Update 可用时显示的缺省消息对话框，以及启动 Direct Update 进程时显示的缺省进度屏幕。您可以实现定制的 Direct Update 用户界面行为，或者通过覆盖此函数并实现您自己的逻辑来定制 Direct Update 对话框。

在下面的示例代码中，`handleDirectUpdate` 函数在 Direct Update 对话框中实现定制消息。将此代码添加到 Cordova 项目的 `www/js/index.js` 文件中。
定制 Direct Update UI 的其他示例：
* 使用第三方 JavaScript 框架（例如，Dojo 或 jQuery Mobile、Ionic...）创建的对话框
* 执行 Cordova 插件的完全本机 UI
* 向用户显示的备用 HTML 以及选项等。

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Creates a dialog.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

用户单击对话框按钮时，您随即可以通过运行 `directUpdateContext.start()` 方法启动 Direct Update 进程。这将显示缺省进度屏幕，该屏幕与 MobileFirst 服务器先前版本中显示的屏幕相似。

此方法支持以下调用类型：
* 未指定参数时，MobileFirst 服务器使用缺省进度屏幕。
* 提供侦听器函数（如 `directUpdateContext.start(directUpdateCustomListener)`）时，Direct Update 进程在后台运行，同时该进程会向侦听器发送生命周期事件。定制的侦听器必须实现以下方法：

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

Direct Update 进程执行期间，根据以下规则启动侦听器方法：
* 使用保存更新文件大小的 `totalSize` 参数调用 `onStart`。
* 使用状态 `DOWNLOAD_IN_PROGRESS`、`totalSize` 和 `completedSize`（到目前为止的下载量）多次调用 `onProgress`。
* 使用状态 `UNZIP_IN_PROGRESS` 调用 `onProgress`。
* 使用以下其中一个最终状态码调用 `onFinish`：

| 状态码 | 描述|
|:------------|:------------|
| `SUCCESS`|Direct Update 完成，没有任何错误。|
| `CANCELED`|Direct Update 已取消（例如，因为调用了 `stop()` 方法）。|
| `FAILURE_NETWORK_PROBLEM`|更新期间网络连接发生问题。|
| `FAILURE_DOWNLOADING`|文件没有完全下载。|
| `FAILURE_NOT_ENOUGH_SPACE`|设备上没有足够的空间来下载和解包更新文件。|
| `FAILURE_UNZIPPING`|解包更新文件时发生问题。|
| `FAILURE_ALREADY_IN_PROGRESS`|Direct Update 已经在运行时调用了启动方法。|
| `FAILURE_INTEGRITY`| 无法验证更新文件的真实性。|
| `FAILURE_UNKNOWN`|意外内部错误。|

如果您实现了定制的 Direct Update 侦听器，那么必须确保在 Direct Update 进程完成并且已调用了 `onFinish()` 方法时，重新装入应用程序。如果 Direct Update 进程未能成功完成，那么还必须调用 `wl_directUpdateChalengeHandler.submitFailure()`。

以下示例显示定制 Direct Update 侦听器的实现：
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //show custom error message

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

### 场景：运行无 UI Direct Update
{: #scenario-running-ui-less-direct-updates }
应用程序位于前台时，{{site.data.keyword.mobilefoundation_short}} 支持无 UI Direct Update。

要运行无 UI Direct Update，请实现 `directUpdateCustomListener`。向 `onStart` 和 `onProgress` 方法提供空的函数实现。空实现会使 Direct Update 进程在后台运行。

要完成 Direct Update 进程，必须重新装入应用程序。提供了以下选项：
* `onFinish` 方法也可以为空。在这种情况下，应用程序重新启动后将应用 Direct Update。
* 可以实现用于通知或要求用户重新启动应用程序的定制对话框。（请参阅以下示例。）
* `onFinish` 方法可以通过调用 `WL.Client.reloadApp()` 强制重新装入应用程序。

下面是 `directUpdateCustomListener` 实现的示例：

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

实现 `wl_directUpdateChallengeHandler.handleDirectUpdate` 函数。将已创建的 `directUpdateCustomListener` 实现作为参数传递给该函数。确保调用了 `directUpdateContext.start(directUpdateCustomListener)`。下面是 `wl_directUpdateChallengeHandler.handleDirectUpdate` 实现的示例：

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

**注：**应用程序转到后台时，Direct Update 进程会暂挂。

### 场景：处理 Direct Update 故障
{: #scenario-handling-a-direct-update-failure }
此场景显示如何处理由连接丢失等原因导致的 Direct Update 故障。在此场景中，即使在脱机方式下也阻止用户使用应用程序。这将显示一个对话框，向用户提供重试选项。

创建全局变量以存储 Direct Update 上下文，以便可以在日后 Direct Update 进程失败时使用。例如：

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

实现 Direct Update 质询处理程序。在此保存 Direct Update 上下文。例如：

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  savedDirectUpdateContext = directUpdateContext; // save direct update context

  var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

  WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
    text : WL.ClientMessages.update,
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

使用 Direct Update 上下文创建用于启动 Direct Update 进程的函数。例如：

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
```
{: codeblock}

实现 `directUpdateCustomListener`。在 `onFinish` 方法中添加状态检查。如果状态一开始为 `FAILURE`，请使用**重试**选项打开仅模态对话框。例如：

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
      WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
        text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
  }
};
```
{: codeblock}

用户单击**重试**按钮后，应用程序会重新启动 Direct Update 进程。

## 增量和完全 Direct Update
{: #delta-and-full-direct-update }
增量 Direct Update 支持应用程序仅下载自上次更新后更改的文件，而不是下载应用程序的整个 Web 资源。这将缩短下载时间，节省带宽，并且改进总体用户体验。

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span>**重要信息：**仅当客户机应用程序的 Web 资源比服务器上当前部署的应用程序低一个版本时，才能执行**增量更新**。如果客户机应用程序比当前部署的应用程序低不止一个版本（即更新客户机应用程序后，该应用程序至少部署到服务器两次），请执行**完全更新**（即下载并更新了整个 Web 资源）。


## 样本应用程序
{: #sample-application }
[单击以下载 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80) Cordova 项目。  

### 样本用法
{: #sample-usage }
访问样本的 README.md 文件以获取指示信息。

## CDN 支持
{: #cdn_support}

您可以配置为通过 CDN（内容交付网络）而不是 MobileFirst 服务器来处理 Direct Update 请求。

### 使用 CDN 的优势
{: #advantages-of-using-a-cdn }
使用 CDN 而不是 MobileFirst 服务器来处理 Direct Update 请求具有以下优势：

* 除去了 MobileFirst 服务器的网络开销。
* 提高了传输速率，传输速率高于处理来自 MobileFirst 服务器的请求时 250 MB/秒的限制。
* 确保所有用户无论其地理位置在哪里，都可以获得更统一的 Direct Update 体验。

### 一般需求
{: #general-requirements }
要通过 CDN 处理 Direct Update 请求，请确保配置符合以下条件：

* CDN 必须是 MobileFirst 服务器前端或其他逆向代理服务器前端（如果需要）的逆向代理。
* 在开发环境中构建应用程序时，将目标服务器设置为 CDN 主机和端口，而不设置为 MobileFirst 服务器的主机和端口。例如，运行 MobileFirst CLI 命令 `mfpdev server add` 时，提供 CDN 主机和端口。
* 在 CDN 管理面板上，需要标记以下 Direct Update URL 以进行高速缓存，从而确保 CDN 将 Direct Update 请求之外的其他所有请求传递给 MobileFirst 服务器。对于 Direct Update 请求，CDN 会确定其是否包含内容。如果包含内容，CDN 会返回内容，而不转至 MobileFirst 服务器；如果不包含内容，CDN 会转至 MobileFirst 服务器，获取 Direct Update 归档（.zip 文件），并存储该归档以用于该特定 URL 的下一次请求。对于使用 {{site.data.keyword.mobilefoundation_short}} V8.0 构建的应用程序，Direct Update URL 为：`PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`。
`PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` 前缀是用于所有运行时请求的常量。例如：`http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

在此示例中，还存在其他请求参数，这些参数也是请求的一部分。

* CDN 必须允许对请求参数进行高速缓存。两个不同 Direct Update 归档可能唯一的区别是请求参数不同。
* CDN 必须在 Direct Update 响应上支持 TTL。需要支持 TTL 才能对同一版本执行多次 Direct Update。
* CDN 不能更改或除去服务器/客户机协议中使用的 HTTP 头。

### 示例 CDN 配置
{: #example-cdn-configuration }
此示例使用 Akamai CDN 配置来高速缓存 Direct Update 归档。以下任务由网络管理员、MobileFirst 管理员和 Akamai 管理员来完成：

#### 网络管理员
{: #network-administrator }
在 DNS 中为 MobileFirst 服务器创建其他域。例如，如果您的服务器域为 `yourcompany.com`，那么您需要创建其他域，例如 `cdn.yourcompany.com`。
在新的 `cdn.yourcompany.com` 域的 DNS 中，将 `CNAME` 设置为 Akamai 提供的域名。例如，`yourcompany.com.akamai.net`。

#### MobileFirst 管理员
{: #mobilefirst-administrator }
将新的 `cdn.yourcompany.com` 域设置为 MobileFirst 应用程序的 MobileFirst 服务器 URL。例如，对于 Ant 构建器任务，该属性为：
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Akamai 管理员
{: #akamai-administrator }
1. 打开 Akamai 属性管理器，将**主机名**属性设置为新域的值。

    ![将“主机名”属性设置为新域的值](images/direct_update_cdn_3.jpg)

2. 在“缺省规则”选项卡上，配置原始 MobileFirst 服务器主机和端口，并将**定制转发主机头**值设置为新创建的域。

    ![将“定制转发主机头”值设置为新创建的域](images/direct_update_cdn_4.jpg)

3. 从**高速缓存选项**列表中，选择**无存储**。

    ![从“高速缓存选项”列表中，选择“无存储”](images/direct_update_cdn_5.jpg)

4. 在**静态内容配置**选项卡中，根据应用程序的 Direct Update URL 来配置匹配条件。例如，创建条件：`IF“路径”“匹配以下其中一项”direct_update_URL`。

    ![根据应用程序的 Direct Update URL 来配置匹配条件](images/direct_update_cdn_6.jpg)

5. 配置高速缓存键行为，以在高速缓存键中使用所有请求参数（您必须执行此操作，这样才能为不同的应用程序或版本高速缓存不同的 Direct Update 归档）。例如，从**行为**列表中，选择`包含所有参数（保留请求的顺序）`。

    ![配置高速缓存键行为，以在高速缓存键中使用所有请求参数](images/direct_update_cdn_8.jpg)

6. 将值设置为与以下值类似，以配置高速缓存行为来高速缓存 Direct Update URL 并设置 TTL。

      ![设置值以配置高速缓存行为](images/direct_update_cdn_7.jpg)

|字段|值|
|:------|:------|
|高速缓存选项|高速缓存|
|强制重新验证旧对象|无法验证时提供旧对象|
|最长时效|3 分钟|


## 安全 Direct Update
{: #secure-dc }

安全 Direct Update 在缺省情况下禁用。安全 Direct Update 可防止第三方攻击者更改从 MobileFirst 服务器（或从内容交付网络 (CDN)）传输到客户机应用程序的 Web 资源。

**启用 Direct Update 真实性：**  
使用首选工具，从 MobileFirst 服务器密钥库中抽取公用密钥，并将其转换为 Base64。  
然后，应该如下所示使用生成的值：

1. 打开**命令行**窗口并导航至 Cordova 项目的根目录。
2. 运行 `mfpdev app config` 命令，并选择 **Direct Update 真实性公用密钥**选项。
3. 提供公用密钥并确认。

未来向客户机应用程序进行的任何 Direct Update 交付都将通过 Direct Update 真实性加以保护。

要使安全 Direct Update 发挥作用，必须在 MobileFirst 服务器中部署用户定义的密钥库文件，并且匹配的公用密钥的副本必须包含在已部署的客户机应用程序中。

此主题描述如何将公用密钥绑定到新的客户机应用程序和升级后的现有客户机应用程序。有关在 MobileFirst 服务器中配置密钥库的更多信息，请参阅
[配置 MobileFirst 服务器密钥库 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

服务器提供的内置密钥库可用于为开发阶段测试安全 Direct Update。

>**注：**将公用密钥绑定到客户机应用程序并重新构建之后，无需将其重新上传到 {{ site.data.keys.mf_server }}。但是，如果您先前向市场发布应用程序时没有使用公用密钥，那么必须重新发布该公用密钥。

缺省情况下，MobileFirst 服务器提供有以下哑元公用密钥以用于开发目的：

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

**重要信息：**不要将公用密钥用于生产目的。

### 生成和部署密钥库
{: #generating-and-deploying-the-keystore }
有许多工具可用于通过密钥库生成证书以及从密钥库中抽取公用密钥。以下示例演示了使用 JDK 密钥工具实用程序和 openSSL 的过程。

1. 从 {{ site.data.keys.mf_server }} 中部署的密钥库文件中抽取公用密钥。  
   >**注：**公用密钥必须为 Base64 编码。

   例如，假定别名为 `mfp-server`，密钥库文件为 **keystore.jks**。  
   要生成证书，请发出以下命令：

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   这将生成证书文件。  
   发出以下命令抽取公用密钥：

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   > **注：**不能单独使用密钥工具来抽取 Base64 格式的公用密钥。

2. 执行以下其中一个过程：
    * 将生成的文本（不包括 `BEGIN PUBLIC KEY` 和 `END PUBLIC KEY` 标记）复制到应用程序的 mfpclient 属性文件的 `wlSecureDirectUpdatePublicKey` 之后。
    * 从命令提示符中，发出以下命令：`mfpdev app config direct_update_authenticity_public_key <public_key>`

    对于 `<public_key>`，粘贴步骤 1 中生成的文本，不包括 `BEGIN PUBLIC KEY` 和 `END PUBLIC KEY` 标记。

3. 运行 cordova build 命令将公用密钥保存在应用程序中。
