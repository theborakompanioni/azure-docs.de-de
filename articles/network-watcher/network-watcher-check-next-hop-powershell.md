---
title: "Suchen des nächsten Hops mit dem Azure Network Watcher-Feature „Nächster Hop“ – PowerShell | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit PowerShell den Typ und die IP-Adresse des nächsten Hops feststellen können."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1d0136b044f6049e59fa09d824cf244cac703c45
ms.openlocfilehash: 9f53c824b6368dc2a6251fd880f1cabefef884b8
ms.lasthandoff: 02/23/2017


---

# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-powershell"></a>Herausfinden des Typs des nächsten Hops über PowerShell mithilfe der Funktion „Nächster Hop“ in Azure Network Watcher

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-check-next-hop-cli.md)
> - [Azure-REST-API](network-watcher-check-next-hop-rest.md)

„Nächster Hop“ ist ein Feature von Network Watcher, das Ihnen die Möglichkeit bietet, den Typ und die IP-Adresse des nächsten Hops basierend auf einem angegebenen virtuellen Computer abzurufen. Mithilfe dieses Features können Sie ermitteln, ob der von einem virtuellen Computer ausgehende Datenverkehr ein Gateway, das Internet oder virtuelle Netzwerke durchquert, um zum Ziel zu gelangen.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Szenario verwenden Sie das Azure-Portal, um den Typ und die IP-Adresse des nächsten Hops zu finden.

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird „Nächster Hop“ verwendet, ein Network Watcher-Feature, mit dem der Typ und die IP-Adresse des nächsten Hops für eine Ressource ermittelt werden können. Weitere Informationen zu „Nächster Hop“ finden Sie unter [Übersicht über „Nächster Hop“](network-watcher-next-hop-overview.md).

## <a name="retrieve-network-watcher"></a>Abrufen von Network Watcher

Der erste Schritt besteht im Abrufen der Network Watcher-Instanz. Die `$networkWatcher`-Variable wird an das Cmdlet zur Überprüfung des nächsten Hops übergeben.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a>Abrufen eines virtuellen Computers

„Nächster Hop“ gibt den nächsten Hop und die IP-Adresse des nächsten Hops von einem virtuellen Computer zurück. Eine ID eines virtuellen Computers ist zum Ausführen des Cmdlets erforderlich. Wenn Sie die ID des virtuellen Computers bereits kennen, können Sie diesen Schritt überspringen.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> „Nächster Hop“ setzt voraus, dass die VM-Ressource für die Ausführung zugeordnet wird.

## <a name="get-the-network-interfaces"></a>Abrufen der Netzwerkschnittstellen

Die IP-Adresse einer NIC auf dem virtuellen Computer wird benötigt. In diesem Beispiel rufen wir die NICs auf einem virtuellen Computer ab. Wenn Sie die IP-Adresse bereits kennen, die Sie auf dem virtuellen Computer testen möchten, können Sie diesen Schritt überspringen.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkInterfaceIDs.ForEach({$_})}
```

## <a name="get-next-hop"></a>Abrufen des nächsten Hops

Jetzt rufen wir das `Get-AzureRmNetworkWatcherNextHop`-Cmdlet ab. Wir übergeben dem Cmdlet Network Watcher, VM-ID, Quell-IP-Adresse und Ziel-IP-Adresse. In diesem Beispiel entspricht die Ziel-IP-Adresse einem virtuellen Computer in einem anderen virtuellen Netzwerk. Zwischen den beiden virtuellen Netzwerken liegt ein Gateway für virtuelle Netzwerke.

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a>Überprüfen der Ergebnisse

Nach Abschluss des Vorgangs werden die Ergebnisse bereitgestellt. Die IP-Adresse des nächsten Hops sowie der Typ der entsprechenden Ressource werden zurückgegeben. In diesem Szenario ist es die öffentliche IP-Adresse des Gateways für virtuelle Netzwerke.

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

Die folgende Liste zeigt die derzeit verfügbaren NextHopType-Werte:

**Typ des nächsten Hops**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Keine

## <a name="next-steps"></a>Nächste Schritte

Informationen zur programmgesteuerten Überprüfung der Einstellungen Ihrer Netzwerksicherheitsgruppe finden Sie unter [NSG-Überwachung mit Network Watcher](network-watcher-nsg-auditing-powershell.md).


















