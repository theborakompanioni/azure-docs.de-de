---
title: Informationen zum Enterprise Integration Pack-Connector zum Codieren von X12-Nachrichten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: erikre
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 54626bdb7b07a91e5a2b1d16fed10be6cccacc74


---
# <a name="get-started-with-encode-x12-message"></a>Erste Schritte mit dem Codieren von X12-Nachrichten
Überprüft EDI- und partnerspezifische Eigenschaften, konvertiert XML-codierte Nachrichten in EDI-Transaktionssätze im Austausch und fordert eine technische Bestätigung und/oder eine Funktionsbestätigung an.

## <a name="create-the-connection"></a>Erstellen der Verbindung
### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zum Codieren von X12-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](app-service-logic-enterprise-integration-create-integration-account.md) sowie zu [Partnern](app-service-logic-enterprise-integration-partners.md) und zur [X12-Vereinbarung](app-service-logic-enterprise-integration-x12.md).

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Codierung von X12-Nachrichten her:
1. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](app-service-logic-create-a-logic-app.md) finden Sie ein Beispiel.
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste die Option „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie anschließend „x12“ in das Suchfeld ein.  Wählen Sie entweder „X12 - Encode X12 Message by agreement name“ (X12 – X12-Nachricht nach Vereinbarungsname codieren) oder „X12 - Encode to X 12 message by identities“ (X12 – X12-Nachricht nach Identitäten codieren) aus.  
   
    ![Nach x12 suchen](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto erstellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Integrationskontoverbindung](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskontoverbindung erstellt](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 
5. Wählen Sie **Erstellen**
6. Beachten Sie, dass die Verbindung erstellt wurde.
   
    ![Verbindungsdetails des Integrationskontos](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>„X12 - Encode X12 Message by agreement name“ (X12 – X12-Nachricht nach Vereinbarungsname codieren)
1. Wählen Sie in der Dropdownliste die Option „X12Agreement“ und im nächsten Feld die zu codierende XML-Nachricht aus.
   
    ![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>„X12 - Encode to X 12 message by identities“ (X12 – X12-Nachricht nach Identitäten codieren)
1. Geben Sie den Absenderqualifizierer und -bezeichner und den Empfängerqualifizierer und -bezeichner wie in der X12-Vereinbarung konfiguriert an.  Wählen Sie die zu codierende XML-Nachricht aus.
   
   ![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>Die X12-Codierung führt die folgenden Schritte aus:
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
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 




<!--HONumber=Nov16_HO3-->


