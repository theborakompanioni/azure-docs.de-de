---
title: Steuern des Routings und Verwenden virtueller Anwendungen mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft Docs
description: Erfahren Sie, wie Sie das Routing und virtuelle Anwendungen mithilfe der Azure-Befehlszeilenschnittstelle steuern.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 8be23107d3a377854a4bd17d99652fb5b84c99dc
ms.openlocfilehash: d134dd27088b29c53a6d3cbe5c3305e0ab45fd11


---
# <a name="create-user-defined-routes-udr-using-the-azure-cli"></a>Erstellen benutzerdefinierter Routen (UDR) mithilfe der Azure-Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
- [PowerShell](virtual-network-create-udr-arm-ps.md)
- [Azure-Befehlszeilenschnittstelle](virtual-network-create-udr-arm-cli.md)
- [Vorlage](virtual-network-create-udr-arm-template.md)
- [PowerShell (klassisch)](virtual-network-create-udr-classic-ps.md)
- [CLI (klassisch)](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: die Bereitstellung mit dem Azure Resource Manager und die klassische Bereitstellung. Stellen Sie sicher, dass Sie die [Bereitstellungsmodelle und -tools](../resource-manager-deployment-model.md) verstanden haben, bevor Sie mit Azure-Ressouren arbeiten. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel.
>

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie haben auch die Möglichkeit, [benutzerdefinierte Routen im klassischen Bereitstellungsmodell zu erstellen](virtual-network-create-udr-classic-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Die folgenden Beispielbefehle für die Azure-Befehlszeilenschnittstelle setzen voraus, dass bereits eine einfache Umgebung erstellt wurde, die auf dem obigen Szenario basiert. Wenn Sie die Befehle so ausführen möchten, wie sie in diesem Dokument angezeigt werden, erstellen Sie zunächst die Testumgebung durch Bereitstellen [dieser Vorlage](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before). Klicken Sie auf **In Azure bereitstellen**, ersetzen Sie bei Bedarf die Standardparameterwerte, und befolgen Sie dann die Anweisungen im Portal.

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen der benutzerdefinierten Route für das Front-End-Subnetz
Führen Sie zum Erstellen der Routingtabelle und der für das Front-End-Subnetz erforderlichen Route anhand des oben beschriebenen Szenarios die folgenden Schritte aus.

1. Führen Sie den folgenden Befehl aus, um eine Routingtabelle für das Front-End-Subnetz zu erstellen:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Ausgabe:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Parameter:
   
   * **-g (or --resource-group)**. Name der Ressourcengruppe, in der die UDR erstellt wird. In diesem Szenario *TestRG*.
   * **-l (oder --location)**. Azure-Region, in der die neue UDR erstellt wird. In diesem Szenario *westus*.
   * **-n (oder --name)**. Name der neuen UDR. In diesem Szenario *UDR-FrontEnd*.
2. Führen Sie den folgenden Befehl aus, um in der Routingtabelle eine Route zu erstellen, die sämtlichen an das Back-End-Subnetz (192.168.2.0/24) gerichteten Datenverkehr an den virtuellen Computer **FW1** (192.168.0.4) umleitet:

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Ausgabe:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Parameter:
   
   * **-r (oder --route-table-name)**. Der Name der Routingtabelle, der die Route hinzugefügt wird. In diesem Szenario *UDR-FrontEnd*.
   * **-a (oder --address-prefix)**. Das Adresspräfix für das Zielsubnetz der Pakete. In diesem Szenario *192.168.2.0/24*.
   * **-y (oder --next-hop-type)**. Der Typ des Zielobjekts für den Datenverkehr. Mögliche Werte sind *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet* oder *None*.
   * **-p (oder --next-hop-ip-address**). Die IP-Adresse für den nächsten Hop. In diesem Szenario *192.168.0.4*.
3. Führen Sie den folgenden Befehl aus, um die oben erstellte Routingtabelle dem Subnetz **FrontEnd** zuzuordnen:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Ausgabe:
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Parameter:
   
   * **-e (oder --vnet-name**. Name des VNets mit dem Subnetz. In diesem Szenario *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen der benutzerdefinierten Route für das Back-End-Subnetz
Führen Sie die folgenden Schritte aus, um die Routingtabelle und die für das Back-End-Subnetz erforderliche Route anhand des oben beschriebenen Szenarios zu erstellen:

1. Führen Sie den folgenden Befehl aus, um eine Routingtabelle für das Back-End-Subnetz zu erstellen:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Führen Sie den folgenden Befehl aus, um in der Routingtabelle eine Route zu erstellen, die sämtlichen an das Front-End-Subnetz (192.168.1.0/24) gerichteten Datenverkehr an den virtuellen Computer **FW1** (192.168.0.4) umleitet:

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Führen Sie den folgenden Befehl aus, um die oben erstellte Routingtabelle dem Subnetz **BackEnd** zuzuordnen:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Aktivieren der IP-Weiterleitung auf "FW1"
Führen Sie die folgenden Schritte aus, um die IP-Weiterleitung in der vom virtuellen Computer **FW1** verwendeten NIC zu aktivieren:

1. Führen Sie den folgenden Befehl aus, und achten Sie auf den Wert für **Enable IP forwarding** (IP-Weiterleitung aktivieren). Er sollte auf *false*festgelegt sein.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Ausgabe:
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Führen Sie den folgenden Befehl aus, um die IP-Weiterleitung zu aktivieren:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Ausgabe:
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Parameter:
   
   * **-f (oder --enable-ip-forwarding)**. *TRUE* oder *FALSE*




<!--HONumber=Nov16_HO3-->


