---
title: "Erstellen eines Lastenausgleichs für den Internetzugriff – Azure CLI | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen Load Balancer mit Internetzugriff in Ressourcen-Manager über die Azure-Befehlszeilenschnittstelle (CLI) erstellen."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: 002fa917e0cc17c34e0e617a28f2e21ba9b7e35f

---
# <a name="creating-an-internet-load-balancer-using-the-azure-cli"></a>Erstellen eines Internet-Load Balancers mithilfe der Azure-Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Vorlage](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie können auch erfahren, wie Sie [mithilfe klassischer Bereitstellung einen Load Balancer mit Internetzugriff erstellen](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Bereitstellen der Lösung mithilfe der Azure-Befehlszeilenschnittstelle

Die folgenden Schritte zeigen, wie Sie einen internen Load Balancer mit Internetzugriff mit dem Azure Resource Manager und CLI erstellen. Mit dem Azure Resource Manager werden die einzelnen Teilressourcen erstellt sowie individuell konfiguriert und dann zusammengeführt, um eine Ressource zu erstellen.

Zum Bereitstellen eines Load Balancers müssen Sie die folgenden Objekte erstellen und konfigurieren:

* Front-End-IP-Konfiguration: Enthält öffentliche IP-Adressen für eingehenden Netzwerkdatenverkehr.
* Back-End-Adresspool: Enthält Netzwerkschnittstellen (NICs), die virtuellen Computern den Empfang von Netzwerkdatenverkehr des Load Balancers ermöglichen.
* Lastenausgleichsregeln: Enthält Regeln für das Zuordnen eines öffentlichen Ports des Load Balancers zu einem Port im Back-End-Adresspool.
* NAT-Eingangsregeln: Enthält Regeln für das Zuordnen eines öffentlichen Ports des Load Balancers zu einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool.
* Tests: Enthält Integritätstests zum Prüfen der Verfügbarkeit von VM-Instanzen im Back-End-Adresspool.

Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Einrichten der Befehlszeilenschnittstelle für die Verwendung des Resource Managers

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nie verwendet haben, ziehen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) zurate, und folgen Sie den Anweisungen bis zu dem Punkt, an dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **azure config mode** aus, um in den Ressourcen-Manager-Modus zu wechseln, wie unten dargestellt.

    ```azurecli
        azure config mode arm
    ```

    Erwartete Ausgabe:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen eines virtuellen Netzwerks und einer öffentlichen IP-Adresse für den Front-End-IP-Adresspool

1. Erstellen Sie ein virtuelles Netzwerk (VNET) mit dem Namen *NRPVnet* in der Region „USA, Osten“ mithilfe einer Ressourcengruppe mit dem Namen *NRPRG*.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    Erstellen Sie ein Subnetz mit dem Namen *NRPVnetSubnet* mit dem CIDR-Block 10.0.0.0/24 in *NRPVnet*.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. Erstellen Sie eine öffentliche IP-Adresse mit dem Namen *NRPPublicIP*, die von einem Front-End-IP-Adresspool verwendet werden soll, mit dem DNS-Namen *loadbalancernrp.eastus.cloudapp.azure.com*. Der folgende Befehl verwendet den statische Zuordnungstyp und ein Leerlaufzeitlimit von 4 Minuten.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > Der Load Balancer verwendet den Domänennamen der öffentlichen IP-Adresse als seinen vollqualifizierten Domänennamen (FQDN). Dies ist eine Abkehr von der klassischen Bereitstellung, bei der der Clouddienst als vollqualifizierter Domänenname (FQDN) des Load Balancers verwendet wird.
   > In diesem Beispiel lautet der FQDN *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers

Der folgende Befehl erstellt einen Load Balancer namens *NRPlb* in der Ressourcengruppe *NRPRG* in der Azure-Region *USA, Osten*.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Erstellen eines Front-End-IP-Pools und eines Back-End-Adresspools
Im folgenden Beispiel wird der Front-End-IP-Adresspool erstellt, der den eingehenden Netzwerkdatenverkehr für den Load Balancer empfängt. Außerdem wird der Back-End-IP-Pool erstellt, an den der Front-End-Adresspool den einem Lastenausgleich unterzogenen Netzwerkdatenverkehr sendet.

