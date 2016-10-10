<properties 
   pageTitle="Überwachen von Zugriffs- und Leistungsprotokollen sowie Metriken für Application Gateway | Microsoft Azure"
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
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# Diagnoseprotokollierung und Metriken für Application Gateway

Azure bietet die Möglichkeit, Ressourcen anhand von Protokollierung und Metriken zu überwachen.

[**Protokollierung**](#enable-logging-with-powershell): Die Protokollierung ermöglicht das Speichern und Nutzen von Leistungs-, Zugriffs- und anderen Protokollen von einer Ressource für Überwachungszwecke.

[**Metriken**](#metrics): Application Gateway verfügt derzeit über eine Metrik. Mit dieser Metrik wird der Durchsatz des Anwendungsgateways in Bytes pro Sekunde gemessen.

Sie können in Azure verschiedene Protokolltypen verwenden, um Anwendungsgateways zu verwalten und eventuelle Fehler zu beheben. Auf einige dieser Protokolle können Sie über das Portal zugreifen, und alle Protokolle können aus einem Azure-Blob Storage extrahiert und in anderen Tools wie [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md),Excel und PowerBI angezeigt werden. In der folgenden Liste finden Sie weitere Informationen über die verschiedenen Typen von Protokollen:

- **Überwachungsprotokolle:** Sie können [Azure-Überwachungsprotokolle](../azure-portal/insights-debugging-with-events.md) (ehemals Betriebsprotokolle) verwenden, um alle an Ihr Azure-Abonnement übermittelten Vorgänge und deren Status anzuzeigen. Überwachungsprotokolle sind standardmäßig aktiviert und können im Azure-Vorschauportal angezeigt werden.
- **Zugriffsprotokolle:** Sie können diese Art von Protokoll verwenden, um Anwendungsgateway-Zugriffsmuster anzuzeigen und wichtige Informationen zu analysieren, z.B. die IP des Aufrufers, die angeforderte URL, die Antwortlatenz, den Rückgabecode sowie eingehende und ausgehende Bytes. Das Zugriffsprotokoll wird alle 300 Sekunden erstellt. Dieses Protokoll enthält einen Datensatz pro Instanz von Application Gateway. Die Application Gateway-Instanz kann anhand der instanceId-Eigenschaft identifiziert werden.
- **Leistungsprotokolle:** Mithilfe dieses Protokolls können Sie die Leistung von Application Gateway-Instanzen anzeigen. In diesem Protokoll werden Leistungsinformationen pro Instanz erfasst, z.B. insgesamt bereitgestellte Anforderungen, Durchsatz in Byte, Anzahl von Anforderungen mit Fehlern, Anzahl von fehlerfreien und fehlerhaften Back-End-Instanzen. Das Leistungsprotokoll wird alle 60 Sekunden erstellt.
- **Firewallprotokolle:** Mithilfe dieses Protokolls können Sie die Anforderungen anzeigen, die entweder über den Erkennungs- oder über den Schutzmodus eines Anwendungsgateways protokolliert werden, das mit Web Application Firewall konfiguriert ist.

>[AZURE.WARNING] Protokolle sind nur für Ressourcen verfügbar, die im Ressourcen-Manager-Bereitstellungsmodell bereitgestellt werden. Sie können Protokolle nicht für Ressourcen im klassischen Bereitstellungsmodell verwenden. Für ein besseres Verständnis der beiden Modelle lesen Sie den Artikel [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../resource-manager-deployment-model.md).

## Aktivieren der Protokollierung mit PowerShell

Die Überwachungsprotokollierung ist automatisch für alle Ressourcen-Manager-Ressourcen aktiviert. Sie müssen die Zugriffs- und Leistungsprotokollierung aktivieren, um mit der Erfassung von Daten aus diesen Protokollen zu beginnen. Führen Sie die folgenden Schritte aus, um die Protokollierung zu aktivieren:

1. Notieren Sie sich die Ressourcen-ID Ihres Speicherkontos, unter dem die Protokolldaten gespeichert werden. Diese weist folgendes Format auf: /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppenname>/providers/Microsoft.Storage/storageAccounts/<Speicherkontoname>. Sie können jedes Speicherkonto Ihres Abonnements verwenden. Sie können das Vorschauportal verwenden, um nach diesen Informationen zu suchen.

	![Vorschauportal – Application Gateway-Diagnose](./media/application-gateway-diagnostics/diagnostics1.png)

2. Notieren Sie sich die Ressourcen-ID Ihres Anwendungsgateways, für das die Protokollierung aktiviert werden soll. Diese weist folgendes Format auf: /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppenname>/providers/Microsoft.Network/applicationGateways/<Anwendungsgatewayname>. Sie können das Vorschauportal verwenden, um nach diesen Informationen zu suchen.

	![Vorschauportal – Application Gateway-Diagnose](./media/application-gateway-diagnostics/diagnostics2.png)

3. Aktivieren Sie die Diagnoseprotokollierung mit dem folgenden PowerShell-Cmdlet:

		Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true 	

>[AZURE.INFORMATION] Für Überwachungsprotokolle ist kein separates Speicherkonto erforderlich. Für die Nutzung des Speichers für die Zugriffs- und Leistungsprotokollierung fallen Dienstgebühren an.

## Aktivieren der Protokollierung über das Azure-Portal

### Schritt 1

Navigieren Sie im Azure-Portal zu Ihrer Ressource. Klicken Sie auf **Diagnoseprotokolle**. Wenn Sie die Diagnose zum ersten Mal konfigurieren, sieht das Blatt wie in der folgenden Abbildung dargestellt aus:

Für Application Gateway sind 3 Protokolle verfügbar.

- Zugriffsprotokoll
- Leistungsprotokoll
- Firewallprotokoll

Klicken Sie auf **Diagnose aktivieren**, um die Erfassung von Daten zu starten.

![Blatt mit Diagnoseeinstellungen][1]

### Schritt 2

Auf dem Blatt **Diagnoseeinstellungen** werden die Einstellungen für die Diagnoseprotokolle festgelegt. In diesem Beispiel wird Log Analytics zum Speichern der Protokolle verwendet. Klicken Sie unter **Log Analytics** auf **Konfigurieren**, um den Arbeitsbereich zu konfigurieren. Event Hubs und ein Speicherkonto können ebenso verwendet werden, um die Diagnoseprotokolle zu speichern.

![Diagnoseblatt][2]

### Schritt 3

Wählen Sie einen vorhandenen OMS-Arbeitsbereich aus, oder erstellen Sie einen neuen. In diesem Beispiel wird ein vorhandener verwendet.

![OMS-Arbeitsbereiche][3]

### Schritt 4

Wenn Sie fertig sind, bestätigen Sie die Einstellungen, und klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

![Auswahl bestätigen][4]

## Überwachungsprotokoll

Dieses Protokoll (früher als "Betriebsprotokoll" bekannt) wird standardmäßig von Azure generiert. Die Protokolle werden 90 Tage lang im Azure-Ereignisprotokollspeicher aufbewahrt. Weitere Informationen zu diesen Protokollen finden Sie im Artikel [Anzeigen von Ereignis- und Überwachungsprotokollen](../azure-portal/insights-debugging-with-events.md).

## Zugriffsprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie in den vorherigen Schritten für die jeweiligen Application Gateways aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Jeder Application Gateway-Zugriff wird wie im folgenden Beispiel gezeigt im JSON-Format protokolliert:

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


## Firewallprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es, wie in den vorherigen Schritten ausgeführt, für die jeweiligen Anwendungsgateways aktiviert haben. Für dieses Protokoll muss zudem die Web Application Firewall auf einem Anwendungsgateway konfiguriert werden. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Die folgenden Daten werden protokolliert:

	{
		"resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
		"operationName": "ApplicationGatewayFirewall",
		"time": "2016-09-20T00:40:04.9138513Z",
		"category": "ApplicationGatewayFirewallLog",
		"properties":     {
			"instanceId":"ApplicationGatewayRole_IN_0",
			"clientIp":"108.41.16.164",
			"clientPort":1815,
			"requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
			"ruleId":"OWASP_973336",
			"message":"XSS Filter - Category 1: Script Tag Vector",
			"action":"Logged",
			"site":"Global",
			"message":"XSS Filter - Category 1: Script Tag Vector",
			"details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
	}

## Anzeigen und Analysieren des Überwachungsprotokolls

Mit einer der folgenden Methoden können Sie die Überwachungsprotokolldaten anzeigen und analysieren:

- **Azure-Tools:** Rufen Sie Informationen aus den Überwachungsprotokollen über Azure PowerShell, die Azure-Befehlszeilenschnittstelle, die Azure REST-API oder über das Azure-Vorschauportal ab. Schrittweise Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md).
- **Power BI:** Wenn Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Audit Logs Content Pack for Power BI](https://powerbi.microsoft.com/de-DE/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren, die Sie im Istzustand oder angepasst verwenden können.

## Anzeigen und Analysieren von Zugriffs-, Leistungs- und Firewallprotokollen

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) kann die Leistungsindikator- und Ereignisprotokolldateien aus Ihrem Blobspeicherkonto erfassen und enthält Visualisierungen und leistungsfähige Suchfunktionen, um Ihre Protokolle zu analysieren.

Sie können auch eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für Zugriffs- und Leistungsprotokolle abrufen. Sobald Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, PowerBI oder einem anderen Datenvisualisierungstool anzeigen.

>[AZURE.TIP] Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von Github verwenden

## Metriken

Metriken sind ein Feature für bestimmte Azure-Ressourcen, damit Sie die Leistungsindikatoren im Portal anzeigen können. Für Application Gateway steht zum Zeitpunkt der Abfassung dieses Artikels eine Metrik zur Verfügung. Diese Metrik misst den Durchsatz und kann im Portal angezeigt werden. Navigieren Sie zu einem Anwendungsgateway, und klicken Sie auf **Metriken**. Wählen Sie den Durchsatz im Abschnitt **Verfügbare Metriken** aus, um die Werte anzuzeigen. In der folgenden Abbildung sehen Sie ein Beispiel mit den Filtern, die zum Anzeigen der Daten in verschiedenen Zeiträumen verwendet werden können.

Eine Liste der aktuell unterstützten Metriken finden Sie unter [Unterstützte Metriken von Azure Monitor](../azure-portal/monitoring-supported-metrics.md)

![Metrikanzeige][5]

## Warnungsregeln

Warnungsregeln können anhand der Metriken für eine Ressource gestartet werden. Dies bedeutet für Application Gateway, dass eine Warnung einen Webhook aufrufen oder eine E-Mail an einen Administrator senden kann, wenn der Durchsatz des Anwendungsgateways für einen angegebenen Zeitraum oberhalb oder unterhalb eines Schwellenwerts bzw. genau auf einem Schwellenwert liegt.

Das folgende Beispiel führt Sie durch die Erstellung einer Warnungsregel, die eine E-Mail an einen Administrator sendet, nachdem ein Durchsatzschwellenwert verletzt wurde.

### Schritt 1

Klicken Sie zu Beginn auf **Metrikwarnung hinzufügen**. Dieses Blatt kann auch über das Blatt mit den Metriken aufgerufen werden.

![Blatt mit Warnungsregeln][6]

### Schritt 2

Füllen Sie auf dem Blatt **Regel hinzufügen** die Abschnitte für den Namen, die Bedingung und die Benachrichtigung aus, und klicken Sie zum Abschluss auf **OK**.

Die Auswahl unter **Bedingung** lässt 4 Werte zu, **Größer als**, **Größer oder gleich**, **Kleiner als** und **Kleiner oder gleich**.

Unter **Zeitraum** können Sie einen Zeitraum von fünf Minuten bis zu sechs Stunden auswählen.

Durch Auswahl von **E-Mail-Besitzer, Mitwirkende und Leser** kann die E-Mail-Adresse dynamisch basierend auf den Benutzern festgelegt werden, die auf diese Ressource zugreifen können. Andernfalls können Sie im Textfeld **Weitere Administrator-E-Mail(s)** eine durch Komma getrennte Liste von Benutzern angeben.

![Blatt zum Hinzufügen von Regeln][7]

Wenn der Schwellenwert verletzt wird, trifft eine E-Mail ein, die in etwa so aussieht wie in der folgenden Abbildung dargestellt:

![E-Mail zu Schwellenwertverletzung][8]

Eine Liste der Warnungen wird angezeigt, sobald eine Metrikwarnung erstellt wurde. Diese zeigt eine Übersicht über alle Warnungsregeln.

![Warnungsregelansicht][9]

Weitere Informationen zu Warnungsbenachrichtigungen finden Sie unter [Empfangen von Warnungsbenachrichtigungen](../azure-portal/insights-receive-alert-notifications.md).

Weitere Informationen zu Webhooks und deren Verwendung mit Warnungen finden Sie unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../azure-portal/insights-webhooks-alerts.md).

## Nächste Schritte

- Visualisieren von Leistungsindikator- und Ereignisprotokollen mit [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
- Blogbeitrag [Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx).
- Blogbeitrag [View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png

<!---HONumber=AcomDC_0928_2016-->