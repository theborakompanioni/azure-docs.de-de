---
title: "Erstellen eines internen Lastenausgleichs – Azure CLI | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie in Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle einen internen Load Balancer erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 128b91c685b5f7e494a69ca5b04165a0ee7cbb78
ms.contentlocale: de-de
ms.lasthandoff: 03/21/2017

---

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Erstellen eines internen Load Balancers mithilfe der Azure-Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
> * [Azure-Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Vorlage](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des [klassischen Bereitstellungsmodells](load-balancer-get-started-ilb-classic-cli.md) empfohlen wird.

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Bereitstellen der Lösung mithilfe der Azure-Befehlszeilenschnittstelle

Die folgenden Schritte zeigen, wie Sie einen internen Load Balancer mit Internetzugriff mit Azure Resource Manager und der Befehlszeilenschnittstelle erstellen. Mit dem Azure Resource Manager werden die einzelnen Teilressourcen erstellt sowie individuell konfiguriert und dann zusammengeführt, um eine Ressource zu erstellen.

Zum Bereitstellen eines Load Balancers müssen folgende Objekte erstellt und konfiguriert werden:

* **Front-End-IP-Konfiguration**: Enthält öffentliche IP-Adressen für eingehenden Netzwerkdatenverkehr
* **Back-End-Adresspool**: Enthält Netzwerkschnittstellen (NICs), die virtuellen Computern den Empfang von Netzwerkdatenverkehr des Load Balancers ermöglichen
* **Lastenausgleichsregeln**: Enthält Regeln, die einen öffentlichen Port des Load Balancers einem Port im Back-End-Adresspool zuordnen
* **NAT-Eingangsregeln**: Enthält Regeln, die einen öffentlichen Port des Load Balancers einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool zuordnen
* **Tests**: Enthält Integritätstests zum Prüfen der Verfügbarkeit von VM-Instanzen im Back-End-Adresspool

Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Einrichten der Befehlszeilenschnittstelle für die Verwendung des Resource Managers

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md). Folgen Sie den Anweisungen, bis Sie Ihr Azure-Konto und Ihr Abonnement auswählen müssen.
2. Führen Sie den Befehl **azure config mode** aus, um in den Resource Manager-Modus zu wechseln, wie unten dargestellt:

    ```azurecli
    azure config mode arm
    ```

    Erwartete Ausgabe:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Schritte zum Erstellen eines internen Load Balancers

1. Melden Sie sich bei Azure an.

    ```azurecli
    azure login
    ```

    Geben Sie Ihre Azure-Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

2. Ändern Sie die Befehlstools in den Azure Resource Manager-Modus.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Alle Ressourcen in Azure Resource Manager werden einer Ressourcengruppe zugeordnet. Erstellen Sie eine Ressourcengruppe, wenn Sie noch keine erstellt haben.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Erstellen Sie einen internen Load Balancer.

1. Erstellen eines internen Load Balancers

    Im folgenden Szenario wird die Ressourcengruppe „nrprg“ in der Region „USA, Osten“ erstellt.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Alle Ressourcen für einen internen Load Balancer, wie z.B. virtuelle Netzwerke und Subnetze virtueller Netzwerke, müssen sich in derselben Ressourcengruppe und in derselben Region befinden.

2. Erstellen Sie eine Front-End-IP-Adresse für den internen Load Balancer.

    Die verwendete IP-Adresse muss sich im Subnetzbereich des virtuellen Netzwerks befinden.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Erstellen Sie den Back-End-Adresspool.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Nach der Definition einer Front-End-IP-Adresse und eines Back-End-Adresspools können Sie Load Balancer-Regeln, NAT-Eingangsregeln und angepasste Integritätstests erstellen.

4. Erstellen Sie eine Load Balancer-Regel für den internen Load Balancer.

    Wenn Sie die oben aufgeführten Schritten ausgeführt haben, erstellt der Befehl eine Load Balancer-Regel, die am Port 1433 im Front-End-Pool lauscht und den ausgeglichenen Netzwerkdatenverkehr ebenfalls über Port 1433 zurück an den Back-End-Adresspool sendet.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Erstellen Sie eingehende NAT-Regeln.

    Mit NAT-Eingangsregeln werden Endpunkte in einem Load Balancer erstellt, die für eine bestimmte virtuelle Computerinstanz gelten. In den vorherigen Schritten wurden zwei NAT-Regeln für den Remotedesktop erstellt.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Erstellen Sie Integritätstests für den Load Balancer.

    Ein Integritätstest überprüft alle VM-Instanzen, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. VM-Instanzen mit Fehlern beim Test werden aus dem Load Balancer entfernt, bis sie wieder online geschaltet werden und beim Test überprüft wurde, dass sie fehlerfrei sind.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > Die Microsoft Azure-Plattform nutzt eine statische, öffenlich routingfähige IPv4-Adresse für eine Vielzahl von administrativen Szenarien. Die IP-Adresse lautet 168.63.129.16. Diese IP-Adresse sollte nicht durch Firewalls blockiert werden, da dies zu unerwartetem Verhalten führen kann.
    > In Bezug auf den internen Lastenausgleich in Azure wird diese IP-Adresse von Überwachungstests aus dem Load Balancer verwendet, um den Integritätsstatus von virtuellen Computern in einer Gruppe mit Lastenausgleich zu bestimmen. Wenn eine Netzwerksicherheitsgruppe verwendet wird, um den Datenverkehr auf virtuellen Azure-Computern in einer Gruppe mit internem Lastenausgleich einzuschränken, oder wenn eine Netzwerksicherheitsgruppe einem Subnetz eines virtuellen Netzwerks zugewiesen ist, stellen Sie sicher, dass eine Netzwerksicherheitsregel hinzugefügt wird, um Datenverkehr von 168.63.129.16 zuzulassen.

## <a name="create-nics"></a>Erstellen von NICs

Sie müssen NICs erstellen (oder vorhandene ändern) und diese NAT-Regeln, Load Balancer-Regeln und Tests zuordnen.

1. Erstellen Sie eine NIC mit dem Namen *lb-nic1-be*, und ordnen Sie sie dann der NAT-Regel *rdp1* und dem Back-End-Adresspool *beilb* zu.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
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

2. Erstellen Sie eine NIC mit dem Namen *lb-nic2-be*, und ordnen Sie sie dann der NAT-Regel *rdp2* und dem Back-End-Adresspool *beilb* zu.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Erstellen Sie einen virtuellen Computer mit dem Namen *DB1*, und ordnen Sie ihn dann der NIC mit dem Namen *lb-nic1-be* zu. Ein Speicherkonto namens *web1nrp* wird erstellt, bevor der folgende Befehl ausgeführt wird:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > VMs in einem Load Balancer müssen in derselben Verfügbarkeitsgruppe enthalten sein. Erstellen Sie mit `azure availset create` eine Verfügbarkeitsgruppe.

4. Erstellen Sie einen virtuellen Computer (VM) mit dem Namen *DB2*, und ordnen Sie ihn dann der NIC mit dem Namen *lb-nic2-be* zu. Ein Speicherkonto namens *web1nrp* wurde erstellt, bevor der folgende Befehl ausgeführt wird.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Löschen eines Load Balancers

Verwenden Sie zum Entfernen eines Load Balancers den folgenden Befehl:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Load Balancer-Verteilungsmodus mithilfe der Quell-IP-Affinität](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)


