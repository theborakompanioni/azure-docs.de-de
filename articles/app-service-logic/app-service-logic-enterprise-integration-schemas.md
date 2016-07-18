<properties 
	pageTitle="Übersicht über Schemas und das Enterprise Integration Pack | Microsoft Azure App Service" 
	description="Erfahren Sie, wie Sie Schemas mit dem Enterprise Integration Pack und Logik-Apps verwenden" 
	services="app-service\logic" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="app-service-logic" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/29/2016" 
	ms.author="deonhe"/>

# Informationen zu Schemas und zum Enterprise Integration Pack  

## Gründe für die Verwendung eines Schemas
Mithilfe von Schemas bestätigen Sie, dass von Ihnen empfangene XML-Dokumente gültig sind, was heißt, dass die Dokumente die erwarteten Daten in einem vordefinierten Format enthalten.

## Hinzufügen eines Schemas
Gehen Sie im Azure-Portal so vor:
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Schema hinzufügen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  Wählen Sie die Kachel **Schemas** aus. ![](./media/app-service-logic-enterprise-integration-schemas/schema-1.png)
5. Wählen Sie auf dem geöffneten Blatt „Schema“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/schema-2.png)
6. Geben Sie einen **Namen** für das Schema ein. Wählen Sie dann zum Hochladen der Schemadatei das Ordnersymbol rechts neben dem Textfeld **Schema** aus. Wählen Sie nach Abschluss des Hochladevorgangs die Schaltfläche **OK** aus. ![](./media/app-service-logic-enterprise-integration-schemas/schema-3.png)
7. Wählen Sie das Benachrichtigungssymbol *Glocke* aus, um den Status des Schemahochladevorgangs anzuzeigen. ![](./media/app-service-logic-enterprise-integration-schemas/schema-4.png)
8. Wählen Sie die Kachel **Schemas** aus. Dadurch wird die Kachel aktualisiert. An der Erhöhung der Anzahl der Schemas erkennen Sie, dass das neue Schema erfolgreich hinzugefügt wurde. Nachdem Sie die Kachel **Schemas** ausgewählt haben, wird das neu hinzugefügte Schema rechts auf dem Blatt „Schemas“ angezeigt. ![](./media/app-service-logic-enterprise-integration-schemas/schema-5.png)


## Verwenden von Schemas
- Schemas dienen zum Überprüfen von Nachrichten, die in einem B2B-Szenario ausgetauscht werden.

## Bearbeiten von Schemas
1. Wählen Sie die Kachel **Schemas** aus.
2. Wählen Sie auf dem geöffneten Blatt „Schemas“ das Schema aus, das Sie bearbeiten möchten.
3. Wählen Sie auf dem Blatt „Schemas“ den Link **Hochladen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/edit-1.png)
4. Wählen Sie im eingeblendeten Dialogfeld zur Dateiauswahl die Schemadatei aus, die Sie hochladen möchten.
5. Wählen Sie dann **Öffnen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/edit-2.png)
6. Sie erhalten eine Benachrichtigung, die angibt, dass das Hochladen erfolgreich war. ![](./media/app-service-logic-enterprise-integration-schemas/edit-3.png)

## Löschen von Schemas
1. Wählen Sie die Kachel **Schemas** aus.
2. Wählen Sie auf dem geöffneten Blatt „Schemas“ das Schema aus, das Sie löschen möchten.
3. Wählen Sie auf dem Blatt „Schemas“ auf der Menüleiste den Link **Löschen** aus. ![](./media/app-service-logic-enterprise-integration-schemas/delete-1.png)
4. Wenn Sie das ausgewählte Schema wirklich löschen möchten, wählen Sie im Dialogfeld „Schema löschen“ **Ja** aus, um Ihre Auswahl zu bestätigen. ![](./media/app-service-logic-enterprise-integration-schemas/delete-2.png)
5. Auf der aktualisierten Liste auf dem Blatt „Schemas“ wird das gelöschte Schema nicht mehr angezeigt. ![](./media/app-service-logic-enterprise-integration-schemas/delete-3.png)

## Nächste Schritte

- [Weitere Informationen zu Vereinbarungen](./app-service-logic-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")

      

<!---HONumber=AcomDC_0706_2016-->