---
title: "Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Konfigurieren Sie die benötigte Verbindungsinfrastruktur, um SAP HANA in Azure (große Instanzen) zu verwenden."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 0c87a162eb478aecf207738fd6c9515aa515a847
ms.lasthandoff: 04/03/2017


---
# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen) 

Nachdem der Kauf von SAP HANA in Azure (große Instanzen) zwischen Ihnen und Microsoft Enterprise-Kundenbetreuungsteam abgewickelt wurde, sind die folgenden Informationen erforderlich:

- Kundenname
- Geschäftliche Kontaktinformationen (samt E-Mail-Adresse und Telefonnummer)
- Technische Kontaktinformationen (samt E-Mail-Adresse und Telefonnummer)
- Technische Kontaktinformationen zum Netzwerk (samt E-Mail-Adresse und Telefonnummer)
- Bereitstellungsregion von Azure (USA, Westen oder USA, Osten ab September 2016)
- SKU für SAP HANA in Azure (große Instanzen) (Konfiguration)
- Für jede Azure-Region, in der die Bereitstellung erfolgt:
  - Ein IP-Adressbereich des Typs „/29“ für P2P-Verbindungen
  - Ein CIDR-Block (für den von HANA [große Instanzen] verwendeten NAT-Pool, „/24“ empfohlen)
- Für jedes Azure-VNet, das eine Verbindung mit HANA (große Instanzen) herstellt, unabhängig von der Azure-Region:
  - Ein oder mehrere IP-Adressbereiche des Typs „/28s“ oder „/27“ (für das VNet-Gatewaysubnetz von Kunden)
  - Ein oder mehrere CIDR-Blöcke (für das VNet-Mandantensubnetz von Kunden, „/24“ empfohlen)
- Daten für jedes HANA (große Instanzen)-System:
  - Gewünschter Hostname
  - Gewünschte IP-Adresse aus dem NAT-Pool
- Nummer des Azure-Abonnements, mit dem SAP HANA in Azure (große Instanzen) direkt verbunden wird
- SAP HANA-SID-Name für die SAP HANA-Instanz (zum Erstellen der benötigten SAP HANA-bezogenen Datenträgervolumes erforderlich)

Nachdem Sie die Informationen angegeben haben, wird SAP HANA in Azure (große Instanzen) von Microsoft bereitgestellt.

Sie erhalten Informationen zum Einrichten des Netzwerks für folgende Zwecke:

- Verknüpfen Ihrer Azure-VNets mit der ExpressRoute-Verbindung, über die Azure-VNets mit HANA (große Instanzen) verbunden werden
  - Für Azure Resource Manager:
     - Autorisierungsschlüssel
     - ExpressRoute-Peer-ID
- Zugreifen auf HANA (große Instanzen) über die eingerichtete ExpressRoute-Verbindung und das Azure-VNet

## <a name="creating-an-azure-vnet"></a>Erstellen eines Azure-VNets

Dieses Azure-VNet muss mithilfe des Azure Resource Manager-Bereitstellungsmodells erstellt werden. Das alte allgemein als ASM bekannte Azure-Bereitstellungsmodell wird für diese Lösung nicht unterstützt.

Das Azure-VNet, das erstellt wird, muss mindestens ein Mandantensubnetz und ein Gatewaysubnetz aufweisen. Diesen müssen die angegebenen IP-Adressbereiche zugewiesen werden, die an Microsoft zu übermitteln sind.

> [!IMPORTANT] 
> Dem VNet im Azure-Abonnement dürfen nur Mandanten- und Gatewayadressblöcke zugewiesen werden. P2P- und NAT-Pooladressblöcke müssen von den VNet- und Subnetzadressräumen getrennt sein, da sie sich außerhalb des Azure-Abonnements befinden.

