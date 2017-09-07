---
title: Erstellen einer Azure Network Watcher-Instanz | Microsoft Docs
description: "Diese Seite enthält die Schritte zum Erstellen einer Network Watcher-Instanz mit dem Portal und der Azure-REST-API."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 2aeaffdd5ab552e18677cbd1a24a748dd14bf172
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017

---

# <a name="create-an-azure-network-watcher-instance"></a>Erstellen einer Azure Network Watcher-Instanz

Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Überwachung auf Szenarioebene erlaubt die umfassende Diagnose von Problemen auf Netzwerkebene. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

> [!NOTE]
> Da Network Watcher derzeit nur CLI 1.0 unterstützt, werden die Anweisungen zum Erstellen einer neuen Network Watcher-Instanz für CLI 1.0 bereitgestellt.

## <a name="create-a-network-watcher-in-the-portal"></a>Erstellen einer Network Watcher-Instanz im Portal

Navigieren Sie zu **Weitere Dienste** > **Netzwerk** > **Network Watcher**. Sie können alle Abonnements auswählen, für die Sie Network Watcher aktivieren möchten. Mit dieser Aktion erstellen Sie eine Network Watcher-Instanz in jeder verfügbaren Region.

![Erstellen einer Network Watcher-Instanz][1]

Wenn Sie Network Watcher über das Portal aktivieren, wird der Name der Network Watcher-Instanz automatisch auf NetworkWatcher_Regionsname festgelegt, wobei Regionsname der Azure-Region entspricht, in der die Instanz aktiviert wurde.  Ein in der Region „West Central US“ (USA, Westen-Mitte) aktivierter Network Watcher erhält beispielsweise den Namen NetworkWatcher_westcentralus.

Zudem wird die Network Watcher-Instanz automatisch zu einer Ressourcengruppe mit dem Namen NetworkWatcherRG hinzugefügt.  Wenn diese Ressourcengruppe nicht bereits vorhanden ist, wird sie erstellt.

Wenn Sie den Namen einer Network Watcher-Instanz und der Ressourcengruppe, der sie angehört, anpassen möchten, können Sie die im Folgenden beschriebenen Powershell-, REST-API- oder ARMClient-Methoden verwenden.  Bei jeder Methode muss die Ressourcengruppe bereits vorhanden sein, damit Sie die Network Watcher-Instanz dort einfügen können.  

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












