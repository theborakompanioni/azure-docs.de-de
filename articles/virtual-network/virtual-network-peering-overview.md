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
ms.date: 10/17/2016
ms.author: narayan
translationtype: Human Translation
ms.sourcegitcommit: 15afcad97941fc595478e36e826a73831f40475e
ms.openlocfilehash: eb05b504c5cf13cd852a5e01cc3bec79fd20d547


---
# <a name="virtual-network-peering"></a>Peering in virtuellen Netzwerken
VNET-Peering ist ein Mechanismus, mit dem zwei virtuelle Netzwerke (VNETs) in der gleichen Region über das Azure-Backbonenetzwerk miteinander verbunden werden können. Nach dem Peering werden die beiden virtuellen Netzwerke für alle Verbindungszwecke als einzelnes Element angezeigt. Sie werden zwar weiterhin als separate Ressourcen verwaltet, jedoch können virtuelle Computer in den verknüpften VNets über private IP-Adressen direkt miteinander kommunizieren.

Der Datenverkehr zwischen virtuellen Computern in den virtuellen Netzwerken, die mittels Peering verknüpft sind, wird ähnlich wie der Datenverkehr zwischen virtuellen Computern im gleichen Netzwerk über die Azure-Infrastruktur geleitet. Die Verwendung von VNet-Peering bietet unter anderem folgende Vorteile:

* Eine Verbindung mit hoher Bandbreite und niedriger Latenz zwischen Ressourcen in unterschiedlichen virtuellen Netzwerken
* Möglichkeit zur Verwendung von Ressourcen wie virtuellen Netzwerkgeräten und VPN-Gateways als Transitpunkte in einem per Peering verknüpften VNet
* Möglichkeit zum Peering von zwei virtuellen Netzwerken, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden oder eines virtuellen Netzwerks, das mit dem Azure Resource Manager-Modell erstellt wurde, mit einem virtuellen Netzwerk, das mit dem klassischen Bereitstellungsmodell erstellt wurde Weitere Informationen zu den Unterschieden zwischen den beiden Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).

Anforderungen und zentrale Aspekte des VNet-Peerings:

* Die beiden mittels Peering verknüpften virtuellen Netzwerke müssen sich in der gleichen Azure-Region befinden.
* Die mittels Peering verknüpften virtuellen Netzwerke müssen über IP-Adressräume ohne Überschneidungen verfügen.
* VNet-Peering erfolgt zwischen zwei virtuellen Netzwerken, und es besteht keine abgeleitete transitive Beziehung zwischen Peerings. Wenn beispielsweise VNetA mittels Peering mit VNetB verknüpft ist und VNetB mittels Peering mit VNetC verknüpft ist, ist VNetA *nicht* mittels Peering mit VNetC verknüpft.
* Sie können virtuellen Netzwerken in zwei verschiedenen Abonnements mittels Peering verknüpfen, sofern ein privilegierter Benutzer beider Abonnements das Peering autorisiert und die Abonnements dem gleichen Active Directory-Mandanten zugewiesen sind.
* Virtuelle Netzwerke können mittels Peering miteinander verknüpft werden, wenn beide mit dem Resource Manager-Bereitstellungsmodell erstellt wurden oder wenn eines davon mit dem Resource Manager-Bereitstellungsmodul und das andere mit dem klassischen Bereitstellungsmodell erstellt wurde. Virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können hingegen nicht miteinander verknüpft werden. Beim Peering von virtuellen Netzwerken, die mit verschiedenen Bereitstellungsmodellen erstellt wurden, müssen die virtuellen Netzwerke zum *selben* Abonnement gehören. Das Peering von virtuellen Netzwerken, die in verschiedenen Bereitstellungsmodellen erstellt wurden und zu *unterschiedlichen* Abonnements gehören, ist mit der **Vorschauversion** möglich. Weitere Informationen finden Sie im Artikel [Erstellen des VNet-Peerings mit PowerShell-Cmdlets](virtual-networks-create-vnetpeering-arm-ps.md).
* Für die Kommunikation zwischen virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken bestehen zwar keine weiteren Bandbreiteneinschränkungen, es gilt jedoch die auf der VM-Größe basierende Bandbreitenobergrenze. Weitere Informationen zur Bandbreitenobergrenze für verschiedene VM-Größen finden Sie in den Artikeln zu VM-Größen unter [Windows](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux](../virtual-machines/virtual-machines-linux-sizes.md).

