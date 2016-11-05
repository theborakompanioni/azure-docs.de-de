---
title: Erstellen eines internen Load Balancers mit der Azure-Befehlszeilenschnittstelle in Resource Manager | Microsoft Docs
description: Erfahren Sie, wie Sie in Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle einen internen Load Balancer erstellen.
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: sewhee

---
# Erstellen eines internen Load Balancers mithilfe der Azure-Befehlszeilenschnittstelle
[!INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

[classic deployment model](load-balancer-get-started-ilb-classic-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## Bereitstellen der Lösung mithilfe der Azure-Befehlszeilenschnittstelle
Die folgenden Schritte zeigen, wie Sie einen internen Load Balancer mit Internetzugriff mit Azure Resource Manager und der Befehlszeilenschnittstelle erstellen. Mit dem Azure Resource Manager werden die einzelnen Teilressourcen erstellt sowie individuell konfiguriert und dann zusammengeführt, um eine Ressource zu erstellen.

Zum Bereitstellen eines Load Balancers müssen folgende Objekte erstellt und konfiguriert werden:

* **Front-End-IP-Konfiguration**: Enthält öffentliche IP-Adressen für eingehenden Netzwerkdatenverkehr
* **Back-End-Adresspool**: Enthält Netzwerkschnittstellen (NICs), die virtuellen Computern den Empfang von Netzwerkdatenverkehr des Load Balancers ermöglichen
* **Lastenausgleichsregeln**: Enthält Regeln, die einen öffentlichen Port des Load Balancers einem Port im Back-End-Adresspool zuordnen
* **NAT-Eingangsregeln**: Enthält Regeln, die einen öffentlichen Port des Load Balancers einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool zuordnen
* **Tests**: Enthält Integritätstests zum Prüfen der Verfügbarkeit von VM-Instanzen im Back-End-Adresspool

Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](load-balancer-arm.md).

## Einrichten der Befehlszeilenschnittstelle für die Verwendung des Resource Managers
1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md). Folgen Sie den Anweisungen, bis Sie Ihr Azure-Konto und Ihr Abonnement auswählen müssen.
2. Führen Sie den Befehl **azure config mode** aus, um in den Resource Manager-Modus zu wechseln, wie unten dargestellt:
   
        azure config mode arm
   
    Erwartete Ausgabe:
   
        info:    New mode is arm

## Schritte zum Erstellen eines internen Load Balancers
1. Melden Sie sich bei Azure an.
   
        azure login
   
    Geben Sie Ihre Azure-Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
2. Ändern Sie die Befehlstools in den Azure Resource Manager-Modus.
   
        azure config mode arm

## Erstellen einer Ressourcengruppe
Alle Ressourcen in Azure Resource Manager werden einer Ressourcengruppe zugeordnet. Erstellen Sie eine Ressourcengruppe, wenn Sie noch keine erstellt haben.

    azure group create <resource group name> <location>

## Erstellen Sie einen internen Load Balancer.
1. Erstellen eines internen Load Balancers
   
    Im folgenden Szenario wird die Ressourcengruppe „nrprg“ in der Region „USA, Osten“ erstellt.
   
        azure network lb create -n nrprg -l eastus
   
   > [!NOTE]
   > Alle Ressourcen für einen internen Load Balancer, wie z.B. virtuelle Netzwerke und Subnetze virtueller Netzwerke, müssen sich in derselben Ressourcengruppe und in derselben Region befinden.
   > 
   > 
2. Erstellen Sie eine Front-End-IP-Adresse für den internen Load Balancer.
   
    Die verwendete IP-Adresse muss sich im Subnetzbereich des virtuellen Netzwerks befinden.
   
        azure network lb frontend-ip create -g nrprg -l ilbset -n feilb -a 10.0.0.7 -e nrpvnetsubnet -m nrpvnet
   
    Verwendete Parameter:
   
   * **-g**: Ressourcengruppe
   * **-l**: Name der Gruppe mit internem Lastenausgleich
   * **-n**: Name der Front-End-IP
   * **-a**: private IP-Adresse innerhalb des Subnetzbereichs
   * **-e**: Subnetzname
   * **-m**: Name des virtuellen Netzwerks
