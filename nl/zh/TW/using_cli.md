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

{{ site.data.keyword.mobilefoundation_short }} 提供「指令行介面 (CLI)」工具，讓開發人員 **mfpdev** 可以輕鬆地管理 {{site.data.keyword.mobilefoundation_short}} 用戶端及伺服器構件。

所有 `mfpdev` 指令都可以在互動或直接模式下執行。在互動模式下，會提示輸入指令所需的參數，並使用一些預設值。在直接模式下，必須提供參數與指令。


## 安裝 {{site.data.keyword.mobilefoundation_short}} CLI
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} 可以用作 [NPM 登錄 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.npmjs.com){: new_window} 中的 NPM 套件。

請確定已安裝 **node.js** 及 **npm** 來安裝 NPM 套件。請遵循 [nodejs.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://nodejs.org/){: new_window} 中的安裝指示，來安裝 **node.js**。若要確認已適當地安裝 **node.js**，請執行下列指令：
```bash
node -v
```
{: codeblock}

支援的最低 node.js 版本為 **4.2.3**。此外，由於節點及 npm 套件快速發展，{{site.data.keyword.mobilefoundation_short}} CLI 可能無法搭配節點及 npm 的所有可用版本（包括最新版本）完全運作。若要讓 CLI 正常運作，請確定節點的版本為 **6.11.1**，而 npm 版本為 **3.10.10**。
{: note}

若為 MobileFirst CLI 臨時修正程式 *8.0.2018100112* 版及更新版本，您可以使用 Node 8.x 版或 10.x 版。

* 若要安裝 {{site.data.keyword.mobilefoundation_short}} CLI，請執行下列指令：
    ```bash
    npm install -g mfpdev-cli
    ```
    {: codeblock}

* 如果已從「MobileFirst 作業主控台」的「下載中心」下載 CLI 壓縮檔 (*.zip*)，請使用下列指令：
    ```bash
    npm install -g <path-to-mfpdev-cli.tgz>
    ```
    {: codeblock}

* 若要安裝 CLI，但不安裝選用的相依關係，請新增 `--no-optional` 旗標：
    ```bash
    npm install -g --no-optional path-to-mfpdev-cli.tgz
    ```
    {: codeblock}

安裝使用 Node 8 的 MobileFirst CLI 時，您可能會在終端機視窗中看到下列幾個錯誤：
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

此錯誤是由於 [node-gyp 中的已知錯誤 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/nodejs/node-gyp/issues/1547){: new_window} 所造成。可以忽略這些錯誤，因為它不影響 MobileFirst CLI 的作用。此問題適用於 `mfpdev-cli` 臨時修正程式層次 *8.0.2018100112* 及更新版本。若要解決此錯誤，請在安裝期間使用 `--no-optional` 旗標。

* 若要確認已正確地安裝 CLI，請執行下列指令：
    ```bash
    mfpdev
    ```
    {: codeblock}

* CLI 說明即列印為輸出。

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

## {{site.data.keyword.mobilefoundation_short}} CLI 指令的清單
{: #list_cli_commands}

<table summary="具有鏈結可帶給您指令進一步資訊的 {{site.data.keyword.mobilefoundation_short}} CLI 指令">
<caption>表 1. {{site.data.keyword.mobilefoundation_short}} CLI 指令</caption>
 <thead>
 <th colspan="6">一般 [mfpdev](#mfpdev) 指令</th>
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

針對 mfpdev 指令行介面的預覽瀏覽器類型、預覽逾時值及伺服器逾時值，設定您的配置喜好設定。

```bash
mfpdev config
```
{: codeblock}

顯示您環境的相關資訊，包括作業系統、記憶體耗用量、節點版本，以及指令行介面版本。如果現行目錄為 Cordova 應用程式，則也會顯示 Cordova `cordova info` 指令所提供的資訊。

```bash
mfpdev info
```
{: codeblock}

顯示目前使用中的 {{site.data.keyword.mobilefoundation_short}} CLI 的版本號碼。

```bash
mfpdev -v
```
{: codeblock}

除錯模式：產生除錯輸出。

```bash
mfpdev [-d|--debug]
```
{: codeblock}

詳細除錯模式：產生詳細除錯輸出。

```bash
mfpdev [-dd|--ddebug]
```
{: codeblock}

在指令輸出中暫停使用顏色。

```bash
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

顯示 MobileFirst CLI (mfpdev) 指令的說明。將指令名稱作為引數時，可對每個指令類型或指令顯示更明確的說明文字。例如，`mfpdev help server add`。

```bash
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

使用 MobileFirst Server 來登錄應用程式。

```bash
mfpdev app register
```
{: codeblock}

若要將應用程式登錄至非預設伺服器及運行環境，請使用下列指令：

```bash
mfpdev app register <server> <runtime>
```
{: codeblock}

對於 Cordova Windows 平台，必須在指令中新增 `-w <platform>` 引數。platform 引數是要登錄之 Windows 平台的逗點區隔清單。有效值為 windows、windows8 及 windowsphone8。

```bash
mfpdev app register -w windows8
```
{: codeblock}

您可以指定要用於應用程式的後端伺服器及運行環境。若為 Cordova 應用程式，使用這個指令，您可以配置數個其他層面，例如系統訊息的預設語言，以及是否執行總和檢查安全檢查。包括 Cordova 應用程式的其他配置參數。

```bash
mfpdev app config
```
{: codeblock}

從伺服器中擷取現有應用程式配置。

```bash
mfpdev app pull
```
{: codeblock}

將應用程式配置傳送至伺服器。

```bash
mfpdev app push
```
{: codeblock}

不需要目標平台類型的實際裝置，即可預覽 Cordova 應用程式。您可以在「行動式瀏覽器模擬器」或 Web 瀏覽器中檢視預覽。

```bash
mfpdev app preview
```
{: codeblock}

將存在於 www 目錄中的應用程式資源，包裝成可以用於直接更新處理程序的壓縮檔 (*.zip*)。

```bash
mfpdev app webupdate
```
{: codeblock}

此指令會將已更新的 Web 資源包裝成壓縮檔 (*.zip*)，然後將它上傳至已登錄的預設 MobileFirst Server。您可在 `[cordova-project-root-folder]/mobilefirst/` 資料夾中找到包裝的 Web 資源。

若要將 Web 資源上傳至不同的伺服器實例，請提供伺服器名稱及運行環境，作為指令的一部分：

```bash
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

您可以使用 -build 參數，搭配已包裝的 Web 資源來產生壓縮檔 (*.zip*)，但不將它上傳至伺服器。
```bash
mfpdev app webupdate --build
```
{: codeblock}

若要上傳先前建置的套件，請執行下列指令：
```bash
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

也可以選擇使用 `-encrypt` 參數來加密套件的內容：
```bash
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

顯示 MobileFirst Server 的相關資訊。

```bash
mfpdev server info
```
{: codeblock}

將伺服器定義新增至您的環境。

```bash
mfpdev server add
```
{: codeblock}

編輯伺服器定義。

```bash
mfpdev server edit
```
{: codeblock}

若要將伺服器設為預設伺服器，請使用下列指令：

```bash
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

移除您環境中的伺服器定義。

```bash
mfpdev server remove
```
{: codeblock}

開啟「MobileFirst 作業主控台」。

```bash
mfpdev server console
```
{: codeblock}

若要開啟另一部伺服器的主控台，請提供伺服器名稱，作為指令的參數：

```bash
mfpdev server console <server-name>
```
{: codeblock}

取消登錄應用程式，並移除 MobileFirst Server 中的配接器。

```bash
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

建立配接器。

```bash
mfpdev adapter create
```
{: codeblock}

建置配接器。

```bash
mfpdev adapter build
```
{: codeblock}

在現行目錄及其子目錄中，尋找並建置所有配接器。

```bash
mfpdev adapter build all
```
{: codeblock}

將配接器部署至 MobileFirst Server。

```bash
mfpdev adapter deploy
```
{: codeblock}

若要部署至不同伺服器，請使用下列指令：
```bash
mfpdev adapter deploy <server_name>
```
{: codeblock}

在現行目錄及其子目錄中尋找所有配接器，並將它們部署至 MobileFirst Server。

```bash
mfpdev adapter deploy all
```
{: codeblock}

在 MobileFirst Server 上呼叫配接器的程序。

```bash
mfpdev adapter call
```
{: codeblock}

從伺服器中擷取現有配接器配置。
```bash
mfpdev adapter pull
```
{: codeblock}

將配接器配置傳送至伺服器。
```bash
mfpdev adapter push
```
{: codeblock}

## 更新及解除安裝 {{site.data.keyword.mobilefoundation_short}} CLI
{: #update_uninstall_mf_cli}

若要更新指令行介面，請執行下列指令：

```bash
npm update -g mfpdev-cli
```
{: codeblock}

若要解除安裝指令行介面，請執行下列指令：

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
