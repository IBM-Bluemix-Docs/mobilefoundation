---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-14"

keywords: JSONStore, offline storage, jsonstore error codes

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:pre: .pre}


# JSONStore
{: #jsonstore}
{{site.data.keyword.mobilefoundation_short}} **JSONStore** ist eine optionale clientseitige API, die ein schlankes, dokumentorientiertes Speichersystem bereitstellt. JSONStore ermöglicht das persistente Speichern von **JSON-Dokumenten**. Dokumente in einer Anwendung sind auch dann in JSONStore verfügbar, wenn das Gerät, auf dem die Anwendung ausgeführt wird, offline ist. Dieser persistente und immer verfügbare Speicher kann nützlich sein, da Benutzer auch dann Zugriff auf Dokumente haben, wenn das Gerät keine Netzverbindung herstellen kann.
{: shortdesc}

Da Entwickler mit der Terminologie für relationale Datenbanken vertraut sind, wird diese mitunter in der vorliegenden Dokumentation verwendet, um JSONStore zu erklären. Es gibt jedoch viele Unterschiede zwischen einer relationalen Datenbank und JSONStore. Das strikte Schema, das zum Speichern von Daten in relationalen Datenbanken verwendet wird, unterscheidet sich z. B. von dem Ansatz bei JSONStore. Bei JSONStore können Sie jeden beliebigen JSON-Inhalt speichern und den Inhalt indexieren, den Sie durchsuchen müssen.

## Hauptmerkmale
{: #key-features-jsonstore }
* Datenindexierung für eine effiziente Suche
* Verfahren zur Verfolgung lokaler Änderungen an den gespeicherten Daten
* Unterstützung vieler Benutzer
* Die 256-Bit-AES-Verschlüsselung gespeicherter Daten bietet Sicherheit und Vertraulichkeit. Sie können den Schutz für die verschiedenen Benutzer mit einem Kennwortschutz segmentieren, wenn mehrere Benutzer auf einem Gerät vorhanden ist.

Ein einzelner Speicher kann viele Sammlungen enthalten und jede Sammlung kann viele Dokumente enthalten. Eine MobileFirst-Anwendung kann auch aus mehreren Speichern bestehen. Weitere Informationen finden Sie im Abschnitt zur Unterstützung mehrerer Benutzer in JSONStore.

## Supportebene
{: #support-level-jsonstore }
* JSONStore wird in nativen iOS-und Android-Anwendungen unterstützt (keine Unterstützung für native Windows-Anwendungen (Universal und UWP)).
* JSONStore wird in Cordova iOS-, Android- und Windows-Anwendungen (Universal und UWP) unterstützt.


## Allgemeine JSONStore-Terminologie
{: #general-jsonstore-terminology }
### Dokument
{: #document }
Ein Dokument ist der Grundbaustein von JSONStore.

Ein JSONStore-Dokument ist ein JSON-Objekt mit einer automatisch generierten Kennung (`_id`) und JSON-Daten. Es ist in der Datenbankterminologie mit einem Datensatz oder einer Zeile vergleichbar. Der Wert von `_id` ist immer eine eindeutige Ganzzahl innerhalb einer bestimmten Sammlung. Bestimmte Funktionen wie `add`, `replace` und `remove` in der Klasse `JSONStoreInstance` akzeptieren ein Array von Dokumenten oder Objekten. Diese Methoden sind nützlich, um Operationen für verschiedene Dokumente oder Objekte gleichzeitig auszuführen.

Einzelnes Dokument: 

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

Array mit Dokumenten: 

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Sammlung
{: #collection }
Eine JSONStore-Sammlung ist in der Datenbankterminologie mit einer Tabelle vergleichbar.  
Das folgende Codebeispiel zeigt nicht, wie die Dokumente auf Platte gespeichert werden, veranschaulicht aber gut, wie eine Sammlung auf einer höheren Ebene aussieht.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Speicher
{: #store }
Ein Speicher ist die persistente JSONStore-Datei, die aus einer oder mehreren Sammlungen besteht.  
Ein Speicher ist in der Datenbankterminologie mit einer relationalen Datenbank vergleichbar. Ein Speicher wird auch als JSONStore bezeichnet.

### Suchfelder
{: #search-fields }
Ein Suchfeld ist ein Schlüssel/Wert-Paar.  
Suchfelder sind Schlüssel, die indexiert werden, um kürzere Abrufzeiten zu erreichen. Sie sind in der Datenbankterminologie mit Spaltenfeldern oder Attributen vergleichbar.

Zusätzliche Suchfelder sind Schlüssel, die indexiert werden, jedoch zu den gespeicherten JSON-Daten gehören. Diese Felder definieren den Schlüssel, dessen Werte (in der JSON-Sammlung) indexiert werden und zur Beschleunigung eines Suchvorgangs verwendet werden können.

Gültige Datentypen sind: String (Zeichenfolge), Boolean (boolescher Wert), Number (Zahl) und Integer (Ganzzahl). Diese Typen sind nur Typhinweise. Es gibt keine Typvalidierung. Außerdem bestimmen diese Typen, wie indexierbare Felder gespeichert werden. Beispiel: `{age: 'number'}` indexiert 1 als 1.0, während `{age: 'integer'}` 1 als 1 indexiert.

Suchfelder und zusätzliche Suchfelder:

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

Es können nur Schlüssel in einem Objekt indexiert werden, nicht das Objekt selbst. Arrays werden in einem Durchgriffsmodus verarbeitet. Sie können also ein Array oder einen bestimmten Index des Arrays (arr [n]) nicht indexieren, wohl aber Objekte in einem Array.

Werte in einem Array indexieren:

```javascript

var searchFields = {
    'people.name' : 'string', // stimmt mit carlos und tim in myObject überein
    'people.age' : 'integer' // stimmt mit 99 und 100 in myObject überein
};

var myObject = {
    people : [
        {name: 'carlos', age: 99},
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### Abfragen
{: #queries }
Abfragen sind Objekte, die Suchfelder oder zusätzliche Suchfelder verwenden, um nach Dokumenten zu suchen.  
Bei den folgenden Beispielen wird davon ausgegangen, dass das Suchfeld "name" den Typ "string" und das Suchfeld "age" den Typ "integer" aufweist.

Dokumente suchen, in denen `name` mit `carlos` übereinstimmt:

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

Dokumente suchen, in denen `name` mit `carlos` und `age` mit `99` übereinstimmt:

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### Abfrageabschnitte
{: #query-parts }
Abfrageabschnitte werden verwendet, um erweiterte Suchvorgänge zu erstellen. Einige JSONStore-Operationen, beispielsweise bestimmte Versionen von `find` oder `count`, akzeptieren Abfrageabschnitte. Alle Elemente innerhalb eines Abfrageabschnitts werden durch `AND`-Anweisungen verknüpft, während die Abfrageabschnitte selbst durch `OR`-Anweisungen verknüpft werden. Die Suchkriterien geben nur dann eine Übereinstimmung zurück, wenn für alle Elemente in einem Abfrageabschnitt **true** gilt. Sie können mehrere Abfrageabschnitte verwenden, um Übereinstimmungen zu suchen, die für mindestens einen Abfrageabschnitt gelten.

Suchvorgänge mit Abfrageabschnitten funktionieren nur in Suchfeldern der höchsten Ebene. Beispiel: `name`, nicht `name.first`. Verwenden Sie mehrere Sammlungen, bei denen alle Suchfelder der höchsten Ebene angehören, um dieses Verhalten zu vermeiden. Die folgenden Abfrageabschnitte funktionieren mit Suchfeldern, die nicht der höchsten Ebene angehören: `equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike` und `notLeftLike`. Bei der Verwendung von Suchfeldern, die nicht der höchsten Ebene angehören, ist das Verhalten unbestimmt.

## Funktionstabelle
{: #features-table }
Vergleichen Sie die JSONStore-Funktionen mit den Funktionen anderer Datenspeichertechnologien und -formate.

JSONStore ist eine JavaScript-API zum Speichern von Daten in Cordova-Anwendungen, die das MobileFirst-Plug-in, eine Objective-C-API für native iOS-Anwendungen und eine Java-API für native Android-Anwendungen verwenden. Zu Referenzzwecken finden Sie nachstehend einen Vergleich der verschiedenen JavaScript-Speichertechnologien mit den Funktionen von JSONStore.

JSONStore ist mit Technologien wie LocalStorage, Indexed DB, der Cordova-Speicher-API und der Cordova-Datei-API vergleichbar. Die Tabelle zeigt einige von JSONStore bereitgestellte Funktionen im Vergleich mit anderen Technologien. Die JSONStore-Funktion ist nur auf iOS- und Android-Geräten und -Simulatoren verfügbar.

| Funktion                                            | JSONStore      | LocalStorage | IndexedDB | Cordova-Speicher-API | Cordova-Datei-API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Android-Unterstützung (Cordova- &amp; native Anwendungen)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| iOS-Unterstützung (Cordova- & native Anwendungen)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal und Windows 10 UWP (Cordova-Anwendungen)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| Datenverschlüsselung	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Maximaler Speicher	                                 |Verfügbarer Speicherplatz |    ~5 MB     |   ~5 MB 	 | Verfügbarer Speicherplatz	   | Verfügbarer Speicherplatz  |
| Zuverlässiger Speicher (siehe Hinweis)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| Verfolgung lokaler Änderungen	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Mehrbenutzerunterstützung                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Indexierung	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| Speichertyp	                                 | JSON-Dokumente | Schlüssel/Wert-Paare | JSON-Dokumente | Relational (SQL) | Zeichenfolgen     |
{: caption="Tabelle 1. Featurevergleich" caption-side="top"}

Zuverlässiger Speicher bedeutet, dass Ihre Daten nicht gelöscht werden, es sei denn, eines der folgenden Ereignisse tritt ein:
* Die Anwendung wird von dem Gerät entfernt.
* Es wird eine der Methoden aufgerufen, mit denen Daten entfernt werden.
{: note}

## Unterstützung mehrerer Benutzer
{: #multiple-user-support }
Bei JSONStore können Sie in einer einzigen MobileFirst-Anwendung viele Speicher erstellen, die aus verschiedenen Sammlungen bestehen.

Die init-API (JavaScript-API) oder die open-API (native iOS- und native Android-API) kann ein Objekt "options" mit einem Benutzernamen akzeptieren. Unterschiedliche Speicher sind separate Dateien im Dateisystem. Der Benutzername wird als Dateiname für den Speicher verwendet. Diese separaten Speicher können aus Gründen der Sicherheit und des Datenschutzes mit unterschiedlichen Passwörtern verschlüsselt werden. Wenn Sie die closeAll-API aufrufen, wird der Zugriff auf alle Sammlungen beendet. Es ist auch möglich, durch Aufrufen der changePassword-API das Kennwort eines verschlüsselten Speichers zu ändern.

Ein Beispiel für einen Anwendungsfall wären verschiedene Mitarbeiter, die ein physisches Gerät (z. B. ein iPad oder Android-Tablet) und eine MobileFirst-Anwendung gemeinsam nutzen. Die Unterstützung mehrerer Benutzer ist nützlich, wenn die Mitarbeiter in unterschiedlichen Schichten arbeiten und bei der Nutzung der MobileFirst-Anwendung private Daten von verschiedenen Kunden verarbeiten.

## Sicherheit
{: #security-jsonstore }
Sie können alle Sammlungen in einem Speicher sichern, indem Sie sie verschlüsseln.

Übergeben Sie zum Verschlüsseln aller Sammlungen in einem Speicher ein Kennwort an die `init`-API (JavaScript-API) oder die `open`-API (native iOS- und native Android-API). Wenn kein Kennwort übergeben wird, wird kein Dokument in den Speichersammlungen verschlüsselt.

Einige Sicherheitsartefakte (z. B. Salt) werden in der Schlüsselkette (iOS), den gemeinsam genutzten Vorgaben (Android) und dem Fach für Berechtigungsnachweise (Windows Universal 8.1 und Windows 10 UWP) gespeichert. Der Speicher wird mit einem 256-Bit-AES-Schlüssel (AES - Advanced Encryption Standard) verschlüsselt. Alle Schlüssel werden mit Password-Based Key Derivation Function 2 (PBKDF2) verstärkt. Sie können die Datensammlungen für eine Anwendung verschlüsseln, jedoch nicht zwischen verschlüsselten und Klartextformaten wechseln oder Formate in einem Speicher mischen.

Der Schlüssel, der die Daten im Speicher schützt, basiert auf dem Benutzerkennwort, das Sie angeben. Der Schlüssel läuft nicht ab, aber Sie können ihn ändern, indem Sie die changePassword-API aufrufen.

Der Datenschutzschlüssel (DPK) ist der Schlüssel, mit dem der Inhalt des Speichers entschlüsselt wird. Der Datenschutzschlüssel wird auch dann in der iOS-Schlüsselkette gespeichert, wenn die Anwendung deinstalliert wird. Verwenden Sie die destroy-API, um den Schlüssel in der Schlüsselkette und alles andere zu entfernen, was JSONStore in die Anwendung einbringt. Dieser Prozess ist nicht auf Android anwendbar, da der verschlüsselte Datenschutzschlüssel in den gemeinsam genutzten Einstellungen gespeichert ist und bei der Deinstallation der Anwendung gelöscht wird.

Wenn JSONStore zum ersten Mal eine Sammlung mit einem Kennwort öffnet, was bedeutet, dass der Entwickler Daten im Speicher verschlüsseln möchte, benötigt JSONStore ein zufälliges Token. Dieses zufällige Token kann vom Client oder vom Server kommen.

Wenn der Schlüssel "localKeyGen" in der JavaScript-Implementierung der JSONStore-API vorhanden ist und sein Wert "true" ist, wird lokal ein kryptografisch sicheres Token generiert. Andernfalls wird das Token durch Kontaktaufnahme mit dem Server generiert, was eine Verbindung zum MobileFirst-Server erfordert. Dieses Token ist nur beim ersten Öffnen des Speichers mit einem Kennwort erforderlich. Standardmäßig generieren die nativen Implementierungen (Objective-C und Java) ein kryptografisch sicheres Token. Sie können aber auch mit der Option "secureRandom" ein Token übergeben.

Der Kompromiss liegt zwischen den folgenden Optionen:
* Einen Speicher offline öffnen und dem Client vertrauen, dass er dieses zufällige Token generiert (weniger sicher) oder
* Den Speicher mit Zugriff auf den MobileFirst-Server öffnen (erfordert eine Verbindung) und dem Server vertrauen (sicherer)

### Sicherheitsdienstprogramme
{: #security-utilities }
Die clientseitige MobileFirst-API stellt einige Sicherheitsdienstprogramme bereit, mit denen Sie die Daten Ihrer Benutzer schützen können. Funktionen wie JSONStore sind ausgezeichnet für den Schutz Ihrer JSON-Objekte geeignet. Es wird jedoch nicht empfohlen, binäre Blobs in einer JSONStore-Sammlung zu speichern.

Speichern Sie Binärdaten stattdessen im Dateisystem und speichern Sie die Dateipfade und andere Metadaten in einer JSONStore-Sammlung. Wenn Sie Dateien wie Bilder schützen möchten, können Sie sie als Base64-Zeichenfolgen codieren, sie verschlüsseln und die Ausgabe auf Platte schreiben.

Um die Daten zu entschlüsseln, können Sie die Metadaten in einer JSONStore-Sammlung suchen, die verschlüsselten Daten von der Platte lesen und sie mithilfe der gespeicherten Metadaten entschlüsseln.

Diese Metadaten können den Schlüssel, Initialisierungsvektor (IV), Dateityp, Dateipfad, das Salt und andere Attribute enthalten.

Hier finden Sie weitere Informationen zu [JSONStore-Sicherheitsdienstprogrammen](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities).
{: tip}

### Verschlüsselung von Windows 8.1 Universal und Windows 10 UWP
{: #windows-81-universal-and-windows-10-uwp-encryption }
Sie können alle Sammlungen in einem Speicher sichern, indem Sie sie verschlüsseln.

JSONStore verwendet [SQLCipher ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://sqlcipher.net/){: new_window} als zugrunde liegende Datenbanktechnologie. SQLCipher ist ein von Zetetic erstellter Build von SQLite. LLC fügt eine Verschlüsselungsebene zu der Datenbank hinzu.

JSONStore verwendet SQLCipher auf allen Plattformen. Unter Android und iOS ist eine kostenlose Open-Source-Version von SQLCipher verfügbar, die als Community Edition bezeichnet wird und in die Versionen von JSONStore integriert ist, die in Mobile Foundation enthalten sind. Die Windows-Versionen von SQLCipher sind nur mit einer kostenpflichtig Lizenz verfügbar und können von Mobile Foundation nicht direkt weitergegeben werden.

Stattdessen enthält JSONStore für Windows 8 Universal SQLite als zugrunde liegende Datenbank. Um Daten für eine dieser beiden Plattformen zu verschlüsseln, müssen Sie Ihre eigene Version von SQLCipher erwerben und die in Mobile Foundation enthaltene SQLite-Version austauschen.

Wenn Sie keine Verschlüsselung benötigen, ist JSONStore mit der SQLite-Version in Mobile Foundation voll funktionsfähig (außer Verschlüsselung).

#### SQLite durch SQLCipher für Windows Universal und Windows UWP ersetzen
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. Führen Sie die Erweiterung SQLCipher für Windows Runtime 8.1/ 10 aus, die im Lieferumfang von SQLCipher für Windows Runtime Commercial Edition enthalten ist.
2. Suchen Sie nach der Installation der Erweiterung die SQLCipher-Version der gerade erstellten Datei **sqlite3.dll**. Es gibt je eine Version für x86, x64 und ARM.

    ```bash
     C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
    ```
    {: codeblock}

3. Kopieren Sie diese Datei und versetzen Sie sie in Ihre MobileFirst-Anwendung.

    ```bash
    <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
    ```
    {: codeblock}

## Leistung
{: #performance-jsonstore }
Die folgenden Faktoren können sich auf die Leistung von JSONStore auswirken.

### Netz
{: #network-jsonstore }
* Prüfen Sie die Netzkonnektivität, bevor Sie Operationen wie das Senden aller vorläufigen Dokumenten an einen Adapter ausführen.
* Die Menge der Daten, die über das Netz an einen Client gesendet werden, wirkt sich stark auf die Leistung aus. Senden Sie nur die Daten, die von der Anwendung benötigt werden, statt alle Daten in Ihrer Back-End-Datenbank zu kopieren.
* Wenn Sie einen Adapter verwenden, sollten Sie das Flag "compressResponse" auf "true" setzen. Dadurch werden Antworten komprimiert, was in der Regel weniger Bandbreite beansprucht und eine schnellere Übertragungszeit als ohne Komprimierung bewirkt.

### Hauptspeicher
{: #memory-jsonstore }
* Wenn Sie die JavaScript-API verwenden, werden JSONStore-Dokumente als Zeichenfolgen zwischen der nativen Ebene (Objective-C, Java oder C#) und der JavaScript-Ebene serialisiert und deserialisiert. Eine Möglichkeit zum Abmildern möglicher Speicherprobleme besteht in der Verwendung von Begrenzungen und Offsets bei Verwendung der find-API. Auf diese Weise begrenzen Sie die Menge des Speichers, der für die Ergebnisse zugeordnet ist, und können Funktionen wie die Paginierung (Anzahl x der Ergebnisse pro Seite anzeigen) implementieren.
* Anstatt lange Schlüsselnamen zu verwenden, die schließlich als Zeichenfolgen serialisiert und deserialisiert werden, sollten Sie diese langen Schlüsselnamen kleineren Namen zuordnen (z. B. `myVeryVeryVerLongKeyName` zu `k` oder `key`). Idealerweise ordnen Sie sie kurzen Schlüsselnamen zu, wenn Sie sie vom Adapter an den Client senden, und den ursprünglichen langen Schlüsselnamen, wenn Sie Daten an das Back-End senden.
* Sie sollten die Daten in einem Speicher in verschiedene Sammlungen aufteilen. Verwenden Sie kleine Dokumente in verschiedenen Sammlungen statt monolithischer Dokumente in einer einzigen Sammlung. Diese Empfehlung hängt davon ab, wie eng die Daten verwandt sind, sowie von den Anwendungsfällen für die betreffenden Daten.
* Wenn Sie die add-API mit einem Array von Objekten verwenden, treten möglicherweise Speicherprobleme auf. Rufen Sie zur Abmilderung dieses Problems diese Methoden mit weniger JSON-Objekten gleichzeitig auf.
* JavaScript und Java™ verfügen über Garbage-Collectors, während Objective-C einen automatischen Referenzzähler aufweist. Binden Sie ihn ein, aber machen Sie sich nicht komplett abhängig von ihm. Versuchen Sie, Referenzen zu ignorieren, die nicht mehr verwendet werden, und verwenden Sie Profilermittlungstools, um zu überprüfen, ob die Speicherbelegung verringert wird, wenn Sie dies erwarten.

### CPU
{: #cpu-jsonstore }
* Die Anzahl der verwendeten Suchfelder und zusätzlichen Suchfelder wirkt sich auf die Leistung aus, wenn Sie die Add-Methode aufrufen, welche die Indexierung durchführt. Indexieren Sie nur die Werte, die in Abfragen für die Find-Methode verwendet werden.
* Standardmäßig verfolgt JSONStore lokale Änderungen an seinen Dokumenten. Sie können dieses Verhalten inaktivieren, sodass ein paar Zyklen eingespart werden können, indem Sie das Flag `markDirty` auf **false** setzen, wenn Sie die add-, remove- und replace-APIs verwenden.
* Die Aktivierung der Sicherheit bewirkt einen gewissen Zusatzaufwand bei den `init`- oder `open`-APIs und bei anderen Operationen, die mit Dokumenten in der Sammlung arbeiten. Überlegen Sie, ob die Sicherheit wirklich erforderlich ist. Die open-API ist beispielsweise mit Verschlüsselung viel langsamer, weil sie die Verschlüsselungsschlüssel generieren muss, die für Verschlüsselung und Entschlüsselung verwendet werden.
* Die `replace`- und `remove`-APIs sind von der Größe der Sammlung abhängig, denn sie müssen die komplette Sammlung durchgehen, um alle Vorkommen des jeweiligen Elements zu ersetzen oder zu entfernen. Da die APIs jeden Datensatz durchgehen müssen, müssen sie jeden einzelnen Datensatz entschlüsseln. Dadurch werden die APIs bei Verschlüsselung deutlich langsamer. Bei größeren Sammlungen wird diese Leistungseinbuße noch auffälliger.
* Die `count`-API ist relativ teuer. Sie können jedoch eine Variable für den Zähler für diese Sammlung verwenden. Aktualisieren Sie die Variable jedes Mal, wenn Sie Dokumente in der Sammlung speichern oder aus der Sammlung entfernen.
* Die `find`-APIs (`find`, `findAll` und `findById`) sind von der Verschlüsselung betroffen, weil sie jedes Dokument entschlüsseln müssen, um feststellen zu können, ob es eine Übereinstimmung enthält oder nicht. Wenn beim Suchen mit einer Abfrage ein Limit übergeben wird, ist die Suche potenziell schneller, weil sie stoppt, sobald eine bestimmte Anzahl von Ergebnissen erreicht ist. JSONStore muss die restlichen Dokumente nicht entschlüsseln, um herauszufinden, ob noch weitere Suchergebnisse vorhanden sind.

## Gemeinsamer Zugriff
{: #concurrency-jsonstore }
### Gemeinsamer Zugriff in JavaScript
{: #javascript-jsonstore }
Die meisten für eine Sammlung ausführbaren Operationen wie "add" und "find" sind asynchron. Diese Operationen geben eine jQuery-Zusicherung (Promise) zurück, die eingelöst wird, wenn die Operation erfolgreich abgeschlossen wird, und die zurückgewiesen wird, wenn ein Fehler auftritt. Diese Zusicherungen sind mit Callback-Funktionen bei Erfolg und Fehler vergleichbar.

Ein jQuery-Aufschub (Deferred) ist eine Zusicherung, die eingelöst oder zurückgewiesen werden kann. Die folgenden Beispiele sind keine spezifischen JSONStore-Beispiele, sondern sollen Ihnen helfen, ganz allgemein die Verwendung von Zusicherung (Promise) und Aufschub (Deferred) zu verstehen.

Anstelle von Zusicherungen und Callback-Funktionen können Sie auch die JSONStore-Ereignisse `success` und `failure` überwachen. Führen Sie Aktionen aus, die auf den an den Ereignislistener übergebenen Argumenten basieren.

Beispieldefinition für Zusicherung:

```javascript
var asyncOperation = function () {
  // Vorausgesetzt wird, dass Sie jQuery über $ in der Umgebung definiert haben.
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```
{: codeblock}

Verwendungsbeispiel für Zusicherung:

```javascript
// Die an .then übergebene Funktion wird nach 1000 ms ausgeführt.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Beispieldefinition für Callback:

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

Verwendungsbeispiel für Callback: 

```javascript
// Die an asyncOperation übergebene Funktion wird nach 1000 ms ausgeführt.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

Beispielereignisse:

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Enthält Informationen zum Ereignis
  // data - Daten, die nach Abschluss der Operation (add, find usw.) gesendet werden
  // src - Name der Operation (add, find, push, usw.)
  // collectionName - Name der Sammlung
});
```
{: codeblock}

### Gemeinsamer Zugriff in Objective-C
{: #objective-c-jsonstore }
Wenn Sie die native iOS-API für JSONStore verwenden, werden alle Operationen zu einer synchronen Dispatchwarteschlange hinzugefügt. Dadurch wird sichergestellt, dass Operationen mit Auswirkungen auf den Speicher der Reihe nach in einem Thread ausgeführt werden, der nicht der Hauptthread ist. Weitere Informationen finden Sie in der Apple-Dokumentation unter [Grand Central Dispatch (GCD) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}.

### Gemeinsamer Zugriff in Java
{: #java-jsonstore }
Wenn Sie die native Android-API für JSONStore verwenden, werden alle Operationen im Hauptthread ausgeführt. Für ein asynchrones Verhalten müssen Sie Threads erstellen oder Threadpools verwenden. Alle Store-Operationen sind threadsicher.

## Analyse
{: #analytics-jsonstore }
Sie können wichtige Analysedaten mit Bezug zu JSONStore erfassen.

### Dateiinformationen
{: #file-information }
Dateiinformationen werden einmal pro Anwendungssitzung erfasst, wenn die JSONStore-API mit auf **true** gesetztem Flag "analytics" aufgerufen wird. Eine Anwendungssitzung ist als das Laden der Anwendung in den Speicher und das Entfernen der Anwendung aus dem Speicher definiert. Anhand dieser Informationen können Sie feststellen, wie viel Speicher vom JSONStore-Inhalt der Anwendung belegt wird.

### Leistungsmessdaten
{: #performance-metrics }
Leistungsmessdaten werden jedes Mal erfasst, wenn eine JSONStore-API mit Informationen zur Start- und Endzeit einer Operation aufgerufen wird. Anhand dieser Informationen können Sie feststellen, wie lange bestimmte Operationen (in Millisekunden) dauern.

### JSONStore-Beispiel für iOS
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

### JSONStore-Beispiel für Android
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

### JSONStore-Beispiel für JavaScript
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## Externe Daten verwenden
{: #working-with-external-data }
Für die Arbeit mit externen Daten können Sie die Konzepte **Pull** und **Push** nutzen.

### Pull
{: #pull }
Viele Systeme verwenden den Begriff "Pull" für das Abrufen von Daten von einer externen Quelle.  
Es gibt drei wichtige Komponenten:

#### Pull-Operation aus externer Datenquelle
{: #external-data-source-pull }
Diese Quelle kann eine Datenbank, eine REST- oder SOAP-API oder Ähnliches sein. Die einzige Anforderung ist, dass die Quelle vom MobileFirst-Server aus oder direkt von der Clientanwendung aus zugänglich sein muss. Im Idealfall soll diese Quelle Daten im JSON-Format zurückgeben.

#### Transportschicht für Pull-Operation
{: #transport-layer-pull }
Diese Quelle bestimmt, wie Daten von der externen Quelle in Ihre interne Quelle, d. h. eine JSONStore-Sammlung innerhalb des Speichers, gelangen. Eine Alternative ist ein Adapter.

#### Interne Datenquellen-API für Pull-Operation
{: #internal-data-source-api-pull }
Diese Quelle umfasst die JSONStore-APIs, mit denen Sie JSON-Daten zu einer Sammlung hinzufügen können.

Sie können den internen Speicher mit Daten füllen, die aus einer Datei oder einem Eingabefeld gelesen werden, oder mit fest codierten Daten in einer Variablen. Die Daten müssen nicht ausschließlich von einer externen Quelle mit erforderlicher Netzkommunikation stammen.
{: note}

Alle nachfolgenden Codebeispiele sind Pseudocode, der so ähnlich wie JavaScript aussieht.

Verwenden Sie Adapter für die Transportschicht. Zu den Vorteilen, die Sie bei Verwendung von Adaptern haben, gehören die XML-JSON-Umsetzung, die Sicherheit, Filter und die Entkopplung von server- und clientseitigem Code.
{: note}
##### Externe Datenquelle: Back-End-REST-Endpunkt
{: #external-data-source-backend-rest-endpoint-head}
Stellen Sie sich vor, Sie hätten einen REST-Endpunkt, der Daten aus einer Datenbank liest und als Array mit JSON-Objekten zurückgibt.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

Die zurückgegebenen Daten können wie im folgenden Beispiel aussehen:

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

##### Transportschicht: Adapter
{: #transport-layer-adapter}
Stellen Sie sich vor, Sie hätten einen Adapter namens "people" erstellt und die Prozedur "getPeople" definiert. Die Prozedur ruft den REST-Endpunkt auf und gibt das Array mit JSON-Objekten an den Client zurück. Hier sind weitere Optionen möglich, z. B., dass nur ein Teil der Daten an den Client zurückgegeben wird.

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Auf dem Client können Sie die Daten mit der WLResourceRequest-API abrufen. Zusätzlich könnten Sie einige Parameter vom Client an den Adapter übergeben. Ein Beispiel wäre der Tag, an dem der Client zuletzt über den Adapter neue Daten von der externen Quelle abgerufen hat.

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

Sie können auch von den Vorteilen profitieren, die mit der Übergabe von `compressResponse`, `timeout` und anderen Parametern an die `WLResourceRequest`-API verbunden sind.  
{: note}

Alternativ können Sie den Adapter übergehen und jQuery.ajax oder Ähnliches verwenden, um direkten Kontakt zu dem REST-Endpunkt mit den Daten aufzunehmen, die Sie speichern möchten.

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### Interne Datenquellen-API: JSONStore
{: #internal-data-source-api-jsonstore}
Nachdem Sie die Antwort vom Back-End empfangen haben, können Sie JSONStore verwenden, um mit den Daten zu arbeiten.
In JSONStore gibt es eine Möglichkeit, lokale Änderungen zu verfolgen, nämlich APIs, die Dokumente als vorläufig markieren. Die API erfasst die letzte für das Dokument ausgeführte Operation und den Zeitpunkt, zu dem das Dokument als vorläufig markiert wurde. Diese Informationen können Sie nutzen, um Funktionen wie die Datensynchronisation zu implementieren.

Die change-API akzeptiert die Daten und einige Optionen:

###### replaceCriteria
{: #replacecriteria}
  
Diese Suchfelder sind Teil der Eingabedaten. Sie werden verwendet, um bereits in einer Sammlung enthaltene Dokumente zu finden. Beispiel:

```javascript
['id', 'ssn']
```
{: codeblock}

Dies ist Ihr ausgewähltes Ersetzungskriterium und Sie übergeben das folgende Array als Eingabedaten:

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

Die Sammlung `people`  enthält bereits das folgende Dokument:

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

Die Operation `change` sucht nach einem Dokument, das exakt mit der folgenden Abfrage übereinstimmt:

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

Die Operation `change` führt dann eine Ersetzung mit den Eingabedaten durch, sodass die Sammlung Folgendes enthält:

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

Der Name wurde von `Carlitos` in `Carlos` geändert. Wenn mehrere Dokumente das Ersetzungskriterium erfüllen, wird in allen diesen Dokumenten die Ersetzung mit den jeweiligen Eingabedaten durchgeführt.

###### addNew 
{: #addnew}
 
Wenn keine Dokumente den Ersetzungskriterien entsprechen, prüft die change-API den Wert dieses Flags. Wenn das Flag auf **true** gesetzt ist, erstellt die change-API ein neues Dokument und fügt es zu dem Speicher hinzu. Andernfalls wird keine weitere Aktion ausgeführt.

###### markDirty 
{: #markdirty}
 
Bestimmt, ob die change-API ersetzte oder hinzugefügte Dokumente als vorläufig markiert.

Der Adapter gibt ein Daten-Array zurück:

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

Sie können andere APIs verwenden, um Änderungen an gespeicherten lokalen Dokumenten zu verfolgen. Verwenden Sie für die Sammlung, für die Sie Operationen ausführen, immer einen Zugriffsmechanismus.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

Dann können Sie Daten (Array mit JSON-Objekten) hinzufügen und entscheiden, ob diese als vorläufig markiert werden sollen. Normalerweise setzen Sie das Flag "markDirty" auf "false", wenn Sie Änderungen von der externen Quelle abrufen. Setzen Sie das Flag auf "true", wenn Sie Daten lokal hinzufügen.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

Sie können auch ein Dokument ersetzen und bei Bedarf entscheiden, ob das Dokument mit den Ersetzungen als vorläufig markiert werden soll.

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

Auf die gleiche Weise können Sie ein Dokument entfernen und das Entfernen bei Bedarf als vorläufig markieren. Dokumente, die entfernt und als vorläufig markiert wurden, werden nicht angezeigt, wenn Sie die find-API verwenden. Sie befinden sich aber noch in der Sammlung, bis Sie die `markClean`-API verwenden, die die Dokumente physisch aus der Sammlung entfernt. Wenn das Dokument nicht als vorläufig markiert ist, wird es physisch aus der Sammlung entfernt.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### Push
{: #push }
Viele Systeme verwenden den Begriff "Push" für das Senden von Daten an eine externe Quelle.

Es gibt drei wichtige Komponenten:

#### Interne Datenquellen-API für Push-Operation
{: #internal-data-source-api-push }
Diese Komponente ist die JSONStore-API, die Dokumente mit nur lokalen (vorläufigen) Änderungen zurückgibt.

#### Transportschicht für Push-Operation
{: #transport-layer-push }
Diese Komponente definiert, wie Sie Kontakt mit der externen Datenquelle aufnehmen möchten, um die Änderungen zu senden.

#### Push-Operation in externe Datenquelle
{: #external-data-source-push }
Diese Quelle ist in der Regel eine Datenbank, ein REST- oder SOAP-Endpunkt oder Ähnliches, die bzw. der die vom Client an den Daten vorgenommenen Aktualisierungen empfängt.

Alle nachfolgenden Codebeispiele sind Pseudocode, der so ähnlich wie JavaScript aussieht.

Verwenden Sie Adapter für die Transportschicht. Zu den Vorteilen, die Sie bei Verwendung von Adaptern haben, gehören die XML-JSON-Umsetzung, die Sicherheit, Filter und die Entkopplung von server- und clientseitigem Code.
{: note}

##### Interne Datenquellen-API: JSONStore  
{: #internal-data-source-api-jsonstore-head}
Wenn Sie einen Zugriffsmechanismus für die Sammlung haben, können Sie die `getAllDirty`-API aufrufen, um alle als vorläufig markierten Dokumente abzurufen. Diese Dokumente enthalten nur lokale Änderungen, die Sie über eine Transportschicht an die externe Datenquelle senden möchten.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

Das Argument `dirtyDocs` sieht wie im folgenden Beispiel aus:

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

Die Felder sind:
* `_id`:  Internes Feld, das von JSONStore verwendet wird. Jedem Dokument ist eine eindeutige ID zugeordnet.
* `json`: Daten, die gespeichert wurden.
* `_operation`:  Letzte Operation, die für das Dokument ausgeführt wurde. Gültige Werte sind "add", "store", "replace" und "remove".
* `_dirty`: Zeitmarke, die als Zahl gespeichert wird, um anzugeben, wann das Dokument als vorläufig markiert wurde.

##### Transportschicht: MobileFirst-Adapter  
{: #transport-layr-mobilefirst-adapter }
Sie können entscheiden, dass vorläufige Dokumente an einen Adapter zu gesendet werden sollen. Angenommen, Sie haben einen Adapter `people`, der mit einer Prozedur `updatePeople` definiert ist.

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

Sie können auch von den Vorteilen profitieren, die mit der Übergabe von `compressResponse`, `timeout` und anderen Parametern an die `WLResourceRequest`-API verbunden sind.
{: note}

Auf dem MobileFirst-Server gibt es für den Adapter die Prozedur `updatePeople`, die wie im folgenden Beispiel aussehen könnte:

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Anstatt die Ausgabe von der `getAllDirty`-API auf dem Client zu übertragen, müssen Sie möglicherweise die Nutzdaten aktualisieren, sodass sie ein vom Back-End erwartetes Format haben. Unter Umständen müssen Sie die Ersetzungs-, Lösch- und Einschlussoperationen auf separate Back-End-API-Aufrufe aufteilen.

Alternativ können Sie über das Array `dirtyDocs` iterieren und das Feld `_operation` überprüfen. Senden Sie dann Ersetzungsoperationen an eine Prozedur, Löschoperationen an eine andere Prozedur und Einschlussoperationen an eine weitere Prozedur. Im obigen Beispiel werden alle vorläufigen Dokumente in einem Schritt an den Adapter gesendet.

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Antworten in arguments[len] ansehen
  }
});
```
{: codeblock}

Alternativ können Sie den Adapter übergehen und direkten Kontakt zum REST-Endpunkt aufnehmen.

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

##### Externe Datenquelle: Back-End-REST-Endpunkt
{: #external-data-source-backend-rest-endpoint }
Das Back-End akzeptiert Änderungen oder lehnt sie ab und übermittelt dann eine Antwort an den Client. Wenn der Client die Antwort sieht, kann er Dokumente, die aktualisiert wurden, an die markClean-API übergeben.

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

Dokumente, die als bereinigt markiert sind, werden in der Ausgabe der `getAllDirty`-API nicht angezeigt.

## Fehlerbehebung für JSONStore
{: #troubleshooting-jsonstore }

## Beim Anfordern von Unterstützung Informationen bereitstellen
{: #provide-information-when-you-ask-for-help }
Es ist besser, mehr Informationen zur Verfügung zu stellen, als zu riskieren, dass nicht genügend Informationen bereitgestellt werden. Die folgende Liste ist ein guter Ausgangspunkt dafür, welche Informationen erforderlich sind, um Ihnen bei der Lösung JSONStore-Problemen behilflich sein zu können.

* Betriebssystem und Version. Beispielsweise virtuelle Maschine mit Windows XP SP3 oder Mac OSX 10.8.3.
* Eclipse-Version. Beispielsweise Eclipse Indigo 3.7 Java EE.
* JDK-Version. Beispielsweise Java SE Runtime Environment (Build 1.7).
* {{ site.data.keys.product }}-Version. Beispielsweise IBM Worklight V5.0.6 Developer Edition.
* iOS-Version. Beispielsweise iOS Simulator 6.1 oder iPhone 4S iOS 6.0 (nicht mehr verwendet, siehe "Veraltete Features und API-Elemente").
* Android-Version. Beispielsweise Android Emulator 4.1.1 oder Samsung Galaxy Android 4.0 API-Ebene 14.
* Windows-Version. Beispielsweise Windows 8, Windows 8.1 oder Windows Phone 8.1.
* ADB-Version. Beispielsweise Android Debug Bridge Version 1.0.31.
* Protokolle wie Xcode-Ausgaben unter iOS oder logcat-Ausgaben unter Android.

## Versuchen, das Problem zu isolieren
{: #try-to-isolate-the-issue }
Führen Sie die folgenden Schritte aus, um das Problem zu isolieren, um es präziser zu melden.

1. Setzen Sie den Emulator (Android) oder Simulator (iOS) zurück und rufen Sie die destroy-API auf, um mit einem bereinigten System zu beginnen.
2. Stellen Sie sicher, dass Sie in einer unterstützten Produktionsumgebung arbeiten.
    * Android >= 2.3-Emulator oder -Gerät (ARM v7/ARM v8/x86)
    * iOS >= 6.0-Simulator oder -Gerät (nicht mehr verwendet)
    * Windows 8.1/10-Emulator oder -Gerät (ARM/x86/x64)
3. Versuchen Sie, die Verschlüsselung zu inaktivieren, indem Sie kein Kennwort an die init- oder open-API übergeben.
4. Sehen Sie sich die von JSONStore generierte SQLite-Datenbankdatei an. Die Verschlüsselung muss inaktiviert sein.

   * Android-Emulator:

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * iOS-Simulator:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Windows 8.1 Universal- / Windows 10 UWP-Simulator

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

    Eine reine JavaScript-Implementierung, die in einem Web-Browser (Firefox, Chrome, Safari, Internet Explorer) ausgeführt wird, verwendet keine SQLite-Datenbank. Die Datei wird im lokalen HTML5-Speicher gespeichert.
    {: note}
   * Sehen Sie sich die Suchfelder (`searchFields`) mit `.schema` an und wählen Sie Daten mit `SELECT * FROM <collection-name>;` aus. Geben Sie `.exit` ein, um sqlite3 zu beenden. Wenn Sie einen Benutzernamen an die Methode "init" übergeben, hat die Datei den Namen **the-username.sqlite**. Wenn Sie keinen Benutzernamen übergeben, hat die Datei standardmäßig die Bezeichnung **jsonstore.sqlite**.
5. Aktivieren Sie für JSONStore die Option "verbose" (nur Android).

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```
    {: codeblock}

6. Verwenden Sie den Debugger.

## Allgemeine Probleme
{: #common-issues-jsonstore }
Wenn Sie die folgenden JSONStore-Merkmale verstehen, können Sie einige allgemeine Probleme lösen, die möglicherweise auftreten.  

* Die einzige Möglichkeit, Binärdaten in JSONStore zu speichern, besteht darin, sie zuerst in Base64 zu codieren. Speichern Sie Dateinamen oder Pfade anstelle der tatsächlichen Dateien in JSONStore.
* Der Zugriff auf JSONStore-Daten von nativem Code ist nur in {{ site.data.keys.v62_product_full }} V6.2.0 möglich.
* Es gibt abgesehen von der Begrenzung des Betriebssystems für mobile Geräte keine Begrenzung hinsichtlich der Datenmenge, die Sie in JSONStore speichern können.
* JSONStore stellt einen persistenten Datenspeicher bereit. Die Daten werden nicht nur im Speicher aufbewahrt.
* Die init-API schlägt fehl, wenn der Sammlungsname mit einer Ziffer oder einem Symbol beginnt. IBM Worklight Version 5.0.6.1 und aktuellere Versionen geben einen entsprechenden Fehler zurück: `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* In Suchfeldern wird zwischen Zahlen vom Typ "number" und vom Typ "integer" unterschieden. Numerische Werte wie `1` und `2` werden als `1.0` und `2.0` gespeichert, wenn der Typ `number` ist. Sie werden als `1` und `2` gespeichert, wenn der Typ `integer` ist.
* Falls eine Anwendung gestoppt werden muss oder abstürzt, lautet der Fehlercode immer -1, wenn die Anwendung erneut gestartet wird und die `init`- oder `open`-API aufgerufen wird. Sollte dieses Problem auftreten, rufen Sie zuerst die `closeAll`-API auf.
* Die JavaScript-Implementierung von JSONStore erwartet, dass Code seriell aufgerufen wird. Warten Sie, bis eine Operation beendet ist, bevor Sie die nächste Operation aufrufen.
* Transaktionen werden nicht in Android 2.3.x für Cordova-Anwendungen unterstützt.
* Wenn Sie JSONStore auf einem 64-Bit-Gerät verwenden, wird möglicherweise der folgende Fehler angezeigt: `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* Dieser Fehler bedeutet, dass es in Ihrem Android-Projekt native 64-Bit-Bibliotheken gibt. JSONStore funktioniert momentan nicht, wenn Sie diese Bibliotheken verwenden. Navigieren Sie zur Bestätigung in Ihrem Android-Projekt zu **src/main/libs** oder **src/main/jniLibs** und überprüfen Sie, ob es einen Ordner x86_64 oder arm64-v8a gibt. Ist das der Fall, löschen Sie diese Ordner, damit JSONStore wieder funktioniert.
* In einigen Fällen (oder Umgebungen) wird im Ablauf `wlCommonInit()` eingegeben, bevor das JSONStore-Plug-in initialisiert wird. Dadurch scheitern JSONStore-bezogene API-Aufrufe. Das Bootprogramm `cordova-plugin-mfp` ruft automatisch `WL.Client.init` auf, wonach die Funktion `wlCommonInit` ausgelöst wird. Dieser Initialisierungsprozess unterscheidet sich für das JSONStore-Plug-in. Das JSONStore-Plug-in hat keine Möglichkeit, den Aufruf von `WL.Client.init` zu stoppen (_halt_). In verschiedenen Umgebungen kann es vorkommen, dass im Ablauf `wlCommonInit()` eingegeben wird, bevor `mfpjsonjslloaded` abgeschlossen ist.
Der Entwickler kann die richtige Reihenfolge von `mfpjsonjsloaded` und `mfpjsloaded` sicherstellen indem er `WL.CLient.init`
manuell aufruft. Dadurch entfällt die Notwendigkeit für plattformspezifischen Code.

  Führen Sie die folgenden Schritte aus, um den manuellen Aufruf von `WL.CLient.init` zu konfigurieren:                              

  1. Ändern Sie in der Datei `config.xml` die Eigenschaft `clientCustomInit` in **true**.

  + Gehen Sie in der Datei `index.js` wie folgt vor:                                   
    * Fügen Sie am Anfang der Datei die folgende Zeile hinzu:                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * Übernehmen Sie den Aufruf von `WL.JSONStore.init` in `wlCommonInit()`.                    

    * Fügen Sie die folgende Funktion hinzu:  
    ```javascript                                         
    function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }
    ```                                                                     

Auf diese Weise wird (außerhalb von `wlCommonInit`) auf das Ereignis `mfpjsonjsloaded` gewartet.
So wird sichergestellt, dass das Script geladen wurde und nachfolgend `WL.Client.init` aufruft, was wiederum `wlCommonInit` auslöst und dann `WL.JSONStore.init` aufruft.

## Speicherinterna
{: #store-internals }
Es folgt ein Beispiel für die Speicherung von JSONStore-Daten.

Dieses vereinfachte Beispiel enthält folgende Schlüsselelemente:

* `_id` ist die eindeutige Kennung (z. B. AUTO INCREMENT PRIMARY KEY).
* `json` enthält eine exakte Darstellung des zu speichernden JSON-Objekts.
* `name` und `age` sind Suchfelder.
* `key` ist ein zusätzliches Suchfeld.

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |
{: caption="Tabelle 2. Beispiel für JSONStore-Datenschlüsselelemente" caption-side="top"}

Wenn Sie für die Suche eine der folgenden Abfragen oder eine Kombination der Abfragen `{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}` verwenden, wird das Dokument `{_id: 1, json: {name: 'carlos', age: 99} }` zurückgegeben.

Die übrigen internen JSONStore-Felder sind:

* `_dirty`: Bestimmt, ob das Dokument als vorläufig markiert wurde oder nicht. Dieses Feld ist nützlich, um Änderungen an den Dokumenten zu verfolgen.
* `_deleted`: Markiert, ob ein Dokument gelöscht wurde oder nicht. Dieses Feld ist nützlich, um Objekte aus der Sammlung zu entfernen, die später genutzt werden können, um mit Ihrem Back-End Änderungen zu verfolgen und zu entscheiden, ob diese entfernt werden sollen oder nicht.
* `_operation`: Zeichenfolge, die die letzte für das Dokument auszuführende Operation angibt (z. B. 'replace').

## JSONStore-Fehler
{: #jsonstore-errors }
### JavaScript-Fehler
{: #javascript-errors }
JSONStore verwendet ein Objekt `error`, um Nachrichten zur Ursache von Fehlern zurückzugeben.

Wenn während einer JSONStore-Operation (z. B. in den Methoden `find` und `add` der Klasse `JSONStoreInstance`) ein Fehler auftritt, wird ein Objekt `error` zurückgegeben. Es stellt Informationen zur Ursache des Fehlers bereit.

```javascript
var errorObject = {
  src: 'find', // Fehlgeschlagene Operation.
  err: -50, // Fehlercode.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Fehlernachricht.
  col: 'people', // Sammlungsname.
  usr: 'jsonstore', // Benutzername.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Fehlerbezogenes Dokument.
  res: {...} // Antwort von Server.
}
```
{: codeblock}
Nicht alle Schlüssel/Wert-Paare sind Teil jedes Fehlerobjekts. Der 'doc'-Wert ist beispielsweise nur verfügbar, wenn die Operation wegen eines Dokuments fehlgeschlagen ist (z. B. wenn die Methode `remove` in der Klasse `JSONStoreInstance`) ein Dokument nicht entfernen konnte.

### Objective-C-Fehler
{: #objective-c-errors }
Alle APIs, die fehlschlagen können, verwenden einen Fehlerparameter mit der Adresse eines NSError-Objekts. Wenn Sie nicht über Fehler benachrichtigt werden möchten, können Sie `nil` übergeben. Wenn eine Operation fehlschlägt, wird als Adresse ein NSError mit einer Fehlerangabe und potenziell mit Benutzerinformationen (`userInfo`) angegeben. Die Informationen für `userInfo` können zusätzliche Details enthalten (z. B. das Dokument, das den Fehler verursacht hat).

```objc
// Dieser NSError verweist auf einen Fehler, wenn ein Fehler auftritt.
NSError* error = nil;

// Löschung durchführen.
[JSONStore destroyDataAndReturnError:&error];
```

### Java-Fehler
{: #java-errors }
Alle Java-API-Aufrufe lösen je nach aufgetretenem Fehler eine bestimmte Ausnahme aus. Sie können jede Ausnahme einzeln behandeln oder `JSONStoreException` als Umbrella für alle JSONStore-Ausnahmen verwenden.

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Fehlerbedingung behandeln.
}
```
{: codeblock}
### Liste der Fehlercodes
{: #list-of-error-codes }
Liste der allgemeinen Fehlercodes und deren Beschreibung:

|Fehlercode      | Beschreibung |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Nicht erkannter Fehler. |
| -75 OS\_SECURITY\_FAILURE | Dieser Fehlercode bezieht sich auf das Flag 'requireOperatingSystemSecurity'. Er kann angezeigt werden, wenn die API 'destroy' die durch die Betriebssystemsicherheit (Touch-ID mit Rückgriff auf einen Kenncode) geschützten Sicherheitsmetadaten nicht entfernen kann oder die API 'init' bzw. 'open' die Sicherheitsmetadaten nicht finden kann. Der Fehler kann auch auftreten, wenn das Gerät keine Unterstützung für die Betriebssystemsicherheit bietet, die Verwendung der Betriebssystemsicherheit jedoch angefordert wurde. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore ist geschlossen. Versuchen Sie zuerst, die Methode 'open' in der JSONStore-Klasse aufzurufen, um den Zugriff auf den Store zu aktivieren. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | Beim Rollback der Transaktion ist ein Fehler aufgetreten. |
| -47 TRANSACTION\_FAILURE\_DURING\_REMOVE\_COLLECTION |Während einer laufenden Transaktion kann 'removeCollection' nicht aufgerufen werden. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | Während einer laufenden Transaktion kann 'destroy' nicht aufgerufen werden. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | Wenn Transaktionen vorhanden sind, kann 'closeAll' nicht aufgerufen werden. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | Während laufender Transaktionen kann ein Store nicht initialisiert werden. |
| -43 TRANSACTION_FAILURE | Es gab ein Problem mit Transaktionen. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | Der Rollback einer Transaktion kann nicht festgeschrieben werden, wenn keine laufende Transaktion vorhanden ist. |
| -41 TRANSACTION\_IN\_POGRESS | Während einer laufenden Transaktion kann keine neue Transaktion gestartet werden. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Es gibt ein Problem mit FIPS. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Es gibt ein Problem beim Abrufen der Dateiinformationen aus dem Dateisystem. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Es gibt ein Problem beim Ersetzen von Dokumenten aus einer Sammlung. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Es gibt ein Problem beim Entfernen von Dokumenten aus einer Sammlung.	. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Es gibt ein Problem beim Speichern des Schutzschlüssels für Daten. |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Es gibt ein Problem beim Indexieren der Eingabedaten. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Vergewissern Sie sich, dass die an die Suchfelder (searchFields) übergebenen Daten vom Typ 'string', 'integer', 'number' oder 'boolean' sind. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | Eine Operation für ein Dokument-Array, z. B. die Methode 'replace', kann bei der Ausführung für ein bestimmtes Dokument fehlschlagen. Das Dokument, das fehlgeschlagen ist, wird zurückgegeben und die Transaktion wird rückgängig gemacht. Bei Android tritt dieser Fehler auch bei dem Versuch auf, JSONStore in nicht unterstützten Architekturen zu verwenden. |
| -10 ACCEPT\_CONDITION\_FAILED | Die vom Benutzer angegebene Funktion 'accept' hat 'false' zurückgegeben. |
| -9 OFFSET\_WITHOUT\_LIMIT | Wenn Sie ein Offset verwenden möchten, müssen Sie ein Limit angeben. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Validierungsfehler. Muss eine positive Ganzzahl sein. |
| -7 INVALID_USERNAME | Validierungsfehler (zulässig sind nur [A-Z] oder [a-z] oder [0-9]). |
| -6 USERNAME\_MISMATCH\_DETECTED | Für die Abmeldung muss ein JSONStore-Benutzer zuerst die Methode 'closeAll' aufrufen. Es kann immer nur jeweils ein Benutzer vorhanden sein. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |Es gab ein Problem, als die Methode 'destroy' versucht hat, die Datei mit dem Store-Inhalt zu löschen. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Es gab ein Problem, als die Methode 'destroy' versucht hat, die Keychain (iOS) (iOS) oder gemeinsame Benutzervorgaben (Android) zu löschen. |
| -3 INVALID\_KEY\_ON\_PROVISION | An einen verschlüsselten Store wurde das falsche Kennwort übergeben. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | Suchfelder sind nicht dynamisch. Es ist nicht möglich, Suchfelder zu ändern, ohne die Methode 'destroy' oder 'removeCollection' vor der Methode 'init' oder 'open' mit den neuen Suchfeldern aufzurufen. Dieser Fehler kann auftreten, wenn Sie den Namen oder Typ des Suchfelds ändern. Beispiel: {key: 'string'} in {key: 'number'} oder {myKey: 'string'} in {theKey: 'string'}. |
| -1 PERSISTENT\_STORE\_FAILURE | Generischer Fehler. Es gab eine Fehlfunktion im nativen Code, wahrscheinlich beim Aufrufen der Methode 'init'. |
| 0 SUCCESS | In einigen Fällen gibt der native JSONStore-Code 0 zurück, um Erfolg anzuzeigen. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | Validierungsfehler. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | Validierungsfehler. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | Validierungsfehler. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | Validierungsfehler. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | Validierungsfehler. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | Validierungsfehler. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | Validierungsfehler. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |Die Abfrage, die alle als vorläufig markierten Dokumente auswählt, ist fehlgeschlagen. SQL-Beispiel für die Abfrage: SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | Wenn Funktionen wie die Methoden "push" und "load" der Klasse "JSONStoreCollection" verwendet werden sollen, muss ein Adapter an die Methode "init" übergeben werden. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | Validierungsfehler |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | Validierungsfehler |
| 12 ADAPTER_FAILURE | Es gab ein Problem beim Aufrufen von WL.Client.invokeProcedure, insbesondere beim Herstellen der Verbindung zum Adapter. Dieser Fehler unterscheidet sich von einem Fehler in dem Adapter, der versucht, ein Back-End aufzurufen. |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | Validierungsfehler |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | Das Aufrufen der Methode "enhance" der Klasse "JSONStoreCollection" zum Ersetzen einer vorhandenen Funktion ("find" und "add") ist nicht zulässig. |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Das Dokument wird per Push-Operation an einen Adapter gesendet, aber JSONStore kann das Dokument nicht als nicht vorläufig markieren. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | Wenn eine Sammlung mit einem Kennwort initialisiert werden soll, muss eine Verbindung zum {{ site.data.keys.mf_server }} bestehen, der ein "sicheres zufälliges" Token zurückgibt. Ab IBM Worklight Version 5.0.6 können Entwickler das sichere zufällige Token generieren, indem sie über das Objekt "options" {localKeyGen: true} an die Methode "init" übergeben. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | Es konnten keine Daten geladen werden, weil WL.Client.invokeProcedure das Fehler-Callback aufgerufen hat. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | Das an die Methode "init" übergebene Objekt "load" hat die Validierung nicht bestanden. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | Es gab ein Problem mit dem Schlüssel, der beim Aufrufen der Methode "add" im Objekt "load" verwendet wurde. |
| 20 UNDEFINED\_PUSH\_OPERATION | Für das Senden vorläufiger Dokumente per Push-Operation an den Server ist keine Prozedur definiert. Beispiel: Die Methoden "init" (neues Dokument ist vorläufig, operation = "add") und "push" (findet das neue Dokument mit operation = "add") wurden aufgerufen. In dem Adapter, der mit der Sammlung verbunden ist, wurde jedoch kein Schlüssel "add" mit der Prozedur "add" gefunden. Das Verbinden eines Adapters wird in der Methode "init" durchgeführt. |
| 21 INVALID\_ADD\_INDEX\_KEY | Es gab ein Problem mit zusätzlichen Suchfeldern. |
| 22 INVALID\_SEARCH\_FIELD | Eines Ihrer Suchfelder ist ungültig. Vergewissern Sie sich, dass keines Ihrer übergebenen Suchfelder __id,json, _deleted oder _operation ist. |
| 23 ERROR\_CLOSING\_ALL | Generischer Fehler. Beim Aufruf der Methode "closeAll" durch nativen Code ist ein Fehler aufgetreten. |
| 24 ERROR\_CHANGING\_PASSWORD | Das Kennwort kann nicht geändert werden. Das übergebene alte Kennwort war beispielsweise falsch. |
| 25 ERROR\_DURING\_DESTROY | Generischer Fehler. Beim Aufruf der Methode "destroy" durch nativen Code ist ein Fehler aufgetreten. |
| 26 ERROR\_CLEARING\_COLLECTION | Generischer Fehler. Beim Aufruf der Methode "removeCollection" durch nativen Code ist ein Fehler aufgetreten. |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | Validierungsfehler. |
| 28 INVALID\_SORT\_OBJECT | Das bereitgestellte Array für die Sortierung ist ungültig, da eines der JSON-Objekte ungültig ist. Die korrekte Syntax ist ein Array mit JSON-Objekten, in dem jedes Objekt nur eine einzige Eigenschaft enthält. Diese Eigenschaft sucht nach dem Feld, das für die Sortierung verwendet werden soll, und prüft, ob die Sortierung auf- oder absteigend sein soll. Beispiel: {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | Das bereitgestellte Array für das Filtern der Ergebnisse ist ungültig. Die richtige Syntax für dieses Array ist ein Array mit Zeichenfolgen, in dem jede Zeichenfolge ein Suchfeld oder ein internes JSONStore-Feld ist. Weitere Informationen finden Sie in "Speicherinterna". |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | Validierungsfehler, weil das Array kein reines Array mit JSON-Objekten ist. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | Validierungsfehler. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | Validierungsfehler. |
{: caption="Tabelle 3. Allgemeine Fehlercodes" caption-side="top"}