3. Erstellen Sie den Back-End-Adresspool.
   
        azure network lb address-pool create -g nrprg -l ilbset -n beilb
   
    Verwendete Parameter:
   
   * **-g**: Ressourcengruppe
   * **-l**: Name der Gruppe mit internem Lastenausgleich
   * **-n**: Name des Back-End-Adresspools
     
     Nach der Definition einer Front-End-IP-Adresse und eines Back-End-Adresspools können Sie Load Balancer-Regeln, NAT-Eingangsregeln und angepasste Integritätstests erstellen.
4. Erstellen Sie eine Load Balancer-Regel für den internen Load Balancer.
   
    Wenn Sie die oben aufgeführten Schritten ausgeführt haben, erstellt der Befehl eine Load Balancer-Regel, die am Port 1433 im Front-End-Pool lauscht und den ausgeglichenen Netzwerkdatenverkehr ebenfalls über Port 1433 zurück an den Back-End-Adresspool sendet.
   
        azure network lb rule create -g nrprg -l ilbset -n ilbrule -p tcp -f 1433 -b 1433 -t feilb -o beilb
   
    Verwendete Parameter:
   
   * **-g**: Ressourcengruppe
   * **-l**: Name der Gruppe mit internem Lastenausgleich
   * **-n**: Name der Load Balancer-Regel
   * **-p**: Protokoll, das für die Regel verwendet wird
   * **-f**: Port, der auf den eingehenden Netzwerkdatenverkehr im Load Balancer-Front-End lauscht
   * **-b**: Port, der den Netzwerkdatenverkehr im Back-End-Adresspool empfängt
5. Erstellen Sie eingehende NAT-Regeln.
   
    Mit NAT-Eingangsregeln werden Endpunkte in einem Load Balancer erstellt, die für eine bestimmte virtuelle Computerinstanz gelten. In den vorherigen Schritten wurden zwei NAT-Regeln für den Remotedesktop erstellt.
   
        azure network lb inbound-nat-rule create -g nrprg -l ilbset -n NATrule1 -p TCP -f 5432 -b 3389
   
        azure network lb inbound-nat-rule create -g nrprg -l ilbset -n NATrule2 -p TCP -f 5433 -b 3389
   
    Verwendete Parameter:
   
   * **-g**: Ressourcengruppe
   * **-l**: Name der Gruppe mit internem Lastenausgleich
   * **-n**: Name der NAT-Eingangsregel
   * **-p**: Protokoll, das für die Regel verwendet wird
   * **-f**: Port, der auf den eingehenden Netzwerkdatenverkehr im Load Balancer-Front-End lauscht
   * **-b**: Port, der den Netzwerkdatenverkehr im Back-End-Adresspool empfängt
6. Erstellen Sie Integritätstests für den Load Balancer.
   
    Ein Integritätstest überprüft alle VM-Instanzen, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. VM-Instanzen mit Fehlern beim Test werden aus dem Load Balancer entfernt, bis sie wieder online geschaltet werden und beim Test überprüft wurde, dass sie fehlerfrei sind.
   
        azure network lb probe create -g nrprg -l ilbset -n ilbprobe -p tcp -i 300 -c 4
   
    Verwendete Parameter:
   
   * **-g**: Ressourcengruppe
   * **-l**: Name der Gruppe mit internem Lastenausgleich
   * **-n**: Name des Integritätstests
   * **-p**: vom Integritätstest verwendetes Protokoll
   * **-i**: Überprüfungsintervall in Sekunden
   * **-c**: Anzahl der Überprüfungen

    >[AZURE.NOTE] Die Microsoft Azure-Plattform nutzt eine statische, öffenlich routingfähige IPv4-Adresse für eine Vielzahl von administrativen Szenarien. Die IP-Adresse lautet 168.63.129.16. Diese IP-Adresse sollte nicht durch Firewalls blockiert werden, da dies zu unerwartetem Verhalten führen kann. In Bezug auf den internen Lastenausgleich in Azure wird diese IP-Adresse von Überwachungstests aus dem Load Balancer verwendet, um den Integritätsstatus von virtuellen Computern in einer Gruppe mit Lastenausgleich zu bestimmen. Wenn eine Netzwerksicherheitsgruppe verwendet wird, um den Datenverkehr auf virtuellen Azure-Computern in einer Gruppe mit internem Lastenausgleich einzuschränken, oder wenn eine Netzwerksicherheitsgruppe einem Subnetz eines virtuellen Netzwerks zugewiesen ist, stellen Sie sicher, dass eine Netzwerksicherheitsregel hinzugefügt wird, um Datenverkehr von 168.63.129.16 zuzulassen.

