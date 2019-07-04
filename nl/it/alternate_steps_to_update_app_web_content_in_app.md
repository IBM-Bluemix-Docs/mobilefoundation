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

# Procedura alternativa per aggiornare i contenuti web nell'applicazione
{: #alternate_steps_to_update_app_web_content_in_app}

Controlla il seguente elenco per dei modi alternativi per aggiornare i contenuti web nella tua applicazione.

* Crea il file `.zip` e caricalo in un altro server Mobile Foundation: `mfpdev app webupdate [server-name] [runtime-name]`.
  Ad esempio:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* Carica un file `.zip` precedentemente generato: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Ad esempio:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* Carica manualmente le risorse web impacchettate nel server Mobile Foundation:
  1. Crea il file .zip senza caricarlo:
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Carica la Mobile Foundation Operations Console e fai clic sulla voce dell'applicazione.
  3. Fai clic su **Carica il file delle risorse web** per caricare le risorse web impacchettate.    
      ![Carica il file .zip dell'aggiornamento diretto dalla console](images/upload-direct-update-package.png "Carica il file .zip dell'aggiornamento diretto dalla console con il pulsante Carica il file delle risorse web evidenziato")

Immetti il comando `mfpdev help app webupdate` per ulteriori informazioni.
{: tip}
