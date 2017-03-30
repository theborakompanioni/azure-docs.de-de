---
title: "Azure- und Linux-VM-Netzwerke (Übersicht) | Microsoft Docs"
description: "Eine Übersicht über Azure- und Linux-VM-Netzwerke."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: b5420e35-0463-4456-9803-6142db150f2e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/25/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: f5f611ce6944d37e0afcc9a14fa79d0356d55347
ms.lasthandoff: 03/17/2017


---
# <a name="azure-and-linux-vm-network-overview"></a>Azure- und Linux-VM-Netzwerke (Übersicht)
## <a name="virtual-networks"></a>Virtuelle Netzwerke
Ein virtuelles Azure-Netzwerk (VNet) ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Es ist eine logische Isolierung von der Azure-Cloud für Ihr Abonnement. Sie können die IP-Adressblöcke, DNS-Einstellungen, Sicherheitsrichtlinien und Routentabellen in diesem Netzwerk vollständig steuern. Außerdem können Sie Ihr VNet in Subnetze segmentieren und virtuelle Azure IaaS-Computer (VMs) und/oder Cloud Services (PaaS-Rolleninstanzen) starten. Zudem können Sie das virtuelle Netzwerk mit einer der Konnektivitätsoptionen in Azure mit Ihrem lokalen Netzwerk verbinden. Im Wesentlichen können Sie Ihr Netzwerk mit vollständiger Kontrolle über IP-Adressblöcke auf Azure ausdehnen und von der Azure-Skalierung auf Unternehmensebene profitieren.

* [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)

Führen Sie zum Erstellen eines VNet mithilfe der Azure-Befehlszeilenschnittstelle die folgenden Schritte aus.

* [Erstellen von VNets mithilfe der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-networks-create-vnet-arm-cli.md)

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Eine Netzwerksicherheitsgruppe (NSG) enthält eine Zugriffssteuerungsliste (Access Control List, ACL) zum Zulassen oder Verweigern von Netzwerkdatenverkehr an Ihre VM-Instanzen in einem virtuellen Netzwerkwerk. NSGs können Subnetzen oder einzelnen VM-Instanzen innerhalb dieses Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VM-Instanzen in diesem Subnetz. Darüber hinaus kann Datenverkehr zu einer einzelnen virtuellen Maschine weiter beschränkt werden, indem eine NSG direkt diesem virtuellen Computer zugewiesen wird.

* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Erstellen von NSGs in der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-networks-create-nsg-arm-cli.md)

## <a name="user-defined-routes"></a>Benutzerdefinierte Routen
Wenn Sie virtuelle Computer in einem virtuellen Netzwerk (VNet) in Azure hinzufügen, werden Sie feststellen, dass die virtuellen Computer automatisch über das Netzwerk miteinander kommunizieren können. Sie müssen kein Gateway angeben, auch wenn die virtuellen Computer sich in unterschiedlichen Subnetzen befinden. Das gleiche gilt für die Kommunikation zwischen den virtuellen Computern und dem öffentlichen Internet und sogar für das lokale Netzwerk, wenn eine Hybridverbindung zwischen Azure und Ihrem eigenen Rechenzentrum vorhanden ist.

* [Was sind benutzerdefinierte Routen und IP-Weiterleitung?](../virtual-network/virtual-networks-udr-overview.md)
* [Erstellen benutzerdefinierter Routen (UDR) in der Azure-Befehlszeilenschnittstelle](../virtual-network/virtual-network-create-udr-arm-cli.md)

## <a name="associating-a-fqdn-to-your-linux-vm"></a>Zuordnen eines FQDN zu Ihrem virtuellen Linux-Computer
Beim Erstellen eines virtuellen Computers (VM, Virtual Machine) im Azure-Portal mit dem Resource Manager-Bereitstellungsmodell wird automatisch eine öffentliche IP als Ressource für den virtuellen Computer erstellt. Mit dieser IP-Adresse greifen Sie per Remotezugriff auf den virtuellen Computer zu. Obwohl das Portal standardmäßig keinen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) erstellt, können Sie nach der Erstellung des virtuellen Computers einen solchen hinzufügen.

* [Erstellen eines vollständig qualifizierten Domänennamens im Azure-Portal](virtual-machines-linux-portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="network-interfaces"></a>Netzwerkschnittstellen
Eine Netzwerkschnittstelle (NIC) ist eine Verbindung zwischen einem virtuellen Computer (VM) und dem zugrunde liegenden Softwarenetzwerk. In diesem Artikel wird erläutert, was eine Netzwerkschnittstelle ist und wie sie im Azure Resource Manager-Bereitstellungsmodell verwendet wird.

* [Virtuelle Netzwerkschnittstellen](../virtual-network/virtual-network-network-interface.md)

## <a name="virtual-nics-and-dns-labeling"></a>Virtuelle NICs und DNS-Bezeichnung
Wenn Sie einen Server haben, der persistent sein muss, aber dieser Server häufig umkonfiguriert und bereitgestellt wird, können Sie die DNS-Bezeichnung der NIC als persistenten Namen im VNet verwenden.  Mit den folgenden Schritten richten Sie eine dauerhaft benannte NIC mit einer statischen IP-Adresse ein.

* [Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network-gateways"></a>Gateways für virtuelle Netzwerke
Ein virtuelles Netzwerkgateway dient zum Senden von Netzwerkdatenverkehr zwischen virtuellen Azure-Netzwerken und lokalen Standorten sowie zwischen virtuellen Netzwerken innerhalb von Azure (VNet-zu-VNet). Beim Konfigurieren eines VPN-Gateways müssen Sie ein Gateway des virtuellen Netzwerks und eine dazugehörige Verbindung erstellen und konfigurieren.

* [Informationen zu VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)

## <a name="internal-load-balancing"></a>Interner Lastenausgleich
Ein Azure Load Balancer ist ein Layer-4-Load Balancer (TCP, UDP). Der Load Balancer sorgt für hohe Verfügbarkeit, indem er eingehenden Datenverkehr zwischen funktionierenden Dienstinstanzen in Clouddiensten oder auf virtuelle Computer verteilt, die in einer Gruppe für den Lastenausgleich definiert wurden. Der Azure Load Balancer kann diese Dienste auch auf mehreren Ports, mehreren IP-Adressen oder beidem leisten.

* [Erstellen eines internen Load Balancers mithilfe der Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-internet-arm-cli.md)


