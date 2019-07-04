---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: command line, cli, mfpdev-cli, cli commands

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}
{:note: .note}

# Mobile Foundation CLI
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }} 提供了命令行界面 (CLI) 工具，供开发者 **mfpdev** 轻松管理 {{site.data.keyword.mobilefoundation_short}} 客户机和服务器工件。

所有 `mfpdev` 命令可以通过交互或直接方式运行。在交互方式下，系统将提示您输入命令所需的参数，并且将使用部分缺省值。在直接方式下，参数必须随命令一起提供。


## 安装 {{site.data.keyword.mobilefoundation_short}} CLI
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} 在 [NPM 注册表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.npmjs.com){: new_window} 中作为 NPM 软件包提供。

确保安装了 **node.js** 和 **npm** 以安装 NPM 包。请遵循 [nodejs.org ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://nodejs.org/){: new_window} 中的安装指示信息来安装 **node.js**。要确认 **node.js** 是否已正确安装，请运行以下命令：
```bash
node -v
```
{: codeblock}

支持的最低 node.js 版本为 **4.2.3**。此外，在 Node 和 NPM 软件包快速更新时，{{site.data.keyword.mobilefoundation_short}} CLI 可能无法在所有可用的 Node 和 NPM 版本（包括最新版本）上以全功能运行。为了使 CLI 正常运行，请确保 Node 的版本为 **6.11.1**，NPM 的版本为 **3.10.10**。
{: note}

对于 MobileFirst CLI 临时修订版本 *8.0.2018100112* 和更高版本，可以使用 Node V8.x 或 V10.x。

* 要安装 {{site.data.keyword.mobilefoundation_short}} CLI，请运行以下命令：
    ```bash
    npm install -g mfpdev-cli
    ```
    {: codeblock}

* 如果已从 MobileFirst Operations Console 的下载中心下载 CLI 压缩文件 (*.zip*)，请使用以下命令：
    ```bash
    npm install -g <path-to-mfpdev-cli.tgz>
    ```
    {: codeblock}

* 要安装不包含可选依赖项的 CLI，请添加 `--no-optional` 标志：
    ```bash
    npm install -g --no-optional path-to-mfpdev-cli.tgz
    ```
    {: codeblock}

使用 Node 8 安装 MobileFirst CLI 时，可能会在终端窗口中看到下面的其中一些错误：
```text
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```
{: codeblock}

导致此错误的原因是 [node-gyp 中的已知错误 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}。可以忽略这些错误，因为它们不会影响 MobileFirst CLI 正常运行。此问题适用于 `mfpdev-cli` 临时修订级别 *8.0.2018100112* 和更高版本。要解决此错误，请在安装期间使用 `--no-optional` 标志。

* 要确认 CLI 是否正确安装，请运行以下命令：
    ```bash
    mfpdev
    ```
    {: codeblock}

* CLI 帮助将作为输出显示。

    ```
     NAME
         IBM MobileFirst Foundation Command Line Interface (CLI).

     SYNOPSIS
         mfpdev <command> [options]

     DESCRIPTION
         The IBM MobileFirst Foundation Command Line Interface (CLI) is a command-line
         for developing MobileFirst applications. The command-line can be used by itself, or in conjunction  with the IBM MobileFirst Foundation Operations Console. Some functions are available from  the command-line only and not the console.

         For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of IBM MobileFirst Foundation at
         https://www.ibm.com/support/knowledgecenter.
         ...
         ...
         ...
    ```
    {: screen}

## {{site.data.keyword.mobilefoundation_short}} CLI 命令列表
{: #list_cli_commands}

<table summary="具有链接的 {{site.data.keyword.mobilefoundation_short}} CLI 命令，通过链接您可了解命令的更多信息">
<caption>表 1. {{site.data.keyword.mobilefoundation_short}} CLI 命令</caption>
 <thead>
 <th colspan="6">常规 [mfpdev](#mfpdev) 命令</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[server](#mfpdev_server)</td>
 <td>[adapter](#mfpdev_adapter)</td>
 <td>[help](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### mfpdev
{: #mfpdev}

为 mfpdev 命令行界面设置预览浏览器类型、预览超时值和服务器超时值的配置首选项。

```bash
mfpdev config
```
{: codeblock}

显示有关您环境的信息，包括操作系统、内存消耗量、Node 版本以及命令行界面版本。如果当前目录为 Cordova 应用程序，那么还将显示 Cordova 的 `cordova info` 命令提供的信息。

```bash
mfpdev info
```
{: codeblock}

显示当前使用的 {{site.data.keyword.mobilefoundation_short}} CLI 的版本号。

```bash
mfpdev -v
```
{: codeblock}

调试方式：生成调试输出。

```bash
mfpdev [-d|--debug]
```
{: codeblock}

详细调试方式：生成详细调试输出。

```bash
mfpdev [-dd|--ddebug]
```
{: codeblock}

禁止命令输出中使用彩色。

```bash
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

显示 MobileFirst CLI (mfpdev) 命令的帮助。使用命令名作为自变量时，将显示每种命令类型或每个命令的更具体帮助文本。例如，`mfpdev help server add`。

```bash
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

向 MobileFirst 服务器注册应用程序。

```bash
mfpdev app register
```
{: codeblock}

要向非缺省服务器和运行时注册应用程序，请使用以下命令：

```bash
mfpdev app register <server> <runtime>
```
{: codeblock}

对于 Cordova Windows 平台，必须在命令中添加 `-w <platform>` 自变量。platform 自变量是要向其注册的 Windows 平台的逗号分隔列表。有效值为 windows、windows8 和 windowsphone8。

```bash
mfpdev app register -w windows8
```
{: codeblock}

您可以指定要用于应用程序的后端服务器和运行时。对于 Cordova 应用程序，使用此命令可配置其他若干方面，例如系统消息的缺省语言，以及是否执行校验和安全性检查。还包含 Cordova 应用程序的其他配置参数。

```bash
mfpdev app config
```
{: codeblock}

从服务器中检索现有应用程序配置。

```bash
mfpdev app pull
```
{: codeblock}

将应用程序配置发送到服务器。

```bash
mfpdev app push
```
{: codeblock}

预览 Cordova 应用程序，无需目标平台类型的实际设备。您可以在移动浏览器模拟器或 Web 浏览器中查看预览。

```bash
mfpdev app preview
```
{: codeblock}

将 www 目录中提供的应用程序资源打包成压缩 (*.zip*) 文件，以便可用于 Direct Update 进程。

```bash
mfpdev app webupdate
```
{: codeblock}

此命令将更新后的 Web 资源打包成压缩 (*.zip*) 文件，并将其上传到注册的缺省 MobileFirst 服务器。可以在 `[cordova-project-root-folder]/mobilefirst/` 文件夹中找到打包的 Web 资源。

要将 Web 资源上传到其他服务器实例中，请在命令中提供相应的服务器名称和运行时：

```bash
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

可以使用 -build 参数来生成具有打包 Web 资源的压缩 (*.zip*) 文件，但不将其上传到服务器。
```bash
mfpdev app webupdate --build
```
{: codeblock}

要上传先前构建的包，请使用 -file 参数：
```bash
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

此外，还可以选择使用 `–encrypt` 参数来加密包的内容：
```bash
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

显示有关 MobileFirst 服务器的信息。

```bash
mfpdev server info
```
{: codeblock}

将服务器定义添加到您的环境。

```bash
mfpdev server add
```
{: codeblock}

编辑服务器定义。

```bash
mfpdev server edit
```
{: codeblock}

要将服务器设置为缺省服务器，请使用以下命令：

```bash
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

从环境中除去服务器定义。

```bash
mfpdev server remove
```
{: codeblock}

打开 MobileFirst Operations Console。

```bash
mfpdev server console
```
{: codeblock}

要打开其他服务器的控制台，请提供服务器名称作为命令的参数：

```bash
mfpdev server console <server-name>
```
{: codeblock}

从 MobileFirst 服务器中注销应用程序并除去适配器。

```bash
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

创建适配器。

```bash
mfpdev adapter create
```
{: codeblock}

构建适配器。

```bash
mfpdev adapter build
```
{: codeblock}

查找并构建当前目录及其子目录下的所有适配器。

```bash
mfpdev adapter build all
```
{: codeblock}

将适配器部署到 MobileFirst 服务器。

```bash
mfpdev adapter deploy
```
{: codeblock}

要部署到其他服务器，请使用以下命令：
```bash
mfpdev adapter deploy <server_name>
```
{: codeblock}

查找当前目录及其子目录下的所有适配器，并将其部署到 MobileFirst 服务器。

```bash
mfpdev adapter deploy all
```
{: codeblock}

调用 MobileFirst 服务器上的适配器程序。

```bash
mfpdev adapter call
```
{: codeblock}

从服务器中检索现有适配器配置。
```bash
mfpdev adapter pull
```
{: codeblock}

将适配器配置发送到服务器。
```bash
mfpdev adapter push
```
{: codeblock}

## 更新和卸载 {{site.data.keyword.mobilefoundation_short}} CLI
{: #update_uninstall_mf_cli}

要更新命令行界面，请运行以下命令：

```bash
npm update -g mfpdev-cli
```
{: codeblock}

要卸载命令行界面，请运行以下命令：

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
