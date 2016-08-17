<properties 
	pageTitle="Übersicht über Integrationskonten und das Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
	description="Erfahren Sie alles über Integrationskonten, das Enterprise Integration Pack und Logik-Apps" 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="logic-apps" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/08/2016" 
	ms.author="deonhe"/>

# Übersicht über Integrationskonten

## Was ist eine Integrationskonto?
Ein Integrationskonto ist ein Azure-Konto, mit dem Apps für die Unternehmensintegration Artefakte wie Schemas, Zuordnungen, Zertifikate, Partner und Vereinbarungen verwalten können. Von Ihnen erstellte Integrations-Apps benötigen ein Integrationskonto, um z.B. auf ein Schema, eine Zuordnung oder ein Zertifikat zuzugreifen.

## Erstellen eines Integrationskontos 
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)
3. Wählen im Menü oben auf der Seite die Schaltfläche *Hinzufügen* aus. ![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)
4. Geben Sie den **Namen** ein, und wählen Sie das **Abonnement** aus, das Sie verwenden möchten. Erstellen Sie entweder eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus. Wählen Sie einen **Speicherort** aus, in dem Ihr Integrationskonto gehostet wird. Wählen Sie einen **Tarif** und dann die Schaltfläche **Erstellen** aus.

  Das Integrationskonto wird dann am ausgewählten Speicherort bereitgestellt. Dieser Vorgang sollte nicht länger als 1 Minute dauern. ![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)
5. Aktualisieren Sie die Seite. Das neue Integrationskonto ist nun aufgeführt. Glückwunsch! ![](./media/app-service-logic-enterprise-integration-accounts/account-5.png)

## Verknüpfen eines Integrationskontos mit einer Logik-App
Damit Ihre Logik-Apps auf Zuordnungen, Schemas, Vereinbarungen und andere Artefakte zugreifen können, die in Ihrem Integrationskonto enthalten sind, müssen Sie zuerst das Integrationskonto mit Ihrer Logik-App verknüpfen.

### Es folgen die Schritte zum Verknüpfen eines Integrationskontos mit einer Logik-App. 

#### Voraussetzungen
- Ein Integrationskonto
- Eine Logik-App

>[AZURE.NOTE]Vergewissern Sie sich, dass sich Ihr Integrationskonto und Ihre Logik-App am **gleichen Azure-Speicherort** befinden, bevor Sie beginnen.

1. Wählen Sie im Menü Ihrer Logik-App den Link **Einstellungen** aus. ![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)
2. Wählen Sie auf dem Blatt „Einstellungen“ das Element **Integrationskonto** aus. ![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)
3. Wählen Sie das Integrationskonto aus, das Sie mit Ihrer Logik-App verknüpfen möchten, in der Dropdownliste **Integrationskonto wählen** aus. ![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)
4. Speichern Sie Ihre Arbeit. ![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)
5. Es wird die Benachrichtigung angezeigt, dass Ihr Integrationskonto mit Ihrer Logik-App verknüpft wurde und dass alle Artefakte in Ihrem Integrationskonto nun Ihrer Logik-App zur Verfügung stehen. ![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)

Nun da Ihr Integrationskonto mit Ihrer Logik-App verknüpft ist, können Sie zu Ihrer Logik-App wechseln und B2B-Connectors verwenden, wie z.B. die XML-Validierung, Codierung/Decodierung von Flatfiles oder Transformation, um Apps mit B2B-Features zu erstellen.
    
## Löschen eines Integrationskontos
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, das Sie löschen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4. Wählen Sie den Link **Löschen** im Menü aus. ![](./media/app-service-logic-enterprise-integration-accounts/delete.png)
5. Bestätigen Sie Ihre Auswahl.

## Verschieben eines Integrationskontos
Sie können ein Integrationskonto einfach in ein neues Abonnement und eine neue Ressourcengruppe verschieben. Gehen Sie folgendermaßen vor, wenn Sie Ihr Integrationskonto verschieben müssen:

>[AZURE.IMPORTANT] Nach dem Verschieben eines Integrationskontos müssen Sie alle Skripts für die Verwendung der neuen Ressourcen-IDs ändern.

1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, das Sie löschen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4. Wählen Sie den Link **Verschieben** im Menü aus. ![](./media/app-service-logic-enterprise-integration-accounts/move.png)
5. Bestätigen Sie Ihre Auswahl.

## Nächste Schritte
- [Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")
- [Weitere Informationen zu Vereinbarungen](./app-service-logic-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")


 

<!---HONumber=AcomDC_0803_2016-->