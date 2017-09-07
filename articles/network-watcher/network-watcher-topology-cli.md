---
title: "Anzeigen der Azure Network Watcher-Topologie – Azure CLI | Microsoft-Dokumentation"
description: In diesem Artikel wird die Verwendung der Azure CLI zum Abfragen Ihrer Netzwerktopologie beschrieben.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 5be8e103f9a1f32117a4ed3be73bff021db1186d
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="view-network-watcher-topology-with-azure-cli"></a>Anzeigen der Network Watcher-Topologie mit der Azure CLI

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST-API](network-watcher-topology-rest.md)

Das Topologiefeature von Network Watcher bietet eine visuelle Darstellung der Netzwerkressourcen in einem Abonnement. Im Portal wird Ihnen diese Visualisierung automatisch angezeigt. Die der Topologieansicht im Portal zugrunde liegenden Informationen können mit PowerShell abgerufen werden.
Diese Funktion verleiht den Topologieinformationen Flexibilität, da die Daten von anderen Tools zur Visualisierung genutzt werden können.

In diesem Artikel wird die plattformübergreifende Azure CLI 1.0 verwendet, die für Windows, Mac und Linux zur Verfügung steht. Network Watcher verwendet derzeit Azure CLI 1.0 als Unterstützung für die Befehlszeilenschnittstelle.

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

In diesem Szenario verwenden Sie das Cmdlet `network watcher topology` zum Abrufen der Topologieinformationen. Außerdem gibt es einen Artikel zum [Abrufen der Netzwerktopologie mit REST-API](network-watcher-topology-rest.md).

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird die Topologieantwort für eine bestimmte Ressourcengruppe abgerufen.

## <a name="retrieve-topology"></a>Abrufen der Topologie

Das Cmdlet `network watcher topology` ruft die Topologie für eine bestimmte Ressourcengruppe ab. Fügen Sie das Argument „--json“ hinzu, um die Ausgabe im JSON-Format anzuzeigen.

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a>Ergebnisse

Die zurückgegebenen Ergebnisse weisen eine Eigenschaft namens „Resources“ auf, welche den JSON-Antworttext für das Cmdlet `network watcher topology` enthält.  Die Antwort enthält die Ressourcen in der Netzwerksicherheitsgruppe und ihre Zuordnungen (d.h. „Contains“, „Associated“).

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Sicherheitsregeln, die auf Ihre Netzwerkressourcen angewendet werden, finden Sie unter [Übersicht über die Sicherheitsgruppenansicht](network-watcher-security-group-view-overview.md).