1. Erstellen Sie einen Front-End-IP-Adresspool, der die im vorherigen Schritt erstellte öffentliche IP-Adresse dem Load Balancer zuordnet.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Richten Sie einen Back-End-Adresspool für den Empfang des eingehenden Datenverkehrs vom Front-End-IP-Adresspool ein.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>Erstellen von Lastenausgleichs- und NAT-Regeln sowie Tests

In diesem Beispiel werden die folgenden Elemente erstellt:

* eine NAT-Regel, um sämtlichen an Port 21 eingehenden Datenverkehr für Port 22<sup>1</sup> zu übersetzen.
* eine NAT-Regel, um sämtlichen, bei Port 23 eingehenden Datenverkehr für Port 22 zu übersetzen.
* eine Load Balancer-Regel für die gleichmäßige Verteilung des gesamten an Port 80 eingehenden Datenverkehrs an Port 80 der Adressen im Back-End-Pool.
* eine Testregel, die den Integritätsstatus der Seite *HealthProbe.aspx*überprüft.

<sup>1</sup> NAT-Regeln sind einer bestimmten Instanz eines virtuellen Computers hinter dem Load Balancer zugeordnet. Der an Port 21 eingehende Netzwerkdatenverkehr wird an eine bestimmte VM an Port 22 gesendet, die mit dieser NAT-Regel zugeordnet wird. Sie müssen ein Protokoll (UDP oder TCP) für eine NAT-Regel angeben. Einem Port kann jeweils nur eines der beiden Protokolle zugewiesen werden.

1. Erstellen Sie die NAT-Regeln.

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Erstellen Sie eine Load Balancer-Regel.

    ```azurecli
        azure network lb rule create --resource-group nrprg nrplb --lb-name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Erstellen Sie einen Integritätstest.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Überprüfen Sie Ihre Einstellungen.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Erwartete Ausgabe:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Erstellen von NICs

Sie müssen NICs erstellen (oder vorhandene ändern) und diese NAT-Regeln, Load Balancer-Regeln und Tests zuordnen.

1. Erstellen Sie eine NIC mit dem Namen *lb-nic1-be*, und ordnen Sie sie der NAT-Regel *rdp1* und dem Back-End-Adresspool *NRPbackendpool* zu.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    Erwartete Ausgabe:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Erstellen Sie eine NIC mit dem Namen *lb-nic2-be*, und ordnen Sie sie der NAT-Regel *rdp2* und dem Back-End-Adresspool *NRPbackendpool* zu.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. Erstellen Sie einen virtuellen Computer (VM) mit dem Namen *web1*, und ordnen Sie ihn der NIC mit dem Namen *lb-nic1-be* zu. Ein Speicherkonto namens *web1nrp* wurde erstellt, bevor der folgende Befehl ausgeführt wurde.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > VMs in einem Load Balancer müssen in derselben Verfügbarkeitsgruppe enthalten sein. Erstellen Sie mit `azure availset create` eine Verfügbarkeitsgruppe.

    Die Ausgabe sollte in etwa wie folgt aussehen:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > Die Informationsmeldung **Dies ist eine NIC ohne konfigurierte öffentliche IP-Adresse** wird erwartet, da sich die für den Load Balancer erstellte NIC über die öffentliche IP-Adresse des Load Balancers mit dem Internet verbindet.

    Da die NIC *lb-nic1-be* der NAT-Regel *rdp1* zugeordnet ist, können Sie über RDP an Port 3441 auf dem Load Balancer eine Verbindung mit *web1* herstellen.

4. Erstellen Sie einen virtuellen Computer (VM) mit dem Namen *web2*, und ordnen Sie ihn der NIC mit dem Namen *lb-nic2-be* zu. Ein Speicherkonto namens *web1nrp* wurde erstellt, bevor der folgende Befehl ausgeführt wurde.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Aktualisieren eines vorhandenen Load Balancers
Sie können Regeln hinzufügen, die auf einen vorhandenen Load Balancer verweisen. Im nächsten Beispiel wird dem vorhandenen Load Balancer **NRPlb**

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Löschen eines Load Balancers
Verwenden Sie den folgenden Befehl zum Entfernen eines Load Balancers:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Nächste Schritte
[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Jan17_HO4-->


