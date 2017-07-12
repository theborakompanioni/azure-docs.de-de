---
title: "Voraussetzungen für die Einführung von Azure ExpressRoute | Microsoft-Dokumentation"
description: "Diese Seite enthält eine Liste der Anforderungen, die erfüllt sein müssen, bevor Sie eine Azure ExpressRoute-Verbindung anfordern können."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 8629235511e0dda149ceef6a9c834c3042f64f90
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
<a id="expressroute-prerequisites--checklist" class="xliff"></a>

# Voraussetzungen und Checkliste für ExpressRoute
Zum Herstellen einer Verbindung mit den Microsoft-Clouddiensten über ExpressRoute müssen Sie sicherstellen, dass die unten in den Abschnitten genannten Voraussetzungen erfüllt sind.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

<a id="azure-account" class="xliff"></a>

## Azure-Konto
* Ein gültiges und aktives Microsoft Azure-Konto. Dieses Konto ist zum Einrichten der ExpressRoute-Verbindung erforderlich. Bei ExpressRoute-Verbindungen handelt es sich um Ressourcen in Azure-Abonnements. Ein Azure-Abonnement ist Voraussetzung, auch wenn die Konnektivität auf andere Microsoft-Clouddienste (nicht Azure) wie Office 365 und Dynamics 365 beschränkt ist.
* Ein aktives Office 365-Abonnement (bei Nutzung von Office 365-Diensten). Weitere Informationen finden Sie im Abschnitt [Besondere Vorgaben für Office 365](#office-365-specific-requirements) dieses Artikels.

<a id="connectivity-provider" class="xliff"></a>

## Konnektivitätsanbieter

* Sie können mit einem [ExpressRoute-Konnektivitätspartner](expressroute-locations.md#partners) zusammenarbeiten, um eine Verbindung mit der Microsoft Cloud herzustellen. Zum Einrichten einer Verbindung zwischen Ihrem lokalen Netzwerk und Microsoft stehen Ihnen [drei Möglichkeiten](expressroute-introduction.md)zur Verfügung.
* Wenn Ihr Anbieter kein ExpressRoute-Konnektivitätspartner ist, können Sie die Verbindung zur Microsoft Cloud über einen [Cloud-Exchange-Anbieter](expressroute-locations.md#connectivity-through-exchange-providers)herstellen.

<a id="network-requirements" class="xliff"></a>

## Netzwerkanforderungen
* **Redundante Konnektivität**: Eine redundante physische Verbindung zwischen Ihnen und Ihrem Anbieter ist nicht erforderlich. Für Microsoft ist es aber erforderlich, dass zwischen den Routern von Microsoft und den Peering-Routern redundante BGP-Sitzungen eingerichtet werden, auch wenn [zum Cloud-Exchange nur eine physische Verbindung besteht](expressroute-faqs.md#onep2plink).
* **Routing**: Je nachdem, wie Sie die Verbindung zur Microsoft Cloud herstellen, müssen Sie oder Ihr Anbieter die BGP-Sitzungen für [Routingdomänen](expressroute-circuit-peerings.md)einrichten und verwalten. Einige Ethernet-Konnektivitätsanbieter oder Cloud-Exchange-Anbieter bieten BGP-Verwaltung als Dienst an, der einen Mehrwert schafft.
* **NAT**: Microsoft akzeptiert nur öffentliche IP-Adressen über Microsoft-Peering. Wenn Sie private IP-Adressen in Ihrem lokalen Netzwerk verwenden, müssen Sie oder Ihr Anbieter die privaten IP-Adressen [mithilfe der NAT](expressroute-nat.md)in öffentliche IP-Adressen übersetzen.
* **QoS**: Skype for Business umfasst verschiedene Dienste (z.B. Sprachanrufe, Videoanrufe, SMS) mit jeweils unterschiedlichen QoS-Anforderungen. Sie und Ihr Anbieter müssen daher die [QoS-Anforderungen](expressroute-qos.md)berücksichtigen.
* **Netzwerksicherheit**: Berücksichtigen Sie die [Netzwerksicherheit](../best-practices-network-security.md) beim Herstellen der Verbindung mit der Microsoft Cloud über ExpressRoute.

<a id="office-365" class="xliff"></a>

## Office 365
Wenn Sie Office 365 auf ExpressRoute aktivieren möchten, helfen Ihnen die Informationen in den folgenden Dokumenten zu den Anforderungen für Office 365 weiter.

* [Overview of ExpressRoute for Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd) (Überblick über ExpressRoute für Office 365)
* [Routing with ExpressRoute for Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408) (Routing mit ExpressRoute für Office 365)
* [URLs und IP-Adressbereiche von Office 365](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Netzwerkplanung und Leistungsoptimierung für Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Network bandwidth calculators and tools](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132) (Netzwerk- und Migrationsplanung für Office 365)
* [Office 365-Integration in lokale Umgebungen](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [ExpressRoute unter Office 365 – Schulungsvideos für Fortgeschrittene](https://channel9.msdn.com/series/aer/)

<a id="dynamics-365" class="xliff"></a>

## Dynamics 365
Wenn Sie Dynamics 365 für ExpressRoute aktivieren möchten, helfen Ihnen die Informationen in den folgenden Dokumenten zu den Anforderungen für Dynamics 365 weiter.

* [Whitepaper: Dynamics 365 und ExpressRoute](http://download.microsoft.com/download/B/2/8/B2896B38-9832-417B-9836-9EF240C0A212/Microsoft%20Dynamics%20365%20and%20ExpressRoute.pdf)
* [URLs](https://support.microsoft.com/kb/2655102) und [IP-Adressbereiche von Dynamics 365](https://support.microsoft.com/kb/2728473)

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Suchen Sie einen ExpressRoute-Konnektivitätsanbieter. Siehe [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md).
* Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [QoS](expressroute-qos.md) an.
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  * [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-classic.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)

