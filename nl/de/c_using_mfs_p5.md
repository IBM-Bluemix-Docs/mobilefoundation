---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation, mobile analytics, professional plan, configure database

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Professional Per Device-Plan verwenden
{: #using_mobilefoundation_p5}

Mit dem Professional Per Device-Plan können Benutzer mobile Anwendungen erstellen, testen und in Produktionsumgebungen ausführen, unabhängig von der Anzahl mobiler Benutzer oder Geräte. Die Gebühren basieren auf der Anzahl der täglichen Clientgeräte. Dieser Plan unterstützt große Bereitstellungen und die Hochverfügbarkeit.
Nach der Erstellung der Serviceinstanz von {{site.data.keyword.mobilefoundation_short}}: Professional Per Device lesen Sie die folgende Prozedur, um die Arbeit mit dem Service zu beginnen.

## Voraussetzungen für den Professional Per Device-Plan
{: #prerequisites_p5}

Beachten Sie Folgendes, bevor Sie die Serviceinstanz von {{site.data.keyword.mobilefoundation_short}}: Professional Per Device konfigurieren.
* {{site.data.keyword.mobilefoundation_short}}: Professional Per Device wird nur mit {{site.data.keyword.Db2_on_Cloud_short}} (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}}-Plänen unterstützt.

* Sie benötigen Zugriff auf die Berechtigungsnachweise der {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz, bevor Sie die Einstellungen für die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz konfigurieren.

Die {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz kann sich in jedem `Bereich` in Ihrer {{site.data.keyword.Bluemix_notm}}-`Organisation` bzw. in jeder anderen `Organisation` befinden, auf die Sie zugreifen können. Stellen Sie sicher, dass Sie über die Berechtigungen für den Zugriff auf den `Bereich` verfügen, in dem sich die {{site.data.keyword.Db2_on_Cloud_short}}- oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz befindet.
{: note}

## Datenbankverbindung hinzufügen
{: #configure_dashdb_p5}

###  Erste Schritte
{: #firststeps_p5}

Führen Sie nach der Erstellung der Serviceinstanz von {{site.data.keyword.mobilefoundation_short}}: Professional Per Device die beschriebene Prozedur aus, um mit der Verwendung des Service zu beginnen.

### Datenbankverbindung einrichten
{: #connect_dashdb_p5}

Nachdem die {{site.data.keyword.mobilefoundation_short}}: Professional Per Device-Serviceinstanz erstellt wurde, wird die Seite *Übersicht* angezeigt, auf der Sie die Verbindungsinformationen für die {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz angeben, zu der die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz eine Verbindung herstellen soll.

Sie können auch eine neue {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz erstellen, wenn keine solche Instanz vorhanden ist.

Führen Sie die folgenden Schritte aus, um eine neue Db2 on Cloud-Serviceinstanz zu erstellen:

1. Wählen Sie auf der Seite *Übersicht* den Abschnitt **Neuen Service erstellen** aus.

+ Wählen Sie `Ja` für die Option **Hochverfügbarkeitskonfiguration** aus, wenn Sie eine {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz mit Hochverfügbarkeitsfunktionalität benötigen.

+ Prüfen Sie die Plandetails und klicken Sie auf **Erstellen**.

Eine neue {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz wird erstellt. Mit dieser steht eine dedizierte {{site.data.keyword.Db2_on_Cloud_short}}-Instanz mit 8 GB RAM und 2 vCPUs sowie 500 GB Speicher zur Verfügung.

Führen Sie die folgenden Schritte aus, um die Verbindung zu einer vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz oder zu der Serviceinstanz von {{site.data.keyword.Db2_on_Cloud_short}} herzustellen, die Sie erstellt haben:

1. Wählen Sie die {{site.data.keyword.Bluemix_notm}} `Organisation` aus, in der sich die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet.

+ Wählen Sie den {{site.data.keyword.Bluemix_notm}}-`Bereich`, in dem sich die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet, in der Liste der Bereiche aus, die in der ausgewählten `Organisation` verfügbar sind.   
Wenn die `Organisation` und der `Bereich`, in denen sich Ihre {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet, nicht angezeigt werden, prüfen Sie, ob Sie ein Mitglied der betreffenden `Organisation` bzw. des betreffenden `Bereichs` sind. Sie benötigen die Zugriffsrolle *Entwickler* für die Organisation und den Bereich, da der {{site.data.keyword.mobilefoundation_short}}-Service über den {{site.data.keyword.Db2_on_Cloud_short}}-Service auf die Berechtigungsnachweise zugreift.
{: note}
+ Wählen Sie den `Servicenamen` und die `Berechtigungsnachweise` für {{site.data.keyword.Db2_on_Cloud_short}} aus, um eine Verbindung zur vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz herzustellen.

+  Testen Sie die Verbindung zur angegebenen {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz.

+  Klicken Sie auf **Hinzufügen**. Mit dieser Aktion werden die erforderlichen Tabellen in der konfigurierten {{site.data.keyword.Db2_on_Cloud_short}}-Datenbankserviceinstanz erstellt.

Nach einigen Sekunden können Sie auf die Seite `Übersicht` zugreifen, auf der die Lernprogramme und Videos zur Verfügung stehen, die Sie beim Einstieg in die Verwendung des {{site.data.keyword.mobilefoundation_short}}-Service unterstützten.

Sie können die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz, die zur Verwendung durch die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz konfiguriert ist, nicht ändern. Sie können jedoch dieselbe {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz für mehrere {{site.data.keyword.mobilefoundation_short}}-Serviceinstanzen verwenden, da jede {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz ein eigenes Schema in der ausgewählten {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz erstellt.
{: note}

## Erstellten MobileFirst-Server mit Professional Per Device-Plan starten
{: #start_mobilefoundation_p5}

* Um den {{site.data.keyword.mfserver_short_notm}} mit den Standardeinstellungen zu starten, klicken Sie auf **Basisserver starten**.

* Diese Auswahl erstellt einen {{site.data.keyword.mfserver_long_notm}} mit den folgenden Einstellungen:
    -  Zwei Knoten mit jeweils 1 GB Hauptspeicher. Diese Größe ist für Entwicklungs- und kleinere Testaktivitäten sowie für kleinere Produktionsworkloads ausreichend.

    -	`Benutzername` und `Kennwort` werden automatisch für Sie generiert. Sie können darauf zugreifen, wenn der Server betriebsbereit ist.

Der Prozess der Erstellung Ihres Servers wird gestartet. Dieser Prozess dauert ungefähr 10 Minuten; in einem Nachrichtenfenster wird der Fortschritt dieser Operation angezeigt. Ist der Vorgang abgeschlossen, wird ein Dashboard mit folgenden Informationen angezeigt:

  -	Status Ihres Servers, der ausgeführt wird (Zustand, Größe).

  -	Für Sie erstellte Serverroute. Verwenden Sie diese Route in Ihrer mobilen Anwendung, um eine Verbindung zum {{site.data.keyword.mfserver_short_notm}} herzustellen.

  -	Ihr persönlicher `Benutzername` und das `Kennwort` für den Zugriff auf die {{site.data.keyword.mfp_oc_short_notm}}. Das `Kennwort` wird ausgeblendet. Klicken Sie auf das Symbol **Kennwort anzeigen**, um es einzublenden.

*	Klicken Sie auf **Konsole starten**, um die {{site.data.keyword.mfp_oc_short_notm}} zu öffnen.


Mit der Konsole können Sie Ihre mobilen Apps, Adapter und Geräte verwalten, Ihren Server als mobiles Back-End verwenden, Push-Benachrichtigungen senden usw.

## MobileFirst-Server unter Verwendung des Professional Per Device-Plans neu erstellen
{: #recreate_mobilefoundation_p5}

*	Klicken Sie auf die Schaltfläche **Neu erstellen**, um den Server erneut zu erstellen.

* Diese Aktion stoppt Ihre vorhandenen Server und löscht die Daten. Eine neue Serverinstanz wird mit einer aktualisierten Version erstellt, falls verfügbar. Diese Aktion nimmt einige Minuten in Anspruch.

Daten der vorherigen Serverinstanz, einschließlich Informationen zu den Apps und Adaptern, werden in der konfigurierten {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz beibehalten. Diese Daten werden zur erneuten Erstellung des Servers verwendet.
{: note}

##	Erweiterte Konfiguration im Professional Per Device-Plan einrichten
{: #using_mfs_advanced_p5}

Mit der Option **Server mit erweiterter Konfiguration starten** auf der Seite `Übersicht` können Sie einen Server mit erweiterten oder benutzerdefinierten Einstellungen erstellen. Sie können die Servereinstellungen auch aktualisieren, um Ihre Serverkonfiguration anzupassen; klicken Sie hierfür auf die Registerkarte **Einstellungen**. {{site.data.keyword.mobilefoundation_short}} bietet Ihnen Zugriff auf einige erweiterte Einstellungen.

*	Auf der Registerkarte **Topologie** können Sie die Servergröße und die Anzahl der Serverinstanzen auswählen, die den jeweiligen Anforderungen entsprechen. Die Standardgröße von 1 GB für den Server ist für die Entwicklung und kleinere Tests ausreichend.
  - Wählen Sie in Abhängigkeit von Ihrem Bedarf die richtige Größe für Ihren Server aus.

  - **Instanzen** zeigt die Anzahl der erstellten Instanzen an.

## Mobile Analytics im Professional Per Device-Plan
{: #mobile_analytics_p5}

Mobile Analytics-Server ist in der Serviceinstanz des Mobile Foundation: Developer-Plans enthalten und vorkonfiguriert.

* Starten Sie die Mobile Analytics Console über {{site.data.keyword.mfp_oc_short_notm}}.

Weitere Informationen zu Mobile Analytics finden Sie unter [hier](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}.
