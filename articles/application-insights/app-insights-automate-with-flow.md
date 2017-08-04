---
title: Automatisieren von Azure Application Insights-Prozessen mit Microsoft Flow
description: In diesem Artikel erfahren Sie, wie Sie Microsoft Flow zur schnellen Automatisierung von wiederholbaren Prozessen mit dem Application Insights-Connector verwenden.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: d0cd0cc97fa61d3401f6101292b82132622c9e81
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---

# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Automatisieren von Azure Application Insights-Prozessen mit dem Connector für Microsoft Flow

Ertappen Sie sich dabei, wie Sie wiederholt dieselben Abfragen von Ihren Telemetriedaten ausführen, um zu überprüfen, ob Ihr Dienst ordnungsgemäß ausgeführt wird? Möchten Sie diese Abfragen für die Suche nach Trends und Anomalien automatisieren und eigene Workflows dafür erstellen? Dann benötigen Sie den Azure Application Insights-Connector (Vorschau) für Microsoft Flow.

Mit dieser Integration können Sie nun zahlreiche Prozesse automatisieren, ohne eine einzige Codezeile zu schreiben. Nachdem Sie einen Flow mit einer Application Insights-Aktion erstellt haben, wird Ihre Application Insights Analytics-Abfrage automatisch ausgeführt. 

Sie können auch weitere Aktionen hinzufügen. Mit Microsoft Flow stehen Ihnen Hunderte von Aktionen zur Verfügung. So können Sie mit Microsoft Flow beispielsweise automatisch eine E-Mail-Benachrichtigung senden oder einen Fehler in Visual Studio Team Services protokollieren. Außerdem können Sie eine der zahlreichen [Vorlagen](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) verwenden, die für den Connector für Microsoft Flow zur Verfügung stehen. Diese Vorlagen beschleunigen den Vorgang zum Erstellen eines Flow. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Erstellen eines Flows für Application Insights

In diesem Tutorial erfahren Sie, wie Sie einen Flow erstellen, der zum Gruppieren von Attributen in den Daten für eine Webanwendung den Autoclusteralgorithmus von Analytics verwendet. Die Ergebnisse werden automatisch per E-Mail gesendet. Dies ist nur eins von vielen Beispielen für die gemeinsame Verwendung von Microsoft Flow und Application Insights Analytics. 

### <a name="step-1-create-a-flow"></a>Schritt 1: Erstellen eines Flow
1. Melden Sie sich bei [Microsoft Flow](http://flow.microsoft.com) an, und wählen Sie **Meine Flows** aus.
2. Klicken Sie auf **Flow ohne Vorlage erstellen**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>Schritt 2: Erstellen eines Triggers für Ihren Flow
1. Wählen Sie **Zeitplan** und anschließend **Zeitplan – Wiederholung** aus.
2. Wählen Sie im Feld **Häufigkeit** die Option **Tag** aus, und geben Sie im Feld **Intervall** den Wert **1** ein.

    ![Trigger-Dialogfeld für Microsoft Flow](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>Schritt 3: Hinzufügen einer Application Insights-Aktion
1. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen**.
2. Suchen Sie nach **Azure Application Insights**.
3. Klicken Sie auf **Azure Application Insights – Visualize Analytics query Preview** (Azure Application Insights – Analytics-Abfrage visualisieren (Vorschau)).

    ![Fenster „Analytics-Abfrage ausführen“](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Schritt 4: Herstellen einer Verbindung mit einer Application Insights-Ressource

Für diesen Schritt benötigen Sie eine Anwendungs-ID und einen API-Schlüssel für Ihre Ressource. Diese können Sie über das Azure-Portal abrufen, wie in der folgenden Abbildung zu sehen:

![Anwendungs-ID im Azure-Portal](./media/app-insights-automate-with-flow/appid.png) 

- Geben Sie einen Namen für Ihre Verbindung sowie die Anwendungs-ID und den API-Schlüssel ein.

    ![Fenster für die Microsoft Flow-Verbindung](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Schritt 5: Angeben der Analytics-Abfrage und des Diagrammtyps
Diese Beispielabfrage wählt die nicht erfolgreichen Anforderungen des letzten Tages aus und gleicht sie mit Ausnahmen ab, die im Zusammenhang mit dem Vorgang aufgetreten sind. Analytics führt den Abgleich auf der Grundlage des Bezeichners „operation_Id“ durch. Die Ergebnisse werden anschließend mithilfe des automatischen Clusteralgorithmus segmentiert. 

Vergewissern Sie sich beim Erstellen eigener Abfragen, dass sie ordnungsgemäß in Analytics funktionieren, bevor Sie sie zu Ihrem Flow hinzufügen.

- Fügen Sie die folgende Analytics-Abfrage hinzu, und wählen Sie den Diagrammtyp „HTML-Tabelle“ aus. 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```
    
    ![Konfigurationsfenster für Analytics-Abfragen](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>Schritt 6: Konfigurieren des Flow zum Senden von E-Mails

1. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen**.
2. Suchen Sie nach **Office 365 Outlook**.
3. Klicken Sie auf **Office 365 Outlook – E-Mail senden**.

    ![Fenster zum Auswählen von Office 365 Outlook](./media/app-insights-automate-with-flow/flow2b.png)

4. Gehen Sie im Fenster **E-Mail senden** wie folgt vor:

   a. Geben Sie die E-Mail-Adresse des Empfängers ein.

   b. Geben Sie einen Betreff für die E-Mail ein.

   c. Klicken Sie auf das Feld **Text**, und wählen Sie im dynamischen Inhaltsmenü, das auf der rechten Seite geöffnet wird, das Feld **Text** aus.

   d. Klicken Sie auf **Erweiterte Optionen anzeigen**.

    ![Konfiguration von Office 365 Outlook](./media/app-insights-automate-with-flow/flow5.png)

5. Gehen Sie im dynamischen Inhaltsmenü wie folgt vor:

    a. Wählen Sie **Anlagenname** aus.

    b. Wählen Sie **Anlageninhalt** aus.
    
    c. Wählen Sie im Feld **Ist HTML** die Option **Ja** aus.

    ![Fenster zum Konfigurieren von Office 365-E-Mails](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>Schritt 7: Speichern und Testen Ihres Flow
- Geben Sie im Feld **Flowname** einen Namen für den Flow ein, und klicken Sie anschließend auf **Flow erstellen**.

    ![Fenster für die Flowerstellung](./media/app-insights-automate-with-flow/flow8.png)

Sie können warten, bis der Trigger die Aktion ausführt, oder den Flow anhand der Schritte unter [Ausführen des Triggers bei Bedarf](https://flow.microsoft.com/blog/run-now-and-six-more-services/) sofort ausführen.

Wenn der Flow ausgeführt wird, erhalten die Empfänger, die Sie in der E-Mail-Liste festgelegt haben, eine E-Mail, die in etwa wie folgt aussieht:

![Beispiel-E-Mail](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Erstellung von [Analytics-Abfragen](app-insights-analytics-using.md).
- Erfahren Sie mehr über [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->