![Einfaches VNet-Peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Konnektivität
Nachdem zwei virtuelle Netzwerke mittels Peering verknüpft wurden, können virtuelle Computer oder Cloud Services-Rollen im virtuellen Netzwerk eine direkte Verbindung mit anderen Ressourcen des mittels Peering verknüpften virtuellen Netzwerks herstellen. Diese beiden virtuellen Netzwerke verfügen über uneingeschränkte Konnektivität auf IP-Ebene.

Die Netzwerklatenz für einen Roundtrip zwischen zwei virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken unterscheidet sich nicht von der Latenz für einen Roundtrip innerhalb eines einzelnen virtuellen Netzwerks. Der Netzwerkdurchsatz basiert auf der Bandbreite, die für den virtuellen Computer proportional zu seiner Größe zulässig ist. Im Peering bestehen keine weiteren Bandbreiteneinschränkungen.

Der Datenverkehr zwischen den virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken wird nicht über ein Gateway, sondern direkt über die Back-End-Infrastruktur von Azure geleitet.

Virtuelle Computer in einem virtuellen Netzwerk können im mittels Peering verknüpften virtuellen Netzwerk auf die Endpunkte mit internem Lastenausgleich (Internal Load Balancing, ILB) zugreifen. Netzwerksicherheitsgruppen (NSG) können in beiden VNets angewendet werden, um den Zugriff auf andere virtuelle Netzwerkwerke oder Subnetze bei Bedarf zu blockieren.

Beim Konfigurieren des Peerings können Sie die NSG-Regeln zwischen den virtuellen Netzwerken öffnen oder schließen. Wenn Sie vollständige Konnektivität zwischen den mittels Peering verknüpften virtuellen Netzwerken öffnen (Standardoption), können Sie NSG in bestimmten Subnetzen oder auf virtuellen Computern verwenden, um den Zugriff jeweils spezifisch zu blockieren oder zu verweigern. Weitere Informationen finden Sie im Artikel [Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

In mittels Peering verknüpften virtuellen Netzwerken funktioniert die von Azure bereitgestellte interne DNS-Namensauflösung für virtuelle Computer nicht. Virtuelle Computer besitzen interne DNS-Namen, die nur im lokalen virtuellen Netzwerk aufgelöst werden können. Sie können jedoch virtuelle Computer, die in mittels Peering verknüpften virtuellen Netzwerken ausgeführt werden, als DNS-Server für ein virtuelles Netzwerk konfigurieren. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="service-chaining"></a>Dienstverkettung
Sie können benutzerdefinierte Routingtabellen konfigurieren, die auf virtuelle Computer in mittels Peering verknüpften virtuellen Netzwerken als IP-Adresse für den nächsten Hop verweisen (siehe Diagramm weiter unten in diesem Artikel). So erzielen Sie eine Dienstverkettung, mit der Sie Datenverkehr über benutzerdefinierte Routingtabellen aus einem virtuellen Netzwerk an ein virtuelles Gerät leiten können, das in einem per Peering verknüpften VNet ausgeführt wird.

Sie können auch Hub-and-Spoke-Umgebungen erstellen, in denen der Hub Infrastrukturkomponenten wie etwa ein virtuelles Netzwerk-Gerät hosten kann. Alle virtuellen Spoke-Netzwerke können mittels Peering mit dem Hub verknüpft werden. Gleiches gilt für eine Teilmenge des Datenverkehrs an Geräte, die im virtuellen Hub-Netzwerk ausgeführt werden. Kurz gesagt: Beim VNet-Peering kann die IP-Adresse für den nächsten Hop in der benutzerdefinierten Routingtabelle die IP-Adresse eines virtuellen Computers im mittels Peering verknüpften virtuellen Netzwerk sein. Weitere Informationen finden Sie im Artikel [Was sind benutzerdefinierte Routen und IP-Weiterleitung?](virtual-networks-udr-overview.md).

## <a name="gateways-and-on-premises-connectivity"></a>Gateways und lokale Konnektivität
Jedes virtuelle Netzwerk kann unabhängig davon, ob eine mittels Peering hergestellte Verknüpfung mit einem anderen virtuellen Netzwerk besteht, weiterhin über ein eigenes Gateway verfügen und dieses zum Herstellen einer Verbindung mit einem lokalen Netzwerk verwenden. Benutzer können auch dann [VNet-zu-VNet-Verbindungen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) unter Verwendung von Gateways konfigurieren, wenn die virtuellen Netzwerke mittels Peering miteinander verknüpft sind.

Wenn beide Optionen für die Verbindungen zwischen VNets konfiguriert sind, fließt der Datenverkehr zwischen den virtuellen Netzwerken über die Peeringkonfiguration (also über den Azure-Backbone).

Wenn virtuelle Netzwerke mittels Peering verknüpft sind, können die Benutzer das Gateway im verknüpften virtuellen Netzwerk auch als Transitpunkt zu einem lokalen Netzwerk konfigurieren. In diesem Fall kann das virtuelle Netzwerk, das ein Remotegateway verwendet, kein eigenes Gateway besitzen. Ein virtuelles Netzwerk kann nur ein einzelnes Gateway besitzen. Bei diesem Gateway kann es sich entweder um ein lokales Gateway oder um ein Remotegateway (im mittels Peering verknüpften virtuellen Netzwerk) handeln, wie im folgenden Bild zu sehen ist:

![VNet-Peering – Transit](./media/virtual-networks-peering-overview/figure02.png)

Gatewaytransit wird in der Peeringbeziehung zwischen virtuellen Netzwerken, die mit unterschiedlichen Bereitstellungsmodellen erstellt wurden, nicht unterstützt. Sie müssen beide virtuellen Netzwerke in der Peeringbeziehung mit dem Resource Manager-Bereitstellungsmodell erstellen, um den Gatewaytransit verwenden zu können.

Wenn die virtuellen Netzwerke, die gemeinsam eine einzelne ExpressRoute-Verbindung nutzen, mittels Peering verknüpft sind, fließt der Datenverkehr zwischen ihnen über die Peeringbeziehung (also über das Azure-Backbonenetzwerk). Sie können in den einzelnen virtuellen Netzwerken weiterhin lokale Gateways verwenden, um eine Verbindung mit der lokalen Umgebung herzustellen. Alternativ können Sie ein gemeinsam genutztes Gateway verwenden und den Transit für lokale Konnektivität konfigurieren.

## <a name="provisioning"></a>Bereitstellung
VNet-Peering ist ein privilegierter Vorgang. Es ist eine separate Funktion des VirtualNetworks-Namespace. Einem Benutzer können bestimmte Rechte für die Autorisierung des Peerings erteilt werden. Ein Benutzer mit Lese-/Schreibzugriff auf das virtuelle Netzwerk erbt diese Rechte automatisch.

Ein Administrator oder privilegierter Benutzer der Peeringfunktion kann einen Peeringvorgang für ein anderes virtuelles Netzwerk initiieren. Falls eine übereinstimmende Anforderung für das Peering auf der anderen Seite vorhanden ist und weitere Anforderungen erfüllt sind, kommt das Peering zustande.

Informationen zum Einrichten des VNet-Peerings zwischen zwei virtuellen Netzwerken finden Sie in den Artikeln im Abschnitt [Nächste Schritte](#next-steps).

## <a name="limits"></a>Einschränkungen
Pro virtuellem Netzwerk ist nur eine begrenzte Anzahl von Peerings zulässig. Weitere Informationen finden in den [netzwerkspezifischen Grenzwerten](../azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Preise
Für ein- und ausgehenden Datenverkehr, der VNet-Peering verwendet, fällt eine geringe Gebühr an. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Nächste Schritte
Hier erhalten Sie weitere Informationen zum Erstellen eines VNET-Peerings mit:

* [Azure-Portal](virtual-networks-create-vnetpeering-arm-portal.md)
* [Azure PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
* [Azure Resource Manager-Vorlage](virtual-networks-create-vnetpeering-arm-template-click.md)



<!--HONumber=Feb17_HO1-->


