---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestisci i dispositivi
{: #manage_devices}

L'accesso ai dispositivi e lo stato dei dispositivi possono essere gestiti dalla Mobile Foundation Operations Console. Un dispositivo è identificato in modo univoco con un identificativo denominato ID dispositivo, assegnato dall'SDK client Mobile Foundation. Puoi anche impostare un nome di visualizzazione per il tuo dispositivo. Entrambi i campi di ID dispositivo e nome di visualizzazione del dispositivo sono indicizzati per la ricerca.

Gli sviluppatori di applicazioni possono utilizzare il metodo `setDeviceDisplayName` della classe `WLClient` per impostare il nome di visualizzazione del dispositivo. Vedi [qui](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/) per la documentazione di `WLClient`. Gli sviluppatori di adattatori Java possono anche impostare il nome di visualizzazione del dispositivo utilizzando il metodo `setDeviceDisplayName` della classe `com.ibm.mfp.server.registration.external.model MobileDeviceData`.
{: tip}

Il server Mobile Foundation conserva le informazioni sullo stato per ogni dispositivo che accede al server.
I valori di stato possibili sono:
* Attivo
* Perso
* Rubato
* Scaduto
* Disabilitato

Lo stato predefinito per un dispositivo è **Attivo**, che indica che l'accesso da questo dispositivo non è bloccato. Puoi modificare lo stato in **Perso**, **Rubato** o **Disabilitato** per bloccare l'accesso alle tue risorse dell'applicazione dal dispositivo. Puoi sempre ripristinare lo stato **Attivo** per consentire nuovamente l'accesso.

Lo stato **Scaduto** è uno stato speciale impostato dal server Mobile Foundation dopo che un dispositivo ha raggiunto una durata di inattività preconfigurata. Questo stato viene utilizzato per la traccia delle licenze e non influenza i diritti di accesso del dispositivo. Quando un dispositivo con lo stato **Scaduto** si riconnette al server, il suo stato viene ripristinato su **Attivo** e al dispositivo viene concesso l'accesso al server.

## Blocca i dispositivi
{: #block_devices}

1. Vai alla pagina **Dispositivi** dalla Mobile Foundation Operations Console.
2. Utilizza il campo **Ricerca** per cercare un dispositivo fornendo l'ID utente associato al dispositivo oppure fornendo il nome di visualizzazione del dispositivo (se è impostato su un dispositivo).
3. Per ciascuno dei dispositivi restituiti nei risultati della ricerca, puoi vedere l'ID dispositivo, il nome di visualizzazione, il modello del dispositivo, il sistema operativo e l'elenco di ID utente associati al dispositivo.
4. La colonna dello stato del dispositivo mostra lo stato del dispositivo. Puoi modificare lo stato del dispositivo in **Perso**, **Rubato** o **Disabilitato** per bloccare l'accesso dal dispositivo alle risorse dell'applicazione protette.

   Modificare lo stato nuovamente in **Attivo** ripristina i diritti di accesso originali.
   {: note}


## Annulla la registrazione dei dispositivi
{: #unregister_devices}

1. Vai alla pagina **Dispositivi** dalla Mobile Foundation Operations Console.
2. Utilizza il campo **Ricerca** per cercare un dispositivo fornendo l'ID utente associato al dispositivo oppure fornendo il nome di visualizzazione del dispositivo (se è impostato su un dispositivo).
3. Per ciascuno dei dispositivi restituiti nei risultati della ricerca, puoi vedere l'ID dispositivo, il nome di visualizzazione, il modello del dispositivo, il sistema operativo e l'elenco di ID utente associati al dispositivo.
4. Seleziona *Annulla registrazione* dalla colonna **Azioni**.

   L'annullamento della registrazione di un dispositivo elimina i dati di registrazione di tutte le applicazioni Mobile Foundation installate sul dispositivo. Inoltre, vengono eliminati il nome di visualizzazione del dispositivo, l'elenco di utenti associati al dispositivo e gli attributi pubblici che l'applicazione ha registrato per questo dispositivo.
   {: note}


Non **annullare la registrazione** di un dispositivo se si intendeva **bloccare** il dispositivo. L'annullamento della registrazione di un dispositivo rimuove tutti i dati correlati al dispositivo. Se un utente prova ad accedere al server Mobile Foundation utilizzando lo stesso dispositivo, gli viene richiesto di registrare il dispositivo e la registrazione creerà un nuovo ID dispositivo per il dispositivo nel server. Ciò significa che il dispositivo riottiene l'accesso al server Mobile Foundation.
{: important}
