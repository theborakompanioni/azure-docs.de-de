---
title: "Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung: CLI: Azure | Microsoft-Dokumentation"
description: "Dieses Dokument bietet Ihnen eine Übersicht über das Verknüpfen virtueller Netzwerke (VNETs) mit ExpressRoute-Verbindungen über das Resource Manager-Bereitstellungsmodell und die CLI."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 0ea696e796ec3a943bc028f56da417978b728b82
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung mithilfe der CLI

In diesem Artikel können Sie virtuelle Netzwerke (VNETs) mithilfe der CLI mit Azure-ExpressRoute-Verbindungen verknüpfen. Um mithilfe der Azure CLI eine Verknüpfung durchzuführen, müssen die virtuellen Netzwerke mit dem Resource Manager-Bereitstellungsmodell erstellt werden. Sie können Teil desselben Abonnements sein oder zu einem anderen Abonnement gehören. Wenn Sie eine andere Methode für die Verbindung Ihres VNET mit einer ExpressRoute-Verbindung verwenden möchten, können Sie einen Artikel aus der folgenden Liste auswählen:

> [!div class="op_single_selector"]
> * [Azure-Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassisch)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen

* Sie benötigen die neueste Version der Befehlszeilenschnittstelle (CLI). Weitere Informationen finden Sie unter [Installieren der Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Sie benötigen eine aktive ExpressRoute-Verbindung. 
  * Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](howto-circuit-cli.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren. 
  * Stellen Sie sicher, dass privates Azure-Peering für die Verbindung konfiguriert ist. Informationen zum Routing finden Sie unter [Konfigurieren des Routings](howto-routing-cli.md) . 
  * Stellen Sie sicher, dass das private Azure-Peering konfiguriert ist. Das BGP-Peering zwischen Ihrem Netzwerk und Microsoft muss aktiv sein, damit End-to-End-Konnektivität bereitgestellt werden kann.
  * Vergewissern Sie sich, dass ein virtuelles Netzwerk und ein virtuelles Netzwerkgateway erstellt und vollständig bereitgestellt wurden. Befolgen Sie die Anweisungen zum [Konfigurieren eines virtuellen Netzwerkgateways für ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Achten Sie darauf, `--gateway-type ExpressRoute` zu verwenden.

* Sie können bis zu zehn virtuelle Netzwerke mit einer ExpressRoute-Standardverbindung verknüpfen. Alle virtuellen Netzwerke müssen sich in der gleichen geopolitischen Region befinden, wenn Sie eine ExpressRoute-Standardverbindung verwenden. 

* Wenn Sie das ExpressRoute-Premium-Add-On aktivieren, können Sie ein virtuelles Netzwerk außerhalb der geopolitischen Region der ExpressRoute-Verbindung oder eine größere Anzahl von virtuellen Netzwerken mit der ExpressRoute-Verbindung verknüpfen. Weitere Informationen zum Premium-Add-On finden Sie in den [FAQs](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Herstellen einer Verbindung zwischen einem virtuellen Netzwerk in demselben Abonnement und einer Verbindung

Sie können anhand des folgenden Beispiels ein virtuelles Netzwerkgateway mit einer ExpressRoute-Verbindung verbinden. Stellen Sie sicher, dass das Gateway für das virtuelle Netzwerk erstellt wurde und für das Erstellen von Verknüpfungen bereit ist, bevor Sie den Befehl ausführen.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Herstellen einer Verbindung zwischen einem virtuellen Netzwerk in einem anderen Abonnement und einer Verbindung

Sie können eine ExpressRoute-Verbindung für mehrere Abonnements freigeben. Die Abbildung unten zeigt eine einfache schematische Darstellung der Freigabe von Lasten für ExpressRoute-Verbindungen für mehrere Abonnements.

Jede der kleineren Clouds innerhalb der großen Cloud stellt Abonnements dar, die zu verschiedenen Abteilungen innerhalb einer Organisation gehören. Jede der Abteilungen innerhalb der Organisation kann ihr eigenes Abonnement zum Bereitstellen von Diensten verwenden, für die Verbindung mit dem lokalen Netzwerk kann jedoch eine einzelne gemeinsam genutzte ExpressRoute-Verbindung verwendet werden. Eine einzelne Abteilung (in diesem Beispiel: IT) kann die ExpressRoute-Verbindung besitzen. Andere Abonnements innerhalb der Organisation können die ExpressRoute-Verbindung nutzen.

> [!NOTE]
> Konnektivitäts- und Bandbreitengebühren für die dedizierte Verbindung werden dem Besitzer der ExpressRoute-Verbindung in Rechnung gestellt. Alle virtuellen Netzwerke verwenden gemeinsam dieselbe Bandbreite.
> 
> 

![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Verwaltung – Verbindungsbesitzer und Verbindungsbenutzer

Der „Verbindungsbesitzer“ ist ein autorisierter Poweruser der ExpressRoute-Verbindungsressource. Der Verbindungsbesitzer kann Autorisierungen erstellen, die durch „Verbindungsbenutzer“ eingelöst werden können. Verbindungsbenutzer sind Besitzer virtueller Netzwerkgateways, die sich nicht im selben Abonnement wie die ExpressRoute-Verbindung befinden. Verbindungsbenutzer können Autorisierungen einlösen (eine Autorisierung für jedes virtuelle Netzwerk).

Der Besitzer der Verbindung hat die Möglichkeit, Autorisierungen jederzeit zu ändern und zu widerrufen. Wenn eine Autorisierung widerrufen wird, führt dies dazu, dass alle Verbindungen aus dem Abonnement gelöscht werden, dessen Zugriff aufgehoben wurde.

### <a name="circuit-owner-operations"></a>Aktionen als Verbindungsbesitzer

**So erstellen Sie eine Autorisierung**

Der Verbindungsbesitzer erstellt eine Autorisierung, wodurch ein Autorisierungsschlüssel erstellt wird. Dieser kann von einem Verbindungsbenutzer verwendet werden, um dessen virtuelle Netzwerkgateways mit der ExpressRoute-Verbindung zu verbinden. Eine Autorisierung gilt nur für jeweils eine Verbindung.

Im folgenden Beispiel wird veranschaulicht, wie Sie eine Autorisierung erstellen:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

Die Antwort enthält Schlüssel und Status für die Autorisierung:

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**So überprüfen Sie Autorisierungen**

Mit dem folgenden Beispiel kann der Besitzer einer Verbindung alle für eine bestimmte Verbindung ausgestellten Autorisierungen überprüfen:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**So fügen Sie Autorisierung hinzu**

Mit dem folgenden Beispiel kann der Verbindungsbesitzer Autorisierungen hinzufügen:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**So löschen Sie Autorisierungen**

Mit dem folgenden Beispiel kann der Besitzer einer Verbindung Autorisierungen für einen Benutzer widerrufen oder löschen:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Aktionen als Verbindungsbenutzer

Der Verbindungsbenutzer benötigt die Peer-ID und einen Autorisierungsschlüssel vom Verbindungsbesitzer. Der Autorisierungsschlüssel ist eine GUID.

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**So lösen Sie eine Verbindungsautorisierung ein**

Mit dem folgenden Beispiel können Benutzer einer Verbindung eine Verknüpfungsautorisierung abrufen:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**So geben Sie eine Verbindungsautorisierung frei**

Sie können eine Autorisierung durch das Löschen der Verbindung freigeben, die die ExpressRoute-Verbindung mit dem virtuellen Netzwerk verknüpft.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
