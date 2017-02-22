---
title: Codieren von X12-Nachrichten in Azure Logic Apps | Microsoft-Dokumentation
description: Verwenden des im Enterprise Integration Pack enthaltenen X12-Encoders in Ihren Logik-Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: bd339e60aff1edca7f86e0ae82746f55eb67b296
ms.openlocfilehash: 9f7b53cfafa8c14ab46cf80015afc8da4e0fda2b


---
# <a name="get-started-with-encode-x12-message"></a>Erste Schritte mit dem Codieren von X12-Nachrichten
Überprüft EDI- und partnerspezifische Eigenschaften, konvertiert XML-codierte Nachrichten in EDI-Transaktionssätze im Austausch und fordert eine technische Bestätigung und/oder eine Funktionsbestätigung an.

## <a name="prereqs"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zum Codieren von X12-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](logic-apps-enterprise-integration-create-integration-account.md) sowie zu [Partnern](logic-apps-enterprise-integration-partners.md) und zur [X12-Vereinbarung](logic-apps-enterprise-integration-x12.md).

## <a name="encode-x12-messages"></a>Codieren von X12-Nachrichten

1. [Erstellen Sie eine Logik-App](logic-apps-create-a-logic-app.md).
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste die Option „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie anschließend „x12“ in das Suchfeld ein.  Wählen Sie entweder „X12 - Encode X12 Message by agreement name“ (X12 – X12-Nachricht nach Vereinbarungsname codieren) oder „X12 - Encode to X 12 message by identities“ (X12 – X12-Nachricht nach Identitäten codieren) aus:  
   
    ![Nach x12 suchen](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto erstellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Integrationskontoverbindung](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png) 
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden:
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskontoverbindung erstellt](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png) 
5. Klicken Sie auf **Erstellen**.
6. Beachten Sie, dass die Verbindung erstellt wurde.
   
    ![Verbindungsdetails des Integrationskontos](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Encode X12 Message by agreement name (X12-Nachricht nach Vereinbarungsname codieren)
Wählen Sie in der Dropdownliste die Option „X12Agreement“ und im nächsten Feld die zu codierende XML-Nachricht aus:     
    ![Pflichtfelder ausfüllen](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png) 

#### <a name="encode-x12-messages-by-identities"></a>Encode to X&12; message by identities (X12-Nachricht nach Identitäten codieren)
Geben Sie den Absenderqualifizierer und -bezeichner und den Empfängerqualifizierer und -bezeichner wie in der X12-Vereinbarung konfiguriert an.  Wählen Sie die zu codierende XML-Nachricht aus:
   
   ![Pflichtfelder ausfüllen](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>Details zum Codieren von X12-Nachrichten

Der Connector zur Codierung von X12-Nachrichten führt Folgendes aus:

* Vereinbarungsauflösung durch Zuordnen der Kontexteigenschaften von Absender und Empfänger.
* Serialisierung des EDI-Austauschs mit Konvertierung von XML-codierten Nachrichten in EDI-Transaktionssätze im Austausch
* Anwendung von Header- und Nachspannsegmenten für den Transaktionssatz
* Generierung einer Austauschkontrollnummer, einer Gruppenkontrollnummer und einer Transaktionssatz-Kontrollnummer für jeden ausgehenden Austausch
* Ersetzung von Trennzeichen in den Nutzlastdaten
* Überprüfung von EDI- und partnerspezifischen Eigenschaften
  * Schemavalidierung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
  * EDI-Überprüfung für Transaktionssatz-Datenelemente
  * Erweiterte Überprüfung für Transaktionssatz-Datenelemente
* Anforderung einer technischen Bestätigung und/oder einer Funktionsbestätigung (sofern konfiguriert)
  * Eine technische Bestätigung wird als Ergebnis der Headerüberprüfung generiert. Die technische Bestätigung meldet den Status der Verarbeitung eines Austauschheaders und -nachspanns durch den Adressempfänger.
  * Eine Funktionsbestätigung wird als Ergebnis der Textüberprüfung generiert. Die Funktionsbestätigung meldet jeden Fehler, der bei der Verarbeitung des empfangenen Dokuments aufgetreten ist.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 




<!--HONumber=Jan17_HO5-->


