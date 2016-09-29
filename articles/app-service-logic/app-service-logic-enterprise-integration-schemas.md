<properties 
	pageTitle="Übersicht über Schemas und das Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
	description="Erfahren Sie, wie Sie Schemas mit dem Enterprise Integration Pack und Logik-Apps verwenden" 
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
	ms.date="07/29/2016" 
	ms.author="deonhe"/>

# Informationen zu Schemas und zum Enterprise Integration Pack  

## Gründe für die Verwendung eines Schemas
Mithilfe von Schemas bestätigen Sie, dass von Ihnen empfangene XML-Dokumente gültig sind, was heißt, dass die Dokumente die erwarteten Daten in einem vordefinierten Format enthalten.

## Hinzufügen eines Schemas
Gehen Sie im Azure-Portal so vor:
1. Wählen Sie **Weitere Dienste**. ![](./media/app-service-logic-enterprise-integration-overview/overview-11.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-21.png)
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Schema hinzufügen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-31.png)
4.  Wählen Sie die Kachel **Schemas** aus. ![](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)
5. Wählen Sie auf dem geöffneten Blatt „Schema“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)
6. Geben Sie einen **Namen** für das Schema ein. Wählen Sie dann zum Hochladen der Schemadatei das Ordnersymbol rechts neben dem Textfeld **Schema** aus. Wählen Sie nach Abschluss des Hochladevorgangs die Schaltfläche **OK** aus. ![](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)
7. So fügen Sie eine Schemadatei hinzu, die größer als 2MB (max. 8MB) ist:
   * Laden Sie das Schema in den Speicher hoch, und kopieren Sie den URI. ![](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)
   * Wählen Sie in „Schema hinzufügen“ „Große Datei“, und geben Sie den URI in „Inhalts-URI“ an. ![](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png) 8. Sie sollten das neu hinzugefügte Schema sehen. ![](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)

## Verwenden von Schemas
- Schemas dienen zum Überprüfen von Nachrichten, die in einem B2B-Szenario ausgetauscht werden.

## Bearbeiten von Schemas
1. Wählen Sie die Kachel **Schemas** aus.
2. Wählen Sie auf dem geöffneten Blatt „Schemas“ das Schema aus, das Sie bearbeiten möchten.
3. Wählen Sie auf dem Blatt „Schemas“ den Link **Hochladen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)
4. Wählen Sie im eingeblendeten Dialogfeld zur Dateiauswahl die Schemadatei aus, die Sie hochladen möchten.
5. Wählen Sie in der Dateiauswahl **Öffnen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)
6. Sie erhalten eine Benachrichtigung, die angibt, dass das Hochladen erfolgreich war.

## Löschen von Schemas
1. Wählen Sie die Kachel **Schemas** aus.
2. Wählen Sie auf dem geöffneten Blatt „Schemas“ das Schema aus, das Sie löschen möchten.
3. Wählen Sie auf dem Blatt „Schemas“ auf der Menüleiste den Link **Löschen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)
4. Wenn Sie das ausgewählte Schema wirklich löschen möchten, wählen Sie im Dialogfeld „Schema löschen“ **Ja** aus, um Ihre Auswahl zu bestätigen. ![](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)
5. Auf der aktualisierten Liste auf dem Blatt „Schemas“ wird das gelöschte Schema nicht mehr angezeigt. ![](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)

## Nächste Schritte

- [Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0914_2016-->