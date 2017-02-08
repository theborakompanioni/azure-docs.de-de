---
title: Informationen zum Enterprise Integration Pack-Connector zum Codieren von EDIFACT-Nachrichten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: 7f9feb7b9ae01c18712f14035ac8c56256cf4d26


---
# <a name="get-started-with-encode-edifact-message"></a>Erste Schritte mit der Codierung von EDIFACT-Nachrichten
Überprüfung von EDI- und partnerspezifischen Eigenschaften 

## <a name="create-the-connection"></a>Erstellen der Verbindung
### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Codierung von EDIFACT-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), zu [Partnern](logic-apps-enterprise-integration-partners.md) und zur [EDIFACT-Vereinbarung](../logic-apps/logic-apps-enterprise-integration-edifact.md).

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von EDIFACT-Nachrichten her:
1. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md) finden Sie ein Beispiel.
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „EDIFACT“ in das Suchfeld ein.  Wählen Sie entweder „Encode EDIFACT Message by agreement name“ (EDIFACT-Nachricht nach Vereinbarungsname codieren) oder „EDIFACT message by identities“ (EDIFACT-Nachricht nach Identitäten codieren) aus.
   
    ![Suchen nach EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben.
   
    ![Herstellen einer Integrationskontoverbindung](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskontoverbindung](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)
5. Klicken Sie auf **Erstellen**
6. Beachten Sie, dass die Verbindung erstellt wurde.
   
    ![Verbindungsdetails des Integrationskontos](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>Encode EDIFACT Message by agreement name (EDIFACT-Nachricht nach Vereinbarungsname codieren)
1. Geben Sie den EDIFACT-Vereinbarungsnamen und die zu codierende XML-Nachricht an.
   
   ![Pflichtfelder ausfüllen](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>Encode EDIFACT Message by identities (EDIFACT-Nachricht nach Identitäten codieren)
1. Geben Sie die Senderqualifizierer und -bezeichner und Empfängerqualifizierer und -bezeichner wie in der EDIFACT-Vereinbarung konfiguriert an.  Wählen Sie die zu codierende XML-Nachricht aus.
   
    ![Pflichtfelder ausfüllen](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-does-following"></a>Die EDIFACT-Codierung führt die folgenden Schritte aus:
* Auflösen der Vereinbarung durch Abgleich von Senderqualifizierer und -bezeichner sowie von Empfängerqualifizierer und -bezeichner
* Serialisieren des EDI-Austauschs, Konvertieren von XML-codierten Nachrichten in EDI-Transaktionssätze im Austausch
* Anwendung von Header- und Nachspannsegmenten für den Transaktionssatz
* Generierung einer Austauschkontrollnummer, einer Gruppenkontrollnummer und einer Transaktionssatz-Kontrollnummer für jeden ausgehenden Austausch
* Ersetzung von Trennzeichen in den Nutzlastdaten
* Überprüfung von EDI- und partnerspezifischen Eigenschaften
  * Schemaüberprüfung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
  * EDI-Überprüfung für Transaktionssatz-Datenelemente
  * Erweiterte Überprüfung für Transaktionssatz-Datenelemente
* Generieren eines XML-Dokuments für jeden Transaktionssatz
* Generieren einer technischen Bestätigung (Kontrollbestätigung) und/oder einer Funktionsbestätigung (sofern konfiguriert)
  * Als technische Bestätigung kennzeichnet die Kontrollbestätigung den Empfang eines Austauschvorgangs.
  * Als Funktionsbestätigung dient die Kontrollbestätigung dazu, die Annahme oder Ablehnung des empfangenen Austauschs, der Gruppe oder der Nachricht mit einer Liste von Fehlern oder nicht unterstützter Funktionen anzugeben.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 




<!--HONumber=Jan17_HO3-->


