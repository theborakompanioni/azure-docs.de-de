---
title: "Überwachen von Vorgängen, Ereignissen und Leistungsindikatoren für NSGs | Microsoft Docs"
description: "Erfahren Sie, wie Sie Leistungsindikatoren-, Ereignis- und Überwachungsprotokolle für NSGs aktivieren."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: f907f388ecb897c05e822f2abd5431b3976b7987
ms.openlocfilehash: 10581234a4475d0d3b32c7891fcf97eed55f7a1d
ms.lasthandoff: 03/02/2017


---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)

Sie können die folgenden Diagnoseprotokoll-Kategorien für Netzwerksicherheitsgruppen aktivieren:

* **Ereignis:** Enthält Einträge, für die NSG-Regeln auf virtuelle Computer und Instanzrollen auf Grundlage der MAC-Adresse angewendet werden. Der Status für diese Regeln wird alle 60 Sekunden erfasst.
* **Regelzähler**: Enthält Einträge darüber, wie oft jede NSG-Regel angewendet wurde, um Datenverkehr zuzulassen oder zu verweigern.

> [!NOTE]
> Diagnoseprotokolle sind nur für NSGs verfügbar, die im Azure Resource Manager-Bereitstellungsmodell bereitgestellt werden. Sie können keine Diagnoseprotokollierung für Netzwerksicherheitsgruppen aktivieren, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden. Zum besseren Verständnis der beiden Modelle lesen Sie den Artikel [Azure Resource Manager vs. classic deployment: Understand deployment models and the state of your resources](../resource-manager-deployment-model.md) (Azure Resource Manager im Vergleich zur klassischen Bereitstellung: Grundlegendes zu den Bereitstellungsmodellen und zum Status Ihrer Ressourcen).

Aktivitätsprotokollierung (vormals bekannt als Audit oder Betriebsprotokolle) ist für alle Netzwerksicherheitsgruppen unabhängig davon , in welchem Azure-Bereitstellungsmodell sie erstellt wurden, standardmäßig aktiviert. Um zu bestimmen, welche Vorgänge für Netzwerksicherheitsgruppen im Aktivitätsprotokoll abgeschlossen wurden, suchen Sie nach Einträgen, die die folgenden Ressourcentypen enthalten: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Lesen Sie den Artikel [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) (Übersicht zum Azure-Aktivitätsprotokoll), um weitere Informationen zu Aktivitätsprotokollen zu erhalten. 

## <a name="enable-diagnostic-logging"></a>Aktivieren der Diagnoseprotokollierung

Diagnoseprotokollierung muss für *jede* NSG aktiviert sein, für die Sie Daten sammeln möchten. Im Artikel [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) (Übersicht zu Azure-Diagnoseprotokollen) wird erläutert, wohin Diagnoseprotokolle gesendet werden können. Wenn Sie nicht über eine NSG verfügen, führen Sie die Schritte im Artikel [Verwalten von NSGs mithilfe des Azure-Portals](virtual-networks-create-nsg-arm-pportal.md) aus, um eine zu erstellen. Sie können die NSG-Diagnoseprotokollierung mit einer der folgenden Methoden aktivieren:

### <a name="azure-portal"></a>Azure-Portal

