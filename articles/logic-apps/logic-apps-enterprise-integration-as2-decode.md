---
title: "Decodieren von AS2-Nachrichten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Verwenden des im Enterprise Integration Pack enthaltenen AS2-Decoders für Azure Logic Apps"
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
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: da3fc08cc5c506dba02ccda16940a28a4600a288
ms.lasthandoff: 03/10/2017


---
# <a name="decode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Decodieren von AS2-Nachrichten für Azure Logic Apps mit dem Enterprise Integration Pack 

Verwenden Sie den Connector „AS2-Nachricht decodieren“, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Dieser Connector ermöglicht die digitale Signierung, Entschlüsselung und Bestätigungen durch Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs).

## <a name="before-you-start"></a>Vorbereitung

Sie benötigen Folgendes:

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein bereits definiertes und mit Ihrem Azure-Abonnement verknüpftes [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md). Für die Verwendung des Connectors „AS2-Nachricht decodieren“ ist ein Integrationskonto erforderlich.
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind.
* Eine bereits in Ihrem Integrationskonto definierte [AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md).

## <a name="decode-as2-messages"></a>Decodieren von AS2-Nachrichten

1. [Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

2. Da der Connector „AS2-Nachricht decodieren“ über keine Trigger verfügt, müssen Sie einen Trigger zum Starten Ihrer Logik-App hinzufügen (beispielsweise einen Anforderungstrigger). Fügen Sie im Logik-App-Designer einen Trigger hinzu, und fügen Sie anschließend Ihrer Logik-App eine Aktion hinzu.

3.    Geben Sie „AS2“ als Filter in das Suchfeld ein. Wählen Sie **AS2 – AS2-Nachricht decodieren** aus.
   
    ![Nach „AS2“ suchen](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Falls Sie noch keine Verbindungen mit Ihrem Integrationskonto erstellt haben, werden Sie aufgefordert, eine solche Verbindung zu erstellen. Benennen Sie die Verbindung, und wählen Sie das Integrationskonto aus, mit dem Sie eine Verbindung herstellen möchten.
   
    ![Erstellen der Integrationsverbindung](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

    | Eigenschaft | Details |
    | --- | --- |
    | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
    | Integrationskonto* |Geben Sie einen Namen für Ihr Integrationskonto ein. Achten Sie darauf, dass sich Ihr Integrationskonto und Ihre Logik-App am gleichen Azure-Standort befinden. |

5.    Am Ende sollten Ihre Verbindungsdetails in etwa wie im folgenden Beispiel aussehen. Wählen Sie **Erstellen** aus, um die Verbindungserstellung abzuschließen.

    ![Details zur Integrationsverbindung](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. Wählen Sie nach Erstellung der Verbindung wie in diesem Beispiel dargestellt **Body** (Hauptteil) und **Headers** (Kopfzeilen) aus den Anforderungsausgaben aus.
   
    ![Integrationsverbindung erstellt](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Beispiel:

    ![Wählen Sie bei den Ausgaben der Anforderung den Text und die Header aus.](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>Details des AS2-Decoders

Der Connector „AS2-Nachricht decodieren“ führt folgende Aufgaben aus: 

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

## <a name="try-this-sample"></a>Beispiel zum Ausprobieren

Wenn Sie eine uneingeschränkt funktionsfähige Logik-App und ein AS2-Beispielszenario bereitstellen möchten, sehen Sie sich [Azure Logic Apps - AS2 Send Receive](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) (Azure Logic Apps – AS2: Senden/Empfangen) an.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md) 


