
<properties
   pageTitle="Peering in virtuellen Azure-Netzwerken | Microsoft Azure"
   description="Enthält Informationen zum VNet-Peering in Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="narayan" />

# VNet-Peering

VNet-Peering ist ein Mechanismus, mit dem zwei virtuelle Netzwerke in der gleichen Region über das Azure-Backbonenetzwerk miteinander verbunden werden können. Nach dem Peering werden die beiden virtuellen Netzwerke für alle Verbindungszwecke als einzelnes Element angezeigt. Sie werden zwar weiterhin als separate Ressourcen verwaltet, virtuelle Computer in diesen virtuellen Netzwerken können aber über private IP-Adressen direkt miteinander kommunizieren.

Der Datenverkehr zwischen virtuellen Computern in den virtuellen Netzwerken, die mittels Peering verknüpft sind, wird ähnlich wie der Datenverkehr zwischen virtuellen Computern im gleichen Netzwerk über die Azure-Infrastruktur geleitet. Die Verwendung von VNet-Peering bietet unter anderem folgende Vorteile:

- Niedrige Latenz, Verbindung mit hoher Bandbreite zwischen Ressourcen in unterschiedlichen virtuellen Netzwerken
- Möglichkeit zur Verwendung von Ressourcen wie virtuellen Netzwerkgeräten und VPN-Gateways als Transitpunkte in einem per Peering verknüpften VNet
- Möglichkeit zur Verknüpfung eines virtuellen Netzwerks, das auf dem Azure Resource Manager basiert, mit einem virtuellen Netzwerk, das auf dem klassischen Bereitstellungsmodell basiert, und Nutzung der uneingeschränkten Konnektivität zwischen Ressourcen in diesen virtuellen Netzwerken

Anforderungen und zentrale Aspekte des VNet-Peerings:

- Die beiden mittels Peering verknüpften virtuellen Netzwerke müssen sich in der gleichen Azure-Region befinden.
- Die mittels Peering verknüpften virtuellen Netzwerke müssen über IP-Adressräume ohne Überschneidungen verfügen.
- VNet-Peering erfolgt zwischen zwei virtuellen Netzwerken, und es besteht keine abgeleitete transitive Beziehung. Wenn also beispielsweise das virtuelle Netzwerk A mittels Peering mit dem virtuellen Netzwerk B und das virtuelle Netzwerk B mittels Peering mit dem virtuellen Netzwerk C verknüpft ist, ergibt sich daraus keine Verknüpfung zwischen dem virtuellen Netzwerk A und dem virtuellen Netzwerk C.
- Das Peering kann zwischen virtuellen Netzwerken in zwei verschiedenen Abonnements hergestellt werden, sofern ein privilegierter Benutzer beider Abonnements das Peering autorisiert und die Abonnements dem gleichen Active Directory-Mandanten zugewiesen sind.
- Ein virtuelles Netzwerk, das auf dem Resource Manager-Bereitstellungsmodell basiert, kann mittels Peering mit einem anderen virtuellen Netzwerk verknüpft werden, das ebenfalls auf diesem Modell oder aber auf dem klassischen Bereitstellungsmodell basiert. Virtuelle Netzwerke, die auf dem klassischen Bereitstellungsmodell basieren, können hingegen nicht untereinander mittels Peering verknüpft werden.
- Für die Kommunikation zwischen virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken bestehen zwar keine weiteren Bandbreiteneinschränkungen, es gilt jedoch die auf der VM-Größe basierende Bandbreitenobergrenze.


![Einfaches VNet-Peering](./media/virtual-networks-peering-overview/figure01.png)

## Konnektivität
Nachdem zwei virtuelle Netzwerke mittels Peering verknüpft wurden, kann ein virtueller Computer (Web-/Workerrolle) im virtuellen Netzwerk eine direkte Verbindung mit anderen virtuellen Computern des mittels Peering verknüpften virtuellen Netzwerks herstellen. Diese beiden Netzwerke verfügen über uneingeschränkte Konnektivität auf IP-Ebene.

