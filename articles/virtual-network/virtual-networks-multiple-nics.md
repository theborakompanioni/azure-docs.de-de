---
title: "Erstellen eines virtuellen Computers (VM) (klassisch) mit mehreren Netzwerkkarten über PowerShell | Microsoft Docs"
description: "Erfahren Sie, wie Sie virtuelle Computer mit mehreren Netzwerkkarten über PowerShell erstellen und konfigurieren."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5e91f640ab72fd3a5fffcb0f9d7bac1e9e031249
ms.lasthandoff: 04/03/2017


---
# <a name="create-a-vm-classic-with-multiple-nics"></a>Erstellen eines virtuellen Computers (VM) (klassisch) mit mehreren Netzwerkschnittstellenkarten (NICs)
Sie können virtuelle Computer (VMs) in Azure erstellen und jedem virtuellen Computer mehrere Netzwerkkarten (NICs) zuordnen. Viele virtuelle Netzwerkanwendungen, z.B. Lösungen für die Anwendungsübermittlung und WAN-Optimierung, erfordern mehrere NICs. Mehrere NICs ermöglichen auch die Isolation des Datenverkehrs zwischen NICs.

![Multi-NIC für virtuelle Computer](./media/virtual-networks-multiple-nics/IC757773.png)

Die Abbildung zeigt einen virtuellen Computer mit drei NICs, die jeweils mit einem anderen Subnetz verbunden sind.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung von Resource Manager.

* Die Internet-VIP (klassische Bereitstellungen) wird nur bei der „Standard“-Netzwerkkarte unterstützt. Es gibt nur eine VIP-Adresse, die an die IP-Adresse der Standard-NIC gebunden ist.
* Derzeit werden öffentliche IP-Adressen auf Instanzebene (LPIP) (klassische Bereitstellungen) für Multi-NIC-VMs nicht unterstützt.
* Die interne Reihenfolge der Netzwerkkarten im virtuellen Computer ist zufällig und kann sich auch zwischen Azure-Infrastrukturaktualisierungen ändern. Die IP-Adressen und die zugehörigen Ethernet-MAC-Adressen ändern sich jedoch nicht. Nehmen wir beispielsweise an,**Eth1** hat die IP-Adresse 10.1.0.100 und die MAC-Adresse 00-0D-3A-B0-39-0D. Nach einer Azure-Infrastrukturupdate und einem Neustart kann Eth1 in **Eth2** geändert worden sein, aber die zugehörige IP-Adresse und die MAC-Adresse bleiben gleich. Wenn der Neustart vom Benutzer initiiert wird, bleibt die NIC-Reihenfolge unverändert.
* Die Adressen der einzelnen NICs in den einzelnen virtuellen Computer müssen sich in einem Subnetz befinden, mehrere NICs in einem virtuellen Computer können Adressen zugewiesen werden, die sich im gleichen Subnetz befinden.
* Größe des virtuellen Computers bestimmt die Anzahl der Netzwerkkarten, die Sie für einen virtuellen Computer erstellen können. Bestimmen Sie anhand der Artikel zu [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)- und [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-VM-Größen, wie viele Netzwerkkarten jede VM-Größe unterstützt. 

## <a name="network-security-groups-nsgs"></a>Netzwerksicherheitsgruppen
In einer Ressourcen-Manager-Bereitstellung kann jede Netzwerkkarte in einem virtuellen Computer einer Netzwerksicherheitsgruppe (NSG) zugeordnet werden, einschließlich Netzwerkkarten eines virtuellen Computers, bei dem mehrere Netzwerkkarten aktiviert wurden. Wenn einer NIC eine Adresse innerhalb eines Subnetzes, das einer NSG zugeordnet ist, zugewiesen wird, dann gelten die Regeln der NSG des Subnetzes auch für diese NIC. Sie können nicht nur Subnetze, sondern auch NICs Netzwerksicherheitsgruppen zuordnen.

Wenn ein Subnetz einer NSG zugeordnet ist und eine Netzwerkkarte in diesem Subnetz einzeln einer NSG zugeordnet ist, dann werden die zugehörigen NSG-Regeln in der **Reihenfolge der Flussrichtung** entsprechend der Richtung des Datenverkehrs angewendet, der in die oder aus der Netzwerkkarte übertragen wird:

* **Eingehender Datenverkehr**, dessen Ziel die fragliche Netzwerkkarte ist, durchläuft zunächst das Subnetz. Hierbei werden die NSG-Regeln des Subnetzes angewendet, bevor er an die Netzwerkkarte übergeben wird, wodurch die NSG-Regeln der NIC zum Tragen kommen.
* **Ausgehender Datenverkehr** , dessen Quelle die fragliche Netzwerkkarte ist, durchläuft zunächst die NIC, wobei die NSG-Regeln der NIC angewendet werden, bevor er an das Subnetz übergeben wird, wodurch die NSG-Regeln des Subnetzes zum Tragen kommen.

Hier finden Sie weitere Informationen zu [Netzwerksicherheitsgruppen](virtual-networks-nsg.md) und deren Anwendung auf Grundlage von Zuordnungen zu Subnetzen, virtuellen Computern und Netzwerkkarte.

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>So konfigurieren Sie eine Multi-NIC-VM in einer klassischen Bereitstellung.
Mithilfe der folgenden Anweisungen können Sie einen virtuellen Multi-NIC-Computer mit 3 NICs erstellen: einer Standard-NIC und zwei zusätzlichen NICs. Mit den Konfigurationsschritten wird ein virtueller Computer erstellt, der gemäß dem folgenden Ausschnitt aus der Dienstkonfigurationsdatei konfiguriert wird:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Um die PowerShell-Befehle in diesem Beispiel ausführen zu können, müssen die folgenden Voraussetzungen erfüllt sein.

* Ein Azure-Abonnement.
* Ein konfiguriertes virtuelles Netzwerk. Weitere Informationen zu virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](virtual-networks-overview.md) .
* Die neueste Version von Azure PowerShell wurde heruntergeladen und installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Um einen virtuellen Computer mit mehreren NICs (Multi-NIC-VM) zu erstellen, führen Sie die folgenden Schritte aus, indem Sie jeden Befehl innerhalb einer einzelnen PowerShell-Sitzung eingeben:

