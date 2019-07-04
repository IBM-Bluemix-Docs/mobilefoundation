---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: disable apps, remote disabling of apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Disabilita in remoto una versione dell'applicazione
{: #remotely_disable_an_app_version}

In questa sezione, discutiamo di come disabilitare l'accesso utente a una specifica versione di un'applicazione su uno specifico sistema operativo mobile e di come fornire un messaggio personalizzato all'utente.

Puoi utilizzare la Mobile Foundation Operations Console per gestire l'accesso all'applicazione.

1. Seleziona la tua versione dell'applicazione dalla sezione **Applicazioni** nella barra laterale di navigazione della console e seleziona quindi la scheda **Gestisci** dell'applicazione.
2. Modifica lo stato in **Accesso disabilitato**.
3. Fornisci un URL per la nuova versione dell'applicazione (di norma, nell'appropriato app store pubblico o privato) nel campo **URL della versione più recente**.
   Per alcun ambienti, l'Application Center fornisce un URL per accedere alla vista dei dettagli di una versione dell'applicazione direttamente. Vedi [Application properties](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties).
   {: tip}

4. Nel campo **Messaggio di notifica predefinito**, aggiungi il messaggio di notifica personalizzato da visualizzare quando l'utente prova ad accedere all'applicazione. Il seguente messaggio di esempio indirizza gli utenti all'esecuzione di un upgrade alla versione più recente:
   `This version is no longer supported. Please upgrade to the latest version.`
5. Nella sezione **Locale supportati** puoi, facoltativamente, fornire il messaggio di notifica in altre lingue.
6. Seleziona **Salva** per applicare le tue modifiche.

Quando un utente esegue un'applicazione che era stata disabilitata in remoto, viene visualizzata una finestra di dialogo con il messaggio personalizzato configurato. Il messaggio viene visualizzato a qualsiasi interazione dell'applicazione che richiede l'accesso a una risorsa protetta oppure quando l'applicazione prova ad ottenere un token di accesso. Se hai fornito un URL di upgrade della versione, la finestra di dialogo visualizza un pulsante **Ottieni nuova versione** per eseguire l'upgrade a una versione più recente, oltre al pulsante **Chiudi** predefinito. <br/>
Se un utente chiude la finestra di dialogo senza eseguire l'upgrade della versione, può continuare a lavorare con le risorse dell'applicazione che non sono protette. Tuttavia, qualsiasi interazione dell'applicazione che richiede l'accesso a una risorsa protetta determina la rivisualizzazione della finestra di dialogo e all'applicazione o all'utente non viene concesso l'accesso alla risorsa.
