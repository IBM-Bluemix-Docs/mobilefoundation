---

copyright:
  years: 2018
lastupdated: "2019-06-06"

keywords: export apps, adapters export

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Esporta e importa applicazioni e adattatori
{: #export_import_apps_adapters}

Da {{site.data.keyword.mfp_oc_short_notm}}, in determinate condizioni, puoi esportare un'applicazione o una delle sue versioni e, successivamente, eseguirne l'importazione su un server differente. Puoi anche esportare e reimportare gli adattatori. Utilizza questa funzionalità per scopi di riutilizzo o backup oppure per eseguire la migrazione tra le istanze {{site.data.keyword.mfserver_short_notm}}.

Se ti viene concesso il ruolo di amministratore **mfpadmin** e il ruolo di distributore **mfpdeployer**, puoi esportare una versione o tutte le versioni di un'applicazione. L'applicazione o la versione vengono esportati come un file compresso `.zip`, che salva *ID applicazione*, *descrittori*, *dati di autenticità* e *risorse web*. Puoi successivamente importare l'archivio per ridistribuire l'applicazione o la versione allo stesso server o a uno differente.

Valuta attentamente il tuo caso d'uso:
* Il file di esportazione include i dati di autenticità dell'applicazione. Tali dati sono specifici alla build di un'applicazione mobile. L'applicazione mobile include l'URL del server. Pertanto, se desideri utilizzare un altro server, devi ricreare l'applicazione. Trasferire solo i file dell'applicazione esportati non funzionerebbe.
* Alcune risorse utente possono variare da un server all'altro. Le credenziali di push sono differenti, a seconda che lavori in un ambiente di sviluppo o di produzione.
* La configurazione di runtime dell'applicazione (che contiene lo stato attivo o disabilitato e i profili di log) può essere trasferita in alcuni casi, ma non in tutti.
* Trasferire le risorse web potrebbe non avere senso in alcuni casi, ad esempio quando ricrei l'applicazione per utilizzare un nuovo server.
{: tip}

##  Prerequisito
{: #prereq}

Avvia {{site.data.keyword.mfp_oc_short_notm}}.

##  Procedura
{: #procedure}

1.  Dalla barra laterale di navigazione, seleziona un'applicazione o una versione dell'applicazione oppure un adattatore.

2.  Seleziona **Azioni > Esporta applicazione**, **Esporta versione** o **Esporta adattatore**.
     Ti viene richiesto di salvare il file di archivio `.zip` che racchiude le risorse esportate. L'aspetto della finestra di dialogo dipende dal tuo browser e la cartella di destinazione dipende dalle impostazioni del tuo browser.

3.   Salva il file di archivio.
      Il nome del file di archivio include il nome e la versione dell'applicazione o dell'adattatore, ad esempio `export_applications_com.sample.zip`.

4.   Per riutilizzare un archivio di esportazione esistente, seleziona **Azioni > Importa applicazione**, **Importa versione** o **Importa adattatore**, passa all'archivio e fai clic su **Distribuisci**.
      Il frame di console principale visualizza i dettagli dell'applicazione o dell'adattatore importati.

##    Risultati
{: #results}

Se esegui l'importazione sullo stesso server, l'applicazione o la versione non verranno necessariamente ripristinate come erano state esportate. Vale a dire che la ridistribuzione al momento dell'importazione non rimuove le modifiche successive. Piuttosto, se qualche risorsa dell'applicazione è stata modificata nel tempo intercorso tra l'esportazione e la ridistribuzione al momento dell'importazione, solo le risorse incluse nell'archivio esportato vengono ridistribuite nel loro stato originale.
<br/>
Ad esempio, se esporti un'applicazione senza dati di autenticità e successivamente carichi i dati di autenticità, quando poi esegui l'importazione dell'archivio iniziale i dati di autenticità non vengono cancellati.
