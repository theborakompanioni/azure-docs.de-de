<properties
   pageTitle="Azure Virtual Network-Peering | Microsoft Azure"
   description="Enthält Informationen zum VNet-Peering in Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="narayanannamalai"
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

VNet-Peering ist ein Mechanismus zum Verbinden von zwei Virtual Networks in derselben Region über das Azure-Backbonenetzwerk. Nach dem Peering werden die beiden Virtual Networks für alle Verbindungszwecke als ein Element angezeigt. Sie werden weiterhin als separate Ressourcen verwaltet, aber virtuelle Computer in diesen VNets können über die private IP-Adresse direkt miteinander kommunizieren. Der Datenverkehr zwischen virtuellen Computern in den VNets, die per Peering verknüpft sind, wird über die Azure-Infrastruktur geleitet, also ähnlich wie Datenverkehr zwischen VMs in demselben VNet. Einige Vorteile der Verwendung von VNet-Peering:

- Niedrige Latenz, Verbindung mit hoher Bandbreite zwischen Ressourcen in unterschiedlichen VNets
- Möglichkeit zur Verwendung von Ressourcen, z.B. virtuelle Netzwerkgeräte und VPN Gateways in per Peering verknüpften VNets (Transit)
- Verbinden von Resource Manager-VNet mit einem klassischen VNet und Aktivieren der vollständigen Konnektivität zwischen Ressourcen in diesen VNets

Anforderungen und wichtige Aspekte des VNet-Peerings:

- Die beiden Virtual Networks, die per Peering verknüpft werden, müssen sich in derselben Azure-Region befinden.
- Die per Peering verknüpften VNets müssen über sich nicht überlappende IP-Adressräume verfügen.
- VNet-Peering erfolgt zwischen zwei virtuellen Netzwerken, und es ist keine abgeleitete transitive Beziehung vorhanden. Wenn VNet A per Peering mit VNet B verknüpft ist und VNet B per Peering mit VNet C verknüpft ist, bedeutet dies nicht, dass VNet A mit VNet C verknüpft ist.
- Das Peering kann zwischen virtuellen Netzwerken in zwei verschiedenen Abonnements hergestellt werden, sofern der privilegierte Benutzer der jeweiligen Abonnements das Peering autorisiert.
- Ein Resource Manager-VNet kann per Peering mit einem anderen Resource Manager-VNet oder klassischen VNet verknüpft werden, aber für zwei klassische VNets ist dies nicht möglich.
- Obwohl für die Kommunikation zwischen virtuellen Computern in per Peering verknüpften VNets keine weiteren Bandbreiteneinschränkungen bestehen, gilt die auf der VM-Größe basierende Bandbreitenobergrenze.


![VNet-Peering – Einfach](./media/virtual-networks-peering-overview/figure01.png)

## Konnektivität 
Wenn zwei VNets per Peering verknüpft wurden, kann ein virtueller Computer (Web-/Workerrolle) im VNet eine direkte Verbindung mit anderen virtuellen Computern im verknüpften VNet herstellen. Hierbei besteht vollständige Konnektivität auf IP-Ebene. Die Netzwerklatenz für den Roundtrip zwischen zwei virtuellen Computern in per Peering verknüpften VNets entspricht der Netzwerklatenz im lokalen VNet. Der Netzwerkdurchsatz basiert auf der Bandbreite, die für den virtuellen Computer proportional zu seiner Größe zulässig ist. Es besteht keine weitere Einschränkung in Bezug auf die zulässige Bandbreite. Der Datenverkehr zwischen den virtuellen Computern in per Peering verknüpften VNets wird direkt über die Back-End-Infrastruktur von Azure weitergeleitet, und nicht über ein Gateway.

Virtuelle Computer in einem VNet können auf Endpunkte mit internem Lastenausgleich (Internal Load Balancer, ILB) im per Peering verknüpften VNet zugreifen. Netzwerksicherheitsgruppen können in beiden VNets angewendet werden, um den Zugriff auf das jeweils andere VNet oder Subnetz bei Bedarf zu blockieren. Wenn Benutzer das Peering konfigurieren, können sie die Netzwerksicherheitsgruppen-Regeln zwischen den VNets öffnen oder schließen. Wenn sich Benutzer für das Öffnen der vollständigen Konnektivität zwischen den per Peering verknüpften VNets entscheiden (Standardoption), können sie NSGs dann in bestimmten Subnetzen oder auf virtuellen Computern verwenden, um den Zugriff jeweils zu blockieren oder zu verweigern.

Die von Azure bereitgestellte interne DNS-Namensauflösung für virtuelle Computer funktioniert in per Peering verknüpften VNets nicht. Virtuelle Computer verfügen über interne DNS-Namen, die nur im lokalen Virtual Network aufgelöst werden können. Benutzer können virtuelle Computer, die in verknüpften VNets ausgeführt werden, aber als DNS-Server für ein Virtual Network konfigurieren.

