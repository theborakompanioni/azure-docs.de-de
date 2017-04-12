---
title: Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen per Azure CLI | Microsoft-Dokumentation
description: Informationen zum Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer mit Azure CLI | Resource Manager
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5322840700dca3902286f62f62e44bb3746a6148
ms.lasthandoff: 04/03/2017


---
# <a name="load-balancing-on-multiple-ip-configurations"></a>Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)

In diesem Artikel wird beschrieben, wie Sie Azure Load Balancer mit mehreren IP-Adressen an einer sekundären Netzwerkschnittstellenkarte (Network Interface Card, NIC) verwenden. In diesem Szenario verwenden wir zwei virtuelle Computer unter Windows mit jeweils einer primären und einer sekundären NIC. Die sekundären NICs haben jeweils zwei IP-Konfigurationen. Jeder virtuelle Computer hostet die beiden Websites „contoso.com“ und „fabrikam.com“. Jede Website ist an eine der IP-Konfigurationen der sekundären NIC gebunden. Wir verwenden Azure Load Balancer, um zwei Front-End-IP-Adressen verfügbar zu machen (jeweils eine für jede Website), damit der Datenverkehr an die entsprechende IP-Konfiguration für die Website verteilt werden kann. Dieses Szenario verwendet die gleiche Portnummer auf beiden Front-Ends sowie beide Back-End-Pool-IP-Adressen.

![Abbildung zum LB-Szenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Schritte für den Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen

Führen Sie die folgenden Schritte aus, um das in diesem Artikel beschriebene Szenario umzusetzen:

1. [Installieren und konfigurieren Sie die Azure CLI anhand der Schritte im verlinkten Artikel](../cli-install-nodejs.md), und melden Sie sich dann an Ihrem Azure-Konto an.
2. [Erstellen Sie eine Ressourcengruppe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-groups-and-choose-deployment-locations) mit dem Namen *contosofabrikam*, wie oben beschrieben.

    ```azurecli
    azure group create contosofabrikam westcentralus
    ```

3. [Erstellen Sie eine Verfügbarkeitsgruppe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) für die beiden VMs. Führen Sie für dieses Szenario den folgenden Befehl aus:

    ```azurecli
    azure availset create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. [Erstellen Sie ein virtuelles Netzwerk](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) mit dem Namen *myVNet* und ein Subnetz mit dem Namen *mySubnet*:

    ```azurecli
    azure network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus

    azure network vnet subnet create --resource-group contosofabrikam --vnet-name myVnet --name mySubnet --address-prefix 10.0.0.0/24
    ```

5. [Erstellen Sie den Lastenausgleich](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-load-balancer-and-ip-pools) mit dem Namen *mylb*:

    ```azurecli
    azure network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. Erstellen Sie zwei dynamische öffentliche IP-Adressen für die Front-End-IP-Konfigurationen Ihres Lastenausgleichs:

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. Erstellen Sie die beiden Front-End-IP-Konfigurationen *contosofe* und *fabrikamfe*:

    ```azurecli
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. Erstellen Sie die Back-End-Adresspools (*contosopool* und *fabrikampool*), einen [Test (](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-load-balancer-health-probe) - *HTTP*) und die Lastenausgleichsregeln (*HTTPc* und *HTTPf*):

    ```azurecli
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    azure network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. Führen Sie den folgenden Befehl aus, und überprüfen Sie die Ausgabe, um [sicherzustellen, dass der Lastenausgleich richtig erstellt wurde](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#verify-the-load-balancer):

    ```azurecli
    azure network lb show --resource-group contosofabrikam --name mylb
    ```

10. [Erstellen Sie die öffentliche IP](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp* und das [Speicherkonto](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-storage-account), *mystorageaccont1* für den ersten virtuellen Computer VM1. Dies ist hier dargestellt:

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. [Erstellen Sie die Netzwerkschnittstellen](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-nic-to-use-with-the-linux-vm) für VM1, fügen Sie eine zweite IP-Konfiguration (*VM1-ipconfig2*) hinzu, und [erstellen Sie den virtuellen Computer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) wie unten gezeigt:

    ```azurecli
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. Wiederholen Sie die Schritte 10 bis 11 für Ihre zweite VM:

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. Abschließend müssen Sie DNS-Ressourceneinträge konfigurieren, um auf die entsprechende Front-End-IP-Adresse des Lastenausgleichs zu verweisen. Sie können Ihre Domänen in Azure DNS hosten. Weitere Informationen zur Verwendung von Azure DNS mit Lastenausgleich finden Sie unter [Verwenden von Azure DNS mit anderen Azure-Diensten](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie in [Verwenden von Lastenausgleichsdiensten in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md), wie Sie Lastenausgleichsdienste in Azure kombinieren.
- Erfahren Sie in [Log Analytics für den Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md), wie Sie in Azure verschiedene Protokolltypen verwenden, um den Load Balancer zu verwalten und eventuelle Fehler zu beheben.