Die Netzwerklatenz für einen Roundtrip zwischen zwei virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken unterscheidet sich nicht von der Latenz für einen Roundtrip innerhalb eines lokalen virtuellen Netzwerks. Der Netzwerkdurchsatz basiert auf der Bandbreite, die für den virtuellen Computer proportional zu seiner Größe zulässig ist. Weitere Bandbreiteneinschränkungen bestehen nicht.

Der Datenverkehr zwischen den virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken wird nicht über ein Gateway, sondern direkt über die Back-End-Infrastruktur von Azure geleitet.

Virtuelle Computer in einem virtuellen Netzwerk können im mittels Peering verknüpften virtuellen Netzwerk auf die Endpunkte mit internem Lastenausgleich (Internal Load Balancing, ILB) zugreifen. Netzwerksicherheitsgruppen (NSGs) können bei Bedarf in beiden virtuellen Netzwerken angewendet werden, um den Zugriff auf andere virtuelle Netzwerke oder Subnetze zu blockieren.

Beim Konfigurieren von Peering können die Benutzer die NSG-Regeln zwischen den virtuellen Netzwerken öffnen oder schließen. Wenn sich der Benutzer für das Öffnen der vollständigen Konnektivität zwischen den mittels Peering verknüpften virtuellen Netzwerken entscheidet (Standardoption), können NSGs in bestimmten Subnetzen oder auf virtuellen Computern verwendet werden, um den Zugriff jeweils spezifisch zu blockieren oder zu verweigern.

In mittels Peering verknüpften virtuellen Netzwerken funktioniert die von Azure bereitgestellte interne DNS-Namensauflösung für virtuelle Computer nicht. Virtuelle Computer besitzen interne DNS-Namen, die nur im lokalen virtuellen Netzwerk aufgelöst werden können. Benutzer können virtuelle Computer, die in mittels Peering verknüpften virtuellen Netzwerken ausgeführt werden, aber als DNS-Server für ein virtuelles Netzwerk konfigurieren.

## Dienstverkettung
Benutzer können benutzerdefinierte Routingtabellen konfigurieren, die auf virtuelle Computer, welche sich in mittels Peering verknüpften virtuellen Netzwerken befinden, als IP-Adresse für den nächsten Hop verweisen (siehe Diagramm weiter unten in diesem Artikel). So können Benutzer eine Dienstverkettung erzielen, durch die sie Datenverkehr über benutzerdefinierte Routingtabellen aus einem virtuellen Netzwerk an ein virtuelles Gerät leiten können, das in einem mittels Peering verknüpften virtuellen Netzwerk ausgeführt wird.

Benutzer können auch Hub-and-Spoke-Umgebungen erstellen, in denen der Hub Infrastrukturkomponenten wie etwa ein virtuelles Netzwerk-Gerät hosten kann. Alle virtuellen Spoke-Netzwerke können mittels Peering mit dem Hub verknüpft werden. Gleiches gilt für eine Teilmenge des Datenverkehrs an Geräte, die im virtuellen Hub-Netzwerk ausgeführt werden. Kurz gesagt: Beim VNet-Peering kann die IP-Adresse für den nächsten Hop in der benutzerdefinierten Routingtabelle die IP-Adresse eines virtuellen Computers im mittels Peering verknüpften virtuellen Netzwerk sein.

## Gateways und lokale Konnektivität
Jedes virtuelle Netzwerk kann unabhängig davon, ob eine mittels Peering hergestellte Verknüpfung mit einem anderen virtuellen Netzwerk besteht, weiterhin über ein eigenes Gateway verfügen und dieses zum Herstellen einer Verbindung mit der lokalen Umgebung verwenden. Benutzer können auch dann [VNet-zu-VNet-Verbindungen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) unter Verwendung von Gateways konfigurieren, wenn die virtuellen Netzwerke mittels Peering miteinander verknüpft sind.

