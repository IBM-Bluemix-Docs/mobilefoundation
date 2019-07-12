---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, restrict backend access, tampered apps

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

# Zugriff von manipulierten Apps auf das Back-End verhindern
{: #prevent_tampered_apps_accessing_backend}

Mit der Anwendungsauthentizität kann die Gültigkeit einer Anwendung vor dem Aktivieren von Services überprüft werden. Dadurch wird verhindert, dass manipulierte Anwendungen auf die Back-End-Services zugreifen können.
{: shortdesc}

Um Ihre Anwendung ordnungsgemäß zu sichern, aktivieren Sie die vordefinierte Sicherheitsprüfung der MobileFirst-Anwendungsauthentizität (``appAuthenticity``). Diese Überprüfung validiert die Authentizität der Anwendung, bevor Services für die Anwendung bereitgestellt werden. Für Anwendungen in der Produktionsumgebung sollte diese Funktion aktiviert sein.

Zum Aktivieren der Anwendungsauthentizität können Sie die Anweisungen unter **MobileFirst Operations Console → [Ihre Anwendung] → Authentizität** befolgen oder sich die folgenden Informationen durchlesen.

<dl>
  <dt>Verfügbarkeit</dt>
  <dd>Die Anwendungsauthentizität ist auf allen unterstützten Plattformen (iOS, Android, Windows 8.1 Universal, Windows 10 UWP) für Cordova-Anwendungen und native Anwendungen verfügbar.</dd>
  <dt>Einschränkungen</dt>
  <dd>Die Anwendungsauthentizität bietet unter iOS keine Unterstützung für **Bitcode**. Bevor Sie die Anwendungsauthentizität verwenden, müssen Sie daher Bitcode in den Xcode-Projekteigenschaften inaktivieren.</dd>
</dl>

## Ablauf für Anwendungsauthentizität
{: #appauthenticityflow}

Die Sicherheitsprüfung der Anwendungsauthentizität findet während der Registrierung der Anwendung bei MobileFirst Server statt, d. h. wenn eine Instanz der Anwendung zum ersten Mal versucht, eine Verbindung zum Server herzustellen. Die Authentizitätsprüfung wird standardmäßig nicht erneut ausgeführt.

Sobald die App-Authentizität aktiviert ist und wenn der Kunde Änderungen in seiner Anwendung vornehmen muss, muss die Anwendungsversion aktualisiert werden.

Weitere Informationen zum Anpassen dieses Verhaltens finden Sie im Abschnitt [Anwendungsauthentizität konfigurieren](#configappauthenticity).

### Anwendungsauthentizität aktivieren
{: #enableappauthenticity}

Führen Sie die folgenden Schritte aus, um die Anwendungsauthentizität in Ihrer Anwendung zu aktivieren:

1. Öffnen Sie die MobileFirst Operations Console in Ihrem bevorzugten Browser.
2. Wählen Sie Ihre Anwendung in der Navigationsseitenleiste aus und klicken Sie auf den Menüpunkt **Authentizität**.
3. Klicken Sie im Feld **Status** auf die Schaltfläche **An/Aus**.

![Anwendungsauthentizität aktivieren](/images/enable_application_authenticity.png)

MobileFirst Server validiert die Authentizität der Anwendung bei dem ersten Versuch, eine Verbindung zum Server herzustellen. Wenn Sie diese Validierung auch auf geschützte Ressourcen anwenden möchten, fügen Sie die Sicherheitsprüfung 'appAuthenticity' zum Schutzbereich hinzu.

### Anwendungsauthentizität inaktivieren
{: #disableappauthenticity}

Einige Änderungen an der Anwendung während der Entwicklung können dazu führen, dass die Authentizität der Anwendung nicht validiert werden kann. Es wird daher empfohlen, die Anwendungsauthentizität während des Entwicklungsprozesses zu inaktivieren. Für Anwendungen in der Produktionsumgebung sollte diese Funktion aktiviert sein.

Klicken Sie im Feld **Status** erneut auf die Schaltfläche **An/Aus**, um die Anwendungsauthentizität zu inaktivieren.

## Anwendungsauthentizität konfigurieren
{: #configappauthenticity}

Standardmäßig wird die Anwendungsauthentizität nur während der Clientregistrierung überprüft. Wie bei jeder anderen Sicherheitsprüfung können Sie jedoch die Anwendung oder Ressourcen mit der Sicherheitsprüfung ``appAuthenticity`` von der Konsole aus schützen. Befolgen Sie dazu die Anweisungen unter "Ressourcen schützen".

Sie können die vordefinierte Sicherheitsprüfung für die Anwendungsauthentizität mit der folgenden Eigenschaft konfigurieren:

* ``expirationSec``: Der Standardwert liegt bei 3600 Sekunden (1 Stunde). Die Eigenschaft definiert die Zeit bis zum Ablauf des Authentizitätstokens.

Eine durchgeführte Authentizitätsprüfung findet erst erneut statt, wenn das Token gemäß dem festgelegten Wert abgelaufen ist.

Gehen Sie wie folgt vor, um die Eigenschaft ``expirationSec`` zu konfigurieren:

1. Laden Sie die MobileFirst Operations Console, navigieren Sie zu **[Ihre Anwendung] → Sicherheit → Konfigurationen für Sicherheitsprüfungen** und klicken Sie auf **Neu**.
2. Suchen Sie nach dem Bereichselement ``appAuthenticity``.
3. Legen Sie einen neuen Wert in Sekunden fest.

    ![Ablaufzeit in Sekunden konfigurieren](/images/configuring_expirationSec.png)

## Build Time Secret (BTS)
{: #buildtimesecret}

Build Time Secret (BTS) ist ein **optionales Tool zur Verbesserung der Authentizitätsprüfung** von iOS-Anwendungen. Das Tool fügt einen zur Buildzeit festgelegten geheimen Schlüssel in die Anwendung ein. Dieser Schlüssel wird später während der Validierung der Authentizität verwendet.

Das BTS-Tool kann über **MobileFirst Operations Console → Download Center** heruntergeladen werden.

Verwenden Sie das Tool wie folgt in Xcode:

1. Klicken Sie auf der Registerkarte **Build Phases** auf die Schaltfläche mit dem **+** und erstellen Sie eine neue Scriptausführungsphase (**Run Script Phase**).
2. Kopieren Sie den Pfad des BTS-Tools und fügen Sie ihn in die neu erstellte Phase ein.
3. Ziehen Sie die Scriptausführungsphase mit der Maus an eine Position vor der Ressourcenkompilierungsphase (**Compile sources phase**).

Das Tool sollte bei der Erstellung einer Produktionsversion der Anwendung verwendet werden.

## Anwendungsauthentizität - Fehlerbehebung
{: #troubleshooting-app-authenticity}

### Befehl 'reset'
{: #trblreset}

Der Algorithmus für die Anwendungsauthentizität verwendet während der Validierung Anwendungsdaten und Metadaten. Das erste Gerät, das nach Aktivierung der Anwendungsauthentizität eine Verbindung zum Server herstellt, liefert einen “Fingerabdruck” der Anwendung, der einige dieser Daten enthält.

Sie können diesen Fingerabdruck zurücksetzen und neue Daten für den Algorithmus bereitstellen. Dies kann während der Entwicklung nützlich sein (z. B. nach dem Ändern der Anwendung in Xcode). Wenn Sie den Fingerabdruck zurücksetzen möchten, verwenden Sie den Befehl **reset** in der 'mfpadm'-CLI.

Nach dem Zurücksetzen des Fingerabdrucks funktioniert die Sicherheitsprüfung 'appAuthenticity' weiter wie zuvor. (Dies ist für den Benutzer transparent.)

### Validierungstypen
{: #trblvalidationtypes}

MobileFirst Platform Foundation bietet statische und dynamische App-Authentizität für Anwendungen. Diese Validierungstypen unterscheiden sich von dem Algorithmus und den Attributen, die zum Generieren von Seedwerten für die App-Authentizität verwendet werden. Wenn die Anwendungsauthentizität aktiviert ist, wird standardmäßig der dynamische Validierungsalgorithmus verwendet. Beide Validierungstypen gewährleisten die Sicherheit der Anwendung. Bei der dynamischen App-Authentizität werden strenge Validierungen und Prüfungen durchgeführt. Der Algorithmus für die statische App-Authentizität ist etwas weniger strikt. Es werden nicht alle Validierungen durchgeführt, die bei der dynamischen App-Authentizität verwendet werden.

Die dynamische App-Authentizität kann in der MobileFirst-Konsole konfiguriert werden. Der interne Algorithmus sorgt dafür, dass App-Authentizitätsdaten basierend auf den in der Konsole ausgewählten Optionen generiert werden. Für die statische App-Authentizität muss die 'mfpadm'-CLI verwendet werden.

Verwenden Sie die 'mfpadm-CLI', um die statische App-Authentizität zu aktivieren und den Validierungstyp zu wechseln:

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [LAUFZEIT] [APP-NAME] [UMGEBUNG] [VERSION] set authenticity-validation TYP
```
{: codeblock}

Der TYP kann entweder dynamisch oder statisch sein.