1. Wählen Sie ein VM-Image aus der Azure-VM-Image-Galerie aus. Beachten Sie, dass sich die Images häufig ändern und nach Region verfügbar sind. Das im folgenden Beispiel angegebene Image kann sich möglicherweise ändern oder nicht in Ihrer Region verfügbar sein. Geben Sie daher unbedingt das Image an, das Sie benötigen.

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. Erstellen Sie eine Konfiguration für den virtuellen Computer.

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. Erstellen Sie die Standardadministratoranmeldung.

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. Fügen Sie der Konfiguration des virtuellen Computers zusätzliche Netzwerkkarten hinzu.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. Geben Sie das Subnetz und die IP-Adresse für die Standard-NIC an.

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. Erstellen Sie den virtuellen Computer in Ihrem virtuellen Netzwerk.

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > Das virtuelle Netzwerk, das Sie hier angeben, muss bereits vorhanden sein (wie unter den Voraussetzungen angegeben). Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen **MultiNIC-VNet**angegeben.
    >

## <a name="limitations"></a>Einschränkungen
Es gelten die folgenden Einschränkungen, wenn mehrere NICs verwendet werden:

* Multi-NIC-VMs müssen in virtuellen Azure-Netzwerken (VNets) erstellt werden. Andere VMs als VNet-VMs können nicht mit mehreren NICs konfiguriert werden.
* Alle VMs in einer Verfügbarkeitsgruppe müssen entweder mehrere NICs oder eine einzelne NIC verwenden. Eine Mischung aus Multi-NIC-VMs und VMs mit einer einzelnen NIC (Single-NIC-VM) innerhalb einer Verfügbarkeitsgruppe ist nicht zulässig. Die gleichen Regeln gelten für VMs in einem Clouddienst. Multi-NIC-VMs benötigen nicht die gleiche Anzahl von NICs, so lange sie jeweils über mindestens zwei NICs verfügen.
* Eine Single-NIC-VM kann nach ihrer Bereitstellung nicht mit mehreren NICs konfiguriert werden (und umgekehrt). Sie muss gelöscht und neu erstellt werden.

