---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content in apps, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 在应用程序中更新 Web 内容的备用步骤
{: #alternate_steps_to_update_app_web_content_in_app}

查看以下有关更新应用程序中 Web 内容的备用方法的列表。

* 构建 `.zip` 文件，并将其上传到其他 Mobile Foundation 服务器中：`mfpdev app webupdate [server-name] [runtime-name]`。例如：
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* 上传先前生成的 `.zip` 文件：`mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`。
例如：
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* 将打包的 Web 资源手动上传到 Mobile Foundation 服务器：
  1. 构建 .zip 文件，但不上传：
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. 装入 Mobile Foundation Operations Console，然后单击应用程序条目。
  3. 单击**上传 Web 资源文件**，以上传打包的 Web 资源。    
      ![通过控制台上传 Direct Update .zip 文件](images/upload-direct-update-package.png "通过控制台上传 Direct Update .zip 文件，其中“上传 Web 资源文件”按钮突出显示")

运行 `mfpdev help app webupdate` 命令以了解更多信息。
{: tip}
