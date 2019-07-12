---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# App-Versionen verwalten
{: #manage_app_versions}

Die Mobile Foundation-Funktionen für das Anwendungsmanagement ermöglichen Benutzern und Administratoren des Mobile Foundation-Servers eine detaillierte Steuerung des Benutzer- und Gerätezugriffs auf Anwendungen.

Der Mobile Foundation-Server verfolgt alle Versuche, auf Ihre mobile Infrastruktur zuzugreifen, und speichert Informationen zur Anwendung, zum Benutzer und zu dem Gerät, auf dem die Anwendung installiert ist. Die Zuordnung von Anwendung, Benutzer und Gerät bildet die Basis für die Serverfunktionen zur Verwaltung mobiler Anwendungen.

Über die Mobile Foundation Operations Console können Sie den Zugriff auf Ihre Ressourcen überwachen und verwalten. Außerdem können Sie Ihre spezielle Anwendungsversion verwalten.

1.  Navigieren Sie zu der Mobile Foundation Operations Console, klicken Sie auf **Anwendungen**, wählen Sie die Anwendung aus, die Sie verwalten möchten, und wählen Sie in der angezeigten Liste **Versionen** die gewünschte Version der Anwendung aus.
    ![Anwendungsversion verwalten](images/app_version_management.png)

2. Auf der Registerkarte **Management** werden die Optionen zum Festlegen des Anwendungsstatus der ausgewählten Anwendungsversion angezeigt. Die folgenden Statustypen werden für eine Anwendungsversion unterstützt:
   * Aktiv
   * Aktiv und mit Benachrichtigung
   * Zugriff inaktiviert
3. Eine Anwendungsversion kann inaktiviert werden, indem Sie unter **Anwendungszugriff > Status** die Option *Zugriff inaktiviert* auswählen.
4. Sie können im Bereich **Direct Update** auch konfigurieren, dass die aktualisierten Webressourcen Ihrer Cordova-Anwendung hochgeladen werden. Benutzern, die mit dieser speziellen Anwendungsversion eine Verbindung zum Mobile Foundation-Server herstellen, wird dann die Option zur Aktualisierung ihrer Anwendung mit 'Direct Update' (Direkte Aktualisierung) angezeigt.
5. Sie können mithilfe der folgenden Optionen im Menü **Aktionen** auch die folgenden Aktionen für die ausgewählte Anwendungsversion ausführen:
   *  Version löschen
   *  Version klonen
   *  Version exportieren


Weitere Informationen zum Verwalten von Geräten finden Sie im Abschnitt [Geräte verwalten](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices).
Weitere Informationen zum Inaktivieren einer App-Version über Fernzugriff finden Sie im Abschnitt [App-Version über Fernzugriff inaktivieren](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version).
{: note}
