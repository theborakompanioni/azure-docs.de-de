<properties
	pageTitle="Azure AD-Domänendienste: Netzwerkrichtlinien | Microsoft Azure"
	description="Netzwerkaspekte für Azure Active Directory-Domänendienste"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="maheshu"/>

# Netzwerkaspekte für Azure AD-Domänendienste

## Auswählen eines virtuellen Azure-Netzwerks
Die folgenden Richtlinien dienen Ihnen als Hilfe bei der Auswahl eines virtuellen Netzwerks zur Verwendung mit Azure Active Directory-Domänendiensten.

### Typ des virtuellen Azure-Netzwerks

- Sie können Azure AD-Domänendienste in einem klassischen virtuellen Azure-Netzwerk aktivieren.

- Azure Active Directory-Domänendienste **können nicht in virtuellen Netzwerken aktiviert werden, die mit Azure Resource Manager erstellt wurden**.

- Sie können ein Resource Manager-basiertes virtuelles Netzwerk mit einem klassischen virtuellen Netzwerk verbinden, in dem Azure AD-Domänendienste aktiviert sind. Danach können Sie Azure AD-Domänendienste im Resource Manager-basierten virtuellen Netzwerk verwenden.

- **Regionale virtuelle Netzwerke**: Wenn Sie ein vorhandenes virtuelles Netzwerk verwenden möchten, sollten Sie sicherstellen, dass es sich um ein regionales virtuelles Netzwerk handelt.

    - Virtuelle Netzwerke, die den Vorgängermechanismus der Affinitätsgruppen verwenden, können nicht mit Azure Active Directory-Domänendiensten eingesetzt werden.

	- [Migrieren Sie ältere virtuelle Netzwerke zu regionalen virtuellen Netzwerken](../virtual-network/virtual-networks-migrate-to-regional-vnet.md), um Azure AD-Domänendienste zu verwenden.


### Azure-Region für das virtuelle Netzwerk

- Ihre mit Azure AD-Domänendiensten verwaltete Domäne wird in derselben Azure-Region wie das virtuelle Netzwerk bereitgestellt, das Sie zum Aktivieren des Diensts auswählen.

- Wählen Sie ein virtuelles Netzwerk in einer Azure-Region aus, die über Unterstützung durch Azure AD-Domänendienste verfügt.

- Informationen zu den Azure-Regionen, in denen Azure AD-Domänendienste verfügbar sind, finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services/).


### Anforderungen an das virtuelle Netzwerk

- **Nähe zu Ihren Azure-Workloads**: Wählen Sie das virtuelle Netzwerk aus, in dem virtuelle Computer gehostet werden bzw. gehostet werden sollen, die Zugriff auf die Azure Active Directory-Domänendienste benötigen.

- **Benutzerdefinierte/eigene DNS-Server**: Stellen Sie sicher, dass für das virtuelle Netzwerk keine benutzerdefinierten DNS-Server konfiguriert sind.

- **Vorhandene Domänennamen mit dem gleichen Domänennamen**: Stellen Sie sicher, dass in diesem virtuellen Netzwerk keine Domäne mit dem gleichen Domänennamen vorhanden ist. Angenommen, im ausgewählten virtuellen Netzwerk befindet sich eine Domäne namens „contoso.com“. Später versuchen Sie, in diesem virtuellen Netzwerk eine verwaltete Domäne der Azure AD-Domänendienste mit dem gleichen Domänennamen (also „contoso.com“) zu aktivieren. Beim Aktivieren der Azure AD-Domänendienste tritt daraufhin ein Fehler auf. Der Grund für diesen Fehler ist ein Namenskonflikt in Bezug auf den Domänennamen in diesem virtuellen Netzwerk. In dem Fall müssen Sie einen anderen Namen verwenden, um die verwaltete Domäne der Azure AD-Domänendienste einzurichten. Alternativ können Sie auch die Bereitstellung der bestehenden Domäne aufheben und mit der Aktivierung der Azure AD-Domänendienste fortfahren.

> [AZURE.WARNING] Nach dem Aktivieren des Diensts können Sie keine Domänendienste in ein anderes virtuelles Netzwerk verschieben.


## Netzwerksicherheitsgruppen und Subnetzentwurf
Eine [Netzwerksicherheitsgruppe (NSG)](../virtual-network/virtual-networks-nsg.md) enthält eine Zugriffssteuerungsliste (Access Control List, ACL) zum Zulassen oder Verweigern von Netzwerkdatenverkehr an Ihre VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnetzen oder einzelnen VM-Instanzen innerhalb dieses Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VM-Instanzen in diesem Subnetz. Darüber hinaus kann Datenverkehr zu einem einzelnen virtuellen Computer weiter beschränkt werden, indem eine NSG direkt diesem virtuellen Computer zugewiesen wird.

