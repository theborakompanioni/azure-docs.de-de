---
title: "Richtlinien für die Azure-Netzwerkinfrastruktur | Microsoft Docs"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung virtueller Netzwerke in Azure-Infrastrukturdiensten."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e2d45973-5eba-4904-8ba0-1821f64feed7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 1e52ae69951b6b1feee6207033a85a583d13bcc2
ms.openlocfilehash: 5db3100d94af1461d538eb1bb9b16b7bf387517a


---
# <a name="azure-networking-infrastructure-guidelines"></a>Richtlinien für die Azure-Netzwerkinfrastruktur
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel werden die erforderlichen Planungsschritte für virtuelle Netzwerke in Azure sowie die Verbindungen zwischen vorhandenen lokalen Umgebungen erläutert.

## <a name="implementation-guidelines-for-virtual-networks"></a>Implementierungsrichtlinien für virtuelle Netzwerke
Entscheidungen:

* Welchen virtuellen Netzwerktyp benötigen Sie, um Ihre IT-Workload oder -Infrastruktur zu hosten (cloudbasiert oder standortübergreifend)?
* Wie groß muss der Adressraum bei standortübergreifenden virtuellen Netzwerken sein, um die Subnetze und virtuellen Computer jetzt und zukünftig bei erwartungsgemäßer Erweiterung zu hosten?
* Werden Sie zentrale virtuelle Netzwerke oder individuelle virtuelle Netzwerke für jede Ressourcengruppe erstellen?

Aufgaben:

* Definieren Sie den Adressraum für die zu erstellenden virtuellen Netzwerke.
* Definieren Sie den Satz von Subnetzen und den Adressraum für die Netzwerke.
* Definieren Sie für standortübergreifende virtuelle Netzwerke den Adressraumsatz des lokalen Netzwerks für die lokalen Speicherorte, die von den virtuellen Computern im virtuellen Netzwerk erreicht werden müssen.
* Arbeiten Sie mit dem lokalen Netzwerkteam zusammen, um sicherzustellen, dass beim Erstellen von standortübergreifenden virtuellen Netzwerken das richtige Routing konfiguriert wird.
* Erstellen Sie das virtuelle Netzwerk mit der Benennungskonvention.