Um die Protokollierung über das Portal zu aktivieren, melden Sie sich beim [Portal](https://portal.azure.com) an. Klicken Sie auf **Weitere Dienste**, und geben Sie dann *Netzwerksicherheitsgruppen* ein. Wählen Sie die NSG aus, für die Sie die Protokollierung aktivieren möchten. Befolgen Sie die Anweisungen für Nicht-Computeressourcen im Artikel [Enable Diagnostic Logs in the portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#enable-diagnostic-logs-in-the-portal) (Aktivieren von Diagnoseprotokollen im Portal). Wählen Sie **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter** oder beide Kategorien von Protokollen.

### <a name="powershell"></a>PowerShell

Zur Aktivierung der Protokollierung mit PowerShell führen Sie die Anweisungen im Artikel [Enable diagnostic logs via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#enable-diagnostic-logs-via-powershell) (Aktivieren von Diagnoseprotokollen mit PowerShell) aus. Werten Sie die folgenden Informationen aus, bevor Sie einen Befehl aus dem Artikel eingeben:

- Sie können den für den `-ResourceId`-Parameter zu verwendenden Wert bestimmen, indem Sie den folgenden [Text] nach Bedarf ersetzen und dann den Befehl `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]` eingeben. Die ID-Ausgabe des Befehls sieht etwa wie folgt aus: */subscriptions/[Abonnement-ID]/resourceGroups/[Ressourcengruppe]/providers/Microsoft.Network/networkSecurityGroups/[NSG-Name]*.
- Wenn Sie nur Daten aus der Protokollkategorie erfassen möchten, fügen Sie `-Categories [category]` am Ende des Befehls im Artikel hinzu, wobei die Kategorie entweder *NetworkSecurityGroupEvent* oder *NetworkSecurityGroupRuleCounter* ist. Wenn Sie den `-Categories`-Parameter nicht verwenden, wird die Datensammlung für beide Protokollkategorien aktiviert.

### <a name="azure-command-line-interface-cli"></a>Azure-Befehlszeilenschnittstelle (CLI)

Zur Aktivierung der Protokollierung mit CLI führen Sie die Anweisungen im Artikel [Enable diagnostic logs via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#enable-diagnostic-logs-via-cli) (Aktivieren von Diagnoseprotokollen mit CLI) aus. Werten Sie die folgenden Informationen aus, bevor Sie einen Befehl aus dem Artikel eingeben:

- Sie können den für den `-ResourceId`-Parameter zu verwendenden Wert bestimmen, indem Sie den folgenden [Text] nach Bedarf ersetzen und dann den Befehl `azure network nsg show [resource-group-name] [nsg-name]` eingeben. Die ID-Ausgabe des Befehls sieht etwa wie folgt aus: */subscriptions/[Abonnement-ID]/resourceGroups/[Ressourcengruppe]/providers/Microsoft.Network/networkSecurityGroups/[NSG-Name]*.
- Wenn Sie nur Daten aus der Protokollkategorie erfassen möchten, fügen Sie `-Categories [category]` am Ende des Befehls im Artikel hinzu, wobei die Kategorie entweder *NetworkSecurityGroupEvent* oder *NetworkSecurityGroupRuleCounter* ist. Wenn Sie den `-Categories`-Parameter nicht verwenden, wird die Datensammlung für beide Protokollkategorien aktiviert.

## <a name="logged-data"></a>Protokollierte Daten

JSON-formatierte Daten werden für beide Protokolle geschrieben. Die für jeden Protokolltyp geschriebenen spezifischen Daten werden in den folgenden Abschnitten aufgeführt:

### <a name="event-log"></a>Ereignisprotokoll
Dieses Protokoll enthält Informationen darüber, welche NSG-Regeln basierend auf der MAC-Adresse für virtuelle Computer und Clouddienst-Rolleninstanzen gelten. Die folgenden Beispieldaten werden für jedes Ereignis protokolliert:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>Regelzählerprotokoll

Dieses Protokoll enthält Informationen über jede Regel, die auf Ressourcen angewendet. Die folgenden Beispieldaten werden jedes Mal protokolliert, wenn eine Regel angewendet wird:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Anzeigen und Analysieren von Protokollen

Lesen Sie den Artikel [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) (Übersicht zum Azure-Aktivitätsprotokoll), um zu erfahren, wie Sie Aktivitätsprotokolldaten anzeigen. Lesen Sie den Artikel [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) (Übersicht zu Azure-Diagnoseprotokollen), um zu erfahren, wie Sie Diagnoseprotokolldaten anzeigen. Wenn Sie Diagnosedaten an Log Analytics senden, können Sie mithilfe der Verwaltungslösung für die [Azure-Netzwerksicherheitsgruppen-Analyse](../log-analytics/log-analytics-azure-networking-analytics.md) (Vorschau) verbesserte Einblicke erhalten. 

