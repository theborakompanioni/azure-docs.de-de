---
title: "Überprüfen der Konnektivität mit Azure Network Watcher – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Auf dieser Seite erfahren Sie, wie Sie die Konnektivitätsprüfung von Network Watcher mithilfe von Azure CLI 2.0 verwenden."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: c1deaa40bfda0bf3858ad56d3d6a90df34351278
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---

# <a name="check-connectivity-with-azure-network-watcher-using-azure-cli-20"></a>Überprüfen der Konnektivität mit Azure Network Watcher mithilfe von Azure CLI 2.0

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Azure-REST-API](network-watcher-connectivity-rest.md)

Erfahren Sie, wie Sie mit der Konnektivität überprüfen können, ob von einem virtuellen Computer zu einem bestimmten Endpunkt eine direkte TCP-Verbindung hergestellt werden kann.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Ressourcen verfügen:

* Eine Instanz von Network Watcher in der Region, in der Sie die Konnektivität überprüfen möchten

* Virtuelle Computer, mit denen Sie die Konnektivität überprüfen.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> Für die Konnektivitätsprüfung ist die VM-Erweiterung `AzureNetworkWatcherExtension` erforderlich. Informationen zur Installation der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Windows](../virtual-machines/windows/extensions-nwa.md) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="register-the-preview-capability"></a>Registrieren der Vorschaufunktion 

Die Konnektivitätsprüfung befindet sich derzeit in der öffentlichen Vorschauphase. Wenn Sie diese Funktion verwenden möchten, müssen Sie sie registrieren. Führen Sie zu diesem Zweck das folgende CLI-Beispiel aus:

```azurecli 
az feature register --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck

az provider register --namespace Microsoft.Network 
``` 

Um zu überprüfen, ob die Registrierung erfolgreich durchgeführt wurde, führen Sie den folgenden CLI-Befehl aus:

```azurecli
az feature show --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck 
```

Wenn das Feature ordnungsgemäß registriert wurde, sollte die Ausgabe der folgenden ähneln: 

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowNetworkWatcherConnectivityCheck",
  "name": "Microsoft.Network/AllowNetworkWatcherConnectivityCheck",
  "properties": {
    "state": "Registered"
  },
  "type": "Microsoft.Features/providers/features"
}
``` 

## <a name="check-connectivity-to-a-virtual-machine"></a>Überprüfen der Konnektivität zu einem virtuellen Computer

In diesem Beispiel wird die Konnektivität zu einem virtuellen Zielcomputer über Port 80 überprüft.

### <a name="example"></a>Beispiel

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a>Antwort

Die folgende Antwort stammt aus dem vorherigen Beispiel.  In dieser Antwort ist der `ConnectionStatus` **Nicht erreichbar**. Wie Sie sehen, sind alle gesendeten Tests fehlgeschlagen. Bei der Konnektivität ist am virtuellen Gerät ein Fehler aufgetreten. Der Grund dafür ist eine benutzerdefinierte `NetworkSecurityRule` mit der Bezeichnung **UserRule_Port80**, deren Konfiguration eingehenden Datenverkehr über Port 80 blockiert. Diese Informationen können bei der Untersuchung von Konnektivitätsproblemen verwendet werden.

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "bb01d336-d881-4808-9fbc-72f091974d68",
      "issues": [],
      "nextHopIds": [
        "f8b074e9-9980-496b-a35e-619f9bcbf648"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "10.1.2.4",
      "id": "f8b074e9-9980-496b-a35e-619f9bcbf648",
      "issues": [],
      "nextHopIds": [
        "8a5857f3-6ab8-4b11-b9bf-a046d66b8696"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fw
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.3.4",
      "id": "8a5857f3-6ab8-4b11-b9bf-a046d66b8696",
      "issues": [
        {
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule"
        }
      ],
      "nextHopIds": [
        "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/au
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.4.4",
      "id": "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/db
Nic0/ipConfigurations/ipconfig1",
      "type": "VnetLocal"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="validate-routing-issues"></a>Überprüfen von Routingproblemen

In diesem Beispiel wird die Konnektivität zwischen einem virtuellen Computer und einem Remoteendpunkt überprüft.

### <a name="example"></a>Beispiel

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a>Antwort

Im folgenden Beispiel wird der `connectionStatus` als **Nicht erreichbar** angezeigt. In den `hops`-Details können Sie unter `issues` sehen, dass der Datenverkehr aufgrund einer `UserDefinedRoute` blockiert war.

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "f2cb1868-2049-4839-b8ed-57a480d06f95",
      "issues": [
        {
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute"
        }
      ],
      "nextHopIds": [
        "da4022db-0ab0-48c4-a507-dd4c03561ca5"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "13.107.21.200",
      "id": "da4022db-0ab0-48c4-a507-dd4c03561ca5",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Unknown",
      "type": "Destination"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="check-website-latency"></a>Überprüfen der Websitelatenz

Im folgenden Beispiel wird die Konnektivität zu einer Website überprüft.

### <a name="example"></a>Beispiel

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address http://bing.com --dest-port 80
```

### <a name="response"></a>Antwort

In der folgenden Antwort wird der `connectionStatus` als **Erreichbar** angezeigt. Wenn eine Verbindung erfolgreich ist, werden Latenzwerte bereitgestellt.

```json
{
  "avgLatencyInMs": 2,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "639c2d19-e163-4dfd-8737-5018dd1168ae",
      "issues": [],
      "nextHopIds": [
        "fd43a6e7-c758-4f48-90aa-8db99105a4a3"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "204.79.197.200",
      "id": "fd43a6e7-c758-4f48-90aa-8db99105a4a3",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="check-connectivity-to-a-storage-endpoint"></a>Überprüfen der Konnektivität zu einem Speicherendpunkt

Im folgenden Beispiel wird die Konnektivität von einem virtuellen Computer mit einem Blobspeicherkonto überprüft.

### <a name="example"></a>Beispiel

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a>Antwort

Der folgende JSON-Code ist die Beispielantwort auf die Ausführung des vorherigen Cmdlets. Da die Überprüfung erfolgreich ist, wird die Eigenschaft `connectionStatus` als **Erreichbar** angezeigt.  Die Details in Bezug auf die Anzahl der Hops, die zum Erreichen des Speicherblobs und der Latenz benötigt werden, werden bereitgestellt.

```json
{
  "avgLatencyInMs": 1,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "5136acff-bf26-4c93-9966-4edb7dd40353",
      "issues": [],
      "nextHopIds": [
        "f8d958b7-3636-4d63-9441-602c1eb2fd56"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "1.2.3.4",
      "id": "f8d958b7-3636-4d63-9441-602c1eb2fd56",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).

Lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), um herauszufinden, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist.

