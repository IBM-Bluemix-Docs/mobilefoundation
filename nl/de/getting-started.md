---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: getting started, mobile foundation, plans, configure mobile foundation server, sample app, setup

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Einführung - Lernprogramm
{: #getting-started}

{{site.data.keyword.mobilefoundation_long}} beschleunigt die Einrichtung einer {{site.data.keyword.mfp_full}}-Umgebung, mit der Sie mobile Unternehmens-Apps entwickeln, testen und ausführen können. {{site.data.keyword.mobilefoundation_short}} bietet die folgenden unterschiedlichen Servicepläne:
* **Developer**: Stellt eine Instanz von Foundation Server im Konto des Benutzers bereit. Lässt eine beliebige Anzahl von Anwendungen zu, wobei die Gesamtanzahl der verbundenen Geräte über alle Anwendungen hinweg auf 10 begrenzt ist. Dieser Plan ist kostenlos und nur für Entwicklungs- und Testzwecke zu verwenden.
* **Professional Per Device**: Stellt eine Instanz von Foundation Server im Konto des Benutzers bereit und wird anhand der aktiv verbundenen Geräte berechnet.
* **Professional 1 Application**: Stellt eine Instanz von Foundation Server im Konto des Benutzers bereit und ermöglicht es, dass eine beliebige Anzahl von Benutzern und Geräten nur für eine einzige Anwendung aktiv verbunden werden kann.    
{: shortdesc}

Alle verfügbaren Pläne sind [hier](https://cloud.ibm.com/catalog/services/mobile-foundation) aufgeführt.
{: note}

Erstellen Sie eine {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz, die einen der unterstützten Pläne verwendet, indem Sie diesem einführenden Lernprogramm folgen. Danach können Sie eine Anwendung registrieren. Laden Sie die registrierte Anwendung herunter, bearbeiten Sie sie, implementieren Sie einen Adapter und testen Sie abschließend die Anwendung.

## Vorbereitungen
{: #prereqs-gs}

Sie benötigen ein {{site.data.keyword.Bluemix}}-Konto und eine {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz.

## Schritt 1: {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz erstellen
{: #step1create}

1. Wählen Sie im {{site.data.keyword.Bluemix_notm}}-**Katalog** [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation) aus. Die Servicekonfigurationsanzeige wird geöffnet.
2. Legen Sie einen Namen für die Serviceinstanz fest oder verwenden Sie den voreingestellten Namen.
3. Wählen Sie die Region, die Organisation und den Bereich aus, in denen die Serviceinstanz erstellt werden soll.
4. Wählen Sie Ihren **Preisstrukturplan** aus und klicken Sie auf **Erstellen**.

## Schritt 2: Mobilen Kanal erstellen
{: #buildmobilechannel}


### Für {{site.data.keyword.mobilefoundation_short}}: Developer-Plan
{: #buildchanneldevplan}

Nachdem Sie eine Instanz von {{site.data.keyword.mobilefoundation_short}}: Developer erstellt haben, können Sie mit der Erstellung Ihres mobilen Kanals beginnen, indem Sie die folgenden Schritte ausführen.

* Sie können sofort auf den Mobile Foundation-Server zugreifen und diesen verwenden.

  Diese Auswahl erstellt einen {{site.data.keyword.mfserver_long_notm}} mit den folgenden Einstellungen:
  *	1 GB Hauptspeicher. Diese Größe ist für die Entwicklung und kleinere Tests ausreichend.

  * Für den Zugriff auf den Mobile Foundation-Server über die Befehlszeilenschnittstelle (CLI) benötigen Sie die Berechtigungsnachweise, die zur Verfügung stehen, wenn Sie auf **Serviceberechtigungsnachweise** im Navigationsbereich der IBM Cloud-Konsole klicken.

### Für {{site.data.keyword.mobilefoundation_short}}: Professional Per Device-Plan
{: #buildchannelprofdeviceplan}

Nachdem Sie eine Instanz von {{site.data.keyword.mobilefoundation_short}}: Professional Per Device-Service erstellt haben, können Sie mit der Erstellung Ihres mobilen Kanals beginnen, indem Sie die folgenden Schritte ausführen.

  1.  Stellen Sie auf dem {{site.data.keyword.Bluemix_notm}} eine Verbindung zu einer vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz her.

      1.  Wählen Sie die {{site.data.keyword.Bluemix_notm}}-`Organisation` aus, in der die {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz vorhanden ist.

      + Wählen Sie den {{site.data.keyword.Bluemix_notm}}-`Bereich`, in dem sich die {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz befindet, in der Liste der Bereiche aus, die in der ausgewählten `Organisation` verfügbar sind.

      + Wählen Sie den `Servicenamen` und die `Berechtigungsnachweise` für {{site.data.keyword.Db2_on_Cloud_short}} (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}} aus, um eine Verbindung zur vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz herzustellen.

      + Testen Sie die Verbindung zu der ausgewählten {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz, indem Sie auf **Testverbindung** klicken.

      + Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter** in dem Popup-Fenster, in dem Sie zur Bestätigung des ausgewählten {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Service aufgefordert werden. Mit dieser Aktion werden die erforderlichen Tabellen in der konfigurierten {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Datenbankserviceinstanz erstellt.

      Nachdem Sie eine {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Verbindung zu der {{site.data.keyword.mobilefoundation_short}}-Instanz hinzugefügt haben, können Sie daran keine  Änderungen mehr vornehmen.
      {: note}
  2.  Erstellen Sie den Server und starten Sie ihn.

      1. Erstellen Sie eine {{site.data.keyword.mobilefirst_notm}}-Serverinstanz mit der Standardkonfiguration, indem Sie auf **Basisserver starten** klicken.

      + Diese Auswahl stellt einen {{site.data.keyword.mfserver_long_notm}} mit den folgenden Einstellungen bereit:
          - Zwei Knoten mit jeweils 1 GB Hauptspeicher. Diese Größe ist für Entwicklungs- und kleinere Testaktivitäten sowie für kleinere Produktionsworkloads ausreichend.

          -	`Benutzername` und `Kennwort` werden automatisch für Sie generiert. Sie können darauf zugreifen, wenn der Server betriebsbereit ist.

          Der Prozess der Erstellung Ihres Servers wird gestartet. Dieser Prozess dauert ungefähr 10 Minuten; in einem Nachrichtenfenster wird der Fortschritt dieser Operation angezeigt. Ist der Vorgang abgeschlossen, wird ein Dashboard mit folgenden Informationen angezeigt:
            -	Status Ihres Servers, der ausgeführt wird (Zustand, Größe).
            -	Für Sie erstellte Serverroute. Verwenden Sie diese Route in Ihrer mobilen Anwendung, um eine Verbindung zum {{site.data.keyword.mfserver_short_notm}} herzustellen.
            -	Ihr persönlicher `Benutzername` und das `Kennwort` für den Zugriff auf die {{site.data.keyword.mfp_oc_short_notm}}. Das `Kennwort` wird ausgeblendet. Klicken Sie auf das Symbol **Kennwort anzeigen**, um es einzublenden.

      +	Klicken Sie auf **Konsole starten**, um die {{site.data.keyword.mfp_oc_short_notm}} zu öffnen.      

      Zur Erstellung einer {{site.data.keyword.mobilefirst_notm}}-Serverinstanz mit erweiterter Konfiguration für Topologie, Sicherheit und weitere Serverkonfigurationen klicken Sie auf **Server mit erweiterter Konfiguration starten**. Weitere Informationen hierzu finden Sie im Abschnitt [Erweiterte Konfiguration einrichten](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#using_mfs_advanced_p5).
      {: tip}

### Für {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application-Plan
{: #buildchannelprof1appplan}

Nachdem Sie eine Instanz von {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application-Service erstellt haben, können Sie mit der Erstellung Ihres mobilen Kanals beginnen, indem Sie die folgenden Schritte ausführen.

  1.  Stellen Sie auf dem {{site.data.keyword.Bluemix_notm}} eine Verbindung zu einer vorhandenen {{site.data.keyword.Db2_on_Cloud_long}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL_full}}-Serviceinstanz her.

      1.  Wählen Sie die {{site.data.keyword.Bluemix_notm}}-`Organisation` aus, in der die {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz vorhanden ist.

      + Wählen Sie den {{site.data.keyword.Bluemix_notm}}-`Bereich`, in dem sich die {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz befindet, in der Liste der Bereiche aus, die in der ausgewählten `Organisation` verfügbar sind.

      + Wählen Sie den `Servicenamen` und die `Berechtigungsnachweise` für {{site.data.keyword.Db2_on_Cloud_short}} (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}} aus, um eine Verbindung zur vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz herzustellen.

      + Testen Sie die Verbindung zu der ausgewählten {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Serviceinstanz, indem Sie auf **Testverbindung** klicken.

      + Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter** in dem Popup-Fenster, in dem Sie zur Bestätigung des ausgewählten {{site.data.keyword.Db2_on_Cloud_short}}- oder {{site.data.keyword.composeForPostgreSQL}}-Service aufgefordert werden. Mit dieser Aktion werden die erforderlichen Tabellen in der konfigurierten {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Datenbankserviceinstanz erstellt.

      Nachdem Sie eine {{site.data.keyword.Db2_on_Cloud_short}}- (jeder andere als der **Lite**-Plan) oder {{site.data.keyword.composeForPostgreSQL}}-Verbindung zu der {{site.data.keyword.mobilefoundation_short}}-Instanz hinzugefügt haben, können Sie daran keine  Änderungen mehr vornehmen.
      {: note}

  2.  Erstellen Sie den Server und starten Sie ihn.

      1. Erstellen Sie eine {{site.data.keyword.mobilefirst_notm}}-Serverinstanz mit der Standardkonfiguration, indem Sie auf **Basisserver starten** klicken.

        `Die Basisserverinstanz umfasst einen zentralen Knoten und 1 GB Speicherkapazität.`

      + `Benutzername` und `Kennwort` werden automatisch für Sie generiert. Sie können darauf zugreifen, wenn der Server betriebsbereit ist.  

        Der Prozess der Erstellung Ihres Servers wird gestartet. Dieser Prozess dauert ungefähr 10 Minuten; in einem Nachrichtenfenster wird der Fortschritt dieser Operation angezeigt. Ist der Vorgang abgeschlossen, wird ein Dashboard mit folgenden Informationen angezeigt:
          -	Status Ihres Servers, der ausgeführt wird (Zustand, Größe).
          -	Für Sie erstellte Serverroute. Verwenden Sie diese Route in Ihrer mobilen Anwendung, um eine Verbindung zum {{site.data.keyword.mfserver_short_notm}} herzustellen.
          -	Ihr persönlicher `Benutzername` und das `Kennwort` für den Zugriff auf die {{site.data.keyword.mfp_oc_short_notm}}. Das `Kennwort` wird ausgeblendet. Klicken Sie auf das Symbol **Kennwort anzeigen**, um es einzublenden.

      +  Klicken Sie auf **Konsole starten**, um die {{site.data.keyword.mfp_oc_short_notm}} zu öffnen.  

      Zur Erstellung einer {{site.data.keyword.mobilefirst_notm}}-Serverinstanz mit erweiterter Konfiguration für Topologie, Sicherheit und weitere Serverkonfigurationen klicken Sie auf **Server mit erweiterter Konfiguration starten**. Weitere Informationen hierzu finden Sie im Abschnitt [Erweiterte Konfiguration einrichten](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#using_mfs_advanced_p2).
      {: tip}

Weitere Informationen zur Einführung in {{site.data.keyword.mobilefoundation_short}} finden Sie in [MobileFirst-Server mithilfe des Mobile Foundation-Service einrichten![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/){: new_window}.
{: note}

## Schritt 3: Anwendung in {{site.data.keyword.mobilefoundation_short}} registrieren
{: #registerapp}

Nachdem Sie Ihre Mobile Foundation-Serverinstanz erstellt und gestartet haben, können Sie die folgenden Schritte ausführen, um eine Android-Anwendung zu registrieren.

  1.  Rufen Sie {{site.data.keyword.mfp_oc_short_notm}} auf, indem Sie folgende URL laden: `http://<your-server-host>:<server-port>/mfpconsole`. Verwenden Sie den `Benutzernamen` und das `Kennwort`, die bei der Bereitstellung generiert wurden.

  + Klicken Sie im {{site.data.keyword.mfp_oc_short_notm}}-**Dashboard** auf **Neu** neben **Anwendungen**.

  + Geben Sie *MFPStarterAndroid* als **Anwendungsnamen** an.

  + Wählen Sie unter **Plattform auswählen** die Option *Android* aus.

  + Geben Sie *com.ibm.mfpstarterandroid* als **Anwendungs-ID** an.

  + Geben Sie *1.0* als **Version** ein.

  + Klicken Sie auf **Anwendung registrieren**.

## Schritt 4: Musteranwendung herunterladen
{: #downloadapp}

  1.  Wählen Sie im {{site.data.keyword.mfp_oc_short_notm}}-**Dashboard** die Option **MFPStarterAndroid** unter **Anwendungen** aus.

  + Klicken Sie auf **Startercode abrufen** und wählen Sie das Android-Anwendungsbeispiel für den Download aus.

## Schritt 5: Beispielanwendung bearbeiten
{: #editapp}

  1. Importieren Sie die im vorherigen Schritt heruntergeladene Android-Beispiel-App in Android Studio.

  + Wählen Sie im Seitenleistenmenü **Projekt** in Android Studio die Datei **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java** aus.

    * Fügen Sie die folgenden Importe hinzu
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * Fügen Sie das folgende Code-Snippet ein und ersetzen Sie damit den Aufruf für `WLAuthorizationManager.getInstance().obtainAccessToken`.

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Druckt "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## Schritt 6: Adapter bereitstellen
{: #deployadapter}

  1. Laden Sie dieses [Adapterartefakt](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} herunter und stellen Sie es über **Aktionen → Adapter bereitstellen** in {{site.data.keyword.mfp_oc_short_notm}} bereit.

## Schritt 7: Anwendung testen
{: #testapp}

  1. Wählen Sie im Seitenleistenmenü **Projekt** in Android Studio die Datei **app → src → main →assets → mfpclient.properties** aus und bearbeiten Sie die Eigenschaften `protocol`, `host` und `port`, indem Sie die korrekten Werte für Ihren MobileFirst-Server angeben.

   Die Werte lauten normalerweise 'https', *Ihre Serveradresse* und '443'.
   {: tip}

  2. Klicken Sie auf **App ausführen** in Android Studio.
     * Die App wird in einem Einheitenemulator gestartet.
     * Klicken Sie in der Anwendung auf **Pingsignal an MobileFirst-Server absetzen**. `Mit MobileFirst-Server verbunden` wird angezeigt.
     * Wenn die Anwendung eine Verbindung zum MobileFirst-Server herstellen konnte, setzt der bereitgestellte Java-Adapter einen Ressourcenanforderungsaufruf ab.
     * Die Adapterantwort wird dann in der Android Studio-LogCat-Ansicht angezeigt.


## Weitere Schritte
{: #nextsteps-gs}

Sie können die [Lernprogramme zum Schnelleinstieg ![Symbol für externen Link](../../icons/launch-glyph.svg "Lernprogramme zum Schnelleinstieg")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} ausführen, um mit weiteren Beispielanwendungen zu arbeiten und die Funktionsweise von {{site.data.keyword.mobilefoundation_short}} zu erkunden.

In den Lernprogrammen für den Schnelleinstieg wird die Funktionsweise von {{site.data.keyword.mobilefoundation_short}} für iOS-, Android-, Web-, Cordova-, Windows-, React Native-, Ionic- und Xamarin-Apps erläutert.
