---
title: Automatisieren von Azure Application Insights-Prozessen mit Microsoft Flow
description: "In diesem Artikel erfahren Sie, wie Sie Microsoft Flow zur schnellen Automatisierung von wiederholbaren Prozessen mit dem Application Insights-Connector für Flow verwenden können."
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
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 7e17fde55097a8e852be33bac66940a05fdd9df6
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---

# <a name="automate-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Automatisieren von Azure Application Insights-Prozessen mit dem Connector für Microsoft Flow

Ertappen Sie sich dabei, wie Sie wiederholt dieselben Abfragen zu Ihren Telemetriedaten ausführen, um zu überprüfen, ob Ihr Dienst ordnungsgemäß ausgeführt wird? Möchten Sie diese Abfragen zum Suchen nach Trends und Anomalien automatisieren und Ihre eigenen Workflows zu diesen erstellen? Dann ist der Application Insights-Connector (Vorschau) für Microsoft Flow genau das Richtige für Sie!
Mit dieser Integration können nun zahlreiche Prozesse automatisiert werden, ohne dass eine einzige Codezeile geschrieben werden muss. Nachdem Sie einen Flow mit einer Application Insights-Aktion erstellt haben, wird Ihre Application Insights Analytics-Abfrage automatisch ausgeführt. Sie können auch weitere Aktionen hinzufügen. Mit Flow stehen Ihnen Hunderte von Aktionen zur Verfügung. Beispielsweise können Sie mit Flow automatisch eine E-Mail-Benachrichtigung senden oder einen Fehler in Visual Studio Team Services erstellen. Sie können auch eine der zahlreichen [Vorlagen](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) verwenden, die für den Connector für Microsoft Flow verfügbar sind. Diese Vorlagen beschleunigen den Vorgang zum Erstellen eines Flow. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="tutorial-for-creating-a-flow-for-application-insights"></a>Tutorial zum Erstellen eines Flow für Application Insights

In diesem Tutorial erfahren Sie, wie Sie einen Flow erstellen, der zum Gruppieren von Attributen in den Daten für eine Webanwendung den Autoclusteralgorithmus von Analytics verwendet. Der Flow sendet die Ergebnisse automatisch per E-Mail. Dies ist nur ein Beispiel dafür, wie Sie Microsoft Flow und Application Insights Analytics gemeinsam verwenden können. 

### <a name="step-1-create-a-flow"></a>Schritt 1: Erstellen eines Flow
1. Melden Sie sich unter http://flow.microsoft.com an, und wählen Sie **Meine Flows**.
2. Klicken Sie auf **Flow ohne Vorlage erstellen**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>Schritt 2: Erstellen eines Triggers für Ihren Flow
1. Wählen Sie **Zeitplan** und dann **Zeitplan – Wiederholung**.
2. Legen Sie die **Häufigkeit** auf „Tag“ mit einem **Intervall** von 1 fest.

![Dialogfeld „Flowtrigger“](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-azure-application-insights-action"></a>Schritt 3: Hinzufügen einer Azure Application Insights-Aktion
1. Klicken Sie auf **Neuer Schritt** und dann auf **Aktion hinzufügen**.
2. Suchen Sie nach Azure Application Insights.
3. Klicken Sie auf „Azure Application Insights – Analytics-Abfrage visualisieren Vorschau“.

![Bildschirm „Analytics-Abfrage ausführen“](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Schritt 4: Herstellen einer Verbindung mit einer Application Insights-Ressource

**Voraussetzung**

Für die Durchführung dieses Schritts benötigen Sie eine Anwendungs-ID und einen API-Schlüssel für Ihre Ressource. Diese können Sie über das Azure-Portal abrufen, wie in der folgenden Abbildung veranschaulicht wird:

![Anwendungs-ID im Azure-Portal](./media/app-insights-automate-with-flow/appid.png) 

- Geben Sie einen Namen für Ihre Verbindung zusammen mit der Anwendungs-ID und dem API-Schlüssel ein.

![Bildschirm „Flowverbindung“](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Schritt 5: Angeben der Analytics-Abfrage und des Diagrammtyps
In diesem Beispiel werden die fehlerhaften Anforderungen des letzten Tages ausgewählt und mit Ausnahmen korreliert, die im Zusammenhang mit dem Vorgang aufgetreten sind. Analytics führt die Korrelation basierend auf dem Bezeichner „operation_Id“ durch. Die Abfrage segmentiert die Ergebnisse dann mit dem Autoclusteralgorithmus. Wenn Sie eigene Abfragen erstellen, überprüfen Sie unbedingt, ob diese ordnungsgemäß in Analytics funktionieren, bevor Sie sie zu Ihrem Flow hinzufügen.

- Fügen Sie die folgenden Analytics-Abfrage hinzu, und wählen Sie als Diagrammtyp „HTML-Tabelle“. 

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
![Bildschirm „Konfiguration von Analytics-Abfragen“](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>Schritt 6: Konfigurieren des Flow zum Senden von E-Mails

1. Klicken Sie auf **Neuer Schritt**, und wählen Sie anschließend **Aktion hinzufügen**.
2. Suchen Sie nach Office 365 Outlook.
3. Klicken Sie auf „Office 365 Outlook – E-Mail senden“.
![Auswahl von Office 365 Outlook](./media/app-insights-automate-with-flow/flow2b.png)

4. Fügen Sie in der E-Mail-Aktion die folgenden Daten hinzu:
 - Geben Sie die E-Mail-Adresse des Empfängers ein.
 - Geben Sie einen Betreff für die E-Mail ein.
 - Setzen Sie den Cursor in das **Text**-Feld, und wählen Sie im dynamischen Inhaltsmenü, das auf der rechten Seite geöffnet wird, das Feld **Text**.
 - Klicken Sie auf **Erweiterte Optionen anzeigen**.

 ![Konfiguration von Office 365 Outlook](./media/app-insights-automate-with-flow/flow5.png)

5. Führen Sie im dynamischen Inhaltsmenü folgende Schritte durch:
- Wählen Sie **Anlagenname**.
- Wählen Sie **Anlageninhalt**.
- Wählen Sie **Ja** im Feld **HTML festlegen**.

![Bildschirm „Konfiguration von Office 365-E-Mails“](./media/app-insights-automate-with-flow/flow7.png)
### <a name="step-7-save-and-test-your-flow"></a>Schritt 7: Speichern und Testen Ihres Flow
- Fügen Sie einen Namen zu Ihrem Flow hinzu, und klicken Sie auf **Flow erstellen**.

![Bildschirm „Erstellung mit Microsoft Flow“](./media/app-insights-automate-with-flow/flow8.png)

Sie können warten, bis der Trigger diese Aktion ausgeführt hat, oder den Flow sofort anhand der Schritte unter [Ausführen des Triggers bei Bedarf](https://flow.microsoft.com/blog/run-now-and-six-more-services/) ausführen.

Wenn der Flow ausgeführt wird, erhalten die Empfänger, die Sie in der E-Mail-Liste festgelegt haben, eine E-Mail, die in etwa wie folgt aussieht:

![Beispiel-E-Mail](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Erstellung von [Analytics-Abfragen](app-insights-analytics-using.md).
- Erfahren Sie mehr über [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->






