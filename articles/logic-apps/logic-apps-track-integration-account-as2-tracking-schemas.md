---
title: "AS2-Nachverfolgungsschemas für die B2B-Überwachung – Azure Logic Apps | Microsoft-Dokumentation"
description: "Verwenden Sie AS2-Nachverfolgungsschemas, um B2B-Nachrichten von Transaktionen in Ihrem Azure-Integrationskonto zu überwachen."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: padmavc
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 5913c81088724ef946ae147f4f3154fa6aefd22e
ms.openlocfilehash: e9c9cfda4dda1ec3f1b002016118bd49d540e90a
ms.lasthandoff: 03/01/2017


---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-to-monitor-success-errors-and-message-properties"></a>Starten oder Aktivieren der Nachverfolgung von AS2-Nachrichten und MDNs für die Überwachung von Erfolgs-, Fehler- und Nachrichteneigenschaften
Sie können diese AS2-Nachverfolgungsschemas in Ihrem Azure-Integrationskonto als Unterstützung bei der Überwachung von B2B-Transaktionen (Business-to-Buiness) verwenden:

* AS2-Nachrichten-Nachverfolgungsschema
* AS2-MDN-Nachverfolgungsschema

## <a name="as2-message-tracking-schema"></a>AS2-Nachrichten-Nachverfolgungsschema
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Eigenschaft | Typ | Beschreibung |
| --- | --- | --- |
| senderPartnerName | String | Name des Absenderpartners der AS2-Nachricht (Optional) |
| receiverPartnerName | String | Name des Empfängerpartners der AS2-Nachricht (Optional) |
| as2To | String | Name des Empfängers der AS2-Nachricht aus den Headern der AS2-Nachricht (Erforderlich) |
| as2From | String | Name des Absenders der AS2-Nachricht aus den Headern der AS2-Nachricht (Erforderlich) |
| agreementName | String | Name der AS2-Vereinbarung, nach der Nachrichten aufgelöst werden (Optional) |
| direction | String | Richtung des Nachrichtenflusses (Empfangen oder Senden) (Erforderlich) |
| messageId | String | ID der AS2-Nachricht aus den Headern der AS2-Nachricht (Optional) |
| dispositionType |String | MDN-Dispositionstypwert (Message Disposition Notification) (Optional) |
| fileName | String | Dateiname aus dem Header der AS2-Nachricht (Optional) |
| isMessageFailed |Boolean | Gibt an, ob die AS2-Nachricht fehlgeschlagen ist. (Erforderlich) |
| isMessageSigned | Boolean | Gibt an, ob die AS2-Nachricht signiert wurde. (Erforderlich) |
| isMessageEncrypted | Boolean | Gibt an, ob die AS2-Nachricht verschlüsselt wurde. (Erforderlich) |
| isMessageCompressed |Boolean | Gibt an, ob die AS2-Nachricht komprimiert wurde. (Erforderlich) |
| correlationMessageId | String | ID der AS2-Nachricht zum Korrelieren von Nachrichten mit MDNs (Optional) |
| incomingHeaders |Wörterbuch von JToken | Headerdetails von eingehenden AS2-Nachrichten (Optional) |
| outgoingHeaders |Wörterbuch von JToken | Headerdetails von ausgehenden AS2-Nachrichten (Optional) |
| isNrrEnabled | Boolean | Verwenden Sie den Standardwert, wenn der Wert unbekannt ist. (Erforderlich) |
| isMdnExpected | Boolean | Verwenden Sie den Standardwert, wenn der Wert unbekannt ist. (Erforderlich) |
| mdnType | Enum | Zulässige Werte sind **NotConfigured**, **Sync** und **Async**. (Erforderlich) |

## <a name="as2-mdn-tracking-schema"></a>AS2-MDN-Nachverfolgungsschema
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Eigenschaft | Typ | Beschreibung |
| --- | --- | --- |
| senderPartnerName | String | Name des Absenderpartners der AS2-Nachricht (Optional) |
| receiverPartnerName | String | Name des Empfängerpartners der AS2-Nachricht (Optional) |
| as2To | String | Name des Partners, der die AS2-Nachricht erhält (Erforderlich) |
| as2From | String | Name des Partners, der die AS2-Nachricht sendet (Erforderlich) |
| agreementName | String | Name der AS2-Vereinbarung, nach der Nachrichten aufgelöst werden (Optional) |
| direction |String | Richtung des Nachrichtenflusses (Empfangen oder Senden) (Erforderlich) |
| messageId | String | ID der AS2-Nachricht (Optional) |
| originalMessageId |String | ID der ursprünglichen AS2-Nachricht (Optional) |
| dispositionType | String | MDN-Dispositionstypwert (Optional) |
| isMessageFailed |Boolean | Gibt an, ob die AS2-Nachricht fehlgeschlagen ist. (Erforderlich) |
| isMessageSigned |Boolean | Gibt an, ob die AS2-Nachricht signiert wurde. (Erforderlich) |
| isNrrEnabled | Boolean | Verwenden Sie den Standardwert, wenn der Wert unbekannt ist. (Erforderlich) |
| statusCode | Enum | Zulässige Werte sind **Accepted**, **Rejected** und **AcceptedWithErrors**. (Erforderlich) |
| micVerificationStatus | Enum | Zulässige Werte sind **NotApplicable**, **Succeeded** und **Failed**. (Erforderlich) |
| correlationMessageId | String | Korrelations-ID. Die ursprüngliche Nachrichten-ID (die Nachrichten-ID der Nachricht, für die MDN konfiguriert ist). (Optional) |
| incomingHeaders | Wörterbuch von JToken | Gibt Headerdetails von eingehenden Nachrichten an. (Optional) |
| outgoingHeaders |Wörterbuch von JToken | Gibt Headerdetails von ausgehenden Nachrichten an. (Optional) |

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über das [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Informieren Sie sich über das [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md).   
* Informieren Sie sich über das [benutzerdefinierte B2B-Nachverfolgungsschema](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Informieren Sie sich über das [X12-Nachverfolgungsschema](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Informieren Sie sich über das [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

