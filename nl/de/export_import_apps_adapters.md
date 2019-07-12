---

copyright:
  years: 2018
lastupdated: "2019-06-06"

keywords: export apps, adapters export

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Anwendungen und Adapter exportieren und importieren
{: #export_import_apps_adapters}

Unter bestimmten Bedingungen können Sie eine Anwendung oder eine Version einer Anwendung vom {{site.data.keyword.mfp_oc_short_notm}} exportieren und anschließend auf einen anderen Server importieren. Sie können auch Adapter exportieren und reimportieren. Nutzen Sie diese Möglichkeit für Wiederverwendungs- und Sicherungszwecke oder zur Migration zwischen {{site.data.keyword.mfserver_short_notm}}-Instanzen.

Wenn Ihnen die Administratorrolle **mfpadmin** und die Deployer-Rolle **mfpdeployer** zugeordnet sind, können Sie eine Version oder alle Versionen einer Anwendung exportieren. Die Anwendung oder Version wird als komprimierte Datei `.zip` exportiert, in der die *Anwendungs-ID*, *Deskriptoren*, *Authentizitätsdaten* und *Webressourcen* gespeichert sind. Später können Sie das Archiv importieren, um die Anwendung oder Version auf demselben oder einem anderen Server erneut zu implementieren.

Überdenken Sie Ihren Anwendungsfall gründlich:
* Die Exportdatei enthält die Anwendungsauthentizitätsdaten. Diese Daten sind spezifisch für den Build einer mobilen App. Die mobile App enthält die URL des Servers. Wenn Sie also einen anderen Server verwenden möchten, müssen Sie einen neuen App-Build erstellen. Nur die Übertragung der exportierten App-Dateien reicht nicht aus.
* Bestimmte Artefakte können je nach Server verschieden sein. In einer Entwicklungsumgebung werden beispielsweise andere Push-Berechtigungsnachweise als in einer Produktionsumgebung verwendet.
* Die Anwendungslaufzeitkonfiguration (mit dem Aktivierungs-/Inaktivierungszustand und den Protokollprofilen) kann nicht in allen Fällen übertragen werden.
* Die Übertragung von Webressourcen ist in einigen Fällen nicht sinnvoll, z. B. wenn Sie einen neuen App-Build erstellen, weil Sie einen neuen Server verwenden möchten.
{: tip}

##  Voraussetzung
{: #prereq}

Starten Sie den {{site.data.keyword.mfp_oc_short_notm}}.

##  Vorgehensweise
{: #procedure}

1.  Wählen Sie in der Navigationsseitenleiste eine Anwendung, eine Anwendungsversion oder einen Adapter aus.

2.  Wählen Sie **Aktionen > Anwendung exportieren**, **Version exportieren** oder **Adapter exportieren** aus.
     Sie werden aufgefordert, das ZIP-Archiv (`.zip`) zu speichern, in dem die exportierten Ressourcen zusammengefasst sind. Das genaue Aussehen des Dialogfensters hängt von Ihrem Browser ab und der gewählte Zielordner von Ihren Browsereinstellungen.

3.   Speichern Sie die Archivdatei.
      Der Name der Archivdatei enthält den Namen und die Version der Anwendung oder des Adapters, z. B. `export_applications_com.sample.zip`.

4.   Wenn Sie ein vorhandenes Exportarchiv wiederverwenden möchten, wählen Sie ** Aktionen > Anwendung importieren ** oder **Version importieren** oder **Adapter importieren** aus, navigieren Sie zum Archiv und klicken Sie auf **Implementieren**.
      Im Hauptkonsolrahmen werden die Details der importierten Anwendung oder des importierten Adapters angezeigt.

##    Ergebnisse
{: #results}

Wenn Sie die Anwendung oder Version auf denselben Server importieren, wird sie nicht zwingend so wiederhergestellt, wie sie exportiert wurde. Änderungen, die nachträglich vorgenommen wurden, werden beim Import und bei der damit verbundenen erneuten Implementierung nicht entfernt. Werden zwischen dem Export und der erneuten Implementierung beim Import Anwendungsressourcen modifiziert, werden nur die im exportierten Archiv enthaltenen Dateien in ihrem ursprünglichen Zustand wiederhergestellt.
<br/>
Angenommen, Sie exportieren eine Anwendung ohne Authentizitätsdaten, laden danach die Authentizitätsdaten hoch und importieren dann das ursprüngliche Archiv. Die Authentizitätsdaten werden in dem Fall nicht gelöscht.
