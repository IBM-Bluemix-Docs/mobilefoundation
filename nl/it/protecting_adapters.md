---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, adapter security

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

# Protezione degli adattatori
{: #protecting_adapters}

Nel tuo adattatore, puoi specificare l'ambito di protezione per un metodo Java o una procedura di risorsa JavaScript oppure per un'intera classe di risorse Java, come descritto nelle seguenti sezioni [Java](#protect-java-adapter-resources) e [JavaScript](#protect-javascript-adapter-resources). Un ambito è definito come una stringa di uno o più elementi di ambito separati da spazi (“scopeElement1 scopeElement2 …”) oppure null per applicare l'ambito predefinito.

L'ambito MobileFirst predefinito è `RegisteredClient`, che richiede un token di accesso per accedere alla risorsa e verifica che la richiesta di risorsa venga da un'applicazione registrata presso MobileFirst Server. La protezione viene sempre applicata, a meno che tu non [disabiliti la protezione delle risorse](#disabling-resource-protection). Pertanto, anche se non imposti un ambito per la tua risorsa, essa è comunque protetta.

`RegisteredClient` è una parola chiave MobileFirst riservata. Non definire controlli di sicurezza o elementi di ambito personalizzati con questo nome.
{: note}

## Protezione delle risorse dell'adattatore Java
{: #protect-java-adapter-resources}

Per assegnare un ambito di protezione a una classe o a un metodo JAX-RS, aggiungi l'annotazione `@OAuthSecurity` o alla dichiarazione di classe o metodo e imposta l'elemento `scope` dell'annotazione sul tuo ambito preferito. Sostituisci `YOUR_SCOPE` con una stringa di uno o più elementi di ambito (“scopeElement1 scopeElement2 …”):

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

Un ambito di classe si applica a tutti i metodi nella classe, fatta eccezione per i metodi che hanno una loro annotazione `@OAuthSecurity`.

Quando l'elemento enabled dell'annotazione `@OAuthSecurity` è impostato su `false`, l'elemento di ambito viene ignorato. Vedi [Disabilitazione della protezione delle risorse Java](#disabling-java-resource-protection).
{: note}

### Esempi
{: #protect-java-adap-res-example}
Il seguente codice protegge un metodo `helloUser` con un ambito che contiene gli elementi di ambito `UserAuthentication` e `Pincode`:

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

Il seguente codice protegge una classe `WebSphereResources` con il controllo di sicurezza `LtpaBasedSSO` predefinito:

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

## Protezione delle risorse dell'adattatore JavaScript
{: #protect-javascript-adapter-resources}

Per assegnare un ambito di protezione a una procedura JavaScript, nel file **adapter.xml** imposta l'attributo di ambito dell'elemento <procedure> sul tuo ambito preferito. Sostituisci `PROCEDURE_NANE` con il nome della tua procedura e `YOUR SCOPE` con una stringa di uno o più elementi di ambito (“scopeElement1 scopeElement2 …”):

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

Quando l'attributo `secured` dell'elemento <procedure> è impostato su false, l'attributo `scope` viene ignorato. Vedi [Disabilitazione della protezione delle risorse JavaScript](#disabling-javascript-resource-protection).
{: note}

### Esempio
{: #protect-javascript-adap-res-example}

Il seguente codice protegge una procedura `userName` con un ambito che contiene gli elementi di ambito `UserAuthentication` e `Pincode`:

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## Disabilitazione della protezione delle risorse dell'adattatore
{: #disabling-adapter-resource-protection}

Puoi disabilitare la [protezione delle risorse MobileFirst predefinita](#protecting_adapters_resources) per una specifica risorsa dell'adattatore Java o JavaScript o per un'intera classe Java, come descritto nelle seguenti sezioni Java e JavaScript. Quando la protezione delle risorse è disabilitata, il framework di sicurezza MobileFirst non richiede un token per accedere alla risorsa.

### Disabilitazione della protezione delle risorse Java
{: #disabling-java-resource-protection}

Per disabilitare completamente la protezione OAuth per un metodo di risorsa o una classe di risorse Java, aggiungi l'annotazione `@OAuthSecurity` alla dichiarazione di risorsa o classe e imposta il valore dell'elemento `enabled` su `false`:

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

Il valore predefinito dell'elemento `enabled` dell'annotazione è `true`. Quando l'elemento `enabled` è impostato su `false`, l'elemento `scope` viene ignorato e la risorsa o la classe di risorse non sono protette.

Quando assegni un ambito a un metodo di una classe non protetta, il metodo viene protetto indipendentemente dall'annotazione della classe a meno che tu non imposti l'elemento `enabled` dell'annotazione della risorsa su `false`.
{: note}

#### Esempi
{: #disble-java-adap-res-example}

Il seguente codice disabilita la protezione delle risorse per un metodo `helloUser`:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

Il seguente codice disabilita la protezione delle risorse per una classe `MyUnprotectedResources`:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### Disabilitazione della protezione delle risorse JavaScript
{: #disabling-javascript-resource-protection}

Per disabilitare completamente la protezione OAuth per una risorsa dell'adattatore JavaScript (procedura), nel file **adapter.xml** imposta l'attributo `secured` dell'elemento <procedure> su `false`:

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

Quando l'attributo `secured` è impostato su `false`, l'attributo `scope` viene ignorato e la risorsa non è protetta.

#### Esempio
{: #disable-javascript-res-prot-example}

Il seguente codice disabilita la protezione delle risorse per una procedura `userName`:

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## Risorse non protette
{: #unprotected-resources}

Una risorsa non protetta è una risorsa che non richiede un token di accesso. Il framework di sicurezza MobileFirst non gestisce l'accesso alle risorse non protette e non convalida o controlla l'identità dei client che accedono a tali risorse. Pertanto, funzioni quali Direct Update, il blocco dell'accesso ai dispositivi o la disabilitazione in remoto di un'applicazione non sono supportate per le risorse non protette.
