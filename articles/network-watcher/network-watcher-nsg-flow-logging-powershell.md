---
title: "Verwalten von Flowprotokollen für Netzwerksicherheitsgruppen mit Azure Network Watcher – PowerShell | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie Flowprotokolle für Netzwerksicherheitsgruppen in Azure Network Watcher mit PowerShell verwaltet werden."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 7a8f685c34709f6b2a2c7627f1a66659720100c0
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---


# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Konfigurieren von Flowprotokollen für Netzwerksicherheitsgruppen mit PowerShell

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-nsg-flow-logging-cli.md)
> - [REST-API](network-watcher-nsg-flow-logging-rest.md)

Flowprotokolle für Netzwerksicherheitsgruppen sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe anzeigen können. Diese Flowprotokolle sind im JSON-Format geschrieben und zeigen ausgehende und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr.

## <a name="register-insights-provider"></a>Registrieren eines Anbieters von Insights

Der Anbieter **Microsoft.Insights** muss registriert sein, damit die Datenflussprotokollierung ordnungsgemäß funktioniert. Wenn Sie sich nicht sicher sind, ob der Anbieter **Microsoft.Insights** registriert ist, führen Sie folgendes Skript aus.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Aktivieren von Flowprotokollen für Netzwerksicherheitsgruppen

Der Befehl zum Aktivieren von Flowprotokollen wird im folgenden Beispiel gezeigt:

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG-Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

## <a name="disable-network-security-group-flow-logs"></a>Deaktivieren von Flowprotokollen für Netzwerksicherheitsgruppen

Verwenden Sie das folgende Beispiel, um Flowprotokolle zu deaktivieren:

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Herunterladen eines Flowprotokolls

Bei der Erstellung wird der Speicherort eines Flowprotokolls definiert. Ein nützliches Tool für den Zugriff auf diese in einem Speicherkonto gespeicherten Flowprotokolle ist der Microsoft Azure-Speicher-Explorer, der hier heruntergeladen werden kann: http://storageexplorer.com/

Wenn ein Speicherkonto angegeben wird, werden Paketerfassungsdateien in einem Speicherkonto am folgenden Speicherort gespeichert:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Informationen zur Struktur des Protokolls finden Sie unter [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md) (Einführung in die Flowprotokollierung für Netzwerksicherheitsgruppen).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ihre NSG-Flowprotokolle mit Power BI visualisieren](network-watcher-visualize-nsg-flow-logs-power-bi.md).

Erfahren Sie, wie Sie [Ihre NSG-Flowprotokolle mit Open Source-Tools visualisieren](network-watcher-visualize-nsg-flow-logs-open-source-tools.md).
