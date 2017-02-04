---
title: Informationen zum Enterprise Integration Pack-Connector zum Decodieren von EDIFACT-Nachrichten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
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
ms.date: 08/15/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 8bf5fe5527e3de6f2e0950b3ee494b88cd1063a1
ms.openlocfilehash: 96dbedf2dc072795712e4b0a41c424e633263220


---
# <a name="get-started-with-decode-edifact-message"></a>Erste Schritte mit der Decodierung von EDIFACT-Nachrichten
Überprüfung von EDI- und partnerspezifischen Eigenschaften, Generieren eines XML-Dokuments für jeden Transaktionssatz und einer Bestätigung für die verarbeitete Transaktion

## <a name="create-the-connection"></a>Erstellen der Verbindung
### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Decodierung von EDIFACT-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](logic-apps-enterprise-integration-create-integration-account.md), zu [Partnern](logic-apps-enterprise-integration-partners.md) und zur [EDIFACT-Vereinbarung](logic-apps-enterprise-integration-edifact.md).

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von EDIFACT-Nachrichten her:
1. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](logic-apps-create-a-logic-app.md) finden Sie ein Beispiel.
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „EDIFACT“ in das Suchfeld ein.  Wählen Sie „Decode EDIFACT Message“ (EDIFACT-Nachricht decodieren).
   
    ![Suchen nach EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben.
   
    ![Erstellen eines Integrationskontos](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)  
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskonto erstellt](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  
5. Klicken Sie auf **Erstellen**
6. Beachten Sie, dass die Verbindung erstellt wurde.
   
    ![Verbindungsdetails des Integrationskontos](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  
7. Wählen Sie die zu decodierende EDIFACT-Flatfile-Nachricht aus.
   
    ![Pflichtfelder ausfüllen](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>Die EDIFACT-Decodierung führt die folgenden Schritte aus:
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




<!--HONumber=Jan17_HO3-->


