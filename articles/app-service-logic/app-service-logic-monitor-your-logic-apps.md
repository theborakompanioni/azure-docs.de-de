<properties 
	pageTitle="Überwachen Ihrer Logik-Apps in Azure App Service | Microsoft Azure" 
	description="Erfahren Sie, wie Sie die Ergebnisse Ihrer Logik-Apps überprüfen können." 
	authors="jeffhollan" 
	manager="erikre" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="logic-apps"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/22/2016"
	ms.author="jehollan"/>

# Überwachen von Logik-Apps

Nach dem [Erstellen einer Logik-App](app-service-logic-create-a-logic-app.md) können Sie den vollständigen Verlauf ihrer Ausführung im Azure-Portal einsehen. Sie können auch Dienste wie Azure-Diagnose und Azure-Warnungen einrichten, um Ereignisse in Echtzeit zu überwachen und in bestimmten Situationen benachrichtigt zu werden (beispielsweise, wenn innerhalb einer Stunde mehr als fünf Ausführungen nicht erfolgreich waren).

## Überwachen im Azure-Portal

Wählen Sie zum Anzeigen des Verlaufs **Durchsuchen** und dann **Logic Apps** aus. Eine Liste mit allen Logik-Apps Ihres Abonnements wird angezeigt. Wählen Sie die zu überwachende Logik-App aus. Daraufhin erscheint eine Liste mit allen Aktionen und Triggern, die für diese Logik-App aufgetreten sind.

