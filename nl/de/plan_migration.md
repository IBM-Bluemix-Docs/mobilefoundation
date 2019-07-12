---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Mobile Foundation-Serviceplan migrieren

Mobile Foundation-Instanzen, die mit veralteten Plänen erstellt wurden, müssen mit den neuen Plänen aktualisiert werden. Je nach der Nutzung einer Instanz müssen möglicherweise auch die Pläne aktualisiert werden.
{: shortdesc}

## Beispielszenario: Vom Professional Per Device-Plan auf den Professional 1 Application-Plan migrieren

1. Wählen Sie im IBM Cloud-Dashboard die IBM Mobile Foundation-Instanz aus, die Sie migrieren möchten.
2. Wählen Sie im Navigationsbereich die Option **Plan** aus.
   ![Vorhandener Mobile Foundation-Plan](images/existing-plan.png)
3. Wählen Sie in den aufgeführten Preisstrukturplan die Option "Professional 1 Application" aus.
   ![Neuer Mobile Foundation-Plan](images/new-plan.png)
4. Klicken Sie auf die Schaltfläche **Speichern** und bestätigen Sie die Planmigration.
     Die Migration auf Professional 1 Application ist jetzt abgeschlossen und alle vorhandenen Daten wurden beibehalten. Die Abrechnung wurde geändert und es gibt keine Ausfallzeit.
5. Nach der Migration des Plans muss die Mobile Foundation-Instanz vom Service-Dashboard neu erstellt werden, damit die richtige Konfiguration wirksam wird. Für diese Aktualisierung ist eine kurze Ausfallzeit erforderlich. Sie müssen diese Ausfallzeit planen. Wählen Sie im Navigationsbereich die Option **Verwalten** aus und klicken Sie auf **Neu erstellen**.

Wenn Sie sich in einem veralteten Plan befinden, müssen Sie eine Migration auf einen neuen Plan durchführen.
{: note}

## Unterstützte Planmigrationen

* Der *Developer*-Plan (veraltet) kann nur auf den neuen *Developer*-Plan migriert werden.
* Der *Developer Pro *-Plan (veraltet) kann nur auf den *Professional Per Device*- oder *Professional 1 Application*-Plan migriert werden.
* Der *Professional Per Capacity*-Plan (veraltet) kann nur auf den *Professional Per Device*- oder *Professional 1 Application*-Plan migriert werden.
* Der *Professional Per Device*-Plan (veraltet) kann nur auf den *Professional 1 Application*-Plan migriert werden.
* Der *Professional 1 Application*-Plan (veraltet) kann nur auf den *Professional Per Device*-Plan migriert werden.
* Für den neuen *Developer*-Plan wird die Planaktualisierung nicht unterstützt.
