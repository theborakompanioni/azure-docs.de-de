---
title: Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen | Microsoft-Dokumentation
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
ms.date: 11/28/2016
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: 8761e32f22471e00e33605916fc145881c8fe378
ms.openlocfilehash: fed40bd97114a2f4eafedc5587b4744d73b2a0ff

---

# <a name="load-balancing-on-multiple-ip-configurations"></a>Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-multiple-ip.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](load-balancer-multiple-ip-cli.md)
>

In diesem Artikel wird beschrieben, wie Sie Azure Load Balancer mit mehreren IP-Adressen pro virtueller Netzwerkschnittstellenkarte (Network Interface Card, NIC) verwenden. Die Unterstützung für mehrere IP-Adressen auf einer NIC ist eine Funktion, die sich derzeit noch in der Vorschauphase befindet. Weitere Informationen finden Sie im Abschnitt [Einschränkungen](#limitations) in diesem Artikel. Das folgende Szenario veranschaulicht, wie dieses Feature mit Load Balancer funktioniert.

In diesem Szenario verwenden wir zwei virtuelle Computer unter Windows mit jeweils einer NIC. Jede NIC verfügt über Konfigurationen mit mehreren IP-Adressen. Jeder virtuelle Computer hostet beide Websites für „contoso.com“ und „fabrikam.com“. Jede Website ist an eine der IP-Konfigurationen auf der NIC gebunden. Wir verwenden Load Balancer, um zwei Front-End-IP-Adressen verfügbar zu machen (jeweils eine für jede Website), um den Datenverkehr an die entsprechende IP-Konfiguration für die Website zu verteilen. Dieses Szenario verwendet die gleiche Portnummer auf beiden Front-Ends sowie beide Back-End-Pool-IP-Adressen.

![Abbildung zum LB-Szenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="limitations"></a>Einschränkungen

Zu diesem Zeitpunkt ist die Konfiguration für den Lastenausgleich für sekundäre IP-Konfigurationen nur mithilfe von Azure PowerShell und der Azure-CLI möglich. Diese Einschränkung ist nicht dauerhaft und kann sich jederzeit ändern. Besuchen Sie diese Seite regelmäßig, um nach Updates zu suchen.

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) , und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an.

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Schritte für den Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen

Führen Sie die folgenden Schritte aus, um das in diesem Artikel beschriebene Szenario umzusetzen:

1. Installieren Sie Azure PowerShell. Unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.
2. Erstellen Sie mithilfe der folgenden Einstellungen eine Ressourcengruppe:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Weitere Informationen finden Sie in Schritt 2 unter [Erstellen einer Ressourcengruppe](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Erstellen Sie eine Verfügbarkeitsgruppe](../virtual-machines/virtual-machines-windows-create-availability-set.md?toc=%2fazure%2fload-balancer%2ftoc.json) mit Ihren virtuellen Computern. Führen Sie für dieses Szenario den folgenden Befehl aus:

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
    $NicName = "VM1-NIC"
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

12. Die unten aufgeführten Befehle rufen die NICs ab und fügen anschließend beide IP-Konfigurationen der NICs zum Back-End-Adresspool des Lastenausgleichs hinzu:

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. Abschließend müssen Sie DNS-Ressourceneinträge konfigurieren, um auf die entsprechende Front-End-IP-Adresse des Lastenausgleichs zu verweisen. Sie können Ihre Domänen in Azure DNS hosten. Weitere Informationen zur Verwendung von Azure DNS mit Lastenausgleich finden Sie unter [Verwenden von Azure DNS mit anderen Azure-Diensten](../dns/dns-for-azure-services.md).



<!--HONumber=Nov16_HO5-->


