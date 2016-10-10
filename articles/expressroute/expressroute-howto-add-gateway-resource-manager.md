<properties
   pageTitle="Hinzufügen eines VNet-Gateways zu einem virtuellen Netzwerk für ExpressRoute mit Resource Manager und PowerShell | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Sie einem bereits erstellten Resource Manager-VNet für ExpressRoute ein VNet-Gateway hinzufügen."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/22/2016"
   ms.author="charwen"/>

# Konfigurieren eines virtuellen Netzwerkgateways für ExpressRoute mit Resource Manager und PowerShell


> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell – klassisch](expressroute-howto-add-gateway-classic.md)


Dieser Artikel führt Sie durch die Schritte, die zum Hinzufügen, Ändern der Größe und Entfernen eines virtuellen Netzwerkgateways (VNet) für ein vorhandenes VNet erforderlich sind. Die Schritte für diese Konfiguration gelten speziell für VNets, die unter Verwendung des **Resource Manager-Bereitstellungsmodells** erstellt wurden und in einer ExpressRoute-Konfiguration verwendet werden.

**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Vorbereitungen

Stellen Sie sicher, dass Sie die für diese Konfiguration erforderlichen Azure PowerShell-Cmdlets installiert haben (1.0.2 oder höher). Wenn Sie die Cmdlets noch nicht installiert haben, müssen Sie dies tun, bevor Sie mit der Konfiguration beginnen. Weitere Informationen zum Installieren von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

	
## Nächste Schritte

Nachdem Sie das VNet-Gateway erstellt haben, können Sie Ihr VNet mit einer ExpressRoute-Verbindung verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md).

<!---HONumber=AcomDC_0928_2016-->