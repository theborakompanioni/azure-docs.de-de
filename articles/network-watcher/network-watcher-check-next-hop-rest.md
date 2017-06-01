---
title: "Suchen des nächsten Hops mit dem Azure Network Watcher-Feature „Nächster Hop“ – REST | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure-REST-API den Typ und die IP-Adresse des nächsten Hops feststellen können."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 644713d365191bf5e51517d0cc565efbc2abc144
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a>Herausfinden des Typs des nächsten Hops über die Azure-REST-API mithilfe der Funktion „Nächster Hop“ in Azure Network Watcher

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure-REST-API](network-watcher-check-next-hop-rest.md)

„Nächster Hop“ ist ein Feature von Network Watcher, das Ihnen die Möglichkeit bietet, den Typ und die IP-Adresse des nächsten Hops basierend auf einem angegebenen virtuellen Computer abzurufen. Mithilfe dieser Funktion können Sie ermitteln, ob der von einem virtuellen Computer ausgehende Datenverkehr ein Gateway, das Internet oder virtuelle Netzwerke durchquert, um zum Ziel zu gelangen.

## <a name="before-you-begin"></a>Voraussetzungen

ARMClient dient zum Aufrufen der REST-API mithilfe von PowerShell. Sie finden [ARMClient auf Chocolatey](https://chocolatey.org/packages/ARMClient).

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird „Nächster Hop“ verwendet, ein Network Watcher-Feature, mit dem der Typ und die IP-Adresse des nächsten Hops für eine Ressource ermittelt werden können. Weitere Informationen zu „Nächster Hop“ finden Sie unter [Übersicht über „Nächster Hop“](network-watcher-next-hop-overview.md).

In diesem Szenario führen Sie Folgendes durch:

* Abrufen des nächsten Hops für einen virtuellen Computer.

## <a name="log-in-with-armclient"></a>Anmelden mit ARMClient

Melden Sie sich mit Ihren Azure-Anmeldeinformationen bei ARMClient an.

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Abrufen eines virtuellen Computers

Führen Sie das folgende Skript aus, um einen virtuellen Computer zurückzugeben. Diese Informationen werden für die Ausführung des nächsten Hops benötigt.

Der folgende Code benötigt Werte für die folgenden Variablen:

- **subscriptionId:** die zu verwendende Abonnement-ID.
- **resourceGroupName:** der Name einer Ressourcengruppe, die virtuelle Computer enthält.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Die ID des virtuellen Computers aus der folgenden Ausgabe wird im nächsten Beispiel verwendet:

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a>Abrufen des nächsten Hops

Sobald der Autorisierungsheader erstellt wurde, kann der nächste Hop von einem virtuellen Computer abgerufen werden. Damit das Codebeispiel funktioniert, müssen die folgenden Werte ersetzt werden.

> [!Important]
> Für Network Watcher-REST-API-Aufrufe ist der Ressourcengruppenname im Anforderungs-URI die Ressourcengruppe, die die Network Watcher-Instanz enthält, nicht die Ressourcen, für die Sie die Diagnoseaktionen ausführen.

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> „Nächster Hop“ setzt voraus, dass die VM-Ressource für die Ausführung zugeordnet wird.

## <a name="results"></a>Ergebnisse

Der folgende Codeausschnitt ist ein Beispiel der empfangenen Ausgabe. Die Ergebnisse umfassen die folgenden Werte:

* **nextHopType**: Mögliche Werte sind „Internet“, „VirtualAppliance“, „VirtualNetworkGateway“, „VnetLocal“, „HyperNetGateway“ oder „None“.
* **nextHopIpAddress**: Die IP-Adresse des nächsten Hops.
* **routeTableId**: Der Wert ist entweder ein URI für die Routingtabelle, die der Route zugeordnet ist, oder es wird der Wert von *System Route* zurückgegeben, wenn keine benutzerdefinierte Route definiert ist.

Im Folgenden sehen Sie die Ergebnisse im JSON-Format.

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den nächsten Hop für einen virtuellen Computer ermitteln konnten, können Sie die Sicherheit Ihrer Netzwerkressourcen anzeigen, indem Sie die [Übersicht der Sicherheitsansicht](network-watcher-security-group-view-overview.md) aufrufen.