Wenn beide Optionen für Verbindungen zwischen virtuellen Netzwerken konfiguriert sind, fließt der Datenverkehr zwischen den virtuellen Netzwerken über die Peeringkonfiguration (also über den Azure-Backbone).

Wenn virtuelle Netzwerke mittels Peering verknüpft sind, können die Benutzer auch das Gateway im mittels Peering verknüpften virtuellen Netzwerk als Transitpunkt für die lokale Umgebung konfigurieren. In diesem Fall kann das virtuelle Netzwerk, das ein Remotegateway verwendet, kein eigenes Gateway besitzen. Ein virtuelles Netzwerk kann immer nur ein einzelnes Gateway besitzen. Dabei kann es sich entweder um ein lokales Gateway oder um ein Remotegateway (im mittels Peering verknüpften virtuellen Netzwerk) handeln, wie im folgenden Bild zu sehen.

Gatewaytransit wird in der Peeringbeziehung zwischen virtuellen Netzwerken, die auf dem Resource Manager-Modell basieren, und virtuellen Netzwerken, die auf dem klassischen Bereitstellungsmodell basieren, nicht unterstützt. Beide virtuellen Netzwerke in der Peeringbeziehung müssen auf dem Resource Manager-Bereitstellungsmodell basieren, um den Gatewaytransit verwenden zu können.

Wenn die virtuellen Netzwerke, die gemeinsam eine einzelne ExpressRoute-Verbindung nutzen, mittels Peering verknüpft sind, fließt der Datenverkehr zwischen ihnen über die Peeringbeziehung (also über das Azure-Backbonenetzwerk). Benutzer können in den einzelnen virtuellen Netzwerken weiterhin lokale Gateways verwenden, um eine Verbindung mit der lokalen Umgebung herzustellen. Alternativ können sie ein gemeinsam genutztes Gateway verwenden und den Transit für lokale Konnektivität konfigurieren.

![VNet-Peering – Transit](./media/virtual-networks-peering-overview/figure02.png)

## Bereitstellung
VNet-Peering ist ein privilegierter Vorgang. Es ist eine separate Funktion des VirtualNetworks-Namespace. Einem Benutzer können bestimmte Rechte für die Autorisierung des Peerings erteilt werden. Ein Benutzer mit Lese-/Schreibzugriff auf das virtuelle Netzwerk erbt diese Rechte automatisch.

Ein Administrator oder privilegierter Benutzer der Peeringfunktion kann einen Peeringvorgang für ein anderes virtuelles Netzwerk initiieren. Falls eine übereinstimmende Anforderung für das Peering auf der anderen Seite vorhanden ist und weitere Anforderungen erfüllt sind, kommt das Peering zustande.

Informationen zum Einrichten des VNet-Peerings zwischen zwei virtuellen Netzwerken finden Sie in den Artikeln aus dem Abschnitt „Nächste Schritte“.

## Grenzen
Pro virtuellem Netzwerk ist nur eine begrenzte Anzahl von Peerings zulässig. Weitere Informationen finden in den [netzwerkspezifischen Grenzwerten](../azure-subscription-service-limits.md#networking-limits).

## Preise
Während der Beurteilungsphase fallen für das VNet-Peering keine Kosten an. Nach der Veröffentlichung fällt eine geringe Gebühr für eingehenden und ausgehenden Datenverkehr an, für den das Peering verwendet wird. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/virtual-network).


## Nächste Schritte
- [Einrichten des Peerings zwischen virtuellen Netzwerken](virtual-networks-create-vnetpeering-arm-portal.md)
- Weitere Informationen zu [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)
- Weitere Informationen zu [benutzerdefinierten Routen und IP-Weiterleitung](virtual-networks-udr-overview.md)

<!---HONumber=AcomDC_0928_2016-->