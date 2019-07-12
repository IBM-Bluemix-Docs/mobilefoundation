---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, set up alerts, alert definitions

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Alerts einrichten
{: #set_up_alerts}

Im Lieferumfang von Mobile Analytics ist das Feature 'Alerts' enthalten; dieses ermöglicht die Definition von Alerts für die Überwachung von Anwendungen. Sie können Schwellenwerte so konfigurieren, dass bei ihrem Überschreiten Alerts ausgelöst werden, über die die Mobile Analytics-Konsolenüberwachung benachrichtigt wird. Die ausgelösten Alerts können in der Konsole visualisiert oder sie können so konfiguriert werden, dass sie für eine entsprechende Verarbeitung an einen angepassten Webhook weitergeleitet werden.

Dieses Feature ist eine Möglichkeit, um Schwellenwertüberschreitungen zu ermitteln und darüber Alerts auszugeben, die mittels Anwendungsfehlerprotokollen, Anwendungsabstürzen und Netztransaktionen festgestellt werden. Durch reaktive Schwellenwerte und Alerts werden die Antworten auf Anwendungsleistungsbedingungen flexibel gestaltet ohne dass die entsprechenden Ansichten/Diagramme ununterbrochen überwacht werden müssen.

Dieses Feature gibt es in der Betaversion.
{: note}

In den folgenden Abschnitten wird die Erstellung, Verwaltung von Alerts und deren Anzeige in der Mobile Analytics-Konsole beschrieben.

## Alertdefinition für Anwendungsprotokolle erstellen (App-Protokolle)
{: #creating_alert_def}

Sie können eine Alertdefinition erstellen, die auf Anwendungsprotokollen basiert.  Wenn Sie z. B. Ihre Anwendungsprotokolle alle 5 Minuten überwachen möchten, um zu überprüfen, ob Ihre Anwendung einer bestimmten Version mehr als drei Mal Fehler protokolliert hat, können Sie hier die Vorgehensweise in diesem Fall festlegen.

1.  Klicken Sie in der Mobile Analytics-Konsole auf **Definitionen**, um zur Seite mit den Alertdefinitionen zu wechseln.
2.  Klicken Sie auf **Alert erstellen**.
3.  Geben Sie die folgenden Werte an:
    * **Alertname**: *Alert für App-Protokolle*
    * **Nachricht**: *Alert für Fehlernachricht*
    * **Abfragehäufigkeit**: *5 Minuten*
    * **Ereignistyp**: *App-Protokolle*
        * **Eigenschaft**: *Protokollebene*
            * **Wert**: *Fehler*
            * **Schwellenwert**: *Summe für Anwendungsinstanz*<br/>
              Wenn Sie die Option *Durchschnitt für Anwendung* ausgewählt haben, wird aus den App-Protokollen und der Anzahl der Geräte ein Durchschnittswert berechnet. Beispiel: Wenn Sie über zwei Geräte verfügen und von einem Gerät sechs App-Protokolle gesendet werden, vom anderen Gerät dagegen nur drei Protokolle, beträgt der Durchschnitt 4,5 Protokolle.
              {: note}
            * **Operator**: *Größer gleich*
            * Schwellenwert: *3*
4.  Klicken Sie auf **Weiter** und geben Sie den folgenden Wert an:
    * **Methode**: *Nur Analytics Console*<br/>
      Wählen Sie außerdem die Option für *Analytics Console und Network Post* aus, wenn Sie zusätzlich eine POST-Nachricht mit einer JSON-Nutzlast an Ihre angepasste URL senden wollen. Wenn Sie diese Option auswählen, sind die folgenden Felder verfügbar.
      * **Network Post-URL**
      * **Header**
      * **Authentifizierungstyp**
      * **Anforderungshauptteil POST**
5. Klicken Sie auf **Speichern**.  

Sie haben nun eine Alertdefinition für einen Alert erstellt, der nach Ablauf eines Intervalls von fünf Minuten ausgelöst wird, wenn die Anzahl der App-Protokolle den Schwellenwert von 3 Fehlerprotokollen erreicht oder überschreitet.

Dieser Alert bleibt so lange aktiv und führt die Überwachung mit der eingerichteten Häufigkeit durch, bis die Alertdefinition inaktiviert oder gelöscht wird.
{: note}

## Alertdefinition für Anwendungsabstürze erstellen
{: #creating_alert_crashes}

Im Folgenden finden Sie ein Beispiel für das Einrichten von Alerts für Anwendungsabstürze.  Dabei findet die Überwachung alle zwei Minuten statt und es werden alle Anwendungsabstürze festgestellt; anschließend wird ein Alert ausgelöst, wenn die Anzahl der festgestellten Abstürze über 5 liegt.

1.  Klicken Sie in der Mobile Analytics-Konsole auf **Definitionen**, um zur Seite mit den Alertdefinitionen zu wechseln.
2.  Klicken Sie auf **Alert erstellen**.
3.  Geben Sie die folgenden Werte an:
    * **Alertname**: *Alert für Anwendungsabstürze*
    * **Nachricht**: *Alert für App-Absturz*
    * **Abfragehäufigkeit**: *2 Minuten*
    * **Ereignistyp**: *Anwendungsabstürze*
        * **Anwendungsname**: *Beliebige Anwendung*
        * **Beliebige Anwendung**: *Beliebige Version*
    * **Schwellenwert**: *Absturzanzahl*
    * **Operator**: *Größer gleich*
    * Schwellenwert: *5*
4.  Klicken Sie auf **Verteilungsmethode** oder auf **Weiter** und geben Sie den folgenden Wert an:
    * **Methode**: *Nur Analytics Console*<br/>
      Wählen Sie außerdem die Option für *Analytics Console und Network Post* aus, wenn Sie zusätzlich eine POST-Nachricht mit einer JSON-Nutzlast an Ihre angepasste URL senden wollen. Wenn Sie diese Option auswählen, sind die folgenden Felder verfügbar.
      * **Network Post-URL**
      * **Header**
      * **Authentifizierungstyp**
      * **Anforderungshauptteil POST**
5. Klicken Sie auf **Speichern**.  

Dieser Alert bleibt so lange aktiv und führt die Überwachung mit der eingerichteten Häufigkeit durch, bis die Alertdefinition inaktiviert oder gelöscht wird.
{: note}

## Alertdefinitionen verwalten
{: #managing_alert_def}

Sie können Ihre Alertdefinitionen über die Seite **Definitionen** des Alerts verwalten.

Für jede Alertdefinition können Sie die folgenden Operationen ausführen:
* Wechseln Sie das Kontrollkästchen unter der Spalte **Aktiviert**, um eine bestimmte Alertdefinition zu aktivieren oder zu inaktivieren.
* Wenn Sie eine Kopie einer Alertdefinition erstellen und einige Werte ändern möchten, klicken Sie auf das Symbol zum Duplizieren.
* Klicken Sie auf das Stiftsymbol, wenn Sie eine Alertdefinition bearbeiten möchten.
* Klicken Sie auf das Papierkorbsymbol, wenn Sie eine Alertdefinition löschen möchten.

## Alerts anzeigen
{: #viewing_alerts}

Sie können die ausgelösten Alerts über die Seite **Protokolle** für die Alerts anzeigen.

1.  Klicken Sie in der Mobile Analytics-Konsole auf **Protokolle**.
2.  Klicken Sie für eine beliebige Anzahl von Alerts auf das Plussymbol (**+**). Durch diese Aktion werden die Alertdefinition und der Abschnitt **Alertinstanzen** angezeigt.
    Wurde die entsprechende Alertdefinition nicht gelöscht oder modifiziert, können Sie die Alertdefinition durch Klicken auf **Alert bearbeiten** bearbeiten. Sonst steht die Schaltfläche **Alert bearbeiten** nicht zur Verfügung und die folgende Nachricht wird angezeigt:
    `This alert definition has since been modified or deleted.`
3.  Sie können optional einen Alert auswählen und auf das Papierkorbsymbol klicken, um den Alert zu löschen.
