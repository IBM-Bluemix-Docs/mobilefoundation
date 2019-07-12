---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: troubleshooting techniques

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Allgemeine Fehlerbehebung
{: #general-troubleshooting}

Anhand der Fehlerbehebungsinformationen können Sie Probleme, die bei {{site.data.keyword.IBM_notm}} Produkten auftreten, eingrenzen und beheben. Diese Informationen enthalten Anweisungen zur Verwendung der Problembestimmungsressourcen, die mit den {{site.data.keyword.IBM_notm}} Produkten, z. B. {{site.data.keyword.mobilefoundation_short}}, bereitgestellt werden.
{: shortdesc}

## Verfahren zur Fehlerbehebung
{: #ts_overview}

Die *Fehlerbehebung* ist eine systematische Methode zur Behebung eines Problems. Das Ziel der Fehlerbehebung ist es, festzustellen, warum etwas nicht wie erwartet funktioniert und wie das Problem behoben werden kann. Bestimmte einheitliche Verfahren können bei der Fehlerbehebung nützlich sein.

Der erste Schritt im Fehlerbehebungsprozess ist die vollständige Beschreibung des Problems. Problembeschreibungen unterstützen Sie und die Mitarbeiter des {{site.data.keyword.IBM_notm}} Technical Support bei der Suche nach der Problemursache. Dieser Schritt umfasst die folgenden grundlegenden Fragen:

- Welche Symptome weist das Problem auf?
- Wo tritt das Problem auf?
- Wann tritt das Problem auf?
- Unter welchen Bedingungen tritt das Problem auf?
- Kann das Problem reproduziert werden?

Die Antworten auf diese Fragen liefern in der Regel eine gute Beschreibung des Problems, die zu einer Problemlösung beitragen kann.

### Welche Symptome weist das Problem auf?

Bei der Beschreibung eines Problems lautet die offensichtlichste Frage **Was ist das Problem?** Diese Frage erscheint zunächst einfach. Sie können sie jedoch in mehrere fokussiertere Fragen unterteilen, die ein weitaus genaueres Bild des Problems liefern. Hierzu können die beispielsweise die folgenden Fragen gehören:

- Wer oder was meldet das Problem?
- Wie lauten die Fehlercodes und -nachrichten?
- Wie schlägt das System fehl? Handelt es sich beispielsweise um eine Schleife, eine Blockierung, einen Absturz, eine Leistungsverschlechterung oder ein fehlerhaftes Ergebnis?

### Wo tritt das Problem auf?

Den Ursprung des Problems festzustellen, ist nicht immer einfach. Dieser Schritt ist jedoch einer der wichtigsten bei der Behebung eines Problems. Zahlreiche Technologieebenen können sich zwischen der Komponente, die den Fehler meldet, und der Komponente, bei der der Fehler auftritt, befinden. Netze, Platten und Treiber sind nur einige der Komponenten, die bei der Untersuchung von Problemen zu berücksichtigen sind.

Mithilfe der folgenden Fragen können Sie den Ursprung des Problems eingrenzen, um so die Ebene zu isolieren, auf der der Fehler auftritt:

- Ist das Problem für eine Plattform oder ein Betriebssystem spezifisch oder tritt es plattform- bzw. betriebssystemunabhängig auf?
- Werden die aktuelle Umgebung und Konfiguration unterstützt?
- Tritt das Problem bei allen Benutzern auf?
- (Bei Installationen an mehreren Standorten.) Tritt das Problem an allen Standorten auf?

Wenn eine Ebene das Problem meldet, befindet sich der Ursprung des Problems nicht notwendigerweise auf dieser Ebene. Bei der Identifizierung des Problemursprungs spielen genaue Kenntnisse der betreffenden Umgebung eine wichtige Rolle. Nehmen Sie sich Zeit, um die Umgebung, in der das Problem auftritt, umfassend zu beschreiben. Zu den relevanten Informationen gehören z. B. das Betriebssystem und die Betriebssystemversion, die gesamte verwendete Software einschließlich der Versionsinformationen sowie Details zur Hardware. Vergewissern Sie sich, dass eine Umgebung verwendet wird, bei der es sich um eine unterstützte Konfiguration handelt. Zahlreiche Probleme sind auf nicht kompatible Softwareversionen zurückzuführen, die nicht zusammen ausgeführt werden dürfen oder die noch nicht abschließend auf Kompatibilität getestet wurden.

### Wann tritt das Problem auf?

Erstellen Sie eine detaillierte Zeitachse der Ereignisse, die im Vorfeld eines Fehlers auftreten, vor allem bei Problemen, die nur ein einziges Mal vorkommen. Am einfachsten lässt sich eine Zeitachse rückwärts erstellen: Beginnen Sie mit dem Zeitpunkt, zu dem ein Fehler gemeldet wurde (so genau wie möglich, gegebenenfalls bis auf die Millisekunde genau), und arbeiten Sie die verfügbaren Protokolle und Informationen zeitlich rückwärts durch. Normalerweise müssen Sie nur bis zum ersten verdächtigen Ereignis zurück gehen, das Sie in einem Diagnoseprotokoll finden.

Beantworten Sie die folgenden Fragen, um eine detaillierte Zeitachse der Ereignisse zu erstellen:

- Tritt das Problem ausschließlich zu einer bestimmten Uhrzeit auf?
- Wie häufig tritt das Problem auf?
- Welche Abfolge von Ereignissen tritt bis zu dem Zeitpunkt auf, an dem das Problem gemeldet wird?
- Tritt das Problem nach einer Umgebungsänderung auf, wie z. B. nach einem Upgrade oder der Installation von Software oder Hardware?

Die Antworten auf diese Art von Fragen können Ihnen ein Bezugssystem liefern, in dem Sie das Problem untersuchen können.

### Unter welchen Bedingungen tritt das Problem auf?

Ein wichtiger Teil der Fehlerbehebung besteht darin, festzustellen, welche Systeme und Anwendungen zum Zeitpunkt des Auftretens eines Problems aktiv sind. Diese Fragen zur verwendeten Umgebung können Sie bei der Ermittlung der zugrunde liegenden Fehlerursache unterstützen:

- Tritt das Problem immer bei der Ausführung derselben Task auf?
- Muss eine bestimmte Abfolge von Ereignissen stattfinden, damit das Problem auftritt?
- Treten bei anderen Anwendungen zum selben Zeitpunkt Fehler auf?

Die Antworten auf diese Art von Fragen unterstützen Sie bei der Beschreibung der Umgebung, in der das Problem auftritt, und bei der Korrelation möglicher Abhängigkeiten. Obwohl mehrere Probleme in der gleichen Zeit aufgetreten sind, muss das nicht bedeuten, dass die Probleme nicht unbedingt zusammenhängen.

### Kann das Problem reproduziert werden?

Aus Sicht der Fehlerbehebung ist das ideale Problem ein reproduzierbares Problem. Normalerweise stehen bei einem reproduzierbaren Problem zusätzliche Tools und Prozeduren für die Untersuchung zur Verfügung. Daher sind das Debugging und die Fehlerbehebung bei reproduzierbare Probleme häufig einfacher.

Reproduzierbare Probleme können jedoch auch einen Nachteil haben: Wenn das Problem einen signifikanten Einfluss auf die Geschäftsabläufe hat, soll es natürlich nicht erneut auftreten. Reproduzieren Sie das Problem in einer Test- oder Entwicklungsumgebung, falls möglich. Dies bietet Ihnen größere Flexibilität und Kontrolle bei der Untersuchung.

- Kann das Problem auf einem Testsystem reproduziert werden?
- Tritt derselbe Problemtyp bei mehreren Benutzern oder Anwendungen auf?
- Kann das Problem durch die Ausführung eines einzelnen Befehls, einer Befehlsgruppe oder einer bestimmten Anwendung reproduziert werden?


##  Bekannte Einschränkungen
{: #knownlimitations_mfp}

* In der Benutzerschnittstelle des {{site.data.keyword.mobilefoundation_short}}-Service wird zum Anzeigen von Zahlen nicht das vom Benutzer ausgewählte ländereinstellungsspezifische Muster verwendet.

## Hilfe und Unterstützung für Mobile Foundation anfordern
{: #getting_help_mobilefoundation}

Sollten bei der Verwendung von {{site.data.keyword.mobilefoundation_short}} Probleme auftreten oder Fragen entstehen, können Sie nach Informationen suchen oder Fragen in einem Forum stellen. Sie haben auch die Möglichkeit, ein Support-Ticket zu öffnen.

Wenn Sie zum Stellen einer Frage die Foren nutzen, versehen Sie Ihre Frage mit entsprechenden Tags, damit die {{site.data.keyword.Bluemix_notm}}-Entwicklerteams Ihre Frage auf Anhieb erkennen können.

Bei technischen Fragen zur Entwicklung und zur Bereitstellung einer App mit {{site.data.keyword.mobilefoundation_short}} können Sie Ihre Frage an [Stack Overflow ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://stackoverflow.com/search?q=ibm-mobilefirst+bluemix){:new_window} richten. Versehen Sie Ihre Frage mit den Tags `bluemix` und `ibm-mobilefirst`.

Bei Fragen zum Service sowie zu einführenden Anweisungen können Sie das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/mobilefirst/?smartspace=bluemix){:new_window} verwenden. Verwenden Sie dabei die Tags `bluemix` und `mobilefirst`.

Weitere Informationen zum Öffnen eines IBM Support-Tickets oder zu Supportebenen und zur Priorität von Tickets finden Sie im Abschnitt zum [Kontaktieren des Supports](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support){: new_window}.
