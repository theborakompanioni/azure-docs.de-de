---
title: Automatisieren von Azure Application Insights-Prozessen mithilfe von Logic Apps
description: "In diesem Artikel erfahren Sie, wie Sie wiederholbare Prozesse schnell automatisieren können, indem Sie Ihrer Logik-App den Application Insights-Connector hinzufügen."
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
ms.author: sewhee
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 06bfb75a07b7902fcf245271def5d40e9941a89e
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---

# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Automatisieren von Application Insights-Prozessen mithilfe von Logic Apps

Ertappen Sie sich dabei, wie Sie wiederholt dieselben Abfragen von Ihren Telemetriedaten ausführen, um zu überprüfen, ob Ihr Dienst ordnungsgemäß funktioniert? Möchten Sie diese Abfragen für die Suche nach Trends und Anomalien automatisieren und eigene Workflows dafür erstellen? Dann benötigen Sie den Azure Application Insights-Connector (Vorschau) für Logic Apps.

Mit dieser Integration können Sie zahlreiche Prozesse automatisieren, ohne eine einzige Codezeile zu schreiben. Mit dem Application Insights-Connector können Sie Logik-Apps erstellen und schnell beliebige Application Insights-Prozesse automatisieren. 

Sie können auch weitere Aktionen hinzufügen. Das Logic Apps-Feature von Azure App Service ermöglicht Hunderte von Aktionen. So können Sie mit einer Logik-App beispielsweise automatisch eine E-Mail-Benachrichtigung senden oder einen Fehler in Visual Studio Team Services protokollieren. Darüber hinaus können Sie eine der zahlreichen verfügbaren [Vorlagen](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) verwenden, um die Erstellung Ihrer Logik-App zu beschleunigen. 

## <a name="create-a-logic-app-for-application-insights"></a>Erstellen einer Logik-App für Application Insights

In diesem Tutorial erfahren Sie, wie Sie eine Logik-App erstellen, die den automatischen Clusteralgorithmus von Analytics verwendet, um Attribute in den Daten für eine Webanwendung zu gruppieren. Die Ergebnisse werden automatisch per E-Mail gesendet. Dies ist nur eins von vielen Beispielen für die gemeinsame Verwendung von Application Insights Analytics und Logic Apps. 

### <a name="step-1-create-a-logic-app"></a>Schritt 1: Erstellen einer Logik-App
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Bereich **Neu** die Option **Web und mobil** und anschließend **Logik-App** aus.

    ![Fenster für eine neue Logik-App](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Schritt 2: Erstellen eines Triggers für Ihre Logik-App
1. Wählen Sie im Fenster **Logik-App-Designer** unter **Starten Sie mit einem gängigen Trigger** die Option **Wiederholung** aus.

    ![Fenster „Logik-App-Designer“](./media/automate-with-logic-apps/logicapp2.png)

2. Wählen Sie im Feld **Häufigkeit** die Option **Tag** aus, und geben Sie anschließend im Feld **Intervall** den Wert **1** ein.

    ![Logik-App-Designer-Fenster „Wiederholung“](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Schritt 3: Hinzufügen einer Application Insights-Aktion
1. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen**.

2. Geben Sie im Suchfeld **Aktion auswählen** den Text **Azure Application Insights** ein.

3. Klicken Sie unter **Aktionen** auf **Azure Application Insights – Visualize Analytics query Preview** (Azure Application Insights – Analytics-Abfrage visualisieren (Vorschau)).

    ![Logik-App-Designer-Fenster „Aktion auswählen“](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Schritt 4: Herstellen einer Verbindung mit einer Application Insights-Ressource

Für diesen Schritt benötigen Sie eine Anwendungs-ID und einen API-Schlüssel für Ihre Ressource. Diese können Sie über das Azure-Portal abrufen, wie in der folgenden Abbildung zu sehen:

![Anwendungs-ID im Azure-Portal](./media/automate-with-logic-apps/appid.png) 

Geben Sie einen Namen für Ihre Verbindung sowie die Anwendungs-ID und den API-Schlüssel ein.

![Verbindungsfenster für den Logik-App-Designer-Flow](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Schritt 5: Angeben der Analytics-Abfrage und des Diagrammtyps
Im folgenden Beispiel wählt die Abfrage die nicht erfolgreichen Anforderungen des letzten Tages aus und gleicht sie mit Ausnahmen ab, die im Zusammenhang mit dem Vorgang aufgetreten sind. Analytics gleicht die nicht erfolgreichen Anforderungen auf der Grundlage des Bezeichners „operation_Id“ ab. Die Ergebnisse werden anschließend mithilfe des automatischen Clusteralgorithmus segmentiert. 

Vergewissern Sie sich beim Erstellen eigener Abfragen, dass sie ordnungsgemäß in Analytics funktionieren, bevor Sie sie zu Ihrem Flow hinzufügen.

1. Fügen Sie im Feld **Abfrage** die folgende Analytics-Abfrage hinzu: 

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

2. Wählen Sie im Feld **Diagrammtyp** die Option **HTML-Tabelle** aus.

    ![Konfigurationsfenster für Analytics-Abfragen](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>Schritt 6: Konfigurieren der Logik-App zum Senden von E-Mails

1. Klicken Sie auf **Neuer Schritt**, und wählen Sie anschließend **Aktion hinzufügen** aus.

2. Geben Sie im Suchfeld den Text **Office 365 Outlook** ein.

3. Klicken Sie auf **Office 365 Outlook – E-Mail senden**.

    ![Auswählen von Office 365 Outlook](./media/automate-with-logic-apps/flow2b.png)

4. Gehen Sie im Fenster **E-Mail senden** wie folgt vor:

   a. Geben Sie die E-Mail-Adresse des Empfängers ein.

   b. Geben Sie einen Betreff für die E-Mail ein.

   c. Klicken Sie auf das Feld **Text**, und wählen Sie im dynamischen Inhaltsmenü, das auf der rechten Seite geöffnet wird, das Feld **Text** aus.

   d. Klicken Sie auf **Erweiterte Optionen anzeigen**.

      ![Konfiguration von Office 365 Outlook](./media/automate-with-logic-apps/flow5.png)

5. Gehen Sie im dynamischen Inhaltsmenü wie folgt vor:

    a. Wählen Sie **Anlagenname** aus.

    b. Wählen Sie **Anlageninhalt** aus.
    
    c. Wählen Sie im Feld **Ist HTML** die Option **Ja** aus.

      ![Bildschirm „Konfiguration von Office 365-E-Mails“](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Schritt 7: Speichern und Testen Ihrer Logik-App
* Klicken Sie zum Speichern der Änderungen auf **Speichern**.

Sie können warten, bis die Logik-App durch den Trigger ausgeführt wird, oder **Ausführen** auswählen, um sie sofort auszuführen.

![Bildschirm für die Logik-App-Erstellung](./media/automate-with-logic-apps/step7.png)

Wenn Ihre Logik-App ausgeführt wird, erhalten die Empfänger, die Sie in der E-Mail-Liste festgelegt haben, eine E-Mail, die in etwa wie folgt aussieht:

![E-Mail der Logik-App](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Erstellung von [Analytics-Abfragen](app-insights-analytics-using.md).
- Informieren Sie sich ausführlicher über [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->






