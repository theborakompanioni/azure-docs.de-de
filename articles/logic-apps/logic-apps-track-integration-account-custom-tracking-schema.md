---
title: Benutzerdefinierte Nachverfolgungsschemas | Microsoft-Dokumentation
description: Weitere Informationen zum benutzerdefinierten Nachverfolgungsschema
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: ef7681f1411e1ea5b3411b714d5fb9f5a8be4f17


---
# <a name="custom-tracking-schemas"></a>Benutzerdefinierte Nachverfolgungsschemas
Sie können ein benutzerdefiniertes Nachverfolgungsschema in Ihrem Azure-Integrationskonto als Unterstützung bei der Überwachung von B2B-Transaktionen (Business-to-Business) verwenden.

## <a name="custom-tracking-schema"></a>Benutzerdefiniertes Nachverfolgungsschema
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Eigenschaft | Typ | Beschreibung |
| --- | --- | --- |
| sourceType |   | Typ der Ausführungsquelle Zulässige Werte sind **Microsoft.Logic/workflows** und **custom** (benutzerdefiniert). (Obligatorisch) |
| Quelle |   | Wenn der Quelltyp **Microsoft.Logic/workflows** ist, müssen die Quellinformationen diesem Schema folgen. Wenn der Quelltyp **custom** ist, ist das Schema ist ein JToken. (Obligatorisch) |
| systemId | String | System-ID der Logik-App (Obligatorisch) |
| runId | String | Ausführungs-ID der Logik-App (Obligatorisch) |
| operationName | String | Name des Vorgangs (z.B. Aktion oder Trigger) (Obligatorisch) |
| repeatItemScopeName | String | Elementnamen wiederholen, wenn sich die Aktion innerhalb einer `foreach`/`until`-Schleife befindet. (Obligatorisch) |
| repeatItemIndex | Integer | Gibt an, ob sich die Aktion innerhalb einer `foreach`/`until`-Schleife befindet. Gibt den Index des Wiederholungselements an. (Obligatorisch) |
| trackingId | String | Überwachungs-ID zum Korrelieren der Nachrichten (Optional) |
| correlationId | String | Korrelations-ID zum Korrelieren der Nachrichten (Optional) |
| clientRequestId | String | Kann vom Client zum Korrelieren von Nachrichten ausgefüllt werden. (Optional) |
| eventLevel |   | Ebene des Ereignisses. (Obligatorisch) |
| eventTime |   | Zeit des Ereignisses im UTC-Format YYYY-MM-DDTHH:MM:SS.00000Z. (Obligatorisch) |
| recordType |   | Typ des Nachverfolgungsdatensatzes Der zulässige Wert ist **custom**. (Obligatorisch) |
| record |   | Benutzerdefinierter Datensatztyp Das zulässige Format ist JToken. (Obligatorisch) |

## <a name="b2b-protocol-tracking-schemas"></a>B2B-Protokollnachverfolgungsschemas
Informationen zu B2B-Protokollnachverfolgungsschemas finden Sie unter:
* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12-Nachverfolgungsschemas](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md)   
* Weitere Informationen zum [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)
* Weitere Informationen zum [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)



<!--HONumber=Jan17_HO3-->


