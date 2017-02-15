---
title: Informationen zum Enterprise Integration Pack-Connector zum Decodieren von AS2-Nachrichten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: erikre
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 5836e0d2bb7d1d97e9f175396f51047af0f84ca5


---
# <a name="get-started-with-decode-as2-message"></a>Erste Schritte mit der Decodierung von AS2-Nachrichten
Stellen Sie eine Verbindung mit der Decodierung von AS2-Nachrichten her, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Sie ermöglicht die digitale Signierung, Entschlüsselung und Bestätigungen über Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs).

## <a name="create-the-connection"></a>Erstellen der Verbindung
### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Decodierung von AS2-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](app-service-logic-enterprise-integration-create-integration-account.md) sowie zu [Partnern](app-service-logic-enterprise-integration-partners.md) und zur [AS2-Vereinbarung](app-service-logic-enterprise-integration-as2.md).

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von AS2-Nachrichten her:
1. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](app-service-logic-create-a-logic-app.md) finden Sie ein Beispiel.
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (beispielsweise einen Anforderungstrigger).  Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu.  Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „AS2“ in das Suchfeld ein.  Wählen Sie „AS2 – Decode AS2 Message“ (AS2 – AS2-Nachricht decodieren) aus.
   
    ![Suchen von AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto erstellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Erstellen der Integrationsverbindung](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)
4. Geben Sie die Details des Integrationskontos ein.  Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
      Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
      ![Integrationsverbindung](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)
5. Wählen Sie **Erstellen**
6. Beachten Sie, dass die Verbindung erstellt wurde.  Fahren Sie nun mit den weiteren Schritten in Ihrer Logik-App fort.
   
    ![Integrationsverbindung erstellt](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 
7. Wählen Sie bei den Ausgaben der Anforderung den Text und die Header aus.
   
    ![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>Die AS2-Decodierung führt folgende Schritte aus:
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

## <a name="try-it-for-yourself"></a>Probieren Sie es selbst aus
Versuchen Sie es doch einfach mal. Klicken Sie [hier](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) , um Ihre eigene voll funktionsfähige Logik-App mit den AS2-Features von Logic Apps bereitzustellen. 

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 




<!--HONumber=Nov16_HO3-->


