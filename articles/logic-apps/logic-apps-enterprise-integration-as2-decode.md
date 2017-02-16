---
title: Decodieren von AS2-Nachrichten in Azure Logic Apps | Microsoft-Dokumentation
description: Verwendung des im Enterprise Integration Pack enthaltenen AS2-Decoders mit Logic Apps
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
ms.date: 01/27/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 2f407a428aa176cc5c2a3b6bb236b522bda5ab64
ms.openlocfilehash: 37562ff385305088590c793147b8ad268148c40b


---
# <a name="get-started-with-decoding-as2-message-in-your-logic-apps"></a>Erste Schritte der Decodierung von AS2-Nachrichten in Ihren Logic Apps
Stellen Sie eine Verbindung mit der Decodierung von AS2-Nachrichten her, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Sie ermöglicht die digitale Signierung, Entschlüsselung und Bestätigungen über Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs).

## <a name="prereqs"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Decodierung von AS2-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details [zum Erstellen eines Integrationskontos](logic-apps-enterprise-integration-create-integration-account.md) sowie zu [Partnern](logic-apps-enterprise-integration-partners.md) und zur [AS2-Vereinbarung](../logic-apps/logic-apps-enterprise-integration-as2.md).

## <a name="decode-as2-messages"></a>Decodieren von AS2-Nachrichten
1. [Erstellen Sie eine Logic App.](../logic-apps/logic-apps-create-a-logic-app.md)
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste die Option „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie anschließend „AS2“ in das Suchfeld ein.  Wählen Sie „AS2 – AS2-Nachricht decodieren“ aus:
   
    ![Suchen von AS2](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Erstellen der Integrationsverbindung](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden:
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
      Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
      ![Integrationsverbindung](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)
5. Klicken Sie auf **Erstellen**.
6. Beachten Sie, dass die Verbindung erstellt wurde.  Fahren Sie nun mit den weiteren Schritten in Ihrer Logic App fort:
   
    ![Integrationsverbindung erstellt](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 
7. Wählen Sie bei den Ausgaben der Anforderung den Text und die Header aus:
   
    ![Pflichtfelder ausfüllen](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>Details des AS2-Decoders
Der Connector zur Decodierung von AS2-Nachrichten führt Folgendes aus: 

* Verarbeiten der AS2/HTTP-Header
* Überprüfen der Signatur (sofern konfiguriert)
* Entschlüsseln der Nachrichten (falls konfiguriert)
* Dekomprimieren der Nachrichten (falls konfiguriert)
* Abgleichen einer empfangenen MDN mit der ursprünglichen ausgehenden Nachricht
* Aktualisieren und Korrelieren von Datensätzen in der Nichtabstreitbarkeits-Datenbank
* Schreiben von Datensätzen für AS2-Statusberichte
* Der Inhalt der Ausgabenutzlast ist base64-codiert.
* Bestimmen, ob eine MDN erforderlich ist und ob die MDN synchron oder asynchron sein soll (ausgehend von der Konfiguration in der AS2-Vereinbarung)
* Generieren einer synchronen oder asynchronen MDN (ausgehend von der Konfiguration in der Vereinbarung)
* Festlegen der Korrelationstoken und Eigenschaften für die MDN

## <a name="try-it-yourself"></a>Probieren Sie es selbst.
Probieren Sie es aus! Stellen Sie mit [AS2-Logic App-Vorlage und -Szenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) eine voll funktionsfähige Logic App bereit.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md) 




<!--HONumber=Jan17_HO5-->


