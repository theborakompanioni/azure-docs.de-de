---
title: Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen in Azure | Microsoft-Dokumentation
description: "Lastenausgleich in Konfigurationen mit primären und sekundären IP-Adressen"
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 70cf84918898a65560ecd40d501b5fdc3775b0f6
ms.lasthandoff: 04/27/2017

---

# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen mit PowerShell

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)

In diesem Artikel wird beschrieben, wie Sie Azure Load Balancer mit mehreren IP-Adressen an einer sekundären Netzwerkschnittstellenkarte (Network Interface Card, NIC) verwenden. In diesem Szenario verwenden wir zwei virtuelle Computer unter Windows mit jeweils einer primären und einer sekundären NIC. Die sekundären NICs haben jeweils zwei IP-Konfigurationen. Jeder virtuelle Computer hostet die beiden Websites „contoso.com“ und „fabrikam.com“. Jede Website ist an eine der IP-Konfigurationen der sekundären NIC gebunden. Wir verwenden Azure Load Balancer, um zwei Front-End-IP-Adressen verfügbar zu machen (jeweils eine für jede Website), damit der Datenverkehr an die entsprechende IP-Konfiguration für die Website verteilt werden kann. Dieses Szenario verwendet die gleiche Portnummer auf beiden Front-Ends sowie beide Back-End-Pool-IP-Adressen.

![Abbildung zum LB-Szenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Schritte für den Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen

Führen Sie die folgenden Schritte aus, um das in diesem Artikel beschriebene Szenario umzusetzen:

1. Installieren Sie Azure PowerShell. Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.
2. Erstellen Sie mithilfe der folgenden Einstellungen eine Ressourcengruppe:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Weitere Informationen finden Sie in Schritt 2 unter [Erstellen einer Ressourcengruppe](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Erstellen Sie eine Verfügbarkeitsgruppe](../virtual-machines/windows/create-availability-set.md?toc=%2fazure%2fload-balancer%2ftoc.json) mit Ihren virtuellen Computern. Führen Sie für dieses Szenario den folgenden Befehl aus:

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Führen Sie die Anweisungsschritte 3 bis 5 im Artikel [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) aus, um die Erstellung eines virtuellen Computers mit einer NIC vorzubereiten. Führen Sie Schritt 6.1 aus, und verwenden Sie anstelle von Schritt 6.2 Folgendes:

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Führen Sie anschließend die Schritte 6.3 bis 6.8 unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) aus.

5. Fügen Sie jedem virtuellen Computer eine zweite IP-Konfiguration hinzu. Folgen Sie den Anweisungen im Artikel [Zuweisen von mehreren IP-Adressen zu virtuellen Computern](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add). Verwenden Sie die folgenden Konfigurationseinstellungen:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    Sie müssen in diesem Tutorial den sekundären IP-Konfigurationen keine öffentlichen IPs zuweisen. Bearbeiten Sie den Befehl, um den Teil zur Zuweisung der öffentlichen IP zu entfernen.

6. Führen Sie die Schritte 4 bis 6 dieses Artikels erneut für den zweiten virtuellen Computer (VM2) aus. Ersetzen Sie dabei unbedingt den Namen des virtuellen Computers durch den Namen des zweiten virtuellen Computers. Beachten Sie, dass Sie kein virtuelles Netzwerk für den zweiten virtuellen Computer erstellen müssen. Sie können abhängig von Ihrem Anwendungsfall ein neues Subnetz erstellen.

7. Erstellen Sie zwei öffentliche IP-Adressen, und speichern Sie sie wie gezeigt in den entsprechenden Variablen:

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. Erstellen Sie zwei Front-End-IP-Konfigurationen:

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. Erstellen Sie die Back-End-Adresspools, einen Test und die Lastenausgleichsregeln:

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Erstellen Sie nach der Erstellung dieser Ressourcen den Lastenausgleich:

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. Fügen sie den zweiten Back-End-Adresspool und die Front-End-IP-Konfiguration zum neu erstellten Lastenausgleich hinzu:

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. Die unten aufgeführten Befehle rufen die NICs ab und fügen anschließend beide IP-Konfigurationen der sekundären NICs zum Back-End-Adresspool des Lastenausgleichs hinzu:

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. Abschließend müssen Sie DNS-Ressourceneinträge konfigurieren, um auf die entsprechende Front-End-IP-Adresse des Lastenausgleichs zu verweisen. Sie können Ihre Domänen in Azure DNS hosten. Weitere Informationen zur Verwendung von Azure DNS mit Lastenausgleich finden Sie unter [Verwenden von Azure DNS mit anderen Azure-Diensten](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie in [Verwenden von Lastenausgleichsdiensten in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md), wie Sie Lastenausgleichsdienste in Azure kombinieren.
- Erfahren Sie in [Log Analytics für den Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md), wie Sie in Azure verschiedene Protokolltypen verwenden, um den Load Balancer zu verwalten und eventuelle Fehler zu beheben.

