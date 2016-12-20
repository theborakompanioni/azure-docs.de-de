---
title: "Übersicht über internen Lastenausgleich | Microsoft Docs"
description: "Übersicht über den internen Lastenausgleich und seine Funktionen. Funktionsweise eines Lastenausgleichs für Azure und mögliche Szenarios zum Konfigurieren interner Endpunkte"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 177b01989ab2f3475cd853d89789b88e02d04be4
ms.openlocfilehash: 0194ffe54462e1a432ea10e69608e158f9cc54dc

---

# <a name="internal-load-balancer-overview"></a>Interner Lastenausgleich (Übersicht)

Im Gegensatz zum Lastenausgleichsmodul mit Internetzugriff leitet das interne Lastenausgleichsmodul (Internal Load Balancer, ILB) Datenverkehr nur an Ressourcen innerhalb des Clouddiensts oder an Ressourcen weiter, die per VPN auf die Azure-Infrastruktur zugreifen. Die Infrastruktur schränkt den Zugriff auf die virtuellen IP-Adressen mit Lastenausgleich eines Clouddiensts oder eines virtuellen Netzwerks ein, sodass diese nie direkt für einen Internetendpunkt verfügbar gemacht werden. Dies ermöglicht die Ausführung interner Branchenanwendungen (LOB-Anwendungen) in Azure und den Zugriff auf diese Anwendungen in der Cloud oder über lokale Ressourcen.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Szenarien, in denen ggf. die Verwendung eines internen Lastenausgleichsmoduls erforderlich ist

Der interne Azure-Lastenausgleich (ILB) ermöglicht Lastenausgleich zwischen virtuellen Computern in einem Clouddienst oder virtuellen Netzwerk mit regionalem Umfang. Informationen zur Verwendung und Konfiguration virtueller Netzwerke mit regionalem Umfang finden Sie unter [Regionale virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) im Azure-Blog. Vorhandene virtuelle Netzwerke, die für eine Affinitätsgruppe konfiguriert wurden, können kein ILB verwenden.

ILB ermöglicht die folgenden Arten des Lastenausgleichs:

* Innerhalb eines Clouddiensts von virtuellen Computern zu einer Gruppe von virtuellen Computern, die sich im selben Clouddienst befinden (siehe Abbildung 1).
* In einem virtuellen Netzwerk von virtuellen Computern im virtuellen Netzwerk zu einer Gruppe von virtuellen Computern im selben Clouddienst des virtuellen Netzwerks (siehe Abbildung 2).
* In einem standortübergreifenden virtuellen Netzwerk von lokalen Computern zu einer Gruppe von virtuellen Computern im selben Clouddienst des virtuellen Netzwerks (siehe Abbildung 3).
* Internetanwendungen mit mehreren Ebenen, bei denen die Back-End-Ebenen keine Internetanbindung haben, jedoch Lastenausgleich für Datenverkehr für die mit dem Internet verbundene Ebene erfordern.
* Lastenausgleich für Branchenanwendungen, die in Azure gehostet werden, ohne dass zusätzliche Hardware oder Software für den Lastenausgleich erforderlich ist. Einbeziehen von lokalen Servern in die Gruppe der Computer, für deren Datenverkehr Lastenausgleich stattfindet.

## <a name="internet-facing-multi-tier-applications"></a>Anwendungen mit mehreren Ebenen mit Internetanbindung

Die Webebene verfügt über Endpunkte mit Internetanbindung für Internetclients und ist Teil einer Gruppe mit Lastenausgleich. Der Lastenausgleich verteilt den eingehenden Datenverkehr von Webclients für den TCP-Port 443 (HTTPS) an den Webserver.

Die Datenbankserver befinden sich hinter einem ILB-Endpunkt, den die Webserver zur Speicherung verwenden. Dieser Datenbankdienst-Endpunkt mit Lastenausgleich, für dessen Datenverkehr Lastenausgleich auf den Datenbankservern in der ILB-Gruppe ausgeführt wird.

In der folgenden Abbildung ist die Anwendung mit mehreren Ebenen und Internetzugriff im gleichen Clouddienst dargestellt.

![Interner Lastenausgleich eines einzelnen Clouddiensts](./media/load-balancer-internal-overview/IC736321.png)

Abbildung 1: Anwendung mit mehreren Ebenen mit Internetzugriff

Ein anderes mögliches Szenario für eine Anwendung mit mehreren Ebenen ist eine Bereitstellung des ILB in einem anderen Clouddienst als dem, der für den ILB-Dienst verwendet wird.

Clouddienste mit im gleichen virtuellen Netzwerk haben Zugriff auf den ILB-Endpunkt. In der folgenden Abbildung ist zu sehen, dass sich die Front-End-Webserver in einem anderen Clouddienst befinden als das Datenbank-Back-End und dass sie einen ILB-Endpunkt im gleichen virtuellen Netzwerk verwenden.

![Interner Lastenausgleich zwischen Clouddiensten](./media/load-balancer-internal-overview/IC744147.png)

Abbildung 2: Front-End-Server in einem anderen Clouddienst

## <a name="intranet-line-of-business-applications"></a>Intranet-Branchenanwendungen

Für Datenverkehr von Clients im lokalen Netzwerk wird in einer Gruppe von LOB-Servern mithilfe der VPN-Verbindung mit dem Azure-Netzwerk Lastenausgleich ausgeführt.

Der Clientcomputer hat Zugriff auf eine IP-Adresse des Azure-VPN-Diensts über ein Point-to-Site-VPN. Er ermöglicht die Verwendung der Branchenanwendung, die hinter dem ILB-Endpunkt gehostet wird.

![Interner Lastenausgleich mit Punkt-zu-Standort-VPN](./media/load-balancer-internal-overview/IC744148.png)

Abbildung 3: Hinter dem LB-Endpunkt gehostete Branchenanwendungen

Ein weiteres Szenario für Branchenanwendungen ist eine Site-to-Site-VPN-Verbindung mit dem virtuellen Netzwerk, in dem der ILB-Endpunkt konfiguriert ist. Dadurch kann lokaler Netzwerkdatenverkehr an den ILB-Endpunkt weitergeleitet werden.

![Interner Lastenausgleich mit Standort-zu-Standort-VPN](./media/load-balancer-internal-overview/IC744150.png)

Abbildung 4: An den ILB-Endpunkt weitergeleiteter lokaler Netzwerkdatenverkehr

## <a name="next-steps"></a>Nächste Schritte

[Unterstützung von Azure Resource Manager für Azure Load Balancer](load-balancer-arm.md)

[Erste Schritte zum Konfigurieren des Lastenausgleichs für Internetverbindungen](load-balancer-get-started-internet-arm-ps.md)

[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO3-->


