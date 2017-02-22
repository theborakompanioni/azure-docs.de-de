---
title: Verwenden des SAP-Connectors mit lokalem Datengateway in Azure Logic Apps | Microsoft-Dokumentation
description: "Logic Apps ermöglicht Ihnen mühelos das Herstellen einer Verbindung mit einem lokalen SAP-System im Rahmen Ihres Workflows."
services: logic-apps
documentationcenter: dev-center-name
author: padmavc
manager: anneta
ms.service: logic-apps
ms.devlang: wdl
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/10/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 12584e9aa9c1400aba040320bd3a5f3ba3017470
ms.openlocfilehash: a483f527d15372c94fe7fe813dc49c3d6423e1e3


---
# <a name="get-started-with-the-sap-connector"></a>Erste Schritte mit dem SAP-Connector 

Hybridcloudkonnektivität ist der Kern von Logic Apps. Das lokale Datengateway ermöglicht das Verwalten von Daten und den sicheren Zugriff auf Ressourcen, die für Logic Apps lokal sind. In diesem Artikel veranschaulichen wir mit einem einfachen Szenario das Herstellen einer Verbindung mit einem lokalen SAP-System : Anfordern eines IDocs und Zurücksenden der Antwort.    

 > [!NOTE]
 > Dieser SAP-Connector unterstützt die folgenden SAP-Systeme. Derzeit gilt in Logic Apps eine Timeout-Einschränkung, die Anforderungen von über 90 Sekunden blockiert. Derzeit gilt eine Einschränkung für die Anzahl der angezeigten Felder in der Dateiauswahl (Pfade können manuell hinzugefügt werden).
 >
 >

## <a name="prerequisites"></a>Voraussetzungen
- Installieren und konfigurieren Sie das neueste [lokale Datengateway](https://www.microsoft.com/en-us/download/details.aspx?id=53127).  

    Installieren Sie das neueste lokale Datengateway, Version 1.15.6150.1 oder höher, sofern noch nicht geschehen. Anweisungen finden Sie in [diesem Artikel](http://aka.ms/logicapps-gateway). Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie mit den verbleibenden Schritten fortfahren können.

- Herunterladen und Installieren der aktuellen SAP-Clientbibliothek auf dem Computer mit dem universellen Gateway

## <a name="use-sap-connector"></a>Verwenden des SAP-Connectors

1. Wir erstellen zunächst einen HTTP-Anforderungstrigger und geben dann im Suchfeld „SAP“ ein, um die SAP-Connector-Aktion auszuwählen.    
 ![Suchen nach SAP](media/logic-apps-using-sap-connector/picture1.png)

2. Wählen Sie „SAP“ (je nach Ihrer SAP-Einrichtung „ApplicationHost“ oder „MessagingHost“), und erstellen Sie mit dem universellen Gateway eine Verbindung dafür.
 - Wenn es noch keine Verbindung gibt, werden Sie aufgefordert, eine zu herstellen.
 - Aktivieren Sie das Kontrollkästchen für die Verbindung über das lokale Datengateway. Anschließend werden weitere Felder angezeigt.
 - Festlegen der Zeichenfolge für den Verbindungsnamen
 - Wählen Sie das im vorhergehenden Schritt erstellte Gateway aus, oder wählen Sie „Gateway installieren“, um ein neues Gateway zu installieren.   
 ![Hinzufügen der Verbindungszeichenfolge für SAP](media/logic-apps-using-sap-connector/picture2.png)   
  
  > [!NOTE]
  > Es gibt zwei verschiedene SAP-Verbindungen, eine für den [Anwendungshost](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) und eine für den [Messaginghost](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)
  >
  >

3. Nachdem Sie alle Details angegeben haben, klicken Sie auf „Erstellen“. Logic Apps konfiguriert und testet die Verbindung, um sicherzustellen, dass sie ordnungsgemäß funktioniert. Wenn alles ausgecheckt wird, werden Optionen für die zuvor ausgewählte Karte angezeigt. Suchen Sie die richtige IDoc-Kategorie mithilfe der Dateiauswahl, oder geben Sie den Pfad manuell ein und wählen die HTTP-Antwort im Textfeld aus.    
 ![SAP-AKTION](media/logic-apps-using-sap-connector/picture3.png)

4. Erstellen Sie eine HTTP-Antwort durch Hinzufügen einer neuen Aktion. Die Antwortnachricht muss aus der SAP-Ausgabe stammen.

5. Speichern Sie Ihre Logik-App, und testen Sie sie durch Senden eines IDoc über die URL des HTTP-Triggers.

6. Nachdem das IDoc gesendet wurde, warten Sie auf die Antwort der Logik-App.   

  > [!TIP]
  > Lesen Sie, wie Sie [Logik-Apps überwachen](../logic-apps/logic-apps-monitor-your-logic-apps.md).
  >
  >

7. Alles fertig. Sie haben nun eine funktionsfähige Logik-App, die den SAP-Connector nutzt. Sie können nun beginnen, die anderen angebotenen Funktionen zu untersuchen:
  - BAPI
  - RFC

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr zum [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- Erstellen einer [lokalen Verbindung](../logic-apps/logic-apps-gateway-connection.md) mit Logic Apps.


<!--HONumber=Jan17_HO3-->


