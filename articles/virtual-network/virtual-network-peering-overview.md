---
title: Peering in virtuellen Azure-Netzwerken | Microsoft-Dokumentation
description: Weitere Informationen zum Peering in virtuellen Netzwerken in Azure
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/06/2017
ms.author: narayan
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: 87853ff1e33007137c65e03882b5ebc74a372023
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="virtual-network-peering"></a>Peering in virtuellen Netzwerken
VNET-Peering ist ein Mechanismus, mit dem zwei virtuelle Netzwerke (VNETs) in der gleichen Region über das Azure-Backbonenetzwerk miteinander verbunden werden können. Nach dem Peering werden die beiden virtuellen Netzwerke für Verbindungszwecke als einzelnes Element angezeigt. Die beiden virtuellen Netzwerke werden zwar weiterhin als separate Ressourcen verwaltet, virtuelle Computer in diesen virtuellen Peernetzwerken können aber über private IP-Adressen direkt miteinander kommunizieren.

Der Datenverkehr zwischen virtuellen Computern in den virtuellen Netzwerken, die mittels Peering verknüpft sind, wird ähnlich wie der Datenverkehr zwischen virtuellen Computern im gleichen Netzwerk über die Azure-Infrastruktur geleitet. Die Verwendung von VNET-Peering bietet unter anderem folgende Vorteile:

* Niedrige Latenz, Verbindung mit hoher Bandbreite zwischen Ressourcen in unterschiedlichen virtuellen Netzwerken
* Möglichkeit zur Verwendung von Ressourcen wie virtuellen Netzwerkgeräten und VPN-Gateways als Transitpunkte in einem per Peering verknüpften virtuellen Netzwerk
* Möglichkeit zum Peering von zwei virtuellen Netzwerken, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden oder eines virtuellen Netzwerks, das mit dem Azure Resource Manager-Modell erstellt wurde, mit einem virtuellen Netzwerk, das mit dem klassischen Bereitstellungsmodell erstellt wurde Weitere Informationen zu den Unterschieden zwischen den beiden Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).

Anforderungen und zentrale Aspekte des VNET-Peerings:

