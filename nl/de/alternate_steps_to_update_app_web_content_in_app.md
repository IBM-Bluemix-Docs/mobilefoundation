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

# Alternative Schritte zur Aktualisierung von Webinhalten in Apps
{: #alternate_steps_to_update_app_web_content_in_app}

Überprüfen Sie die folgende Liste, in der einige alternative Methoden zum Aktualisieren von Webinhalten in Ihrer App aufgeführt sind. 

* Erstellen Sie die `.zip`-Datei und laden Sie sie an einen anderen Mobile Foundation-Server hoch: `mfpdev app webupdate [server-name] [runtime-name]`.
  Beispiel:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* Laden Sie eine zuvor generierte `.zip`-Datei hoch: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Beispiel:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* Laden Sie manuell gepackte Webressourcen an den Mobile Foundation-Server hoch:
  1. Erstellen Sie die .zip-Datei, ohne sie hochzuladen:
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Laden Sie die Mobile Foundation Operations Console und klicken Sie auf den Anwendungseintrag.
  3. Klicken Sie auf **Webressourcendatei hochladen**, um die gepackten Webressourcen hochzuladen.    
      ![ZIP-Datei für Direct Update aus der Konsole hochladen](images/upload-direct-update-package.png "ZIP-Datei für Direct Update aus der Konsole mit hervorgehobener Schaltfläche 'Webressourcendatei hochladen' hochladen")

Führen Sie den Befehl `mfpdev help app webupdate` aus, um weitere Informationen zu erhalten.
{: tip}
