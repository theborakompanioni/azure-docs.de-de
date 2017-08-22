---
title: "Überwachen von Zugriffsprotokollen, Leistungsprotokollen, Back-End-Integrität und Metriken für Application Gateway | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Zugriffs- und Leistungsprotokolle für Application Gateway aktivieren und verwalten."
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
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 12c252340b82aba5ee69b12db83353750782e7c5
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway

Mit Azure Application Gateway können Sie Ressourcen auf die folgenden Arten überwachen:

* [Back-End-Integrität](#back-end-health): Application Gateway bietet über das Azure-Portal sowie mithilfe von PowerShell die Möglichkeit zur Überwachung der Integrität der Server in den Back-End-Pools. Sie können die Integrität der Back-End-Pools auch über die Leistungsdiagnoseprotokolle ermitteln.

* [Protokolle](#diagnostic-logs): Protokolle ermöglichen das Speichern und Nutzen von Leistungs-, Zugriffs- und anderen Daten einer Ressource zu Überwachungszwecken.

* [Metriken](#metrics): Application Gateway verfügt derzeit über eine Metrik. Mit dieser Metrik wird der Durchsatz des Anwendungsgateways in Bytes pro Sekunde gemessen.

## <a name="back-end-health"></a>Back-End-Integrität

Application Gateway bietet über das Portal sowie mithilfe von PowerShell oder die Befehlszeilenschnittstelle (CLI) die Möglichkeit zur Überwachung der Integrität einzelner Mitglieder der Back-End-Pools. Eine Zusammenfassung der Integrität von Back-End-Pools können Sie auch über die Leistungsdiagnoseprotokolle ermitteln. 

Der Back-End-Integritätsbericht spiegelt die Ausgabe des Application Gateway-Integritätstests an die Back-End-Instanzen wider. Wenn der Testvorgang erfolgreich ist und das Back-End Datenverkehr empfangen kann, wird es als fehlerfrei angesehen. Andernfalls wird es als nicht fehlerfrei eingestuft.

> [!IMPORTANT]
> Wenn sich eine Netzwerksicherheitsgruppe (NSG) in einem Application Gateway-Subnetz befindet, sollten Sie die Portbereiche 65503 - 65534 im Application Gateway-Subnetz für eingehenden Datenverkehr öffnen. Diese Ports sind erforderlich, damit die Back-End-Integritäts-API verwendet werden kann.


### <a name="view-back-end-health-through-the-portal"></a>Anzeigen der Back-End-Integrität über das Portal

Im Portal wird die Back-End-Integrität automatisch bereitgestellt. Wählen Sie in einem vorhandenen Anwendungsgateway die Option **Überwachung** > **Back-End-Integrität**. 

Jedes Mitglied im Back-End-Pool (ob NIC, IP-Adresse oder FQDN) ist auf dieser Seite aufgeführt. Der Name des Back-End-Pools, der Port, der Name der Back-End-HTTP-Einstellungen und der Integritätsstatus werden angezeigt. Gültige Werte für den Integritätsstatus sind **Fehlerfrei**, **Fehlerhaft** und **Unbekannt**.

> [!NOTE]
> Stellen Sie beim Back-End-Integritätsstatus **Unbekannt** sicher, dass der Zugriff auf das Back-End nicht durch eine NSG-Regel, eine benutzerdefinierte Route (UDR) oder ein benutzerdefiniertes DNS im virtuellen Netzwerk blockiert ist.

![Back-End-Integrität][10]

### <a name="view-back-end-health-through-powershell"></a>Anzeigen der Back-End-Integrität mit PowerShell

Der folgende PowerShell-Code veranschaulicht, wie Sie die Back-End-Integrität mit dem `Get-AzureRmApplicationGatewayBackendHealth`-Cmdlet anzeigen:

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a>Anzeigen der Back-End-Integrität mit Azure CLI 2.0

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Ergebnisse

Der folgende Codeausschnitt enthält ein Beispiel für die Antwort:

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

## <a name="diagnostic-logging"></a>Diagnoseprotokolle

Sie können in Azure verschiedene Protokolltypen verwenden, um Anwendungsgateways zu verwalten und eventuelle Fehler zu beheben. Sie können auf einige dieser Protokolle über das Portal zugreifen. Alle Protokolle können aus Azure Blob Storage extrahiert und in anderen Tools wie [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel und Power BI angezeigt werden. In der folgenden Liste finden Sie weitere Informationen über die verschiedenen Typen von Protokollen:

* **Aktivitätsprotokoll:** Sie können das Feature [Azure-Aktivitätsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) (ehemals Betriebs- und Überwachungsprotokolle) verwenden, um alle an Ihr Azure-Abonnement übermittelten Vorgänge und deren Status anzuzeigen. Aktivitätsprotokolleinträge werden standardmäßig gesammelt und können im Azure-Portal angezeigt werden.
* **Zugriffsprotokoll:** Sie können diese Art von Protokoll verwenden, um Application Gateway-Zugriffsmuster anzuzeigen und wichtige Informationen zu analysieren, z.B. die IP des Aufrufers, die angeforderte URL, die Antwortlatenz, den Rückgabecode sowie eingehende und ausgehende Bytes. Ein Zugriffsprotokoll wird alle 300 Sekunden erstellt. Dieses Protokoll enthält einen Datensatz pro Instanz von Application Gateway. Die Application Gateway-Instanz kann anhand der instanceId-Eigenschaft identifiziert werden.
* **Leistungsprotokoll:** Mithilfe dieses Protokolls können Sie die Leistung von Application Gateway-Instanzen anzeigen. In diesem Protokoll werden Leistungsinformationen für jede Instanz erfasst, z.B. insgesamt bereitgestellte Anforderungen, Durchsatz in Byte, Anzahl von Anforderungen mit Fehlern und die Anzahl von fehlerfreien und fehlerhaften Back-End-Instanzen. Ein Leistungsprotokoll wird alle 60 Sekunden erstellt.
* **Firewallprotokoll:** Mithilfe dieses Protokolls können Sie die Anforderungen anzeigen, die entweder über den Erkennungs- oder über den Schutzmodus eines Anwendungsgateways protokolliert werden, das mit der Web Application Firewall konfiguriert wurde.

> [!NOTE]
> Protokolle sind nur für Ressourcen verfügbar, die über das Azure Resource Manager-Bereitstellungsmodell bereitgestellt werden. Sie können Protokolle nicht für Ressourcen im klassischen Bereitstellungsmodell verwenden. Ein besseres Verständnis der beiden Modelle können Sie entwickeln, indem Sie den Artikel [Grundlegendes zur Bereitstellung über den Resource Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md) lesen.

Sie haben drei Möglichkeiten, um Ihre Protokolle zu speichern:

* **Speicherkonto:** Speicherkonten eignen sich am besten für Protokolle, die eine längere Zeit gespeichert und bei Bedarf überprüft werden.
* **Event Hubs:** Event Hubs sind eine hervorragende Möglichkeit für die Integration in andere SIEM-Tools (Security Information and Event Management), um Warnungen für Ihre Ressourcen zu erhalten.
* **Log Analytics:** Log Analytics ist am besten für eine allgemeine Echtzeitüberwachung Ihrer Anwendung oder zum Beobachten von Trends geeignet.

### <a name="enable-logging-through-powershell"></a>Ermöglichen der Protokollierung mit PowerShell

Die Aktivitätsprotokollierung ist automatisch für alle Resource Manager-Ressourcen aktiviert. Sie müssen die Zugriffs- und Leistungsprotokollierung aktivieren, um mit der Erfassung von Daten aus diesen Protokollen zu beginnen. Führen Sie zum Aktivieren der Protokollierung die folgenden Schritte aus:

1. Notieren Sie sich die Ressourcen-ID Ihres Speicherkontos, unter dem die Protokolldaten gespeichert werden. Dieser Wert weist folgendes Format auf: /subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Storage/storageAccounts/\<Speicherkontoname\>. Sie können jedes Speicherkonto Ihres Abonnements verwenden. Sie können das Azure-Portal verwenden, um nach diesen Informationen zu suchen.

    ![Portal: Ressourcen-ID für Speicherkonto](./media/application-gateway-diagnostics/diagnostics1.png)

2. Notieren Sie sich die Ressourcen-ID Ihres Anwendungsgateways, für das die Protokollierung aktiviert ist. Dieser Wert weist folgendes Format auf: /subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Network/applicationGateways/\<Anwendungsgatewayname\>. Sie können das Portal verwenden, um nach diesen Informationen zu suchen.

    ![Portal: Ressourcen-ID für das Anwendungsgateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. Aktivieren Sie die Diagnoseprotokollierung mit dem folgenden PowerShell-Cmdlet:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Für Aktivitätsprotokolle ist kein separates Speicherkonto erforderlich. Für die Nutzung des Speichers für die Zugriffs- und Leistungsprotokollierung fallen Dienstgebühren an.

### <a name="enable-logging-through-the-azure-portal"></a>Ermöglichen der Protokollierung über das Azure-Portal

1. Suchen Sie im Azure-Portal nach Ihrer Ressource, und klicken Sie auf **Diagnoseprotokolle**.

   Für Application Gateway sind drei Protokolle verfügbar:

   * Zugriffsprotokoll
   * Leistungsprotokoll
   * Firewallprotokoll

2. Klicken Sie auf **Diagnose aktivieren** , um die Erfassung von Daten zu starten.

   ![Aktivieren der Diagnose][1]

3. Auf dem Blatt **Diagnoseeinstellungen** werden die Einstellungen für die Diagnoseprotokolle angegeben. In diesem Beispiel werden die Protokolle in Log Analytics gespeichert. Klicken Sie unter **Log Analytics** auf **Konfigurieren**, um den Arbeitsbereich zu konfigurieren. Sie können auch Event Hubs und ein Speicherkonto verwenden, um die Diagnoseprotokolle zu speichern.

   ![Starten des Konfigurationsprozesses][2]

4. Wählen Sie einen vorhandenen OMS-Arbeitsbereich (Operations Management Suite) aus, oder erstellen Sie einen neuen. In diesem Beispiel wird ein vorhandener Arbeitsbereich verwendet.

   ![Optionen für OMS-Arbeitsbereiche][3]

5. Überprüfen Sie die Einstellungen, und klicken Sie auf **Speichern**.

   ![Blatt „Diagnoseeinstellungen“ mit Auswahl][4]

### <a name="activity-log"></a>Aktivitätsprotokoll

Das Aktivitätsprotokoll wird von Azure standardmäßig generiert. Die Protokolle werden 90 Tage lang im Azure-Ereignisprotokollspeicher aufbewahrt. Weitere Informationen zu diesen Protokollen finden Sie im Artikel [Anzeigen von Ereignis- und Aktivitätsprotokollen](../monitoring-and-diagnostics/insights-debugging-with-events.md).

### <a name="access-log"></a>Zugriffsprotokoll

Das Zugriffsprotokoll wird nur generiert, wenn Sie es auf jeder Application Gateway-Instanz gemäß den obigen Schritten aktiviert haben. Die Daten werden in dem Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Jeder Application Gateway-Zugriff wird wie im folgenden Beispiel im JSON-Format protokolliert:


|Wert  |Beschreibung  |
|---------|---------|
|instanceId     | Application Gateway-Instanz, von der die Anforderung bereitgestellt wurde        |
|clientIP     | Ursprungs-IP für die Anforderung        |
|clientPort     | Ursprungsport für die Anforderung       |
|httpMethod     | Von der Anforderung verwendete HTTP-Methode       |
|requestUri     | URI der empfangenen Anforderung        |
|RequestQuery     | **Server-Routed**: Back-End-Poolinstanz, an die die Anforderung gesendet wurde. </br> **X-AzureApplicationGateway-LOG-ID**: Korrelations-ID, die für die Anforderung verwendet wurde. Kann für die Behandlung von Datenverkehrsproblemen auf den Back-End-Servern verwendet werden. </br>**SERVER-STATUS**: HTTP-Antwortcode, den Application Gateway vom Back-End empfangen hat.       |
|UserAgent     | Benutzer-Agent aus dem HTTP-Anforderungsheader        |
|httpStatus     | HTTP-Statuscode, der vom Application Gateway an den Client zurückgegeben wurde       |
|httpVersion     | HTTP-Version der Anforderung        |
|receivedBytes     | Größe des empfangenen Pakets in Byte        |
|sentBytes| Größe des gesendeten Pakets in Byte|
|timeTaken| Dauer (in Millisekunden), bis eine Anforderung verarbeitet und die dazugehörige Antwort gesendet wurde. Dies wird als Intervall zwischen dem Zeitpunkt, zu dem Application Gateway das erste Byte einer HTTP-Anforderung empfängt, bis zu dem Zeitpunkt berechnet, zu dem der Vorgang zum Senden der Antwort abgeschlossen ist. Hierbei ist der Hinweis wichtig, dass das Feld „Time-Taken“ normalerweise die Zeitdauer enthält, die von den Anforderungs- und Antwortpaketen für die Übermittlung über das Netzwerk benötigt wird. |
|sslEnabled| Gibt an, ob für die Kommunikation an die Back-End-Pools SSL verwendet wurde. Gültige Werte sind „on“ und „off“.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a>Leistungsprotokoll

Das Leistungsprotokoll wird nur generiert, wenn Sie es auf jeder Application Gateway-Instanz gemäß den obigen Schritten aktiviert haben. Die Daten werden in dem Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Die Daten für das Leistungsprotokoll werden in Intervallen von einer Minute generiert. Die folgenden Daten werden protokolliert:


|Wert  |Beschreibung  |
|---------|---------|
|instanceId     |  Application Gateway-Instanz, für die Leistungsdaten generiert werden. Für ein Anwendungsgateway mit mehreren Instanzen ist eine Zeile pro Instanz vorhanden.        |
|healthyHostCount     | Anzahl von fehlerfreien Hosts im Back-End-Pool        |
|unHealthyHostCount     | Anzahl von fehlerhaften Hosts im Back-End-Pool        |
|requestCount     | Anzahl von bereitgestellten Anforderungen        |
|latency | Wartezeit (in Millisekunden) für Anforderungen von der Instanz an das Back-End, über das die Anforderungen bereitgestellt werden |
|failedRequestCount| Anzahl von fehlerhaften Anforderungen|
|throughput| Durchschnittlicher Durchsatz seit dem letzten Protokoll, gemessen in Bytes pro Sekunde.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
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

> [!NOTE]
> Latenz wird von dem Zeitpunkt, zu dem das erste Byte der HTTP-Anforderung empfangen wird, bis zu dem Zeitpunkt berechnet, zu dem das letzte Byte der HTTP-Antwort gesendet wird. Dies ist die Summe der Application Gateway-Verarbeitungszeit plus Netzwerkkosten zum Back-End plus die Zeit, die das Back-End zum Verarbeiten der Anforderung benötigt.

### <a name="firewall-log"></a>Firewallprotokoll

Das Firewallprotokoll wird nur generiert, wenn Sie es für jedes Anwendungsgateway gemäß den obigen Schritten aktiviert haben. Für dieses Protokoll muss zudem die Web Application Firewall auf einem Anwendungsgateway konfiguriert sein. Die Daten werden in dem Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Die folgenden Daten werden protokolliert:


|Wert  |Beschreibung  |
|---------|---------|
|instanceId     | Application Gateway-Instanz, für die Firewalldaten generiert werden. Für ein Anwendungsgateway mit mehreren Instanzen ist eine Zeile pro Instanz vorhanden.         |
|clientIp     |   Ursprungs-IP für die Anforderung      |
|clientPort     |  Ursprungsport für die Anforderung       |
|requestUri     | URI der empfangenen Anforderung       |
|ruleSetType     | Regelsatztyp: Der verfügbare Wert ist OWASP.        |
|ruleSetVersion     | Verwendete Regelsatzversion. Verfügbare Werte sind 2.2.9 und 3.0.     |
|ruleId     | Regel-ID des auslösenden Ereignisses        |
|message     | Benutzerfreundliche Meldung für das auslösende Ereignis. Weitere Details im Abschnitt „Details“.        |
|action     |  Aktion, die für die Anforderung durchgeführt wird. Verfügbare Werte sind „Blocked“ und „Allowed“.      |
|site     | Standort, für den das Protokoll generiert wurde. Derzeit ist nur „Global“ aufgeführt, da Regeln global sind.|
|details     | Details zum auslösenden Ereignis        |
|details.message     | Beschreibung der Regel        |
|details.data     | Spezifische Daten in der Anforderung, die eine Übereinstimmung mit der Regel ergeben         |
|details.file     | Konfigurationsdatei, die die Regel enthalten hat        |
|details.line     | Nummer der Zeile in der Konfigurationsdatei, die das Ereignis ausgelöst hat       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-the-activity-log"></a>Anzeigen und Analysieren des Aktivitätsprotokolls

Mit einer der folgenden Methoden können Sie die Aktivitätsprotokolldaten anzeigen und analysieren:

* **Azure-Tools:** Rufen Sie Informationen aus dem Aktivitätsprotokoll mit Azure PowerShell, der Azure CLI, der Azure-REST-API oder dem Azure-Portal ab. Detaillierte Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Wenn Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Activity Logs Content Pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren, die Sie im Istzustand oder angepasst verwenden können.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Anzeigen und Analysieren der Zugriffs-, Leistungs- und Firewallprotokolle

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) kann die Indikator- und Ereignisprotokolldateien aus Ihrem Blob-Speicherkonto erfassen. Die Anwendung umfasst Visualisierungen und leistungsfähige Suchfunktionen zum Analysieren Ihrer Protokolle.

Sie können auch eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für Zugriffs- und Leistungsprotokolle abrufen. Nachdem Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, Power BI oder einem anderen Datenvisualisierungstool anzeigen.

> [!TIP]
> Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von GitHub verwenden.
> 
> 

## <a name="metrics"></a>Metriken

Metriken sind ein Feature für bestimmte Azure-Ressourcen, damit Sie die Leistungsindikatoren im Portal anzeigen können. Für Application Gateway ist derzeit eine Metrik verfügbar. Dies ist die Metrik für den Durchsatz, die Sie im Portal anzeigen können. Navigieren Sie zu einem Anwendungsgateway, und klicken Sie auf **Metriken**. Wählen Sie im Abschnitt **Verfügbare Metriken** den Durchsatz aus, um die Werte anzuzeigen. Die folgende Abbildung enthält ein Beispiel mit den Filtern, die Sie zum Anzeigen der Daten in verschiedenen Zeiträumen verwenden können.

![Ansicht „Metrik“ mit Filtern][5]

Eine aktuelle Liste mit Metriken finden Sie unter [Unterstützte Metriken von Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

### <a name="alert-rules"></a>Warnungsregeln

Sie können Warnungsregeln basierend auf Metriken für eine Ressource starten. Eine Warnung kann beispielsweise einen Webhook aufrufen oder eine E-Mail an einen Administrator senden, wenn der Durchsatz des Anwendungsgateways für einen angegebenen Zeitraum oberhalb oder unterhalb eines Schwellenwerts bzw. genau auf einem Schwellenwert liegt.

Im folgenden Beispiel wird schrittweise die Erstellung einer Warnungsregel beschrieben, die eine E-Mail an einen Administrator sendet, nachdem ein Durchsatzschwellenwert verletzt wurde:

1. Klicken Sie auf **Metrikwarnung hinzufügen**, um das Blatt **Regel hinzufügen** zu öffnen. Sie können dieses Blatt auch über das Blatt mit den Metriken erreichen.

   ![Schaltfläche „Metrikwarnung hinzufügen“][6]

2. Füllen Sie auf dem Blatt **Regel hinzufügen** die Abschnitte für den Namen, die Bedingung und die Benachrichtigung aus, und klicken Sie auf **OK**.

   * Wählen Sie in der Auswahl unter **Bedingung** einen von vier Werten aus: **Größer als**, **Größer oder gleich**, **Kleiner als** oder **Kleiner oder gleich**.

   * Wählen Sie unter **Period** (Zeitraum) einen Zeitraum zwischen fünf Minuten und sechs Stunden aus.

   * Wenn Sie **E-Mail-Besitzer, Mitwirkende und Leser** wählen, kann die E-Mail-Adresse dynamisch basierend auf den Benutzern festgelegt werden, die auf diese Ressource zugreifen können. Andernfalls können Sie im Feld **Weitere Administrator-E-Mail(s)** eine durch Kommas getrennte Liste mit Benutzern angeben.

   ![Blatt „Regel hinzufügen“][7]

Wenn der Schwellenwert überschritten wird, trifft eine E-Mail ein, die in etwa wie in der folgenden Abbildung aussieht:

![E-Mail zu überschrittenem Schwellenwert][8]

Nach dem Erstellen einer Metrikwarnung wird eine Liste mit Warnungen angezeigt. Dies ist eine Übersicht über alle Warnungsregeln.

![Liste mit Warnungen und Regeln][9]

Weitere Informationen zu Warnungsbenachrichtigungen finden Sie unter [Empfangen von Warnungsbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Weitere Informationen zu Webhooks und deren Verwendung mit Warnungen finden Sie unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="next-steps"></a>Nächste Schritte

* Visualisieren von Leistungsindikator- und Ereignisprotokollen mit [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
* Blogbeitrag [Visualize your Azure activity log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) (Visualisieren von Azure-Aktivitätsprotokollen mit Power BI)
* Blogbeitrag [View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Anzeigen und Analysieren von Azure-Aktivitätsprotokollen in Power BI und mehr)

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

