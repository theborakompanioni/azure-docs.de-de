---
title: Codieren von AS2-Nachrichten in Azure Logic Apps | Microsoft-Dokumentation
description: Verwendung des im Enterprise Integration Pack enthaltenen AS2-Encoders mit Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: a2ca24c1800a01e6a15af35937a654093cf224af
ms.openlocfilehash: 754ec2e829babd2e5ca3e5e5290db950ef5035e7


---
# <a name="get-started-with-encode-as2-message"></a>Erste Schritte mit dem Codieren von AS2-Nachrichten
Stellen Sie eine Verbindung mit der Codierung von AS2-Nachrichten her, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Dieses Feature ermöglicht das digitale Signieren, Verschlüsseln und Bestätigen unter Verwendung von Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs), was in einer Unterstützung der Nichtabstreitbarkeit resultiert.

## <a name="prereqs"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zum Codieren von AS2-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](logic-apps-enterprise-integration-create-integration-account.md) sowie zu [Partnern](logic-apps-enterprise-integration-partners.md) und zur [AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md).

## <a name="encode-as2-messages"></a>Codieren von AS2-Nachrichten
1. [Erstellen einer Logik-App](logic-apps-create-a-logic-app.md)
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste die Option „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie anschließend „AS2“ in das Suchfeld ein.  Wählen Sie „AS2 – AS2-Nachricht codieren“ aus:
   
    ![AS2 suchen](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Integrationskontoverbindung erstellen](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden:
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
      Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
      ![Integrationsverbindung hergestellt](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)  
5. Klicken Sie auf **Erstellen**.
6. Beachten Sie, dass die Verbindung erstellt wurde.  Geben Sie sowohl AS2-From- und AS2-To-Bezeichner (gemäß Konfiguration in der Vereinbarung) als auch Details zum Text (Nutzlast der Nachricht) an:
   
    ![Pflichtfelder ausfüllen](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>Details des AS2-Encoders
Der Connector zur Codierung von AS2-Nachrichten führt Folgendes aus: 

* Anwenden von AS2-/HTTP-Headern
* Signieren ausgehender Nachrichten (sofern konfiguriert)
* Verschlüsseln ausgehender Nachrichten (sofern konfiguriert)
* Komprimieren der Nachricht(sofern konfiguriert)

## <a name="try-it-yourself"></a>Probieren Sie es selbst.
Probieren Sie es aus! Stellen Sie mit [AS2-Logic App-Vorlage und -Szenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) eine voll funktionsfähige Logic App bereit.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 




<!--HONumber=Jan17_HO5-->


