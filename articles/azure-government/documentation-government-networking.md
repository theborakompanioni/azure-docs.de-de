---
title: Azure Government-Netzwerk | Microsoft Docs
description: "Dieser Artikel enthält einen Vergleich der Features sowie Anleitungen für private Verbindungen mit Azure Government."
services: Azure-Government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
editor: 
ms.assetid: 3da70579-ecda-421a-8ebf-d52906334e9b
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 09/28/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: 14aa9126d3426c560c1f9497ed0d7a92448137b4
ms.openlocfilehash: 39e67d449c0fa37b76354dc522216f9473df7b97


---
# <a name="azure-government-networking"></a>Azure Government-Netzwerk
## <a name="expressroute-private-connectivity"></a>ExpressRoute (private Verbindung)
ExpressRoute ist allgemein in Azure Government verfügbar. Weitere Informationen (u.a. zu Partnern und Peeringstandorten) finden Sie in der [öffentlichen Dokumentation zu ExpressRoute](../expressroute/index.md).

### <a name="variations"></a>Variationen
ExpressRoute ist in Azure Government allgemein verfügbar. 

* Government-Kunden nutzen physisch isolierte Kapazität über eine dedizierte Azure Government-ExpressRoute-Leitung.
* Azure Government (Gov) bietet höhere Verfügbarkeit und Stabilität, indem mehrere Regionspaare genutzt werden, die mindestens 800 Kilometer voneinander entfernt sind. 
* Die Azure Gov-ExpressRoute-Konnektivität wird standardmäßig mit Aktiv/Aktiv-Redundanz und Unterstützung von Bursting und einer Übertragungsrate von bis zu 10 GBit/s bereitgestellt (der Mindestwert ist 50 MBit/s).
* Azure Gov-ExpressRoute-Standorte bieten optimierte Pfade (kürzeste Hops, niedrigste Latenz, höchste Leistung usw.) für Kunden und geografisch redundante Azure Gov-Regionen.
* Die private Azure Gov-ExpressRoute-Verbindung nutzt das Internet nicht und ist nicht vom Internet abhängig.
* Die physische und logische Azure Gov-Infrastruktur ist physisch dediziert und getrennt, und der Zugriff auf die Infrastruktur ist auf US-Bürger beschränkt.
* Microsoft besitzt und betreibt die gesamte Fiberinfrastruktur zwischen Azure Gov-Regionen und Azure Gov-ExpressRoute-Meet-Me-Standorten.
* Azure Gov-ExpressRoute bietet Konnektivität mit Microsoft Azure, O365 und CRM-Clouddiensten.

### <a name="considerations"></a>Überlegungen
Es gibt zwei grundlegende Dienste, die private Netzwerkverbindungen in Azure Government bereitstellen: VPN (Site-to-Site für eine typische Organisation) und ExpressRoute.

Azure ExpressRoute wird zum Herstellen privater Verbindungen zwischen Azure Government-Datencentern und einer Infrastruktur verwendet, die sich an Ihrem Standort oder in einer Kollokationsumgebung befindet. ExpressRoute-Verbindungen werden nicht über das öffentliche Internet hergestellt. Sie bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit und weniger Latenz als herkömmliche Internetverbindungen. In bestimmten Fällen lassen sich durch die Verwendung von ExpressRoute-Verbindungen zum Übertragen von Daten zwischen lokalen Systemen und Azure erhebliche Kosteneinsparungen erzielen.   

Mit ExpressRoute stellen Sie Verbindungen mit Azure an einem ExpressRoute-Standort (Exchange-Anbietereinrichtung) her oder verbinden sich direkt über Ihr vorhandenes WAN (z.B. ein MPLS-VPN), das von einem Netzwerkdienstanbieter bereitgestellt wird.

![alt text](./media/azure-government-capability-private-connectivity-options.PNG)  ![alt text](./media/government-capability-expressroute.PNG)  

Damit Netzwerkdienste Azure Government-Kundenanwendungen und -Lösungen unterstützen, wird dringend empfohlen, ExpressRoute (private Verbindung) für das Herstellen einer Verbindung mit Azure Government zu implementieren. Bei der Verwendung von VPN-Verbindungen sollte Folgendes berücksichtigt werden:

* Kunden müssen sich an die autorisierende Person/Agentur wenden, um zu erfragen, ob eine private Verbindung oder ein anderer sicherer Verbindungsmechanismus erforderlich ist, und weitere zu berücksichtigende Einschränkungen zu identifizieren.
* Kunden müssen entscheiden, ob das Site-to-Site-VPN über eine Zone für private Verbindungen weitergeleitet werden soll.
* Kunden müssen eine MPLS-Verbindung oder ein VPN von einem lizenzierten Dienstanbieter für private Verbindungen anfordern.

Alle Kunden, die eine Architektur mit privaten Verbindungen nutzen, müssen überprüfen, ob für die Kundenverbindung mit dem GN/I-Edgerouter-Abgrenzungspunkt (Gateway Network/Internet) für Azure Government eine entsprechende Implementierung durchgeführt und beibehalten wird. Dementsprechend muss Ihre Organisation die Netzwerkverbindung zwischen Ihrer lokalen Umgebung und dem GN/C-Edgerouter-Abgrenzungspunkt (Gateway Network/Customer) für Azure Government einrichten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.




<!--HONumber=Nov16_HO3-->


