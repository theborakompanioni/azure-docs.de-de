---
title: "Häufige PowerShell-Befehle für virtuelle Azure-Netzwerke | Microsoft-Dokumentation"
description: "Enthält häufig verwendete PowerShell-Befehle als Einstiegshilfe für die Erstellung eines virtuellen Netzwerks und der zugeordneten Ressourcen für VMs."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 6ddb1915c8073a7b64ba03ff6b90e701ad337c3e
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Häufige PowerShell-Befehle für virtuelle Azure-Netzwerke

Zur Erstellung eines virtuellen Computers müssen Sie ein [virtuelles Netzwerk](../../virtual-network/virtual-networks-overview.md) erstellen oder über ein vorhandenes virtuelles Netzwerk verfügen, dem die VM hinzugefügt werden kann. Normalerweise müssen Sie beim Erstellen einer VM auch die in diesem Artikel beschriebenen Ressourcen erstellen.

Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

Einige Variablen könnten hilfreich sein, wenn Sie mehr als einen der Befehle aus diesem Artikel ausführen:

- $location – Der Speicherort der Netzwerkressourcen. Sie können [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) verwenden, um eine [geografische Region](https://azure.microsoft.com/regions/) zu finden, die für Sie funktioniert.
- $myResourceGroup – Der Name der Ressourcengruppe, in der die Netzwerkressourcen gespeichert sind.

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

| Aufgabe | Befehl |
| ---- | ------- |
| Erstellen von Subnetzkonfigurationen |$subnet1 = [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" -AddressPrefix XX.X.X.X/XX<BR>$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet2" -AddressPrefix XX.X.X.X/XX<BR><BR>Ein typisches Netzwerk kann über ein Subnetz für einen [Load Balancer mit Internetzugriff](../../load-balancer/load-balancer-internet-overview.md) und ein separates Subnetz für einen [internen Load Balancer](../../load-balancer/load-balancer-internal-overview.md) verfügen. |
| Erstellen eines virtuellen Netzwerks |$vnet = [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix XX.X.X.X/XX -Subnet $subnet1, $subnet2 |
| Überprüfen auf einen eindeutigen Domänennamen |[Test-AzureRmDnsAvailability](https://docs.microsoft.com/powershell/module/azurerm.network/test-azurermdnsavailability) -DomainNameLabel "myDNS" -Location $location<BR><BR>Sie können einen DNS-Domänennamen für eine [öffentliche IP-Ressource](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) angeben. Dadurch erstellen Sie für domainname.location.cloudapp.azure.com eine Zuordnung zur öffentlichen IP-Adresse auf den von Azure verwalteten DNS-Servern. Der Name darf nur Buchstaben, Zahlen und Bindestriche enthalten. Das erste und letzte Zeichen muss jeweils ein Buchstabe oder eine Zahl sein, und der Domänenname muss für den Azure-Standort eindeutig sein. Wenn **TRUE** zurückgegeben wird, ist der vorgeschlagene Name global eindeutig. |
| Erstellen einer öffentlichen IP-Adresse |$pip = [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) -Name "myPublicIp" -ResourceGroupName $myResourceGroup -DomainNameLabel "myDNS" -Location $location -AllocationMethod Dynamic<BR><BR>Für die öffentliche IP-Adresse wird der zuvor getestete Domänenname verwendet. Sie wird bei der Front-End-Konfiguration des Load Balancers genutzt. |
| Erstellen einer Front-End-IP-Konfiguration |$frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) -Name "myFrontendIP" -PublicIpAddress $pip<BR><BR>Die Front-End-Konfiguration enthält die öffentliche IP-Adresse, die Sie zuvor für eingehenden Netzwerk-Datenverkehr erstellt haben. |
| Erstellen eines Back-End-Adresspools |$beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) -Name "myBackendAddressPool"<BR><BR>Stellt interne Adressen für das Back-End des Load Balancers bereit, auf die über eine Netzwerkschnittstelle zugegriffen wird. |
| Erstellen eines Tests |$healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) -Name "myProbe" -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2<BR><BR>Enthält Integritätstests, die zum Überprüfen der Verfügbarkeit von Instanzen virtueller Computer im Back-End-Adresspool verwendet werden. |
| Erstellen einer Lastenausgleichsregel |$lbRule = [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80<BR><BR>Enthält Regeln, mit denen ein öffentlicher Port auf dem Load Balancer einem Port im Back-End-Adresspool zugewiesen wird. |
| Erstellen einer eingehenden NAT-Regel |$inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) -Name "myInboundRule1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389<BR><BR>Enthält Regeln, mit denen ein öffentlicher Port auf dem Load Balancer einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool zugeordnet wird. |
| Einrichten eines Load Balancers |$loadBalancer = [New-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) -ResourceGroupName $myResourceGroup -Name "myLoadBalancer" -Location $location -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule -LoadBalancingRule $lbRule -BackendAddressPool $beAddressPool -Probe $healthProbe |
| Erstellen einer Netzwerkschnittstelle |$nic1= [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) -ResourceGroupName $myResourceGroup -Name "myNIC" -Location $location -PrivateIpAddress XX.X.X.X -Subnet $subnet2 -LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] -LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Erstellen Sie eine Netzwerkschnittstelle, indem Sie die öffentliche IP-Adresse und das zuvor erstellte virtuelle Netzwerksubnetz verwenden. |

