---
title: "Übersicht über die XML-Überprüfung im Enterprise Integration Pack | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Überprüfung im Enterprise Integration Pack und in Logik-Apps erfolgt"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7a40b20c8845d09547e80eb230c73ef7683eb2b7


---
# <a name="enterprise-integration-with-xml-validation"></a>Unternehmensintegration mit XML-Überprüfung
## <a name="overview"></a>Übersicht
In B2B-Szenarien müssen die Vereinbarungspartner häufig überprüfen, ob die untereinander ausgetauschten Nachrichten gültig sind, ehe die Verarbeitung der Daten beginnen kann. Im Enterprise Integration Pack können Sie den XML-Connector „Überprüfung“ verwenden, um Dokumente im Abgleich mit einem vordefinierten Schema auf Gültigkeit zu überprüfen.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Überprüfen eines Dokuments mit dem XML-Connector „Überprüfung“
1. Erstellen Sie eine Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](app-service-logic-enterprise-integration-accounts.md "Erfahren Sie, wie Sie ein Integrationskonto mit einer Logik-App verknüpfen"), das das Schema enthält, das Sie zum Überprüfen der XML-Daten verwenden.
2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu.  
   ![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Fügen Sie die Aktion **XML-Überprüfung** hinzu, indem Sie zuerst **Aktion hinzufügen** auswählen.  
4. Geben Sie *xml* in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten. 
5. Wählen Sie **XML-Überprüfung**    aus.  
   ![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Wählen Sie das Textfeld **INHALT** aus.  
   ![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Wählen Sie das „Body“-Tag als den Inhalt aus, den Sie überprüfen möchten.   
   ![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Wählen Sie das Listenfeld **SCHEMANAME** und dann das Schema aus, das Sie verwenden möchten, um den zuvor eingegebenen *Inhalt* zu überprüfen.     
   ![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Speichern Sie Ihre Arbeit.  
   ![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

An diesem Punkt ist das Einrichten des Überprüfungsconnectors abgeschlossen. In einer realen Anwendung werden die überprüften Daten in einer branchenspezifischen Anwendung wie Salesforce gespeichert. Sie können problemlos eine Aktion zum Senden der Ausgabe der Überprüfung an Salesforce hinzufügen. 

Sie können jetzt Ihre Überprüfungsaktion testen, indem Sie eine Anforderung an den HTTP-Endpunkt stellen.  

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")   




<!--HONumber=Nov16_HO3-->


