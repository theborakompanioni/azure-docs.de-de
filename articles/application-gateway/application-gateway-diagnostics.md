---
title: "Überwachen von Zugriff, Leistungsprotokollen, Back-End-Integrität und Metriken für Application Gateway | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Zugriffs- und Leistungsprotokolle für das Application Gateway aktivieren und verwalten."
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
translationtype: Human Translation
ms.sourcegitcommit: ca5291e00fcf4fbd9927fe3cadad01f62b235d10
ms.openlocfilehash: c6829c94bb8e5de3bec155bf326ac61c300477cb


---
# <a name="backend-health-diagnostics-logging-and-metrics-for-application-gateway"></a>Back-End-Integrität, Diagnoseprotokollierung und Metriken für Application Gateway

Azure bietet die Möglichkeit, Ressourcen anhand von Protokollierung und Metriken zu überwachen. Application Gateway stellt diese Funktionen mit Back-End-Integrität, Protokollierung und Metriken bereit.

[**Back-End-Integrität**](#backend-health): Application Gateway bietet über das Portal sowie mithilfe von PowerShell die Möglichkeit zur Überwachung der Integrität der Server in den Back-End-Pools. Informationen zur Integrität der Back-End-Pools sind auch über die Leistungsdiagnoseprotokolle zu finden.

[**Protokollierung**](#enable-logging-with-powershell): Die Protokollierung ermöglicht das Speichern und Nutzen von Leistungs-, Zugriffs- und anderen Protokollen von einer Ressource zu Überwachungszwecken.

[**Metriken**](#metrics) : Application Gateway verfügt derzeit über eine Metrik. Mit dieser Metrik wird der Durchsatz des Anwendungsgateways in Bytes pro Sekunde gemessen.

## <a name="backend-health"></a>Back-End-Integrität

Application Gateway bietet über das Portal sowie mithilfe von PowerShell oder CLI die Möglichkeit zur Überwachung der Integrität einzelner Mitglieder der Back-End-Pools. Eine Zusammenfassung der Integrität von Back-End-Pools sind auch über die Leistungsdiagnoseprotokolle zu finden. Der Back-End-Integritätsbericht gibt die Ausgabe des Application Gateway-Integritätstests an die Back-End-Instanzen wieder. Wenn der Test bestanden wurde und das Back-End Datenverkehr verarbeiten kann, gilt es als fehlerfrei; andernfalls gilt es als fehlerhaft.

> [!important]
> Wenn eine NSG in einem Application Gateway-Subnetz vorhanden ist, müssen die Portbereiche 65503 bis 65534 für die Application Gateway-Instanzen geöffnet werden.

### <a name="view-backend-health-through-the-portal"></a>Anzeigen der Back-End-Integrität über das Portal

Zum Anzeigen der Back-End-Integrität sind keine Maßnahmen erforderlich. Navigieren Sie in einem vorhandenen Anwendungsgateway zu **Überwachung** > **Back-End-Integrität**. Jedes Mitglied im Back-End-Pool (ob NIC, IP-Adresse oder FQDN) ist auf dieser Seite aufgeführt. Der Name des Back-End-Pools, der Port, der Name der Back-End-HTTP-Einstellungen und der Integritätsstatus werden angezeigt. Gültige Werte für den Integritätsstatus sind „Fehlerfrei“, „Fehlerhaft“ und „Unbekannt“.

> [!WARNING]
> Stellen Sie bei einem Back-End-Integritätsstatus von **Unbekannt** sicher, dass der Zugriff auf das Back-End nicht durch eine Regel der Netzwerksicherheitsgruppe (NSG) oder benutzerdefiniertes DNS im VNet blockiert wird.

![Back-End-Integrität][10]

### <a name="view-backend-health-with-powershell"></a>Anzeigen der Back-End-Integrität mit PowerShell

Informationen zur Back-End-Integrität können auch über PowerShell abgerufen werden. Der folgende PowerShell-Code zeigt, wie mit dem Cmdlet `Get-AzureRmApplicationGatewayBackendHealth` die Back-End-Integrität abgerufen wird.

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

Die Ergebnisse werden zurückgegeben. Ein Beispiel für die Antwort im folgenden Codeausschnitt dargestellt.

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Diagnoseprotokollierung

Sie können in Azure verschiedene Protokolltypen verwenden, um Anwendungsgateways zu verwalten und eventuelle Fehler zu beheben. Auf einige dieser Protokolle können Sie über das Portal zugreifen, und alle Protokolle können aus einem Azure-Blob Storage extrahiert und in anderen Tools wie [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md),Excel und PowerBI angezeigt werden. In der folgenden Liste finden Sie weitere Informationen über die verschiedenen Typen von Protokollen:

* **Aktivitätsprotokoll:** Sie können das Feature [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/insights-debugging-with-events.md) (ehemals Betriebs- und Überwachungsprotokolle) verwenden, um alle an Ihr Azure-Abonnement übermittelten Vorgänge und deren Status anzuzeigen. Aktivitätsprotokolleinträge werden standardmäßig gesammelt und können im Azure-Portal angezeigt werden.
* **Zugriffsprotokolle:** Sie können diese Art von Protokoll verwenden, um Anwendungsgateway-Zugriffsmuster anzuzeigen und wichtige Informationen zu analysieren, z.B. die IP des Aufrufers, die angeforderte URL, die Antwortlatenz, den Rückgabecode sowie eingehende und ausgehende Bytes. Das Zugriffsprotokoll wird alle 300 Sekunden erstellt. Dieses Protokoll enthält einen Datensatz pro Instanz von Application Gateway. Die Application Gateway-Instanz kann anhand der instanceId-Eigenschaft identifiziert werden.
* **Leistungsprotokolle:** Mithilfe dieses Protokolls können Sie die Leistung von Application Gateway-Instanzen anzeigen. In diesem Protokoll werden Leistungsinformationen pro Instanz erfasst, z.B. insgesamt bereitgestellte Anforderungen, Durchsatz in Byte, Anzahl von Anforderungen mit Fehlern, Anzahl von fehlerfreien und fehlerhaften Back-End-Instanzen. Das Leistungsprotokoll wird alle 60 Sekunden erstellt.
* **Firewallprotokolle:** Mithilfe dieses Protokolls können Sie die Anforderungen anzeigen, die entweder über den Erkennungs- oder über den Schutzmodus eines Anwendungsgateways protokolliert werden, das mit Web Application Firewall konfiguriert ist.

> [!WARNING]
> Protokolle sind nur für Ressourcen verfügbar, die im Ressourcen-Manager-Bereitstellungsmodell bereitgestellt werden. Sie können Protokolle nicht für Ressourcen im klassischen Bereitstellungsmodell verwenden. Für ein besseres Verständnis der beiden Modelle lesen Sie den Artikel [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md) .

Für das Speichern Ihrer Protokolle stehen drei verschiedene Optionen zur Auswahl.

* Speicherkonto: Speicherkonten eignen sich am besten für Protokolle, die eine längere Zeit gespeichert und bei Bedarf überprüft werden.
* Event Hubs: Event Hubs sind eine hervorragende Möglichkeit für die Integration in andere SEIM-Tools, um Warnungen für Ihre Ressourcen zu erhalten.
* Log Analytics: Log Analytics wird am besten für eine allgemeine Echtzeitüberwachung Ihrer Anwendung oder zum Beobachten von Trends verwendet.

### <a name="enable-logging-with-powershell"></a>Aktivieren der Protokollierung mit PowerShell

Die Aktivitätsprotokollierung ist automatisch für alle Resource Manager-Ressourcen aktiviert. Sie müssen die Zugriffs- und Leistungsprotokollierung aktivieren, um mit der Erfassung von Daten aus diesen Protokollen zu beginnen. Führen Sie die folgenden Schritte aus, um die Protokollierung zu aktivieren:

1. Notieren Sie sich die Ressourcen-ID Ihres Speicherkontos, unter dem die Protokolldaten gespeichert werden. Dieser Wert weist folgendes Format auf: /subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Storage/storageAccounts/\<Speicherkontoname\>. Sie können jedes Speicherkonto Ihres Abonnements verwenden. Sie können das Vorschauportal verwenden, um nach diesen Informationen zu suchen.

    ![Vorschauportal – Application Gateway-Diagnose](./media/application-gateway-diagnostics/diagnostics1.png)

2. Notieren Sie sich die Ressourcen-ID Ihres Anwendungsgateways, für das die Protokollierung aktiviert werden soll. Dieser Wert weist folgendes Format auf: /subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Network/applicationGateways/\<Anwendungsgatewayname\>. Sie können das Vorschauportal verwenden, um nach diesen Informationen zu suchen.

    ![Vorschauportal – Application Gateway-Diagnose](./media/application-gateway-diagnostics/diagnostics2.png)

3. Aktivieren Sie die Diagnoseprotokollierung mit dem folgenden PowerShell-Cmdlet:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Für Aktivitätsprotokolle ist kein separates Speicherkonto erforderlich. Für die Nutzung des Speichers für die Zugriffs- und Leistungsprotokollierung fallen Dienstgebühren an.

### <a name="enable-logging-with-azure-portal"></a>Aktivieren der Protokollierung über das Azure-Portal

#### <a name="step-1"></a>Schritt 1

Navigieren Sie im Azure-Portal zu Ihrer Ressource. Klicken Sie auf **Diagnoseprotokolle**. Wenn Sie die Diagnose zum ersten Mal konfigurieren, sieht das Blatt wie in der folgenden Abbildung dargestellt aus:

Für Application Gateway sind 3 Protokolle verfügbar.

* Zugriffsprotokoll
* Leistungsprotokoll
* Firewallprotokoll

Klicken Sie auf **Diagnose aktivieren** , um die Erfassung von Daten zu starten.

![Blatt mit Diagnoseeinstellungen][1]

#### <a name="step-2"></a>Schritt 2

Auf dem Blatt **Diagnoseeinstellungen** werden die Einstellungen für die Diagnoseprotokolle festgelegt. In diesem Beispiel wird Log Analytics zum Speichern der Protokolle verwendet. Klicken Sie unter **Log Analytics** auf **Konfigurieren**, um den Arbeitsbereich zu konfigurieren. Event Hubs und ein Speicherkonto können ebenso verwendet werden, um die Diagnoseprotokolle zu speichern.

![Diagnoseblatt][2]

#### <a name="step-3"></a>Schritt 3

Wählen Sie einen vorhandenen OMS-Arbeitsbereich aus, oder erstellen Sie einen neuen. In diesem Beispiel wird ein vorhandener verwendet.

![OMS-Arbeitsbereiche][3]

#### <a name="step-4"></a>Schritt 4

Wenn Sie fertig sind, bestätigen Sie die Einstellungen, und klicken Sie auf **Speichern** , um die Einstellungen zu speichern.

![Auswahl bestätigen][4]

### <a name="activity-log"></a>Aktivitätsprotokoll

Dieses Protokoll (früher als "Betriebsprotokoll" bekannt) wird standardmäßig von Azure generiert.  Die Protokolle werden 90 Tage lang im Azure-Ereignisprotokollspeicher aufbewahrt. Weitere Informationen zu diesen Protokollen finden Sie im Artikel [Anzeigen von Ereignis- und Aktivitätsprotokollen](../monitoring-and-diagnostics/insights-debugging-with-events.md).

### <a name="access-log"></a>Zugriffsprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie in den vorherigen Schritten für die jeweiligen Application Gateways aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Jeder Application Gateway-Zugriff wird wie im folgenden Beispiel gezeigt im JSON-Format protokolliert:

```json
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
```

### <a name="performance-log"></a>Leistungsprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie in den vorherigen Schritten für die jeweiligen Application Gateways aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Die folgenden Daten werden protokolliert:

```json
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
```

### <a name="firewall-log"></a>Firewallprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie in den vorherigen Schritten für die jeweiligen Application Gateways aktiviert haben. Für dieses Protokoll muss zudem die Web Application Firewall auf einem Anwendungsgateway konfiguriert sein. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Die folgenden Daten werden protokolliert:

```json
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
```

### <a name="view-and-analyze-the-activity-log"></a>Anzeigen und Analysieren des Aktivitätsprotokolls

Mit einer der folgenden Methoden können Sie die Aktivitätsprotokolldaten anzeigen und analysieren:

* **Azure-Tools:** Rufen Sie Informationen aus den Aktivitätsprotokollen über Azure PowerShell, die Azure-Befehlszeilenschnittstelle, die Azure REST-API oder über das Azure-Vorschauportal ab.  Detaillierte Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Wenn Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing) -Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Activity Logs Content Pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren, die Sie im Istzustand oder angepasst verwenden können.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Anzeigen und Analysieren der Zugriffs-, Leistungs- und Firewallprotokolle

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) kann die Leistungsindikator- und Ereignisprotokolldateien aus Ihrem Blobspeicherkonto erfassen und enthält Visualisierungen und leistungsfähige Suchfunktionen, um Ihre Protokolle zu analysieren.

Sie können auch eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für Zugriffs- und Leistungsprotokolle abrufen. Sobald Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, PowerBI oder einem anderen Datenvisualisierungstool anzeigen.

> [!TIP]
> Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von Github verwenden
> 
> 

## <a name="metrics"></a>Metriken

Metriken sind ein Feature für bestimmte Azure-Ressourcen, damit Sie die Leistungsindikatoren im Portal anzeigen können. Für Application Gateway steht zum Zeitpunkt der Abfassung dieses Artikels eine Metrik zur Verfügung. Diese Metrik misst den Durchsatz und kann im Portal angezeigt werden. Navigieren Sie zu einem Anwendungsgateway, und klicken Sie auf **Metriken**. Wählen Sie im Abschnitt **Verfügbare Metriken** den Durchsatz aus, um die Werte anzuzeigen. In der folgenden Abbildung sehen Sie ein Beispiel mit den Filtern, die zum Anzeigen der Daten in verschiedenen Zeiträumen verwendet werden können.

Eine Liste der aktuell unterstützten Metriken finden Sie unter [Unterstützte Metriken von Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![Metrikanzeige][5]

### <a name="alert-rules"></a>Warnregeln

Warnungsregeln können anhand der Metriken für eine Ressource gestartet werden. Dies bedeutet für Application Gateway, dass eine Warnung einen Webhook aufrufen oder eine E-Mail an einen Administrator senden kann, wenn der Durchsatz des Anwendungsgateways für einen angegebenen Zeitraum oberhalb oder unterhalb eines Schwellenwerts bzw. genau auf einem Schwellenwert liegt.

Das folgende Beispiel führt Sie durch die Erstellung einer Warnungsregel, die eine E-Mail an einen Administrator sendet, nachdem ein Durchsatzschwellenwert verletzt wurde.

#### <a name="step-1"></a>Schritt 1

Klicken Sie zu Beginn auf **Metrikwarnung hinzufügen** . Dieses Blatt kann auch über das Blatt mit den Metriken aufgerufen werden.

![Blatt mit Warnungsregeln][6]

#### <a name="step-2"></a>Schritt 2

Füllen Sie auf dem Blatt **Regel hinzufügen** die Abschnitte für den Namen, die Bedingung und die Benachrichtigung aus, und klicken Sie zum Abschluss auf **OK**.

Die Auswahl unter **Bedingung** lässt vier Werte zu: **Größer als**, **Größer oder gleich**, **Kleiner als** und **Kleiner oder gleich**.

Unter **Zeitraum** können Sie einen Zeitraum von fünf Minuten bis zu sechs Stunden auswählen.

Durch Auswahl von **E-Mail-Besitzer, Mitwirkende und Leser** kann die E-Mail-Adresse dynamisch basierend auf den Benutzern festgelegt werden, die auf diese Ressource zugreifen können. Andernfalls können Sie im Textfeld **Weitere Administrator-E-Mail(s)** eine durch Komma getrennte Liste von Benutzern angeben.

![Blatt zum Hinzufügen von Regeln][7]

Wenn der Schwellenwert verletzt wird, trifft eine E-Mail ein, die in etwa so aussieht wie in der folgenden Abbildung dargestellt:

![E-Mail zu Schwellenwertverletzung][8]

Eine Liste der Warnungen wird angezeigt, sobald eine Metrikwarnung erstellt wurde. Diese zeigt eine Übersicht über alle Warnungsregeln.

![Warnungsregelansicht][9]

Weitere Informationen zu Warnungsbenachrichtigungen finden Sie unter [Empfangen von Warnungsbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Weitere Informationen zu Webhooks und deren Verwendung mit Warnungen finden Sie unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Nächste Schritte

* Visualisieren von Leistungsindikator- und Ereignisprotokollen mit [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
* Blogbeitrag [Visualize your Azure Activity Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) (Visualisieren von Azure-Aktivitätsprotokollen mit Power BI).
* Informationen hierzu finden Sie im Blogbeitrag [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Anzeigen und Analysieren von Azure-Aktivitätsprotokollen in Power BI und mehr).

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png


<!--HONumber=Jan17_HO4-->


