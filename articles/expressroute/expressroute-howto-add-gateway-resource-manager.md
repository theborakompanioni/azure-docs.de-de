---
title: "Hinzufügen eines VNet-Gateways zu einem virtuellen Netzwerk für ExpressRoute mit Resource Manager und PowerShell | Microsoft Docs"
description: "In diesem Artikel wird beschrieben, wie Sie einem bereits erstellten Resource Manager-VNet für ExpressRoute ein VNet-Gateway hinzufügen."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: charwen
translationtype: Human Translation
ms.sourcegitcommit: 2a0db85dc5699b362aa6a920cc26c40bdfdfc28d
ms.openlocfilehash: 80ffdd66a02f84b844b660e6c88586d4e99ff9e7
ms.lasthandoff: 12/14/2016


---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Konfigurieren eines virtuellen Netzwerkgateways für ExpressRoute mit Resource Manager und PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassisch – PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Dieser Artikel führt Sie durch die Schritte, die zum Hinzufügen, Ändern der Größe und Entfernen eines virtuellen Netzwerkgateways (VNet) für ein vorhandenes VNet erforderlich sind. Die Schritte für diese Konfiguration gelten speziell für VNets, die unter Verwendung des **Resource Manager-Bereitstellungsmodells** erstellt wurden und in einer ExpressRoute-Konfiguration verwendet werden. 

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Vorbereitungen
Stellen Sie sicher, dass Sie die für diese Konfiguration erforderlichen Azure PowerShell-Cmdlets installiert haben (1.0.2 oder höher). Wenn Sie die Cmdlets noch nicht installiert haben, müssen Sie dies tun, bevor Sie mit der Konfiguration beginnen. Weitere Informationen zum Installieren von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das VNet-Gateway erstellt haben, können Sie Ihr VNet mit einer ExpressRoute-Verbindung verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md).


