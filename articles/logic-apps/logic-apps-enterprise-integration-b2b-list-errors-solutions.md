---
title: "Logic Apps-B2B-Liste von Fehlern und Lösungen: Azure App Service | Microsoft-Dokumentation"
description: "Logic Apps-B2B-Liste von Fehlern und Lösungen"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 1865d75f1b4c2aa18d5a3130f639572d19563b3e
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---

# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a>Logic Apps-B2B-Liste von Fehlern und Lösungen  
Dieser Artikel hilft Ihnen bei der Problembehandlung von Fehlern, die möglicherweise in Logic Apps-B2B-Szenarien auftreten können, und schlägt entsprechende Aktionen zur Korrektur dieser Fehler vor.


## <a name="agreement-resolution"></a>Vereinbarungsauflösung

### <a name="no-agreement-found"></a>*Keine Vereinbarung gefunden 

|   |   |  
|---|---|
| Fehlerbeschreibung | Keine Vereinbarung mit Vereinbarungsauflösungsparametern gefunden|    
| Benutzeraktion | Die Vereinbarung sollte dem Integrationskonto mit vereinbarten Geschäftsidentitäten hinzugefügt werden.</br> Die Geschäftsidentitäten sollten mit den Eingabemeldungs-IDs übereinstimmen.|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a>*Keine Vereinbarung mit Identitäten gefunden

|   |   | 
|---|---|
| Fehlerbeschreibung | Keine Vereinbarung mit Identitäten gefunden: 'AS2Identity'::'Partner1' und 'AS2Identity'::'Partner3'| 
| Benutzeraktion | Ungültiges AS2-From oder AS2-To für Vereinbarung konfiguriert. </br> Korrektur von AS2-From- oder AS2-To-Header der AS2-Nachricht oder Vereinbarung zur Übereinstimmung mit AS2-IDs in AS2-Nachrichtenheadern mit Vereinbarungskonfigurationen |
|   |   |     

## <a name="as2"></a>AS2

### <a name="-missing-as2-message-headers"></a>* AS2-Nachrichtenheader fehlen  

|   |   |  
|---|---|
| Fehlerbeschreibung| Ungültige AS2-Header. Einer der „AS2-To“- oder „AS2-From“- Header ist leer| 
| Benutzeraktion | Eine AS2-Nachricht ohne AS2-From oder AS2-To bzw. beide Header wurde empfangen. </br> Überprüfen von AS2-From- und AS2-To-Header der AS2-Nachricht und Korrektur anhand der Vereinbarungskonfiguration |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a>* Text und Header von AS2-Nachricht fehlen    

|   |   |  
|---|---|
| Fehlerbeschreibung| Der Inhalt der Anforderung ist NULL oder leer | 
| Benutzeraktion | Es wurde eine AS2-Nachricht ohne Nachrichtentext empfangen |
|  |  | 

### <a name="-as2-message-decryption-failure"></a>* Entschlüsselungsfehler bei AS2-Nachricht

|   |   | 
|---|---|
| Fehlerbeschreibung |  [processed/Error: decryption-failed] | 
| Benutzeraktion | Der AS2-Nachricht vor dem Senden an Partner @base64ToBinary hinzufügen 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a>* MDN-Entschlüsselungsfehler

|   |   | 
|---|---|
| Fehlerbeschreibung |  [processed/Error: decryption-failed] | 
| Benutzeraktion | MDN vor dem Senden an Partner @base64ToBinary hinzufügen 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a>*Signaturzertifikat fehlt

|   |   |  
|---|---|
| Fehlerbeschreibung| Das Signaturzertifikat wurde nicht für die AS2-Partei konfiguriert. </br> AS2-From: partner1 AS2-To: partner2 | 
| Benutzeraktion | Konfigurieren von AS2-Vereinbarungseinstellungen mit richtigem Zertifikat für Signatur |
|  |  | 

## <a name="x12-and-edifact"></a>X12 und EDIFACT

### <a name="-leading-or-trailing-space-found"></a>*Vorangestelltes oder nachstehendes Leerzeichen gefunden    
    
|   |   | 
|---|---|
| Fehlerbeschreibung | Fehler bei der Analyse. Der Edifact-Transaktionssatz mit ID „123456“ im Austausch (ohne Gruppe) mit ID „987654“, Absender-ID „Partner1“ und Empfänger-ID „Partner2“ wird aufgrund folgender Fehler angehalten: Vorangestellte/nachstehende Trennlinie gefunden |
| Benutzeraktion | Die zu konfigurierenden Vereinbarungseinstellungen erlauben führende und nachstehende Leerzeichen. </br> Vereinbarungseinstellungen bearbeiten, sodass führende und nachstehende Leerzeichen erlaubt sind |
|   |   |

![Leerzeichen erlauben](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-the-agreement"></a>* Duplikatüberprüfung wurde in der Vereinbarung aktiviert

|   |   | 
|---|---| 
| Fehlerbeschreibung | Doppelte Kontrollnummer |
| Benutzeraktion | Dieser Fehler zeigt an, dass die empfangene Nachricht über doppelte Kontrollnummern verfügt. </br> Korrektur der Kontrollnummer und erneutes Senden der Nachricht |
|   |   |

### <a name="-missing-schema-in-the-agreement"></a>* Schema in der Vereinbarung fehlt

|   |   | 
|---|---| 
| Fehlerbeschreibung | Fehler bei der Analyse. Der X12-Transaktionssatz mit ID „564220001“ in der Funktionsgruppe mit ID „56422“, in Austausch mit ID „000056422“, mit Absender-ID „12345678       “, Empfänger-ID „87654321       “ wird mit folgender Fehlermeldung angehalten: „Die Nachricht weist einen unbekannten Dokumenttyp auf und konnte in keines der vorhandenen Schemas aufgelöst werden, die in der Vereinbarung konfiguriert wurden.“ |
| Benutzeraktion | Konfigurieren des Schemas in den Vereinbarungseinstellungen  |
|   |   |

### <a name="-incorrect-schema-in-the-agreement"></a>* Falsches Schema in der Vereinbarung

|   |   | 
|---|---| 
| Fehlerbeschreibung | Die Nachricht weist einen unbekannten Dokumenttyp auf und konnte in keines der vorhandenen Schemas aufgelöst werden, die in der Vereinbarung konfiguriert wurden. |
| Benutzeraktion | Konfigurieren des korrekten Schemas in den Vereinbarungseinstellungen  |
|   |   |

## <a name="flat-file"></a>Flatfile

### <a name="-input-message-with-no-body"></a>* Eingabenachricht ohne Text

|   |   | 
|---|---|
| Fehlerbeschreibung | InvalidTemplate. Verarbeitung der Vorlagensprachausdrücke in Aktion „Flat_File_Decoding“, Eingaben in Zeile „1“ und Spalte „1902“ nicht möglich: „Erforderliche Eigenschaft 'Content' erwartet einen Wert, erhält jedoch NULL. Pfad ‚.‘“. |
| Benutzeraktion | Dieser Fehler weist darauf hin, dass die Eingabenachricht keinen Text enthält |
|   |   | 

## <a name="learn-more"></a>Weitere Informationen
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
