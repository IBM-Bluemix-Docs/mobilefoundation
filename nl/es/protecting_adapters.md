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

# Protección de adaptadores
{: #protecting_adapters}

En el adaptador puede especificar el ámbito de protección para el método Java o un procedimiento de recurso de JavaScript, o para toda una clase de recursos Java, tal y como se indica en las siguientes secciones [Java](#protect-java-adapter-resources) y [JavaScript](#protect-javascript-adapter-resources). Un ámbito se define como una serie de uno o más elementos de ámbito separados por espacios ("scopeElement1 scopeElement2..."), o nulo para aplicar el ámbito predeterminado.

El ámbito MobileFirst predeterminado es `RegisteredClient`, que requiere una señal de acceso para acceder al recurso y verifica que la solicitud de recurso procede de una aplicación que está registrada con el servidor de MobileFirst. Esta protección siempre se aplica, a menos que [inhabilite la protección de recurso](#disabling-resource-protection). Por lo tanto, incluso si no establece un ámbito para el recurso, este sigue protegido.

`RegisteredClient` es una palabra clave reservada de MobileFirst. No defina elementos de ámbito de persona personalizados o comprobaciones de seguridad con este nombre.
{: note}

## Protección de recursos de adaptador Java
{: #protect-java-adapter-resources}

Para asignar un ámbito de protección a un método o clase JAX-RS añada la anotación `@OAuthSecurity` a la declaración de método o clase y establezca el elemento `scope` de la anotación a su ámbito preferido. Sustituya `YOUR_SCOPE` por una serie de uno o más elementos de ámbito (“scopeElement1 scopeElement2 …”):

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

Un ámbito de clase se aplica a todos los métodos de la clase, excepto los métodos que tienen su propia anotación `@OAuthSecurity`.

Cuando el elemento habilitado de la anotación `@OAuthSecurity` se establece en `false`, el elemento de ámbito se ignora. Consulte [Inhabilitación de la protección de recursos Java](#disabling-java-resource-protection).
{: note}

### Ejemplos
{: #protect-java-adap-res-example}
El código siguiente protege un método `helloUser` con un ámbito que contiene los elementos de ámbito `UserAuthentication` y `Pincode`:

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

El código siguiente protege una clase `WebSphereResources` con la comprobación de seguridad predefinida `LtpaBasedSSO`:

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

## Protección de recursos de adaptador JavaScript
{: #protect-javascript-adapter-resources}

Para asignar un ámbito de protección a un procedimiento JavaScript, en el archivo **adapter.xml**, establezca el atributo de ámbito del elemento <procedure> en el ámbito que prefiera. Sustituya `PROCEDURE_NANE` por el nombre del procedimiento y `YOUR SCOPE` por una serie de uno o más elementos de ámbito (“scopeElement1 scopeElement2 …”):

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

Cuando el atributo `secured` del elemento <procedure> se establece en false, el atributo `scope` se ignora. Consulte [Inhabilitación de la protección de recursos JavaScript](#disabling-javascript-resource-protection).
{: note}

### Ejemplo
{: #protect-javascript-adap-res-example}

El código siguiente protege un método `userName` con un ámbito que contiene los elementos de ámbito `UserAuthentication` y `Pincode`:

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## Inhabilitación de la protección de recursos de adaptador
{: #disabling-adapter-resource-protection}

Puede inhabilitar la [protección de recursos de MobileFirst predeterminada](#protecting_adapters_resources) para un recurso de adaptador Java o JavaScript específico o para toda una clase Java, tal y como se indica en las siguientes secciones Java y JavaScript. Cuando la protección de recursos está inhabilitada, la infraestructura de seguridad de MobileFirst no requiere una señal para acceder al recurso.

### Inhabilitación de la protección de recurso Java
{: #disabling-java-resource-protection}

Para inhabilitar una protección OAuth para el método o clase de recurso Java, añada la anotación `@OAuthSecurity` a la declaración de clase o recurso y establezca el valor del elemento `enabled` en `false`:

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

El valor predeterminado del elemento `enabled` de la anotación es `true`. Cuando el elemento `enabled` se establezca en `false`, el elemento `scope` se ignora y el recurso o clase de recurso queda desprotegido.

Cuando asigne un ámbito a un método de una clase desprotegida, el método se protege pese a la anotación de clase, a menos que establezca el elemento `enabled` de la anotación de recurso en `false`.
{: note}

#### Ejemplos
{: #disble-java-adap-res-example}

El código siguiente inhabilita la protección de recurso de un método `helloUser`:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

El código siguiente inhabilita la protección de un recurso para la clase `MyUnprotectedResources`:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### Inhabilitación de la protección de recurso JavaScript
{: #disabling-javascript-resource-protection}

Para inhabilitar por completo la protección OAuth para un recurso de adaptador de JavaScript (procedimiento), en el archivo **adapter.xml**, establezca el atributo `secured` del elemento <procedure> en `false`:

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

Cuando el atributo `secured` se establece en `false`, el atributo `scope` se ignora y el recurso queda desprotegido.

#### Ejemplo
{: #disable-javascript-res-prot-example}

El código siguiente inhabilita la protección de recurso para un procedimiento `userName`:

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## Recursos desprotegidos
{: #unprotected-resources}

Un recurso desprotegido es un recurso que no requiere una señal de acceso. La infraestructura de seguridad de MobileFirst no gestiona el acceso a recursos no protegidos, y no valida ni comprueba la identidad de los clientes que acceden a estos recursos. Por lo tanto, no se da soporte a las funciones como Direct Update, bloqueo del acceso a un dispositivo o la inhabilitación remota de una aplicación en los recursos desprotegidos.
