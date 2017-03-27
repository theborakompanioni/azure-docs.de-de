---
title: "Anzeigen der Azure Network Watcher-Topologie – PowerShell | Microsoft-Dokumentation"
description: In diesem Artikel wird die Verwendung von PowerShell zum Abfragen Ihrer Netzwerktopologie beschrieben.
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: c8d5fe6177b36aa73febe308b47b1fc330181924
ms.lasthandoff: 03/22/2017

---

# <a name="view-network-watcher-topology-with-powershell"></a>Anzeigen der Network Watcher-Topologie mit PowerShell

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-topology-cli.md)
> - [REST-API](network-watcher-topology-rest.md)

Das Topologiefeature von Network Watcher bietet eine visuelle Darstellung der Netzwerkressourcen in einem Abonnement. Im Portal wird Ihnen diese Visualisierung automatisch angezeigt. Die der Topologieansicht im Portal zugrunde liegenden Informationen können mit PowerShell abgerufen werden.
Diese Funktion verleiht den Topologieinformationen Flexibilität, da die Daten von anderen Tools zur Visualisierung genutzt werden können.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

Die Verbindung wird durch zwei Beziehungen umgesetzt.

- **Einschlussbeziehung** – Beispiel: VNet enthält ein Subnetz mit einer NIC.
- **Zuordnungsbeziehung** – Beispiel: Die NIC ist einem virtuellen Computer zugeordnet.

Die folgende Liste enthält Eigenschaften, die beim Abfragen der Topologie-REST-API zurückgegeben werden.

* **name**: Der Name der Ressource.
* **id**: Der URI der Ressource.
* **location**: Der Standort, an dem die Ressource vorliegt.
* **associations**: Eine Liste der Zuordnungen für das referenzierte Objekt.
    * **name**: Der Name der referenzierten Ressource.
    * **resourceId**: resourceId ist der URI der Ressource, auf die in der Zuordnung verwiesen wird.
    * **associationType**: Dieser Wert verweist auf die Beziehung zwischen dem untergeordneten Objekt und dem übergeordneten Objekt. Gültige Werte sind **Contains** oder **Associated**.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Szenario verwenden Sie das Cmdlet `Get-AzureRmNetworkWatcherTopology` zum Abrufen der Topologieinformationen. Außerdem gibt es einen Artikel zum [Abrufen der Netzwerktopologie mit REST-API](network-watcher-topology-rest.md).

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird die Topologieantwort für eine bestimmte Ressourcengruppe abgerufen.

## <a name="retrieve-network-watcher"></a>Abrufen von Network Watcher

Der erste Schritt besteht im Abrufen der Network Watcher-Instanz. Die Variable `$networkWatcher` wird an das Cmdlet `Get-AzureRmNetworkWatcherTopology` übergeben.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a>Abrufen der Topologie

Das Cmdlet `Get-AzureRmNetworkWatcherTopology` ruft die Topologie für eine bestimmte Ressourcengruppe ab.

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a>Ergebnisse

Die zurückgegebenen Ergebnisse weisen eine Eigenschaft namens „Resources“ auf, welche den JSON-Antworttext für das Cmdlet `Get-AzureRmNetworkWatcherTopology` enthält.  Die Antwort enthält die Ressourcen in der Netzwerksicherheitsgruppe und ihre Zuordnungen (d.h. „Contains“, „Associated“).

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Visualisieren der Datenflussprotokolle von Netzwerksicherheitsgruppen mit Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md), wie Sie Ihre NSG-Datenflussprotokolle mit Power BI visualisieren.



