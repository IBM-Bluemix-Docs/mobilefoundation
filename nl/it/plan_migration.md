---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Migra il piano di servizio Mobile Foundation

Le istanze di Mobile Foundation create utilizzando i piani obsoleti devono essere aggiornate ai nuovi piani. Un aggiornamento del piano può anche essere necessario in base all'utilizzo dell'istanza.
{: shortdesc}

## Scenario di esempio: Esegui la migrazione dal piano Professional Per Device al piano Professional 1 Application

1. Dal dashboard, IBM Cloud, seleziona l'istanza di IBM Mobile Foundation di cui vuoi eseguire la migrazione.
2. Seleziona **Piano** dalla navigazione.
   ![Piano Mobile Foundation esistente](images/existing-plan.png)
3. Dai piani dei prezzi elencati, seleziona Professional 1 Application.
   ![Nuovo piano Mobile Foundation](images/new-plan.png)
4. Fai clic sul pulsante **Salva** e conferma la migrazione del piano.
     La migrazione a Professional 1 Application è ora completata e tutti i dati esistenti vengono conservati. La fatturazione viene modificata e non si verifica alcun tempo di inattività.
5. Dopo la migrazione del piano, l'istanza di Mobile Foundation deve essere creata nuovamente dal dashboard del servizio perché la corretta configurazione diventi effettiva. Questo aggiornamento richiede un breve tempo di inattività. Dovrai programmare le tue attività per tenere conto di questo tempo di inattività. Seleziona **Gestisci** dalla navigazione e fai clic su **Ricrea**.

Se usi uno dei piani obsoleti, devi eseguire la migrazione a un nuovo piano.
{: note}

## Migrazioni del piano supportate

* Il piano *Developer* (obsoleto) può essere aggiornato solo al nuovo piano *Developer*.
* Il piano *Developer Pro* (obsoleto) può essere aggiornato solo ai piani *Professional Per Device* o *Professional 1 Application*.
* Il piano *Professional Per Capacity* (obsoleto) può essere aggiornato solo ai piani *Professional Per Device* o *Professional 1 Application*.
* Il piano *Professional Per Device* può essere aggiornato solo al piano *Professional 1 Application*.
* Il piano *Professional 1 Application* può essere aggiornato solo al piano *Professional Per Device*.
* L'aggiornamento del piano non è supportato per il nuovo piano *Developer*.
