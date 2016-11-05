---
title: Informationen zum Enterprise Integration Pack-Connector zum Codieren von AS2-Nachrichten | Microsoft Docs
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: erikre
editor: ''

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: padmavc

---
# Erste Schritte mit dem Codieren von AS2-Nachrichten
Stellen Sie eine Verbindung mit der Codierung von AS2-Nachrichten her, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Dieses Feature ermöglicht das digitale Signieren, Verschlüsseln und Bestätigen unter Verwendung von Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs), was in einer Unterstützung der Nichtabstreitbarkeit resultiert.

## Erstellen der Verbindung
### Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zum Codieren von AS2-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](app-service-logic-enterprise-integration-create-integration-account.md) sowie zu [Partnern](app-service-logic-enterprise-integration-partners.md) und zur [AS2-Vereinbarung](app-service-logic-enterprise-integration-as2.md).

### Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Codierung von AS2-Nachrichten her:
1. Ein Beispiel finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](app-service-logic-create-a-logic-app.md).
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten (etwa einen Anforderungstrigger). Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu. Wählen Sie in der Dropdownliste die Option „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie anschließend „AS2“ in das Suchfeld ein. Wählen Sie „AS2 – Encode AS2 Message“ (AS2 – AS2-Nachricht codieren) aus.
   
    ![AS2 suchen](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto erstellt haben, werden Sie zur Eingabe der Verbindungsdetails aufgefordert:
   
    ![Integrationskontoverbindung erstellen](./media/app-service-logic-enterprise-integration-AS2connector/as2encodeimage1.png)
4. Geben Sie die Details des Integrationskontos ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
      Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
      ![Integrationsverbindung hergestellt](./media/app-service-logic-enterprise-integration-AS2connector/as2encodeimage2.png)
5. Wählen Sie **Erstellen** aus.
6. Beachten Sie, dass die Verbindung erstellt wurde. Geben Sie sowohl AS2-From- und AS2-To-Bezeichner (gemäß Konfiguration in der Vereinbarung) als auch Details zum Text (Nutzlast der Nachricht) an.
   
    ![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-AS2connector/as2encodeimage3.png)

## Die AS2-Codierung führt folgende Schritte aus:
* Anwenden von AS2-/HTTP-Headern
* Signieren ausgehender Nachrichten (sofern konfiguriert)
* Verschlüsseln ausgehender Nachrichten (sofern konfiguriert)
* Komprimieren der Nachricht(sofern konfiguriert)

## Probieren Sie es selbst aus
Versuchen Sie es doch einfach mal. Klicken Sie [hier](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/), um Ihre eigene voll funktionsfähige Logik-App mit den AS2-Features von Logic Apps bereitzustellen.

## Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0914_2016-->