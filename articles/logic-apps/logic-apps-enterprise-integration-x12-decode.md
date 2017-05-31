---
title: "Decodieren von X12-Nachrichten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Überprüfen von EDI und Generieren von Bestätigungen mit dem X12-Nachrichtendecoder im Enterprise Integration Pack für Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6408fdf494035b37e0025dd8439e800a80bffb4e
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="decode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Decodieren von X12-Nachrichten für Azure Logic Apps mit dem Enterprise Integration Pack

Mit dem Connector zum Decodieren von X12-Nachrichten können Sie den Umschlag für eine Handelspartnervereinbarung sowie EDI- und partnerspezifische Eigenschaften überprüfen, Austauschvorgänge in Transaktionssätze aufteilen oder gesamte Austauschvorgänge beibehalten und Bestätigungen für verarbeitete Transaktionen generieren. Um diesen Connector verwenden zu können, müssen Sie ihn einem vorhandenen Trigger in Ihrer Logik-App hinzufügen.

## <a name="before-you-start"></a>Vorbereitung

Sie benötigen Folgendes:

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein bereits definiertes und mit Ihrem Azure-Abonnement verknüpftes [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md). Für die Verwendung des Connectors „X12-Nachricht decodieren“ ist ein Integrationskonto erforderlich.
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind.
* Eine bereits in Ihrem Integrationskonto definierte [X12-Vereinbarung](logic-apps-enterprise-integration-x12.md).

## <a name="decode-x12-messages"></a>Decodieren von X12-Nachrichten

1. [Erstellen Sie eine Logik-App](logic-apps-create-a-logic-app.md).

2. Da der Connector „X12-Nachricht decodieren“ über keine Trigger verfügt, müssen Sie einen Trigger zum Starten Ihrer Logik-App hinzufügen (beispielsweise einen Anforderungstrigger). Fügen Sie im Logik-App-Designer einen Trigger hinzu, und fügen Sie anschließend Ihrer Logik-App eine Aktion hinzu.

3.    Geben Sie „x12“ als Filter in das Suchfeld ein. Wählen Sie **X12 – X12-Nachricht decodieren** aus.
   
    ![Nach „x12“ suchen](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Falls Sie noch keine Verbindungen mit Ihrem Integrationskonto erstellt haben, werden Sie aufgefordert, eine solche Verbindung zu erstellen. Benennen Sie die Verbindung, und wählen Sie das Integrationskonto aus, mit dem Sie eine Verbindung herstellen möchten. 

    ![Details zur Integrationskontoverbindung angeben](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

    | Eigenschaft | Details |
    | --- | --- |
    | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
    | Integrationskonto* |Geben Sie einen Namen für Ihr Integrationskonto ein. Achten Sie darauf, dass sich Ihr Integrationskonto und Ihre Logik-App am gleichen Azure-Standort befinden. |

5.    Am Ende sollten Ihre Verbindungsdetails in etwa wie im folgenden Beispiel aussehen. Wählen Sie **Erstellen** aus, um die Verbindungserstellung abzuschließen.
   
    ![Verbindungsdetails des Integrationskontos](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Wählen Sie nach der Verbindungserstellung wie in diesem Beispiel dargestellt die zu decodierende X12-Flatfilenachricht aus.

    ![Integrationskontoverbindung erstellt](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Beispiel:

    ![Zu decodierende X12-Flatfilenachricht auswählen](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a>Details des Decodierens von X12-Nachrichten

Der Connector „X12-Nachricht decodieren“ führt folgende Aufgaben aus:

* Überprüfen des Umschlags anhand der Handelspartnervereinbarung
* Überprüfung von EDI- und partnerspezifischen Eigenschaften
  * Strukturelle EDI-Überprüfung und erweiterte Schemaüberprüfung
  * Überprüfung der Struktur des Austauschumschlags
  * Schemaüberprüfung des Umschlags anhand des Kontrollschemas
  * Schemaüberprüfung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
  * EDI-Überprüfung für Transaktionssatz-Datenelemente 
* Sicherstellen, dass der Austausch, die Gruppe und die Transaktionssatz-Kontrollnummern keine Duplikate sind
  * Überprüfen der Austauschkontrollnummer in Bezug auf zuvor empfangene Austauschvorgänge
  * Überprüfen der Gruppenkontrollnummer in Bezug auf andere Gruppenkontrollnummern im Austausch
  * Überprüfen der Transaktionssatz-Kontrollnummer in Bezug auf andere Transaktionssatz-Kontrollnummern in dieser Gruppe
* Trennt den Austausch in Transaktionssätze oder behält den gesamten Austausch bei:
  * Austausch in Transaktionssätze trennen – Transaktionssätze bei Fehler anhalten: Trennt jeden Austausch in Transaktionssätze und analysiert jeden Transaktionssatz. 
  Die Aktion „X12 decodieren“ gibt nur die Transaktionssätze, die die Überprüfung nicht bestehen, in `badMessages` und die restlichen Transaktionssätze in `goodMessages` aus.
  * Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten: Trennt jeden Austausch in Transaktionssätze und analysiert jeden Transaktionssatz. 
  Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, gibt die Aktion „X12 decodieren“ alle Transaktionssätze in diesem Austausch in `badMessages` aus.
  * Austausch beibehalten – Transaktionssätze bei Fehler anhalten: Behält den Austausch bei und verarbeitet den gesamten Batchaustausch. 
  Die Aktion „X12 decodieren“ gibt nur die Transaktionssätze, die die Überprüfung nicht bestehen, in `badMessages` und die restlichen Transaktionssätze in `goodMessages` aus.
  * Austausch beibehalten – Austausch bei Fehler anhalten: Behält den Austausch bei und verarbeitet den gesamten Batchaustausch. 
  Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, gibt die Aktion „X12 decodieren“ alle Transaktionssätze in diesem Austausch in `badMessages` aus. 
* Generieren einer technischen Bestätigung und/oder einer Funktionsbestätigung (sofern konfiguriert)
  * Als Ergebnis der Headerüberprüfung wird eine technische Bestätigung generiert. Die technische Bestätigung meldet den Status der Verarbeitung eines Austauschheaders und -nachspanns durch den Adressempfänger.
  * Eine Funktionsbestätigung wird als Ergebnis der Textüberprüfung generiert. Die Funktionsbestätigung meldet jeden Fehler, der bei der Verarbeitung des empfangenen Dokuments aufgetreten ist.

## <a name="view-the-swagger"></a>Anzeigen von Swagger
Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/x12/). 

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 