## <a name="get-information-about-network-resources"></a>Abrufen von Informationen zu Netzwerkressourcen

| Aufgabe | Befehl |
| ---- | ------- |
| Auflisten von virtuellen Netzwerken |[Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) -ResourceGroupName $myResourceGroup<BR><BR>Listet alle virtuellen Netzwerke in der Ressourcengruppe auf. |
| Abrufen von Informationen zu einem virtuellen Netzwerk |Get-AzureRmVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup |
| Auflisten von Subnetzen in einem virtuellen Netzwerk |Get-AzureRmVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup &#124; Select Subnets |
| Abrufen von Informationen zu einem Subnetz |[Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" -VirtualNetwork $vnet<BR><BR>Ruft Informationen zu dem Subnetz im angegebenen virtuellen Netzwerk ab. Der $vnet-Wert gibt das von „Get-AzureRmVirtualNetwork“ zurückgegebene Objekt an. |
| Auflisten von IP-Adressen |[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) -ResourceGroupName $myResourceGroup<BR><BR>Listet die öffentlichen IP-Adressen in der Ressourcengruppe auf. |
| Auflisten von Load Balancern |[Get-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermloadbalancer) -ResourceGroupName $myResourceGroup<BR><BR>Listet alle Load Balancer in der Ressourcengruppe auf. |
| Auflisten von Netzwerkschnittstellen |[Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) -ResourceGroupName $myResourceGroup<BR><BR>Listet alle Netzwerkschnittstellen in der Ressourcengruppe auf. |
| Abrufen von Informationen zu einer Netzwerkschnittstelle |Get-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Ruft Informationen zu einer bestimmten Netzwerkschnittstelle ab. |
| Abrufen der IP-Konfiguration einer Netzwerkschnittstelle |[Get-AzureRmNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterfaceipconfig) -Name "myNICIP" -NetworkInterface $nic<BR><BR>Ruft Informationen zur IP-Konfiguration der angegebenen Netzwerkschnittstelle ab. Der $nic-Wert gibt das von „Get-AzureRmNetworkInterface“ zurückgegebene Objekt an. |

## <a name="manage-network-resources"></a>Verwalten von Netzwerkressourcen

| Aufgabe | Befehl |
| ---- | ------- |
| Hinzufügen eines Subnetzes zu einem virtuellen Netzwerk |[Add-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) -AddressPrefix XX.X.X.X/XX -Name "mySubnet1" -VirtualNetwork $vnet<BR><BR>Fügt einem vorhandenen virtuellen Netzwerk ein Subnetz hinzu. Der $vnet-Wert gibt das von „Get-AzureRmVirtualNetwork“ zurückgegebene Objekt an. |
| Löschen eines virtuellen Netzwerks |[Remove-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup<BR><BR>Entfernt das angegebene virtuelle Netzwerk aus der Ressourcengruppe. |
| Löschen einer Netzwerkschnittstelle |[Remove-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermnetworkinterface) -Name "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Entfernt die angegebene Netzwerkschnittstelle aus der Ressourcengruppe. |
| Löschen eines Load Balancers |[Remove-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermloadbalancer) -Name "myLoadBalancer" -ResourceGroupName $myResourceGroup<BR><BR>Entfernt den angegebenen Load Balancer aus der Ressourcengruppe. |
| Löschen einer öffentlichen IP-Adresse |[Remove-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress)-Name "myIPAddress" -ResourceGroupName $myResourceGroup<BR><BR>Entfernt die angegebene öffentliche IP-Adresse aus der Ressourcengruppe. |

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die soeben erstellte Netzwerkschnittstelle beim [Erstellen einer VM](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Erfahren Sie, wie Sie eine [VM mit mehreren Netzwerkschnittstellen erstellen](../../virtual-network/virtual-networks-multiple-nics.md).


