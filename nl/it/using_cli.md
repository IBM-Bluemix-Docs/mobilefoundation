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

# CLI di Mobile Foundation
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }} fornisce uno strumento della riga di comando (CLI) per lo sviluppatore **mfpdev**, per gestire facilmente le risorse di client e server {{site.data.keyword.mobilefoundation_short}}.

Tutti i comandi `mfpdev` possono essere eseguiti in modalità interattiva o diretta. Nella modalità interattiva, verranno richiesti i parametri obbligatori per il comando e verranno utilizzati alcuni valori predefiniti. Nella modalità diretta, i parametri devono essere forniti insieme al comando


## Installazione della CLI {{site.data.keyword.mobilefoundation_short}}
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} è disponibile come un pacchetto NPM nel [registro NPM ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.npmjs.com){: new_window}.

Assicurati che siano installati **node.js** e **npm** per installare i pacchetti NPM. Segui le istruzioni di installazione in [nodejs.org ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://nodejs.org/){: new_window} per installare **node.js**. Per verificare se **node.js** è stato installato correttamente, immetti il seguente comando:
```bash
node -v
```
{: codeblock}

La versione di node.js minima supportata è la **4.2.3**. Inoltre, con la rapida evoluzione dei pacchetti node e npm, la CLI {{site.data.keyword.mobilefoundation_short}} potrebbe non essere completamente funzionante con tutte le versioni disponibili di node e npm, incluse le ultime versioni. Assicurati che node sia alla versione **6.11.1** e npm sia **3.10.10**, per il corretto funzionamento della CLI.
{: note}

Per le versioni *8.0.2018100112* e superiori di fix temporanee della CLI MobileFirst, puoi utilizzare Node versioni 8.x o 10.x.

* Per installare la CLI {{site.data.keyword.mobilefoundation_short}}, immetti il seguente comando:
    ```bash
    npm install -g mfpdev-cli
    ```
    {: codeblock}

* Se il file compresso (*.zip*) della CLI è stato scaricato dal Download Center di MobileFirst Operations Console, utilizza il seguente comando:
    ```bash
    npm install -g <path-to-mfpdev-cli.tgz>
    ```
    {: codeblock}

* Per installare la CLI senza le dipendenze facoltative, aggiungi l'indicatore `--no-optional`:
    ```bash
    npm install -g --no-optional path-to-mfpdev-cli.tgz
    ```
    {: codeblock}

Durante l'installazione della CLI MobileFirst che utilizza Node 8, potresti visualizzare alcuni dei seguenti errori nella finestra del terminale:
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

Questo errore è dovuto a un [bug noto in node-gyp ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}. Questi errori possono essere ignorati in quanto non influiscono sul funzionamento della CLI MobileFirst. Questo problema è applicabile per la fix temporanea `mfpdev-cli` di livello *8.0.2018100112* e superiore. Per superare questo errore, utilizza l'indicatore `--no-optional` durante l'installazione.

* Per confermare che la CLI è stata installata correttamente, immetti il seguente comando:
    ```bash
    mfpdev
    ```
    {: codeblock}

* La guida della CLI viene stampata come output.

    ```
     NAME
     IBM MobileFirst Foundation Command Line Interface (CLI).

     SYNOPSIS
     mfpdev <command> [options]

     DESCRIPTION
     The IBM MobileFirst Foundation Command Line Interface (CLI) is a command-line
     for developing MobileFirst applications. The command-line can be used by itself, or in conjunction  with the IBM MobileFirst Foundation Operations Console. Some functions are available from  the command-line only and not the console.

         For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of IBM MobileFirst Foundation at
     https://www.ibm.com/support/knowledgecenter.
         ...
         ...
         ...
    ```
    {: screen}

## Elenco dei comandi CLI {{site.data.keyword.mobilefoundation_short}}
{: #list_cli_commands}

Comandi CLI <table summary="{{site.data.keyword.mobilefoundation_short}} con link che ti portano ad ulteriori informazioni sul comando">
<caption>Tabella 1. Comandi CLI {{site.data.keyword.mobilefoundation_short}}</caption>
 <thead>
 <th colspan="6">Comandi [mfpdev](#mfpdev) generali</th>
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

Imposta le tue preferenze di configurazione del tipo di browser di anteprima, il valore di timeout di anteprima e il valore di timeout del server per l'interfaccia riga di comando mfpdev.

```bash
mfpdev config
```
{: codeblock}

Visualizza le informazioni sul tuo ambiente, inclusi il sistema operativo, il consumo della memoria, la versione di node e la versione dell'interfaccia riga di comando. Se la directory corrente è un'applicazione Cordova, vengono anche visualizzate le informazioni fornite dal comando `cordova info` Cordova.

```bash
mfpdev info
```
{: codeblock}

Visualizza il numero di versione della CLI {{site.data.keyword.mobilefoundation_short}} al momento in utilizzo.

```bash
mfpdev -v
```
{: codeblock}

Modalità di debug: produce l'output di debug.

```bash
mfpdev [-d|--debug]
```
{: codeblock}

Modalità di debug dettagliata: produce l'output di debug dettagliato.

```bash
mfpdev [-dd|--ddebug]
```
{: codeblock}

Elimina l'utilizzo del colore nell'output del comando.

```bash
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

Visualizza la guida dei comandi (mfpdev) della CLI MobileFirst. Con il nome del comando come argomento, visualizza un testo di guida più specifico per ogni tipo di comando o comando. Ad esempio, `mfpdev help server add`.

```bash
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

Registra la tua applicazione con un server MobileFirst.

```bash
mfpdev app register
```
{: codeblock}

Per registrare un'applicazione su un server e un runtime non predefiniti, utilizza il seguente comando:

```bash
mfpdev app register <server> <runtime>
```
{: codeblock}

Per la piattaforma Cordova Windows, l'argomento `-w <platform>` deve essere aggiunto al comando. L'argomento di piattaforma è un elenco separato da virgole delle piattaforme Windows da registrare. I valori validi sono windows, windows8 e windowsphone8.

```bash
mfpdev app register -w windows8
```
{: codeblock}

Puoi specificare il server di backend e il runtime da utilizzare per la tua applicazione. Per le applicazioni Cordova, utilizzando questo comando, puoi configurare ulteriori aspetti come la lingua predefinita per i messaggi di sistema e se effettuare un controllo di sicurezza checksum. Altri parametri di configurazione sono inclusi nelle applicazioni Cordova.

```bash
mfpdev app config
```
{: codeblock}

Richiama una configurazione dell'applicazione esistente dal server.

```bash
mfpdev app pull
```
{: codeblock}

Invia la configurazione dell'applicazione al server.

```bash
mfpdev app push
```
{: codeblock}

Visualizza un'anteprima della tua applicazione Cordova senza richiedere un dispositivo effettivo del tipo della piattaforma di destinazione. Puoi visualizzare l'anteprima nel simulatore del browser mobile o nel tuo browser web.

```bash
mfpdev app preview
```
{: codeblock}

Riunisce le risorse dell'applicazione presenti nella directory www in un file compresso (*.zip*) che può essere utilizzato per il processo di aggiornamento diretto.

```bash
mfpdev app webupdate
```
{: codeblock}

Questo comando impacchetta le risorse web aggiornate in un file compresso (*.zip*) e lo carica nel server MobileFirst predefinito registrato. Le risorse web impacchettate possono essere trovate nella cartella `[cordova-project-root-folder]/mobilefirst/`.

Per caricare le risorse web in un'istanza del server diversa, fornisci il runtime e il nome del server come parte del comando:

```bash
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

Puoi utilizzare il parametro –build per generare il file compresso (*.zip*) con le risorse web impacchettate senza caricarlo in un server.
```bash
mfpdev app webupdate --build
```
{: codeblock}

Per caricare un pacchetto che è stato precedentemente creato, utilizza il parametro –file:
```bash
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

C'è anche l'opzione per crittografare il contenuto del pacchetto utilizzando il parametro `–encrypt`:
```bash
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

Visualizza le informazioni sul server MobileFirst.

```bash
mfpdev server info
```
{: codeblock}

Aggiunge una definizione server al tuo ambiente.

```bash
mfpdev server add
```
{: codeblock}

Modifica una definizione del server.

```bash
mfpdev server edit
```
{: codeblock}

Per impostare un server come predefinito, utilizza il seguente comando:

```bash
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

Rimuove una definizione del server dal tuo ambiente:

```bash
mfpdev server remove
```
{: codeblock}

Apre la MobileFirst Operations Console.

```bash
mfpdev server console
```
{: codeblock}

Per aprire la console di un altro server, fornisci il nome del server come parametro del comando:

```bash
mfpdev server console <server-name>
```
{: codeblock}

Annulla la registrazione e rimuove gli adattatori dal server MobileFirst.

```bash
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

Crea un adattatore.

```bash
mfpdev adapter create
```
{: codeblock}

Esegue la build di un adattatore.

```bash
mfpdev adapter build
```
{: codeblock}

Trova e crea tutti gli adattatori nella directory corrente e nelle relative sottodirectory.

```bash
mfpdev adapter build all
```
{: codeblock}

Distribuisce un adattatore al server MobileFirst.

```bash
mfpdev adapter deploy
```
{: codeblock}

Per distribuire a un server differente, utilizza il seguente comando:
```bash
mfpdev adapter deploy <server_name>
```
{: codeblock}

Trova tutti gli adattatori nella directory corrente e nelle relative sottodirectory e li distribuisce al server MobileFirst.

```bash
mfpdev adapter deploy all
```
{: codeblock}

Richiama la procedura dell'adattatore sul server MobileFirst.

```bash
mfpdev adapter call
```
{: codeblock}

Richiama una configurazione dell'adattatore esistente dal server.
```bash
mfpdev adapter pull
```
{: codeblock}

Invia la configurazione dell'adattatore al server.
```bash
mfpdev adapter push
```
{: codeblock}

## Aggiornamento e disinstallazione della CLI {{site.data.keyword.mobilefoundation_short}}
{: #update_uninstall_mf_cli}

Per aggiornare l'interfaccia riga di comando, esegui il comando:

```bash
npm update -g mfpdev-cli
```
{: codeblock}

Per disinstallare l'interfaccia riga di comando, esegui il comando:

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
