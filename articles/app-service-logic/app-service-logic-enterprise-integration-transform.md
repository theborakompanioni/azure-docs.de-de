<properties 
	pageTitle="Übersicht zu Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
	description="Nutzen Sie die Enterprise Integration Pack-Features, um die Integration von Geschäftsprozessen mithilfe des Microsoft Azure App Service-Diensts zu ermöglichen." 
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

# Unternehmensintegration mit XML-Transformationen

## Übersicht
Der Unternehmensintegrationsconnector „Transformieren“ wandelt Daten von einem Format in ein anderes um. Angenommen, es gibt eine eingehende Nachricht, die das aktuelle Datum im Format „JahrMonatTag“ enthält. Mithilfe einer Transformation können Sie das Datum in das Format „TagMonatJahr“ umformatieren.

## Aufgaben einer Transformation
Eine Transformation, auch als Zuordnung bezeichnet, besteht aus einem XML-Quellschema (der Eingabe) und einem XML-Zielschema (der Ausgabe). Sie können verschiedene integrierte Funktionen verwenden, um Daten zu ändern oder zu steuern, einschließlich Zeichenfolgenbearbeitungen, bedingter Zuordnungen, arithmetischer Ausdrücke, Datum-Uhrzeit-Formatierungen und sogar Schleifenkonstrukten.

## Erstellen einer Transformation
Sie können mit dem Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas) eine Transformation/Zuordnung erstellen. Nachdem Sie das Erstellen und Testen der Transformation abgeschlossen haben, laden Sie sie in Ihr Integrationskonto hoch.

## Verwenden einer Transformation
Nachdem Sie die Transformation in Ihr Integrationskonto hochgeladen haben, können Sie sie zum Erstellen einer Logik-App nutzen. Die Logik-App führt dann Ihre Transformationen aus, sobald sie ausgelöst wird (und es eingegebenen Inhalt gibt, der transformiert werden muss).

**So erfolgt eine Transformation**:

### Voraussetzungen 
Führen Sie in der Vorschauversion die folgenden Schritte aus:

-  [Erstellen eines Azure Functions-Containers](https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Erstellen eines Azure Functions-Containers")
-  [Hinzufügen einer Funktion zum Azure Functions-Container](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Diese Vorlage erstellt eine auf einem Webhook basierende in C# geschriebene Azure-Funktion mit Transformationsfunktionen zur Verwendung von Logik-Apps in Integrationsszenarien.")
-  Erstellen eines Integrationskontos und Hinzufügen einer Zuordnung

>[AZURE.TIP] Notieren Sie sich den Namen des Azure Functions-Containers sowie der Azure-Funktion; Sie werden sie im nächsten Schritt benötigen.

Sobald die Voraussetzungen erfüllt sind, können Sie Ihre Logik-App erstellen:

1. Erstellen Sie eine Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](./app-service-logic-enterprise-integration-accounts.md "Erfahren Sie, wie Sie ein Integrationskonto mit einer Logik-App verknüpfen"), das die Zuordnung enthält.
2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu. ![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)
3. Fügen Sie die Aktion **XML transformieren** hinzu, indem Sie zuerst **Aktion hinzufügen** auswählen. ![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)
4. Geben Sie *Transformieren* in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten. ![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)
5. Wählen Sie die Aktion **XML transformieren** aus. ![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)
6. Wählen Sie den **FUNKTIONSCONTAINER**, der die Funktion enthält, die Sie verwenden möchten. Dies ist der Name des Azure Functions-Containers, den Sie zuvor erstellt haben.
7. Wählen Sie die **FUNKTION** aus, die Sie verwenden möchten. Dies ist der Name der Azure-Funktion, die Sie zuvor erstellt haben.
8. Fügen Sie den XML-**INHALT** hinzu, der transformiert werden soll. Sie können beliebige XML-Daten verwenden, die Sie in der HTTP-Anforderung als **INHALT** empfangen. Wählen Sie in diesem Beispiel den Text der HTTP-Anforderung aus, die die Logik-App ausgelöst hat.
9. Wählen Sie den Namen der **ZUORDNUNG** aus, mit deren Hilfe die Transformation erfolgen soll. Die Zuordnung muss bereits in Ihrem Integrationskonto enthalten sein. In einem früheren Schritt haben Sie Ihrer Logik-App bereits Zugriff auf Ihr Integrationskonto gewährt, das Ihre Zuordnung enthält.
10. Speichern Sie Ihre Arbeit. ![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png)

An diesem Punkt ist das Einrichten der Zuordnung abgeschlossen. In einer realen Anwendung werden die transformierten Daten in einer branchenspezifischen Anwendung wie Salesforce gespeichert. Sie können problemlos eine Aktion zum Senden der Ausgabe der Transformation an Salesforce hinzufügen.

Sie können jetzt Ihre Transformation testen, indem Sie eine Anforderung an den HTTP-Endpunkt stellen.

## Features und Anwendungsfälle

- Die in einer Zuordnung erstellte Transformation kann einfach sein, beispielsweise das Kopieren eines Namen und einer Adresse aus einem Dokument in ein anderes. Oder Sie können mithilfe der standardmäßigen Zuordnungsvorgänge komplexere Transformationen erstellen.
- Mehrere Zuordnungsvorgänge oder Funktionen sind verfügbar, einschließlich Zeichenfolgen, Datum-Uhrzeit-Funktionen usw.
- Sie können Daten zwischen den Schemas direkt kopieren. Im Mapper, der im SDK enthalten ist, ist dies so einfach wie das Zeichnen einer Linie, die die Elemente im Quellschema mit ihren Gegenstücken im Zielschema verbindet.
- Wenn Sie eine Zuordnung erstellen, zeigen Sie eine grafische Darstellung der Zuordnung einschließlich aller erstellten Beziehungen und Links an.
- Verwenden Sie das Feature Testzuordnung zum Hinzufügen einer XML-Beispielnachricht. Mit einem einfachen Mausklick können Sie die erstellte Zuordnung testen und die generierte Ausgabe anzeigen.
- Hochladen vorhandener Zuordnungen
- Unterstützung für das XML-Format.


## Weitere Informationen
- [Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")
- [Weitere Informationen zu Zuordnungen](./app-service-logic-enterprise-integration-maps.md "Informationen zu Zuordnungen für die Unternehmensintegration")
 

<!---HONumber=AcomDC_0803_2016-->