---
title: Codieren von EDIFACT-Nachrichten in Azure Logic Apps | Microsoft-Dokumentation
description: Verwendung des im Enterprise Integration Pack enthaltenen EDIFACT-Encoders mit Logic Apps
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
ms.date: 01/27/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 5bc7011d7b0d22a7f8c11a2fee8d002c24d3467c
ms.openlocfilehash: 94d120cd8a5e33733ecc39af96d2719ad59ab090


---
# <a name="get-started-with-encode-edifact-message"></a>Erste Schritte mit der Codierung von EDIFACT-Nachrichten
Überprüfen von EDI- und partnerspezifischen Eigenschaften 

## <a name="prereqs"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Codierung von EDIFACT-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), zu [Partnern](logic-apps-enterprise-integration-partners.md) und zur [EDIFACT-Vereinbarung](../logic-apps/logic-apps-enterprise-integration-edifact.md).

## <a name="encode-edifact-messages"></a>Codierung von EDIFACT-Nachrichten
1. [Erstellen Sie eine Logic App.](../logic-apps/logic-apps-create-a-logic-app.md)
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „EDIFACT“ in das Suchfeld ein.  Wählen Sie entweder „EDIFACT-Nachricht nach Vereinbarungsnamen codieren“ oder „EDIFACT-Nachricht nach Identitäten codieren“ aus:
   
    ![Suchen nach EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Herstellen einer Integrationskontoverbindung](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden:
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskontoverbindung](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)
5. Klicken Sie auf **Erstellen**.
6. Beachten Sie, dass die Verbindung hergestellt wurde:
   
    ![Verbindungsdetails des Integrationskontos](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>Encode EDIFACT Message by agreement name (EDIFACT-Nachricht nach Vereinbarungsname codieren)
Geben Sie den EDIFACT-Vereinbarungsnamen und die zu codierende XML-Nachricht an:
   
   ![Pflichtfelder ausfüllen](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>Encode EDIFACT Message by identities (EDIFACT-Nachricht nach Identitäten codieren)
Geben Sie die Senderqualifizierer und -bezeichner und Empfängerqualifizierer und -bezeichner wie in der EDIFACT-Vereinbarung konfiguriert an. Wählen Sie die zu codierende XML-Nachricht aus:  
    ![Pflichtfelder ausfüllen](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>Details zum Codieren von EDIFACT-Nachrichten
Der Connector zur Codierung von EDIFACT-Nachrichten führt Folgendes aus: 

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




<!--HONumber=Jan17_HO5-->


