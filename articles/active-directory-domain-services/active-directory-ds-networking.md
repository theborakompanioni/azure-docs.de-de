---
title: 'Azure AD Domain Services: Netzwerkrichtlinien | Microsoft Docs'
description: "Netzwerkaspekte für die Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0749a73569286daf9bbbe2c4064db472f41d7171


---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Netzwerkaspekte für die Azure AD Domain Services
## <a name="how-to-select-an-azure-virtual-network"></a>Auswählen eines virtuellen Azure-Netzwerks
Die folgenden Richtlinien dienen Ihnen als Hilfe bei der Auswahl eines virtuellen Netzwerks zur Verwendung mit den Azure Active Directory Domain Services.

### <a name="type-of-azure-virtual-network"></a>Typ des virtuellen Azure-Netzwerks
* Sie können die Azure AD Domain Services in einem klassischen virtuellen Azure-Netzwerk aktivieren.
* Die Azure Active Directory Domain Services **können nicht in virtuellen Netzwerken aktiviert werden, die mit Azure Resource Manager erstellt wurden**.
* Sie können ein Resource Manager-basiertes virtuelles Netzwerk mit einem klassischen virtuellen Netzwerk verbinden, in dem die Azure AD Domain Services aktiviert sind. Danach können Sie die Azure AD Domain Services im Resource Manager-basierten virtuellen Netzwerk verwenden. Weitere Informationen finden Sie im Abschnitt [Netzwerkkonnektivität](active-directory-ds-networking.md#network-connectivity).
* **Regionale virtuelle Netzwerke**: Wenn Sie ein vorhandenes virtuelles Netzwerk verwenden möchten, sollten Sie sicherstellen, dass es sich um ein regionales virtuelles Netzwerk handelt.
  
  * Virtuelle Netzwerke, die den Vorgängermechanismus der Affinitätsgruppen verwenden, können nicht mit den Azure Active Directory Domain Services eingesetzt werden.
  * [Migrieren Sie ältere virtuelle Netzwerke zu regionalen virtuellen Netzwerken](../virtual-network/virtual-networks-migrate-to-regional-vnet.md), um die Azure AD Domain Services zu verwenden.

### <a name="azure-region-for-the-virtual-network"></a>Azure-Region für das virtuelle Netzwerk
* Ihre mit den Azure AD Domain Services verwaltete Domäne wird in derselben Azure-Region wie das virtuelle Netzwerk bereitgestellt, das Sie zum Aktivieren des Diensts auswählen.
* Wählen Sie ein virtuelles Netzwerk in einer Azure-Region aus, die über Unterstützung durch die Azure AD Domain Services verfügt.
* Informationen zu den Azure-Regionen, in denen die Azure AD Domain Services verfügbar sind, finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services/).

### <a name="requirements-for-the-virtual-network"></a>Anforderungen an das virtuelle Netzwerk
* **Nähe zu Ihren Azure-Workloads**: Wählen Sie das virtuelle Netzwerk aus, in dem virtuelle Computer gehostet werden bzw. gehostet werden sollen, die Zugriff auf die Azure Active Directory Domain Services benötigen.
* **Benutzerdefinierte/eigene DNS-Server**: Stellen Sie sicher, dass für das virtuelle Netzwerk keine benutzerdefinierten DNS-Server konfiguriert sind.
* **Vorhandene Domänennamen mit dem gleichen Domänennamen**: Stellen Sie sicher, dass in diesem virtuellen Netzwerk keine Domäne mit dem gleichen Domänennamen vorhanden ist. Angenommen, im ausgewählten virtuellen Netzwerk befindet sich eine Domäne namens „contoso.com“. Später versuchen Sie, in diesem virtuellen Netzwerk eine verwaltete Domäne der Azure AD Domain Services mit dem gleichen Domänennamen (also „contoso.com“) zu aktivieren. Beim Aktivieren der Azure AD Domain Services tritt daraufhin ein Fehler auf. Der Grund für diesen Fehler ist ein Namenskonflikt in Bezug auf den Domänennamen in diesem virtuellen Netzwerk. In dem Fall müssen Sie einen anderen Namen verwenden, um die verwaltete Domäne der Azure AD Domain Services einzurichten. Alternativ können Sie auch die Bereitstellung der bestehenden Domäne aufheben und mit der Aktivierung der Azure AD Domain Services fortfahren.

> [!WARNING]
> Nach dem Aktivieren des Diensts können Sie keine Domänendienste in ein anderes virtuelles Netzwerk verschieben.
> 
> 

## <a name="network-security-groups-and-subnet-design"></a>Netzwerksicherheitsgruppen und Subnetzentwurf
Eine [Netzwerksicherheitsgruppe (NSG)](../virtual-network/virtual-networks-nsg.md) enthält eine Zugriffssteuerungsliste (Access Control List, ACL) zum Zulassen oder Verweigern von Netzwerkdatenverkehr an Ihre VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnetzen oder einzelnen VM-Instanzen innerhalb dieses Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VM-Instanzen in diesem Subnetz. Darüber hinaus kann Datenverkehr zu einem einzelnen virtuellen Computer weiter beschränkt werden, indem eine NSG direkt diesem virtuellen Computer zugewiesen wird.

