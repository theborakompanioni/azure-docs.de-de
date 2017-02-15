---
title: "Übersicht über Schemas und das Enterprise Integration Pack | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Schemas mit dem Enterprise Integration Pack und Logik-Apps verwenden.
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2e0419c46e9380eb7e1848524a99501b4961d35c


---
# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Informationen zu Schemas und zum Enterprise Integration Pack
## <a name="why-use-a-schema"></a>Gründe für die Verwendung eines Schemas
Verwenden Sie Schemas, um zu bestätigen, dass empfangene XML-Dokumente gültig sind und die erwarteten Daten in einem vordefinierten Format enthalten. Schemas dienen zum Überprüfen von Nachrichten, die in einem B2B-Szenario ausgetauscht werden.

## <a name="add-a-schema"></a>Hinzufügen eines Schemas
Gehen Sie im Azure-Portal so vor:  

1. Wählen Sie **Weitere Dienste**.  
   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    
2. Geben Sie im Filtersuchfeld das Wort **Integration** ein, und wählen Sie in der Liste mit den Ergebnissen den Eintrag **Integrationskonten** aus.     
   ![Screenshot des Filtersuchfelds](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Schema hinzufügen.    
   ![Screenshot der Liste mit den Integrationskonten](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  
4. Wählen Sie die Kachel **Schemas** aus.  
   ![Screenshot des EIP-Integrationskontos mit Hervorhebung von „Schemas“](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Hinzufügen einer Schemadatei, die kleiner als 2 MB ist
1. Wählen Sie auf dem Blatt **Schemas**, das geöffnet wird (vorherige Schritte), die Option **Hinzufügen**.  
   ![Screenshot des Blatts „Schemas“ mit Hervorhebung der Schaltfläche „Hinzufügen“](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  
2. Geben Sie einen Namen für das Schema ein. Wählen Sie zum Hochladen der Schemadatei dann das Ordnersymbol neben dem Textfeld **Schema**. Wählen Sie nach Abschluss des Hochladevorgangs die Schaltfläche **OK**.    
   ![Screenshot von „Schema hinzufügen“ mit Hervorhebung von „Kleine Datei“](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Hinzufügen einer Schemadatei, die größer als 2 MB ist (bis maximal 8 MB)
Der Prozess hierfür richtet sich nach der Blobcontainer-Zugriffsebene: **Öffentlich** oder **Kein anonymer Zugriff**. Wählen Sie zum Ermitteln dieser Zugriffsebene im **Azure-Speicher-Explorer** unter **Blobcontainer** den gewünschten Blobcontainer aus. Wählen Sie **Sicherheit** und dann die Registerkarte **Zugriffsebene**.

1. Führen Sie die folgenden Schritte aus, wenn die Blob-Sicherheitszugriffsebene **Öffentlich** lautet.  
   ![Screenshot des Azure-Speicher-Explorers mit Hervorhebung von „Blobcontainer“, „Sicherheit“ und „Öffentlich“](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  
   
    a. Laden Sie das Schema in den Speicher hoch, und kopieren Sie den URI.  
    ![Screenshot des Speicherkontos mit Hervorhebung des URI](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  
   
    b. Wählen Sie unter **Schema hinzufügen** die Option **Große Datei**, und geben Sie den URI im Textfeld **Inhalts-URI** an.  
    ![Screenshot von Schemas mit Hervorhebung der Schaltfläche „Hinzufügen“ und der Option „Große Datei“](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  
2. Führen Sie die folgenden Schritte aus, wenn die Blob-Sicherheitszugriffsebene **Kein anonymer Zugriff** lautet.  
   ![Screenshot des Azure-Speicher-Explorers mit Hervorhebung von „Blobcontainer“, „Sicherheit“ und „Kein anonymer Zugriff“](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  
   
    a. Laden Sie das Schema in den Speicher hoch.  
    ![Screenshot des Speicherkontos](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)
   
    b. Generieren Sie eine Shared Access Signature für das Schema.  
    ![Screenshot des Speicherkontos mit Hervorhebung der Registerkarte „Shared Access Signatures“](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)
   
    c. Wählen Sie unter **Schema hinzufügen** die Option **Große Datei**, und geben Sie den Shared Access Signature-URI im Textfeld**Inhalts-URI** an.  
    ![Screenshot von Schemas mit Hervorhebung der Schaltfläche „Hinzufügen“ und der Option „Große Datei“](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  
3. Auf dem Blatt **Schemas** des EIP-Integrationskontos sollte jetzt das neu hinzugefügte Schema angezeigt werden.  
   ![Screenshot des EIP-Integrationskontos mit Hervorhebung von „Schemas“ und des neuen Schemas](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Bearbeiten von Schemas
1. Wählen Sie die Kachel **Schemas** aus.  
2. Wählen Sie auf dem geöffneten Blatt **Schemas** das Schema aus, das Sie bearbeiten möchten.
3. Wählen Sie auf dem Blatt **Schemas** die Option **Bearbeiten**.  
   ![Screenshot des Blatts „Schemas“](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Wählen Sie im geöffneten Dialogfeld mit der Dateiauswahl die Schemadatei aus, die Sie bearbeiten möchten.
5. Wählen Sie in der Dateiauswahl die Option **Öffnen**.  
   ![Screenshot der Dateiauswahl](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Sie erhalten eine Benachrichtigung, die angibt, dass das Hochladen erfolgreich war.  

## <a name="delete-schemas"></a>Löschen von Schemas
1. Wählen Sie die Kachel **Schemas** aus.  
2. Wählen Sie auf dem geöffneten Blatt **Schemas** das Schema aus, das Sie löschen möchten.  
3. Wählen Sie auf dem Blatt **Schemas** die Option **Löschen**.
   ![Screenshot des Blatts „Schemas“](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  
4. Wählen Sie **Ja**, um Ihre Auswahl zu bestätigen.  
   ![Screenshot der Bestätigungsmeldung „Schema löschen“](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. In der aktualisierten Liste auf dem Blatt **Schemas** wird das gelöschte Schema nicht mehr angezeigt.  
   ![Screenshot des EIP-Integrationskontos mit Hervorhebung von „Schemas“](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  




<!--HONumber=Nov16_HO3-->


