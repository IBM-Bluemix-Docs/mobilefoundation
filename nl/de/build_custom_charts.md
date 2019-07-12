---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, charts, app sessions, crashes, graph

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

# Angepasste Diagramme erstellen
{: #build_custom_charts}

Die Ansicht 'Angepasste Diagramme' in der Mobile Analytics-Konsole bietet Ihnen die Flexibilität, um eigene Visualisierungen für die erfassten und gespeicherten Analysedaten zu erstellen.  Die angepassten Diagramme unterstützen Sie beim weiteren Vertiefen der bereits bereitgestellten Informationen bzw. beim reibungslosen Integrieren dieser Insights mit Business Analytics und angepassten Daten.

In dieser Ansicht können Sie eine beliebige Anzahl der unterstützten Analysedatasets und anschließend einen der unterstützten Diagrammtypen zur Abbildung des Datasets auswählen.  Sie können die Visualisierung sogar noch feiner differenzieren, indem Sie Filter definieren, die auf die abgebildeten Daten angewendet werden.  

Unterstützte Datasets:
 * App-Protokolle
 * App-Sitzungen
 * Angepasste Daten
 * Netztransaktionen

Unterstützte Diagrammtypen:
 * Balkendiagramm
 * Ablaufdiagramm
 * Kurvendiagramm
 * Messgrößengruppe
 * Kreisdiagramm
 * Tabelle

Die Auswahl von Dataset, abzubildendem Diagrammtyp, Definition der Diagrammmerkmale und der anzuwendenden Datenfilter kann in einer entsprechenden Definition konzentriert und gespeichert werden.  Sie können so viele Definitionen zu angepassten Diagrammen erstellen und speichern wie Sie benötigen. Die gespeicherten angepassten Diagramme werden in der entsprechenden Ansicht mit den abgebildeten relevanten Analysedaten gespeichert.

## Angepasstes Diagramm erstellen
{: #creating_custom_chart}

Erstellen Sie ein angepasstes Diagramm, indem Sie die folgenden Schritte ausführen:

1.  Klicken Sie im Mobile Analytics-Dashboard auf der Registerkarte **Angepasste Diagramme** auf die Schaltfläche **Diagramm erstellen**.
2.  Wählen Sie auf der Registerkarte **Allgemeine Einstellungen** die Optionen **Diagrammtitel**, **Ereignistyp** sowie **Diagrammtyp** aus.
3.  Bei Auswahl der Optionen *Ereignistyp* und *Diagrammtyp* wird die Registerkarte **Diagrammdefinition** angezeigt. Verwenden Sie die Registerkarte *Diagrammdefinition*, um das Diagramm für den angegebenen Diagrammtyp zu definieren, den Sie zuvor ausgewählt haben. Nachdem Sie das Diagramm definiert haben, können Sie die Diagrammfilter und die Diagrammeigenschaften festlegen.
4.  **Diagrammfilter** werden verwendet, um das angepasste Diagramm zu optimieren. Für ein Diagramm kann mindestens ein Filter definiert werden.
    Wenn Sie beispielsweise ein Diagramm zur Visualisierung der durchschnittlichen App-Sitzungsdauer definiert haben, können Sie einen Filter wie folgt erstellen, wenn Sie dieses Diagramm nur für eine bestimmte App visualisieren möchten:
    * Wählen Sie **Anwendungsname** als **Eigenschaft** aus.
    * Wählen Sie **Gleich** als **Operator** aus.
    * Wählen Sie den Namen Ihrer App für **Wert** aus.
    * Klicken Sie auf **Filter hinzufügen**.
    Der Filter nach App-Name wird zur Tabelle mit den Filtern für Ihr Diagramm hinzugefügt.
5.  Diagrammeigenschaften stehen für die Diagrammtypen **Tabelle**, **Balkendiagramm** und **Kurvendiagramm** zur Verfügung. Mit den Diagrammeigenschaften soll die Darstellung der Daten verbessert werden, sodass die Visualisierung effektiver ist.
    Wenn Sie ein Diagramm des Typs **Tabelle** erstellt haben, werden die Diagrammeigenschaften so festgelegt, dass die Tabellenseitengröße, das Feld, nach dem sortiert werden soll, und die Sortierreihenfolge des Felds definiert werden.
    Haben Sie ein **Balkendiagramm** oder ein **Kurvendiagramm** erstellt, werden die Diagrammeigenschaften auf Beschriftungsschwellenwertzeilen gesetzt, damit für alle Personen ein Bezugsrahmen hinzugefügt wird, die das Diagramm überwachen.

## Angepasste Insights aus angepassten Datenprotokollen abrufen
{: #creating_custom_chart_for_client_logs}    

Wünschen Sie differenzierte angepasste Insights wie z. B. Informationen zu Benutzern, die sich in der Anwendung bewegen, müssen Sie zunächst die relevanten Informationen zur Benutzerverfolgung als angepasste Daten erfassen und diese protokollieren; dazu gehören z. B. ausgewählte Seiten, ausgewählte Optionen oder angeklickte Schaltflächen.  Informationen zur Vorgehensweise beim Protokollieren von angepassten Daten finden Sie im Thema zur [Instrumentierung Ihrer App](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

Erstellen Sie als Nächstes eine Definition für angepasste Diagramme mit 'Angepasste Daten' als Ereignistyp und wählen Sie einen Diagrammtyp aus. Wenn Sie mit der Definition von **Diagrammeigenschaften** oder **Diagrammfilter** fortfahren, werden die Typen und Werte zu den angepassten Daten in den Dropdown-Feldern angezeigt.  Nehmen Sie eine relevante Auswahl für die Art von Insights vor, nach denen Sie suchen.  

Tiefe und Nutzen angepasster Insights sind gänzlich davon abhängig, wie effektiv oder relevant die Definition und Erfassung angepasster Daten in Ihren Anwendungen ist.
{: note}

## Diagrammtypen
{: #types_of_charts}

### Balkendiagramm
{:  #bar_graph}

Das Balkendiagramm ermöglicht die Visualisierung numerischer Daten über eine X-Achse. Wenn Sie ein Balkendiagramm definieren, müssen Sie zuerst den Wert für die X-Achse auswählen. Sie können aus den folgenden möglichen Werten auswählen.

* **Zeitplan** - Wenn Sie Ihre Daten im Trend sehen möchten (z. B. die durchschnittliche Sitzungsdauer über einen bestimmten Zeitraum), müssen Sie *Zeitplan* für die X-Achse auswählen.
* **Eigenschaft** - Wählen Sie 'Eigenschaft' aus, wenn Sie eine Zähleranalyse für die jeweilige Eigenschaft anzeigen möchten. Wenn Sie 'Eigenschaft' für die X-Achse auswählen, wird 'Gesamt' implizit für die Y-Achse ausgewählt. Wählen Sie beispielsweise *Eigenschaft* für die X-Achse und *Anwendungsname* für *Eigenschaft* aus, um einen Zähler für einen angegebenen Ereignistyp anzuzeigen, der nach App-Name analysiert wird.

Nachdem Sie einen Wert für die X-Achse definiert haben, können Sie einen Wert für die Y-Achse definieren. Wenn Sie *Zeitplan* für die X-Achse auswählen, können Sie die folgenden möglichen Werte für die Y-Achse auswählen.

* **Durchschnitt** - Durchschnittswerte einer numerischen Eigenschaft im angegebenen Ereignistyp.
* **Gesamt** - Die gesamte Anzahl einer Eigenschaft im angegebenen Ereignistyp.
* **Eindeutig** - Eine eindeutige Anzahl einer Eigenschaft im angegebenen Ereignistyp.
Nachdem Sie die Diagrammachsen definiert haben, müssen Sie einen Wert für die Eigenschaft auswählen.

### Kurvendiagramm
{:  #line_graph}

Das Kurvendiagramm ermöglicht die Visualisierung einiger Messgrößen über einen bestimmten Zeitraum hinweg. Dieser Typ von Diagramm ist nützlich, wenn Sie den Datentrend über einen bestimmten Zeitraum hinweg visualisieren möchten. Der erste Wert, der beim Erstellen eines Kurvendiagramms definiert wird, ist **Maß**; hierfür sind die folgenden Werte möglich.

* **Durchschnitt** - Durchschnittswerte einer numerischen Eigenschaft im angegebenen Ereignistyp.
* **Gesamt** - Die gesamte Anzahl einer Eigenschaft im angegebenen Ereignistyp.
* **Eindeutig** - Eine eindeutige Anzahl einer Eigenschaft im angegebenen Ereignistyp.
Nachdem Sie die Messwerte definiert haben, müssen Sie einen Wert für **Eigenschaft** auswählen.

### Ablaufdiagramm
{:  #flow_chart}

Das Ablaufdiagramm ermöglicht die Visualisierung einer Ablaufanalyse, bei der eine Eigenschaft in eine andere aufgegliedert wird. Für ein Ablaufdiagramm müssen die folgenden Eigenschaften festgelegt werden.

* **Quelle** - Der Wert eines Quellenknotens in dem Diagramm.
* **Ziel** - Der Wert des Zielknotens in dem Diagramm.
* **Eigenschaft** - Ein Eigenschaftswert des Quellenknotens oder des Zielknotens.
Mit dem Ablaufdiagramm können Sie die Dichteanalyse verschiedener Quellen zu einem Ziel hin oder umgekehrt sichtbar machen. Wenn Sie beispielsweise die Analyse von Protokollebenen für eine App anzeigen möchten, können Sie die folgenden Werte definieren.

* Wählen Sie *Anwendungsname* für **Quelle** aus.
* Wählen Sie *Protokollebene* für **Ziel** aus.
* Wählen Sie den Namen Ihrer App für **Eigenschaft** aus.

### Messgrößengruppe
{:  #metric_group}

Mithilfe der Messgrößengruppe können Sie eine einzelne Messgröße visualisieren, die entweder als ein Durchschnittswert, eine Gesamtanzahl oder als eine eindeutige Anzahl gemessen wird. Für die Definition einer Messgrößengruppe müssen Sie einen der folgenden möglichen Werte für **Maß** definieren.

* **Durchschnitt** - Durchschnittswerte einer numerischen Eigenschaft im angegebenen Ereignistyp.
* **Gesamt** - Die gesamte Anzahl einer Eigenschaft im angegebenen Ereignistyp.
* **Eindeutig** - Eine eindeutige Anzahl einer Eigenschaft im angegebenen Ereignistyp.
Nachdem Sie die Messwerte definiert haben, müssen Sie einen Wert für *Eigenschaft* auswählen. Diese Messgröße wird in der Messgrößengruppe angezeigt.

### Kreisdiagramm
{:  #pie_chart}

Mithilfe des Kreisdiagramms kann die Zähleranalyse von Werten für eine bestimmte Eigenschaft visualisiert werden. Wenn Sie beispielsweise eine Absturzanalyse anzeigen möchten, müssen Sie die folgenden Werte definieren.

* Wählen Sie *App-Sitzung* für **Ereignistyp** aus.
* Wählen Sie *Kreisdiagramm* für **Diagrammtyp** aus.
* Wählen Sie *Geschlossen durch* für **Eigenschaft** aus.
Das daraus resultierende Kreisdiagramm zeigt die Analyse zu App-Sitzungen, die vom Benutzer geschlossen wurden und nicht zu App-Sitzungen, die durch einen Absturz beendet wurden.

### Tabelle
{:  #table}

Die Tabelle ist nützlich, wenn die Rohdaten angezeigt werden sollen. Das Erstellen einer Tabelle ist so einfach wie das Hinzufügen von Spalten für die Rohdaten, die angezeigt werden sollen.
Da nicht alle Eigenschaften für bestimmte Ereignistypen erforderlich sind, ist die Anzeige von Nullwerten in Ihrer Tabelle möglich. Wenn diese Zeilen nicht in Ihrer Tabelle zu sehen sein sollen, müssen Sie für eine bestimmte Eigenschaft auf der Registerkarte **Diagrammfilter** einen Filter des Typs *Ist vorhanden* hinzufügen.