## <a name="secondary-nics-access-to-other-subnets"></a>Zugriff sekundärer Netzwerkschnittstellenkarten (NICs) auf andere Subnetze
Standardmäßig werden sekundäre NICs nicht mit einem Standardgateway konfiguriert, sodass der Datenverkehrsfluss auf den sekundären NICs auf die Grenzen desselben Subnetzes beschränkt ist. Wenn Benutzer die Kommunikation der sekundären NICs mit Adressen außerhalb ihres eigenen Subnetzes zulassen möchten, müssen sie, wie unten beschrieben, der Routingtabelle einen Eintrag hinzufügen, mit dem das Gateway konfiguriert wird.

> [!NOTE]
> Für VMs, die vor Juli 2015 erstellt wurden, ist unter Umständen ein Standardgateway für alle NICs konfiguriert. Das Standardgateway für sekundäre NICs wird erst entfernt, nachdem diese VMs neu gestartet wurden. Bei Betriebssystemen, die das schwache Hostroutingmodell verwenden, z.B. Linux, werden Internetverbindungen ggf. unterbrochen, wenn für eingehenden und ausgehenden Datenverkehr unterschiedliche NICs verwendet werden.
> 

### <a name="configure-windows-vms"></a>Konfigurieren von virtuellen Windows-Computern
Angenommen, Sie haben einen virtuellen Windows-Computer mit zwei Netzwerkkarten, der wie folgt konfiguriert ist:

* IP-Adresse der primären Netzwerkkarte: 192.168.1.4
* IP-Adresse der sekundären Netzwerkkarte: 192.168.2.5

Die IPv4-Routingtabelle für diesen virtuellen Computer sieht wie folgt aus:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Beachten Sie, dass die Standardroute (0.0.0.0) nur für die primäre Netzwerkkarte verfügbar ist. Sie können nicht auf Ressourcen außerhalb des Subnetzes für die sekundäre Netzwerkkarte zugreifen, wie unten dargestellt:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Führen Sie die folgenden Schritte aus, um eine Standardroute für die sekundäre Netzwerkkarte hinzuzufügen:

1. Führen Sie an einer Eingabeaufforderung den folgenden Befehl aus, um die Indexnummer für die sekundäre Netzwerkkarte zu bestimmen:
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. Beachten Sie den zweiten Eintrag in der Tabelle, der in diesem Beispiel den Index 27 aufweist.
3. Führen Sie an einer Eingabeaufforderung den Befehl **route add** aus, wie unten dargestellt. In diesem Beispiel geben Sie 192.168.2.1 als Standardgateway für die sekundäre Netzwerkkarte an:
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. Zum Testen der Verbindung führen Sie an der Eingabeaufforderung von der sekundären Netzwerkkarte aus Ping-Befehle für die verschiedenen Subnetze aus, wie im Beispiel unten dargestellt:
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. Sie können die neu hinzugefügte Route auch in der Routingtabelle überprüfen, wie unten dargestellt:
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurieren von virtuellen Linux-Computern
Da virtuelle Linux-Computer im Standardverhalten schwaches Hostrouting verwenden, wird empfohlen, die sekundäre Netzwerkkarte auf Datenverkehrsflüsse im gleichen Subnetz zu beschränken. Wenn es jedoch in bestimmten Szenarien erforderlich ist, Verbindungen mit Adressen außerhalb des Subnetzes herzustellen, sollten Benutzer das  richtlinienbasierte Routing aktivieren, um sicherzustellen, dass für eingehenden und ausgehenden Datenverkehr dieselbe Netzwerkkarte verwendet wird.

## <a name="next-steps"></a>Nächste Schritte
* Bereitstellen von [MultiNIC-VMs in einem Anwendungsszenario mit 2-Ebenen in einer Ressourcen-Manager-Bereitstellung](virtual-network-deploy-multinic-arm-template.md).
* Bereitstellen von [MultiNIC-VMs in einem Anwendungsszenario mit 2-Ebenen in einer klassischen Bereitstellung](virtual-network-deploy-multinic-classic-ps.md).


