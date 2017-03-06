---
title: "Verwalten von Datenflussprotokollen für Netzwerksicherheitsgruppen mit Azure Network Watcher – Azure CLI | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie Datenflussprotokolle für Netzwerksicherheitsgruppen in Azure Network Watcher mit der Azure CLI verwaltet werden."
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
translationtype: Human Translation
ms.sourcegitcommit: d2a65104743d9497debdc85c134fd1a06114c514
ms.openlocfilehash: 7b85a3857667d41085e3bf20d61b79f0a52338e2
ms.lasthandoff: 02/23/2017


---


# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a>Konfigurieren von Datenflussprotokollen für Netzwerksicherheitsgruppen mit der Azure CLI

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-nsg-flow-logging-cli.md)
> - [REST-API](network-watcher-nsg-flow-logging-rest.md)

Datenflussprotokolle für Netzwerksicherheitsgruppen sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe anzeigen können. Diese Datenflussprotokolle sind im JSON-Format geschrieben und zeigen ausgehende und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr.

## <a name="enable-network-security-group-flow-logs"></a>Aktivieren von Datenflussprotokollen für Netzwerksicherheitsgruppen

Der Befehl zum Aktivieren von Datenflussprotokollen wird im folgenden Beispiel gezeigt:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a>Deaktivieren von Datenflussprotokollen für Netzwerksicherheitsgruppen

Verwenden Sie das folgende Beispiel, um Datenflussprotokolle zu deaktivieren:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
```

## <a name="download-a-flow-log"></a>Herunterladen eines Datenflussprotokolls

Bei der Erstellung wird der Speicherort eines Datenflussprotokolls definiert. Ein nützliches Tool für den Zugriff auf diese in einem Speicherkonto gespeicherten Datenflussprotokolle ist der Microsoft Azure-Speicher-Explorer, der hier heruntergeladen werden kann: http://storageexplorer.com/

Wenn ein Speicherkonto angegeben wird, werden Paketerfassungsdateien in einem Speicherkonto am folgenden Speicherort gespeichert:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Informationen zur Struktur des Protokolls finden Sie unter [Übersicht über Datenflussprotokolle für Netzwerksicherheitsgruppen](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ihre NSG-Datenflussprotokolle mit Power BI visualisieren](network-watcher-visualize-nsg-flow-logs-power-bi.md).

Erfahren Sie, wie Sie [Ihre NSG-Datenflussprotokolle mit Open Source-Tools visualisieren](network-watcher-visualize-nsg-flow-logs-open-source-tools.md).

