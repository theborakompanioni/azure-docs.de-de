---
title: "Azure Networking Analytics-Lösung in Log Analytics | Microsoft Docs"
description: "Mit der Azure Networking Analytics-Lösung in Log Analytics können Sie Protokolle für Azure-Netzwerksicherheitsgruppen und Azure Application Gateway-Protokolle überprüfen."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.translationtype: HT
ms.sourcegitcommit: 80fd9ee9b9de5c7547b9f840ac78a60d52153a5a
ms.openlocfilehash: 06b67322b3812a668a515ecc357171ede1d85441
ms.contentlocale: de-de
ms.lasthandoff: 08/14/2017

---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Azure-Netzwerküberwachungslösungen in Log Analytics

Log Analytics bietet die folgenden Lösungen zur Überwachung Ihrer Netzwerke:
* Netzwerkleistungsmonitor (NPM) zum
 * Überwachen der Integrität Ihres Netzwerks
* Azure Application Gateway-Analyse zur Überprüfung
 * Azure Application Gateway-Protokolle
 * Azure Application Gateway-Metriken
* Azure-Netzwerksicherheitsgruppen-Analyse zur Überprüfung
 * Protokolle für Azure-Netzwerksicherheitsgruppen

## <a name="network-performance-monitor-npm"></a>Netzwerkleistungsmonitor (NPM)

Die [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md)-Verwaltungslösung ist eine Lösung zur Netzwerküberwachung, die die Integrität, Verfügbarkeit und Erreichbarkeit von Netzwerken überwacht.  Sie dient zur Überwachung der Konnektivität zwischen:

* Public Cloud und lokaler Umgebung
* Rechenzentren und Benutzerstandorten (Zweigstellen)
* Subnetzen, die verschiedene Ebenen einer Multi-Tier-Anwendung hosten.

