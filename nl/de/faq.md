---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:note: .note}

# Häufig gestellte Fragen
{: #mfp-faq}

Im Folgenden sind Antworten auf häufig gestellte Fragen im Zusammenhang mit dem {{site.data.keyword.mobilefoundation_long}}-Service aufgeführt.
{: shortdesc}

## Wie werde ich informiert, wenn Aktualisierungen für den {{site.data.keyword.mobilefoundation_short}}-Service verfügbar sind?
{: #maintupdates_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} erstellt einen {{site.data.keyword.mfserver_short_notm}}. Die Benutzer werden über Aktualisierungen des {{site.data.keyword.mobilefoundation_short}}-Servers benachrichtigt. Eine Benachrichtigung wird im Dashboard der Serviceinstanz angezeigt. Der Benutzer kann die Aktualisierung für {{site.data.keyword.mobilefoundation_short}} in einem von ihm festgelegten Wartungsfenster ausführen. Wenn es für Sie hilfreich ist, können Sie den {{site.data.keyword.mobilefoundation_short}}-Server aktualisieren.

Die {{site.data.keyword.mobilefoundation_short}}-Serviceaktualisierung steht zur Verfügung, wenn eine der folgenden Komponenten aktualisiert ist.

* {{site.data.keyword.mfserver_short_notm}}.
* Zugrunde liegende Liberty-Version.
* Zugrunde liegende Java Developer Kit-Version.

## Wie wende ich die Aktualisierungen für den {{site.data.keyword.mobilefoundation_short}}-Service an?
{: #apply_update_mf}
{: faq}

Die Aktualisierung für {{site.data.keyword.mobilefoundation_short}} kann durch Klicken auf die Option für die Neuerstellung angewendet werden.
Bei der Anwendung der Aktualisierung wird die Version des Servers, die in der {{site.data.keyword.mfp_oc_short_notm}} zu sehen ist, so geändert, dass die Version der Serveraktualisierung angezeigt wird.

* Benutzer können keine eigenen Fixes und Aktualisierungen auf ihre Instanz des {{site.data.keyword.mobilefoundation_short}}-Service anwenden.
* Lesen Sie [Server in Professional Per Device-Plan neu erstellen](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5) und [Server in Professional 1 Application-Plan neu erstellen](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2), um den Unterschied im Verhalten zwischen den Plänen zu verstehen, wenn auf **Neu erstellen** geklickt wird.{: note}

## Wie konfiguriere ich eine angepasste Domäne für meine {{site.data.keyword.mobilefoundation_short}}-Serverinstanz?
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} stellt einen {{site.data.keyword.mfserver_short_notm}} bereit, auf den über eine URL zugegriffen werden kann, bei der die Domänennamen auf der {{site.data.keyword.Bluemix_notm}}-**Region** basieren. Sie können auch eine eigene benutzerdefinierte Domäne erstellen.

Bei der Erstellung der URL oder Route basieren die Standarddomänennamen auf der {{site.data.keyword.Bluemix_notm}}-`Region`.

  |Domäne |  Region  |    
  |:----- | :----- |    
  |`mybluemix.net` | US - Süden |    
  |`eu-gb.mybluemix.net` | Vereinigtes Königreich  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Tabelle 1. Anwendungsdomänennamen auf der Basis von Regionen in {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Zur Verwendung einer eigenen Domäne müssen Sie eine benutzerdefinierte Domäne konfigurieren, indem Sie die folgenden Schritte ausführen:

1.	Erstellen Sie eine {{site.data.keyword.mfserver_short_notm}}-Instanz, indem Sie die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz durch die Auswahl eines unterstützten Plans erstellen.

+ Fügen Sie die benutzerdefinierte Domäne, die Sie verwenden möchten, zu Ihrer {{site.data.keyword.Bluemix_notm}}-`Organisation` hinzu. Rufen Sie **Organisationen verwalten > Domänen > Domäne hinzufügen** auf, um eine eigene Domäne hinzuzufügen.

+ Legen Sie eine Route für die Verwendung der benutzerdefinierten Domäne durch den Server fest.

+ Rufen Sie den DNS-Provider für Ihre Domäne auf und fügen Sie einen CNAME-Eintrag hinzu, der den Datenverkehr von Ihrer Domäne zur {{site.data.keyword.Bluemix_notm}}-Standardroute weiterleitet, unter der der Server ausgeführt wird.

+ Wenn Sie für Ihre benutzerdefinierte Domäne `https` konfigurieren möchten, laden Sie das SSL-Zertifikat für Ihre Domäne in {{site.data.keyword.Bluemix_notm}} hoch. Wenn Sie das SSL-Zertifikat hochladen möchten, wechseln Sie zu **Organisationen verwalten > Domänen**. Wählen Sie die benutzerdefinierte Domäne aus, für die Sie das SSL-Zertifikat konfigurieren möchten, und klicken Sie dann auf **Zertifikat hochladen**, um das SSL-Zertifikat für Ihre Domäne hochzuladen. Weitere Informationen finden Sie in [diesem Beitrag ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}.
