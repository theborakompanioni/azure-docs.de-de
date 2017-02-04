---
title: Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal mit einer Abfrage | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie B2B-Nachrichten im Operations Management Suite-Portal nachverfolgen, indem Sie eine Abfrage verwenden.
author: padmavc
manager: anneta
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
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: df6beaaa5de45acfa850049721797fc43f4c9ed7


---
# <a name="track-b2b-messages-in-the-operations-management-suite-portal-by-using-a-query"></a>Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal mit einer Abfrage
Zum Nachverfolgen von B2B-Nachrichten (Business-to-Business) im Operations Management Suite-Portal können Sie eine Abfrage erstellen, mit der Daten nach einer bestimmten Austauschkontrollnummer gefiltert werden.

## <a name="prerequisites"></a>Voraussetzungen

Um das Debuggen durchzuführen und ausführlichere Diagnoseinformationen zu erhalten, aktivieren Sie die Diagnose in Ihrem [Integrationskonto](logic-apps-monitor-b2b-message.md) für die [Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics-and-alerts) mit X12-Connectors. Führen Sie anschließend die Schritte zum [Veröffentlichen von Diagnosedaten](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) im Operations Management Suite-Portal aus.

## <a name="to-create-a-query-to-search-for-a-specific-interchange-control-number"></a>So erstellen Sie eine Abfrage zum Suchen nach einer bestimmten Austauschkontrollnummer

1. Wählen Sie auf der Startseite die Option **Protokollsuche**.  
![Auswählen der Protokollsuche“.](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

2. Geben Sie im Suchfeld **Type="AzureDiagnostics"** ein. Wählen Sie **Hinzufügen**, um die Daten zu filtern.  
![Auswählen der Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

3. Geben Sie **interchange** ein, wählen Sie **event_record_messageProperties_interchangeControlNumber_s** aus, und wählen Sie dann **Hinzufügen**.  
![Filter hinzufügen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query2.png)

4. Wählen Sie die Kontrollnummer aus, anhand der Sie Daten filtern möchten, und wählen Sie anschließend die Option **Anwenden**.  
![Anhand der Kontrollnummer suchen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query3.png)

5. Suchen Sie nach der Abfrage, die Sie erstellt haben, um anhand der ausgewählten Kontrollnummer nach Daten zu suchen.   
![Suchen der Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query4.png)

6. Geben Sie einen Namen für die Abfrage ein, und speichern Sie sie.   
![Speichern der Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query5.png)

7. Wählen Sie die Option **Favoriten**, um die Abfrage anzuzeigen und für zukünftige Suchen zu verwenden.  
![Abfrage anzeigen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query7.png)

8. Sie können die Abfrage aktualisieren, um nach einer neuen Austauschkontrollnummer zu suchen.  
![Aktualisieren der Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query6.png)


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [benutzerdefinierten Nachverfolgungsschemas](logic-apps-track-integration-account-custom-tracking-schema.md)   
* Weitere Informationen zu [AS2-Nachverfolgungsschemas](logic-apps-track-integration-account-as2-tracking-schemas.md)    
* Informieren Sie sich über das [X12-Nachverfolgungsschema](logic-apps-track-integration-account-x12-tracking-schema.md).  
* Informieren Sie sich über das [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).



<!--HONumber=Jan17_HO3-->


