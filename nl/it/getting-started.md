---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: getting started, mobile foundation, plans, configure mobile foundation server, sample app, setup

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Esercitazione introduttiva
{: #getting-started}

{{site.data.keyword.mobilefoundation_long}} accelera la configurazione di un ambiente {{site.data.keyword.mfp_full}} che ti consente di sviluppare, testare ed eseguire applicazioni mobili aziendali. {{site.data.keyword.mobilefoundation_short}} offre i seguenti diversi piani di servizio:
* **Developer**: fornisce un'istanza del server Foundation nell'account dell'utente. Consente molte applicazioni con il numero totale dei dispositivi connessi tra tutte le applicazioni limitato a 10. Gratuito e da utilizzare solo a scopo di sviluppo e di test.
* **Professional Per Device**: fornisce un'istanza del server Foundation nell'account dell'utente e comporta un addebito in base al numero di dispositivi connessi attivamente
* **Professional 1 Application**: fornisce un'istanza del server Foundation nell'account dell'utente e consente molti utenti e dispositivi connessi attivamente per una sola singola applicazione.    
{: shortdesc}

Puoi rivedere tutti i piani disponibili [qui](https://cloud.ibm.com/catalog/services/mobile-foundation).
{: note}

Crea un'istanza del servizio {{site.data.keyword.mobilefoundation_short}} che utilizzi uno dei piani supportati seguendo questa esercitazione introduttiva. Quindi, puoi registrare un'applicazione. Scarica e modifica l'applicazione registrata, distribuisci un adattatore e infine verifica l'applicazione.

## Prima di iniziare
{: #prereqs-gs}

Hai bisogno di un account {{site.data.keyword.Bluemix}} e di un'istanza del servizio {{site.data.keyword.mobilefoundation_short}}.

## Passo 1: crea un'istanza del servizio {{site.data.keyword.mobilefoundation_short}}
{: #step1create}

1. Nel **catalogo** {{site.data.keyword.Bluemix_notm}}, seleziona [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation). Viene aperta la schermata di configurazione del servizio.
2. Fornisci un nome alla tua istanza del servizio o utilizza un nome preimpostato.
3. Scegli la regione, l'organizzazione e lo spazio in cui desideri creare l'istanza del servizio.
4. Seleziona il tuo **Piano dei prezzi** e fai clic su **Crea**.

## Passo 2: crea il tuo canale mobile
{: #buildmobilechannel}


### Per il piano {{site.data.keyword.mobilefoundation_short}}: Developer
{: #buildchanneldevplan}

Dopo che hai creato un'istanza di {{site.data.keyword.mobilefoundation_short}}: Developer, puoi iniziare a creare il tuo canale mobile completando la seguente procedura.

* Puoi accedere e lavorare immediatamente con il server Mobile Foundation.

  Questa selezione crea un {{site.data.keyword.mfserver_long_notm}} con le seguenti impostazioni:
  *	1 GB di memoria. Questa dimensione è sufficiente per lo sviluppo e per delle attività
leggere di test.

  * Per accedere al server Mobile Foundation utilizzando la CLI hai bisogno delle credenziali, che sono disponibili quando fai clic su **Credenziali del servizio** dal pannello di navigazione della console IBM Cloud. 

### Per il piano {{site.data.keyword.mobilefoundation_short}}: Professional Per Device
{: #buildchannelprofdeviceplan}

Dopo che hai creato un'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, puoi iniziare a creare il tuo canale mobile completando la seguente procedura.

  1.  Connettiti a un servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} esistente su {{site.data.keyword.Bluemix_notm}}.

      1.  Seleziona l'`Organizzazione` {{site.data.keyword.Bluemix_notm}} in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

      + Seleziona lo `Spazio` {{site.data.keyword.Bluemix_notm}} in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}}, dall'elenco di spazi disponibili nell'`Organizzazione` selezionata.

      + Seleziona il `Nome servizio` e le `Credenziali` di {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} per connetterti all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} esistente.

      + Verifica la connessione all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} selezionata facendo clic su **Verifica connessione**.

      + Fai clic su **Aggiungi** e quindi su **Continua** nella finestra a comparsa che richiede conferma sul servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} selezionato. Questa azione crea le tabelle richieste nell'istanza del servizio database {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} configurata.

      Dopo aver aggiunto una connessione {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} all'istanza {{site.data.keyword.mobilefoundation_short}}, non potrai modificarla.
      {: note}
  2.  Crea e avvia il server.

      1. Crea un'istanza del server {{site.data.keyword.mobilefirst_notm}} con la configurazione predefinita, fai clic su **Avvia server di base**.

      + Questa selezione fornisce un {{site.data.keyword.mfserver_long_notm}} con le seguenti impostazioni:
          - Due nodi con 1 GB di memoria ciascuno. Questa dimensione è buona per lo sviluppo, per delle attività moderate di test e i carichi di lavoro di produzione su piccola scala.

          -	Il `nome utente` e la `password` ti vengono generati automaticamente. Disporrai dell'accesso ad essi quando il server è avviato e in esecuzione.

          Inizia il processo di creazione del tuo server. Questo processo impiega circa 10 minuti e una finestra di messaggio
indica l'avanzamento di questa operazione. Al suo completamento, viene visualizzato un dashboard
dove puoi vedere:
            -	Lo stato del tuo server in esecuzione (stato, dimensione).
            -	La rotta del server viene creata per te. Utilizza questa rotta nella tua applicazione mobile per collegarti a {{site.data.keyword.mfserver_short_notm}}.
            -	I tuoi `nomeutente` e `password` personali per accedere a {{site.data.keyword.mfp_oc_short_notm}}. La `password` è nascosta. Fai clic sull'icona **Mostra password** per visualizzarla.

      +	Fai clic su **Avvia console** per aprire {{site.data.keyword.mfp_oc_short_notm}}.      

      Per creare un'istanza del server {{site.data.keyword.mobilefirst_notm}} con la configurazione avanzata per topologia, sicurezza e altra configurazione del server, fai clic su **Avvia server con la configurazione avanzata**. Per ulteriori informazioni, vedi [Impostazione della configurazione avanzata](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#using_mfs_advanced_p5).
      {: tip}

### Per il piano {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application
{: #buildchannelprof1appplan}

Dopo che hai creato un'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, puoi iniziare a creare il tuo canale mobile completando la seguente procedura.

  1.  Connettiti a un servizio {{site.data.keyword.Db2_on_Cloud_long}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL_full}} esistente su {{site.data.keyword.Bluemix_notm}}.

      1.  Seleziona l'`Organizzazione` {{site.data.keyword.Bluemix_notm}} in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}}.

      + Seleziona lo `Spazio` {{site.data.keyword.Bluemix_notm}} in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}}, dall'elenco di spazi disponibili nell'`Organizzazione` selezionata.

      + Seleziona il `Nome servizio` e le `Credenziali` di {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} per connetterti all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} esistente.

      + Verifica la connessione all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} selezionata facendo clic su **Verifica connessione**.

      + Fai clic su **Aggiungi** e quindi su **Continua** nella finestra a comparsa che richiede conferma sul servizio {{site.data.keyword.Db2_on_Cloud_short}} o {{site.data.keyword.composeForPostgreSQL}} selezionato. Questa azione crea le tabelle richieste nell'istanza del servizio database {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} configurata.

      Dopo aver aggiunto una connessione {{site.data.keyword.Db2_on_Cloud_short}} (qualsiasi piano diverso dal piano **Lite**) o {{site.data.keyword.composeForPostgreSQL}} all'istanza {{site.data.keyword.mobilefoundation_short}}, non potrai modificarla.
      {: note}

  2.  Crea e avvia il server.

      1. Crea un'istanza del server {{site.data.keyword.mobilefirst_notm}} con la configurazione predefinita, fai clic su **Avvia server di base**.

        `L'istanza del server di base include un singolo nodo, 1 GB di memoria.`

      + Il `nome utente` e la `password` ti vengono generati
