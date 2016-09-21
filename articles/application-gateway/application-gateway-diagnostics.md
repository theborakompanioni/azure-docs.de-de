<properties 
   pageTitle="Überwachen von Zugriffs- und Leistungsprotokollen für das Application Gateway | Microsoft Azure"
   description="Erfahren Sie, wie Sie Zugriffs- und Leistungsprotokolle für das Application Gateway aktivieren und verwalten."
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# Diagnoseprotokollierung für Application Gateway

Sie können in Azure verschiedene Protokolltypen verwenden, um Application Gateways zu verwalten und eventuelle Fehler zu beheben. Auf einige dieser Protokolle können Sie über das Portal zugreifen, und alle Protokolle können aus einem Azure-Blob Storage extrahiert und in anderen Tools wie [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md),Excel und PowerBI angezeigt werden. In der folgenden Liste finden Sie weitere Informationen über die verschiedenen Typen von Protokollen.

- **Überwachungsprotokolle:** Sie können [Azure-Überwachungsprotokolle](../azure-portal/insights-debugging-with-events.md) (ehemals Betriebsprotokolle) verwenden, um alle an Ihr Azure-Abonnement übermittelten Vorgänge und deren Status anzuzeigen. Überwachungsprotokolle sind standardmäßig aktiviert und können im Azure-Vorschauportal angezeigt werden.
- **Zugriffsprotokolle:** Sie können diese Art von Protokoll verwenden, um Application Gateway-Zugriffsmuster anzuzeigen und wichtige Informationen zu analysieren, z.B. IP des Aufrufers, angeforderte URL, Antwortlatenz, Rückgabecode und eingehende und ausgehende Bytes. Das Zugriffsprotokoll wird alle 300 Sekunden erstellt. Dieses Protokoll enthält einen Datensatz pro Instanz von Application Gateway. Die Application Gateway-Instanz kann mit der instanceId-Eigenschaft identifiziert werden.
- **Leistungsprotokolle:** Sie können dieses Protokoll verwenden, um die Leistung von Application Gateway-Instanzen anzuzeigen. In diesem Protokoll werden Leistungsinformationen pro Instanz erfasst, z.B. insgesamt bereitgestellte Anforderungen, Durchsatz in Byte, Anzahl von Anforderungen mit Fehlern, Anzahl von fehlerfreien und fehlerhaften Back-End-Instanzen. Das Leistungsprotokoll wird alle 60 Sekunden erstellt.

>[AZURE.WARNING] Protokolle sind nur für Ressourcen verfügbar, die im Ressourcen-Manager-Bereitstellungsmodell bereitgestellt werden. Sie können Protokolle nicht für Ressourcen im klassischen Bereitstellungsmodell verwenden. Für ein besseres Verständnis der beiden Modelle lesen Sie den Artikel [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../resource-manager-deployment-model.md).

## Aktivieren der Protokollierung
Die Überwachungsprotokollierung ist automatisch für alle Ressourcen-Manager-Ressourcen aktiviert. Sie müssen die Zugriffs- und Leistungsprotokollierung aktivieren, um mit der Erfassung von Daten aus diesen Protokollen zu beginnen. Führen Sie die folgenden Schritte aus, um die Protokollierung zu aktivieren.

1. Notieren Sie sich die Ressourcen-ID Ihres Speicherkontos, unter dem die Protokolldaten gespeichert werden. Diese weist folgendes Format auf: /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppenname>/providers/Microsoft.Storage/storageAccounts/<Speicherkontoname>. Sie können jedes Speicherkonto Ihres Abonnements verwenden. Sie können das Vorschauportal verwenden, um nach diesen Informationen zu suchen.

	![Vorschauportal – Application Gateway-Diagnose](./media/application-gateway-diagnostics/diagnostics1.png)

