---
title: Automatisieren von Azure Application Insights-Prozessen mit einer Azure-Logik-App
description: "In diesem Artikel erfahren Sie, wie Sie wiederholbare Prozesse schnell durch das Hinzufügen des Application Insights-Connectors zu Ihrer Azure-Logik-App automatisieren können."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 571a76253fa62a89f325e0c295e9a7e98e201079
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---

# <a name="automate-application-insights-processes-with-an-azure-logic-app"></a>Automatisieren von Application Insights-Prozessen mit einer Azure-Logik-App

Ertappen Sie sich dabei, wie Sie wiederholt dieselben Abfragen von Ihren Telemetriedaten ausführen, um zu überprüfen, ob Ihr Dienst ordnungsgemäß ausgeführt wird? Möchten Sie diese Abfragen zum Suchen nach Trends und Anomalien automatisieren und Ihre eigenen Workflows zu diesen erstellen? Dann ist der Application Insights-Connector (Vorschau) für Azure Logic Apps genau das Richtige für Sie!
Mit dieser Integration können nun zahlreiche Prozesse automatisiert werden, ohne dass eine einzige Codezeile geschrieben werden muss. Mit dem Application Insights-Connector können Sie Ihre Logik-App erstellen, um beliebige Application Insights-Prozesse schnell automatisieren zu können. Sie können auch weitere Aktionen hinzufügen. Mit Logic Apps stehen Ihnen Hunderte von Aktionen zur Verfügung. Beispielsweise können Sie automatisch eine E-Mail-Benachrichtigung senden oder als Teil Ihrer Logik-App einen Fehler in Visual Studio Team Services erstellen. Sie können auch eine der zahlreichen [Vorlagen](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) verwenden, die für Logic Apps verfügbar sind. Diese Vorlagen beschleunigen den Vorgang zum Erstellen Ihrer Logik-App. 

## <a name="tutorial-for-creating-an-azure-logic-app-for-application-insights"></a>Tutorial zum Erstellen einer Azure-Logik-App für Application Insights

In diesem Tutorial erfahren Sie, wie Sie eine Azure-Logik-App erstellen, die zum Gruppieren von Attributen in den Daten für eine Webanwendung den automatischen Clusteralgorithmus von Analytics verwendet. Der Flow sendet die Ergebnisse automatisch per E-Mail. Dies ist nur ein Beispiel dafür, wie Sie Application Insights Analytics und Azure Logic Apps gemeinsam verwenden können. 

### <a name="step-1-create-a-logic-app"></a>Schritt 1: Erstellen einer Logik-App
1. Melden Sie sich bei https://portal.azure.com an.
2. Erstellen Sie eine neue Logik-App aus dem Menü „Neu/Web + Mobil“.

![Bildschirm der neuen Logik-App](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Schritt 2: Erstellen eines Triggers für Ihre Logik-App
1.  Wählen Sie im Logic App-Designer unter „Starten Sie mit einem gängigen Trigger“ „Wiederholung“ aus.
2.  Legen Sie die Häufigkeit auf „Tag mit einem Intervall von 1“ fest.

![Dialogfeld „Trigger der Logik-App“](./media/automate-with-logic-apps/logicapp2.png)

![Dialogfeld „Häufigkeit der Logik-App“](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-azure-application-insights-action"></a>Schritt 3: Hinzufügen einer Azure Application Insights-Aktion
1. Klicken Sie auf **Neuer Schritt** und dann auf **Aktion hinzufügen**.
2. Suchen Sie nach Azure Application Insights.
3. Klicken Sie auf „Azure Application Insights“ – „Analytics-Abfrage visualisieren“ Vorschau.

![Bildschirm „Analytics-Abfrage ausführen“](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Schritt 4: Herstellen einer Verbindung mit einer Application Insights-Ressource

**Voraussetzung**

Für die Durchführung dieses Schritts benötigen Sie eine Anwendungs-ID und einen API-Schlüssel für Ihre Ressource. Diese können Sie über das Azure-Portal abrufen, wie in der folgenden Abbildung veranschaulicht wird:

![Anwendungs-ID im Azure-Portal](./media/automate-with-logic-apps/appid.png) 

- Geben Sie einen Namen für Ihre Verbindung zusammen mit der Anwendungs-ID und dem API-Schlüssel ein.

![Bildschirm „Flowverbindung“](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Schritt 5: Angeben der Analytics-Abfrage und des Diagrammtyps
In diesem Beispiel werden die fehlerhaften Anforderungen des letzten Tages ausgewählt und mit Ausnahmen korreliert, die im Zusammenhang mit dem Vorgang aufgetreten sind. Analytics führt die Korrelation basierend auf dem Bezeichner „operation_ID“ durch. Die Abfrage segmentiert die Ergebnisse dann mit dem automatischen Clusteralgorithmus. Wenn Sie eigene Abfragen erstellen, überprüfen Sie unbedingt, ob diese ordnungsgemäß in Analytics funktionieren, bevor Sie sie zu Ihrem Flow hinzufügen.

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
![Bildschirm „Konfiguration von Analytics-Abfragen“](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-app-to-send-email"></a>Schritt 6: Konfigurieren der App zum Senden von E-Mails

1. Klicken Sie auf **Neuer Schritt**, und wählen Sie anschließend **Aktion hinzufügen**.
2. Suchen Sie nach „Office 365 Outlook“.
3. Klicken Sie auf „Office 365 Outlook – E-Mail senden“.
![Auswahl von Office 365 Outlook](./media/automate-with-logic-apps/flow2b.png)

4. Fügen Sie in der E-Mail-Aktion die folgenden Daten hinzu:
 - Geben Sie die E-Mail-Adresse des Empfängers ein.
 - Geben Sie einen Betreff für die E-Mail ein.
 - Setzen Sie den Cursor in das **Textfeld**, und wählen Sie im dynamischen Inhaltsmenü, das auf der rechten Seite geöffnet wird, das Feld **Text**.
 - Klicken Sie auf **Erweiterte Optionen anzeigen**.

 ![Konfiguration von Office 365 Outlook](./media/automate-with-logic-apps/flow5.png)

5. Führen Sie im dynamischen Inhaltsmenü folgende Schritte durch:
- Wählen Sie **Anlagenname**.
- Wählen Sie **Anlageninhalt**.
- Wählen Sie **Ja** im Feld **HTML festlegen**.

![Bildschirm „Konfiguration von Office 365-E-Mails“](./media/automate-with-logic-apps/flow7.png)
### <a name="step-7-save-and-test-your-logic-app"></a>Schritt 7: Speichern und Testen Ihrer Logik-App
1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
1. Sie können warten, bis der Trigger die Logik-App ausgeführt hat oder sie mit **Ausführen** sofort ausführen.

![Bildschirm „Erstellen der Logik-App“](./media/automate-with-logic-apps/step7.png)

Wenn Ihre Logik-App ausgeführt wird, erhalten die Empfänger, die Sie in der E-Mail-Liste festgelegt haben, eine E-Mail, die in etwa wie folgt aussieht:

![Logik-App-E-Mail](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Erstellung von [Analytics-Abfragen](app-insights-analytics-using.md).
- Erfahren Sie mehr über [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->






