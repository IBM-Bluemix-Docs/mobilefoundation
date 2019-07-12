---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: disable apps, remote disabling of apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# App-Version über Fernzugriff inaktivieren
{: #remotely_disable_an_app_version}

In diesem Abschnitt wird beschrieben, wie der Benutzerzugriff auf eine bestimmte Version einer Anwendung auf einem bestimmten Betriebssystem für mobile Geräte inaktiviert und für den Benutzer eine entsprechende Nachricht bereitgestellt wird.

Sie können den App-Zugriff über die Mobile Foundation Operations Console verwalten.

1. Wählen Sie in der Navigationsseitenleiste der Konsole im Bereich **Anwendungen** Ihre Anwendungsversion und danach die Registerkarte **Anwendungsmanagement** aus.
2. Ändern Sie den Status in **Zugriff inaktiviert**.
3. Im Feld **URL der neuesten Version** können Sie eine URL für die neue Anwendungsversion angeben (die sich in der Regel im entsprechenden öffentlichen oder privaten App Store befindet).
   In einigen Umgebungen stellt das Application Center eine URL für den direkten Zugriff auf die Detailansicht einer Anwendungsversion bereit. Siehe [Anwendungseigenschaften](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties).
   {: tip}

4. Fügen Sie im Feld **Standardbenachrichtigung** die angepasste Benachrichtigung hinzu, die angezeigt werden soll, wenn der Benutzer versucht, auf die Anwendung zuzugreifen. Die folgende Beispielnachricht weist den Benutzer an, ein Upgrade auf die neueste Version durchzuführen:
   `Diese Version wird nicht mehr unterstützt. Führen Sie ein Upgrade auf die neueste Version durch.`
5. Im Bereich **Unterstützte Ländereinstellungen** können Sie die Benachrichtigung auch in anderen Sprachen angeben.
6. Wählen Sie **Speichern** aus, um Ihre Änderungen anzuwenden.

Wenn ein Benutzer eine über Fernzugriff inaktivierte Anwendung ausführen möchte, wird ein Dialogfenster mit Ihrer angepassten Nachricht angezeigt. Die Nachricht wird für jede Anwendungsinteraktion angezeigt, die Zugriff auf eine geschützte Ressource erfordert, sowie bei jedem Versuch der Anwendung, ein Zugriffstoken anzufordern. Wenn Sie eine URL für ein Versionsupgrade angegeben haben, gibt es in dem Dialogfenster zusätzlich zur Schaltfläche **Schließen** eine Schaltfläche **Neue Version abrufen**, um ein Upgrade auf eine neuere Version durchzuführen. <br/>
Schließt der Benutzer das Dialogfenster, ohne ein Versionsupgrade durchzuführen, kann er die nicht geschützten Anwendungsressourcen weiter verwenden. Anwendungsinteraktionen, die den Zugriff auf eine geschützte Ressource erfordern, bewirken allerdings, dass das Dialogfenster wieder angezeigt wird. Die Anwendung oder der Benutzer erhält keinen Zugriff auf die Ressource.
