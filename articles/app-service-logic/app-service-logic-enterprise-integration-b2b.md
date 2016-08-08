<properties 
	pageTitle="Erstellen von B2B-Lösungen mit dem Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
	description="Informationen zum Empfangen von Daten mithilfe der B2B-Features des Enterprise Integration Packs" 
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

# Informationen zum Empfangen von Daten mithilfe der B2B-Features des Enterprise Integration Packs#

## Übersicht ##

Dieses Dokument gehört zum Enterprise Integration Pack für Logik-Apps. Sehen Sie sich die Übersicht an, um weitere Informationen zu den [Funktionen des Enterprise Integration Packs](./app-service-logic-enterprise-integration-overview.md) zu erhalten.

## Voraussetzungen ##

Um die AS2- und X12-Aktionen verwenden zu können, benötigen Sie ein Unternehmensintegrationskonto.

[Erstellen eines Unternehmensintegrationskontos](./app-service-logic-enterprise-integration-accounts.md)

## Verwenden der B2B-Connectors für Logik-Apps ##

Sobald Sie ein Integrationskonto erstellt und diesem Partner und Vereinbarungen hinzugefügt haben, können Sie eine Logik-App erstellen, die einen B2B-Workflow (Business-to-Business) implementiert.

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie die AS2- und X12-Aktionen zum Erstellen einer B2B-Logik-App verwenden, die Daten von einem Handelspartner empfängt.

1. Erstellen Sie eine neue Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](./app-service-logic-enterprise-integration-accounts.md).
2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu. ![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)
3. Fügen Sie die Aktion **AS2 decodieren** hinzu, indem Sie zuerst **Aktion hinzufügen** auswählen. ![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)
4. Geben Sie **as2** in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)
6. Wählen Sie die Aktion **AS2 – AS2-Nachricht decodieren** aus. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)
7. Fügen Sie den **Text** hinzu, den Sie als Eingabe verwenden. Wählen Sie in diesem Beispiel den Text der HTTP-Anforderung aus, die die Logik-App ausgelöst hat. Alternativ können Sie einen Ausdruck eingeben, um die Header in das Feld **HEADERS** einzugeben:

    @triggerOutputs()['headers']

8. Fügen Sie die **Header** hinzu, die für AS2 erforderlich sind. Diese sind in den HTTP-Anforderungsheadern enthalten. Wählen Sie in diesem Beispiel die Header der HTTP-Anforderung aus, die die Logik-App ausgelöst hat.
9. Fügen Sie jetzt die Nachrichtenaktion „X12 decodieren“ hinzu, indem Sie nochmals **Aktion hinzufügen** auswählen. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)
10. Geben Sie **x12** in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)
11. Wählen Sie die Aktion **X12 – X12-Nachricht decodieren** aus, um sie der Logik-App hinzuzufügen ![](./media/app-service-logic-enterprise-integration-b2b/b2b-11.png).
12. Nun müssen Sie die Eingabe in diese Aktion angeben, die die Ausgabe der obigen AS2-Aktion sein wird. Der tatsächliche Nachrichteninhalt befindet sich in einem JSON-Objekt und ist mit base64 codiert. Deshalb müssen Sie einen Ausdruck als Eingabe angeben. Geben Sie daher den folgenden Ausdruck in das Eingabefeld **Zu decodierende X12-Flatfilenachricht** ein.

    @base64ToString(body('Decode\_AS2\_message')?['Message']?['Content'])

13. In diesem Schritt werden die vom Handelspartner empfangenen X12-Daten decodiert, und in einem JSON-Objekt werden verschiedene Elemente ausgegeben. Um den Partner über den Empfang der Daten zu informieren, können Sie eine Nachricht zurücksenden, die eine AS2 MDN (Message Disposition Notification, Benachrichtigung über den Nachrichtenstatus) in einer HTTP-Antwortaktion enthält.
14. Fügen Sie die Aktion **Antwort** hinzu, indem Sie **Aktion hinzufügen** auswählen. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)
15. Geben Sie das Wort **Antwort** in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)
16. Wählen Sie die Aktion **Antwort** aus, um sie hinzuzufügen. ![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)
17. Legen Sie das Antwortfeld **TEXT** mit dem folgenden Ausdruck fest, um auf die MDN in der Ausgabe der Aktion **X12-Nachricht decodieren** zuzugreifen.

    @base64ToString(body('Decode\_AS2\_message')?['OutgoingMdn']?['Content'])

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)
18. Speichern Sie Ihre Arbeit. ![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)

An diesem Punkt ist das Einrichten der B2B-Logik-App abgeschlossen. In einer realen Anwendung werden die decodierten X12-Daten in einer branchenspezifischen Anwendung oder einem Datenspeicher gespeichert. Sie können hierfür problemlos weitere Aktionen hinzufügen oder eine benutzerdefinierte API schreiben, um eine Verbindung mit Ihren branchenspezifischen Anwendungen herzustellen und diese APIs in Ihrer Logik-App zu verwenden.

## Features und Anwendungsfälle ##

- Die AS2- und X12-Codierungs- und Decodierungsaktionen ermöglichen Ihnen das Empfangen von Daten von und Senden von Daten an Handelspartner mithilfe von Protokollen gemäß Industriestandard und Logik-Apps.
- Sie können AS2 und X12 zusammen oder einzeln verwenden, um Daten nach Bedarf mit Handelspartnern auszutauschen.
- Die B2B-Aktionen erleichtern das Erstellen von Partnern und Vereinbarungen im Integrationskonto und deren Nutzung in einer Logik-App.
- Durch das Erweitern Ihrer Logik-App mit anderen Aktionen können Sie Daten mit anderen Anwendungen und Diensten wie Salesforce austauschen.

## Weitere Informationen ##

[Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md)

<!---HONumber=AcomDC_0727_2016-->