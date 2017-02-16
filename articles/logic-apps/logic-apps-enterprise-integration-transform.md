---
title: "Übersicht über Enterprise Integration Pack | Microsoft-Dokumentation"
description: "Nutzen Sie die Enterprise Integration Pack-Features, um die Integration von Geschäftsprozessen mithilfe des Microsoft Azure App Service-Diensts zu ermöglichen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: cca850073040cde0fb6f035f041419ac777bb0a2


---
# <a name="enterprise-integration-with-xml-transforms"></a>Unternehmensintegration mit XML-Transformationen
## <a name="overview"></a>Übersicht
Der Unternehmensintegrationsconnector „Transformieren“ wandelt Daten von einem Format in ein anderes um. Angenommen, es gibt eine eingehende Nachricht, die das aktuelle Datum im Format „JahrMonatTag“ enthält. Mithilfe einer Transformation können Sie das Datum in das Format „TagMonatJahr“ umformatieren.

## <a name="what-does-a-transform-do"></a>Aufgaben einer Transformation
Eine Transformation, auch als Zuordnung bezeichnet, besteht aus einem XML-Quellschema (der Eingabe) und einem XML-Zielschema (der Ausgabe). Sie können verschiedene integrierte Funktionen verwenden, um Daten zu ändern oder zu steuern, einschließlich Zeichenfolgenbearbeitungen, bedingter Zuordnungen, arithmetischer Ausdrücke, Datum-Uhrzeit-Formatierungen und sogar Schleifenkonstrukten.

## <a name="how-to-create-a-transform"></a>Erstellen einer Transformation
Sie können mit dem Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas)eine Transformation/Zuordnung erstellen. Nachdem Sie das Erstellen und Testen der Transformation abgeschlossen haben, laden Sie sie in Ihr Integrationskonto hoch. 

## <a name="how-to-use-a-transform"></a>Verwenden einer Transformation
Nachdem Sie die Transformation/Zuordnung in Ihr Integrationskonto hochgeladen haben, können Sie sie zum Erstellen einer Logik-App nutzen. Die Logik-App führt Ihre Transformationen aus, sobald sie ausgelöst wird (und es eingegebenen Inhalt gibt, der transformiert werden muss).

**So erfolgt eine Transformation**:

### <a name="prerequisites"></a>Voraussetzungen

* Erstellen eines Integrationskontos und Hinzufügen einer Zuordnung  

Sobald die Voraussetzungen erfüllt sind, können Sie Ihre Logik-App erstellen:  

1. Erstellen Sie eine Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Erfahren Sie, wie Sie ein Integrationskonto mit einer Logik-App verknüpfen."), das die Zuordnung enthält.
2. Fügen Sie Ihrer Logik-App einen **Anforderungstrigger** hinzu.  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Fügen Sie die Aktion **XML transformieren** hinzu, indem Sie zuerst **Aktion hinzufügen**  auswählen.  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Geben Sie *transformieren* in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten.  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Wählen Sie die Aktion **XML transformieren** aus.   
6. Fügen Sie den XML-**INHALT** hinzu, der transformiert werden soll. Sie können beliebige XML-Daten verwenden, die Sie in der HTTP-Anforderung als **INHALT** empfangen. Wählen Sie in diesem Beispiel den Text der HTTP-Anforderung aus, die die Logik-App ausgelöst hat.
7. Wählen Sie den Namen der **ZUORDNUNG** aus, mit deren Hilfe die Transformation erfolgen soll. Die Zuordnung muss bereits in Ihrem Integrationskonto enthalten sein. In einem früheren Schritt haben Sie Ihrer Logik-App bereits Zugriff auf Ihr Integrationskonto gewährt, das Ihre Zuordnung enthält.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Speichern Sie Ihre Arbeit.  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

An diesem Punkt ist das Einrichten der Zuordnung abgeschlossen. In einer realen Anwendung werden die transformierten Daten in einer branchenspezifischen Anwendung wie Salesforce gespeichert. Sie können problemlos eine Aktion zum Senden der Ausgabe der Transformation an Salesforce hinzufügen. 

Sie können jetzt Ihre Transformation testen, indem Sie eine Anforderung an den HTTP-Endpunkt stellen.  

## <a name="features-and-use-cases"></a>Features und Anwendungsfälle
* Die in einer Zuordnung erstellte Transformation kann einfach sein, beispielsweise das Kopieren eines Namen und einer Adresse aus einem Dokument in ein anderes. Oder Sie können mithilfe der standardmäßigen Zuordnungsvorgänge komplexere Transformationen erstellen.  
* Mehrere Zuordnungsvorgänge oder Funktionen sind verfügbar, einschließlich Zeichenfolgen, Datum-Uhrzeit-Funktionen usw.  
* Sie können Daten zwischen den Schemas direkt kopieren. Im Mapper, der im SDK enthalten ist, ist dies so einfach wie das Zeichnen einer Linie, die die Elemente im Quellschema mit ihren Gegenstücken im Zielschema verbindet.  
* Wenn Sie eine Zuordnung erstellen, zeigen Sie eine grafische Darstellung der Zuordnung einschließlich aller erstellten Beziehungen und Links an.
* Verwenden Sie das Feature Testzuordnung zum Hinzufügen einer XML-Beispielnachricht. Mit einem einfachen Mausklick können Sie die erstellte Zuordnung testen und die generierte Ausgabe anzeigen.  
* Hochladen vorhandener Zuordnungen  
* Unterstützung für das XML-Format.

## <a name="learn-more"></a>Weitere Informationen
* [Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  
* [Weitere Informationen zu Zuordnungen](../logic-apps/logic-apps-enterprise-integration-maps.md "Informationen zu Zuordnungen für die Unternehmensintegration")  




<!--HONumber=Jan17_HO3-->