Mehrere Mandantensubnetze können (auch bei Verwendung nicht zusammenhängender Adressbereiche) verwendet werden, doch müssen diese Adressbereiche, wie bereits erwähnt, im Voraus an Microsoft übermittelt werden.

Für diese Mandantensubnetze können Sie einen beliebigen Benennungsstandard wählen. Allerdings **darf es für jedes VNet stets nur ein Gatewaysubnetz geben**, das mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen) verknüpft wird. **Dieses Subnetz muss zudem stets &quot;GatewaySubnet&quot;** genannt werden, um eine ordnungsgemäße Platzierung des ExpressRoute-Gateways sicherzustellen.

> [!WARNING] 
> Es ist wichtig, dass das Gatewaysubnetz immer &quot;GatewaySubnet&quot; heißt.

Das VNet kann mithilfe des Azure-Portals, von PowerShell, der Azure-Vorlage oder Azure CLI erstellt werden (siehe [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).

## <a name="creating-a-gateway-subnet"></a>Erstellen eines Gatewaysubnetzes

Nachdem das Azure-VNet erstellt wurde, muss ein ExpressRoute-Gateway für das VNet erstellt werden, um es mit der ExpressRoute-Verbindung zu verknüpfen, die sich mit dem Kundenmandanten im „Große Instanz“-Stapel verbindet.

> [!NOTE] 
> Dieser Schritt kann bis zu 30 Minuten dauern, da das neue Gateway im vorgesehenen Azure-Abonnement erstellt und anschließend mit dem angegebenen Azure-VNet verbunden wird.

Wenn bereits ein Gateway vorhanden ist, prüfen Sie, ob es ein ExpressRoute-Gateway ist. Falls nicht, muss das Gateway gelöscht und als ExpressRoute-Gateway neu erstellt werden. Wenn bereits ein ExpressRoute-Gateway eingerichtet ist und weil das Azure-VNet für lokale Konnektivität bereits mit der ExpressRoute-Verbindung verknüpft ist, fahren Sie mit dem folgenden Abschnitt „Verknüpfen von VNets“ fort.

- Verwenden Sie entweder das (neue) [Azure-Portal](https://portal.azure.com/) oder PowerShell zum Erstellen eines ExpressRoute-VPN-Gateways, das mit Ihrem VNet verbunden ist.
  - Wenn Sie das Azure-Portal verwenden, fügen Sie ein neues **virtuelles Netzwerkgateway** hinzu und wählen dann **ExpressRoute** als Gatewaytyp aus.
  - Wenn Sie PowerShell verwenden, laden Sie zunächst das neueste [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) herunter, um eine optimale Leistung sicherzustellen. Über die folgenden Befehle erstellen Sie ein ExpressRoute-Gateway. Der Text vor _$_ besteht aus benutzerdefinierten Variablen, die mit Ihren spezifischen Informationen aktualisiert werden müssen.

```
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

In diesem Beispiel wurde die Gateway-SKU „HighPerformance“ verwendet. Ihre Optionen sind „HighPerformance“ oder „UltraPerformance“, die einzigen Gateway-SKUs, die für SAP HANA in Azure (große Instanzen) unterstützt werden.

## <a name="linking-vnets"></a>Verknüpfen von VNets

Nun da das Azure-VNet ein ExpressRoute-Gateway hat, verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen zum Verknüpfen des ExpressRoute-Gateways mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen), die für diesen Zweck erstellt wurde. Dies kann nur mithilfe von PowerShell erfolgen (wird derzeit im Azure-Portal nicht unterstützt).

- Sie führen für jedes VNet-Gateway unter Verwendung einer anderen AuthGUID für jede Verbindung die folgenden Schritte aus. Die ersten beiden unten gezeigten Einträge stammen aus den Informationen, die von Microsoft bereitgestellt werden. Zudem ist die AuthGUID spezifisch für jedes VNet und sein Gateway.

```
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01-5Gb"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Sie müssen diesen Schritt mehrmals ausführen, wenn Sie das Gateway mit mehreren ExpressRoute-Verbindungen verknüpfen möchten, die Ihrem Abonnement zugeordnet sind.

## <a name="adding-more-ip-addresses-or-subnets"></a>Hinzufügen weiterer IP-Adressen oder Subnetze

Verwenden Sie zum Hinzufügen weiterer IP-Adressen und Subnetze das Azure-Portal, PowerShell oder die CLI.

Wenn Sie den zusätzlichen IP-Adressbereich noch nicht mit dem SAP HANA in Azure-Dienstverwaltungsteam abgeklärt haben, beantragen Sie in einer Azure-Supportanforderung dessen Hinzufügung. Führen Sie nach Erhalt einer Bestätigung die nächsten Schritte aus.

Wenn Sie im Azure-Portal ein zusätzliches Subnetz erstellen möchten, lesen Sie den Artikel [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Wie diese Schritte mit PowerShell erfolgen, finden Sie unter [Erstellen eines virtuellen Netzwerks mithilfe von PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="adding-vnets"></a>Hinzufügen von VNets

Nach dem anfänglichen Herstellen von Verbindungen mit einem oder mehreren Azure VNets möchten Sie ggf. weitere hinzufügen, die auf SAP HANA in Azure (große Instanzen) zugreifen. Senden Sie zunächst eine Azure-Supportanforderung, in der Sie sowohl die spezifischen Informationen zur Bestimmung der jeweiligen Azure-Bereitstellung als auch die IP-Adressbereiche für die Mandanten- und Gatewaysubnetze der zusätzlichen Azure VNets angeben. Das SAP HANA in Azure-Dienstverwaltungsteam stellt Ihnen die erforderlichen Informationen bereit, die Sie zum Verbinden der zusätzlichen VNets und von ExpressRoute benötigen.

Schritte zum Hinzufügen eines neuen Azure VNet:

1. Informationen zum ersten Schritt in diesem Prozess finden Sie im Abschnitt _Erstellen eines Azure VNet_ weiter oben.
2. Informationen zum zweiten Schritt in diesem Prozess finden Sie im Abschnitt _Erstellen eines Gatewaysubnetzes_ weiter oben.
3. Öffnen Sie eine Azure-Supportanforderung mit Informationen zum neuen VNet, und fordern Sie einen neuen Autorisierungsschlüssel zum Verknüpfen Ihrer zusätzlichen VNets mit der ExpressRoute-Verbindung von HANA (große Instanzen) an.
4. Nach Erhalt einer Autorisierungsbestätigung verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen zum Ausführen des dritten Schritts in diesem Prozess. Siehe dazu _Verknüpfen von VNets_ weiter oben.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Erhöhen der Bandbreite für die ExpressRoute-Verbindung

Besprechen Sie sich mit dem SAP HANA in Azure-Dienstverwaltungsteam. Falls Ihnen geraten wird, die Bandbreite der ExpressRoute-Verbindung für SAP HANA in Azure (große Instanzen) zu erhöhen, erstellen Sie eine Azure-Supportanforderung. (Sie können eine Erhöhung der Bandbreite einer einzelnen Verbindung um maximal 10 Gbit/s anfordern.) Sie erhalten eine Benachrichtigung, sobald der Vorgang abgeschlossen ist. Es sind keine weiteren Aktionen erforderlich, um diese höhere Geschwindigkeit in Azure zu aktivieren.

## <a name="adding-an-additional-expressroute-circuit"></a>Hinzufügen einer zusätzlichen ExpressRoute-Verbindung

Besprechen Sie sich mit dem SAP HANA in Azure-Dienstverwaltungsteam. Falls Ihnen gesagt wird, dass eine weitere ExpressRoute-Verbindung nötig sei, erstellen Sie eine Supportanforderung zum Erstellen einer weiteren ExpressRoute-Verbindung. Rufen Sie außerdem die entsprechenden Autorisierungsinformationen zum Herstellen einer Verbindung damit auf. Der Adressraum, der in den VNets verwendet wird, muss vor dem Übermitteln der Anforderung definiert werden, damit das SAP HANA in Azure-Dienstverwaltungsteam die Autorisierung erteilen kann.

Nachdem die neue Verbindung erstellt wurde und die SAP HANA in Azure-Dienstverwaltungskonfiguration abgeschlossen ist, erhalten Sie die Benachrichtigung mit den Informationen, die Sie benötigen, um den Vorgang fortzusetzen. Führen Sie die zuvor beschriebenen Schritte zum Erstellen des neuen VNet und zum Herstellen einer Verknüpfung mit dieser zusätzlichen Verbindung durch. Sie können keine Azure VNets mit dieser zusätzlicher Verbindung verknüpfen, wenn diese bereits mit einer anderen ExpressRoute-Verbindung verknüpft sind.

## <a name="deleting-a-subnet"></a>Löschen eines Subnetzes

Zum Entfernen eines VNet-Subnetzes kann das Azure-Portal, PowerShell oder die CLI verwendet werden. Wenn ein Adressbereich entfernt wird, muss das SAP HANA in Azure-Dienstverwaltungsteam über diese Änderung informiert werden, damit dieser Adressbereich aus den Bereichen entfernt wird, mit denen SAP HANA in Azure (große Instanzen) kommunizieren darf.

Es gibt noch keine spezifischen Anleitungen für Azure zum Entfernen von Subnetzen. Doch der Prozess zum Entfernen von Subnetzen ist die Umkehrung des Hinzufügens von Subnetzen. Im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) weitere Informationen zum Erstellen von Subnetzen.

## <a name="deleting-a-vnet"></a>Löschen eines VNet

Sie können ein VNet mithilfe des Azure-Portals, von PowerShell oder der CLI löschen. Das SAP HANA in Azure-Dienstverwaltungsteam entfernt die vorhandenen Autorisierungen für die ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen) und die IP-Adressbereiche (sowohl für den Mandanten als auch das Gateway) für die Kommunikation mit HANA (große Instanzen).

Nachdem das VNet entfernt wurde, beantragen Sie in einer Azure-Supportanforderung das Entfernen der IP-Adressbereiche.

Es gibt noch keine spezifischen Anleitungen für Azure zum Entfernen von VNets. Doch der Prozess zum Entfernen von VNets ist die Umkehrung des zuvor beschriebenen Hinzufügens von VNets. In den Artikeln [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und [Erstellen eines virtuellen Netzwerks mithilfe von PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) finden Sie weitere Informationen zum Erstellen von VNets.

Um sicherzustellen, dass alles entfernt wurde, löschen Sie die folgenden Elemente:

- **Azure Resource Manager:** ExpressRoute-Verbindung, VNet-Gateway, öffentliche IP-Adresse des VNet-Gateways und VNet
- **Klassische VM:** VNet-Gateway und VNet

## <a name="deleting-an-expressroute-circuit"></a>Löschen einer ExpressRoute-Verbindung

Um eine weitere ExpressRoute-Verbindung für SAP HANA in Azure (große Instanzen) zu entfernen, bitten Sie das SAP HANA in Azure-Dienstverwaltungsteam in einer Azure-Supportanforderung, die Verbindung zu löschen. Im Azure-Abonnement können Sie das VNet je nach Anforderung löschen oder beibehalten. Allerdings müssen Sie entweder die Verbindung löschen (bei Verwenden von Azure Resource Manager) oder die Verknüpfung (beim klassischem Modell) zwischen der ExpressRoute-Verbindung von HANA (große Instanzen) und dem verknüpften VNet-Gateway aufheben.

Wenn Sie auch ein VNet entfernen möchten, befolgen Sie die Anleitung zum Löschen eines VNet im vorherigen Abschnitt.



