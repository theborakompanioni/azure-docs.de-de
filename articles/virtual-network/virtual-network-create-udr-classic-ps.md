---
title: "Steuern des Routings und Verwenden virtueller Geräte mithilfe von PowerShell Microsoft Docs"
description: Erfahren Sie, wie Sie das Routing in VNets mithilfe von PowerShell steuern. | klassisch
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: ba7a67b8ae57da165f45bd3552a3dfac5f4ef64b
ms.openlocfilehash: 0bf0f3b64319a42a3b693f64ae705756b10be64e


---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Steuern des Routings und Verwenden virtueller Geräte (klassisch) mithilfe von PowerShell

> [!div class="op_single_selector"]
- [PowerShell](virtual-network-create-udr-arm-ps.md)
- [Azure-Befehlszeilenschnittstelle](virtual-network-create-udr-arm-cli.md)
- [Vorlage](virtual-network-create-udr-arm-template.md)
- [PowerShell (klassisch)](virtual-network-create-udr-classic-ps.md)
- [CLI (klassisch)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: die Bereitstellung mit dem Azure Resource Manager und die klassische Bereitstellung. Stellen Sie sicher, dass Sie die [Bereitstellungsmodelle und -tools](../azure-resource-manager/resource-manager-deployment-model.md) verstanden haben, bevor Sie mit Azure-Ressouren arbeiten. Klicken Sie zum Anzeigen der Dokumentation für verschiedene Tools auf eine Option oben in diesem Artikel. Dieser Artikel gilt für das klassische Bereitstellungsmodell.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Die folgenden Beispielbefehle für Azure PowerShell setzen voraus, dass bereits eine einfache Umgebung erstellt wurde, die auf dem zuvor beschriebenen Szenario basiert. Wenn Sie die in diesem Dokument aufgeführten Befehle ohne Veränderungen ausführen möchten, erstellen Sie zunächst die unter [Erstellen eines VNets (klassisch) mithilfe von PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)beschriebene Umgebung.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen der benutzerdefinierten Route für das Front-End-Subnetz
Führen Sie zum Erstellen der Routingtabelle und der für das Front-End-Subnetz erforderlichen Route anhand des oben beschriebenen Szenarios die folgenden Schritte aus.

1. Führen Sie den folgenden Befehl aus, um eine Routingtabelle für das Front-End-Subnetz zu erstellen:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

    Ausgabe:
   
        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet
2. Führen Sie den folgenden Befehl aus, um in der Routingtabelle eine Route zu erstellen, die sämtlichen an das Back-End-Subnetz (192.168.2.0/24) gerichteten Datenverkehr an den virtuellen Computer **FW1** (192.168.0.4) umleitet:

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```
   
    Ausgabe:
   
        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  
3. Führen Sie den folgenden Befehl aus, um die oben erstellte Routingtabelle dem Subnetz **FrontEnd** zuzuordnen:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen der benutzerdefinierten Route für das Back-End-Subnetz
Führen Sie zum Erstellen der Routingtabelle und der für das Back-End-Subnetz erforderlichen Route anhand des Szenarios die folgenden Schritte aus:

1. Führen Sie den folgenden Befehl aus, um eine Routingtabelle für das Back-End-Subnetz zu erstellen:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. Führen Sie den folgenden Befehl aus, um in der Routingtabelle eine Route zu erstellen, die sämtlichen an das Front-End-Subnetz (192.168.1.0/24) gerichteten Datenverkehr an den virtuellen Computer **FW1** (192.168.0.4) umleitet:

    ```powershell
    Get-AzureRouteTable UDR-BackEnd `
    |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Führen Sie den folgenden Befehl aus, um die oben erstellte Routingtabelle dem Subnetz **BackEnd** zuzuordnen:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Aktivieren der IP-Weiterleitung auf dem virtuellen Computer „FW1“

Führen Sie zum Aktivieren der IP-Weiterleitung auf dem virtuellen Computer „FW1“ die folgenden Schritte aus:

1. Führen Sie den folgenden Befehl aus, um den Status der IP-Weiterleitung zu überprüfen:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

    Ausgabe:
   
        Disabled
2. Führen Sie das folgende Cmdlet aus, um die IP-Weiterleitung für den virtuellen Computer *FW1* zu aktivieren:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```



<!--HONumber=Jan17_HO1-->


