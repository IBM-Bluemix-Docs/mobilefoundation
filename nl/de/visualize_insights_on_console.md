---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, charts, visualize data, analytics console

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Informationen in der Konsole visualisieren
{: #visualize_insights_on_console}

Zur Visualisierung von Insights aus den Analysedaten, die erfasst und von Ihrer Anwendung gesendet wurden, müssen Sie die Mobile Analytics Console über die Mobile Foundation Operations Console durch Klicken auf **Analytics Console** im Navigationsbereich starten.

Die Mobile Analytics Console kann in zwei Modi ausgeführt werden:
  - **Demo-Modus EIN** Dieser Modus ist ausschließlich für Demonstrationszwecke; dabei werden mithilfe von Simulationsdatenfeeds unterschiedliche Analyseansichten (Diagramme und Tabellen) dargestellt.
  - **Demo-Modus AUS** In diesem Modus werden die verschiedenen Analyseansichten dargestellt, die auf Echtzeitdatenfeeds aus Ihren Anwendungen basieren, die [für Mobile Analytics instrumentiert wurden](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

Sämtliche Analyseansichten können durch die Anwendung von Filtern in Bezug auf *Anwendungsname*, *Version*, *Betriebssystem des Geräts* und *Zeitraum* in ihrer Anzahl reduziert werden; dadurch erzielen Sie Insights aus unterschiedlichen Perspektiven.

Zur Visualisierung von Insights für Ihre Anwendung müssen Sie Folgendes sicherstellen.
  - Ihre Anwendung ist instrumentiert, um die relevanten Analysedaten zu erfassen und an den Mobile Analytics-Service zu senden.
  - Sie haben den Demo-Modus in der Analytics Console inaktiviert.
  - Sie wenden die richtigen Filter an. Wählen Sie z. B. einen Zeitraum für die Praxisbereitstellung Ihrer Anwendung sowie einen Zeitraum für die Nutzung durch Benutzer aus.

Die Mobile Analytics Console stellt unterschiedliche Typen von Analysen für die Nutzung der mobilen Anwendung sowie zur Leistung bereit, und zwar auf Basis der Kategorisierung im Navigationsbereich der Analytics Console. In den folgenden Abschnitten werden die verschiedenen Analyseansichten detailliert beschrieben:


## Benutzer
{: #reports_visualized_users}
Mit dieser Ansicht erhalten Sie Insights in *Muster der Benutzereinbindung* wie z. B. in Bezug auf die Anzahl der aktiven Benutzer, die die App in einem angegebenen Datumsbereich verwendet haben, sowie in Bezug auf einen Vergleich zwischen der Anzahl neuer Benutzer und der Anzahl bereits vorhandener Benutzer, die Ihre App wieder verwenden.
Die Diagramme in dieser Ansicht können nach *App-Name*, *Betriebssystem* oder *Betriebssystemversion* gefiltert werden.

## Sitzungen
{: #reports_visualized_sessions}
Mithilfe dieser Ansicht können Sie Insights zu den *Verwendungsmustern* Ihrer Anwendung hinsichtlich der *App-Sitzungen* für den angegebenen Datumsbereich erhalten. Eine Sitzung wird aufgezeichnet, wenn eine App in den Vordergrund eines Geräts gebracht wird.  Sie erhalten Insights dazu, zu welchen Uhrzeiten Ihre Anwendung am meisten und am wenigsten genutzt wird; diese Informationen können für nützliche Business Insights genutzt werden. Die Diagramme in dieser Ansicht können nach *App-Name*, *Betriebssystem* oder *Betriebssystemversion* gefiltert werden.

## Netzanforderungen
{: #reports_visualized_network_requests}
Mithilfe dieser Ansicht können Sie Insights zur Oberfläche und Bedienbarkeit Ihrer Anwendung erhalten, da API-Aufrufe für die Back-End-Systeme getätigt werden.  In dieser Ansicht sind Tabellen und Diagramme enthalten, die Ihnen einen Einblick in die am häufigsten verwendeten Funktionen Ihrer Back-End-Systeme verschaffen und Ihnen zeigen, wie es mit der bisherigen Antwortzeit und Stabilität aussieht und ob Sie eine Neuverteilung Ihrer Back-End-Systeme in Betracht ziehen müssen.

Diese Ansicht enthält Diagramme, die in Abhängigkeit eines ausgewählten Datenbereichs die durchschnittliche Umlaufzeit der ausgehenden API-Aufrufe Ihrer Anwendung, die Anzahl der Anforderungen pro API-Aufruf sowie die Anzahl erfolgreicher und fehlgeschlagener Anforderungen nach Antwortcodes aufzeichnen.  Die Diagramme in dieser Ansicht können nach App-Name, Betriebssystem oder Betriebssystemversion gefiltert werden.

## Abstürze
{: #reports_visualized_crashes}
In dieser Ansicht werden Insights in Bezug auf die Stabilität Ihrer Anwendung in einen bestimmten Zeitraum dargestellt und Sie erhalten Unterstützung bei der Entscheidung darüber, ob Ihr Anwendungsdesign/Ihre Anwendungsimplementierung in irgendeiner Form korrigiert werden muss.  Es werden Diagramme bereitgestellt, bei denen die Anzahl der Abstürze in Kontrast zu der Gesamtanzahl der Verwendungen und der Gesamtabsturzrate gesetzt wird.  Die Diagramme in dieser Ansicht können nach *App-Name*, *Betriebssystem* oder *Betriebssystemversion* gefiltert werden.


## Fehlerbehebung
{: #reports_visualized_troubleshooting}
In dieser Ansicht werden alle notwendigen Informationen bereitgestellt, die ein Anwendungsentwickler möglicherweise für die Fehlerbehebung in Bezug auf eine Anwendung benötigt.  Diese Ansicht enthält eine detailliertere Analyse der Abstürze der Anwendung in Bezug auf die betroffenen Geräte, das Hostbetriebssystem, die jeweilige Zeit des Absturzes, den Stack-Trace zum Zeitpunkt des Absturzes sowie Absturzprotokolle, die für eine detailliertere Analyse heruntergeladen werden können.  

Absturzprotokolle werden gesammelt, indem nach App-Protokollen gesucht wird, die auf der Protokollebene 'Schwerwiegend' (FATAL) erfasst werden.  Das native Analytics-Client-SDK für Android und iOS verarbeitet nicht abgefangene Ausnahmebedingungen und protokolliert Details dazu im Rahmen von Protokollnachrichten der Ebene 'Schwerwiegend' (FATAL).  Im Falle von Cordova müssen jedoch alle Abstürze auf der JavaScript-Ebene vom Entwickler bearbeitet werden und Absturzprotokolle, die an den Mobile Analytics-Service gesendet werden, müssen in der Mobile Analytics-Konsole angezeigt und analysiert werden.
{: note}


## Benutzerfeedback
{: #reports_visualized_userfeedback}
In dieser Ansicht werden Insights in Bezug auf die tatsächliche interaktive Oberfläche und Bedienbarkeit Ihrer Benutzer bei der Verwendung der App beschrieben und wie sich die Benutzer dabei fühlen.

* **App-Eigner** können eine ausführliche, kontextumfassende Ansicht zu Programmfehlern sowie andere Feedback-Daten abrufen, die von Benutzern und Testern gesendet werden und während der Anwendungsausführung erfasst wurden.
* **Entwickler** erhalten präzise Anwendungskontexte, um Programmfehler oder Funktionslücken zu diagnostizieren und zu beheben.
* **App-Eigner** und **Entwickler** können mithilfe dieser Ansicht auch Aktionen zum empfangenen Feedback verwalten, z. B. das Erfassen von Kommentaren oder Links zu Problemen, die auf Fehlersuchsystemen erstellt wurden.  Für jedes Feedback kann auch ein allgemeiner Prüfstatus festgelegt werden, den Sie zur Auswertung der Aktionen zum Benutzerfeedback nutzen können.

## Angepasste Diagramme
Mit dieser Ansicht wird Mobile Analytics um angepasste Fälle erweitert, bei denen **App-Eigner** und **Entwickler** ihre eigenen anwendungsspezifischen Analysen erstellen können.   Mit dieser Funktion können Sie Ihre eigenen Analyseansichten (Diagramme, Tabellen usw.) um die Standardanalysedaten, die vom Client SDK erfasst werden, und auch angepasste Daten oder anwendungsspezifische Daten, die protokolliert werden, erstellen.  Weitere Informationen zu dieser erweiterten Analysefunktion finden Sie [hier](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts).
