<properties 
   pageTitle="Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung über das Resource Manager-Bereitstellungsmodell und das Azure-Portal | Microsoft Azure"
   description="In diesem Dokument erhalten Sie einen Überblick über das Verknüpfen von virtuellen Netzwerken (VNets) mit ExpressRoute-Verbindungen."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/14/2016"
   ms.author="cherylmc" />

# Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung

> [AZURE.SELECTOR]
- [Azure-Portal – Resource Manager](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell – Resource Manager](expressroute-howto-linkvnet-arm.md)
- [PowerShell – klassisch](expressroute-howto-linkvnet-classic.md)



Dieser Artikel unterstützt Sie beim Verknüpfen virtueller Netzwerke (VNets) mit ExpressRoute-Verbindungen über das Resource Manager-Bereitstellungsmodell und das Azure-Portal. Virtuelle Netzwerke können Teil desselben Abonnements sein oder zu einem anderen Abonnement gehören.


**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Konfigurationsvoraussetzungen

- Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
- Sie benötigen eine aktive ExpressRoute-Verbindung. 
	- Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren. 
	
	- Stellen Sie sicher, dass privates Azure-Peering für die Verbindung konfiguriert ist. Informationen zum Routing finden Sie unter [Konfigurieren des Routings](expressroute-howto-routing-portal-resource-manager.md).
	
	- Das private Azure-Peering muss konfiguriert sein, und das BGP-Peering zwischen Ihrem Netzwerk und Microsoft muss aktiv sein, damit End-to-End-Konnektivität bereitgestellt werden kann.
	
	- Sie müssen ein virtuelles Netzwerk und ein virtuelles Netzwerkgateway erstellt und vollständig bereitgestellt haben. Befolgen Sie die Anweisungen, um ein [VPN-Gateway](../articles/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) zu erstellen. (Befolgen Sie nur die Schritte 1 bis 5.)

Sie können bis zu 10 virtuelle Netzwerke mit einer ExpressRoute-Verbindung verknüpfen. Alle ExpressRoute-Verbindungen müssen sich in derselben geopolitischen Region befinden. Wenn Sie das ExpressRoute Premium-Add-On aktiviert haben, können Sie eine größere Anzahl von virtuellen Netzwerken mit der ExpressRoute-Verbindung verknüpfen. Weitere Informationen zum Premium-Add-On finden Sie in den [häufig gestellten Fragen](expressroute-faqs.md).

## Herstellen einer Verbindung zwischen einem VNet in demselben Abonnement und einer Verbindung


### So stellen Sie eine Verbindung her

1. Stellen Sie sicher, dass Ihre ExpressRoute-Verbindung und das private Azure-Peering erfolgreich konfiguriert wurden. Befolgen Sie die Anweisungen in den Artikeln zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und zum [Konfigurieren des Routings](expressroute-howto-routing-arm.md). Ihre ExpressRoute-Verbindung sollte in etwa wie in der folgenden Abbildung dargestellt aussehen.

	![](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)

	>[AZURE.NOTE] BGP-Konfigurationsinformationen werden nicht angezeigt, wenn Ihre Peerings vom Layer 3-Anbieter konfiguriert wurden. Wenn Ihre Verbindung sich im bereitgestellten Zustand befindet, sollten Sie Verbindungen erstellen können.

2. Jetzt können Sie damit beginnen, eine Verbindung zum Verknüpfen des VNet-Gateways mit Ihrer ExpressRoute-Verbindung bereitzustellen. Klicken Sie auf **Verbindung** **>** **Hinzufügen**, um das Blatt **Verbindung hinzufügen** zu öffnen, und konfigurieren Sie dann die Werte. Informationen hierzu finden Sie im unten angegebenen Referenzbeispiel.
	

	![](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)
 
	
3. Nachdem die Verbindung erfolgreich konfiguriert wurde, zeigt das Verbindungsobjekt die Daten für die Verbindung an.

	![](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)


### So löschen Sie eine Verbindung

Sie können eine Verbindung löschen, indem Sie das Symbol **Löschen** auf dem Blatt für Ihre Verbindung auswählen.

## Herstellen einer Verbindung zwischen einem VNet in einem anderen Abonnement und einer Verbindung

Zu diesem Zeitpunkt können Sie virtuelle Netzwerke in verschiedenen Abonnements nicht über das Azure-Portal verbinden. Allerdings können Sie PowerShell dazu verwenden. Weitere Informationen hierzu finden Sie im [PowerShell](expressroute-howto-linkvnet-arm.md)-Artikel.

## Nächste Schritte

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).

<!---HONumber=AcomDC_0420_2016-->