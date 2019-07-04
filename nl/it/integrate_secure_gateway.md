---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Stabilimento di una connessione sicura a un sistema in loco utilizzando il servizio Secure Gateway
{: #integrate_secure_gateway}

Quando crei delle applicazioni mobili aziendali, spesso desideri integrare le tue applicazioni con dei SOR (system of record) esistenti. Per accedere ai dati archiviati nel tuo data center in loco da un'applicazione mobile e per avvalertene, puoi utilizzare il [servizio Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) con il [servizio Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway) su [IBM Cloud](https://cloud.ibm.com/). Il servizio Secure Gateway fornisce una connettività rapida, facile e sicura e stabilisce un tunnel tra IBM Cloud e il sistema remoto al quale desideri stabilire una connessione.

Questa esercitazione spiega come accedere agli endpoint HTTP nel tuo data center in loco dagli adattatori Mobile Foundation in esecuzione su IBM Cloud utilizzando il servizio Secure Gateway.

## Prerequisito
{: #prereq_int_sec_gw}

Per completare questa esercitazione, ti servirà un endpoint HTTP all'interno del tuo firewall aziendale che espone i dati dei SOR (system of record). In alternativa, crea un endpoint di test nel tuo ambiente locale utilizzando [questo progetto `Node.js` di esempio ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject).

Vai alla cartella del progetto ed esegui il tuo server HTTP.

```bash
npm install
node app.js
```
{: codeblock}

## Scenario di integrazione con il servizio Secure Gateway
{: #secure_gateway}

La seguente immagine illustra l'architettura utilizzata nello scenario di integrazione illustrato in questa esercitazione.

![Diagramma dell'architettura](images/SecureGatewayArchi.png "Diagramma dell'architettura del dispositivo, del servizio cloud e della rete in loco")

## Implementazione dell'integrazione di Secure Gateway
{: #implementing_sg_integration}

### Crea un'istanza del servizio Secure Gateway
Accedi a IBM Cloud e crea un'istanza del [servizio Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway/).

![IBM Cloud](images/SecureGatewayInst.gif "Creazione di un'istanza Secure Gateway dal catalogo IBM Cloud")

Dopo che l'istanza del servizio Secure Gateway è stata creata, utilizza la seguente procedura per configurare il servizio Secure Gateway tra IBM Cloud e il tuo ambiente in loco.

### Aggiungi un gateway
{: #add_gateway}

Nel dashboard del servizio Secure Gateway, fai clic su **Add Gateway** per creare un nuovo gateway fornendo qualsiasi nome gateway desiderato.

![Aggiungi Gateway](images/AcmeAddGateway.gif "Fasi per l'aggiunta di un'IU gateway")


### Aggiungi un client Secure Gateway
{: #add_sg_client}

![Aggiungi Client2](images/AcmeAddClient.gif "Fasi per l'aggiunta di un'IU client")

Dall'interno del tuo nuovo gateway dalla scheda **Clients**, fai clic su **Connect Client**.

Puoi utilizzare uno qualsiasi dei client di tua scelta ed eseguire il client Secure Gateway nel tuo ambiente in loco. Questa procedura per configurare il client Secure Gateway è disponibile nella console Secure Gateway.

In questa esercitazione, utilizzeremo l'opzione del contenitore Docker per eseguire il client Secure Gateway.
Utilizza la seguente procedura:
*   Installa Docker sulla tua macchina in loco, se non è già installato.
*   Avvia un terminale ed esegui il client Secure Gateway su un contenitore utilizzando il comando mostrato nella console del servizio.
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId` può essere trovato nella console come mostrato nella precedente immagine.

### Aggiungi una destinazione
{: #add_destination}

![Aggiungi destinazione](images/AcmeAddDest.gif "Fasi per l'aggiunta dell'IU di destinazione")

Dall'interno del tuo nuovo gateway dalla scheda **Destinations**, fai clic su **Add Destination**.

Il servizio Secure Gateway ti consente di stabilire una connessione a un endpoint in loco dall'ambiente cloud IBM o di stabilire una connessione da un ambiente in loco a una risorsa cloud. Per questo scenario, seleziona in loco come ubicazione della risorsa e fornisci nome host/IP e porta della risorsa in loco. Fornisci anche un nome di destinazione a tua scelta.

Per abilitare l'inoltro della richiesta dal client Secure Gateway all'endpoint della risorsa, dovrai aggiungere la risorsa all'elenco di accesso.
Esegui il seguente comando sul terminale del client Secure Gateway (in esecuzione come contenitore sul runtime Docker, in questo scenario) per aggiungere la risorsa all'elenco di accesso.

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` e `resourcePort` fanno riferimento ai dettagli dell'endpoint della risorsa in loco.

La destinazione è ora configurata. Il servizio Secure Gateway compilerà i dettagli di host e porta cloud, che possono essere utilizzati per accedere alla risorsa in loco dall'ambiente cloud.

![Scheda Destination](images/AcmeCloudPopulate.gif "Schermata Host and port details")

### Configurazione del servizio Secure Gateway con Mobile Foundation e l'adattatore Mobile Foundation
{: #configuration_sg_mfp}

In questa esercitazione, useremo l'istanza del servizio Mobile Foundation su IBM Cloud per configurare il server Mobile Foundation. Il servizio Mobile Foundation su IBM Cloud aiuta ad eseguire il provisioning del server Mobile Foundation sul runtime Liberty come un'applicazione Cloud Foundry. Il servizio Mobile Foundation ti consente di prendere qualsiasi progetto Mobile Foundation sviluppato in un ambiente locale ed eseguirlo su IBM Cloud.

### Configurazione del server Mobile Foundation sul cloud IBM
{: #mf_server_setup}

Crea un'istanza del [servizio Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) dalla console IBM Cloud.

Dalla console del servizio Mobile Foundation, crea il [server Mobile Foundation ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/).


### Creazione e distribuzione dell'adattatore Mobile Foundation
{: #deploying_mf_adapter}

In questa esercitazione, stabiliremo una connessione all'endpoint Secure Gateway utilizzando un adattatore Mobile Foundation. [Scarica ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) l'adattatore JavaHTTP di Mobile Foundation.

Crea e distribuisci l'adattatore nella console Mobile Foundation Operations utilizzando i comandi [mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli).
```bash
mfpdev adapter build
mfpdev adapter deploy
```
{: codeblock}

Ulteriori informazioni sulla creazione e sulla distribuzione degli adattatori sono disponibili [qui ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/).
{: tip}

Fornisci i dettagli di host e porta cloud per l'endpoint della risorsa nell'adattatore JavaHTTP, ottenuto dalla sezione precedente.

![AdapterConfiguration ](images/AdapterConfiguration.png "Pagina Java HTTP configurations")

dove `cap-sg-prd-5.securegateway.appdomain.cloud`e `18946` sono, rispettivamente, l'host e la porta di Secure Gateway.

L'adattatore Mobile Foundation è ora configurato e il servizio Mobile Foundation è ora abilitato a funzionare con un sistema in loco all'interno dell'azienda utilizzando il servizio Secure Gateway.

### Creazione e registrazione dell'applicazione di esempio Mobile Foundation
{: #registering_sample_app}

Scarica l'applicazione di esempio Mobile Foundation da [qui](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/), attieniti alle istruzioni sotto il `Readme` e registra l'applicazione nella console Mobile Foundation Operations.

Esegui l'applicazione, fornisci le credenziali per accedere e fai clic sul pulsante *Login* . Fai clic sul pulsante *Fetch Acme Writers* per richiamare il tuo endpoint in loco tramite Secure Gateway utilizzando l'adattatore JavaHTTP distribuito nella tua console Mobile Foundation Operations. Ricevi i dati desiderati dall'ambiente in loco.

![L'applicazione riceve i dati in loco](images/AcmePublishersApp.gif "Applicazione di esempio che riceve i dati")

Puoi stabilire una connessione a più endpoint in loco configurando più destinazioni nel servizio Secure Gateway e distribuendo gli adattatori Mobile Foundation per stabilire una connessione all'host cloud rispettivo dell'endpoint. Puoi anche configurare il servizio Secure Gateway con della sicurezza aggiuntiva per assicurarti che le comunicazioni all'endpoint abbiano luogo nell'ambito della sicurezza HTTPS e lato applicazione. Puoi trovare i [dettagli qui](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg).


## Riepilogo
{: #summary_int_sec_gw}

Utilizzando questa esercitazione, dovresti essere in grado di stabilire una connessione protetta tra gli adattatori Mobile Foundation in esecuzione su IBM Cloud e un endpoint HTTP in loco utilizzando il servizio Secure Gateway.