* Die beiden mittels Peering verknüpften virtuellen Netzwerke müssen sich in der gleichen Azure-Region befinden.
* Die mittels Peering verknüpften virtuellen Netzwerke müssen über IP-Adressräume ohne Überschneidungen verfügen.
* Adressräume können weder hinzugefügt noch aus einem virtuellen Netzwerk gelöscht werden, nachdem ein virtuelles Netzwerk per Peering mit einem anderen virtuellen Netzwerk verknüpft wurde.
* Das VNET-Peering erfolgt zwischen zwei virtuellen Netzwerken. Es besteht keine abgeleitete transitive Beziehung zwischen Peerings. Wenn beispielsweise mittels Peering virtualNetworkA mit virtualNetworkB und virtualNetworkB mit virtualNetworkC verknüpft wird, ist virtualNetworkA *nicht* mit virtualNetworkC verknüpft.
* Sie können virtuelle Netzwerken in zwei verschiedenen Abonnements mittels Peering verknüpfen, sofern ein privilegierter Benutzer beider Abonnements das Peering autorisiert und die Abonnements dem gleichen Active Directory-Mandanten zugewiesen sind.
* Virtuelle Netzwerke können mittels Peering miteinander verknüpft werden, wenn beide mit dem Resource Manager-Bereitstellungsmodell erstellt wurden oder wenn eines davon mit dem Resource Manager-Bereitstellungsmodul und das andere mit dem klassischen Bereitstellungsmodell erstellt wurde. Zwei virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können hingegen nicht mittels Peering miteinander verknüpft werden. Beim Peering von virtuellen Netzwerken, die mit verschiedenen Bereitstellungsmodellen erstellt wurden, müssen die virtuellen Netzwerke zu *demselben* Abonnement gehören. Das Peering von virtuellen Netzwerken, die in verschiedenen Bereitstellungsmodellen erstellt wurden und zu *unterschiedlichen* Abonnements gehören, ist mit der **Vorschauversion** möglich. Weitere Informationen finden Sie im Artikel [Erstellen von VNET-Peerings](virtual-network-create-peering.md#different-subscriptions-different-deployment-models).
* Für die Kommunikation zwischen virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken bestehen zwar keine weiteren Bandbreiteneinschränkungen, es gilt jedoch die auf der VM-Größe basierende Bandbreitenobergrenze. Weitere Informationen zur Bandbreitenobergrenze für verschiedene VM-Größen finden Sie in den Artikeln zu den Größen von virtuellen Computern unter [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

![Einfaches Peering von virtuellen Netzwerken](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Konnektivität
Nachdem zwei virtuelle Netzwerke mittels Peering verknüpft wurden, können virtuelle Computer oder Cloud Services-Rollen im virtuellen Netzwerk eine direkte Verbindung mit anderen Ressourcen des mittels Peering verknüpften virtuellen Netzwerks herstellen. Die beiden virtuellen Netzwerke verfügen über uneingeschränkte Konnektivität auf IP-Ebene.

Die Netzwerklatenz für einen Roundtrip zwischen zwei virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken unterscheidet sich nicht von der Latenz für einen Roundtrip innerhalb eines einzelnen virtuellen Netzwerks. Der Netzwerkdurchsatz basiert auf der Bandbreite, die für den virtuellen Computer proportional zu seiner Größe zulässig ist. Im Peering bestehen keine weiteren Bandbreiteneinschränkungen.

Der Datenverkehr zwischen den virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken wird nicht über ein Gateway, sondern direkt über die Back-End-Infrastruktur von Azure geleitet.

Virtuelle Computer mit einer Verbindung mit einem virtuellen Netzwerk können im mittels Peering verknüpften virtuellen Netzwerk auf die Endpunkte mit internem Lastenausgleich zugreifen. Netzwerksicherheitsgruppen können bei Bedarf in beiden virtuellen Netzwerken angewendet werden, um den Zugriff auf andere virtuelle Netzwerke oder Subnetze zu blockieren.

Beim Konfigurieren des VNET-Peerings können Sie die Regeln für Netzwerksicherheitsgruppen zwischen den virtuellen Netzwerken öffnen oder schließen. Wenn Sie sich für das Öffnen der vollständigen Konnektivität zwischen den mittels Peering verknüpften virtuellen Netzwerken entscheiden (Standardoption), können Sie Netzwerksicherheitsgruppen in bestimmten Subnetzen oder auf virtuellen Computern verwenden, um den Zugriff jeweils spezifisch zu blockieren oder zu verweigern. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie im Artikel [Netzwerksicherheitsgruppen – Übersicht](virtual-networks-nsg.md).

In mittels Peering verknüpften virtuellen Netzwerken funktioniert die von Azure bereitgestellte interne DNS-Namensauflösung für virtuelle Computer nicht. Virtuelle Computer besitzen interne DNS-Namen, die nur im lokalen virtuellen Netzwerk aufgelöst werden können. Sie können virtuelle Computer, die mit mittels Peering verknüpften virtuellen Netzwerken verbunden sind, aber als DNS-Server für ein virtuelles Netzwerk konfigurieren. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="service-chaining"></a>Dienstverkettung
Sie können benutzerdefinierte Routingtabellen konfigurieren, die auf virtuelle Computer in mittels Peering verknüpften virtuellen Netzwerken als IP-Adresse für den nächsten Hop verweisen, um die Dienstverkettung zu aktivieren. Eine Dienstverkettung ermöglicht es, Datenverkehr über benutzerdefinierte Routingtabellen aus einem virtuellen Netzwerk an ein virtuelles Gerät in einem mittels Peering verknüpften virtuellen Netzwerk zu leiten.

Sie können auch Hub-and-Spoke-Umgebungen erstellen, in denen der Hub Infrastrukturkomponenten wie etwa ein virtuelles Netzwerk-Gerät hosten kann. Alle virtuellen Spoke-Netzwerke können dann mittels Peering mit dem virtuellen Hubnetzwerk verknüpft werden. Datenverkehr kann virtuelle Netzwerkgeräte durchlaufen, die im virtuellen Hubnetzwerk ausgeführt werden. Kurz gesagt: Beim VNET-Peering kann die IP-Adresse für den nächsten Hop in der benutzerdefinierten Routingtabelle die IP-Adresse eines virtuellen Computers im mittels Peering verknüpften virtuellen Netzwerk sein. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen – Übersicht](virtual-networks-udr-overview.md).

## <a name="gateways-and-on-premises-connectivity"></a>Gateways und lokale Konnektivität
Jedes virtuelle Netzwerk kann unabhängig davon, ob eine mittels Peering hergestellte Verknüpfung mit einem anderen virtuellen Netzwerk besteht, weiterhin über ein eigenes Gateway verfügen und dieses zum Herstellen einer Verbindung mit einem lokalen Netzwerk verwenden. Sie können auch dann [VNET-zu-VNET-Verbindungen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) unter Verwendung von Gateways konfigurieren, wenn die virtuellen Netzwerke mittels Peering miteinander verknüpft sind.

Wenn beide Optionen für Verbindungen zwischen virtuellen Netzwerken konfiguriert sind, fließt der Datenverkehr zwischen den virtuellen Netzwerken über die Peeringkonfiguration (also über den Azure-Backbone).

Wenn virtuelle Netzwerke mittels Peering verknüpft sind, können Sie auch das Gateway im mittels Peering verknüpften virtuellen Netzwerk als Transitpunkt für ein lokales Netzwerk konfigurieren. In diesem Fall kann das virtuelle Netzwerk, das ein Remotegateway verwendet, kein eigenes Gateway besitzen. Ein virtuelles Netzwerk kann immer nur ein einzelnes Gateway aufweisen. Bei diesem Gateway kann es sich um ein lokales Gateway oder ein Remotegateway (im mittels Peering verknüpften virtuellen Netzwerk) handeln, wie im folgenden Bild zu sehen ist:

![VNet-Peering – Transit](./media/virtual-networks-peering-overview/figure02.png)

Gatewaytransit wird in der Peeringbeziehung zwischen virtuellen Netzwerken, die mit unterschiedlichen Bereitstellungsmodellen erstellt wurden, nicht unterstützt. Sie müssen beide virtuellen Netzwerke in der Peeringbeziehung mit dem Resource Manager-Bereitstellungsmodell erstellen, um den Gatewaytransit verwenden zu können.

Wenn die virtuellen Netzwerke, die gemeinsam eine einzelne ExpressRoute-Verbindung nutzen, mittels Peering verknüpft sind, fließt der Datenverkehr zwischen ihnen über die Peeringbeziehung (also über das Azure-Backbonenetzwerk). Sie können in den einzelnen virtuellen Netzwerken weiterhin lokale Gateways verwenden, um eine Verbindung mit der lokalen Umgebung herzustellen. Alternativ können Sie ein gemeinsam genutztes Gateway verwenden und den Transit für lokale Konnektivität konfigurieren.

## <a name="provisioning"></a>Bereitstellung
Das VNET-Peering ist ein privilegierter Vorgang. Es ist eine separate Funktion des VirtualNetworks-Namespace. Einem Benutzer können bestimmte Rechte für die Autorisierung des Peerings erteilt werden. Ein Benutzer mit Lese-/Schreibzugriff auf das virtuelle Netzwerk erbt diese Rechte automatisch.

Ein Administrator oder privilegierter Benutzer der Peeringfunktion kann einen Peeringvorgang für ein anderes virtuelles Netzwerk initiieren. Wenn eine übereinstimmende Anforderung für das Peering auf der anderen Seite vorhanden ist und weitere Anforderungen erfüllt sind, kommt das Peering zustande.

## <a name="limits"></a>Einschränkungen
Pro virtuellem Netzwerk ist nur eine begrenzte Anzahl von Peerings zulässig. Weitere Informationen finden in den [Azure-Netzwerkgrenzwerten](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Preise
Für ein- und ausgehenden Datenverkehr, der VNET-Peering verwendet, fällt eine Gebühr an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Nächste Schritte

* Führen Sie das [Tutorial zum VNET-Peering](virtual-network-create-peering.md) durch.
* Erfahren Sie mehr über alle [Einstellungen für das VNET-Peering und deren Änderung](virtual-network-manage-peering.md).
