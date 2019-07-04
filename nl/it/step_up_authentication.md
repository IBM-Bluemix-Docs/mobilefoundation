---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, authentication, using challenge handlers

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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# Autenticazione incrementale
{: #step_up_authentication}

Le risorse possono essere protette da diversi controlli di sicurezza. In uno scenario di questo genere, MobileFirst Server invia simultaneamente tutte le verifiche pertinenti all'applicazione.

Un controllo di sicurezza può anche dipendere da un altro controllo di sicurezza. È pertanto importante poter controllare quando vengono inviate le verifiche.

Ad esempio, questa esercitazione descrive un'applicazione che ha due risorse protette da un nome utente e una password, dove la seconda risorsa richiede anche un codice PIN aggiuntivo.

## Riferimento a un controllo di sicurezza
{: #referencing-a-security-check}

Crea due controlli di sicurezza: `StepUpPinCode` e `StepUpUserLogin`.

In questo esempio, `StepUpPinCode` dipende da `StepUpUserLogin`. All'utente dovrebbe essere richiesto di immettere un codice PIN solo dopo un corretto accesso a `StepUpUserLogin`. A questo scopo, `StepUpPinCode` deve essere in grado di fare riferimento alla classe `StepUpUserLogin`.

Il framework MobileFirst fornisce un'annotazione per l'inserimento di un riferimento.

Nella tua classe `StepUpPinCode`, a livello di classe, aggiungi:

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

**Importante**: entrambe le implementazioni di controllo di sicurezza devono essere raggruppate all'interno dello stesso adattatore.
{.important}

Per risolvere questo riferimento, il framework cerca un controllo di sicurezza con la classe appropriata e inserisce il suo riferimento nel controllo di sicurezza dipendente.

Se c'è più di un controllo di sicurezza della stessa classe, l'annotazione ha un parametro di nome facoltativo, che puoi utilizzare per specificare il nome univoco del controllo a cui si fa riferimento.

## Macchina a stati
{: #state-machine}

Tutte le classi che estendono `CredentialsValidationSecurityCheck` (che include sia `StepUpPinCode` sia `StepUpUserLogin`) ereditano una semplice macchina a stati. In qualsiasi dato momento, il controllo di sicurezza può essere in uno di questi stati:

* `STATE_ATTEMPTING`: è stata inviata una verifica e il controllo di sicurezza è in attesa della risposta del client. Il conteggio dei tentativi viene mantenuto durante questo stato.
* `STATE_SUCCESS`: le credenziali sono state convalidate correttamente.
* `STATE_BLOCKED`: il numero massimo di tentativi è stato raggiunto e il controllo è nello stato di bloccato.

Lo stato corrente può essere ottenuto utilizzando il metodo `getState()` ereditato.

In `StepUpUserLogin`, aggiungi un metodo di convenienza per controllare se l'utente è attualmente collegato. Questo metodo viene utilizzato in seguito nell'esercitazione.

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## Il metodo authorize
{: #the-authorize-method}

L'interfaccia `SecurityCheck` definisce un metodo denominato `authorize`. Questo metodo è responsabile dell'implementazione della logica principale del controllo di sicurezza, quale l'invio di una verifica o la convalida della richiesta.

La classe `CredentialsValidationSecurityCheck`, che è estesa da `StepUpPinCode`, già include un'implementazione per questo metodo. Tuttavia, in questo caso, l'obiettivo è controllare lo stato di StepUpUserLogin prima di avviare la modalità di funzionamento predefinita del metodo `authorize`.

Per eseguire tale operazione, **sovrascrivi** il metodo `authorize`:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

Questa implementazione controlla lo stato corrente del riferimento `StepUpUserLogin`:

* Se lo stato è `STATE_SUCCESS` (l'utente è collegato), il flusso normale del controllo di sicurezza continua.
* Se `StepUpUserLogin` è in qualsiasi altro stato, non viene eseguita alcuna operazione: non viene inviata alcuna verifica, né un esito positivo o negativo.

Presumendo che la risorsa sia protetta **sia** da `StepUpPinCode` che da `StepUpUserLogin`, questo flusso garantisce che l'utente sia collegato prima che gli venga richiesta la credenziale secondaria (codice PIN). Il client non riceve entrambe le verifiche contemporaneamente, anche se sono attivati entrambi i controlli di sicurezza.

In alternativa, se la risorsa è protetta **solo** da `StepUpPinCode` (il framework attiverà solo questo controllo di sicurezza), puoi modificare l'implementazione `authorize` per attivare `StepUpUserLogin` manualmente:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        //If StepUpUserLogin is successful, continue the normal processing of StepUpPinCode
        super.authorize(scope, credentials, request, response);
    } else {
        //In any other case, process StepUpUserLogin instead.
        userLogin.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

## Richiama l'utente corrente
{: #retrieve-current-user}

Nel controllo di sicurezza `StepUpPinCode`, sei interessato a conoscere l'ID dell'utente corrente in modo da poter cercare il codice PIN di questo utente in qualche database.

Nel controllo di sicurezza `StepUpUserLogin`, aggiungi il seguente metodo per ottenere l'utente corrente dal **contesto di autorizzazione**:

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

In `StepUpPinCode`, puoi quindi utilizzare il metodo `userLogin.getUser()` per ottenere l'utente corrente dal controllo di sicurezza `StepUpUserLogin` e controllare il codice PIN valido per questo specifico utente:

```java
@Override
protected boolean validateCredentials(Map<String, Object> credentials) {
    //Get the correct PIN code from the database
    User user = userManager.getUser(userLogin.getUser().getId());

    if(credentials!=null && credentials.containsKey(PINCODE_FIELD)){
        String pinCode = credentials.get(PINCODE_FIELD).toString();

        if(pinCode.equals(user.getPinCode())){
            errorMsg = null;
            return true;
        }
        else{
            errorMsg = "Wrong credentials. Hint: " + user.getPinCode();
        }
    }
    return false;
}
```
{: codeblock}

## Gestori delle verifiche
{: #challenge-handlers}

Sul lato client, non ci sono API speciali per gestire più passi. Ogni gestore delle verifiche gestisce piuttosto la sua verifica. In questo esempio, devi registrare due gestori delle verifiche separati: uno per gestire le verifiche da `StepUpUserLogin` e uno per gestire le verifiche da `StepUpPincode`.