![Übersicht](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Das Blatt bietet einige hilfreiche Abschnitte:

- In der **Zusammenfassung** werden alle Ausführungen und der **Triggerverlauf** aufgeführt.
	- Unter **All runs** (Alle Ausführungen) werden die neuesten Logik-App-Ausführungen aufgelistet. Sie können auf eine beliebige Zeile klicken, um Details zur jeweiligen Ausführung anzuzeigen. Wenn Sie auf die Kachel klicken, werden weitere Ausführungen aufgelistet.
	- Unter **Triggerverlauf** werden sämtliche Triggeraktivitäten für die Logik-App aufgelistet. Bei einer Triggeraktivität kann es sich um eine übersprungene Überprüfung auf neue Daten handeln (mit der etwa geprüft werden sollte, ob dem FTP-Server eine neue Datei hinzugefügt wurde). Bei erfolgreichen Aktivitäten wurden Daten zum Auslösen einer Logik-App zurückgegeben, und bei einem Fehler liegt ein Konfigurationsfehler vor.
- Mithilfe der **Diagnose** können Sie Laufzeitdetails und -ereignisse anzeigen und [Azure-Warnungen](#adding-azure-alerts) abonnieren.

### Anzeigen der Ausführungsdetails

Diese Ausführungsliste gibt Aufschluss über **Status**, **Startzeit** und **Dauer** einer bestimmten Ausführung. Wählen Sie eine beliebige Zeile aus, um Details zur jeweiligen Ausführung anzuzeigen.

Die Überwachungsansicht enthält die einzelnen Ausführungsschritte, die Eingaben und Ausgaben sowie ggf. ausgelöste Fehlermeldungen.

![Ausführung und Aktionen](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Sollten Sie weitere Details wie etwa die **Korrelations-ID** der Ausführung benötigen (kann für die REST-API verwendet werden), klicken Sie auf die Schaltfläche **Ausführungsdetails**. Dort finden Sie sämtliche Schritte, Statusinformationen und Ein-/Ausgaben für die Ausführung.

## Azure-Diagnose und -Warnungen

Zusätzlich zu den oben angegebenen Details des Azure-Portals und der REST-API können Sie Ihre Logik-App für die Verwendung der Azure-Diagnose konfigurieren, um ausführlichere Details zu erhalten und Debuggingmaßnahmen zu ergreifen.

1. Klicken Sie auf dem Logik-App-Blatt auf den Diagnosebereich.
1. Klicken Sie, um die **Diagnoseeinstellungen** zu konfigurieren.
1. Konfigurieren Sie einen Event Hub oder ein Speicherkonto für die Datenausgabe.

	![Azure-Diagnoseeinstellungen](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### Hinzufügen von Azure-Warnungen

Nachdem Sie die Diagnose konfiguriert haben, können Sie Azure-Warnungen hinzufügen, die bei einer Überschreitung bestimmter Schwellenwerte ausgelöst werden. Wählen Sie auf dem Blatt **Diagnose** die Kachel **Warnungen** und anschließend **Warnung hinzufügen** aus. Daraufhin werden Sie durch die Konfiguration einer Warnung auf der Grundlage einer Reihe von Schwellenwerten und Metriken geführt.

![Azure-Warnungsmetriken](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

**Bedingung**, **Schwellenwert** und **Zeitraum** können nach Bedarf konfiguriert werden. Zum Schluss können Sie noch eine Ziel-E-Mail-Adresse für Benachrichtigungen oder einen Webhook konfigurieren. Der [Anforderungstrigger](../connectors/connectors-native-reqres.md) in einer Logik-App kann ebenfalls für die Ausführung im Falle einer Warnung verwendet werden (um beispielsweise Aktionen wie [In Slack posten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [SMS senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app) oder [Nachricht einer Warteschlange hinzufügen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app) auszuführen).

### Azure-Diagnoseeinstellungen

Jedes dieser Ereignisse enthält Details zur Logik-App und zum Ereignis (beispielsweise den Status). Im Anschluss sehen Sie ein Beispiel für ein *ActionCompleted*-Ereignis:

```javascript
{
			"time": "2016-07-09T17:09:54.4773148Z",
			"workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
			"resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
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
					"subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
					"resourceGroupName": "MyResourceGroup",
					"workflowId": "cff00d5458f944d5a766f2f9ad142553",
					"workflowName": "MyLogicApp",
					"runId": "08587361146922712057",
					"location": "eastus",
					"actionName": "Http"
				},
				"correlation": {
					"actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
					"clientTrackingId": "my-custom-tracking-id"
				},
				"trackedProperties": {
					"myProperty": "<value>"
				}
			}
		}
```

Zur Nachverfolgung und Überwachung sind insbesondere zwei Eigenschaften hilfreich: *clientTrackingId* und *trackedProperties*.

#### Clientnachverfolgungs-ID

Der Wert der Clientnachverfolgungs-ID korreliert Ereignisse innerhalb der gesamten Ausführung einer Logik-App. Dies schließt auch geschachtelte Workflows ein, die im Rahmen einer Logik-App aufgerufen werden. Falls keine ID angegeben ist, wird automatisch eine generiert. Die Clientnachverfolgungs-ID kann aber auch manuell über einen Trigger angegeben werden. Hierzu muss in der Triggeranforderung (Anforderungstrigger, HTTP-Trigger oder Webhook-Trigger) ein `x-ms-client-tracking-id`-Header mit dem ID-Wert übergeben werden.

#### Nachverfolgte Eigenschaften

Nachverfolgte Eigenschaften können Aktionen in der Workflowdefinition hinzugefügt werden, um Eingaben oder Ausgaben in Diagnosedaten nachzuverfolgen. Dies kann hilfreich sein, wenn Sie Daten wie etwa eine Auftrags-ID in Ihrer Telemetrie nachverfolgen möchten. Schließen Sie zum Hinzufügen einer nachverfolgte Eigenschaft in einer Aktion die `trackedProperties`-Eigenschaft ein. Nachverfolgte Eigenschaften können nur die Eingaben und Ausgaben einer einzelnen Aktion nachverfolgen. Mit den `correlation`-Eigenschaften der Ereignisse ist jedoch eine aktionsübergreifende Korrelation innerhalb einer Ausführung möglich.

```javascript
{
	"myAction": {
		"type": "http",
		"inputs": {
			"uri": "http://uri",
			"headers": {
				"Content-Type": "application/json"
			},
			"body": "@triggerBody()"
		},
		"trackedProperties":{
			"myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
			"myActionHTTPValue": "@action()['outputs']['body']['foo']",
			"transactionId": "@action()['inputs']['body']['bar']"
		}
	}
}
```

### Erweitern Ihrer Lösungen

Sie können diese Telemetrie aus dem Event Hub oder Speicher auch in anderen Diensten wie [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite), [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) und [Power BI](https://powerbi.com) zur Echtzeitüberwachung Ihrer Integrationsworkflows verwenden.

## Nächste Schritte
- [Allgemeine Beispiele und Szenarien für Logik-Apps](app-service-logic-examples-and-scenarios.md)
- [Erstellen einer Bereitstellungsvorlage für Logik-Apps](app-service-logic-create-deploy-template.md)
- [Unternehmensintegrationsfeatures](app-service-logic-enterprise-integration-overview.md)

<!---HONumber=AcomDC_0727_2016-->