automaticamente. Disporrai dell'accesso ad essi quando il server è avviato e in esecuzione.  

        Inizia il processo di creazione del tuo server. Questo processo impiega circa 10 minuti e una finestra di messaggio
indica l'avanzamento di questa operazione. Al suo completamento, viene visualizzato un dashboard
dove puoi vedere:
          -	Lo stato del tuo server in esecuzione (stato, dimensione).
          -	La rotta del server viene creata per te. Utilizza questa rotta nella tua applicazione mobile per collegarti a {{site.data.keyword.mfserver_short_notm}}.
          -	I tuoi `nomeutente` e `password` personali per accedere a {{site.data.keyword.mfp_oc_short_notm}}. La `password` è nascosta. Fai clic sull'icona **Mostra password** per visualizzarla.

      +  Fai clic su **Avvia console** per aprire {{site.data.keyword.mfp_oc_short_notm}}.  

      Per creare un'istanza del server {{site.data.keyword.mobilefirst_notm}} con la configurazione avanzata per topologia, sicurezza e altra configurazione del server, fai clic su **Avvia server con la configurazione avanzata**. Per ulteriori informazioni, vedi [Impostazione della configurazione avanzata](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#using_mfs_advanced_p2).
      {: tip}

Vai a [Using the Mobile Foundation service to set up MobileFirst Server ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/){: new_window} per ulteriori informazioni introduttive a {{site.data.keyword.mobilefoundation_short}}.
{: note}

