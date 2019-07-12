---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: command line, cli, mfpdev-cli, cli commands

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}
{:note: .note}

# Mobile Foundation-CLI
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }} bietet eine Befehlszeilenschnittstelle (Command Line Interface, CLI) als Werkzeug für **mfpdev**-Entwicklerbefehle zur einfachen Verwaltung der {{site.data.keyword.mobilefoundation_short}}-Client- und Serverartefakte.

Alle `mfpdev`-Befehle können entweder im interaktiven oder im Direktmodus ausgeführt werden. Im interaktiven Modus werden Sie zur Eingabe der für den Befehl erforderlichen Parameter aufgefordert und es werden einige Standardwerte verwendet. Im Direktmodus müssen die Parameter zusammen mit dem Befehl angegeben werden.


## {{site.data.keyword.mobilefoundation_short}}-CLI installieren
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} ist als NPM-Paket in der [NPM-Registry ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.npmjs.com){: new_window} verfügbar.

Stellen Sie sicher, dass **node.js** und **npm** installiert sind, um NPM-Pakete zu installieren. Befolgen Sie die Installationsanweisungen unter [nodejs.org ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://nodejs.org/){: new_window} für die Installation von **node.js**. Um zu überprüfen, ob **node.js** ordnungsgemäß installiert ist, führen Sie den folgenden Befehl aus:
```bash
node -v
```
{: codeblock}

Die unterstützte Node.js-Mindestversion ist **4.2.3**. Zudem funktioniert die {{site.data.keyword.mobilefoundation_short}}-CLI angesichts der sich häufig ändernden Node.js- und NPM-Pakete möglicherweise nicht mit allen verfügbaren Knoten- und NPM-Versionen, auch nicht allen neuesten Versionen. Stellen Sie sicher, dass der Knoten die Version **6.11.1** und NPM die Version **3.10.10** hat, damit die CLI gut funktioniert.
{: note}

Für die Version *8.0.2018100112* und höher des vorläufigen Fixes für die MobileFirst-CLI können Sie die Node-Versionen 8.x oder 10.x verwenden.

* Um die {{site.data.keyword.mobilefoundation_short}}-CLI zu installieren, führen Sie den folgenden Befehl aus:
    ```bash
    npm install -g mfpdev-cli
    ```
    {: codeblock}

* Wenn die komprimierte CLI-Datei (*.zip*) vom Download-Center der MobileFirst Operations Console heruntergeladen wurde, verwenden Sie den folgenden Befehl:
    ```bash
    npm install -g <path-to-mfpdev-cli.tgz>
    ```
    {: codeblock}

* Um die CLI ohne optionale Abhängigkeiten zu installieren, fügen Sie das Flag `--no-optional` hinzu:
    ```bash
    npm install -g --no-optional path-to-mfpdev-cli.tgz
    ```
    {: codeblock}

Bei der Installation der MobileFirst-CLI mithilfe von Node 8 werden im Terminalfenster möglicherweise einige der folgenden Fehler angezeigt:
```text
> node-gyp rebuild

gyp ERR! clean error
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```
{: codeblock}

Dieser Fehler wird durch einen [bekannten Programmfehler in node-gyp ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/nodejs/node-gyp/issues/1547){: new_window} verursacht. Diese Fehler können ignoriert werden, da sie sich nicht auf die Funktionsweise der MobileFirst-CLI auswirken. Dieses Problem bezieht sich auf das vorläufige Fix Version *8.0.2018100112* und höher von `mfpdev-cli`. Verwenden Sie während der Installation das Flag `--no-optional`, um diesen Fehler zu beheben.

* Um zu überprüfen, ob die **CLI** korrekt installiert ist, führen Sie den folgenden Befehl aus:
    ```bash
    mfpdev
    ```
    {: codeblock}

* Die CLI-Hilfe wird ausgegeben.

    ```
     NAME
     IBM MobileFirst Foundation Command Line Interface (CLI).

     SYNOPSIS
     mfpdev <command> [options]

     DESCRIPTION
     Die IBM MobileFirst Foundation-CLI (Command Line Interface) ist eine Befehlszeile
     für die Entwicklung von MobileFirst-Anwendungen. Die Befehlszeile kann allein oder in Verbindung mit der IBM MobileFirst Foundation Operations Console verwendet werden. Einige der Funktionen sind nur über die Befehlszeile und nicht über die Konsole verfügbar.

         Weitere Informationen und eine schrittweises Beispiel für die Verwendung der CLI finden Sie im IBM Knowledge Center für Ihre Version von IBM MobileFirst Foundation unter
     https://www.ibm.com/support/knowledgecenter.
         ...
         ...
         ...
    ```
    {: screen}

## Liste der {{site.data.keyword.mobilefoundation_short}}-CLI-Befehle
{: #list_cli_commands}

<table summary="{{site.data.keyword.mobilefoundation_short}}-CLI-Befehle mit Links, über die Sie weitere Informationen zu dem Befehl erhalten">
<caption>Tabelle 1. {{site.data.keyword.mobilefoundation_short}}-CLI-Befehle</caption>
 <thead>
 <th colspan="6">Allgemeine [mfpdev](#mfpdev)-Befehle</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[server](#mfpdev_server)</td>
 <td>[adapter](#mfpdev_adapter)</td>
 <td>[help](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### mfpdev
{: #mfpdev}

Legt Ihre Konfigurationseinstellungen für die Vorschau des Browsertyps, die Vorschau des Zeitlimitwerts und den Server-Zeitlimitwert für die mfpdev-CLI fest.

```bash
mfpdev config
```
{: codeblock}

Zeigt Informationen über ihre Umgebung an, darunter das Betriebssystem, den Speicherbedarf, die Knotenversion und die CLI-Version. Wenn das aktuelle Verzeichnis eine Cordova-Anwendung ist, werden auch die vom Cordova-Befehl `cordova info` bereitgestellten Informationen angezeigt.

```bash
mfpdev info
```
{: codeblock}

Zeigt die Versionsnummer der aktuell verwendeten {{site.data.keyword.mobilefoundation_short}}-CLI an.

```bash
mfpdev -v
```
{: codeblock}

Debugmodus: Erzeugt eine Debugausgabe.

```bash
mfpdev [-d|--debug]
```
{: codeblock}

Ausführlicher Debugmodus: Erzeugt eine ausführliche Debugausgabe.

```bash
mfpdev [-dd|--ddebug]
```
{: codeblock}

Unterdrückt die Verwendung von Farbe in der Befehlsausgabe.

```bash
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

Zeigt Hilfe für die MobileFirst-CLI- (mfpdev-) Befehle an. Mit dem Befehlsnamen als Argument zeigt der Befehl einen spezifischeren Hilfetext für jeden Befehlstyp oder Befehl an. Beispiel: `mfpdev help server add`.

```bash
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

Registriert Ihre App bei einem MobileFirst Server.

```bash
mfpdev app register
```
{: codeblock}

Um eine App bei einem Server und einer Laufzeit zu registrieren, der/die kein Standardserver bzw. keine Standardlaufzeit ist, verwenden Sie den folgenden Befehl:

```bash
mfpdev app register <server> <runtime>
```
{: codeblock}

Bei einer Cordova-Windows-Plattform muss dem Befehl das Argument `-w <platform>` hinzugefügt werden. Das Plattformargument ist eine durch Kommas getrennte Liste der zu registrierende Windows-Plattformen. Gültige Werte sind windows, windows8 und windowsphone8.

```bash
mfpdev app register -w windows8
```
{: codeblock}

Sie können den Back-End-Server und die Laufzeit angeben, der/die für Ihre App verwendet werden sollen. Bei Cordova-Apps ermöglicht der Befehl das Konfigurieren mehrerer zusätzlicher Aspekte wie z. B. die Standardsprache für Systemnachrichten und ob eine Kontrollsummen-Sicherheitsprüfung durchgeführt werden soll. Weitere Konfigurationsparameter sind für Cordova-Apps enthalten.

```bash
mfpdev app config
```
{: codeblock}

Ruft eine vorhandene App-Konfiguration vom Server ab.

```bash
mfpdev app pull
```
{: codeblock}

Sendet eine App-Konfiguration an den Server.

```bash
mfpdev app push
```
{: codeblock}

Zeigen Sie eine Vorschau Ihrer Cordova-App an, ohne dass ein tatsächliches Gerät des Zielplattformtyps erforderlich ist. Sie können die Vorschau entweder im Mobile Browser Simulator oder in Ihrem Web-Browser anzeigen.

```bash
mfpdev app preview
```
{: codeblock}

Packt die Anwendungsressourcen im www-Verzeichnis in eine komprimierte Datei (*.zip*), die für den direkten Aktualisierungsprozess verwendet werden kann.

```bash
mfpdev app webupdate
```
{: codeblock}

Dieser Befehl packt die aktualisierten Webressourcen in eine komprimierte Datei (*.zip*) und lädt sie in den standardmäßig registrierten MobileFirst-Server hoch. Die gepackten Webressourcen sind im Ordner `[cordova-project-root-folder]/mobilefirst/` zu finden.

Um die Webressourcen an eine andere Serverinstanz hochzuladen, geben Sie den Servernamen und die Laufzeit als Teil des Befehls an:

```bash
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

Sie können den Parameter –build zum Generieren der komprimierten Datei (*.zip*) mit den gepackten Webressourcen verwenden, ohne sie an einen Server hochzuladen.
```bash
mfpdev app webupdate --build
```
{: codeblock}

Um ein zuvor erstelltes Paket hochzuladen, verwenden Sie den Parameter –file:
```bash
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

Es gibt auch die Möglichkeit, den Inhalt des Pakets mit dem Parameter `-encrypt` zu verschlüsseln:
```bash
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

Zeigt Informationen über den MobileFirst Server an.

```bash
mfpdev server info
```
{: codeblock}

Fügt eine Serverdefinition zu Ihrer Umgebung hinzu.

```bash
mfpdev server add
```
{: codeblock}

Bearbeiten Sie eine Serverdefinition.

```bash
mfpdev server edit
```
{: codeblock}

Verwenden Sie den folgenden Befehl, um einen Server als Standardserver festzulegen:

```bash
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

Entfernt eine Serverdefinition aus Ihrer Umgebung.

```bash
mfpdev server remove
```
{: codeblock}

Öffnet die MobileFirst Operations Console.

```bash
mfpdev server console
```
{: codeblock}

Um die Konsole eines anderen Servers zu öffnen, geben Sie den Servernamen als Parameter für den Befehl an:

```bash
mfpdev server console <server-name>
```
{: codeblock}

Hebt die Registrierung von Apps auf und entfernt Adapter vom MobileFirst Server.

```bash
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

Erstellt einen Adapter.

```bash
mfpdev adapter create
```
{: codeblock}

Baut einen Adapter auf.

```bash
mfpdev adapter build
```
{: codeblock}

Sucht und baut alle Adapter im aktuellen Verzeichnis und seinen Unterverzeichnissen auf.

```bash
mfpdev adapter build all
```
{: codeblock}

Implementiert einen Adapter auf dem MobileFirst Server.

```bash
mfpdev adapter deploy
```
{: codeblock}

Verwenden Sie den folgenden Befehl, um die Bereitstellung auf einem anderen Server durchzuführen:
```bash
mfpdev adapter deploy <server_name>
```
{: codeblock}

Sucht alle Adapter im aktuellen Verzeichnis und seinen Unterverzeichnissen und implementiert sie auf dem MobileFirst Server.

```bash
mfpdev adapter deploy all
```
{: codeblock}

Ruft die Prozedur eines Adapters auf dem MobileFirst Server auf.

```bash
mfpdev adapter call
```
{: codeblock}

Ruft eine vorhandene Adapterkonfiguration vom Server ab.
```bash
mfpdev adapter pull
```
{: codeblock}

Sendet eine Adapterkonfiguration an den Server.
```bash
mfpdev adapter push
```
{: codeblock}

## {{site.data.keyword.mobilefoundation_short}}-CLI aktualisieren und deinstallieren
{: #update_uninstall_mf_cli}

Führen Sie den folgenden Befehl aus, um die Befehlszeilenschnittstelle zu aktualisieren:

```bash
npm update -g mfpdev-cli
```
{: codeblock}

Führen Sie den folgenden Befehl aus, um die Befehlszeilenschnittstelle zu deinstallieren:

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
