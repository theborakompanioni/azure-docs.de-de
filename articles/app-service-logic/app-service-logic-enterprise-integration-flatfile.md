<properties 
	pageTitle="Informationen zum Codieren bzw. Decodieren von Flatfiles mit Enterprise Integration Pack und Logik-Apps | Microsoft Azure App Service | Microsoft Azure" 
	description="Verwenden Sie die Features von Enterprise Integration Pack und Logik-Apps zum Codieren bzw. Decodieren von Flatfiles." 
	services="app-service\logic" 
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

# Unternehmensintegration mit Flatfiles

## Übersicht

Sie verwenden den Flatfile-Codierungsconnector, der den XML-Inhalt codiert, in einer Logik-App. Sie möchten XML-Inhalt ggf. codieren, bevor Sie ihn in einem B2B-Szenario an einen Geschäftspartner senden. Die Logik-App, die Sie erstellen, kann ihren XML-Inhalt aus einer Vielzahl von Quellen abrufen, so z.B. über einen HTTP-Anforderungstrigger, aus einer anderen Anwendung oder über einen der zahlreichen [Connectors](../connectors/apis-list.md). In der [Dokumentation zu Logik-Apps](./app-service-logic-what-are-logic-apps.md "Weitere Informationen zu Logik-Apps") finden Sie weitere Informationen zur Leistungsfähigkeit von Logik-Apps.

## Erstellen eines Flatfile-Codierungsconnectors

Führen Sie diese Schritte zum Erstellen einer Logik-App und Hinzufügen eines Flatfile-Codierungsconnectors für die Logik-App aus.

1. Erstellen Sie eine Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](./app-service-logic-enterprise-integration-accounts.md "Erfahren Sie, wie Sie ein Integrationskonto mit einer Logik-App verknüpfen"), das das Schema enthält, das Sie zum Codieren der XML-Daten verwenden.
2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)
3. Fügen Sie die Flatfile-Decodierungsaktionen wie folgt hinzu:
-  Wählen Sie das **Pluszeichen** aus.
-  Wählen Sie den Link **Aktion hinzufügen** aus, der angezeigt wird, nachdem Sie das Pluszeichen ausgewählt haben.
-  Geben Sie *Flat* in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten.
-  Wählen Sie in der Liste die Aktion **Flatfilecodierung** aus. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)
6. Wählen Sie im eingeblendeten Dialogfeld „Flatfilecodierung“ das Textfeld **INHALT** aus. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)
7. Wählen Sie das „Body“-Tag als den Inhalt aus, den Sie codieren möchten. Das Inhaltsfeld wird mit dem „Body“-Tag aufgefüllt. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)
8. Wählen Sie das Listenfeld **SCHEMANAME** und dann das Schema aus, das Sie verwenden möchten, um den zuvor eingegebenen *Inhalt* zu codieren. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)
9. Speichern Sie Ihre Arbeit. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)

An diesem Punkt ist das Einrichten des Flatfile-Codierungsconnectors abgeschlossen. In der Praxis werden die codierten Daten in einer branchenspezifischen Anwendung wie Salesforce gespeichert oder an einen Handelspartner gesendet. Mithilfe eines der anderen bereitgestellten Connectors können Sie mühelos eine Aktion zum Senden der Ausgabe der Codierungsaktion an Salesforce oder Ihren Handelspartner hinzufügen.

Sie können nun Ihren Connector testen, indem Sie eine Anforderung an den HTTP-Endpunkt richten und den XML-Inhalt in den Text der Anforderung einbeziehen.

## Erstellen eines Flatfile-Decodierungsconnectors

### Voraussetzung
Zum Ausführen dieser Schritte müssen Sie Ihre Schemadatei bereits in Ihr Integrationskonto hochgeladen haben.

1. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)
2. Fügen Sie die Flatfile-Decodierungsaktionen wie folgt hinzu:
-  Wählen Sie das **Pluszeichen** aus.
-  Wählen Sie den Link **Aktion hinzufügen** aus, der angezeigt wird, nachdem Sie das Pluszeichen ausgewählt haben.
-  Geben Sie *Flat* in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten.
-  Wählen Sie in der Liste die Aktion **Flatfiledecodierung** aus. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)
- Wählen Sie das Steuerelement **INHALT** aus. Eine Liste mit Inhalten aus vorherigen Schritte wird erstellt, die Sie als zu decodierende Inhalte verwenden können. Sie sehen, dass der *Text* aus der eingehenden HTTP-Anforderung als Inhalt verfügbar ist, der decodiert werden kann. Sie können den zu decodierenden Inhalt auch direkt in das Steuerelement **INHALT** eingeben. In diesem Beispiel habe ich *Body* ausgewählt, um den Text der eingehenden HTTP-Anforderung aus Schritt 1 der Decodierungsschritte zu verwenden.
- Wählen Sie das *Body*-Tag aus. Wie Sie sehen, befindet sich das „Body“-Tag nun im Steuerelement INHALT.
- Wählen Sie den Namen des Schemas aus, das Sie zum Decodieren des Inhalts verwenden möchten. In diesem Beispiel wähle ich *OrderFile*. Beachten Sie, dass „OrderFile“ der Name des Schemas ist, das ich zuvor in mein Integrationskonto hochgeladen habe. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)
- Wählen Sie im Menü den Link **Speichern** aus, um Ihre Arbeit zu speichern. ![](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)

An diesem Punkt ist das Einrichten des Flatfile-Decodierungsconnectors abgeschlossen. In einer realen Anwendung werden die decodierten Daten in einer branchenspezifischen Anwendung wie Salesforce gespeichert. Sie können problemlos eine Aktion zum Senden der Ausgabe der Decodierungsaktion an Salesforce hinzufügen.

Sie können nun Ihren Connector testen, indem Sie eine Anforderung an den HTTP-Endpunkt richten und den XML-Inhalt, den Sie decodieren möchten, in den Text der Anforderung einbeziehen.

## Weitere Informationen
- [Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0727_2016-->