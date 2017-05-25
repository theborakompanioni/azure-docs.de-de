---
title: 'Logic Apps B2B EDIFACT Decodieren Beheben UNH2.5: Azure Logic Apps | Microsoft-Dokumentation'
description: Azure Logik-Apps B2B EDIFACT Decodieren Beheben UNH2.5
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
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 90c9b65062acbe78196e220ba167997d44cdce90
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="how-to-handle-edifact-documents-having-unh25-segment"></a>Verarbeiten von EDIFACT-Dokumenten mit UNH2.5-Segmenten
Wenn das Feld „UNH2.5“ im EDIFACT-Dokument vorhanden ist, wird es zur Schemasuche verwendet. 

Beispiel: **EAN008** ist das UNH-Feld in der EDIFACT-Nachricht.  
UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'  

Schritte zum Verarbeiten der Nachricht 
1. Aktualisieren des Schemas
2. Überprüfen der Vereinbarungseinstellungen  

## <a name="update-the-schema"></a>Aktualisieren des Schemas
Um die Nachricht zu verarbeiten, müssen Sie ein Schema mit dem UNH2.5-Stammknotennamen bereitstellen.  Bei diesem Beispiel ist der Schemastammname **EFACT_D03B_ORDERS_EAN008**.  

Für alle D03B_ORDERS mit einem anderen UNH2.5-Segment müssen Sie ein eigenes Schema bereitstellen.  

## <a name="add-schema-to-the-edifact-agreement"></a>Hinzufügen des Schemas zur EDIFACT-Vereinbarung
### <a name="edifact-decode"></a>Decodieren von EDIFACT-Nachrichten
Um die eingehende Nachricht zu decodieren, konfigurieren Sie das Schema in den Empfangseinstellungen der EDIFACT-Vereinbarung.
1. Fügen Sie das Schema dem Integrationskonto hinzu.    
2. Konfigurieren Sie das Schema in den Empfangseinstellungen der EDIFACT-Vereinbarung. 
3. Wählen Sie die EDIFACT-Vereinbarung aus, und klicken Sie auf **Als JSON bearbeiten**.  Fügen Sie den UNH2.5-Wert in der Empfangsvereinbarung dem Abschnitt **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png) hinzu.

### <a name="edifact-encode"></a>Codieren von EDIFACT-Nachrichten
Um die eingehende Nachricht zu codieren, konfigurieren Sie das Schema in den Sendeeinstellungen der EDIFACT-Vereinbarung.
1. Fügen Sie das Schema dem Integrationskonto hinzu.    
2. Konfigurieren Sie das Schema in den Sendeeinstellungen der EDIFACT-Vereinbarung. 
3. Wählen Sie die EDIFACT-Vereinbarung aus, und klicken Sie auf **Als JSON bearbeiten**.  Fügen Sie den UNH2.5-Wert in der Sendevereinbarung dem Abschnitt **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png) hinzu.

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Integrationskontovereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  
