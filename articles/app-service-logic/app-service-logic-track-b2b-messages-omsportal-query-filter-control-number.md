---
title: Nachverfolgen von B2B-Nachrichten im OMS-Portal | Microsoft-Dokumentation
description: 'Gewusst wie: Nachverfolgen von B2B-Nachrichten im OMS-Portal'
author: padmavc
manager: erikre
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 6e56dae0ab30612a7794bd39e6eba2f84bcecc19
ms.openlocfilehash: eae37a357e3e1c00310c7c638d3164118403ee9f


---
# <a name="create-a-query-in-oms-portal"></a>Erstellen einer Abfrage im OMS-Portal 
Schritte zum Erstellen einer Abfrage, die die Daten für eine bestimmte Austauschkontrollnummer filtert

## <a name="prerequisites"></a>Voraussetzungen

Aktivieren Sie die Diagnose für Ihr [Integrationskonto](app-service-logic-monitor-b2b-message.md) und [Logik-Apps](app-service-logic-monitor-your-logic-apps.md#azure-diagnostics-and-alerts) mit X12-Connectors, um umfangreichere Detail- und Debuginformationen zu erhalten.  Führen Sie die [hier](app-service-logic-track-b2b-messages-omsportal.md) angegebenen Schritte zum Veröffentlichen von Diagnosedaten im OMS-Portal aus.

## <a name="create-a-query-to-search-data-for-a-specific-interchange-control-number"></a>Erstellen einer Abfrage zum Suchen von Daten für eine bestimmte Austauschkontrollnummer

1. Auswählen von **Protokollsuche** auf der Startseite  
![Auswählen der Protokollsuche“.](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

2. Geben Sie in das Suchfeld **Type="AzureDiagnostics"** ein. Alle Daten werden abgerufen.  Klicken Sie zum Filtern von Daten auf **Hinzufügen**.  
![Auswählen der Abfrage](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

3. Geben Sie **interchange** ein, wählen Sie **event_record_messageProperties_interchangeControlNumber_s** aus, und klicken Sie auf **Hinzufügen**.  
![Auswählen der Kontrollnummer](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query2.png)

4. Wählen Sie die Kontrollnummer aus, anhand der Sie Daten filtern möchten, und klicken Sie auf **Anwenden**.  
![Auswählen der Kontrollnummer](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query3.png)

5. Sie können die Abfragen suchen, die zum Filtern von Daten für die ausgewählte Kontrollnummer erstellt wurde.   
![Auswählen der Kontrollnummer](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query4.png)

6. Geben Sie der Abfrage einen Namen, und speichern Sie sie.   
![Auswählen der Kontrollnummer](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query5.png) 

7. Wählen Sie **Favoriten** aus, um die Abfrage anzuzeigen und in künftigen Suchen zu verwenden.  
![Auswählen der Kontrollnummer](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query7.png)

8. Die Abfrage kann aktualisiert werden, um eine neue Austauschkontrollnummer zu suchen.  
![Auswählen der Kontrollnummer](./media/app-service-logic-track-b2b-messages-omsportal-query-filter-control-number/query6.png) 


## <a name="next-steps"></a>Nächste Schritte
[Benutzerdefiniertes Nachverfolgungsschema](app-service-logic-track-integration-account-custom-tracking-shema.md "Learn about Custom Tracking Schema")   
[AS2-Nachverfolgungsschema](app-service-logic-track-integration-account-as2-tracking-shemas.md "Learn about AS2 Tracking Schema")    
[X12-Nachverfolgungsschema](app-service-logic-track-integration-account-x12-tracking-shemas.md "Learn about X12 Tracking Schema")  
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Learn about Enterprise Integration Pack") 


<!--HONumber=Nov16_HO3-->


