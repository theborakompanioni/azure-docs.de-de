---
title: Virtuelles Azure-Netzwerk | Microsoft-Dokumentation
description: "Enthält Informationen zu den Konzepten und Features des virtuellen Azure-Netzwerks (Azure Virtual Network)."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: 22c0ee5090d67430fc63ad3f3c09076de0be067c
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="azure-virtual-network"></a>Virtuelles Azure-Netzwerk

Mit dem Dienst für das virtuelle Azure-Netzwerk (Azure Virtual Network) können Sie Azure-Ressourcen über virtuelle Netzwerke (VNets) sicher miteinander verbinden. Ein VNet ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation von der dedizierten Azure-Cloud für Ihr Abonnement. Sie können VNets auch mit Ihrem lokalen Netzwerk verbinden. In der folgenden Abbildung sind einige Funktionen des Diensts für virtuelle Azure-Netzwerke dargestellt:

![Netzwerkdiagramm](./media/virtual-networks-overview/virtual-network-overview.png)

Sie können jeweils auf eine der folgenden Funktionen klicken, um weitere Informationen dazu anzuzeigen:
- **[Isolation:](#isolation)** VNets sind voneinander isoliert. Sie können separate VNets für Entwicklung, Tests und Produktion erstellen, die die gleichen CIDR-Adressblöcke verwenden. Im Gegensatz dazu können Sie mehrere VNets erstellen, für die unterschiedliche CIDR-Adressblöcke verwendet werden, und die Netzwerke verbinden. Sie können ein VNet in mehrere Subnetze segmentieren. Azure ermöglicht die interne Namensauflösung für VMs und Cloud Services-Rolleninstanzen, die mit einem VNet verbunden sind. Sie können ein VNet optional so konfigurieren, dass Ihre eigenen DNS-Server verwendet werden, anstatt die interne Namensauflösung von Azure zu nutzen.
- **[Internetkonnektivität:](#internet)** Für alle Azure Virtual Machines (VMs) und Cloud Services-Rolleninstanzen, die mit einem VNet verbunden sind, besteht standardmäßig Zugriff auf das Internet. Bei Bedarf können Sie auch den Zugriff in eingehender Richtung auf bestimmte Ressourcen ermöglichen.
- **[Konnektivität von Azure-Ressourcen:](#within-vnet)** Azure-Ressourcen, z.B. Cloud Services und VMs, können mit demselben VNet verbunden werden. Für die Verbindung miteinander können für die Ressourcen auch dann private IP-Adressen verwendet werden, wenn sie sich in unterschiedlichen Subnetzen befinden. Azure verfügt über Standardrouting zwischen Subnetzen, VNets und lokalen Netzwerken, sodass Sie keine Routen konfigurieren und verwalten müssen.
- **[VNet-Konnektivität:](#connect-vnets)** VNets können miteinander verbunden werden, sodass Ressourcen, für die eine Verbindung mit einem VNet besteht, mit Ressourcen in einem anderen VNet kommunizieren können.
- **[Lokale Konnektivität:](#connect-on-premises)** VNets können mit lokalen Netzwerken verbunden werden, indem private Netzwerkverbindungen zwischen Ihrem Netzwerk und Azure oder Site-to-Site-VPN-Verbindungen über das Internet verwendet werden.
- **[Filterung von Datenverkehr:](#filtering)** Netzwerkdatenverkehr von VM- und Cloud Services-Rolleninstanzen kann in eingehender und ausgehender Richtung nach IP-Quelladresse und -port, IP-Zieladresse und -port und Protokoll gefiltert werden.
- **[Routing:](#routing)** Sie können das Standardrouting von Azure optional außer Kraft setzen, indem Sie Ihre eigenen Routen konfigurieren oder BGP-Routen über ein Netzwerkgateway verwenden.

## <a name = "isolation"></a>Netzwerkisolation und -segmentierung

Sie können in jedem Azure-[Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und in jeder Azure-[Region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) mehrere VNets implementieren. Jedes VNet ist von anderen VNets isoliert. Für jedes VNet ist Folgendes möglich:
- Geben Sie einen benutzerdefinierten privaten IP-Adressraum ein, indem Sie öffentliche und private Adressen (RFC 1918) verwenden. Azure weist Ressourcen, die mit dem VNet verbunden sind, eine private IP-Adresse aus dem von Ihnen zugewiesenen Adressraum zu.
- Segmentieren Sie das VNet in mindestens ein Subnetz, und ordnen Sie jedem Subnetz einen Teil des VNet-Adressraums zu.
- Verwenden Sie die in Azure enthaltene Namensauflösung, oder geben Sie Ihren eigenen DNS-Server an, der für die mit einem VNet verbundenen Ressourcen genutzt werden kann. Weitere Informationen zur Namensauflösung in VNets finden Sie im Artikel [Namensauflösung für virtuelle Computer und Clouddienste](virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name = "internet"></a>Herstellen einer Verbindung mit dem Internet
Alle Ressourcen, die mit einem VNet verbunden sind, verfügen standardmäßig über Internetkonnektivität in ausgehender Richtung. Die private IP-Adresse der Ressource wird von der Azure-Infrastruktur per SNAT-Vorgang (Source Network Address Translated) in eine öffentliche IP-Adresse übersetzt. Weitere Informationen zur Internetkonnektivität in ausgehender Richtung finden Sie im Artikel [Grundlegendes zu ausgehenden Verbindungen in Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address). Sie können die Standardkonnektivität ändern, indem Sie das benutzerdefinierte Routing und die Filterung von Datenverkehr implementieren.

Um über das Internet in eingehender Richtung mit Azure-Ressourcen bzw. in ausgehender Richtung ohne SNAT über das Internet kommunizieren zu können, muss einer Ressource eine öffentliche IP-Adresse zugewiesen sein. Weitere Informationen zu öffentlichen IP-Adressen finden Sie im Artikel [Öffentliche IP-Adressen](virtual-network-public-ip-address.md).

## <a name="within-vnet"></a>Herstellen einer Verbindung mit Azure-Ressourcen
Sie können mehrere Azure-Ressourcen mit einem VNet verbinden, z.B. virtuelle Computer (VMs), Cloud Services, App Service-Umgebungen und VM-Skalierungsgruppen. VMs stellen Verbindungen mit einem Subnetz in einem VNet über eine Netzwerkschnittstelle (NIC) her. Weitere Informationen zu NICs finden Sie im Artikel [Netzwerkschnittstellen](virtual-network-network-interface.md).

## <a name="connect-vnets"></a>Herstellen von Verbindungen zwischen virtuellen Netzwerken

Sie können VNets miteinander verbinden, sodass Ressourcen, für die eine Verbindung mit einem der VNets besteht, darüber miteinander kommunizieren können. Nutzen Sie eine der folgenden Optionen (oder beide), um VNets miteinander zu verbinden:
- **Peering:** Hierdurch wird ermöglicht, dass Ressourcen, die mit unterschiedlichen Azure-VNets desselben Azure-Standorts verbunden sind, miteinander kommunizieren können. Die Bandbreite und Wartezeit für die VNets entspricht den Werten, die gelten, wenn die Ressourcen mit demselben VNet verbunden sind. Weitere Informationen zum Peering finden Sie im Artikel [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).
- **VNet-zu-VNet-Verbindung:** Hierbei können Ressourcen, für die eine Verbindung mit unterschiedlichen Azure-VNets besteht, an demselben oder verschiedenen Azure-Standorten verbunden werden. Im Gegensatz zum Peering ist die Bandbreite zwischen VNets beschränkt, da der Datenverkehr über ein Azure VPN Gateway fließen muss. Weitere Informationen zum Verbinden von VNets mit einer VNet-zu-VNet-Verbindung finden Sie im Artikel [Konfigurieren von VNet-zu-VNet-Verbindungen](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="connect-on-premises"></a>Herstellen einer Verbindung mit einem lokalen Netzwerk

Sie können Ihr lokales Netzwerk mit einem VNet verbinden, indem Sie eine Kombination der folgenden Optionen verwenden:
- **Point-to-Site-VPN (Virtual Private Network):** Wird zwischen einem einzelnen PC Ihres Netzwerks und dem VNet eingerichtet. Dieser Verbindungstyp ist gut geeignet, wenn Azure noch neu für Sie ist, oder wenn Sie ein Entwickler sind, da keine oder nur sehr geringe Änderungen Ihres vorhandenen Netzwerks erforderlich sind. Für die Verbindung wird das SSTP-Protokoll verwendet, um zwischen dem PC und dem VNet eine verschlüsselte Kommunikation über das Internet zu ermöglichen. Die Dauer der Wartezeit für ein Point-to-Site-VPN ist unvorhersehbar, weil der Datenverkehr über das Internet übertragen wird.
- **Site-to-Site-VPN:** Wird zwischen Ihrem VPN-Gerät und einem Azure VPN Gateway eingerichtet. Bei diesem Verbindungstyp können alle lokalen Ressourcen, die von Ihnen autorisiert werden, auf ein VNet zugreifen. Die Verbindung ist ein IPSec/IKE-VPN, mit dem die verschlüsselte Kommunikation über das Internet zwischen Ihrem lokalen Gerät und dem Azure VPN Gateway ermöglicht wird. Die Dauer der Wartezeit für eine Site-to-Site-Verbindung ist unvorhersehbar, da der Datenverkehr über das Internet übertragen wird.
- **Azure ExpressRoute:** Wird zwischen Ihrem Netzwerk und Azure über einen ExpressRoute-Partner eingerichtet. Diese Verbindung ist privat. Der Datenverkehr wird nicht über das Internet übertragen. Die Dauer der Wartezeit für eine ExpressRoute-Verbindung ist vorhersehbar, da der Datenverkehr nicht über das Internet verläuft.

Weitere Informationen zu allen vorherigen Verbindungsoptionen finden Sie im Artikel [Diagramme zur Verbindungstopologie](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namediagramsaconnection-topology-diagrams).

## <a name="filtering"></a>Filtern des Netzwerkdatenverkehrs
Sie können den Netzwerkdatenverkehr zwischen Subnetzen filtern, indem Sie eine oder beide folgenden Optionen verwenden:
- **Netzwerksicherheitsgruppen (NSG):** Jede Netzwerksicherheitsgruppe kann mehrere Sicherheitsregeln für die eingehende und ausgehende Richtung enthalten, mit denen Sie Datenverkehr nach IP-Adresse, Port und Protokoll für die Quelle und das Ziel filtern können. Sie können eine NSG auf jede Netzwerkschnittstelle einer VM anwenden. Außerdem können Sie eine NSG auf das Subnetz anwenden, mit dem eine Netzwerkschnittstelle oder eine andere Azure-Ressource verbunden ist. Weitere Informationen zu NSGs finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md).
- **Virtuelle Netzwerkgeräte:** Ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) ist eine VM, auf der Software für eine Netzwerkfunktion ausgeführt wird, z.B. eine Firewall. In [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) können Sie eine Liste mit verfügbaren NVAs anzeigen. Es sind auch NVAs verfügbar, die eine WAN-Optimierung und andere Funktionen für den Netzwerkdatenverkehr ermöglichen. NVAs werden normalerweise mit benutzerdefinierten oder BGP-Routen verwendet. Darüber hinaus können Sie eine NVA zum Filtern von Datenverkehr zwischen VNets verwenden.

## <a name="routing"></a>Weiterleiten von Netzwerkdatenverkehr

Azure erstellt Routentabellen, über die Ressourcen, die mit einem beliebigen Subnetz in einem beliebigen VNet verbunden sind, standardmäßig kommunizieren können. Sie können eine oder beide der folgenden Optionen implementieren, um die von Azure erstellten Standardrouten außer Kraft zu setzen:
- **Benutzerdefinierte Routen:** Sie können benutzerdefinierte Routentabellen mit Routen erstellen, über die gesteuert wird, wohin der Datenverkehr für die einzelnen Subnetze geleitet wird. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
- **BGP-Routen:** Wenn Sie Ihr VNet über ein Azure VPN Gateway oder eine ExpressRoute-Verbindung mit dem lokalen Netzwerk verbinden, können Sie die BGP-Routen für Ihre VNets übernehmen.

## <a name="pricing"></a>Preise

Für virtuelle Netzwerke, Subnetze, Routentabellen oder Netzwerksicherheitsgruppen werden keine Gebühren berechnet. Für Internetbandbreite in ausgehender Richtung, öffentliche IP-Adressen, Peering in virtuellen Netzwerken, VPN Gateways und ExpressRoute werden jeweils eigene Preisstrukturen verwendet. Weitere Informationen hierzu finden Sie auf den Preisseiten zu [virtuellen Netzwerken](https://azure.microsoft.com/pricing/details/virtual-network), [VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) und [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute).

## <a name="faq"></a>Häufig gestellte Fragen

Wenn Sie sich die häufig gestellten Fragen zu Virtual Network ansehen möchten, lesen Sie den Artikel [Azure Virtual Network – häufig gestellte Fragen](virtual-networks-faq.md).


## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie Ihr erstes VNet, und verbinden Sie einige VMs damit, indem Sie die Schritte des Artikels [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md) ausführen.
- Erstellen Sie eine Point-to-Site-Verbindung mit einem VNet, indem Sie die Schritte im Artikel [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ausführen.

