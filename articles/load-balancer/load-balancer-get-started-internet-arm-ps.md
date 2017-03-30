---
title: "Erstellen einer Azure-Instanz mit Lastenausgleich für den Internetzugriff – PowerShell | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie im Resource Manager mithilfe von PowerShell einen Load Balancer mit Internetzugriff erstellen.
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: 5abd8365ed883831d4c85ebd14de31dbe45d815d
ms.lasthandoff: 01/24/2017

---

# <a name="get-started"></a>Erstellen eines Load Balancers mit Internetzugriff in Resource Manager unter Verwendung von PowerShell

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Vorlage](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie können auch erfahren, wie Sie [mithilfe des klassischen Bereitstellungsmodells einen Load Balancer mit Internetzugriff erstellen](load-balancer-get-started-internet-classic-cli.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Bereitstellen der Lösung mithilfe von Azure PowerShell

Im Folgenden lernen Sie die Verfahren kennen, mit denen Sie einen Load Balancer mit Internetzugriff mit Azure Resource Manager und PowerShell erstellen. Mit dem Azure Resource Manager werden die einzelnen Teilressourcen erstellt sowie individuell konfiguriert und dann zusammengeführt, um einen Load Balancer zu erstellen.

Zum Bereitstellen eines Load Balancers müssen Sie die folgenden Objekte erstellen und konfigurieren:

* Front-End-IP-Konfiguration: Enthält öffentliche IP-Adressen (PIP) für eingehenden Netzwerkdatenverkehr.
* Back-End-Adresspool: Enthält Netzwerkschnittstellen (NICs), die virtuellen Computern den Empfang von Netzwerkdatenverkehr des Load Balancers ermöglichen.
* Lastenausgleichsregeln: Enthält Regeln, die einen öffentlichen Port des Load Balancers einem Port im Back-End-Adresspool zuordnen.
* NAT-Eingangsregeln: Enthält Regeln, die einen öffentlichen Port des Load Balancers einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool zuordnen.
* Tests: Enthält Integritätstests zum Prüfen der Verfügbarkeit von VM-Instanzen im Back-End-Adresspool.

Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Einrichten von PowerShell für die Verwendung des Resource Managers

Stellen Sie sicher, dass Sie über die neueste Produktionsversion des Azure Resource Manager-Moduls für PowerShell verfügen.

1. Melden Sie sich bei Azure an.

    ```powershell
    Login-AzureRmAccount
    ```

    Geben Sie bei Aufforderung Ihre Anmeldeinformationen ein.

2. Überprüfen Sie die Abonnements für das Konto.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen eines virtuellen Netzwerks und einer öffentlichen IP-Adresse für den Front-End-IP-Adresspool

1. Erstellen Sie ein Subnetz und ein virtuelles Netzwerk.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Erstellen Sie eine öffentliche Azure-IP-Adressressource mit dem Namen **PublicIP** zur Verwendung durch einen Front-End-IP-Pool mit dem DNS-Namen **loadbalancernrp.westus.cloudapp.azure.com**. Der folgende Befehl verwendet den statischen Zuordnungstyp.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > Der Load Balancer verwendet die Domänenbezeichnung der öffentlichen IP-Adresse als Präfix seines vollqualifizierten Domänennamens (FQDN). Dies ist ein Unterschied gegenüber dem klassischen Bereitstellungsmodell, bei dem der Clouddienst als FQDN des Load Balancers verwendet wird.
   > In diesem Beispiel lautet der FQDN **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Erstellen eines Front-End-IP-Adresspools und eines Back-End-Adresspools

1. Erstellen Sie einen Front-End-IP-Pool namens **LB-Frontend**, der die Ressource **PublicIp** verwendet.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. Erstellen Sie einen Back-End-Adresspool namens **LB-backend**.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Erstellen von NAT-Regeln, Lastenausgleichsregel, Test und Load Balancer

In diesem Beispiel werden die folgenden Elemente erstellt:

* eine NAT-Regel, um sämtlichen an Port 3441 eingehenden Datenverkehr für Port 3389 zu übersetzen.
* eine NAT-Regel, um sämtlichen an Port 3442 eingehenden Datenverkehr für Port 3389 zu übersetzen.
* eine Testregel, die den Integritätsstatus der Seite **HealthProbe.aspx**
* eine Load Balancer-Regel für die gleichmäßige Verteilung des gesamten an Port 80 eingehenden Datenverkehrs an Port 80 der Adressen im Back-End-Pool.
* ein Load Balancer, der alle diese Objekte verwendet.

Gehen Sie dazu folgendermaßen vor:

1. Erstellen Sie die NAT-Regeln.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. Erstellen Sie einen Integritätstest. Es gibt zwei Möglichkeiten, einen Test zu konfigurieren:

    HTTP-Test

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TCP-Test

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. Erstellen Sie eine Load Balancer-Regel.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. Erstellen Sie den Load Balancer mithilfe der zuvor erstellten Objekte.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>Erstellen von NICs

Erstellen Sie Netzwerkschnittstellen (oder ändern Sie vorhandene) und ordnen Sie sie den NAT-Regeln, Load Balancer-Regeln und Tests zu:

1. Rufen Sie das virtuelle Netzwerk und das zugehörige virtuelle Subnetz ab, in denen die NICs erstellt werden sollen.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Erstellen Sie eine NIC mit dem Namen **lb-nic1-be**, und ordnen Sie sie der ersten NAT-Regel und dem ersten (und einzigen) Back-End-Adresspool zu.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. Erstellen Sie eine NIC mit dem Namen **lb-nic2-be**, und ordnen Sie sie der zweiten NAT-Regel und dem ersten (und einzigen) Back-End-Adresspool zu.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. Überprüfen Sie die Netzwerkkarten.

        $backendnic1

    Erwartete Ausgabe:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Weisen Sie die Netzwerkkarten mithilfe des Cmdlets `Add-AzureRmVMNetworkInterface` unterschiedlichen virtuellen Computern zu.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Informationen zum Erstellen eines virtuellen Computers und zum Zuweisen einer NIC finden Sie unter [Erstellen eines virtuellen Azure-Computers mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Hinzufügen der Netzwerkschnittstelle zum Load Balancer

1. Rufen Sie den Load Balancer aus Azure ab.

    Laden Sie die Lastenausgleichsressource in eine Variable (sofern nicht schon geschehen). Der Variablenname lautet **$lb**. Verwenden Sie die gleichen Namen wie in der Load Balancer-Ressource, die Sie zuvor erstellt haben.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. Laden Sie die Back-End-Konfiguration in eine Variable.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. Laden Sie die bereits erstellte Netzwerkschnittstelle in eine Variable. Der Variablenname lautet **$nic**. Der Name der Netzwerkschnittstelle entspricht dem im früheren Beispiel.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. Ändern Sie die Back-End-Konfiguration in der Netzwerkschnittstelle.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. Speichern Sie das Netzwerkschnittstellenobjekt.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    Sobald eine Netzwerkschnittstelle zum Back-End-Pool des Load Balancers hinzugefügt ist, empfängt sie Netzwerkdatenverkehr basierend auf den Lastenausgleichsregeln für diese Lastenausgleichsressouce.

## <a name="update-an-existing-load-balancer"></a>Aktualisieren eines vorhandenen Load Balancers

1. Weisen Sie unter Verwendung des Load Balancers aus dem vorherigen Beispiel mithilfe von `Get-AzureLoadBalancer` der Variablen **$slb** ein Load Balancer-Objekt zu.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. Im folgenden Beispiel fügen Sie einem vorhandenen Load Balancer – mithilfe von Port 81 im Front-End-Pool und Port 8181 für den Back-End-Pool – eine NAT-Eingangsregel hinzu.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. Speichern Sie die neue Konfiguration mithilfe von `Set-AzureLoadBalancer`.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>Entfernen eines Load Balancers

Verwenden Sie den Befehl `Remove-AzureLoadBalancer`, um den zuvor erstellten Load Balancer **NRP-LB** aus der Ressourcengruppe **NRP-RG** zu löschen.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Sie können den optionalen Parameter **-Force** verwenden, um die Aufforderung zum Löschen zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)

