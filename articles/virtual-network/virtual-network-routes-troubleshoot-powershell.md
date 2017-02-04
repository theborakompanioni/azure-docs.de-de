---
title: "Problembehandlung bei Routen – PowerShell | Microsoft Docs"
description: Informationen zur Problembehandlung bei Routen im Azure Resource Manager-Bereitstellungsmodell mit Azure PowerShell.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: b568a9bea9679a9edeb708a5f7fcc6d68854574f


---
# <a name="troubleshoot-routes-using-azure-powershell"></a>Problembehandlung bei Routen mit Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

Wenn Probleme mit der Netzwerkverbindung zu oder von Ihrem virtuellen Azure-Computer (VM) auftreten, wird der Datenverkehrsfluss auf dem virtuellen Computer möglicherweise durch Routen beeinträchtigt. Dieser Artikel bietet eine Übersicht über Diagnosefunktionen für Routen zur weiteren Problembehandlung.

Routingtabellen sind Subnetzen zugeordnet und gelten für alle Netzwerkschnittstellen (NICs) im jeweiligen Subnetz. Auf jede Netzwerkschnittstelle können die folgenden Routentypen angewendet werden:

* **Systemrouten:** Standardmäßig verfügt jedes in einem virtuellen Azure-Netzwerk (VNET) erstellte Subnetz über Systemroutingtabellen, über die lokaler VNET-Datenverkehr, lokaler Datenverkehr durch VPN-Gateways und Internetdatenverkehr ermöglicht wird. Auch für per Peering verknüpfte VNETs sind Systemrouten vorhanden.
* **BGP-Routen:** werden über ExpressRoute- oder Standort-zu-Standort-VPN-Verbindungen an Netzwerkschnittstellen weitergegeben. Weitere Informationen zum BGP-Routing finden Sie in den Artikeln [Übersicht über BGP mit Azure-VPN-Gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) und [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).
* **Benutzerdefinierte Routen (UDR):** Bei Verwendung virtueller Netzwerkgeräte oder der Tunnelerzwingung zur Umleitung des Datenverkehrs an ein lokales Netzwerk über eine Standort-zu-Standort-VPN-Verbindung sind der Routingtabelle des Subnetzes möglicherweise benutzerdefinierte Routen zugeordnet. Wenn Sie nicht mit benutzerdefinierten Routen vertraut sind, finden Sie entsprechende Informationen im Artikel zu [benutzerdefinierten Routen](virtual-networks-udr-overview.md#user-defined-routes) .

Aufgrund der verschiedenen Routen, die auf eine Netzwerkschnittstelle angewendet werden können, lassen sich die effektiven Aggregatrouten unter Umständen schwer ermitteln. Für die Problembehandlung der Netzwerkverbindung von virtuellen Computern können Sie alle effektiven Routen für eine Netzwerkschnittstelle im Azure Resource Manager-Bereitstellungsmodell anzeigen.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Verwenden von effektiven Routen zur Problembehandlung des Datenverkehrsflusses auf virtuellen Computern
In diesem Artikel wird anhand des folgenden beispielhaften Szenarios veranschaulicht, wie Probleme mit den effektiven Routen für eine Netzwerkschnittstelle behoben werden:

Über einen virtuellen Computer (*VM1*), der mit einem VNET (*VNet1*, Präfix 10.9.0.0/16) verbunden ist, kann keine Verbindung mit einem anderen virtuellen Computer (VM3) in einem per Peering neu verknüpften VNET (*VNet3*, Präfix 10.10.0.0/16) hergestellt werden. Auf die mit dem virtuellen Computer verbundene Netzwerkschnittstelle VM1-NIC1 werden keine benutzerdefinierten Routen oder BGP-Routen, sondern nur Systemrouten angewendet.

In diesem Artikel wird erläutert, wie die Ursache für den Verbindungsfehler mit der Funktion für effektive Routen im Azure Resource Manager-Bereitstellungsmodell ermittelt werden kann.
Obwohl in diesem Beispiel nur Systemrouten verwendet werden, können mit den gleichen Schritten auch Fehler bei ein- und ausgehenden Verbindungen über jeden anderen Routentyp ermittelt werden.

> [!NOTE]
> Wenn an den virtuellen Computer mehrere NICs angefügt sind, überprüfen Sie die effektiven Routen für jede NIC, um Probleme bei der Netzwerkverbindung zu und von einem virtuellen Computer zu diagnostizieren.
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a>Anzeigen der effektiven Routen für einen virtuellen Computer
Führen Sie die folgenden Schritte aus, um die auf einen virtuellen Computer angewendeten Aggregatrouten anzuzeigen:

### <a name="view-effective-routes-for-a-network-interface"></a>Anzeigen der effektiven Routen für eine Netzwerkschnittstelle
Führen Sie die folgenden Schritte durch, um die auf eine Netzwerkschnittstelle angewendeten Aggregatrouten anzuzeigen:

1. Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich bei Azure an. Wenn Sie nicht mit Azure PowerShell vertraut sind, finden Sie entsprechende Informationen im Artikel [Gewusst wir: Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) .
2. Mit dem folgenden Befehl werden alle Routen zurückgegeben, die auf die Netzwerkschnittstelle *VM1-NIC1* in der Ressourcengruppe *RG1* angewendet werden.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Wenn Sie den Namen einer Netzwerkschnittstelle nicht kennen, geben Sie den folgenden Befehl ein, um die Namen aller Netzwerkschnittstellen in einer Ressourcengruppe abzurufen.*
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   Die folgende Ausgabe ähnelt der Ausgabe für die einzelnen Routen, die auf das Subnetz angewendet werden, mit dem die NIC verbunden ist:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   Beachten Sie die folgenden Angaben in der Ausgabe:
   
   * **Name:**Der Name der effektiven Route kann für benutzerdefinierte Routen leer sein, sofern er nicht ausdrücklich angegeben wurde. 
   * **Status:**gibt den Status der effektiven Route an. Mögliche Werte sind „Aktiv“ oder „Ungültig“.
   * **AddressPrefixes:**gibt das Adresspräfix der effektiven Route in CIDR-Notation an. 
   * **nextHopType:**gibt den nächsten Hop für die angegebene Route an. Mögliche Werte sind *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering* oder *Null*. Der Wert *Null* für **nextHopType** in einer benutzerdefinierten Route gibt möglicherweise eine ungültige Route an. Dies ist beispielsweise der Fall, wenn für **nextHopType** der Wert *VirtualAppliance* angegeben ist und der virtuelle Computer des virtuellen Netzwerkgeräts nicht bereitgestellt oder ausgeführt wird. Wenn für **nextHopType** der Wert *VPNGateway* angegeben ist und im angegebenen VNET kein Gateway bereitgestellt oder ausgeführt wird, wird die Route unter Umständen ungültig.
   * **NextHopIpAddress:**gibt die IP-Adresse des nächsten Hops der effektiven Route an.
   
   Mit dem folgenden Befehl werden die Routen in einer übersichtlichen Tabelle zurückgegeben:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   Die folgende Ausgabe ist ein Teil der für das zuvor beschriebene Szenario zurückgegebenen Ausgabe:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. In der Ausgabe im vorherigen Schritt ist keine Route von *WestUS-VNet1* (Präfix 10.9.0.0/16) zu *WestUS-VNet3* (Präfix 10.10.0.0/16)** aufgeführt. Wie in der folgenden Abbildung dargestellt, weist der VNET-Peeringlink mit dem VNET *WestUS-VNet3* den Status *Verbindung getrennt* auf.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    Der bidirektionale Link für das Peering ist unterbrochen. Dadurch erklärt sich, warum über VM1 keine Verbindung mit VM3 im VNET *WestUS-VNet3* hergestellt werden konnte. Richten Sie erneut einen bidirektionalen VNET-Peeringlink für die VNETs *WestUS-VNet1* und *WestUS-VNet3* ein. Es folgt die Ausgabe, die zurückgegeben wird, nachdem der VNET-Peeringlink korrekt erstellt wurde:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    Nachdem Sie das Problem ermittelt haben, können Sie Routen und Routingtabellen hinzufügen, entfernen oder ändern. Geben Sie den folgenden Befehl ein, um eine Liste der dazu verwendeten Befehle anzuzeigen:
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Überlegungen
Beim Überprüfen der zurückgegebenen Liste der Routen sind einige Punkte zu beachten:

* Das Routing beruht auf der längsten Präfixübereinstimmung (Longest Prefix Match, LPM) zwischen benutzerdefinierten Routen, BGP-Routen und Systemrouten. Wenn mehrere Routen mit identischer längster Präfixübereinstimmung vorhanden sind, wird die Route basierend auf ihrem Ursprung in der folgenden Reihenfolge ausgewählt:
  
  * Benutzerdefinierte Route
  * BGP-Route
  * Systemroute (Standardroute)
    
    Mit effektiven Routen können Sie nur die effektiven Routen mit der jeweils längsten Präfixübereinstimmung basierend auf allen verfügbaren Routen anzeigen. Indem angezeigt wird, wie die Routen für eine bestimmte NIC tatsächlich ausgewertet werden, wird die Problembehandlung bestimmter Routen, die die Verbindung zu/von dem virtuellen Computer beeinträchtigen können, erheblich vereinfacht.
* Wenn benutzerdefinierte Routen vorhanden sind und Datenverkehr an ein virtuelles Netzwerkgerät geleitet wird (mit *VirtualAppliance* als **nextHopType**), müssen Sie sicherstellen, dass die IP-Weiterleitung für das virtuelle Netzwerkgerät aktiviert ist, andernfalls werden Pakete verworfen. 
* Wenn die Tunnelerzwingung aktiviert ist, wird der gesamte ausgehende Internetdatenverkehr lokal weitergeleitet. RDP/SSH-Verbindungen über das Internet mit dem virtuellen Computer werden mit dieser Einstellung möglicherweise nicht hergestellt, je nachdem, wie dieser Datenverkehr lokal behandelt wird. 
  Die Tunnelerzwingung kann in folgenden Fällen aktiviert werden:
  * Wenn eine Standort-zu-Standort-VPN-Verbindung verwendet wird, indem eine benutzerdefinierte Route mit „VPNGateway“ als „nextHopType“ festgelegt wird
  * Wenn eine Standardroute über BGP angekündigt wird
* Damit der VNET-Peering-Datenverkehr ordnungsgemäß weitergeleitet wird, muss eine Systemroute mit **nextHopType:** *nextHopType* für den Präfixbereich des per Peering verknüpften VNET vorhanden sein. Wenn eine solche Route nicht vorhanden ist und der VNET-Peeringlink korrekt ist:
  * Versuchen Sie es nach einigen Sekunden erneut, wenn es sich um einen neu erstellten Peeringlink handelt. Das Weitergeben von Routen an alle Netzwerkschnittstellen in einem Subnetz dauert gelegentlich etwas länger.
  * Die Regeln von Netzwerksicherheitsgruppen (NSGs) können den Datenverkehrsfluss beeinträchtigen. Weitere Informationen finden Sie im Artikel [Problembehandlung bei Netzwerksicherheitsgruppen](virtual-network-nsg-troubleshoot-powershell.md) .




<!--HONumber=Dec16_HO2-->


