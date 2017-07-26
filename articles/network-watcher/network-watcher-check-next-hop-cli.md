---
title: "Suchen des nächsten Hops mit dem Azure Network Watcher-Feature „Nächster Hop“ – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI den Typ und die IP-Adresse des nächsten Hops feststellen können."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d1ee6870ba0188ff2c473e4cca12a5bdc1f97d3d
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---

# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a>Ermitteln des Typs des nächsten Hops über Azure CLI 2.0 mithilfe der Funktion „Nächster Hop“ in Azure Network Watcher

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure-REST-API](network-watcher-check-next-hop-rest.md)

„Nächster Hop“ ist ein Feature von Network Watcher, das Ihnen die Möglichkeit bietet, den Typ und die IP-Adresse des nächsten Hops basierend auf einem angegebenen virtuellen Computer abzurufen. Mithilfe dieses Features können Sie ermitteln, ob der von einem virtuellen Computer ausgehende Datenverkehr ein Gateway, das Internet oder virtuelle Netzwerke durchquert, um zum Ziel zu gelangen.

In diesem Artikel wird unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell verwendet, Azure CLI 2.0, die für Windows, Mac und Linux verfügbar ist.

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI) installieren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Szenario verwenden Sie die Azure CLI, um den Typ und die IP-Adresse des nächsten Hops zu finden.

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird „Nächster Hop“ verwendet, ein Network Watcher-Feature, mit dem der Typ und die IP-Adresse des nächsten Hops für eine Ressource ermittelt werden können. Weitere Informationen zu „Nächster Hop“ finden Sie unter [Übersicht über „Nächster Hop“](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Abrufen des nächsten Hops

Zum Abrufen des nächsten Hops rufen wir das Cmdlet `az network watcher show-next-hop` auf. Wir geben im Cmdlet die Network Watcher-Ressourcengruppe, den NetworkWatcher, die VM-ID, die Quell-IP-Adresse und die Ziel-IP-Adresse an. In diesem Beispiel entspricht die Ziel-IP-Adresse einem virtuellen Computer in einem anderen virtuellen Netzwerk. Zwischen den beiden virtuellen Netzwerken liegt ein Gateway für virtuelle Netzwerke.

Falls noch nicht geschehen, installieren und konfigurieren Sie die neueste [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an. Führen Sie dann den folgenden Befehl aus:

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
Wenn der virtuelle Computer über mehrere NICs verfügt und die IP-Weiterleitung für mindestens eine der NICs aktiviert ist, muss der NIC-Parameter (-i nic-id) angegeben werden. Andernfalls ist die Angabe optional.

## <a name="review-results"></a>Überprüfen der Ergebnisse

Nach Abschluss des Vorgangs werden die Ergebnisse bereitgestellt. Die IP-Adresse des nächsten Hops sowie der Typ der entsprechenden Ressource werden zurückgegeben.

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
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

