---
title: "Einrichten der Ereignisüberwachung mit Azure Event Hubs für Azure Logic Apps | Microsoft-Dokumentation"
description: "Überwachen von Datenströmen zum Empfangen und Senden von Ereignissen für Azure Logic Apps mit Azure Event Hubs"
services: logic-apps
keywords: "Datenstrom, Ereignisüberwachung, Event Hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 2ca27fb8269d1796fb1181fc4d0a8744a592d548
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Überwachen, Empfangen und Senden von Ereignissen mit dem Event Hub-Connector

Um eine Ereignisüberwachung so einzurichten, dass Ihre Logik-App Ereignisse erkennen, empfangen und senden kann, stellen Sie über Ihre Logik-App eine Verbindung zu einem [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) her. Weitere Informationen zu [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Anforderungen

* Es muss ein [Event Hub-Namespace und Event Hub](../event-hubs/event-hubs-create.md) in Azure vorhanden sein. Weitere Informationen finden Sie unter [Gewusst wie: Erstellen eines Event Hub-Namespace und eines Event Hubs](../event-hubs/event-hubs-create.md). 

* Um [einen Connector](https://docs.microsoft.com/azure/connectors/apis-list) in Ihrer Logik-App zu verwenden, müssen Sie zuerst eine Logik-App erstellen. Weitere Informationen finden Sie unter [Gewusst wie: Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a>Überprüfen von Berechtigungen für Event Hub-Namespaces und Suchen nach der Verbindungszeichenfolge

Damit Ihre Logik-App auf einen Dienst zugreifen kann, müssen Sie eine [*Verbindung*](./connectors-overview.md) zwischen Ihrer Logik-App und dem Dienst erstellen, sofern dies noch nicht geschehen ist. Diese Verbindung erlaubt Ihrer Logik-App den Zugriff auf Daten.
Damit Ihre Logik-App auf Ihren Event Hub zugreifen kann, benötigen Sie **Verwaltungsberechtigungen** und die Verbindungszeichenfolge für Ihren Event Hub-Namespace.

Um Ihre Berechtigungen zu überprüfen und die Verbindungszeichenfolge abzurufen, führen Sie folgende Schritte durch.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an. 

2.  Navigieren Sie zu Ihrem Event Hub-*Namespace*, jedoch nicht zum eigentlichen Event Hub. Wählen Sie auf dem Blatt „Namespace“ unter **Einstellungen** die Option **Freigegebene Zugriffsrichtlinien**. Stellen Sie im Bereich **Ansprüche** sicher, dass Sie die **Verwaltungsberechtigungen** für diesen Namespace besitzen.

    ![Verwalten von Berechtigungen für Ihren Event Hub-Namespace](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  Um die Verbindungszeichenfolge für den Event Hub-Namespace zu kopieren, wählen Sie **RootManageSharedAccessKey**. Wählen Sie neben der Verbindungszeichenfolge Ihres Primärschlüssels die Schaltfläche „Kopieren“.

    ![Kopieren der Verbindungszeichenfolge für Event Hub-Namespaces](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Um zu überprüfen, ob Ihre Verbindungszeichenfolge mit Ihrem Event Hub-Namespace oder einem bestimmten Event Hub verknüpft ist, überprüfen Sie die Verbindungszeichenfolge für den Parameter `EntityPath`. Wenn Sie diesen Parameter gefunden haben, gilt die Verbindungszeichenfolge für eine bestimmte Event Hub-Entität und stellt nicht die richtige Zeichenfolge für die Verwendung mit Ihrer Logik-App dar.

4.  Wenn Sie nach dem Hinzufügen eines Triggers oder einer Aktion des Event Hubs nun zur Eingabe von Anmeldeinformationen in Ihrer Logik-App aufgefordert werden, können Sie eine Verbindung zu Ihrem Event Hub-Namespace herstellen. Vergeben Sie einen Namen für Ihre Verbindung, fügen Sie die kopierte Verbindungszeichenfolge ein und wählen Sie **Erstellen**.

    ![Eingeben der Verbindungszeichenfolge für Event Hub-Namespaces](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Nachdem Sie die Verbindung erstellt haben, sollte der Verbindungsname im Trigger oder in der Aktion des Event Hubs angezeigt werden. 
    Danach können Sie mit den anderen Schritten in Ihrer Logik-App fortfahren.

    ![Erstellte Verbindung zum Event Hub-Namespace](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Starten des Workflows beim Empfangen neuer Ereignisse durch Ihren Event Hub

Ein [*Trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) ist ein Ereignis, das einen Workflow in Ihrer Logik-App startet. Um einen Workflow zu starten, wenn neue Ereignisse an Ihren Event Hub gesendet werden, führen Sie diese Schritte zum Hinzufügen des Triggers durch, der dieses Ereignis erkennt.

1.  Navigieren Sie im [Azure-Portal](https://portal.azure.com "Azure-Portal") zu Ihrer vorhandenen Logik-App oder erstellen Sie eine leere Logik-App.

2.  Geben Sie in das Suchfeld für den Logik-App-Designer `event hubs` für Ihren Filter ein. Wählen Sie folgenden Trigger aus: **Wenn Ereignisse im Event Hub verfügbar sind**

    ![Auswählen des Triggers beim Empfangen neuer Ereignisse durch Ihren Event Hub](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Wenn noch keine Verbindung zu Ihrem Event Hub-Namespace besteht, werden Sie aufgefordert, diese Verbindung nun zu erstellen. Vergeben Sie einen Namen für Ihre Verbindung und geben Sie die Verbindungszeichenfolge für Ihren Event Hub-Namespace ein. 
    Beachten Sie ggf. die Informationen unter [Gewusst wie: Suchen nach Ihrer Verbindungszeichenfolge](#permissions-connection-string).

    ![Eingeben der Verbindungszeichenfolge für Event Hub-Namespaces](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Nach der Erstellung der Verbindung werden die Einstellungen für den Trigger **Wenn ein Ereignis in einem Event Hub verfügbar ist** angezeigt.

    ![Trigger-Einstellungen beim Empfangen neuer Ereignisse durch Ihren Event Hub](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Geben Sie den Namen für den zu überwachenden Event Hub ein oder wählen Sie ihn aus. Wählen Sie die Häufigkeit und das Intervall für die Häufigkeit aus, mit der der Event Hub überprüft werden soll.

    > [!TIP]
    > Um optional eine Consumergruppe zum Lesen von Ereignissen auszuwählen, wählen Sie **Erweiterte Optionen anzeigen**. 

    ![Festlegen von Event Hubs oder Consumergruppen](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Sie haben nun einen Trigger zum Starten eines Workflows für Ihre Logik-App eingerichtet. 
    Ihre Logik-App überprüft den festgelegten Event Hub gemäß dem von Ihnen festgelegten Zeitplan. 
    Wenn Ihre App neue Ereignisse im Event Hub findet, führt der Trigger andere Aktionen oder Trigger in Ihrer Logik-App aus.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Senden von Ereignissen über Ihre Logik-App an Ihren Event Hub

Eine [*Aktion*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) ist eine Aufgabe, die von Ihrem Logik-App-Workflow ausgeführt wird. Nachdem Sie Ihrer Logik-App einen Trigger hinzugefügt haben, können Sie eine Aktion zum Ausführen von Vorgängen mit Daten hinzufügen, die von diesem Trigger generiert wurden. Um über Ihre Logik-App ein Ereignis an Ihren Event Hub zu senden, führen Sie folgende Schritte durch.

1.  Wählen Sie im Logik-App-Designer unter Ihrem Logik-App-Trigger **Neuer Schritt** > **Aktion hinzufügen** aus.

    ![Wählen Sie „Neuer Schritt“ und dann „Aktion hinzufügen“.](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Nun können Sie nach einer durchzuführenden Aktion suchen und diese auswählen. 
    Sie können zwar eine beliebige Aktion auswählen, doch für dieses Beispiel wird die Event Hub-Aktion zum Senden von Ereignissen benötigt.

2.  Geben Sie `event hubs` als Filter in das Suchfeld ein.
Wählen Sie folgende Aktion aus: **Ereignis senden**.

    ![Wählen Sie die Aktion „Event Hubs – Ereignis senden“ aus.](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Geben Sie die erforderlichen Details für das Ereignis ein, z. B. den Namen für den Event Hub, mit dem das Ereignis gesendet werden soll. Geben Sie weitere optionale Details zum Ereignis ein, z. B. Inhalte für dieses Ereignis.

    > [!TIP]
    > Um optional die Event Hub-Partition festzulegen, mit dem das Ereignis gesendet werden soll, wählen Sie **Erweiterte Optionen anzeigen**. 

    ![Geben Sie den Namen des Event Hubs und optional Ereignisdetails ein.](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Speichern Sie die Änderungen.

    ![Speichern Ihrer Logik-App](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Sie haben nun eine Aktion zum Senden von Ereignissen über Ihre Logik-App eingerichtet. 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/eventhubs/) an. 

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

*  [Suchen nach anderen Connectors für Azure Logic Apps](./apis-list.md)
