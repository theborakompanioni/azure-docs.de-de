---
title: "Anzeigen der Azure Network Watcher-Topologie – REST-API | Microsoft-Dokumentation"
description: In diesem Artikel wird die Verwendung der REST-API zum Abfragen Ihrer Netzwerktopologie beschrieben.
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 568f3060da372f4a08cec342e04359172522cb69
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="view-network-watcher-topology-with-rest-api"></a>Anzeigen der Network Watcher-Topologie mit der REST-API

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST-API](network-watcher-topology-rest.md)

Das Topologiefeature von Network Watcher bietet eine visuelle Darstellung der Netzwerkressourcen in einem Abonnement. Im Portal wird Ihnen diese Visualisierung automatisch angezeigt. Die der Topologieansicht im Portal zugrunde liegenden Informationen können mit PowerShell abgerufen werden.
Diese Funktion verleiht den Topologieinformationen Flexibilität, da die Daten von anderen Tools zur Visualisierung genutzt werden können.

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

In diesem Szenario rufen Sie die Topologieinformationen ab. ARMclient dient zum Aufrufen der REST-API mithilfe von PowerShell. Sie finden [ARMClient auf Chocolatey](https://chocolatey.org/packages/ARMClient).

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird die Topologieantwort für eine bestimmte Ressourcengruppe abgerufen.

## <a name="log-in-with-armclient"></a>Anmelden mit ARMClient

Melden Sie sich mit Ihren Azure-Anmeldeinformationen bei ARMClient an.

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a>Abrufen der Topologie

Das folgende Beispiel ruft die Topologie über die REST-API ab.  Das Beispiel ist parametrisiert, um mehr Flexibilität bei der Erstellung des Beispiels zu erhalten.  Ersetzen Sie alle Werte, die mit \< \> umschlossen sind.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name to run topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

Die folgende Antwort ist ein Beispiel für eine verkürzte Antwort, die beim Abrufen der Topologie für eine Ressourcengruppe zurückgegeben wird:

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Visualisieren der Datenflussprotokolle von Netzwerksicherheitsgruppen mit Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md), wie Sie Ihre NSG-Datenflussprotokolle mit Power BI visualisieren.


