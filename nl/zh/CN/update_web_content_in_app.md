---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Direct Update
{: #direct_update}

可以通过 **Direct Update** 以无线 (OTA) 方式更新 Cordova 或 Ionic 应用程序中的 Web 资源（JavaScript、HTML、CSS 或图像文件），而无需用户执行 App Store 或 Play Store 更新。通过 Direct Update 功能，企业可以确保最终用户使用最新版本的应用程序。要更新应用程序，需要使用 Mobile Foundation CLI 或通过部署生成的归档文件，将更新后的应用程序 Web 资源打包并上传到 Mobile Foundation 实例。随后，系统将自动激活 Direct Update。此后，每次用户请求受保护资源时，都会强制执行 Direct Update。

![Direct Update 工作原理图](images/internal_function.jpg)

Cordova iOS 和 Cordova Android 平台中支持 Direct Update。
{: note}

对于实时生产或预生产测试阶段，建议在将应用程序发布到应用程序商店之前实施安全 Direct Update。安全 Direct Update 需要从实际 CA 签名服务器证书中抽取的 RSA 密钥对。

1. 请注意，在应用程序发布之后，不要修改密钥库配置。使用新的公用密钥重新配置应用程序并且重新发布应用程序后，才能对下载的更新进行认证。如果不执行前述两个步骤，在客户机上执行 Direct Update 会失败。
2.  Direct Update 仅更新应用程序的 Web 资源。要更新本机资源，必须将新的应用程序版本提交到相应的应用程序商店。
3. 使用 Direct Update 功能并且启用了 Web 资源校验和功能后，每次 Direct Update 都会建立新的校验和库。
4. 如果升级了 Mobile Foundation 服务器，那么该服务器将继续正常处理 Direct Update。但是，如果上传了最新构建的 Direct Update 归档（.zip 文件），那么可能会停止对较旧版本客户机的更新。原因是该归档包含 `cordova-plugin-mfp` 插件的版本。在将该归档提供给移动式客户机之前，服务器会将客户机版本与插件版本进行比较。如果两个版本足够接近（即三个最重要的数字相同），那么 Direct Update 会正常执行。否则，Mobile Foundation 服务器会以静默方式跳过更新。如果版本不匹配，一个解决方案是下载与原始 Cordova 项目中的 `cordova-plugin-mfp` 版本相同的 cordova-plugin-mfp，并重新生成 Direct Update 归档。
{: tip}

执行 Direct Update 后，应用程序不会再使用预打包的 Web 资源，而是改为使用从应用程序沙箱下载的 Web 资源。如果清除了设备上的应用程序高速缓存，将再次使用原始打包的 Web 资源。

Web 资源的 Direct Update 仅适用于特定版本的应用程序。例如，为应用程序 V2.0 生成的更新不会交付给同一应用程序的 V2.1。
{: note}

## 创建和部署更新后的 Web 资源
{: #creating_deploying_updates}

对 Web 资源进行更改后，可以使用 Mobile Foundation CLI 将其打包并上传到 Mobile Foundation 实例。

1.  Direct Update 仅更新应用程序的 Web 资源。要更新本机资源，必须将新的应用程序版本提交到相应的应用程序商店。
2. 如果应用程序的本机部分与上传到 Mobile Foundation 服务的部分明显不同，那么 Mobile Foundation 服务会停止对客户机执行 Direct Update。将 *cordova-plugin-mfp* 插件更新为较新版本时，可能会发生这种情况。在将归档提供给移动式客户机之前，服务器会将客户机版本与插件版本进行比较。如果两个版本足够接近（即版本标识中三个最重要的数字相同），那么 Direct Update 会正常执行。否则，Mobile Foundation 服务器会以静默方式跳过更新。如果版本不匹配，一个解决方案是下载与原始 Cordova 项目中的 *cordova-plugin-mfp* 版本相同的 cordova-plugin-mfp，并重新生成 Direct Update 归档。
{: tip}

1. 打开命令行窗口，并导航至 Cordova 项目的根目录。
2. 运行以下命令：
  ```bash
  mfpdev app webupdate
  ```
  `mfpdev app webupdate` 命令将更新后的 Web 资源打包成 `.zip` 文件，并将其上传到开发者工作站中运行的缺省 Mobile Foundation 服务器。可以在 `[cordova-project-root-folder]/mobilefirst/` 文件夹中找到打包的 Web 资源。

有关在应用程序中更新 Web 内容的备用步骤，请参阅[此处](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app)。

## Direct Update 高级配置
{: #direct_update_advanced_config}

有关 Direct Update 配置的高级主题，请参阅[此处](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration)。
