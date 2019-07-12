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

# Adapter schützen
{: #protecting_adapters}

In Ihrem Adapter können Sie einen Schutzbereich für die Prozedur einer Java-Methode oder einer JavaScript-Ressource oder für eine ganze Java-Ressourcenklasse angeben, wie in den folgenden Abschnitten zu [Java](#protect-java-adapter-resources) und [JavaScript](#protect-javascript-adapter-resources) beschrieben. Ein Bereich ist als eine Zeichenfolge aus einem oder mehreren durch Leerzeichen getrennten Bereichselementen (“Bereichselement1 Bereichselement2 …”) definiert, oder null, um den Standardbereich anzuwenden.

Der MobileFirst-Standardbereich ist `RegisteredClient`, für den ein Zugriffstoken für den Zugriff auf die Ressource erforderlich ist, und der prüft, ob die Ressourcenanforderung von einer Anwendung ist, die bei MobileFirst Server registriert ist. Dieser Schutz wird immer angewendet, es sei denn, Sie [inaktivieren den Ressourcenschutz](#disabling-resource-protection). Daher ist die Ressource auch dann geschützt, wenn Sie keinen Bereich für die Ressource festlegen.

`RegisteredClient` ist ein reserviertes MobileFirst-Schlüsselwort. Definieren Sie keine angepassten Bereichselemente oder Sicherheitsprüfungen mit diesem Namen.
{: note}

## Java-Adapterressourcen schützen
{: #protect-java-adapter-resources}

Um einen Schutzbereich einer JAX-RS-Methode oder -Klasse zuzuordnen, fügen Sie die Annotation `@OAuthSecurity` der Methoden- oder Klassendeklaration hinzu und setzen Sie das Element `scope` der Annotation auf Ihren bevorzugten Bereich. Ersetzen Sie `YOUR_SCOPE` durch eine Zeichenfolge aus einem oder mehreren Bereichselementen (“Bereichselement1 Bereichselement2 …”):

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

Ein Klassenbereich gilt für alle Methoden in der Klasse, mit Ausnahme der Methoden, die über eine eigene Annotation `@OAuthSecurity` verfügen.

Wenn für das aktivierte Element der Anmerkung `@OAuthSecurity` der Wert `false` festgelegt ist, wird das Bereichselement ignoriert. Weitere Informationen finden Sie im Abschnitt [Java-Ressourcenschutz inaktivieren](#disabling-java-resource-protection).
{: note}

### Beispiel
{: #protect-java-adap-res-example}
Der folgende Code schützt eine `helloUser`-Methode mit einem Bereich, der die Bereichselemente `UserAuthentication` und `Pincode` enthält:

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

Der folgende Code schützt eine `WebSphereResources`-Klasse mit der vordefinierten Sicherheitsprüfung `LtpaBasedSSO`:

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

## JavaScript-Adapterressourcen schützen
{: #protect-javascript-adapter-resources}

Um einen Schutzbereich einer JavaScript-Prozedur zuzuordnen, setzen Sie in der Datei **adapter.xml** das Bereichsattribut ('scope') des Elements <procedure> auf Ihren bevorzugten Bereich. Ersetzen Sie `PROCEDURE_NAME` durch den Namen Ihrer Prozedur und `YOUR_SCOPE` durch eine Zeichenfolge aus einem oder mehreren Bereichselementen (“Bereichselement1 Bereichselement2 …”):

```javascript
<procedure name="PROCEDURE_NAME" scope="YOUR_SCOPE">
```
{: codeblock}

Wenn für das Attribut `secured` des Elements <procedure> der Wert 'false' festgelegt ist, wird das Attribut `scope` ignoriert. Weitere Informationen finden Sie im Abschnitt [Java-Ressourcenschutz inaktivieren](#disabling-javascript-resource-protection).
{: note}

### Beispiel
{: #protect-javascript-adap-res-example}

Der folgende Code schützt eine `userName`-Prozedur mit einem Bereich, der die Bereichselemente `UserAuthentication` und `Pincode` enthält:

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## Adapterressourcenschutz inaktivieren
{: #disabling-adapter-resource-protection}

Sie können den [MobileFirst-Standardressourcenschutz](#protecting_adapters_resources) für eine bestimmte Java- oder JavaScript-Adapterressource oder für eine ganze Java-Klasse inaktivieren, wie in den folgenden Abschnitten zu Java und JavaScript beschrieben. Wenn der Ressourcenschutz inaktiviert ist, ist für das Sicherheitsframework von MobileFirst kein Token für den Zugriff auf die Ressource erforderlich.

### Java-Ressourcenschutz inaktivieren
{: #disabling-java-resource-protection}

Um den OAuth-Schutz für eine Java-Ressourcenmethode oder -klasse vollständig zu inaktivieren, fügen Sie die Annotation `@OAuthSecurity` der Ressourcen- oder Klassendeklaration hinzu und setzen Sie den Wert des Elements `enabled` auf `false`:

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

Der Standardwert für das Element `enabled` der Annotation ist `true`. Wenn das Element `enabled` auf `false` gesetzt ist, wird das Element `scope` ignoriert und die Ressource oder Ressourcenklasse ist ungeschützt.

Wenn Sie einem Bereich eine Methode einer ungeschützten Klasse zuordnen, wird die Methode trotz der Klassenannotation geschützt, es sei denn, Sie setzen das Element `enabled` der Ressourcenannotation auf `false`.
{: note}

#### Beispiele
{: #disble-java-adap-res-example}

Mit dem folgenden Code wird der Ressourcenschutz für eine Methode `helloUser` inaktiviert:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

Mit dem folgenden Code wird der Ressourcenschutz für eine Klasse `MyUnprotectedResources` inaktiviert:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### JavaScript-Ressourcenschutz inaktivieren
{: #disabling-javascript-resource-protection}

Um den OAuth-Schutz für eine JavaScript-Adapterressource (Prozedur) vollständig zu inaktivieren, setzen Sie in der Datei **adapter.xml** das Attribut `secured` des Elements <procedure> auf `false`:

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

Wenn das Attribut `secured` auf `false` gesetzt ist, wird das Attribut `scope` ignoriert und die Ressource ist ungeschützt.

#### Beispiel
{: #disable-javascript-res-prot-example}

Mit dem folgenden Code wird der Ressourcenschutz für eine Prozedur `userName` inaktiviert:

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## Nicht geschützte Ressourcen
{: #unprotected-resources}

Eine ungeschützte Ressource ist eine Ressource, für die kein Zugriffstoken erforderlich ist. Das Sicherheitsframework von MobileFirst verwaltet den Zugriff auf ungeschützte Ressourcen nicht und prüft auch nicht die Identität von Clients, die auf diese Ressourcen zugreifen. Daher werden Funktionen wie Direct Update, das Blockieren des Gerätezugriffs oder das Inaktivieren einer Anwendung über Fernzugriff für ungeschützte Ressourcen nicht unterstützt.