Weitere Informationen finden Sie unter [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Application Gateway- und Netzwerksicherheitsgruppen-Analysen
Verwendung der Lösungen:
1. Fügen Sie Log Analytics die Verwaltungslösung hinzu, und
2. Aktivieren Sie die Diagnose, um die Diagnose an einen Log Analytics-Arbeitsbereich weiterzuleiten. Die Protokolle müssen nicht in Azure Blob Storage geschrieben werden.

Sie können die Diagnose und die entsprechende Lösung für Application Gateway und/oder Netzwerksicherheitsgruppen aktivieren.

Wenn Sie die Diagnoseprotokollierung für einen bestimmten Ressourcentyp nicht aktivieren, aber die Lösung installieren, sind die Dashboardblätter für diese Ressource leer und enthalten eine Fehlermeldung.

> [!NOTE]
> Im Januar 2017 wurde die unterstützte Vorgehensweise zum Senden von Protokollen von Application Gateways und Netzwerksicherheitsgruppen an Log Analytics geändert. Wenn Sie die **Azure Networking Analytics**-Lösung (veraltet) sehen, finden Sie unter [Migrieren von der alten Networking Analytics-Lösung](#migrating-from-the-old-networking-analytics-solution) Schritte, die Sie befolgen müssen.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Überprüfen der Details zur Datensammlung für Azure Networking
Die Azure Application Gateway-Analysen und die Analyseverwaltungslösungen für Netzwerksicherheitsgruppen sammeln Diagnoseprotokolle direkt aus Azure Application Gateways und Netzwerksicherheitsgruppen. Die Protokolle müssen nicht in Azure Blob Storage geschrieben werden, und es wird kein Agent für die Datensammlung benötigt.

Die folgende Tabelle zeigt Datenerfassungsmethoden und andere Details zur Art der Erfassung von Daten für Azure Application Gateway- und Netzwerksicherheitsgruppen-Analysen.

| Plattform | Direkt-Agent | System Center Operations Manager-Agent | Azure | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |Bei der Anmeldung |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Azure Application Gateway-Analyselösung in Log Analytics

![Symbol für Azure Application Gateway-Analyse](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

Die folgenden Protokolle werden für Anwendungsgateways unterstützt:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

Für Anwendungsgateways werden folgende Metriken unterstützt:

* Durchsatz innerhalb von fünf Minuten

### <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung
Gehen Sie folgendermaßen vor, um die Azure Application Gateway-Analyselösung zu installieren und zu konfigurieren:

1. Aktivieren Sie die Azure Application Gateway-Analyselösung in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) oder anhand des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses.
2. Aktivieren Sie die Diagnoseprotokollierung für die [Application Gateways](../application-gateway/application-gateway-diagnostics.md), die Sie überwachen möchten.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Aktivieren der Azure Application Gateway-Diagnose im Portal

1. Navigieren Sie im Azure-Portal zur Application Gateway-Ressource, die überwacht werden soll.
2. Wählen Sie *Diagnoseprotokolle* aus, um die folgende Seite zu öffnen.

   ![Abbildung der Azure Application Gateway-Ressource](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. Klicken Sie auf *Diagnose aktivieren*, um die folgende Seite zu öffnen.

   ![Abbildung der Azure Application Gateway-Ressource](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. Um die Diagnose zu aktivieren, klicken Sie unter *Status* auf *Ein*.
5. Klicken Sie auf das Kontrollkästchen *An Log Analytics senden*.
6. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen Arbeitsbereich.
7. Klicken Sie für jeden der Protokolltypen, die erfasst werden sollen, auf das Kontrollkästchen unter **Protokoll**.
8. Klicken Sie auf *Speichern*, um die Protokollierung der Diagnose in Log Analytics zu aktivieren.

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Aktivieren der Azure-Netzwerkdiagnose mit PowerShell

Das folgende PowerShell-Skript zeigt ein Beispiel für die Aktivierung der Diagnoseprotokollierung für Application Gateways.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Verwenden der Azure Application Gateway-Analyse
![Abbildung der Azure Application Gateway-Analyse](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

Nach dem Klicken auf die Kachel **Azure Application Gateway analytics** (Azure Application Gateway-Analyse) in der Übersicht können Sie Zusammenfassungen der Protokolle anzeigen und dann die Details für die folgenden Kategorien anzeigen:

* Application Gateway-Zugriffsprotokolle
  * Client- und Serverfehler für Application Gateway-Zugriffsprotokolle
  * Anforderungen pro Stunde für jedes Anwendungsgateway
  * Anforderungen mit Fehlern pro Stunde für jedes Anwendungsgateway
  * Fehler nach Benutzer-Agent für Anwendungsgateways
* Application Gateway-Leistung
  * Integrität von Hosts für Application Gateway
  * Maximum und 95. Perzentil für Application Gateway-Anforderungen mit einem Fehler

![Abbildung des Azure Application Gateway-Analysedashboards](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Abbildung des Azure Application Gateway-Analysedashboards](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

Prüfen Sie auf dem **Azure Application Gateway**-Analysedashboard die Zusammenfassungsinformationen auf einem der Blätter, und klicken Sie dann auf einen Eintrag, um ausführliche Informationen auf der Seite „Protokollsuche“ anzuzeigen.

Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Analyselösung der Azure-Netzwerksicherheitsgruppe in Log Analytics

![Symbol für Azure-Netzwerksicherheitsgruppen-Analyse](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

Die folgenden Protokolle werden für Netzwerksicherheitsgruppen unterstützt:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung
Gehen Sie folgendermaßen vor, um die Azure Networking Analytics-Lösung zu installieren und zu konfigurieren:

1. Aktivieren Sie die Azure-Netzwerksicherheitsgruppen-Analyselösung in [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) oder mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses.
2. Aktivieren Sie die Diagnoseprotokollierung für die [Netzwerksicherheitsgruppen](../virtual-network/virtual-network-nsg-manage-log.md)-Ressourcen, die Sie überwachen möchten.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Aktivieren der Diagnose für die Azure-Netzwerksicherheitsgruppe im Portal

1. Navigieren Sie im Azure-Portal zur Netzwerksicherheitsgruppen-Ressource, die überwacht werden soll
2. Wählen Sie *Diagnoseprotokolle* aus, um die folgende Seite zu öffnen.

   ![Abbildung der Azure-Netzwerksicherheitsgruppen-Ressource](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. Klicken Sie auf *Diagnose aktivieren*, um die folgende Seite zu öffnen.

   ![Abbildung der Azure-Netzwerksicherheitsgruppen-Ressource](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. Um die Diagnose zu aktivieren, klicken Sie unter *Status* auf *Ein*.
5. Klicken Sie auf das Kontrollkästchen *An Log Analytics senden*.
6. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen Arbeitsbereich.
7. Klicken Sie für jeden der Protokolltypen, die erfasst werden sollen, auf das Kontrollkästchen unter **Protokoll**.
8. Klicken Sie auf *Speichern*, um die Protokollierung der Diagnose in Log Analytics zu aktivieren.

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Aktivieren der Azure-Netzwerkdiagnose mit PowerShell

Das folgende PowerShell-Skript zeigt ein Beispiel für die Aktivierung der Diagnoseprotokollierung für die Netzwerksicherheitsgruppen:
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Verwenden von Azure-Netzwerksicherheitsgruppen-Analysen
Nach dem Klicken auf die Kachel **Azure Network Security Group analytics** (Azure-Netzwerksicherheitsgruppen-Analyse) in der Übersicht können Sie Zusammenfassungen der Protokolle anzeigen und dann die Details für die folgenden Kategorien anzeigen:

* Blockierte Flows der Netzwerksicherheitsgruppe
  * Regeln der Netzwerksicherheitsgruppe mit blockierten Flows
  * MAC-Adressen mit blockierten Flows
* Zugelassene Flows der Netzwerksicherheitsgruppe
  * Regeln der Netzwerksicherheitsgruppe mit zugelassenen Flows
  * MAC-Adressen mit zugelassenen Flows

![Abbildung des Azure-Netzwerksicherheitsgruppen-Analysedashboards](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Abbildung des Azure-Netzwerksicherheitsgruppen-Analysedashboards](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

Prüfen Sie im **Azure Network Security Group analytics** (Azure-Netzwerksicherheitsgruppen-Analyse)-Dashboard die Zusammenfassungsinformationen auf einem der Blätter, und klicken Sie dann auf einen Eintrag, um ausführliche Informationen auf der Seite „Protokollsuche“ anzuzeigen.

Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Migrieren von der alten Networking Analytics-Lösung
Im Januar 2017 wurde die unterstützte Vorgehensweise zum Senden von Protokollen von Azure Application Gateways und Azure-Netzwerksicherheitsgruppen an Log Analytics geändert. Diese Änderungen bieten folgende Vorteile:
+ Protokolle werden direkt in Log Analytics geschrieben, sodass kein Speicherkonto mehr notwendig ist
+ Geringere Latenz ab dem Zeitpunkt, zu dem Protokolle generiert werden, bis zu ihrer Verfügbarkeit in Log Analytics
+ Weniger Konfigurationsschritte
+ Ein gemeinsames Format für alle Typen der Azure-Diagnose

So verwenden Sie die aktualisierte Lösung:

1. [Configure diagnostics to be sent directly to Log Analytics from Azure Application Gateways](#enable-azure-application-gateway-diagnostics-in-the-portal) (Konfigurieren der Diagnose, die direkt aus Azure Application Gateways an Log Analytics gesendet werden soll)
2. [Configure diagnostics to be sent directly to Log Analytics from Azure Network Security Groups](#enable-azure-network-security-group-diagnostics-in-the-portal) (Konfigurieren der Diagnose, die direkt aus Azure-Netzwerksicherheitsgruppen an Log Analytics gesendet werden soll)
2. Aktivieren Sie die *Azure Application Gateway*- und die *Azure-Netzwerksicherheitsgruppen*-Analyselösung mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses.
3. Aktualisieren Sie alle gespeicherten Abfragen, Dashboards oder Warnungen zur Verwendung des neuen Datentyps.
  + Der Typ lautet AzureDiagnostics. Sie können „ResourceType“ verwenden, um nach Azure-Netzwerkprotokollen zu filtern.

    | Alternativen: | Verwendung: |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + Ändern Sie für jedes Feld, dessen Name das Suffix „\_s“, „\_d“ oder „\_g“ enthält, das erste Zeichen in einen Kleinbuchstaben.
   + Für jedes Feld, dessen Name das Suffix „\_o“ enthält, werden die Daten basierend auf den geschachtelten Feldnamen in einzelne Felder aufgeteilt.
4. Entfernen Sie die *Azure Networking Analytics*-Lösung (veraltet).
  + Wenn Sie PowerShell einsetzen, verwenden Sie `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`.

Daten, die vor der Änderung gesammelt wurden, werden in der neuen Lösung nicht angezeigt. Sie können die Abfrage dieser Daten mithilfe des alten Typs und der alten Feldnamen fortsetzen.

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollsuchen in Log Analytics](log-analytics-log-searches.md), um ausführliche Azure-Diagnosedaten anzuzeigen.

