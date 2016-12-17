---
title: Verschieben eines virtuellen Computers oder einer Rolleninstanz in ein anderes Subnetz
description: Hier erfahren Sie, wie virtuelle Computer und Rolleninstanzen in ein anderes Subnetz verschoben werden.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 264e02fa48486acd7a9701c497c4e1fa95a1ce4e


---
# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Verschieben eines virtuellen Computers oder einer Rolleninstanz in ein anderes Subnetz
Sie können PowerShell verwenden, um Ihre virtuellen Computer im selben virtuellen Netzwerk (VNet) zwischen Subnetzen zu verschieben. Rolleninstanzen können anstatt mit PowerShell mithilfe der CSCFG-Datei verschoben werden.

> [!NOTE]
> Dieser Artikel enthält Informationen, die sich nur auf klassische Azure-Bereitstellungen beziehen.
> 
> 

Gründe für das Verschieben virtueller Computer in ein anderes Subnetz Die Subnetzmigration ist sinnvoll, wenn das ältere Subnetz zu klein ist und aufgrund von vorhandenen virtuellen Computer in diesem Subnetz nicht erweitert werden kann. In diesem Fall können Sie ein neues, größeres Subnetz erstellen und die virtuellen Computer zum neuen Subnetz migrieren. Nach Abschluss der Migration können Sie das alte leere Subnetz löschen.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Verschieben eines virtuellen Computers in ein anderes Subnetz
Führen Sie zum Verschieben eines virtuellen Computers das PowerShell-Cmdlet „Set-AzureSubnet“ aus. Verwenden Sie dabei das unten angegebene Beispiel als Vorlage. Im folgenden Beispiel verschieben wir „TestVM“ vom aktuellen Subnetz in Subnetz-2. Bearbeiten Sie das Beispiel Ihrer Umgebung entsprechend. Hinweis: Jedes Mal, wenn Sie das Update-AzureVM-Cmdlet als Teil einer Prozedur ausführen, wird der virtuelle Computer im Rahmen des Aktualisierungsprozesses neu gestartet.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Wenn Sie eine statische interne private IP-Adresse für Ihren virtuellen Computer angegeben haben, müssen Sie diese Einstellung deaktivieren, bevor Sie den virtuellen Computer in ein neues Subnetz verschieben können. Verwenden Sie in diesem Fall Folgendes:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>So verschieben Sie eine Rolleninstanz in ein anderes Subnetz
Bearbeiten Sie zum Verschieben einer Rolleninstanz die CSCFG-Datei. Im folgenden Beispiel verschieben wir „Role0“ im virtuellen Netzwerk *VNETName* vom aktuellen Subnetz in *Subnetz-2*. Da die Rolleninstanz bereits bereitgestellt wurde, ändern Sie einfach den Subnetznamen in „Subnetz-2“. Bearbeiten Sie das Beispiel Ihrer Umgebung entsprechend.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 



<!--HONumber=Nov16_HO3-->


