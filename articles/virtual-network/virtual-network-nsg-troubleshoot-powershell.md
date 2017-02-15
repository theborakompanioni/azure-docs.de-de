---
title: "Problembehandlung bei Netzwerksicherheitsgruppen – PowerShell | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine Problembehandlung bei Netzwerksicherheitsgruppen im Azure Resource Manager-Bereitstellungsmodell mit Azure PowerShell durchführen."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 13ffe90e03dbe99366fb4f6e2788ba7a3c968a30


---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Problembehandlung bei Netzwerksicherheitsgruppen mit Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Wenn Sie Netzwerksicherheitsgruppen (NSGs) auf dem virtuellen Computer (VM) konfiguriert haben und Probleme bei der Verbindung mit dem virtuellen Computer auftreten, bietet dieser Artikel eine Übersicht über Diagnosefunktionen für NSGs zur weiteren Problembehandlung.

Über NSGs können Sie die Typen des Datenverkehrs steuern, der bei den virtuellen Computern ein- und ausgeht. NSGs können auf Subnetze in einem virtuellen Azure-Netzwerk (VNET), auf Netzwerkschnittstellen (NIC) oder auf beide angewendet werden. Die auf eine NIC angewendeten effektiven Regeln sind eine Aggregation der Regeln, die in den auf eine NIC angewendeten NSGs und in dem Subnetz vorhanden sind, mit dem sie verbunden ist. Die Regeln für diese NSGs können manchmal in Konflikt miteinander stehen und sich auf die Netzwerkverbindung eines virtuellen Computers auswirken.  