2. Notieren Sie sich die Ressourcen-ID Ihres Application Gateway, für das die Protokollierung aktiviert werden soll. Diese weist folgendes Format auf: /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengrupppenname>/providers/Microsoft.Network/applicationGateways/<Anwendungsgatewayname>. Sie können das Vorschauportal verwenden, um nach diesen Informationen zu suchen.

	![Vorschauportal – Application Gateway-Diagnose](./media/application-gateway-diagnostics/diagnostics2.png)

3. Aktivieren Sie die Diagnoseprotokollierung mit dem folgenden PowerShell-Cmdlet.

		Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true 	

>[AZURE.INFORMATION] Für Überwachungsprotokolle ist kein separates Speicherkonto erforderlich. Für die Nutzung des Speichers für die Zugriffs- und Leistungsprotokollierung fallen Dienstgebühren an.

## Überwachungsprotokoll

Dieses Protokoll (früher als "Betriebsprotokoll" bekannt) wird standardmäßig von Azure generiert. Die Protokolle werden 90 Tage lang im Azure-Ereignisprotokollspeicher aufbewahrt. Weitere Informationen zu diesen Protokollen finden Sie im Artikel [Anzeigen von Ereignis- und Überwachungsprotokollen](../azure-portal/insights-debugging-with-events.md).

## Zugriffsprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie in den vorherigen Schritten für die jeweiligen Application Gateways aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Jeder Application Gateway-Zugriff wird wie im folgenden Beispiel gezeigt im JSON-Format protokolliert.

	{
		"resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
		"operationName": "ApplicationGatewayAccess",
		"time": "2016-04-11T04:24:37Z",
		"category": "ApplicationGatewayAccessLog",
		"properties": {
			"instanceId":"ApplicationGatewayRole_IN_0",
			"clientIP":"37.186.113.170",
			"clientPort":"12345",
			"httpMethod":"HEAD",
			"requestUri":"/xyz/portal",
			"requestQuery":"",
			"userAgent":"-",
			"httpStatus":"200",
			"httpVersion":"HTTP/1.0",
			"receivedBytes":"27",
			"sentBytes":"202",
			"timeTaken":"359",
			"sslEnabled":"off"
		}
	}

## Leistungsprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie in den vorherigen Schritten für die jeweiligen Application Gateways aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Die folgenden Daten werden protokolliert:

	{
		"resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
		"operationName": "ApplicationGatewayPerformance",
		"time": "2016-04-09T00:00:00Z",
		"category": "ApplicationGatewayPerformanceLog",
		"properties": 
		{
			"instanceId":"ApplicationGatewayRole_IN_1",
			"healthyHostCount":"4",
			"unHealthyHostCount":"0",
			"requestCount":"185",
			"latency":"0",
			"failedRequestCount":"0",
			"throughput":"119427"
		}
	}

## Anzeigen und Analysieren des Überwachungsprotokolls

Mit einer der folgenden Methoden können Sie die Überwachungsprotokolldaten anzeigen und analysieren:

- **Azure-Tools:** Rufen Sie Informationen aus den Überwachungsprotokollen über Azure PowerShell, die Azure-Befehlszeilenschnittstelle, die Azure REST-API oder über das Azure-Vorschauportal ab. Schrittweise Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md).
- **Power BI:** Wenn Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Audit Logs Content Pack for Power BI](https://powerbi.microsoft.com/de-DE/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren, die Sie im Istzustand oder angepasst verwenden können.

## Anzeigen und Analysieren des Zugriffs- und Leistungsprotokolls

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) kann die Leistungsindikator- und Ereignisprotokolldateien aus Ihrem Blobspeicherkonto erfassen und enthält Visualisierungen und leistungsfähige Suchfunktionen, um Ihre Protokolle zu analysieren.

Sie können auch eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für Zugriffs- und Leistungsprotokolle abrufen. Sobald Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, PowerBI oder einem anderen Datenvisualisierungstool anzeigen.

>[AZURE.TIP] Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von Github verwenden

## Nächste Schritte

- Visualisieren von Leistungsindikator- und Ereignisprotokollen mit [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
- Blogbeitrag [Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx).
- Blogbeitrag [View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).

<!---HONumber=AcomDC_0907_2016-->