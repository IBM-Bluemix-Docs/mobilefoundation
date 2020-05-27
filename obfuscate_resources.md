---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-27"

keywords: obfuscating resources, security

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

# Obfuscate resources
{: #obfuscate_resources}

Obfuscation is the process of modifying an executable so that it is no longer useful to a hacker but remains fully functional. Automated code obfuscation makes reverse-engineering a program difficult.

By obfuscation, an application becomes much more difficult to reverse-engineer, protected against trade secret (intellectual property) theft, unauthorized access, bypassing licensing or other controls, and vulnerability.

Code obfuscation consists of many different techniques and application security techniques:

* Rename Obfuscation - Renaming alters the name of methods and variables and makes the decompiled source harder for human to understand with but does not alter application execution. Most preferred method of obfuscation by .NET, iOS, Java, and Android obfuscators.
* String Encryption
* Control Flow Obfuscation
* Instruction Pattern Transformation
* Dummy Code Insertion
* Anti-Tamper, and others.

Obfuscation makes it much more difficult for attackers to review the code and analyze the application. For obfuscation, various third-party tools are available.

* For obfuscation of an android application, refer to this [blog](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/){: external}.

   You need to create the configuration file (proguard-project.txt) for Android ProGuard obfuscation with a MobileFirst Android application.
   {: note}

* For basic obfuscation of Cordova application, refer to [Encrypting Cordova application](#encryptingcordovapackage).

## Encrypting the web resources of your Cordova packages
{: #encryptingcordovapackage}

To minimize the risk of viewing and modifying your web resources by someone, while it is in the `.apk` or `.ipa` package, you can use the following {{site.data.keyword.mobilefoundation_short}} CLI command:

```bash
mfpdev app webencrypt
```
{: codeblock}

or use the `mfpwebencrypt` flag to encrypt the information. This procedure does not provide encryption that is impossible to defeat but it provides a basic level of obfuscation.

### Prerequisites

* You must install the Cordova development tools. This example uses the Apache Cordova CLI. If you use other Cordova development tools, some of the steps are different. Refer to your Cordova tool documentation for instructions.
* You must install the {{site.data.keyword.mobilefoundation_short}} CLI.
* You must install the {{site.data.keyword.mobilefoundation_short}} Cordova plug-in.

The best time to complete this procedure is after you finish your app development and are ready to deploy the app. If you run any of the following commands after you complete the web resources encryption procedure, the content that was encrypted becomes decrypted.

* `cordova prepare`
* `cordova build`
* `cordova run`
* `cordova emulate`
* `mfpdev app webupdate`
* `mfpdev app preview`

If you run one of the listed commands after you encrypt the web resources, you must complete this procedure again to encrypt the web resources.

1. Open a terminal window and navigate to the root directory of the Cordova app that you want to encrypt.
1. Prepare the app by entering one of the following commands:

   ```bash
   cordova prepare
   ```
   {: codeblock}

   ```bash
   mfpdev app webupdate
   ```
   {: codeblock}

1. Complete one of the following procedures to encrypt the content:
   * Enter the following command:

      ```bash
      mfpdev app webencrypt
      ```
      {: codeblock}

      You can view information about the `mfpdev app webencrypt` command by entering 
      ```bash
      mfpdev help app webencrypt
      ```
      {: codeblock}
      {: tip}

   * You can also encrypt the web resources of your Cordova packages by adding the `mfpwebencrypt` flag to the `cordova compile` or to the `cordova build` command when you build your packages.
      ```bash
      cordova compile -- --mfpwebencrypt
      ```
      {: codeblock}
 
         or
 
      ```bash
      cordova build -- --mfpwebencrypt
      ```
      {: codeblock}

      The operating system information in the **www** folder is replaced by a **resources.zip** file that contains the encrypted content.
      If your app is for the Android operating system and the **resources.zip** file is larger than 1 MB, the **resources.zip** file is divided into smaller 768 KB compressed files that are named **resources.zip.nnn**. The variable nnn is a number from 001 through 999.

1. Test the application with the encrypted resources by using the emulator that is provided with the platform-specific tools. For example, you can use the emulator in Android Studio for Android, or Xcode for iOS.

   Do not use the following Cordova commands to test the application after you encrypt it,

   ```bash
   cordova run
   ```
   {: codeblock}

   ```bash
   cordova emulate
   ```
   {: codeblock}

   These commands refresh the content that was encrypted in the www folder, and saves it again as decrypted content. If you use these commands, remember to complete the procedure again to encrypt it before you publish the app.
   {: note}
