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

# 模糊化資源
{: #obfuscate_resources}

模糊化是修改執行檔的處理程序，因此不再適用於駭客，但保有完整功能。自動程式碼模糊化讓程式反向工程更為困難。

藉由模糊化，應用程式對於反向工程人員而言會變得更為困難、防止商業機密（智慧財產）遭竊、未獲授權存取、略過授權或其他控制，以及漏洞。

程式碼模糊化由許多不同的技術及應用程式安全技術組成：

* 重新命名模糊化 - 重新命名會變更方法及變數的名稱，讓人員更難瞭解解編的原始檔，但不會變更應用程式執行。.NET、iOS、Java 及 Android 混淆器大部分偏好的模糊化方法。
* 字串加密
* 控制流程模糊化
* 指示型樣轉換
* 虛擬編碼插入
* 反竄改等等

模糊化讓攻擊者更難檢閱程式碼以及分析應用程式。針對模糊化，可以使用各種協力廠商工具。

* 如需 Android 應用程式的模糊化，請參閱此[部落格](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/)。
    

  您需要建立配置檔 (proguard-project.txt)，來進行 MobileFirst Android 應用程式的 Android ProGuard 模糊化。
  {: note}

* 如需 Cordova 應用程式的基本模糊化，請參閱[加密 Cordova 應用程式](#encryptingcordovapackage)。

## 加密 Cordova 套件的 Web 資源
{: #encryptingcordovapackage}

若要將某人檢視及修改您 Web 資源的風險降到最低，不論它是位於 `.apk` 還是 `.ipa` 套件，您都可以使用下列 MobileFirst CLI 指令：
```bash
mfpdev app webencrypt
```
{: codeblock}
或 `mfpwebencrypt` 旗標將資訊加密。此程序未提供很難破解的加密，而是提供基本模糊化層次。

### 必要條件

* 您必須安裝 Cordova 開發工具。此範例使用 Apache Cordova CLI。如果您使用其他 Cordova 開發工具，則有些步驟會不同。如需指示，請參閱 Cordova 工具文件。
* 您必須安裝 MobileFirst CLI。
* 您必須安裝 MobileFirst Cordova 外掛程式。

完成此程序的最佳時間是在完成應用程式開發並準備好部署應用程式之後。如果您在完成 Web 資源加密程序之後執行下列任何指令，則已加密的內容會變成已解密。

* `cordova prepare`
* `cordova build`
* `cordova run`
* `cordova emulate`
* `  mfpdev app webupdate
  `
* `mfpdev app preview
`

如果您在加密 Web 資源之後執行其中一個列出的指令，則必須重新完成此程序，才能加密 Web 資源。

1. 開啟終端機視窗，並導覽至您要加密之 Cordova 應用程式的根目錄。
2. 輸入下列其中一個指令，以準備應用程式：
    ```bash
    cordova prepare
    ```
    {: codeblock}
    ```bash
    mfpdev app webupdate
    ```
    {: codeblock}
3. 完成下列其中一個程序來加密內容。
    * 輸入下列指令：
      ```bash
      mfpdev app webencrypt
      ```
      {: codeblock}

      您可以檢視 `mfpdev app webencrypt` 指令的相關資訊，請輸入：
      ```bash
      mfpdev help app webencrypt
      ```
      {: codeblock}
      {: tip}

    * 您也可以加密 Cordova 套件的 Web 資源，方法是將 `mfpwebencrypt` 旗標新增至 `cordova compile`，或在建置套件時新增至 `cordova build` 指令。
        ```bash
         cordova compile -- --mfpwebencrypt
         ```
         {: codeblock}
         或
         ```bash
         cordova build -- --mfpwebencrypt
         ```
         {: codeblock}
         **www** 資料夾中的作業系統資訊已取代為包含已加密內容的 **resources.zip** 檔案。
         如果您的應用程式適用於 Android 作業系統，而且 **resources.zip** 檔案大於 1 MB，則會將 **resources.zip** 檔案分成命名為 **resources.zip.nnn** 的較小 768 KB .zip 檔案。y變數 nnn 是 001 到 999 的數字。
4. 利用平台專用工具隨附的模擬器，使用已加密資源來測試應用程式。例如，您可以使用 Android Studio for Android 或 Xcode for iOS 中的模擬器。

在您加密應用程式之後，請不要使用下列 Cordova 指令來測試應用程式。
```bash
cordova run
```
{: codeblock}
```bash
cordova emulate
```
{: codeblock}
這些指令會重新整理 www 資料夾中的已加密內容，並將它重新儲存為已解密內容。如果您使用這些指令，則請記住先重新完成該程序來進行加密，再發佈應用程式。
{: note}
