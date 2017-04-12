---
title: Erstellen einer Azure Network Watcher-Instanz | Microsoft Docs
description: "Diese Seite enthält die Schritte zum Erstellen einer Network Watcher-Instanz mit dem Portal und der Azure-REST-API."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 7d8eb5972d35eac6cb55fc393090cfcc21ec341c
ms.lasthandoff: 03/31/2017

---

# <a name="create-an-azure-network-watcher-instance"></a>Erstellen einer Azure Network Watcher-Instanz

Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Überwachung auf Szenarioebene erlaubt die umfassende Diagnose von Problemen auf Netzwerkebene. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

> [!NOTE]
> Da Network Watcher derzeit nur CLI 1.0 unterstützt, werden die Anweisungen zum Erstellen einer neuen Network Watcher-Instanz für CLI 1.0 bereitgestellt.

## <a name="create-a-network-watcher-in-the-portal"></a>Erstellen einer Network Watcher-Instanz im Portal

Navigieren Sie zu **Weitere Dienste** > **Netzwerk** > **Network Watcher**. Sie können alle Abonnements auswählen, für die Sie Network Watcher aktivieren möchten. Mit dieser Aktion erstellen Sie eine Network Watcher-Instanz in jeder verfügbaren Region.

![Erstellen einer Network Watcher-Instanz][1]

## <a name="create-a-network-watcher-with-powershell"></a>Erstellen einer Network Watcher-Instanz mit PowerShell

Um eine Network Watcher-Instanz zu erstellen, führen Sie das folgende Beispiel aus:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Erstellen einer Network Watcher-Instanz mit der REST-API

ARMClient dient zum Aufrufen der REST-API mithilfe von PowerShell. Sie finden [ARMClient auf Chocolatey](https://chocolatey.org/packages/ARMClient).

### <a name="log-in-with-armclient"></a>Anmelden mit ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Erstellen der Network Watcher-Instanz

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, nachdem Sie über eine Instanz von Network Watcher verfügen, welche Features Ihnen zur Verfügung stehen:

* [Topologie](network-watcher-topology-overview.md)
* [Paketerfassung](network-watcher-packet-capture-overview.md)
* [IP-Datenflussüberprüfung](network-watcher-ip-flow-verify-overview.md)
* [Nächster Hop](network-watcher-next-hop-overview.md)
* [Sicherheitsgruppenansicht](network-watcher-security-group-view-overview.md)
* [NSG-Datenflussprotokollierung](network-watcher-nsg-flow-logging-overview.md)
* [Beheben von Problemen bei Virtual Network-Gateways](network-watcher-troubleshoot-overview.md)

Nachdem eine Network Watcher-Instanz erstellt wurde, kann die Paketerfassung konfiguriert werden. Gehen Sie dabei anhand des Artikels [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md) vor.

[1]: ./media/network-watcher-create/figure1.png












