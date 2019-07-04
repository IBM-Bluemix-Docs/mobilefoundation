---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: obfuscating resources, security

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# 对资源进行模糊处理
{: #obfuscate_resources}

模糊处理是修改可执行文件的过程，目的是使该文件对黑客不再有用，但仍能完全正常运行。自动代码模糊处理使程序的反向工程变得困难。

模糊处理之后，对应用程序进行反向工程会困难得多，这样就可防止商业秘密（知识产权）盗窃、未经授权的访问、绕过许可或其他控制措施以及防止漏洞。

代码模糊处理包含多种不同的方法和应用程序安全方法：

* 重命名模糊处理 - 重命名会更改方法和变量的名称，使反编译源更难让人理解，但不会改变应用程序执行。这是 .NET、iOS、Java 和 Android 模糊处理器进行模糊处理的首选方法。
* 字符串加密
* 控制流模糊处理
* 指令模式变换
* 哑元代码插入
* 防篡改，等等。

通过模糊处理，攻击者要查看代码和分析应用程序就会困难得多。对于模糊处理，提供各种第三方工具。

* 有关 Android 应用程序模糊处理的信息，请参阅此[博客](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/)。
    

  您需要创建配置文件 (proguard-project.txt)，供 Android ProGuard 对 MobileFirst Android 应用程序进行模糊处理。
  {: note}

* 有关 Cordova 应用程序基本模糊处理的信息，请参阅[加密 Cordova 应用程序](#encryptingcordovapackage)。

## 加密 Cordova 包的 Web 资源
{: #encryptingcordovapackage}

要将某人查看和修改 `.apk` 或 `.ipa` 包中 Web 资源的风险降至最低，可以使用以下 MobileFirst CLI 命令：
```bash
mfpdev app webencrypt
```
{: codeblock}
或者使用 `mfpwebencrypt` 标志来加密信息。此过程并未提供无法破解的加密，但提供了基本级别的模糊处理。

### 先决条件

* 必须安装 Cordova 开发工具。此示例使用的是 Apache Cordova CLI。如果您使用的是其他 Cordova 开发工具，那么某些步骤会有所不同。请参阅 Cordova 工具文档以获取指示信息。
* 必须安装 MobileFirst CLI。
* 必须安装 MobileFirst Cordova 插件。

完成此过程的最佳时间是已经完成应用程序开发并准备好部署应用程序时。如果在完成 Web 资源加密过程后运行以下任一命令，那么加密的内容将会解密。

* `cordova prepare`
* `cordova build`
* `cordova run`
* `cordova emulate`
* `mfpdev app webupdate`
* `mfpdev app preview
`

如果在加密 Web 资源后运行以下列出的其中一个命令，那么必须再次完成此过程来对 Web 资源进行加密。

1. 打开终端窗口，并导航至要加密的 Cordova 应用程序的根目录。
2. 通过输入以下其中一个命令来准备应用程序：
    ```bash
      cordova prepare
      ```
    {: codeblock}
    ```bash
      mfpdev app webupdate
      ```
    {: codeblock}
3. 完成以下其中一个过程以加密内容：
    * 输入以下命令：
      ```bash
      mfpdev app webencrypt
      ```
      {: codeblock}

      您可以通过输入以下命令查看有关 `mfpdev app webencrypt` 命令的信息：
      ```bash
      mfpdev help app webencrypt
      ```
      {: codeblock}
      {: tip}

    * 在构建包时，还可以通过将 `mfpwebencrypt` 标志添加到 `cordova compile` 或 `cordova build` 命令，以加密 Cordova 包的 Web 资源。
        ```bash
         cordova compile -- --mfpwebencrypt
         ```
         {: codeblock}
         或者
         ```bash
         cordova build -- --mfpwebencrypt
         ```
         {: codeblock}
    包含加密内容的 **resources.zip** 文件将替换 **www** 文件夹中的操作系统信息。如果应用程序是用于 Android 操作系统，并且 **resources.zip** 文件大于 1 MB，那么 **resources.zip** 文件会分为更小的 768 KB .zip 文件，名为 **resources.zip.nnn**。变量 nnn 是从 001 到 999 的数字。
4. 使用特定于平台的工具随附的仿真器可测试包含加密资源的应用程序。例如，可以使用 Android Studio（对于 Android）或 Xcode（对于 iOS）中的仿真器。

请勿在加密应用程序后使用以下 Cordova 命令来测试应用程序：
```bash
cordova run
```
{: codeblock}
```bash
cordova emulate
```
{: codeblock}
这些命令会刷新 www 文件夹中已加密的内容，并将其重新保存为解密的内容。如果使用这些命令，请务必在发布应用程序之前，再次完成该过程以对其进行加密。
{: note}
