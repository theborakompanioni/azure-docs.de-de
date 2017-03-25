---
title: "Decodieren von EDIFACT-Nachrichten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Überprüfen von EDI und Generieren von XML-Code für Transaktionssätze mit dem EDIFACT-Nachrichtendecoder im Enterprise Integration Pack für Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 176963837f4f3fc8b89e31000ef8722ef3258b11
ms.lasthandoff: 03/10/2017


---

# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Decodieren von EDIFACT-Nachrichten für Azure Logic Apps mit dem Enterprise Integration Pack

Mit dem Connector „EDIFACT-Nachricht decodieren“ können Sie EDI- und partnerspezifische Eigenschaften überprüfen sowie ein XML-Dokument für jeden Transaktionssatz und eine Bestätigung für verarbeitete Transaktionen generieren. Um diesen Connector verwenden zu können, müssen Sie ihn einem vorhandenen Trigger in Ihrer Logik-App hinzufügen.

## <a name="before-you-start"></a>Vorbereitung

Sie benötigen Folgendes:

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein bereits definiertes und mit Ihrem Azure-Abonnement verknüpftes [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md). Für die Verwendung des Connectors „EDIFACT-Nachricht decodieren“ ist ein Integrationskonto erforderlich. 
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind.
* Eine bereits in Ihrem Integrationskonto definierte [EDIFACT-Vereinbarung](logic-apps-enterprise-integration-edifact.md).

## <a name="decode-edifact-messages"></a>Decodierung von EDIFACT-Nachrichten

1. [Erstellen Sie eine Logik-App](logic-apps-create-a-logic-app.md).

2. Da der Connector „EDIFACT-Nachricht decodieren“ über keine Trigger verfügt, müssen Sie einen Trigger zum Starten Ihrer Logik-App hinzufügen (beispielsweise einen Anforderungstrigger). Fügen Sie im Logik-App-Designer einen Trigger hinzu, und fügen Sie anschließend Ihrer Logik-App eine Aktion hinzu.

3.    Geben Sie im Suchfeld den Begriff „EDIFACT“ als Filter ein. Wählen Sie **EDIFACT-Nachricht decodieren** aus.
   
    ![Suchen nach EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Falls Sie noch keine Verbindungen mit Ihrem Integrationskonto erstellt haben, werden Sie aufgefordert, eine solche Verbindung zu erstellen. Benennen Sie die Verbindung, und wählen Sie das Integrationskonto aus, mit dem Sie eine Verbindung herstellen möchten.
   
    ![Erstellen eines Integrationskontos](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

    | Eigenschaft | Details |
    | --- | --- |
    | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
    | Integrationskonto* |Geben Sie einen Namen für Ihr Integrationskonto ein. Achten Sie darauf, dass sich Ihr Integrationskonto und Ihre Logik-App am gleichen Azure-Standort befinden. |

4.    Wenn Sie mit der Verbindungserstellung fertig sind, wählen Sie **Erstellen**. Ihre Verbindungsdetails sollten in etwa wie im folgenden Beispiel aussehen:

    ![Details des Integrationskontos](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Wählen Sie nach der Verbindungserstellung wie in diesem Beispiel dargestellt die zu decodierende EDIFACT-Flatfilenachricht aus.

    ![Integrationskontoverbindung erstellt](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Beispiel:

    ![Wählen Sie die zu decodierende EDIFACT-Flatfilenachricht aus.](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>Details des EDIFACT-Decoders

Der Connector „EDIFACT-Nachricht decodieren“ führt folgende Aufgaben aus: 

* Auflösen der Vereinbarung durch Abgleich von Senderqualifizierer und -bezeichner sowie von Empfängerqualifizierer und -bezeichner
* Aufteilen mehrerer Austauschvorgänge in einer einzelnen Nachricht in separate Vorgänge
* Überprüfen des Umschlags anhand der Handelspartnervereinbarung
* Disassemblieren des Austauschs
* Überprüfung von EDI- und partnerspezifischen Eigenschaften, einschließlich:
  * Überprüfung der Struktur des Austauschumschlags
  * Schemaüberprüfung des Umschlags anhand des Kontrollschemas
  * Schemaüberprüfung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
  * EDI-Überprüfung für Transaktionssatz-Datenelemente
* Sicherstellen, dass der Austausch, die Gruppe und die Transaktionssatz-Kontrollnummern keine Duplikate sind (falls konfiguriert) 
  * Überprüfen der Austauschkontrollnummer in Bezug auf zuvor empfangene Austauschvorgänge 
  * Überprüfen der Gruppenkontrollnummer in Bezug auf andere Gruppenkontrollnummern im Austausch 
  * Überprüfen der Transaktionssatz-Kontrollnummer in Bezug auf andere Transaktionssatz-Kontrollnummern in dieser Gruppe
* Generieren eines XML-Dokuments für jeden Transaktionssatz
* Konvertieren des gesamten Austauschs in XML 
  * Austausch in Transaktionssätze trennen – Transaktionssatz bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die EDIFACT-Decodierung nur diese Transaktionssätze an. 
  * Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument.  Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die EDIFACT-Decodierung den gesamten Austausch an.
  * Austausch beibehalten – Transaktionssätze bei Fehler anhalten: Erstellt ein XML-Dokument für den gesamten Batchaustausch. Die EDIFACT-Decodierung hält nur die Transaktionssätze an, die die Überprüfung nicht bestehen, während alle weiteren Transaktionssätze verarbeitet werden.
  * Austausch beibehalten – Austausch bei Fehler anhalten: Erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die EDIFACT-Decodierung den gesamten Austausch an. 
* Generieren einer technischen Bestätigung (Kontrollbestätigung) und/oder einer Funktionsbestätigung (sofern konfiguriert)
  * Eine technische Bestätigung (Kontrollbestätigung) meldet die Ergebnisse einer Syntaxüberprüfung des vollständigen empfangenen Austauschs.
  * Eine Funktionsbestätigung bestätigt das Akzeptieren oder Ablehnen eines empfangenen Austauschs oder einer Gruppe.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 


