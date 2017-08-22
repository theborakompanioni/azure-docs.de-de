---
title: "Ältere SKUs für virtuelle Azure-Netzwerkgateways | Microsoft-Dokumentation"
description: "Ältere SKUs für virtuelle Netzwerkgateways."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 3b2126b1ecd1613950bbf311ae08fafd4af0d51f
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Arbeiten mit SKUs für virtuelle Netzwerkgateways (Legacy-SKUs)

Dieser Artikel enthält Informationen über die Legacy-SKUs (alte SKUs) für virtuelle Netzwerkgateways. Die Legacy-SKUs sind weiterhin in beiden Bereitstellungsmodellen für VPN-Gateways funktionsfähig, die bereits erstellt wurden. Klassische VPN-Gateways verwenden weiterhin die Legacy-SKUs für vorhandene Gateways und für neue Gateways. Wenn Sie neue Resource Manager-VPN-Gateways erstellen, verwenden Sie die neuen Gateway-SKUs. Informationen zu den neuen SKUs finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Gateway-SKUs

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Voraussichtlicher aggregierter Durchsatz nach SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Unterstützte Konfigurationen nach SKU und VPN-Typ

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Ändern der Größe eines Gateways (Ändern einer Gateway-SKU)

Sie können die Größe einer Gateway-SKU innerhalb einer SKU-Familie ändern. Wenn Sie z.B. eine Standard-SKU haben, können Sie die Größe in eine HighPerformance-SKU ändern. Die Größe Ihrer VPN-Gateways kann zwischen den alten SKUs und den neuen SKU-Familien nicht geändert werden. Beispielsweise können Sie nicht von einer Standard-SKU zu einer VpnGw2-SKU wechseln. 

Um die Größe einer Gateway-SKU für das klassische Bereitstellungsmodell zu ändern, verwenden Sie den folgenden Befehl:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Um die Größe einer Gateway-SKU für das Resource Manager-Bereitstellungsmodell zu ändern, verwenden Sie den folgenden Befehl:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migrieren zu den neuen Gateway-SKUs

Wenn Sie mit dem Resource Manager-Bereitstellungsmodell arbeiten, können Sie zu den neuen Gateway-SKUs migrieren. Bei Verwendung des klassischen Bereitstellungsmodells können Sie nicht zu den neuen SKUs migrieren und müssen stattdessen weiterhin die Legacy-SKUs verwenden.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den neuen Gateway-SKUs finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpngateways.md#gwsku).

Weitere Informationen zu Konfigurationseinstellungen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md).