## Dienstverkettung
Benutzer können benutzerdefinierte Routingtabellen konfigurieren, die auf virtuelle Computer in per Peering verknüpften VNets als nächsten Hop zeigen (wie im Diagramm unten dargestellt). So können Benutzer eine Dienstverkettung erzielen, mit der sie Datenverkehr über benutzerdefinierte Routingtabellen aus einem VNet an ein virtuelles Gerät leiten können, das in einem per Peering verknüpften VNet ausgeführt wird. Benutzer können auch auf effektive Weise Umgebungen vom Typ „Hub-and-Spoke“ erstellen, wobei der Hub Infrastrukturkomponenten hosten kann, z.B. ein virtuelles Netzwerkgerät. Alle Spoke-VNets können per Peering damit verknüpft werden und eine Teilmenge des Datenverkehrs an Geräte leiten, die im Hub-VNet ausgeführt werden. Kurz gesagt: Beim VNet-Peering kann die IP-Adresse für den nächsten Hop in der „benutzerdefinierten Routingtabelle“ die IP-Adresse eines virtuellen Computers im per Peering verknüpften VNet sein.

## Gateways und lokale Konnektivität
Jedes Virtual Network – ob per Peering mit einem anderen VNet verknüpft oder nicht – kann weiterhin über ein eigenes Gateway verfügen und dieses zum Herstellen einer Verbindung mit der lokalen Umgebung verwenden. Benutzer können die VNet-zu-VNet-Verbindung (Link angeben) auch dann mit Gateways konfigurieren, wenn die VNets per Peering miteinander verknüpft sind. Wenn beide Optionen für die Verbindungen zwischen VNets konfiguriert sind, fließt der Datenverkehr zwischen den VNets über die Peeringkonfiguration (also über den Azure-Backbone).

Wenn VNets per Peering verknüpft sind, können die Benutzer auch konfigurieren, dass das Gateway im verknüpften VNet als Transitpunkt zur lokalen Umgebung verwendet wird. In diesem Fall kann das VNet, für das ein Remotegateway verwendet wird, nicht über ein eigenes Gateway verfügen. Ein VNet kann also nur jeweils ein Gateway haben, und zwar entweder ein lokales Gateway oder ein Remotegateway (im verknüpften VNet). Dies ist in der Abbildung unten dargestellt. Der Gatewaytransit zwischen einem Resource Manager-VNet und einem klassischen VNet wird nicht unterstützt. Beide VNets der Peeringbeziehung sollten Resource Manager-VNets sein, damit der Gatewaytransit funktioniert. Wenn die VNets, die eine einzelne ER-Verbindung gemeinsam nutzen, per Peering verknüpft sind, fließt der Datenverkehr dazwischen über die Peeringbeziehung (also über das Azure-Backbonenetzwerk). Benutzer können weiterhin lokale Gateways in jedem VNet verwenden, um eine Verbindung mit der lokalen Verbindung herzustellen, oder ein gemeinsames Gateway nutzen und den Transit für die lokale Konnektivität konfigurieren.

![VNet-Peering – Transit](./media/virtual-networks-peering-overview/figure02.png)

## Bereitstellung
Das VNet-Peering ist ein privilegierter Vorgang. Es ist eine separate Funktion unter dem Virtual Network-Namespace. Einem Benutzer können bestimmte Rechte für die Autorisierung des Peerings erteilt werden. Ein Benutzer, der im VNet über Lese-/Schreibzugriff verfügt, erbt diese Rechte automatisch. Ein Benutzer, der entweder ein Administrator oder ein privilegierter Benutzer der Peeringfunktion ist, kann einen Peeringvorgang mit einem anderen VNet initiieren. Falls eine übereinstimmende Anforderung für das Peering auf der anderen Seite vorhanden ist und weitere Anforderungen erfüllt sind, wird das Peering eingerichtet.

Lesen Sie sich die Gewusst-wie-Artikel durch, um weitere Informationen zum Einrichten des VNet-Peerings zwischen zwei Virtual Networks zu erhalten.

## Grenzen
Die Anzahl von zulässigen Peerings in einem Virtual Network ist beschränkt. Weitere Informationen finden Sie unter [Azure-Netzwerkbeschränkungen](../azure-subscription-service-limits.md#networking-limits).

## Preise
Während des Vorschauzeitraums werden für das VNet-Peering keine Gebühren berechnet. Nach der Veröffentlichung für die allgemeine Verfügbarkeit fällt eine geringe Gebühr für eingehenden und ausgehenden Datenverkehr an, für den das Peering verwendet wird. Weitere Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/virtual-network).


## Nächste Schritte
- [Einrichten von Peering zwischen Virtual Networks](virtual-networks-create-vnetpeering-arm-portal.md)
- Weitere Informationen zu [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)
- Weitere Informationen zu [benutzerdefinierten Routen und IP-Weiterleitung](virtual-networks-udr-overview.md)

<!---HONumber=AcomDC_0803_2016-->