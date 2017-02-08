---
title: Informationen zum Codieren bzw. Decodieren von Flatfiles mit Enterprise Integration Pack und Logik-Apps | Microsoft Azure App Service | Microsoft-Dokumentation
description: Verwenden Sie die Features von Enterprise Integration Pack und Logik-Apps zum Codieren bzw. Decodieren von Flatfiles.
services: app-service\logic
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5283c565c29c357fa2f82535dc07891ab42e6704


---
# <a name="enterprise-integration-with-flat-files"></a>Unternehmensintegration mit Flatfiles
## <a name="overview"></a>Übersicht
Sie möchten XML-Inhalt ggf. codieren, bevor Sie ihn in einem B2B-Szenario (Business-to-Business) an einen Geschäftspartner senden. Bei einer Logik-App, die mit dem Logik-Apps-Feature von Azure App Service erstellt wurde, können Sie hierfür den Flatfile-Codierungsconnector verwenden. Die Logik-App, die Sie erstellen, kann ihren XML-Inhalt aus einer Vielzahl von Quellen abrufen, beispielsweise über einen HTTP-Anforderungstrigger, aus einer anderen Anwendung oder über einen der zahlreichen [Connectors](../connectors/apis-list.md). Weitere Informationen zu Logik-Apps finden Sie in der [Logik-Apps-Dokumentation](app-service-logic-what-are-logic-apps.md "Weitere Informationen zu Logik-Apps").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Erstellen eines Flatfile-Codierungsconnectors
Führen Sie diese Schritte aus, um der Logik-App einen Flatfile-Codierungsconnector hinzuzufügen.

1. Erstellen Sie eine Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](app-service-logic-enterprise-integration-accounts.md "Erfahren Sie, wie Sie ein Integrationskonto mit einer Logik-App verknüpfen"). Dieses Konto enthält das Schema, das Sie zum Codieren der XML-Daten verwenden.  
2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu.  
   ![Screenshot des auszuwählenden Triggers](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Fügen Sie die Flatfile-Codierungsaktion wie folgt hinzu:
   
    a. Wählen Sie das **Pluszeichen** aus.
   
    b. Wählen Sie den Link **Aktion hinzufügen** aus (wird angezeigt, nachdem Sie das Pluszeichen ausgewählt haben).
   
    c. Geben Sie im Suchfeld *Flat* ein, um aus allen Aktionen die Aktion herauszufiltern, die Sie verwenden möchten.
   
    d. Wählen Sie in der Liste die Option **Flatfilecodierung** aus.   
   ![Screenshot der Option „Flatfilecodierung“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. Wählen Sie im Dialogfeld **Flatfilecodierung** das Textfeld **Inhalt** aus.  
   ![Screenshot des Textfelds „Inhalt“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Wählen Sie das „Body“-Tag als den Inhalt aus, den Sie codieren möchten. Das Inhaltsfeld wird mit dem „Body“-Tag aufgefüllt.     
   ![Screenshot des Body-Tags](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Wählen Sie das Listenfeld **Schemaname** und dann das Schema aus, das Sie verwenden möchten, um den eingegebenen Inhalt zu codieren.    
   ![Screenshot des Listenfelds „Schemaname“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Speichern Sie Ihre Arbeit.   
   ![Screenshot des Symbols „Speichern“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

An diesem Punkt ist das Einrichten des Flatfile-Codierungsconnectors abgeschlossen. In einer realen Anwendung werden die codierten Daten meist in einer branchenspezifischen Anwendung gespeichert, z.B. Salesforce. Sie können diese codierten Daten auch an einen Handelspartner senden. Mithilfe eines der anderen bereitgestellten Connectors können Sie mühelos eine Aktion zum Senden der Ausgabe der Codierungsaktion an Salesforce oder Ihren Handelspartner hinzufügen.

Sie können nun Ihren Connector testen, indem Sie eine Anforderung an den HTTP-Endpunkt richten und den XML-Inhalt in den Text der Anforderung einbeziehen.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Erstellen eines Flatfile-Decodierungsconnectors
> [!NOTE]
> Zum Ausführen dieser Schritte müssen Sie Ihre Schemadatei bereits in Ihr Integrationskonto hochgeladen haben.
> 
> 

1. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu.  
   ![Screenshot des auszuwählenden Triggers](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Fügen Sie die Flatfile-Decodierungsaktion wie folgt hinzu:
   
    a. Wählen Sie das **Pluszeichen** aus.
   
    b. Wählen Sie den Link **Aktion hinzufügen** aus (wird angezeigt, nachdem Sie das Pluszeichen ausgewählt haben).
   
    c. Geben Sie im Suchfeld *Flat* ein, um aus allen Aktionen die Aktion herauszufiltern, die Sie verwenden möchten.
   
    d. Wählen Sie in der Liste die Aktion **Flatfiledecodierung** aus.   
   ![Screenshot der Option „Flatfiledecodierung“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
3. Wählen Sie das Steuerelement **Inhalt** aus. Eine Liste mit Inhalten aus vorherigen Schritten wird erstellt, die Sie als zu decodierende Inhalte verwenden können. Sie sehen, dass der *Text* aus der eingehenden HTTP-Anforderung als Inhalt verfügbar ist, der decodiert werden kann. Sie können den zu decodierenden Inhalt auch direkt in das Steuerelement **Inhalt** eingeben.     
4. Wählen Sie das *Body* -Tag aus. Wie Sie sehen, befindet sich das „Body“-Tag nun im Steuerelement **Inhalt** .
5. Wählen Sie den Namen des Schemas aus, das Sie zum Decodieren des Inhalts verwenden möchten. Im folgenden Screenshot ist dargestellt, dass *OrderFile* der ausgewählte Schemaname ist. Dieser Schemaname wurde zuvor in das Integrationskonto hochgeladen.
   
   ![Screenshot des Dialogfelds „Flatfiledecodierung“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. Speichern Sie Ihre Arbeit.  
   ![Screenshot des Symbols „Speichern“](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

An diesem Punkt ist das Einrichten des Flatfile-Decodierungsconnectors abgeschlossen. In einer realen Anwendung werden die decodierten Daten meist in einer branchenspezifischen Anwendung gespeichert, z.B. Salesforce. Sie können problemlos eine Aktion zum Senden der Ausgabe der Decodierungsaktion an Salesforce hinzufügen.

Sie können nun Ihren Connector testen, indem Sie eine Anforderung an den HTTP-Endpunkt richten und den XML-Inhalt, den Sie decodieren möchten, in den Text der Anforderung einbeziehen.  

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  




<!--HONumber=Nov16_HO3-->