## Passo 3: registra la tua applicazione in {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

Dopo che hai creato e avviato la tua istanza del server Mobile Foundation, puoi completare la seguente procedura per registrare un'applicazione Android.

  1.  Richiama {{site.data.keyword.mfp_oc_short_notm}} caricando l'URL: `http://<your-server-host>:<server-port>/mfpconsole`. Utilizza il `nomeutente` e la `password` generate durante il provisioning.

  + Nel **Dashboard** {{site.data.keyword.mfp_oc_short_notm}}, fai clic su **Nuovo** accanto a **Applicazioni**.

  + Fornisci *MFPStarterAndroid* come **Nome applicazione**.

  + **Scegli piattaforma** deve essere *Android*.

  + Fornisci *com.ibm.mfpstarterandroid* come **Identificativo applicazione**.

  + Immetti *1.0* come la **Versione**.

  + Fai clic su **Registra applicazione**.

## Passo 4: scarica l'applicazione di esempio
{: #downloadapp}

  1.  Dal **Dashboard** {{site.data.keyword.mfp_oc_short_notm}}, seleziona **MFPStarterAndroid** in **Applicazioni**.

  + Fai clic su **Ottieni il codice starter** e seleziona di scaricare l'esempio di applicazione Android.

## Passo 5: modifica l'applicazione di esempio
{: #editapp}

  1. Importa l'applicazione Android di esempio scaricata, dal passo precedente, in Android Studio.

  + Dal menu della barra laterale **Progetto** in Android Studio, seleziona il file **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java**.

    * Aggiungi le seguenti importazioni
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * Incolla il seguente frammento di codice, sostituendo la chiamata a `WLAuthorizationManager.getInstance().obtainAccessToken`

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Stamperà "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
                    public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## Passo 6: distribuisci un adattatore
{: #deployadapter}

  1. Scarica questa [risorsa utente adattatore](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} e distribuiscila da {{site.data.keyword.mfp_oc_short_notm}} utilizzando **Azioni → Distribuisci adattatore**.

## Passo 7: verifica l'applicazione
{: #testapp}

  1. In Android Studio, dal menu della barra laterale **Progetto**, seleziona il file **app → src → main →assets → mfpclient.properties** e modifica le proprietà `protocollo`, `host` e `porta` con i valori corretti per il tuo server MobileFirst.

   I valori normalmente sono https, *your-server-address* e 443.
   {: tip}

  2. Fai clic su **Esegui applicazione** in Android Studio.
     * Vedrai che l'applicazione viene avviata su un emulatore di dispositivo.
     * Fai clic su **Esegui ping del server MobileFirst** nella tua applicazione, viene visualizzato `Connected to MobileFirst Server`.
     * Se l'applicazione è riuscita a collegarsi al server MobileFirst, l'adattatore Java che viene distribuito effettuerà una chiamata di richiesta della risorsa.
     * La risposta dell'adattatore viene quindi stampata nella vista LogCat di Android Studio.


## Passi successivi
{: #nextsteps-gs}

Puoi seguire le [Quick Start tutorials ![Icona link esterno](../../icons/launch-glyph.svg "Quick Start tutorials")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} per utilizzare più applicazioni di esempio e per esplorare l'utilizzo di {{site.data.keyword.mobilefoundation_short}}.

Quick Start dispone di esercitazioni che illustrano l'utilizzo di {{site.data.keyword.mobilefoundation_short}} per le applicazioni iOS, Android, Web, Cordova, Windows, React Native, Ionic e Xamarin.
