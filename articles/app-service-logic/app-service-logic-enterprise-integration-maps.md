---
title: "Übersicht über Zuordnungen und Enterprise Integration Pack | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Zuordnungen mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 542278b38584d8e87a5dbf88af020dee3941cfa1


---
# <a name="learn-about-maps-and-the-enterprise-integration-pack"></a>Informationen zu Zuordnungen und Enterprise Integration Pack
## <a name="overview"></a>Übersicht
Für die Unternehmensintegration werden Zuordnungen verwendet, um XML-Daten von einem Format in an anderes zu transformieren. 

## <a name="what-is-a-map"></a>Was ist eine Zuordnung?
Eine Zuordnung ist ein XML-Dokument, das definiert, welche Daten in einem Dokument in ein anderes Format transformiert werden sollen. 

## <a name="why-use-maps"></a>Gründe für Zuordnungen
Angenommen, Sie erhalten regelmäßig B2B-Aufträge oder -Rechnungen von einem Kunden, der das Format JJMMTT für Datumsangaben verwendet. In Ihrer Organisation wird das Datum allerdings im Format TTMMJJJJ gespeichert. Mithilfe einer Zuordnung können das Datumsformat JJMMTT in das Format TTMMJJJJ *transformieren* , ehe die Auftrags- oder Rechnungsdetails in Ihrer Kundendatenbank gespeichert werden.

## <a name="how-do-i-create-a-map"></a>Wie erstelle ich eine Zuordnung?
Das [Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") für Visual Studio 2015 ermöglicht die Erstellung von BizTalk Integration-Projekten.  Durch Erstellen einer Integration Map-Datei können Sie Elemente visuell zwischen zwei XML-Schemadateien zuordnen.  Nach der Erstellung dieses Projekts wird ein XSLT-Dokument ausgegeben.

## <a name="how-to-upload-a-map"></a>Hochladen einer Zuordnung
Gehen Sie im Azure-Portal so vor:  

1. Wählen Sie **Durchsuchen**.  
   ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus.     
   ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Integrationskonto** aus, dem Sie die Zuordnung hinzufügen möchten.  
   ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie die Kachel **Zuordnungen** aus.  
   ![](./media/app-service-logic-enterprise-integration-maps/map-1.png)  
5. Wählen Sie auf dem geöffneten Blatt „Zuordnungen“ die Schaltfläche **Hinzufügen**.  
   ![](./media/app-service-logic-enterprise-integration-maps/map-2.png)  
6. Geben Sie einen Namen**** für die Zuordnung ein. Wählen Sie dann zum Hochladen der Zuordnungsdatei das Ordnersymbol rechts neben dem Textfeld **Zuordnung** aus. Wählen Sie nach Abschluss des Hochladevorgangs die Schaltfläche **OK**.  
   ![](./media/app-service-logic-enterprise-integration-maps/map-3.png)  
7. Die Zuordnung wurde Ihrem Integrationskonto hinzugefügt. Sie erhalten auf dem Bildschirm die Benachrichtigung, ob das Hinzufügen der Zuordnungsdatei erfolgreich war oder nicht. Nachdem Sie die Benachrichtigung erhalten haben, wählen Sie die Kachel **Zuordnungen** aus, auf der auf dem Blatt „Zuordnungen“ die neu hinzugefügte Zuordnung angezeigt wird:    
   ![](./media/app-service-logic-enterprise-integration-maps/map-4.png)  

## <a name="how-to-edit-a-map"></a>Bearbeiten eine Zuordnung
Zum Bearbeiten einer Zuordnung müssen Sie eine neue Zuordnungsdatei mit den gewünschten Änderungen hochladen. Zunächst müssen Sie die Zuordnung herunterladen und bearbeiten. 

Gehen Sie wie folgt vor, um eine neue Zuordnung hochzuladen, die eine vorhandene Zuordnung ersetzt:  

1. Wählen Sie die Kachel **Zuordnungen** aus.  
2. Wählen Sie auf dem geöffneten Blatt „Zuordnungen“ die Zuordnung aus, die Sie bearbeiten möchten.  
3. Wählen Sie auf dem Blatt **Zuordnungen** den Link **Aktualisieren** aus.  
   ![](./media/app-service-logic-enterprise-integration-maps/edit-1.png)   
4. Wählen Sie im eingeblendeten Dialogfeld zur Dateiauswahl die Zuordnungsdatei, die Sie hochladen möchten, und dann **Öffnen** aus.   
   ![](./media/app-service-logic-enterprise-integration-maps/edit-2.png)   
5. Nach dem Hochladen der Zuordnung erhalten Sie eine Benachrichtigung auf dem Bildschirm.    

## <a name="how-to-delete-a-map"></a>Löschen einer Zuordnung
1. Wählen Sie die Kachel **Zuordnungen** aus.  
2. Wählen Sie auf dem geöffneten Blatt „Zuordnungen“ die Zuordnung aus, die Sie löschen möchten.  
3. Wählen Sie den Link **Löschen**.    
   ![](./media/app-service-logic-enterprise-integration-maps/delete.png)   
4. Bestätigen Sie, dass Sie die Zuordnung wirklich löschen möchten.  
   ![](./media/app-service-logic-enterprise-integration-maps/delete-confirmation-1.png)   

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  
* [Weitere Informationen zu Vereinbarungen](app-service-logic-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  
* [Weitere Informationen zu Transformationen](app-service-logic-enterprise-integration-transform.md "Informationen zu Unternehmensintegrationstransformationen")  




<!--HONumber=Nov16_HO3-->


