---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: command line, cli, mfpdev-cli, cli commands

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}

# {{site.data.keyword.mobilefoundation_short}} CLI
{: #mobile_foundation_cli}

{{site.data.keyword.mobilefoundation_short}} provides a Command Line Interface (CLI) tool for the developer `mfpdev`, to easily manage the {{site.data.keyword.mobilefoundation_short}} client and server artifacts.
{: shortdesc}

All `mfpdev` commands can be run in interactive or direct mode. The parameters that are required for the command are prompted for and some default values are used in the interactive mode. In the direct mode, the parameters must be provided along with the command.

## Installing {{site.data.keyword.mobilefoundation_short}} CLI
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} is available as an NPM package in the [NPM registry](https://www.npmjs.com){: external}.

Ensure **node.js** and **npm** are installed to install NPM packages. Follow the installation instructions in [nodejs.org](https://nodejs.org/){: external} to install **node.js**. To confirm if **node.js** is installed properly, run the following command:

```bash
node -v
```
{: codeblock}

Minimum supported node.js version is **4.2.3**. Also, with the fast evolving node and npm packages, the {{site.data.keyword.mobilefoundation_short}} CLI might not be fully functional with all the available versions of node and npm. Ensure that node is on version **6.11.1** and npm version is **3.10.10**, for proper functioning of the CLI.
{: note}

For {{site.data.keyword.mobilefirst_notm}} CLI interim fix versions *8.0.2018100112* and higher, you can use Node versions 8.x or 10.x.

- To install the {{site.data.keyword.mobilefoundation_short}} CLI, run the following command:

   ```bash
   npm install -g mfpdev-cli
   ```
   {: codeblock}

- If the CLI compressed file (*.zip*) was downloaded from the Download Center of the {{site.data.keyword.mobilefirst_notm}} Operations Console, use the following command:

   ```bash
   npm install -g <path-to-mfpdev-cli.tgz>
   ```
   {: codeblock}

* To install the CLI without optional dependencies, add the `--no-optional` flag:

   ```bash
   npm install -g --no-optional path-to-mfpdev-cli.tgz
   ```
   {: codeblock}

When installing {{site.data.keyword.mobilefirst_notm}} CLI that uses Node 8, you might see few of the following errors in the terminal window:

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
{: screen}

This error is due a [known bug in node-gyp](https://github.com/nodejs/node-gyp/issues/1547){: external}. These errors can be ignored as it doesn't affect the functioning of the {{site.data.keyword.mobilefirst_notm}} CLI. This issue is applicable for `mfpdev-cli` interim fix level *8.0.2018100112* and higher. To overcome this error, use the `--no-optional` flag during installation.

- To confirm that the CLI is installed correctly, run the following command:

   ```bash
   mfpdev
   ```
   {: codeblock}

- The CLI help is printed as output.

   ```
   NAME
       IBM MobileFirst Foundation Command Line Interface (CLI).

   SYNOPSIS
       mfpdev <command> [options]

   DESCRIPTION
       The {{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} Command Line Interface (CLI) is a command-line
       for developing {{site.data.keyword.mobilefirst_notm}} applications. The command-line can be used by itself, or in conjunction with the {{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} Operations Console. Some functions are available from the command-line only and not the console.

       For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of {{site.data.keyword.IBM_notm}} {{site.data.keyword.mobilefoundation_short}} at
       https://www.ibm.com/support/knowledgecenter.
       ...
       ...
       ...
   ```
   {: screen}

## List of {{site.data.keyword.mobilefoundation_short}} CLI commands
{: #list_cli_commands}

Following tabs details the list of {{site.data.keyword.mobilefoundation_short}} CLI commands:

| Command | Actions |
|:----------------------|:----------------------|
| mfpdev config | Sets your configuration preferences for preview browser type, preview timeout value, and server timeout value for the mfpdev command-line interface. |
| mfpdev info | Displays information about your environment, including operating system, memory consumption, node version, and command-line interface version. If the current directory is a Cordova application, information that is provided by the Cordova `cordova info` command is also displayed. |
| mfpdev -v | Displays the version number of the {{site.data.keyword.mobilefoundation_short}} CLI currently in use. |
| mfpdev [-d &#x7c; --debug] | Debug mode: Produces debug output. |
| mfpdev [-dd &#x7c; --ddebug] | Verbose debug mode: Produces verbose debug output. |
| mfpdev -no-color | Suppresses use of color in command output. |
{: class="simple-tab-table"}
{: caption="Table 1. General mfpdev commands" caption-side="top"}
{: #simpletabtable1}
{: tab-title="General mfpdev commands"}
{: tab-group="MFP-simple"}

| Command | Actions |
|:----------------------|:----------------------|
| mfpdev app register | Registers your app with a {{site.data.keyword.mobilefoundation_short}} Server. |
| mfpdev app register &lt;server&gt; &lt;runtime&gt; | Use this command to register an app to a non-default server and runtime. |
| mfpdev app register -w windows8 | For Cordova Windows&trade; platform, the `-w <platform>` argument must be added to the command. The platform argument is a comma-separated list of the windows platforms to be registered. Valid values are windows, windows8 and windowsphone8. |
| mfpdev app config | You can specify the backend server and runtime to use for your app. For Cordova apps, by using this command you can configure several more aspects such as the default language for system messages and whether to do a checksum security check. Other configuration parameters are included for Cordova apps. |
| mfpdev app pull | Retrieves an existing app configuration from the server. |
| mfpdev app push | Sends app configuration to the server. |
| mfpdev app preview | Preview your Cordova app without requiring an actual device of the target platform type. You can view the preview in either the Mobile Browser Simulator or your web browser. |
| mfpdev app webupdate | Packages the application resources present in the www directory into a compressed (*.zip*) file that can be used for the direct update process. <br>This command packages the updated web resources to a compressed (*.zip*) file and upload it to the default {{site.data.keyword.mobilefoundation_short}} Server registered. The packaged web resources can be found at the `[cordova-project-root-folder]/mobilefirst/` folder. |
| mfpdev app webupdate &lt;server_name&gt; &lt;runtime&gt; | To upload the web resources to different server instance, provide the server name and runtime as part of the command |
| mfpdev app webupdate --build | You can use the –build parameter to generate the compressed (*.zip*) file with the packaged web resources without uploading it to a server. |
| mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip` | To upload a package that was previously built, use the –file parameter. |
| mfpdev app webupdate --encrypt | There's also the option to encrypt the content of the package by using the `–encrypt` parameter. |
{: class="simple-tab-table"}
{: caption="Table 2. mfpdev App commands" caption-side="top"}
{: #simpletabtable2}
{: tab-title="app"}
{: tab-group="MFP-simple"}

| Command | Actions |
|:----------------------|:----------------------|
| mfpdev server info | Displays information about the {{site.data.keyword.mobilefoundation_short}} Server. |
| mfpdev server add | Adds a server definition to your environment. |
| mfpdev server edit | Edit a server definition. |
| mfpdev server edit &lt;server_name&gt; --setdefault | Use this command to set a server as the default server. |
| mfpdev server remove | Removes a server definition from your environment. |
| mfpdev server console | Opens the {{site.data.keyword.mobilefoundation_short}} Operations Console. |
| mfpdev server console &lt;server-name&gt; | To open the console of another server, provide the server name as a parameter for the command. |
| mfpdev server clean | Unregisters apps and removes adapters from the {{site.data.keyword.mobilefoundation_short}} Server. |
{: class="simple-tab-table"}
{: caption="Table 3. mfpdev server commands" caption-side="top"}
{: #simpletabtable3}
{: tab-title="server"}
{: tab-group="MFP-simple"}

| Command | Actions |
|:----------------------|:----------------------|
| mfpdev adapter create | Creates an adapter. |
| mfpdev adapter build | Builds an adapter. |
| mfpdev adapter build all | Finds and builds all of the adapters in the current directory and its subdirectories. |
| mfpdev adapter deploy | Deploys an adapter to the {{site.data.keyword.mobilefoundation_short}} Server. |
| mfpdev adapter deploy &lt;server_name&gt; | Use this command to deploy the adapter to a different server. |
| mfpdev adapter deploy all | Finds all of the adapters in the current directory and its subdirectories, and deploys them to the {{site.data.keyword.mobilefoundation_short}} Server. |
| mfpdev adapter call | Calls an adapter’s procedure on the {{site.data.keyword.mobilefoundation_short}} Server. |
| mfpdev adapter pull | Retrieves an existing adapter configuration from the server. |
| mfpdev adapter push | Sends adapter configuration to the server. |
{: class="simple-tab-table"}
{: caption="Table 4. mfpdev adapter commands" caption-side="top"}
{: #simpletabtable4}
{: tab-title="adapter"}
{: tab-group="MFP-simple"}

| Command | Actions |
|:----------------------|:----------------------|
| mfpdev help &lt;command name&gt; | Displays help for {{site.data.keyword.mobilefoundation_short}} CLI (`mfpdev`) commands. With command name as an argument, it displays more specific help text for each command type or command. For example, `mfpdev help server add`. |
{: class="simple-tab-table"}
{: caption="Table 5. mfpdev help command" caption-side="top"}
{: #simpletabtable5}
{: tab-title="help"}
{: tab-group="MFP-simple"}

## Updating and uninstalling {{site.data.keyword.mobilefoundation_short}} CLI
{: #update_uninstall_mf_cli}

To update the command-line interface, run the command:

```bash
npm update -g mfpdev-cli
```
{: codeblock}

To uninstall the command-line interface, run the command:

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
