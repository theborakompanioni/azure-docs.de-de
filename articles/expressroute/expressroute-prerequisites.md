<properties
   pageTitle="Voraussetzungen für den Wechsel zu ExpressRoute | Microsoft Azure"
   description="Diese Seite enthält eine Liste der Anforderungen, die erfüllt sein müssen, bevor Sie eine Azure ExpressRoute-Verbindung anfordern können."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/13/2016"
   ms.author="cherylmc"/>


# Voraussetzungen und Checkliste für ExpressRoute  

Zum Herstellen einer Verbindung mit den Microsoft-Clouddiensten über ExpressRoute müssen Sie sicherstellen, dass die in den Abschnitten unten genannten Voraussetzungen erfüllt sind.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## Azure-Konto

- Ein gültiges und aktives Microsoft Azure-Konto. Dies ist erforderlich, um die ExpressRoute-Verbindung einzurichten. Bei ExpressRoute-Verbindungen handelt es sich um Ressourcen innerhalb von Azure-Abonnements. Ein Azure-Abonnement ist eine Anforderung, auch wenn die Konnektivität auf Microsoft-Clouddienste (keine Azure-Dienste) wie z. B. Office 365-Dienste und CRM online beschränkt ist.
- Ein aktives Office 365-Abonnement (bei Nutzung von Office 365-Diensten). Weitere Informationen finden Sie im Abschnitt [Besondere Vorgaben für Office 365](#office-365-specific-requirements) in diesem Artikel.

## Konnektivitätsanbieter
- Sie können mit einem [ExpressRoute-Konnektivitätspartner](expressroute-locations.md#partners) zusammenarbeiten, um eine Verbindung mit der Microsoft Cloud herzustellen. Zum Einrichten einer Verbindung zwischen Ihrem lokalen Netzwerk und Microsoft stehen Ihnen [drei Möglichkeiten](expressroute-introduction.md#howtoconnect) zur Verfügung. 
- Wenn Ihr Anbieter kein ExpressRoute-Konnektivitätspartner ist, können Sie die Verbindung zur Microsoft Cloud über einen [Cloud-Exchange-Anbieter](expressroute-locations.md#nonpartners) herstellen.

## Netzwerkanforderungen
- **Redundante Konnektivität**: Eine redundante physische Verbindung zwischen Ihnen und Ihrem Anbieter ist nicht erforderlich. Für Microsoft ist es aber erforderlich, dass zwischen den Routern von Microsoft und den Peering-Routern redundante BGP-Sitzungen eingerichtet werden, auch wenn [zum Cloud-Exchange nur eine physische Verbindung besteht](expressroute-faqs.md#onep2plink). 
- **Routing**: Je nachdem, wie Sie die Verbindung zur Microsoft Cloud herstellen, müssen Sie oder Ihr Anbieter die BGP-Sitzungen für [Routingdomänen](expressroute-circuit-peerings.md) einrichten und verwalten. Einige Ethernet-Konnektivitätsanbieter oder Cloud-Exchange-Anbieter bieten BGP-Verwaltung als Dienst an, der einen Mehrwert schafft.
- **NAT**: Microsoft akzeptiert nur öffentliche IP-Adressen über Microsoft-Peering. Wenn Sie private IP-Adressen in Ihrem lokalen Netzwerk verwenden, müssen Sie oder Ihr Anbieter die privaten IP-Adressen [mithilfe der NAT](expressroute-nat.md) in öffentliche IP-Adressen übersetzen.
- **QoS**: Skype for Business umfasst verschiedene Dienste (Sprachanrufe, Videoanrufe, SMS) mit jeweils unterschiedlichen QoS-Anforderungen. Sie und Ihr Anbieter müssen daher die [QoS-Anforderungen](expressroute-qos.md) berücksichtigen.
- **Netzwerksicherheit**: Berücksichtigen Sie die [Netzwerksicherheit](../best-practices-network-security.md) beim Verbinden mit der Microsoft Cloud über ExpressRoute.
 
## Office 365

Wenn Sie Office 365 auf ExpressRoute aktivieren möchten, finden Sie in den folgenden Dokumenten weitere Informationen zu den Anforderungen für Office 365.


- [Overview of ExpressRoute for Office 365 (Überblick über ExpressRoute für Office 365)](https://support.office.com/de-DE/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Verwalten von Office 365-Netzwerkdatenverkehr](https://support.office.com/de-DE/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [URLs und IP-Adressbereiche von Office 365](https://support.office.com/de-DE/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Planen des Netzwerks und Leistungsoptimierung für Office 365](https://support.office.com/de-DE/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Netzwerk- und Migrationsplanung für Office 365](https://support.office.com/de-DE/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365-Integration in lokale Umgebungen](https://support.office.com/de-DE/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## CRM Online 
Wenn Sie CRM Online auf ExpressRoute aktivieren möchten, finden Sie in den folgenden Dokumenten weitere Informationen zu den Anforderungen für CRM Online.

- [CRM Online URLs](https://support.microsoft.com/kb/2655102) (CRM-Online-URLs) und [IP address ranges](https://support.microsoft.com/kb/2728473) (IP-Adressbereiche)

## Nächste Schritte

- Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
- Suchen Sie einen ExpressRoute-Konnektivitätsanbieter. Siehe [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md).
- Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [Dienstqualität (QoS)](expressroute-qos.md) an.
- Konfigurieren Sie Ihre ExpressRoute-Verbindung.
	- [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
	- [Konfigurieren des Routings](expressroute-howto-routing-classic.md)
	- [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)

<!---HONumber=AcomDC_0615_2016-->