## Erstellen von NICs
Sie müssen NICs erstellen (oder vorhandene ändern) und diese NAT-Regeln, Load Balancer-Regeln und Tests zuordnen.

1. Erstellen Sie eine NIC mit dem Namen *lb-nic1-be*, und ordnen Sie sie dann der NAT-Regel *rdp1* und dem Back-End-Adresspool *beilb* zu.
   
        azure network nic create -g nrprg -n lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
   
    Parameter:
   
   * **-g**: Ressourcengruppenname
   * **-n:** Name der NIC-Ressource
   * **--subnet-name**: Name des Subnetzes
   * **--subnet-vnet-name**: Name des virtuellen Netzwerks
   * **-d**: ID der Back-End-Poolressource (beginnt mit /subscription/{subscriptionID/resourcegroups/<Name-der-Ressourcengruppe>/providers/Microsoft.Network/loadbalancers/<Name-des-Load-Balancers>/backendaddresspools/<Name-des-Back-End-Pools>)
   * **-e**: ID der NAT-Regel, die der NIC-Ressource zugeordnet wird (beginnt mit „/subscriptions/####################################/resourceGroups/<Name-der-Ressourcengruppe>/providers/Microsoft.Network/loadBalancers/<Name-des-Load-Balancers>/inboundNatRules/<Name-der-NAT-Regel>“)

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

1. Erstellen Sie eine NIC mit dem Namen *lb-nic2-be*, und ordnen Sie sie dann der NAT-Regel *rdp2* und dem Back-End-Adresspool *beilb* zu.
   
        azure network nic create -g nrprg -n lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
2. Erstellen Sie einen virtuellen Computer mit dem Namen *DB1*, und ordnen Sie ihn dann der NIC mit dem Namen *lb-nic1-be* zu. Ein Speicherkonto namens *web1nrp* wird erstellt, bevor der folgende Befehl ausgeführt wird:
   
        azure vm create --resource-group nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
   
   > [!IMPORTANT]
   > VMs in einem Load Balancer müssen in derselben Verfügbarkeitsgruppe enthalten sein. Erstellen Sie mit `azure availset create` eine Verfügbarkeitsgruppe.
   > 
   > 
3. Erstellen Sie einen virtuellen Computer (VM) mit dem Namen *DB2*, und ordnen Sie ihn dann der NIC mit dem Namen *lb-nic2-be* zu. Ein Speicherkonto namens *web1nrp* wurde erstellt, bevor der folgende Befehl ausgeführt wird.
   
        azure vm create --resource-group nrprg --name DB2 --location eastus --vnet-    name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## Löschen eines Load Balancers
Verwenden Sie zum Entfernen eines Load Balancers den folgenden Befehl:

    azure network lb delete -g nrprg -n ilbset

In diesem Beispiel steht **nrprg** für die Ressourcengruppe und **ilbset** für den Namen des internen Load Balancers.

## Nächste Schritte
[Konfigurieren eines Load Balancer-Verteilungsmodus mithilfe der Quell-IP-Affinität](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_0928_2016-->