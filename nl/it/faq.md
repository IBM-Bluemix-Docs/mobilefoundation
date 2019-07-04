---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:note: .note}

# FAQ (domande frequenti)
{: #mfp-faq}

Questa FAQ fornisce risposte a domande comuni sul servizio {{site.data.keyword.mobilefoundation_long}}.
{: shortdesc}

## Come so quando gli aggiornamenti del servizio {{site.data.keyword.mobilefoundation_short}} vengono resi disponibili?
{: #maintupdates_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} crea un {{site.data.keyword.mfserver_short_notm}}. Gli aggiornamenti al server {{site.data.keyword.mobilefoundation_short}} vengono notificati agli utenti. Viene visualizzata una notifica nel dashboard dell'istanza del servizio. L'utente può scegliere di applicare l'aggiornamento a {{site.data.keyword.mobilefoundation_short}} durante una finestra di manutenzione che viene decisa da lui. Puoi scegliere di aggiornare il server {{site.data.keyword.mobilefoundation_short}} quando è conveniente per te.

L'aggiornamento del servizio {{site.data.keyword.mobilefoundation_short}} viene reso disponibile quando viene aggiornato uno dei seguenti componenti.

* {{site.data.keyword.mfserver_short_notm}}.
* Versione Liberty sottostante.
* Versione Java Developer Kit sottostante.

## Come applico gli aggiornamenti al servizio {{site.data.keyword.mobilefoundation_short}}?
{: #apply_update_mf}
{: faq}

L'aggiornamento per {{site.data.keyword.mobilefoundation_short}} può essere applicato facendo clic su **Ricrea**.
Durante l'applicazione dell'aggiornamento, la versione del server, come visto in {{site.data.keyword.mfp_oc_short_notm}}, viene modificata per indicare la versione aggiornata del server.

* Gli utenti non possono applicare le proprie correzioni e i propri aggiornamenti alla loro istanza del servizio {{site.data.keyword.mobilefoundation_short}}.
* Consulta [Ricreare il server nel piano Professional Per Device](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5) e [Ricreare il server nel piano Professional 1 Application](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2) per comprendere la differenza nel comportamento dei piani quando viene fatto clic su **Ricrea**.
{: note}

## Come configuro il dominio personalizzato della mia istanza del server {{site.data.keyword.mobilefoundation_short}}?
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} fornisce un {{site.data.keyword.mfserver_short_notm}}, che è accessibile utilizzando un URL con i nomi di dominio basati sulla {{site.data.keyword.Bluemix_notm}} **Regione**. Puoi anche configurare il tuo dominio personalizzato.

L'URL o la rotta sono creati con i nomi del dominio predefiniti basati sulla `Regione` {{site.data.keyword.Bluemix_notm}}.

  |Dominio |  Regione  |    
  |:----- | :----- |    
  |`mybluemix.net` | Stati Uniti Sud |    
  |`eu-gb.mybluemix.net` | Regno Unito  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Francoforte |   
  {: caption="Tabella 1. Nomi dominio dell'applicazione basati sulla regione in {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Per utilizzare il tuo dominio, dovrai configurare il dominio personalizzato completando la seguente procedura:

1.	Crea un'istanza {{site.data.keyword.mfserver_short_notm}} creando l'istanza del servizio {{site.data.keyword.mobilefoundation_short}} scegliendo uno dei piani supportati.

+ Aggiungi il dominio personalizzato che vuoi utilizzare alla tua `Organizzazione` {{site.data.keyword.Bluemix_notm}}. Per aggiungere il tuo dominio, vai a **Gestisci organizzazioni > DOMINI > AGGIUNGI DOMINIO**.

+ Configura una rotta in modo che il server utilizzi il tuo dominio personalizzato.

+ Vai al provider DNS del tuo dominio e aggiungi una voce CNAME che instrada il traffico dal tuo dominio alla rotta {{site.data.keyword.Bluemix_notm}} predefinita, in cui è in esecuzione il server.

+ Se desideri configurare `https` per il tuo dominio personalizzato, carica il certificato SSL per il tuo dominio in {{site.data.keyword.Bluemix_notm}}. Per caricare il certificato SSL, vai a **Gestisci organizzazioni > DOMINI**, seleziona il dominio personalizzato per cui desideri configurare il certificato SSL e fai clic su **Carica certificato** per caricare il certificato SSL per il tuo dominio. Per ulteriori informazioni, vedi [questo post ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}.
