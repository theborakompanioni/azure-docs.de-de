---
title: "Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke | Microsoft Docs"
description: "Informieren Sie sich über Gateways für virtuelle Netzwerke für ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/01/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a575929f2c42a00b6e6cacd86318253d20b6b51e


---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Informationen zu Gateways für virtuelle Netzwerke für ExpressRoute
Ein Gateway für virtuelle Netzwerke dient zum Senden von Netzwerkdatenverkehr zwischen virtuellen Azure-Netzwerken und lokalen Standorten. Wenn Sie eine ExpressRoute-Verbindung konfigurieren, müssen Sie ein Gateway für virtuelle Netzwerke und eine dazugehörige Verbindung erstellen und konfigurieren.

Beim Erstellen eines Gateways für virtuelle Netzwerke werden mehrere Einstellungen angegeben. Eine dieser erforderlichen Einstellungen gibt an, ob das Gateway für ExpressRoute- oder für Standort-zu-Standort-VPN-Datenverkehr verwendet wird. Beim Resource Manager-Bereitstellungsmodell lautet die Einstellung „-GatewayType“.

Verwenden Sie den Gatewaytyp „ExpressRoute“, wenn Netzwerkdatenverkehr über eine dedizierte private Verbindung gesendet wird. Dies wird auch als ExpressRoute-Gateway bezeichnet. Wenn Netzwerkdatenverkehr verschlüsselt und über das öffentliche Internet gesendet wird, verwenden Sie den Gatewaytyp „Vpn“. Dies wird als VPN Gateway bezeichnet. Bei Site-to-Site-, Point-to-Site- und VNet-zu-VNet-Verbindungen wird jeweils VPN Gateway verwendet. 

Ein virtuelles Netzwerk kann pro Gatewaytyp immer nur über ein einzelnes virtuelles Netzwerkgateway verfügen. So können Sie beispielsweise ein virtuelles Netzwerkgateway mit „-GatewayType Vpn“ und ein virtuelles Netzwerkgateway mit „-GatewayType ExpressRoute“ verwenden. In diesem Artikel steht das ExpressRoute-Gateway für virtuelle Netzwerke im Mittelpunkt.

## <a name="a-namegwskuagateway-skus"></a><a name="gwsku"></a>Gateway-SKUs
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Wenn Sie das Gateway auf eine leistungsfähigere Gateway-SKU aktualisieren möchten, können Sie in den meisten Fällen das PowerShell-Cmdlet „Resize-AzureRmVirtualNetworkGateway“ verwenden. Mit diesem können Sie Upgrades von Standard- auf HighPerformance-SKUs durchführen. Für ein Upgrade auf die UltraPerformance-SKU müssen Sie jedoch das Gateway neu erstellen.

### <a name="a-nameaggthroughputaestimated-aggregate-throughput-by-gateway-sku"></a><a name="aggthroughput"></a>Voraussichtlicher aggregierter Durchsatz nach Gateway-SKU
In der folgenden Tabelle sind die Gatewaytypen und der geschätzte zusammengefasste Durchsatz angegeben. Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

## <a name="a-nameresourcesarest-apis-and-powershell-cmdlets"></a><a name="resources"></a>REST-APIs und PowerShell-Cmdlets
Zusätzliche technische Ressourcen und spezielle Syntaxanforderungen bei der Verwendung von REST-APIs und PowerShell-Cmdlets für Gatewaykonfigurationen für virtuelle Netzwerke finden Sie auf den folgenden Seiten:

| **Klassisch** | **Ressourcen-Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST-API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST-API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu verfügbaren Verbindungskonfigurationen finden Sie in der [ExpressRoute-Übersicht](expressroute-introduction.md) . 




<!--HONumber=Nov16_HO3-->