> [AZURE.NOTE] **Stellen Sie Azure AD-Domänendienste in einem separaten dedizierten Subnetz in Ihrem virtuellen Azure-Netzwerk bereit. Wenden Sie die NSG nicht auf dieses dedizierte Subnetz an. Aktivieren Sie Azure AD-Domänendienste nicht im Gatewaysubnetz Ihres virtuellen Netzwerks.**

![Empfohlener Subnetzentwurf](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

> [AZURE.WARNING] Wenn Sie eine NSG einem Subnetz zuordnen, für das Azure AD-Domänendienste aktiviert sind, kann es passieren, dass die Domäne von Microsoft nicht mehr gewartet und verwaltet werden kann. Darüber hinaus wird die Synchronisierung zwischen Ihrem Azure AD-Mandanten und der verwalteten Domäne beeinträchtigt. **Die Vereinbarung zum Servicelevel (SLA) gilt nicht für Bereitstellungen, bei denen eine NSG auf das Subnetz angewendet wurde, für das Azure AD-Domänendienste aktiviert sind.**


## Netzwerkverbindung
Eine mit Azure AD-Domänendiensten verwaltete Domäne kann nur innerhalb eines einzelnen klassischen virtuellen Netzwerks unter Azure aktiviert werden. Virtuelle Netzwerke, die mit Azure Resource Manager erstellt werden, werden nicht unterstützt.

### Szenarien für die Verbindungsherstellung mit Azure-Netzwerken
Stellen Sie eine Verbindung für virtuelle Azure-Netzwerke zur Verwendung der verwalteten Domäne in den folgenden Bereitstellungsszenarien her:

#### Verwenden der verwalteten Domäne in mehr als einem klassischen virtuellen Azure-Netzwerk
Sie können für andere klassische virtuelle Azure-Netzwerke eine Verbindung mit dem klassischen virtuellen Azure-Netzwerk herstellen, wenn Sie Azure AD-Domänendienste aktiviert haben. Mit dieser Verbindung können Sie die verwaltete Domäne für Ihre Workloads verwenden, die in anderen virtuellen Netzwerken bereitgestellt werden.

![Klassische Konnektivität virtueller Netzwerke](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### Verwenden der verwalteten Domäne in einem Resource Manager-basierten virtuellen Netzwerk
Sie können ein Resource Manager-basiertes virtuelles Netzwerk mit einem klassischen virtuellen Azure-Netzwerk verbinden, in dem Sie Azure AD-Domänendienste aktiviert haben. Mit dieser Verbindung können Sie die verwaltete Domäne für Ihre Workloads verwenden, die in dem Resource Manager-basierten virtuellen Netzwerk bereitgestellt werden.

![Verbindung von Resource Manager zum klassischen virtuellen Netzwerk](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### Netzwerkverbindungsoptionen

- **VNet-zu-VNet-Verbindungen mithilfe von Site-to-Site-VPN-Verbindungen**: Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines virtuellen Netzwerks mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen.

	![Verbindung von virtuellen Netzwerken per VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Weitere Informationen – Verbinden von virtuellen Netzwerken per VPN-Gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **VNet-zu-VNet-Verbindungen per Peering in virtuellen Netzwerken**: Das Peering in virtuellen Netzwerken ist ein Mechanismus, mit dem zwei virtuelle Netzwerke in derselben Region über das Azure-Backbonenetzwerk verbunden werden. Nach dem Peering werden die beiden virtuellen Netzwerke für alle Verbindungszwecke als einzelnes Element angezeigt. Sie werden zwar weiterhin als separate Ressourcen verwaltet, virtuelle Computer in diesen virtuellen Netzwerken können aber über private IP-Adressen direkt miteinander kommunizieren.

    ![Verbindung von virtuellen Netzwerken per Peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

	[Weitere Informationen – Peering in virtuellen Netzwerken](../virtual-network/virtual-network-peering-overview.md)



<br>

## Verwandte Inhalte

- [Peering in virtuellen Azure-Netzwerken](../virtual-network/virtual-network-peering-overview.md)

- [Konfigurieren einer VNet-zu-VNet-Verbindung für das klassische Bereitstellungsmodell](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure-Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)

<!---HONumber=AcomDC_0921_2016-->