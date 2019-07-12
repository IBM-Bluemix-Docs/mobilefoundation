---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Direct Update
{: #direct_update}

Webressourcen (JavaScript, HTML, CSS oder Bilddateien) in Cordova- oder Ionic-Anwendungen können mit **Direct Update** drahtlos aktualisiert werden, ohne dass die Benutzer ein App Store- oder Play Store-Update durchführen müssen. Durch Verwendung der Direct Update-Funktion können Unternehmen sicherstellen, dass die Endbenutzer die jeweils aktuelle Version ihrer Apps nutzen. Um eine Anwendung zu aktualisieren, müssen die aktualisierten Webressourcen der Anwendung mithilfe der Mobile Foundation-CLI oder durch Implementieren einer generierten Archivdatei gepackt und in Ihre Mobile Foundation-Instanz hochgeladen werden. Direct Update wird darauf automatisch aktiviert. Danach muss es bei jeder Benutzeranforderung für eine geschützte Ressource erzwungen werden.

![Diagramm für die Funktionsweise von Direct Update](images/internal_function.jpg)

Direct Update wird auf der Cordova iOS- und der Cordova Android-Plattform unterstützt.
{: note}

Für die Live-Produktion oder die Vorproduktions-Testphase wird empfohlen, Secure Direct Update zu implementieren, bevor Sie Ihre Anwendung im App Store veröffentlichen. Für Secure Direct Update ist ein RSA-Schlüsselpaar erforderlich, welches aus einem reellen signierten Serverzertifikat extrahiert wird.

1. Achten Sie darauf, die Keystore-Konfiguration nicht mehr zu ändern, nachdem die Anwendung veröffentlicht ist. Heruntergeladene Aktualisierungen können erst authentifiziert werden, nachdem die Anwendung mit einem neuen öffentlichen Schlüssel rekonfiguriert und erneut veröffentlicht wurde. Werden die beiden vorherigen Schritte nicht ausgeführt, schlägt Direct Update beim Client fehl. 
2. Mit Direct Update werden nur die Webressourcen der Anwendung aktualisiert. Um native Ressourcen zu aktualisieren, muss eine neue Anwendungsversion an das jeweilige App Store übergeben werden.
3. Wenn Sie die Funktion des Direct Update nutzen und die Prüfsummen-Funktion für Webressourcen aktiviert ist, wird mit jedem Direct Update eine neue Kontrollsummenbasis eingerichtet.
4. Wurde der Mobile Foundation-Server aufgerüstet, bedient er Direct Updates weiterhin ordnungsgemäß. Wurde jedoch ein erst kürzlich erstelltes Direct Update-Archiv (ZIP-Datei) hochgeladen, kann dieses Aktualisierungen an älteren Clients stoppen. Der Grund dafür ist, dass das Archiv die Version des `cordova-plugin-mfp`-Plug-ins enthält. Bevor er mit diesem Archiv einen mobilen Client bedient, vergleicht der Server die Clientversion mit der Plug-in-Version. Liegen die beiden Versionen nahe genug beieinander (d. h. die drei wichtigsten Ziffern sind identisch), wird Direct Update normal durchgeführt. Andernfalls überspringt der Mobile Foundation-Server im Hintergrund die Aktualisierung. Eine Lösung für die Versionsabweichung ist das Herunterladen von `cordova-plugin-mfp` mit derselben Version wie die in Ihrem ursprünglichen Cordova-Projekt und das Neugenerieren des Direct Update-Archivs.
{: tip}

Nach einem Direct Update verwendet die Anwendung die vorgepackten Webressourcen nicht mehr. Stattdessen verwendet sie die heruntergeladenen Webressourcen aus der Sandbox der Anwendung. Wird der Cache der Anwendung im Gerät gelöscht, werden die ursprünglichen gepackten Webressourcen erneut verwendet.

Ein Direct Update von Webressourcen wird nur auf eine bestimmte Version der App angewendet. Aktualisierungen, die für eine Anwendung der Version 2.0 generiert wurden, werden beispielsweise nicht auf Version 2.1 derselben App angewendet.
{: note}

## Aktualisierte Webressourcen erstellen und implementieren
{: #creating_deploying_updates}

Nachdem Sie Änderungen an Ihren Webressourcen vorgenommen haben, können Sie sie mithilfe der Mobile Foundation-CLI packen und in Ihre Mobile Foundation-Instanz hochladen.

1.  Mit Direct Update werden nur die Webressourcen der Anwendung aktualisiert. Um native Ressourcen zu aktualisieren, muss eine neue Anwendungsversion an das jeweilige App Store übergeben werden.
2. Der Mobile Foundation-Service stoppt ein Direct Update an Clients, wenn der native Teil der App sich erheblich von dem in den Mobile Foundation-Service hochgeladenen Teil unterscheidet. Dies kann passieren, wenn das *cordova-plugin-mfp* mit einer neueren Version des Plug-ins aktualisiert wird. Bevor er mit dem Archiv einen mobilen Client bedient, vergleicht der Server die Clientversion mit der Plug-in-Version. Liegen die beiden Versionen nahe genug beieinander (d. h. die drei wichtigsten Ziffern der Versions-ID sind identisch), wird Direct Update normal durchgeführt. Andernfalls überspringt der Mobile Foundation-Server im Hintergrund die Aktualisierung. Eine Lösung für die Versionsabweichung ist das Herunterladen von *cordova-plugin-mfp* mit derselben Version wie die in Ihrem ursprünglichen Cordova-Projekt und das Neugenerieren des Direct Update-Archivs.
{: tip}

1. Öffnen Sie ein Befehlszeilenfenster und navigieren Sie zum Stamm des Cordova-Projekts.
2. Führen Sie den Befehl aus:
  ```bash
  mfpdev app webupdate
  ```
  Der Befehl `mfpdev app webupdate` packt die aktualisierten Webressourcen in eine `.zip`-Datei und lädt diese an den standardmäßigen Mobile Foundation-Server hoch, der in der Entwicklerworkstation ausgeführt wird. Die gepackten Webressourcen sind im Ordner `[cordova-project-root-folder]/mobilefirst/` zu finden.

Informationen zu alternativen Schritten zur Aktualisierung von Webinhalten in Ihrer App finden Sie [hier](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app).

## Erweiterte Konfiguration für Direct Update
{: #direct_update_advanced_config}

Informationen zur erweiterten Konfiguration von Direct Update finden Sie [hier](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration).
