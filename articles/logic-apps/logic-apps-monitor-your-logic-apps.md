---
title: "Überprüfen des Status, Einrichten der Protokollierung und Empfangen von Warnungen – Azure Logic Apps | Microsoft-Dokumentation"
description: "Überwachen des Status und der Leistung für Logik-Apps, Protokollieren von Diagnosedaten und Einrichten von Warnungen"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: 8bacd408e49eb5f6eeb4b8f7eb10feac21830bb0
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps

Nachdem Sie [eine Logik-App erstellt und ausgeführt haben](../logic-apps/logic-apps-create-a-logic-app.md), können Sie dafür den Ausführungsverlauf, den Triggerverlauf, den Status und die Leistung überprüfen. Für die Echtzeit-Ereignisüberwachung und die Durchführung eines umfassenderen Debuggens können Sie die [Diagnoseprotokollierung](#azure-diagnostics) für Ihre Logik-App einrichten. Auf diese Weise können Sie [nach Ereignissen suchen und diese anzeigen](#find-events), z.B. Triggerereignisse, Ausführungsereignisse und Aktionsereignisse. Sie können diese [Diagnosedaten auch mit anderen Diensten verwenden](#extend-diagnostic-data), z.B. Azure Storage und Azure Event Hubs. 

Richten Sie [Warnungen](#add-azure-alerts) ein, um Benachrichtigungen zu Ausfällen oder anderen möglichen Problemen zu erhalten. Beispielsweise können Sie eine Warnung erstellen, mit der erkannt wird, „wenn in einer Stunde mehr als fünf Ausführungen fehlschlagen“. Sie können die Überwachung, Nachverfolgung und Protokollierung auch programmgesteuert einrichten, indem Sie [Einstellungen und Eigenschaften für Azure-Diagnoseereignisse](#diagnostic-event-properties) verwenden.

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Anzeigen des Ausführungs- und Triggerverlaufs für Ihre Logik-App

1. Wählen Sie im Azure-Hauptmenü die Option **Weitere Dienste**, um im [Azure-Portal](https://portal.azure.com) Ihre Logik-App zu ermitteln. Suchen Sie im Suchfeld nach „Logik-Apps“, und wählen Sie **Logik-Apps**.

   ![Suchen nach Ihrer Logik-App](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   Im Azure-Portal werden alle Logik-Apps angezeigt, die Ihrem Azure-Abonnement zugeordnet sind. 

2. Wählen Sie Ihre Logik-App aus, und wählen Sie dann die Option **Übersicht**.

   Im Azure-Portal wird der Ausführungs- und Triggerverlauf für Ihre Logik-App angezeigt. Beispiel:

   ![Ausführungs- und Triggerverlauf von Logik-Apps](media/logic-apps-monitor-your-logic-apps/overview.png)

   * Unter **Ausführungsverlauf** werden alle Ausführungen für Ihre Logik-App angezeigt. 
   * Unter **Triggerverlauf** werden sämtliche Triggeraktivitäten für Ihre Logik-App angezeigt.

   Statusbeschreibungen finden Sie unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Falls Sie die gewünschten Daten nicht finden, können Sie in der Symbolleiste die Option **Aktualisieren** wählen.

3. Wählen Sie unter **Ausführungsverlauf** eine bestimmte Ausführung aus, um die dazugehörigen Schritte anzuzeigen. 

   In der Überwachungsansicht werden die einzelnen Schritte der Ausführung angezeigt. Beispiel:

   ![Aktionen für eine bestimmte Ausführung](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. Wählen Sie **Ausführungsdetails**, um weitere Details zur Ausführung zu erhalten. Unter diesen Informationen werden die Schritte, der Status, die Eingaben und die Ausgaben für die Ausführung zusammengefasst. 

   ![Auswählen von „Ausführungsdetails“](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Sie können beispielsweise die **Korrelations-ID** der Ausführung abrufen, die Sie ggf. benötigen, wenn Sie die [REST-API für Logik-Apps](https://docs.microsoft.com/rest/api/logic) verwenden.

5. Wählen Sie diesen Schritt aus, um die Details zu einem bestimmten Schritt zu erhalten. Sie können jetzt Details wie Eingaben, Ausgaben und Fehler für diesen Schritt prüfen. Beispiel:

   ![Schrittdetails](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Alle Informationen und Ereignisse zur Laufzeit werden im Logic Apps-Dienst verschlüsselt. Sie werden nur entschlüsselt, wenn ein Benutzer das Anzeigen dieser Daten anfordert. Sie können den Zugriff auf diese Ereignisse auch mit der [rollenbasierten Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-what-is.md) steuern.

6. Wechseln Sie zurück in den Bereich **Übersicht**, um Details zu einem bestimmten Triggerereignis zu erhalten. Wählen Sie unter **Triggerverlauf** das Triggerereignis aus. Sie können jetzt Details wie Eingaben und Ausgaben prüfen, z.B.:

   ![Ausgabedetails des Triggerereignisses](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Aktivieren der Diagnoseprotokollierung für Ihre Logik-App

Wenn Sie ein umfangreicheres Debuggen mit Runtimedetails und -ereignissen durchführen möchten, können Sie die Diagnoseprotokollierung mit [Azure Log Analytics](../log-analytics/log-analytics-overview.md) einrichten. Log Analytics ist ein Dienst in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), der Ihre cloudbasierten und lokalen Umgebungen überwacht, um Sie beim Sicherstellen der Verfügbarkeit und Leistung zu unterstützen. 

Sie müssen über einen OMS-Arbeitsbereich verfügen, bevor Sie beginnen können. Informieren Sie sich, wie Sie [einen OMS-Arbeitsbereich erstellen](../log-analytics/log-analytics-get-started.md).

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer Logik-App, und wählen Sie sie aus. 

2. Wählen Sie im Menü des Blatts „Logik-App“ unter **Monitoring** (Überwachung) die Option **Diagnose** > **Diagnoseeinstellungen**.

   ![Navigation zu „Monitoring“ (Überwachung) > „Diagnose“ > „Diagnoseeinstellungen“](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. Wählen Sie unter **Diagnoseeinstellungen** die Option **Ein**.

   ![Aktivieren der Diagnoseprotokolle](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Wählen Sie anschließend den OMS-Arbeitsbereich und die Ereigniskategorie wie gezeigt für die Protokollierung aus:

   1. Wählen Sie **An Log Analytics senden**. 
   2. Wählen Sie unter **Log Analytics** die Option **Konfigurieren**. 
   3. Wählen Sie unter **OMS-Arbeitsbereiche** den OMS-Arbeitsbereich aus, den Sie für die Protokollierung verwenden möchten.
   4. Wählen Sie unter **Protokoll** die Kategorie **WorkflowRuntime** aus.
   5. Wählen Sie das Metrikintervall aus.
   6. Wenn Sie fertig sind, wählen Sie **Speichern** aus.

   ![Auswählen des OMS-Arbeitsbereichs und der Daten für die Protokollierung](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

Nun können Sie nach Ereignissen und anderen Daten für Triggerereignisse, Ausführungsereignisse und Aktionsereignisse suchen.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Suchen nach Ereignissen und Daten für Ihre Logik-App

Führen Sie die hier angegebenen Schritte aus, um nach Ereignissen in Ihrer Logik-App zu suchen, z.B. Triggerereignisse, Ausführungsereignisse und Aktionsereignisse, und diese anzuzeigen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Weitere Dienste**. Suchen Sie nach „log analytics“, und wählen Sie dann wie hier gezeigt die Option **Log Analytics**:

   ![Auswählen von „Log Analytics“](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. Wählen Sie unter **Log Analytics** Ihren OMS-Arbeitsbereich aus. 

   ![Auswählen Ihres OMS-Arbeitsbereichs](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. Wählen Sie unter **Verwaltung** die Option **OMS-Portal**.

   ![Auswählen von „OMS-Portal“](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. Wählen Sie auf Ihrer OMS-Startseite die Option **Protokollsuche**.

   ![Auswählen der Option „Protokollsuche“ auf Ihrer OMS-Startseite](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   Oder

   ![Auswählen von „Protokollsuche“ im OMS-Menü](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. Geben Sie im Suchfeld ein Feld an, nach dem Sie suchen möchten, und drücken Sie die **EINGABETASTE**. Wenn Sie mit der Eingabe beginnen, werden in OMS mögliche Übereinstimmungen und verfügbare Vorgänge angezeigt. 

   Wählen Sie beispielsweise diese Suchabfrage aus, um nach den aufgetretenen Top 10-Ereignissen zu suchen: **Category=WorkflowRuntime |top 10**

   ![Eingeben der Suchzeichenfolge](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Informieren Sie sich über das [Suchen nach Daten in Log Analytics](../log-analytics/log-analytics-log-searches.md).

6. Wählen Sie auf der Ergebnisseite in der Leiste auf der linken Seite den Zeitrahmen für die Anzeige aus.
Wählen Sie **+Hinzufügen**, um die Abfrage durch das Hinzufügen eines Filters zu verfeinern.

   ![Auswählen des Zeitrahmens für Abfrageergebnisse](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. Geben Sie unter **Filter hinzufügen** den Filternamen ein, damit Sie den gewünschten Filter ermitteln können. Wählen Sie den Filter aus, und wählen Sie **+Hinzufügen**.

   In diesem Beispiel wird das Wort „Status“ verwendet, um unter **AzureDiagnostics** nach fehlgeschlagenen Ereignissen zu suchen.
   Hier ist der Filter für **status_s** bereits ausgewählt.

   ![Auswählen des Filters](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. Wählen Sie in der linken Leiste den Filterwert aus, den Sie verwenden möchten, und wählen Sie **Übernehmen**.

   ![Auswählen des Filterwerts und der Option „Übernehmen“](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. Navigieren Sie nun zurück zu der Abfrage, die Sie erstellen. Ihre Abfrage wird mit Ihrem ausgewählten Filter und Wert aktualisiert. Die vorherigen Ergebnisse sind jetzt auch gefiltert.

   ![Zurückwechseln zur Abfrage mit gefilterten Ergebnissen](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. Wählen Sie **Speichern**, um die Abfrage zur späteren Verwendung zu speichern. Informieren Sie sich über das [Speichern Ihrer Abfrage](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Erweitern der Nutzung von Diagnosedaten mit anderen Diensten

In Verbindung mit Azure Log Analytics können Sie die Nutzung der Diagnosedaten Ihrer Logik-App um andere Azure-Dienste erweitern, z.B.: 

* [Archivieren von Azure-Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Streamen von Azure-Diagnoseprotokollen an Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Sie können die Überwachung in Echtzeit dann durchführen, indem Sie Telemetriedaten und Analysen anderer Dienste verwenden, z.B. [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) und [Power BI](../log-analytics/log-analytics-powerbi.md). Beispiel:

* [Datenverbindung: Erfahren Sie mehr über Datenstromeingaben aus Ereignissen in Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Stream Analytics und Power BI: Ein Dashboard mit Echtzeitanalyse von Streamingdaten](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Stellen Sie basierend auf den einzurichtenden Optionen sicher, dass Sie zuerst [ein Azure-Speicherkonto](../storage/storage-create-storage-account.md) oder [einen Azure Event Hub](../event-hubs/event-hubs-create.md) erstellen. Wählen Sie anschließend die Optionen aus, an die Sie Diagnosedaten senden möchten:

![Senden von Daten an das Azure-Speicherkonto oder einen Event Hub](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Beibehaltungsdauern gelten nur, wenn Sie sich für die Verwendung eines Speicherkontos entscheiden.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Einrichten von Warnungen für Ihre Logik-App

Richten Sie [Warnungen in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) ein, um bestimmte Metriken oder überschrittene Schwellenwerte für Ihre Logik-App zu überwachen. Informieren Sie sich über [Metriken in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

Führen Sie die hier angegebenen Schritte aus, um Warnungen ohne [Azure Log Analytics](../log-analytics/log-analytics-overview.md) einzurichten. [Richten Sie auch Log Analytics ein](#azure-diagnostics), wenn Sie erweiterte Kriterien und Aktionen für Warnungen verwenden möchten.

1. Klicken Sie im Menü des Blatts „Logik-App“ unter **Monitoring** (Überwachung) auf **Diagnose** > **Warnungsregeln** > **Warnung hinzufügen**. Dies ist hier dargestellt:

   ![Hinzufügen einer Warnung für Ihre Logik-App](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. Erstellen Sie auf dem Blatt **Warnungsregel hinzufügen** wie hier gezeigt eine Warnung:

   1. Wählen Sie unter **Ressource** Ihre Logik-App aus, falls Sie dies noch nicht getan haben. 
   2. Geben Sie einen Namen und eine Beschreibung für Ihre Warnung ein.
   3. Wählen Sie eine **Metrik** oder ein Ereignis aus, das Sie nachverfolgen möchten.
   4. Wählen Sie eine **Bedingung** aus, geben Sie einen **Schwellenwert** für die Metrik an, und wählen Sie die **Periode** für die Überwachung der Metrik aus.
   5. Wählen Sie aus, ob für die Warnung eine E-Mail gesendet werden soll. 
   6. Geben Sie beliebige weitere E-Mail-Adressen zum Senden der Warnung an. 
   Sie können auch eine Webhook-URL angeben, an die Sie die Warnung senden möchten.

   Bei dieser Regel wird beispielsweise eine Warnung gesendet, wenn innerhalb einer Stunde fünf oder mehr Ausführungen fehlschlagen:

   ![Erstellen einer metrischen Warnungsregel](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> Zum Ausführen einer Logik-App über eine Warnung können Sie den [Anforderungstrigger](../connectors/connectors-native-reqres.md) in Ihren Workflow einbinden, um beispielsweise die folgenden Aufgaben durchzuführen:
> 
> * [Posten an Slack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Senden eines Texts](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Hinzufügen von Nachrichten zu einer Warteschlange](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Einstellungen und Details für Azure-Diagnoseereignisse

Jedes Diagnoseereignis enthält Details zu Ihrer Logik-App und zum Ereignis, z.B. Status, Startzeit, Endzeit usw. Für die programmgesteuerte Einrichtung der Überwachung, Nachverfolgung und Protokollierung können Sie diese Details mit der [REST-API für Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) und der [REST-API für Azure-Diagnose](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows) verwenden.

Beispielsweise verfügt das `ActionCompleted`-Ereignis über die Eigenschaften `clientTrackingId` und `trackedProperties`, die Sie für die Nachverfolgung und Überwachung nutzen können:

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* `clientTrackingId`: Wenn sie nicht angegeben ist, wird diese ID von Azure automatisch generiert, und Ereignisse werden für eine Ausführung der Logik-App korreliert. Dies gilt auch für geschachtelte Workflows, die über die Logik-App aufgerufen werden. Sie können diese ID manuell per Trigger angeben, indem Sie einen `x-ms-client-tracking-id`-Header mit Ihrem benutzerdefinierten ID-Wert in der Triggeranforderung übergeben. Sie können einen Anforderungstrigger, HTTP-Trigger oder Webhooktrigger verwenden.

* `trackedProperties`: Zum Nachverfolgen der Ein- oder Ausgaben in Diagnosedaten können Sie nachverfolgte Eigenschaften Aktionen in der JSON-Definition Ihrer Logik-App hinzufügen. Nachverfolgte Eigenschaften können nur die Ein- und Ausgaben einer einzelnen Aktion nachverfolgen. Mit den `correlation`-Eigenschaften der Ereignisse ist aber eine aktionsübergreifende Korrelation innerhalb einer Ausführung möglich.

  Fügen Sie den Abschnitt `trackedProperties` und die gewünschten Eigenschaften der Aktionsdefinition hinzu, um eine oder mehrere Eigenschaften nachzuverfolgen. Angenommen, Sie möchten in Ihrer Telemetrie ein Datenelement nachverfolgen, z.B. eine „Auftrags-ID“:

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Vorlagen für die Bereitstellungs- und Versionsverwaltung von Logik-Apps](../logic-apps/logic-apps-create-deploy-template.md)
* [Übersicht: B2B-Szenarien und Kommunikation mit dem Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Überwachen von B2B-Nachrichten](../logic-apps/logic-apps-monitor-b2b-message.md)
