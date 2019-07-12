---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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


# Authentifizierung und Sicherheit
{: #basic_authentication}

Das Sicherheitsframework MobileFirst basiert auf dem [OAuth 2.0](http://oauth.net/)-Protokoll. Gemäß diesem Protokoll kann eine Ressource durch einen **Bereich** geschützt werden, der die erforderlichen Berechtigungen für den Zugriff auf die Ressource definiert. Um auf eine geschützte Ressource zugreifen zu können, muss der Client ein entsprechendes **Zugriffstoken** bereitstellen, in das der Bereich für die dem Client gewährte Autorisierung eingebunden ist.

Das OAuth-Protokoll trennt die Rolle des Autorisierungsservers von der Rolle des Ressourcenservers, der die Ressourcen bereitstellt.

* Der Autorisierungsserver verwaltet die Clientberechtigungen und die Tokengenerierung.
* Der Ressourcenserver verwendet den Autorisierungsserver, um das vom Client bereitgestellte Zugriffstoken zu validieren und um sicherzustellen, dass das Token zum Schutzbereich der angeforderten Ressource passt.

Im Zentrum des Sicherheitsframeworks steht ein Autorisierungsserver, der das OAuth-Protokoll implementiert und die OAuth-Endpunkte, mit denen der Client beim Anfordern von Zugriffstoken interagiert, zugänglich macht. Das Sicherheitsframework enthält die logischen Bausteine für die Implementierung einer angepassten Autorisierungslogik für den Autorisierungsserver und das zugrunde liegende OAuth-Protokoll. MobileFirst Server fungiert standardmäßig auch als **Autorisierungsserver**. Sie können aber auch eine IBM WebSphere DataPower-Appliance als Autorisierungsserver konfigurieren, der mit MobileFirst Server interagiert.

Die Clientanwendung kann diese Token dann für den Zugriff auf Ressourcen eines **Ressourcenservers** verwenden, bei dem es sich um MobileFirst Server oder um einen externen Server handeln kann. Der Ressourcenserver überprüft die Gültigkeit des Tokens, um sicherzustellen, dass dem Client der Zugriff auf die angeforderte Ressource gewährt werden kann. Durch die Trennung von Ressourcenserver und Autorisierungsserver wird die Sicherheit für Ressourcen durchgesetzt, die außerhalb von MobileFirst Server ausgeführt werden.

Anwendungsentwickler schützen den Zugriff auf Ressourcen, indem sie für jede Ressource den erforderlichen Bereich definieren und die Sicherheitsprüfungen und Abfrage-Handler implementieren. Das serverseitige Sicherheitsframework und die clientseitige API kümmern sich transparent um den OAuth-Nachrichtenaustausch und die Interaktion mit dem Autorisierungsserver, sodass sich Entwickler vollständig auf die Autorisierungslogik konzentrieren können.

## Autorisierungsentitäten
{: #acs_authorization_entitiesty}

### Zugriffstokens
{: #acs_access_tokens}

Ein MobileFirst-Zugriffstoken ist eine digital signierte Entität, die die Autorisierungsberechtigungen für einen Client beschreibt. Wenn der Autorisierungsanforderung des Clients für einen bestimmten Bereich entsprochen und der Client authentifiziert wurde, sendet der Tokenendpunkt des Autorisierungsservers eine HTTP-Antwort mit dem angeforderten Zugriffstoken an den Client.

#### Struktur des Zugriffstokens

Das MobileFirst-Zugriffstoken enthält die folgenden Informationen:

* **Client-ID**: Eine eindeutige Kennung des Clients.
* **Bereich**: Der Bereich, für den das Token ausgestellt wurde (siehe "OAuth-Bereiche"). Dieser Bereich umfasst nicht den obligatorischen Anwendungsbereich.
* **Tokenablaufzeit**: Die Zeit (in Sekunden), nach der das Token ungültig wird (abläuft).

#### Tokenablaufzeit
{: #token-expiration}

Das ausgestellte Zugriffstoken bleibt gültig, bis der Ablaufzeitpunkt erreicht ist. Die Ablaufzeit des Zugriffstokens ist, verglichen mit den Ablaufzeiten aller Sicherheitsprüfungen im Bereich, die kürzeste. Wenn jedoch die kürzeste Ablaufzeit länger als der maximale Tokenablaufzeitraum der Anwendung ist, wird die Ablaufzeit des Tokens auf die aktuelle Zeit zuzüglich des maximalen Ablaufzeitraums gesetzt. Der Standardwert für den maximalen Tokenablaufzeitraum (d. h. die maximale Gültigkeitsdauer) liegt bei 3.600 Sekunden (1 Stunde). Der Wert kann jedoch konfiguriert werden, indem der Wert der Eigenschaft ``maxTokenExpiration`` angegeben wird.

##### Maximalen Ablaufzeitzeitraum für Zugriffstoken konfigurieren
{: #acs_config-max-access-tokens}

Wählen Sie eine der folgenden alternativen Methoden aus, um für die Anwendung den maximalen Ablaufzeitraum für Zugriffstoken zu konfigurieren:

* Über die MobileFirst Operations Console
    1. Wählen Sie **[Ihre Anwendung]** → **Sicherheit** aus.
    2. Setzen Sie im Abschnitt **Tokenkonfiguration** das Feld **Maximaler Tokanablaufzeitraum (Sekunden)** auf den gewünschten Wert und klicken Sie auf **Speichern**. Sie können diesen Schritt jederzeit wiederholen, um den maximalen Tokenablaufzeitraum zu ändern, oder **Standardwerte wiederherstellen** auswählen, um den Standardwert wiederherzustellen.

* Konfigurationsdatei der Anwendung bearbeiten

    1. Navigieren Sie in einer Befehlszeilenschnittstelle zum Stammordner des Projekts und führen Sie den folgenden Befehl aus.
      ```bash
      mfpdev app pull
      ```
    2. Öffnen Sie die Konfigurationsdatei, die sich im Ordner `[Projektordner]\mobilefirst` befindet.
    3. Bearbeiten Sie die Datei. Definieren Sie dazu die Eigenschaft `maxTokenExpiration` und setzen Sie sie auf den Wert für den maximalen Ablaufzeitraum für Zugriffstoken (in Sekunden):
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. Implementieren Sie die aktualisierte JSON-Konfigurationsdatei. Führen Sie dazu den Befehl ``mfpdev app push`` aus.

##### Antwortstruktur für Zugriffstokens
{: #acs_access-tokens-structure}

Eine erfolgreiche HTTP-Antwort auf eine Zugriffstokenanforderung enthält ein JSON-Objekt mit dem Zugriffstoken und zusätzlichen Daten. Das folgende Beispiel zeigt eine Antwort mit gültigem Token vom Autorisierungsserver:

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

Das JSON-Objekt für die Tokenantwort weist die folgenden Eigenschaftsobjekte auf:

* **token_type**: Der Tokentyp ist immer "*Bearer*" in Übereinstimmung mit den Informationen zur [Spezifikation für die Bearer-Tokenverwendung des OAuth 2.0-Autorisierungsframeworks](https://tools.ietf.org/html/rfc6750).
* **expires_in**: Die Ablaufzeit des Zugriffstokens in Sekunden.
* **access_token**: Das generierte Zugriffstoken (die tatsächlichen Zugriffstokens sind länger als im Beispiel gezeigt).
* **scope**: Der angeforderte Bereich.

Die Informationen für **expires_in** und **scope** sind auch in dem Token selbst enthalten (**access_token**).

**Hinweis**: Die Struktur einer gültigen Zugriffstokenantwort ist relevant, wenn Sie die untergeordnete Klasse `WLAuthorizationManager` verwenden und selbst die OAuth-Interaktion zwischen dem Client und dem Autorisierungsserver sowie den Ressourcenservern verwalten oder einen vertraulichen Client verwenden. Falls Sie die übergeordnete Klasse `WLResourceRequest` verwenden, die den OAuth-Ablauf für den Zugriff auf geschützte Ressourcen einbindet, verarbeitet das Sicherheitsframework die Zugriffstokenantworten für Sie. Weitere Informationen finden Sie in [APIs für die Clientsicherheit](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) und [Vertrauliche Clients](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).

### Aktualisierungstokens
{: #acs_refresh_tokens}

Ein Aktualisierungstoken ist eine besondere Art von Token. Wenn das Zugriffstoken abgelaufen ist, kann das Aktualisierungstoken verwendet werden, um ein neues Zugriffstoken anzufordern. Für die Anforderung eines neuen Zugriffstokens kann ein gültiges Aktualisierungstoken vorgelegt werden. Aktualisierungstokens sind langlebige Token, die länger als Zugriffstokens gültig bleiben.

Eine Anwendung muss Vorsicht walten lassen, wenn sie Aktualisierungstokens verwendet, da ein Benutzer mit einem solchen Token unbegrenzte Zeit authentifiziert bleiben kann. Social-Media-Anwendungen, E-Commerce-Anwendungen, Produktkataloganzeigen und Dienstprogrammanwendungen, bei denen Benutzer nicht regelmäßig vom Anwendungsprovider authentifiziert werden, können Aktualisierungstokens verwenden. Anwendungen, die eine regelmäßige Benutzerauthentifizierung erfordern, sollten die Verwendung von Aktualisierungstoken vermeiden.
MobileFirst-Aktualisierungstoken

Ein MobileFirst-Aktualisierungstoken ist eine digital signierte Entität wie ein Zugriffstoken, die die Autorisierungsberechtigungen eines Clients beschreibt. Aktualisierungstokens können verwendet werden, um neue Zugriffstoken desselben Bereichs zu erhalten. Wenn der Autorisierungsanforderung des Clients für einen bestimmten Bereich entsprochen und der Client authentifiziert wurde, sendet der Tokenendpunkt des Autorisierungsservers eine HTTP-Antwort mit dem angeforderten Zugriffstoken- und Aktualisierungstoken an den Client. Wenn das Zugriffstoken abläuft, sendet der Client ein Aktualisierungstoken an den Tokenendpunkt des Autorisierungsservers, um ein neues Zugriffstoken und ein neues Aktualisierungstoken zu erhalten.

#### Struktur des Aktualisierungstokens
{: #structure_refresh_tokens}

Das MobileFirst-Aktualisierungstoken enthält ähnlich wie ein MobileFirst-Zugriffstoken die folgenden Informationen:

* **Client-ID**: Eine eindeutige Kennung des Clients.
* **Bereich**: Der Bereich, für den das Token ausgestellt wurde (siehe "OAuth-Bereiche"). Dieser Bereich umfasst nicht den obligatorischen Anwendungsbereich.
* **Tokenablaufzeit**: Die Zeit (in Sekunden), nach der das Token ungültig wird (abläuft).

##### Tokenablaufzeit
{: #str-token-expiration}

Der Ablaufzeitraum für Aktualisierungstokens ist länger als der normale Ablaufzeitraum für Zugriffstokens. Ein einmal ausgestelltes Aktualisierungstoken bleibt gültig, bis der Ablaufzeitpunkt erreicht ist. In diesem Gültigkeitszeitraum kann ein Client das Aktualisierungstoken verwenden, um um ein neues Zugriffstoken und ein neues Aktualisierungstoken abzurufen. Der Ablaufzeitraum eines Aktualisierungstokens ist festgelegt und liegt bei 30 Tagen. Jedes Mal, wenn der Client erfolgreich ein neues Zugriffstoken und Aktualisierungstoken erhält, wird der Ablaufzeitraum des Aktualisierungstokens zurückgesetzt, sodass der Client den Eindruck hat, das Token würde nie ablaufen. Die Regeln für die Ablaufzeit des Zugriffstokens entsprechen den im Abschnitt **Zugriffstoken** erläuterten Regeln.

##### Funktion zum Aktivieren des Aktualisierungstokens
{: #acs_enable-refresh-token}

Aktualisierungstokens können mit den folgenden Eigenschaften jeweils auf der Client- und der Serverseite aktiviert werden.

Clientseitige Eigenschaft (Android):
*Dateiname*: mfpclient.properties
*Eigenschaftsname*: wlEnableRefreshToken
*Eigenschaftswert*: true
Beispiel:
*wlEnableRefreshToken*=true

Clientseitige Eigenschaft (iOS):
*Dateiname*: mfpclient.plist
*Eigenschaftsname*: wlEnableRefreshToken
*Eigenschaftswert*: true
Beispiel:
*wlEnableRefreshToken*=true

Serverseitige Eigenschaft:
*Dateiname*: server.xml
*Eigenschaftsname*: mfp.security.refreshtoken.enabled.apps
*Eigenschaftswert*: application bundle id separated by ‘;’

Beispiel:

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
Verwenden Sie unterschiedliche Bundle-IDs für verschiedene Plattformen.

##### Antwortstruktur des Aktualisierungstokens
{: #refresh-token-response-structure}

Das folgende Beispiel zeigt eine gültige Aktualisierungstokenantwort vom Autorisierungsserver:

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

Die Aktualisierungstokenantwort verfügt neben einem Eigenschaftsobjekten, die im Abschnitt zur Struktur der Zugriffstokenantwort erklärt sind, über das Eigenschaftsobjekt `refresh_token`.

**Hinweis**: Die Aktualisierungstokens sind im Vergleich zu Zugriffstokens langlebig. Daher sollten Sie Aktualisierungstokens mit Vorsicht verwenden. Anwendungen, bei denen eine regelmäßige Benutzerauthentifizierung nicht erforderlich ist, sind ideale Kandidaten für die Verwendung von Aktualisierungstokens.

Ab CD-Update 3 unterstützt MobileFirst Aktualisierungstoken für iOS.

#### Sicherheitsprüfungen
{: #acs_securitychecks}

Eine Sicherheitsprüfung ist eine serverseitige Entität, die die Sicherheitslogik zum Schutz serverseitiger Anwendungsressourcen implementiert. Ein einfaches Beispiel für eine Sicherheitsprüfung ist eine Sicherheitsprüfung für die Benutzeranmeldung, die die Berechtigungsnachweise eines Benutzers empfängt und die Berechtigungsnachweise für eine Benutzerregistry überprüft. Ein weiteres Beispiel ist die vordefinierte Sicherheitsprüfung der MobileFirst-Anwendungsauthentizität, bei der die Authentizität der mobilen Anwendung validiert wird und die Anwendungsressourcen vor unbefugtem Zugriff geschützt werden. Dieselbe Sicherheitsprüfung kann genutzt werden, um diverse Ressourcen zu schützen.

Eine Sicherheitsprüfung setzt normalerweise Sicherheitsabfragen ab, auf die der Client in einer bestimmten Weise antworten muss, um die Prüfung zu bestehen. Dieser Handshake erfolgt im Rahmen des OAuth-Ablaufs für die Anforderung eines Zugriffstokens. Der Client verwendet **Abfrage-Handler** für die Behandlung von Abfragen der Sicherheitsprüfungen.

##### Integrierte Sicherheitsprüfungen
{: #builtin-sec-checks}

Die folgenden vordefinierten Sicherheitsprüfungen sind verfügbar:

* Anwendungsauthentizität
* LTPA-basiertes Single Sign-on (SSO)
* Direct Update

#### Challenge-Handler-Entität
{: #challengehandler_entity}

Wenn Sie versuchen, auf eine geschützte Ressource zuzugreifen, kann der Client eine Abfrage erhalten. Bei dieser Abfrage kann es sich um eine Frage, einen Sicherheitstest oder eine Eingabeaufforderung des Servers handeln. Mit der Abfrage soll sichergestellt werden, dass der Client berechtigt ist, auf diese Ressource zuzugreifen. In den meisten Fällen werden im Rahmen der Abfrage Berechtigungsnachweise angefordert, z. B. ein Benutzername und ein Kennwort.

Ein Abfrage-Handler ist eine clientseitige Entität, die die clientseitige Sicherheitslogik und die zugehörige Benutzerinteraktion implementiert.

**Wichtig**: Nachdem eine Abfrage empfangen wurde, kann sie nicht ignoriert werden. Sie muss beantwortet werden oder der laufende Vorgang muss abgebrochen werden. Das Ignorieren einer Abfrage kann zu unerwartetem Verhalten führen.

### Bereiche
{: #scopes}

Sie können Ressourcen, wie z. B. Adapter, vor unbefugtem Zugriff schützen, indem Sie der Ressource einen Bereich zuordnen.

Ein Bereich ist durch eine Zeichenfolge mit einem oder mehreren, jeweils durch ein Leerzeichen getrennten Bereichselement(en) definiert (“Bereichselement1 Bereichselement2 …”) oder auf null gesetzt, wenn der Standardbereich (RegisteredClient) angewendet werden soll. Das MobileFirst-Sicherheitsframework erfordert für jede Adapterressource ein Zugriffstoken. Dies gilt auch dann, wenn der Ressource kein Bereich zugewiesen ist, solange nicht der Ressourcenschutz für die Ressource inaktiviert ist.

#### Bereichselemente
{: #scopeelements}

Ein Bereichselement kann Folgendes sein:

* Der Name einer Sicherheitsprüfung.
* Ein beliebiges Schlüsselwort wie `access-restricted` oder `deletePrivilege`, das die Sicherheitsstufe definiert, die für diese Ressource benötigt wird. Dieses Schlüsselwort wird später einer Sicherheitsprüfung zugeordnet.

#### Bereichszuordnung
{: #scopemapping}

Standardmäßig werden die **Bereichselemente**, die Sie in Ihren **Bereich** schreiben, einer **Sicherheitsprüfung** mit demselben Namen zugeordnet. Wenn Sie z. B. eine Sicherheitsprüfung mit dem Namen `PinCodeAttempts` schreiben, können Sie ein Bereichselement mit demselben Namen in Ihrem Bereich verwenden.

Die Bereichszuordnung ermöglicht die Zuordnung von Bereichselementen zu Sicherheitsprüfungen. Wenn der Client nach einem Bereichselement fragt, definiert diese Konfiguration, welche Sicherheitsprüfungen angewendet werden sollen. Sie können beispielsweise das Bereichselement `access-restricted` Ihrer Sicherheitsprüfung `PinCodeAttempts` zuordnen.

Die Bereichszuordnung ist hilfreich, wenn der Schutz einer Ressource von der Anwendung abhängig sein soll, die versucht, auf die Ressource zuzugreifen. Sie können einen Bereich auch einer Liste mit null oder mehr Sicherheitsprüfungen zuordnen.

Beispiel: scope = `access-restricted deletePrivilege`

* In App A
    * `access-restricted` ist `PinCodeAttempts` zugeordnet.
    * `deletePrivilege` ist einer leeren Zeichenfolge zugeordnet.
* In App B
    * `access-restricted` ist `PinCodeAttempts` zugeordnet.
    * `deletePrivilege` ist `UserLogin` zugeordnet.

Wenn Sie Ihr Bereichselement einer leeren Zeichenfolge zuordnen, wählen Sie im Popup-Menü **Neue Zuordnung von Bereichselementen hinzufügen** keine Sicherheitsprüfung aus.

![Bereichszuordnung](/images/scope_mapping.png "Anzeige 'Neue Zuordnung von Bereichselementen hinzufügen'")

Sie können die JSON-Datei der Anwendung auch manuell mit der erforderlichen Konfiguration bearbeiten und die Änderungen mit Push-Operation zurück an einen MobileFirst-Server übertragen.

1. Navigieren Sie in einem **Befehlszeilenfenster** zum Stammordner des Projekts und führen Sie den Befehl `mfpdev app pull` aus.
2. Öffnen Sie die Konfigurationsdatei, die sich im Ordner `[Projektordner]\mobilefirst` befindet.
3. Bearbeiten Sie die Datei, indem Sie eine Eigenschaft `scopeElementMapping` definieren. Definieren Sie in dieser Eigenschaft Datenpaare, die jeweils aus dem Namen des ausgewählten Bereichselements und einer Zeichenfolge mit null oder mehr durch Leerzeichen getrennten Sicherheitsprüfungen besteht, denen das Element zugeordnet wird. Beispiel:

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. Implementieren Sie die aktualisierte JSON-Konfigurationsdatei, indem Sie den folgenden Befehl ausführen:
  ```bash
  mfpdev app push
  ```

Sie können aktualisierte Konfigurationen auch mit Push-Operation an ferne Server übertragen. Sehen Sie sich dazu das Lernprogramm 'MobileFirst-Artefakte über die MobileFirst-CLI verwalten' an.
{: note}

### Ressourcen schützen
{: #protecting-resources}

Im OAuth-Modell ist eine geschützte Ressource eine Ressource, für die ein Zugriffstoken erforderlich ist. Sie können das MobileFirst-Sicherheitsframework verwenden, um von einer Instanz von MobileFirst Server und von einem externen Server bereitgestellte Ressourcen zu schützen. Für den Schutz einer Ressource weisen Sie ihr einen Bereich zu, der die Berechtigungen definiert, die erforderlich sind, um ein Zugriffstoken für die Ressource beziehen zu können.

Sie können Ihre Ressourcen auf verschiedene Arten schützen:

#### Obligatorischer Anwendungsbereich
{: #mandatoryappscope}

Auf der Anwendungsebene können Sie einen Bereich definieren, der für alle Ressourcen gilt, die von der Anwendung verwendet werden. Das Sicherheitsframework führt diese Prüfungen (sofern vorhanden) zusätzlich zu den Sicherheitsprüfungen des angeforderten Ressourcenbereichs aus.

**Hinweis**:
   * Der obligatorische Anwendungsbereich wird nicht angewendet, wenn Sie auf eine ungeschützte Ressource zugreifen.
   * Das Zugriffstoken, das für den Ressourcenbereich gewährt wird, enthält nicht den obligatorischen Anwendungsbereich.

Wählen Sie in der Navigationsseitenleiste der MobileFirst Operations Console im Abschnitt **Anwendungen** Ihre Anwendung und dann die Registerkarte **Sicherheit** aus. Wählen Sie unter **Obligatorischer Anwendungsbereich** die Option **Zum Bereich hinzufügen** aus.

![Obligatorischer Anwendungsbereich](/images/mandatory-application-scope.png "Anzeige 'Obligatorischen Anwendungsbereich konfigurieren'")

Sie können die JSON-Datei der Anwendung auch manuell mit der erforderlichen Konfiguration bearbeiten und die Änderungen mit Push-Operation zurück an einen MobileFirst-Server übertragen.

1. Navigieren Sie in einem **Befehlszeilenfenster** zum Stammordner des Projekts und führen Sie den Befehl `mfpdev app pull` aus.
2. Öffnen Sie die Konfigurationsdatei, die sich im Ordner **Projektordner\mobilefirst** befindet.
3. Bearbeiten Sie die Datei. Definieren Sie dazu die Eigenschaft `mandatoryScope` und legen Sie als Eigenschaftswert eine Bereichszeichenfolge fest, die Ihre ausgewählten Bereichselemente jeweils mit einem Leerzeichen als Trennzeichen auflistet. Beispiel:

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. Implementieren Sie die aktualisierte JSON-Konfigurationsdatei. Führen Sie dazu den Befehl 'mfpdev app push' aus.

Sie können aktualisierte Konfigurationen auch mit Push-Operation an ferne Server übertragen.

#### Adapterressourcen schützen
{: #protectadapterres}

In Ihrem Adapter können Sie einen Schutzbereich für die Prozedur einer Java-Methode oder einer JavaScript-Ressource oder für eine ganze Java-Ressourcenklasse angeben. Ein Bereich ist als eine Zeichenfolge aus einem oder mehreren durch Leerzeichen getrennten Bereichselementen (“scopeElement1 scopeElement2 …”) definiert, oder null, um den Standardbereich anzuwenden. Weitere Informationen zum Schutz von Adapterressourcen finden Sie im Abschnitt [Adapter schützen](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters).

### Ressourcenschutz inaktivieren
{: #disablingresprotection}

Sie können den MobileFirst-Standardressourcenschutz für eine bestimmte Java- oder JavaScript-Adapterressource oder für eine ganze Java-Klasse inaktivieren, wie in den folgenden Abschnitten zu Java und JavaScript beschrieben. Wenn der Ressourcenschutz inaktiviert ist, ist für das Sicherheitsframework von MobileFirst kein Token für den Zugriff auf die Ressource erforderlich.

#### OAuth-Schutz für Java-Ressourcen inaktivieren
{: #disablejavaresoauthprotection}

Um den OAuth-Schutz für eine Java-Ressourcenmethode oder -klasse vollständig zu inaktivieren, fügen Sie die Annotation `@OAuthSecurity` der Ressourcen- oder Klassendeklaration hinzu und setzen Sie den Wert des Elements `enabled` auf `false`:

```
@OAuthSecurity(enabled = false)
```

Der Standardwert für das Element `enabled` der Annotation ist `true`. Wenn das Element `enabled` auf `false` gesetzt ist, wird das Element `scope` ignoriert und die Ressource oder Ressourcenklasse ist ungeschützt.

**Hinweis**: Wenn Sie einem Bereich eine Methode einer ungeschützten Klasse zuordnen, wird die Methode trotz der Klassenannotation geschützt, es sei denn, Sie setzen das Element `enabled` der Ressourcenannotation auf `false`.

Sehen Sie sich die folgenden Beispiele an:

Mit dem folgenden Code wird der Ressourcenschutz für eine Methode `helloUser` inaktiviert:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

Mit dem folgenden Code wird der Ressourcenschutz für eine Klasse `MyUnprotectedResources` inaktiviert:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### OAuth-Schutz für JavaScript-Ressourcen inaktivieren
{: #disablejavascriptresoauthprotection}

Um den OAuth-Schutz für eine JavaScript-Adapterressource (Prozedur) vollständig zu inaktivieren, setzen Sie in der Datei **adapter.xml** das Attribut `secured` des Elements <procedure> auf `false`:

```javascript
<procedure name="procedureName" secured="false">
```

Wenn das Attribut `secured` auf `false` gesetzt ist, wird das Attribut `scope` ignoriert und die Ressource ist ungeschützt.

Sehen Sie sich das folgende Beispiel an:

Mit dem folgenden Code wird der Ressourcenschutz für eine Prozedur `userName` inaktiviert:

```javascript
<procedure name="userName" secured="false">
```

### Ungeschützte Ressourcen definieren
{: #defunprotectedresources}

Eine ungeschützte Ressource ist eine Ressource, für die kein Zugriffstoken erforderlich ist. Das Sicherheitsframework von MobileFirst verwaltet den Zugriff auf ungeschützte Ressourcen nicht und prüft auch nicht die Identität von Clients, die auf diese Ressourcen zugreifen. Daher werden Funktionen wie Direct Update, das Blockieren des Gerätezugriffs oder das Inaktivieren einer Anwendung über Fernzugriff für ungeschützte Ressourcen nicht unterstützt.

### Externe Ressourcen schützen
{: #protecextresources}

Um externe Ressourcen zu schützen, fügen Sie einen Ressourcenfilter mit einem Validierungsmodul für Zugriffstoken zum externen Ressourcenserver hinzu. Das Modul für die Tokenvalidierung verwendet den Introspektionsendpunkt des Autorisierungsservers des Sicherheitsframeworks, um MobileFirst-Zugriffstokens zu prüfen, bevor der OAuth-Clientzugriff auf die Ressourcen erteilt wird. Sie können die MobileFirst-REST-API für die MobileFirst-Laufzeit verwenden, um ein eigenes Validierungsmodul für Zugriffstoken für einen beliebigen externen Server zu erstellen. Alternativ können Sie auch eine der bereitgestellten MobileFirst-Erweiterungen zum Schutz externer Java-Ressourcen verwenden.