![Empfohlener Subnetzentwurf](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="best-practices-for-choosing-a-subnet"></a>Best Practices zur Auswahl eines Subnetzes
* Stellen Sie Azure AD Domain Services in einem **separaten dedizierten Subnetz** in Ihrem virtuellen Azure-Netzwerk bereit.
* Wenden Sie auf das dedizierte Subnetz für Ihre verwaltete Domäne keine Netzwerksicherheitsgruppen an. Wenn Sie Netzwerksicherheitsgruppen auf das dedizierte Netzwerk anwenden müssen, stellen Sie sicher, dass Sie **keine Ports blockieren, die für den Dienst und zum Verwalten Ihrer Domäne benötigt werden**.
* Schränken Sie die Anzahl verfügbarer IP-Adressen im dedizierten Subnetz für Ihre verwaltete Domäne nicht zu stark ein. Diese Einschränkung verhindert, dass der Dienst zwei Domänencontroller für Ihre verwaltete Domäne zur Verfügung stellt.
* **Aktivieren Sie die Azure AD Domain Services nicht im Gatewaysubnetz** Ihres virtuellen Netzwerks.

> [!WARNING]
> Wenn Sie eine NSG einem Subnetz zuordnen, für das die Azure AD Domain Services aktiviert sind, kann es passieren, dass die Domäne von Microsoft nicht mehr gewartet und verwaltet werden kann. Darüber hinaus wird die Synchronisierung zwischen Ihrem Azure AD-Mandanten und der verwalteten Domäne beeinträchtigt. **Die Vereinbarung zum Servicelevel (SLA) gilt nicht für Bereitstellungen, bei denen eine NSG auf das Subnetz angewendet wurde, mit der das Aktualisieren und Verwalten Ihrer Domäne durch die Azure AD Domain Services blockiert wird.**
> 
> 

### <a name="ports-required-for-azure-ad-domain-services"></a>Erforderliche Ports für die Azure AD Domain Services
Die folgenden Ports werden für Azure AD Domain Services benötigt, um Ihre verwaltete Domäne zu verwalten und zu warten. Stellen Sie sicher, dass diese Ports nicht für das Subnetz blockiert sind, in dem Sie Ihre verwaltete Domäne aktiviert haben.

| Portnummer | Zweck |
| --- | --- |
| 443 |Synchronisierung mit Ihrem Azure AD-Mandanten |
| 3389 |Verwaltung Ihrer Domäne |
| 5986 |Verwaltung Ihrer Domäne |
| 636 |LDAPS-Zugriff (sicheres LDAP) auf Ihre verwaltete Domäne |

## <a name="network-connectivity"></a>Netzwerkverbindung
Eine mit den Azure AD Domain Services verwaltete Domäne kann nur innerhalb eines einzelnen klassischen virtuellen Netzwerks unter Azure aktiviert werden. Virtuelle Netzwerke, die mit Azure Resource Manager erstellt werden, werden nicht unterstützt.

### <a name="scenarios-for-connecting-azure-networks"></a>Szenarien für die Verbindungsherstellung mit Azure-Netzwerken
Stellen Sie eine Verbindung für virtuelle Azure-Netzwerke zur Verwendung der verwalteten Domäne in den folgenden Bereitstellungsszenarien her:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Verwenden der verwalteten Domäne in mehr als einem klassischen virtuellen Azure-Netzwerk
Sie können für andere klassische virtuelle Azure-Netzwerke eine Verbindung mit dem klassischen virtuellen Azure-Netzwerk herstellen, wenn Sie die Azure AD Domain Services aktiviert haben. Mit dieser VPN-Verbindung können Sie die verwaltete Domäne für Ihre Workloads verwenden, die in anderen virtuellen Netzwerken bereitgestellt werden.

![Klassische Konnektivität virtueller Netzwerke](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Verwenden der verwalteten Domäne in einem Resource Manager-basierten virtuellen Netzwerk
Sie können ein Resource Manager-basiertes virtuelles Netzwerk mit einem klassischen virtuellen Azure-Netzwerk verbinden, in dem Sie die Azure AD Domain Services aktiviert haben. Mit dieser Verbindung können Sie die verwaltete Domäne für Ihre Workloads verwenden, die in dem Resource Manager-basierten virtuellen Netzwerk bereitgestellt werden.

![Verbindung von Resource Manager zum klassischen virtuellen Netzwerk](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Netzwerkverbindungsoptionen
* **VNet-zu-VNet-Verbindungen mithilfe von Site-to-Site-VPN-Verbindungen**: Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines virtuellen Netzwerks mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen.
  
    ![Verbindung von virtuellen Netzwerken per VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)
  
    [Weitere Informationen – Verbinden von virtuellen Netzwerken per VPN-Gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* **VNet-zu-VNet-Verbindungen per Peering in virtuellen Netzwerken**: Das Peering in virtuellen Netzwerken ist ein Mechanismus, mit dem zwei virtuelle Netzwerke in derselben Region über das Azure-Backbonenetzwerk verbunden werden. Nach dem Peering werden die beiden virtuellen Netzwerke für alle Verbindungszwecke als einzelnes Element angezeigt. Sie werden zwar weiterhin als separate Ressourcen verwaltet, virtuelle Computer in diesen virtuellen Netzwerken können aber über private IP-Adressen direkt miteinander kommunizieren.
  
    ![Verbindung von virtuellen Netzwerken per Peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)
  
    [Weitere Informationen – Peering in virtuellen Netzwerken](../virtual-network/virtual-network-peering-overview.md)

<br>

## <a name="related-content"></a>Verwandte Inhalte
* [Peering in virtuellen Azure-Netzwerken](../virtual-network/virtual-network-peering-overview.md)
* [Konfigurieren einer VNet-zu-VNet-Verbindung für das klassische Bereitstellungsmodell](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Azure-Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)




<!--HONumber=Nov16_HO3-->


