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

# Erweiterte Authentifizierung
{: #step_up_authentication}

Ressourcen können durch mehrere Sicherheitsprüfungen geschützt werden. In einem solchen Szenario sendet MobileFirst Server alle relevanten Abfragen gleichzeitig an die Anwendung.

Eine Sicherheitsprüfung kann von einer anderen Sicherheitsprüfung abhängig sein. Daher ist es wichtig, dass gesteuert werden kann, wann die Abfragen gesendet werden.

In diesem Lernprogramm ist beispielsweise eine Anwendung beschrieben, deren beide Ressourcen mit einem Benutzernamen und einem Kennwort geschützt sind. Für die zweite Ressource ist zudem ein PIN-Code erforderlich.

## Sicherheitsprüfung referenzieren
{: #referencing-a-security-check}

Erstellen Sie zwei Sicherheitsprüfungen: `StepUpPinCode` und `StepUpUserLogin`.

In diesem Beispiel hängt `StepUpPinCode` von `StepUpUserLogin` ab. Der Benutzer sollte erst nach einer erfolgreichen Anmeldung bei `StepUpUserLogin` aufgefordert werden, einen PIN-Code einzugeben. Zu diesem Zweck muss `StepUpPinCode`  in der Lage sein, die Klasse `StepUpUserLogin` zu referenzieren.

Das MobileFirst-Framework stellt eine Annotation zum Einfügen einer Referenz bereit.

Fügen Sie zu Ihrer Klasse `StepUpPinCode` auf der Klassenebene Folgendes hinzu:

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

**Wichtig**: Beide Implementierungen von Sicherheitsprüfungen müssen in einem Adapter gebündelt werden.
{.important}

Zum Auflösen dieser Referenz sucht das Framework nach einer Sicherheitsprüfung mit der entsprechenden Klasse und fügt diese Referenz in die abhängige Sicherheitsprüfung ein.

Für den Fall, dass es für eine Klasse mehr als eine Sicherheitsprüfung gibt, hat die Annotation einen optionalen Namensparameter, mit dem Sie einen eindeutigen Namen der referenzierten Überprüfung angeben können.

## Zustandsmaschine
{: #state-machine}

Alle Klassen, die `CredentialsValidationSecurityCheck` erweitern (einschließlich `StepUpPinCode` und `StepUpUserLogin`), übernehmen eine einfache Zustandsmaschine. Die Sicherheitsprüfung kann sich zu jedem bestimmten Zeitpunkt in einem der folgenden Zustände befinden:

* `STATE_ATTEMPTING`: Eine Abfrage wurde gesendet und die Sicherheitsprüfung wartet auf die Clientantwort. Während dieses Zustands wird die Anzahl der Versuche gezählt.
* `STATE_SUCCESS`: Die Berechtigungsnachweise wurden erfolgreich geprüft.
* `STATE_BLOCKED`: Die maximale Anzahl von Versuchen ist erreicht und die Überprüfung wird gesperrt.

Der aktuelle Status kann mit der übernommenen Methode `getState()` abgerufen werden.

Fügen Sie in `StepUpUserLogin` eine Methode zur Vereinfachung hinzu, mit der Sie ohne großen Aufwand überprüfen können, ob der Benutzer zurzeit angemeldet ist. Diese Methode wird später im Lernprogramm verwendet.

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## Methode 'authorize'
{: #the-authorize-method}

Die Schnittstelle `SecurityCheck` definiert eine Methode mit der Bezeichnung `authorize`. Diese Methode ist für die Implementierung der Hauptlogik der Sicherheitsprüfung verantwortlich, z. B. für das Senden einer Abfrage oder für die Validierung der Anforderung.

Die Klasse `CredentialsValidationSecurityCheck`, die von `StepUpPinCode` erweitert wird, enthält bereits eine Implementierung für diese Methode. In diesem Fall ist es jedoch das Ziel, den Zustand von 'StepUpUserLogin' zu überprüfen, bevor das Standardverhalten der Methode `authorize` gestartet wird.

Dafür müssen Sie die Methode `authorize` **überschreiben**:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

Diese Implementierung überprüft den aktuellen Zustand der `StepUpUserLogin`-Referenz:

* Wenn der Zustand `STATE_SUCCESS` lautet (d. h., der Benutzer ist angemeldet), wird der normale Ablauf der Sicherheitsprüfung fortgesetzt.
* Wenn sich `StepUpUserLogin` in einem anderen Zustand befindet, wird kein Schritt ausgeführt. Das bedeutet, es wird keine Abfrage gesendet und es gibt keine Erfolgs- oder Fehlermeldung.

Angenommen, die Ressource wird **sowohl** durch `StepUpPinCode` als auch durch `StepUpUserLogin` geschützt. In dem Fall stellt der Ablauf sicher, dass der Benutzer angemeldet ist, bevor er zur Eingabe eines zweiten Berechtigungsnachweises (des PIN-Codes) aufgefordert wird. Der Client emfpängt beide Abfragen nie zur selben Zeit, auch wenn beide Sicherheitsprüfungen aktiviert sind.

Wenn die Ressource dagegen **nur** durch `StepUpPinCode` geschützt wird (das Framework also nur diese Sicherheitsprüfung aktiviert), können Sie die Implementierung von `authorize` so ändern, dass `StepUpUserLogin` manuell ausgelöst wird:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        //Wenn StepUpUserLogin erfolgreich ist, mit normaler Verarbeitung von StepUpPinCode fortfahren
        super.authorize(scope, credentials, request, response);
    } else {
        //In allen anderen Fällen stattdessen StepUpUserLogin verarbeiten
        userLogin.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

## Aktuellen Benutzer abrufen
{: #retrieve-current-user}

On der Sicherheitsprüfung `StepUpPinCode` geht es darum, die ID des aktuellen Benutzers zu erfahren, damit in einer Datenbank nach dem PIN-Code dieses Benutzers gesucht werden kann.

Fügen Sie in der Sicherheitsprüfung `StepUpUserLogin` die folgende Methode hinzu, um den aktuellen Benutzer aus dem **Autorisierungskontext** abzurufen:

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

In `StepUpPinCode` können Sie dann die Methode `userLogin.getUser()` verwenden, um den aktuellen Benutzer aus der Sicherheitsprüfung `StepUpUserLogin` abzurufen und den gültigen PIN-Code für diesen konkreten Benutzer zu überprüfen:

```java
@Override
protected boolean validateCredentials(Map<String, Object> credentials) {
    //Richtigen PIN-Code aus der Datenbank abrufen
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

## Abfrage-Handler
{: #challenge-handlers}

Auf der Clientseite gibt es keine spezifischen APIs für die Handhabung mehrerer Schritte. Vielmehr handhabt jeder Abfrage-Handler seine eigenen Abfragen. In diesem Beispiel müssen Sie zwei separate Abfrage-Handler registrieren, einen für die Abfragen von `StepUpUserLogin` und einen für die Abfragen von `StepUpPincode`.