Sie können alle effektiven Sicherheitsregeln der NSGs anzeigen, die auf NICs der virtuellen Computer angewendet werden. In diesem Artikel wird erläutert, wie Verbindungsprobleme bei virtuellen Computern mithilfe dieser Regeln im Azure Resource Manager-Bereitstellungsmodell behoben werden. Wenn Sie nicht mit dem VNET- und NSG-Konzept vertraut sind, finden Sie entsprechende Informationen in den Übersichtsartikeln zu [virtuellen Netzwerken](virtual-networks-overview.md) und [Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Verwenden von effektiven Sicherheitsregeln zur Problembehandlung des Datenverkehrsflusses auf virtuellen Computern
Das folgende Szenario ist ein Beispiel für ein häufig auftretendes Verbindungsproblem:

Der virtuelle Computer *VM1* gehört zum Subnetz *Subnet1* in einem VNET mit dem Namen *WestUS-VNet1*. Der Versuch, mit RDP über den TCP-Port 3389 eine Verbindung mit dem virtuellen Computer herzustellen, schlägt fehl. NSGs werden auf die NIC *VM1-NIC1* sowie auf das Subnetz *Subnet1* angewendet. Der Datenverkehr an TCP-Port 3389 ist zulässig in der NSG, die der Netzwerkschnittstelle *VM1-NIC1*zugeordnet ist, der TCP-Ping an Port 3389 von VM1 schlägt jedoch fehl.

Obwohl in diesem Beispiel TCP-Port 3389 verwendet wird, können anhand der folgenden Schritte auch Fehler bei eingehenden und ausgehenden Verbindungen über jeden anderen Port ermittelt werden.

## <a name="detailed-troubleshooting-steps"></a>Ausführliche Schritte zur Problembehandlung
Führen Sie die folgenden Schritte aus, um Probleme bei NSGs für einen virtuellen Computer zu beheben:

1. Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich bei Azure an. Wenn Sie nicht mit der Verwendung von Azure PowerShell vertraut sind, finden Sie entsprechende Informationen im Artikel [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) .
2. Geben Sie den folgenden Befehl ein, damit alle NSG-Regeln zurückgegeben werden, die auf die NIC *VM1-NIC1* in der Ressourcengruppe *RG1* angewendet werden:
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Wenn Sie den Namen einer NIC nicht kennen, geben Sie den folgenden Befehl ein, um die Namen aller NICs in einer Ressourcengruppe abzurufen: 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    Der folgende Text ist ein Beispiel für die Ausgabe der effektiven Regeln, die für die NIC *VM1-NIC1* zurückgegeben wurde:
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    Beachten Sie die folgenden Informationen in der Ausgabe:
   
   * Es gibt zwei Abschnitte für **NetworkSecurityGroup**: Einer ist einem Subnetz (*Subnet1*) zugeordnet und der andere einer NIC (*VM1-NIC1*). In diesem Beispiel wurde jeweils eine NSG angewendet.
   * **Zuordnung** gibt die Ressource (Subnetz oder NIC) an, der eine bestimmte NSG zugeordnet ist. Wenn die NSG-Ressource unmittelbar vor dem Ausführen dieses Befehls verschoben oder ihre Zuordnung aufgehoben wird, müssen Sie möglicherweise einen Moment warten, damit die Änderung in der Befehlsausgabe angezeigt wird. 
   * Regelnamen, denen *defaultSecurityRules*vorangestellt ist: Beim Erstellen einer NSG werden mehrere Standardsicherheitsregeln in der NSG erstellt. Standardregeln können nicht entfernt werden, sie können jedoch mit Regeln mit höherer Priorität überschrieben werden.
     Weitere Informationen zu NSG-Standardsicherheitsregeln finden Sie im Artikel [Übersicht über NSGs](virtual-networks-nsg.md#default-rules) .
   * **ExpandedAddressPrefix** erweitert die Adresspräfixe für NSG-Standardtags. Tags stellen mehrere Adresspräfixe dar. Die Erweiterung der Tags kann bei der Problembehandlung der Verbindung von virtuellen Computern zu/von bestimmten Adresspräfixen nützlich sein. Beim VNET-Peering wird beispielsweise das Tag „VIRTUAL_NETWORK“ erweitert, sodass in der vorherigen Ausgabe Präfixe der per Peering verknüpften VNETs angezeigt werden.
     
     > [!NOTE]
     > Mit dem Befehl werden nur effektive Regeln angezeigt, wenn eine NSG einem Subnetz oder einer NIC oder beiden zugeordnet ist. Ein virtueller Computer kann mehrere NICS mit verschiedenen angewendeten NSGS aufweisen. Führen Sie den Befehl bei der Problembehandlung für jede NIC aus.
     > 
     > 
3. Geben Sie die folgenden Befehle zur weiteren Problembehandlung ein, um die Filterung einer größeren Zahl von NSG-Regeln zu vereinfachen: 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    Ein Filter für den RDP-Datenverkehr (TCP-Port 3389) wird auf die Rasteransicht angewendet, wie in der folgenden Abbildung dargestellt:
   
    ![Liste der Regeln](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. Wie Sie in der Rasteransicht sehen können, gibt es für RDP sowohl Zulassungsregeln als auch Ablehnungsregeln. Die Ausgabe aus Schritt 2 gibt an, dass die Regel *DenyRDP* in der auf das Subnetz angewendeten NSG enthalten ist. Für eingehende Regeln werden NSGs, die auf das Subnetz angewendet werden, zuerst verarbeitet. Wenn eine Übereinstimmung gefunden wird, wird die auf die Netzwerkschnittstelle angewendete NSG nicht verarbeitet. In diesem Fall blockiert die Regel *DenyRDP* des Subnetzes die RDP-Verbindung mit dem virtuellen Computer (**VM1**).
   
   > [!NOTE]
   > An einen virtuellen Computer können mehrere NICs angefügt werden. Jede kann mit einem unterschiedlichen Subnetz verbunden sein. Da die Befehle in den vorherigen Schritten für eine NIC ausgeführt werden, müssen Sie sicherstellen, die NIC anzugeben, bei der die Verbindungsprobleme aufgetreten sind. Wenn Sie nicht sicher sind, können Sie die Befehle jeweils für alle an den virtuellen Computer angefügten NICs ausführen.
   > 
   > 
5. Zum Herstellen einer Verbindung mit VM1 per RDP ändern Sie die Regel *Deny RDP (3389)* in der NSG *Subnet1-NSG* in **Allow RDP(3389)**. Vergewissern Sie sich, dass TCP-Port 3389 geöffnet ist, indem Sie eine RDP-Verbindung mit dem virtuellen Computer öffnen oder das Tool PsPing verwenden. Weitere Informationen zu PsPing finden Sie auf der [PsPing-Downloadseite](https://technet.microsoft.com/sysinternals/psping.aspx)
   
    Sie können Regeln anhand der Informationen in der Ausgabe des folgenden Befehls aus einer NSG entfernen:
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a>Überlegungen
Beachten Sie bei der Problembehandlung von Verbindungsproblemen die folgenden Punkte:

* NSG-Standardregeln blockieren den eingehenden Zugriff über das Internet und lassen nur eingehenden VNET-Datenverkehr zu. Es müssen explizit Regeln hinzugefügt werden, die den eingehenden Zugriff über das Internet nach Bedarf zulassen.
* Wenn die Netzverbindung eines virtuellen Computers nicht aufgrund von NSG-Sicherheitsregeln fehlschlägt, kann das Problem folgende Ursachen haben:
  * Firewallsoftware, die im Betriebssystem des virtuellen Computers ausgeführt wird
  * Es sind Routen für virtuelle Geräte oder den lokalen Datenverkehr konfiguriert. Der Internetdatenverkehr kann über die Tunnelerzwingung lokal umgeleitet werden. Eine RDP/SSH-Verbindung über das Internet mit dem virtuellen Computer wird mit dieser Einstellung möglicherweise nicht hergestellt, je nachdem, wie dieser Datenverkehr auf der lokalen Hardware behandelt wird. Im Artikel [Problembehandlung bei Routen](virtual-network-routes-troubleshoot-powershell.md) finden Sie Informationen zur Diagnose von Problemen bei Routen, die möglicherweise den eingehenden und ausgehenden Datenverkehr auf dem virtuellen Computer verhindern. 
* Beim Peering von VNETs wird das Tag „VIRTUAL_NETWORK“ standardmäßig automatisch erweitert und enthält Präfixe für die per Peering verknüpften VNETs. Sie können diese Präfixe in der Liste **ExpandedAddressPrefix** anzeigen, um Probleme im Hinblick auf VNET-Peering-Verbindungen zu beheben. 
* Effektive Sicherheitsregeln werden nur angezeigt, wenn der NIC oder dem Subnetz des virtuellen Computers eine NSG zugeordnet ist. 
* Wenn der NIC oder dem Subnetz keine NSGs zugeordnet sind und dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen ist, sind alle Ports für den eingehenden und ausgehenden Zugriff geöffnet. Wenn der virtuelle Computer über eine öffentliche IP-Adresse verfügt, empfiehlt es sich dringend, NSGs auf die NIC oder das Subnetz anzuwenden.  




<!--HONumber=Feb17_HO2-->