## <a name="virtual-networks"></a>Virtuelle Netzwerke
Virtuelle Netzwerke sind erforderlich, um die Kommunikation zwischen virtuellen Computern zu unterstützen. Sie können Subnetze, benutzerdefinierte IP-Adressen, DNS-Einstellungen, Sicherheitsfilter und Lastenausgleich definieren, genau wie bei physischen Netzwerken. Durch Verwendung eines [VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder einer [ExpressRoute-Verbindung](../expressroute/expressroute-introduction.md) können Sie virtuelle Azure-Netzwerke mit Ihren lokalen Netzwerken verbinden. Hier finden Sie weitere Informationen zu [virtuellen Netzwerken und ihren Komponenten](../virtual-network/virtual-networks-overview.md).

Die Verwendung von Ressourcengruppen bietet Flexibilität beim Entwurf der virtuellen Netzwerkkomponenten. Virtuelle Computer können Verbindungen mit virtuellen Netzwerken außerhalb ihrer eigenen Ressourcengruppe herstellen. Ein gängiger Entwurfsansatz besteht darin, zentralisierte Ressourcengruppen zu erstellen, die Ihre zentrale, von einem gemeinsamen Team verwaltete Netzwerkinfrastruktur enthalten. Anschließend können virtuelle Computer und ihre Anwendungen in verschiedenen Ressourcengruppen bereitgestellt werden. Dieser Ansatz ermöglicht Anwendungsbesitzern, auf die Ressourcengruppe mit ihren virtuellen Computern zuzugreifen, ohne diesen gleichzeitig Zugriff auf die Konfiguration der weiteren virtuellen Netzwerkressourcen zu gewähren.

## <a name="site-connectivity"></a>Standortkonnektivität
### <a name="cloud-only-virtual-networks"></a>Rein cloudbasierte virtuelle Netzwerke
Wenn lokale Benutzer und Computer nicht kontinuierlich mit virtuellen Computern im virtuellen Azure-Netzwerk verbunden sein müssen, wird der Entwurf Ihres virtuellen Netzwerks geradlinig ausfallen:

![Grundlegendes Diagramm eines rein cloudbasierten virtuellen Netzwerks](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Dieser Ansatz eignet sich normalerweise für Internetworkloads, wie z. B. Internetwebserver. Sie können diese virtuellen Computer mit RDP oder Punkt-zu-Standort-VPN-Verbindungen verwalten.

Da sie nicht mit Ihrem lokalen Netzwerk verbunden sind, können nur in Azure bestehende virtuelle Netzwerke einen beliebigen Teil des privaten IP-Adressraums verwenden, auch wenn dieser private Raum bereits lokal verwendet wird.

### <a name="cross-premises-virtual-networks"></a>Standortübergreifende virtuelle Netzwerke
Wenn lokale Benutzer und Computer nicht kontinuierlich mit virtuellen Computern im virtuellen Azure-Netzwerk verbunden sein müssen, erstellen Sie ein standortübergreifendes virtuelles Netzwerk.  Verbinden Sie es über eine ExpressRoute- oder Standort-zu-Standort-VPN-Verbindung mit Ihrem lokalen Netzwerk.

![Diagramm eines standortübergreifenden virtuellen Netzwerks](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

In dieser Konfiguration stellt das virtuelle Azure-Netzwerk im Grunde eine cloudbasierte Erweiterung des lokalen Netzwerks dar.

Da sie mit Ihrem lokalen Netzwerk verbunden sind, müssen standortübergreifende virtuelle Netzwerke einen Teil des von Ihrem Unternehmen genutzten Adressraums verwenden, der eindeutig ist. Ebenso wie verschiedenen Unternehmensstandorten ein spezifisches IP-Subnetz zugewiesen wird, wird Azure bei Erweiterung Ihres Netzwerks zu einem weiteren Standort.

Um Pakete aus Ihrem standortübergreifenden virtuellen Netzwerk in Ihr lokales Netzwerk zu übertragen, müssen Sie den Satz von entsprechenden lokalen Adresspräfixen als Teil der Definition des lokalen Netzwerks für das virtuelle Netzwerk konfigurieren. Je nach dem Adressraum des virtuellen Netzwerks sowie den entsprechenden lokalen Standorten können viele Adresspräfixe im lokalen Netzwerk vorhanden sein.

Sie können ein rein cloudbasiertes virtuelles Netzwerk in ein standortübergreifendes virtuelles Netzwerk konvertieren. Dabei müssen Sie jedoch höchstwahrscheinlich die IP-Adressen des virtuellen Netzwerkadressraums und der Azure-Ressourcen neu zuweisen. Prüfen Sie daher sorgfältig, ob ein virtuelles Netzwerk mit Ihrem lokalen Netzwerk verbunden werden muss, wenn Sie ein IP-Subnetz zuweisen.

## <a name="subnets"></a>Subnetze
Subnetze ermöglichen Ihnen, Ressourcen zu organisieren, die entweder logisch (z.B. in einem Subnetz für virtuelle Computer, die der gleichen Anwendung zugeordnet sind) oder physisch (z.B. in einem Subnetz pro Ressourcengruppe) verknüpft sind. Sie können mit Subnetzen auch Isolationstechniken zur Erhöhung der Sicherheit implementieren.

Für standortübergreifende virtuelle Netzwerke sollten Sie Subnetze gemäß den gleichen Konventionen entwerfen, die für lokale Ressourcen gelten. **Azure verwendet für jedes Subnetz stets die ersten drei IP-Adressen des Adressbereichs**. Um die Anzahl der für das Subnetz benötigten Adressen zu ermitteln, starten Sie mit dem Bestimmen der Anzahl virtueller Computer, die Sie derzeit benötigen. Schätzen Sie das künftige Wachstum, und verwenden Sie dann die folgende Tabelle, um die Größe des Subnetzes zu bestimmen.

| Anzahl der erforderlichen virtuellen Computer | Anzahl der erforderlichen Hostbits | Subnetzgröße |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> Für normale lokale Subnetze ist die maximale Anzahl von Adressen für ein Subnetz mit n Hostbits 2<sup>n</sup>-2. Für ein Azure-Subnetz ist die maximale Anzahl von Adressen für ein Subnetz mit n Hostbits 2<sup>n</sup>-5 (2+3 für die Adressen, die Azure in jedem Subnetz verwendet).
> 
> 

Wenn Sie eine zu kleine Subnetzgröße wählen, müssen Sie den virtuellen Computern im Subnetz neue IP-Adressen zuweisen und die Computer neu bereitstellen.

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Sie können Filterregeln auf den Datenverkehr in Ihren virtuellen Netzwerken anwenden, indem Sie Netzwerksicherheitsgruppen verwenden. Sie können differenzierte Filterregeln zum Schutz Ihrer virtuellen Netzwerkumgebung erstellen, um eingehenden und ausgehenden Datenverkehr, Quell- und Ziel-IP-Bereiche, zugelassene Ports usw. zu steuern. Netzwerksicherheitsgruppen können auf Subnetze innerhalb eines virtuellen Netzwerks oder direkt auf eine bestimmte virtuelle Netzwerkschnittstelle angewendet werden. Es wird empfohlen, ein gewisses Maß an Datenverkehrsfilterung für die Netzwerksicherheitsgruppen in Ihren virtuellen Netzwerken bereitzustellen. Hier finden Sie weitere Informationen zu [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

## <a name="additional-network-components"></a>Zusätzliche Netzwerkkomponenten
Ebenso wie eine lokale physische Netzwerkinfrastruktur können auch virtuelle Azure-Netzwerke mehr als Subnetze und IP-Adressen enthalten. Beim Erstellen Ihrer Anwendungsinfrastruktur möchten Sie möglicherweise einige dieser zusätzlichen Komponenten integrieren:

* [VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) verbinden virtuelle Azure-Netzwerke mit anderen virtuellen Azure-Netzwerken oder ermöglichen über eine Standort-zu-Standort-VPN-Verbindung eine Anbindung an lokale Netzwerke. Richten Sie ExpressRoute-Verbindungen ein, um für fest zugeordnete, sichere Verbindungen zu sorgen. Über Punkt-zu-Standort-VPN-Verbindungen können Sie Benutzern auch einen Direktzugriff bereitstellen.
* [Load Balancer](../load-balancer/load-balancer-overview.md) : Bietet Lastenausgleich für Datenverkehr, ganz nach Wunsch sowohl für externen als auch für internen.
* [Application Gateway](../application-gateway/application-gateway-introduction.md) : Bietet mit HTTP-Lastenausgleich auf der Anwendungsschicht mehr Vorteile für Webanwendungen als die alleinige Bereitstellung von Azure Load Balancer.
* [Traffic Manager](../traffic-manager/traffic-manager-overview.md) : DNS-basierte Verteilung des Datenverkehrs, sodass Endbenutzer zum nächsten verfügbaren Anwendungsendpunkt geleitet werden und Sie Ihre Anwendung außerhalb von Azure-Rechenzentren in unterschiedlichen Regionen hosten können.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Dec16_HO3-->


