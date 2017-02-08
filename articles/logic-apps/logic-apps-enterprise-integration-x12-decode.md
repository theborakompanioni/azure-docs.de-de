---
title: Informationen zum Enterprise Integration Pack-Connector zum Decodieren von X12-Nachrichten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
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
ms.date: 08/15/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: b6ce911b8435f3d6ed1c12c78ba81abe180346a2


---
# <a name="get-started-with-decode-x12-message"></a>Erste Schritte mit der Decodierung von X12-Nachrichten
Überprüfung von EDI- und partnerspezifischen Eigenschaften, Generieren eines XML-Dokuments für jeden Transaktionssatz und einer Bestätigung für die verarbeitete Transaktion

## <a name="create-the-connection"></a>Erstellen der Verbindung
### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Decodierung von X12-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) sowie zu [Partnern](../logic-apps/logic-apps-enterprise-integration-partners.md) und zur [X12-Vereinbarung](../logic-apps/logic-apps-enterprise-integration-x12.md).

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von X12-Nachrichten her:
1. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md) finden Sie ein Beispiel.
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „x12“ in das Suchfeld ein.  Wählen Sie „X12 – Decode X12 Message“ (X12 – X12-Nachricht decodieren) aus.
   
    ![Suchen von x12](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto erstellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Integrationskontoverbindung](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)     
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskontoverbindung erstellt](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 
5. Wählen Sie **Erstellen**
6. Beachten Sie, dass die Verbindung erstellt wurde.
   
    ![Verbindungsdetails des Integrationskontos](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 
7. Wählen Sie die zu decodierende X12-Flatfile-Nachricht aus.
   
    ![Pflichtfelder ausfüllen](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>Die X12-Decodierung führt die folgenden Schritte aus:
* Überprüfen des Umschlags anhand der Handelspartnervereinbarung
* Generieren eines XML-Dokuments für jeden Transaktionssatz
* Überprüfen von EDI- und partnerspezifischen Eigenschaften
  * Strukturelle EDI-Überprüfung und erweiterte Schemaüberprüfung
  * Überprüfung der Struktur des Austauschumschlags
  * Schemaüberprüfung des Umschlags anhand des Kontrollschemas
  * Schemaüberprüfung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
  * EDI-Überprüfung für Transaktionssatz-Datenelemente 
* Sicherstellen, dass der Austausch, die Gruppe und die Transaktionssatz-Kontrollnummern keine Duplikate sind
  * Überprüfen der Austauschkontrollnummer in Bezug auf zuvor empfangene Austauschvorgänge
  * Überprüfen der Gruppenkontrollnummer in Bezug auf andere Gruppenkontrollnummern im Austausch
  * Überprüfen der Transaktionssatz-Kontrollnummer in Bezug auf andere Transaktionssatz-Kontrollnummern in dieser Gruppe
* Konvertieren des gesamten Austauschs in XML 
  * Austausch in Transaktionssätze trennen – Transaktionssatz bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die X12-Decodierung nur diese Transaktionssätze an.
  * Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument.  Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die X12-Decodierung den gesamten Austausch an.
  * Austausch beibehalten – Transaktionssätze bei Fehler anhalten: Erstellt ein XML-Dokument für den gesamten Batchaustausch. Die X12-Decodierung hält nur die Transaktionssätze an, die die Überprüfung nicht bestehen, während alle weiteren Transaktionssätze verarbeitet werden.
  * Austausch beibehalten – Austausch bei Fehler anhalten: Erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die X12-Decodierung den gesamten Austausch an. 
* Generieren einer technischen Bestätigung und/oder einer Funktionsbestätigung (sofern konfiguriert)
  * Als Ergebnis der Headerüberprüfung wird eine technische Bestätigung generiert. Die technische Bestätigung meldet den Status der Verarbeitung eines Austauschheaders und -nachspanns durch den Adressempfänger.
  * Eine Funktionsbestätigung wird als Ergebnis der Textüberprüfung generiert. Die Funktionsbestätigung meldet jeden Fehler, der bei der Verarbeitung des empfangenen Dokuments aufgetreten ist.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 




<!--HONumber=Jan17_HO3-->


