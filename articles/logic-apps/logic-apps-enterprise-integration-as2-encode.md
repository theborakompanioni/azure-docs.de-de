---
title: "Codieren von AS2-Nachrichten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Verwenden des im Enterprise Integration Pack enthaltenen AS2-Encoders für Azure Logic Apps"
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
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 82d64d159412af6ff2bba1d625af66cdd18a4616
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="encode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Codieren von AS2-Nachrichten für Azure Logic Apps mit dem Enterprise Integration Pack

Verwenden Sie den Connector zum Codieren von AS2-Nachrichten, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Dieser Connector ermöglicht die digitale Signierung, Verschlüsselung und Bestätigungen durch Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs), was in einer Unterstützung der Nichtabstreitbarkeit resultiert.

## <a name="before-you-start"></a>Vorbereitung

Sie benötigen Folgendes:

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein bereits definiertes und mit Ihrem Azure-Abonnement verknüpftes [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md). Für die Verwendung des Connectors zum Codieren von AS2-Nachrichten ist ein Integrationskonto erforderlich.
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind.
* Eine bereits in Ihrem Integrationskonto definierte [AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md).

## <a name="encode-as2-messages"></a>Codieren von AS2-Nachrichten

1. [Erstellen Sie eine Logik-App](logic-apps-create-a-logic-app.md).

2. Da der Connector zum Codieren von AS2-Nachrichten über keine Trigger verfügt, müssen Sie einen Trigger zum Starten Ihrer Logik-App hinzufügen (beispielsweise einen Anforderungstrigger). Fügen Sie im Logik-App-Designer einen Trigger hinzu, und fügen Sie anschließend Ihrer Logik-App eine Aktion hinzu.

3.    Geben Sie „AS2“ als Filter in das Suchfeld ein. Wählen Sie **AS2 - Encode AS2 message** (AS2 – AS2-Nachricht codieren) aus.
   
    ![Nach „AS2“ suchen](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Falls Sie noch keine Verbindungen mit Ihrem Integrationskonto erstellt haben, werden Sie aufgefordert, eine solche Verbindung zu erstellen. Benennen Sie die Verbindung, und wählen Sie das Integrationskonto aus, mit dem Sie eine Verbindung herstellen möchten. 
   
    ![Integrationskontoverbindung erstellen](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

    | Eigenschaft | Details |
    | --- | --- |
    | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
    | Integrationskonto* |Geben Sie einen Namen für Ihr Integrationskonto ein. Achten Sie darauf, dass sich Ihr Integrationskonto und Ihre Logik-App am gleichen Azure-Standort befinden. |

5.    Am Ende sollten Ihre Verbindungsdetails in etwa wie im folgenden Beispiel aussehen. Wählen Sie **Erstellen** aus, um die Verbindungserstellung abzuschließen.
   
    ![Details zur Integrationsverbindung](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Geben Sie nach der Verbindungserstellung wie in diesem Beispiel dargestellt Details für **AS2-From** und **AS2-To** (gemäß Konfiguration in der Vereinbarung) sowie **Text** (die Nachrichtennutzlast) ein.
   
    ![Pflichtfelder ausfüllen](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>Details des AS2-Encoders

Der Connector zum Codieren von AS2-Nachrichten führt folgende Aufgaben aus: 

* Anwenden von AS2-/HTTP-Headern
* Signieren ausgehender Nachrichten (sofern konfiguriert)
* Verschlüsseln ausgehender Nachrichten (sofern konfiguriert)
* Komprimieren der Nachricht(sofern konfiguriert)

## <a name="try-this-sample"></a>Beispiel zum Ausprobieren

Wenn Sie eine uneingeschränkt funktionsfähige Logik-App und ein AS2-Beispielszenario bereitstellen möchten, sehen Sie sich [Azure Logic Apps - AS2 Send Receive](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) (Azure Logic Apps – AS2: Senden/Empfangen) an.

## <a name="view-the-swagger"></a>Anzeigen von Swagger
Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/as2/). 

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") 


