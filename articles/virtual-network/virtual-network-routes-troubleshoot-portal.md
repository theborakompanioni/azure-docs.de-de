---
title: Problembehandlung bei Routen – Portal | Microsoft Docs
description: Informationen zur Problembehandlung bei Routen im Azure Resource Manager-Bereitstellungsmodell über das Azure-Portal.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa

---
# <a name="troubleshoot-routes-using-the-azure-portal"></a>Problembehandlung bei Routen über das Azure-Portal
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

1. Melden Sie sich unter „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf **Weitere Dienste** und dann in der angezeigten Liste auf **Virtuelle Computer**.
3. Wählen Sie in der angezeigten Liste einen virtuellen Computer für die Problembehandlung aus. Für den entsprechenden virtuellen Computer wird dann ein Blatt mit Optionen angezeigt.
4. Klicken Sie auf **Diagnose und Problembehandlung**, und wählen Sie dann ein gängiges Problem aus. In diesem Beispiel wird **Ich kann keine Verbindung mit meinem virtuellen Windows-Computer herstellen** ausgewählt. 
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)
5. Unter der Problembeschreibung werden verschiedene Schritte angezeigt (siehe folgende Abbildung): 
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)
   
    Klicken Sie in der Liste der empfohlenen Schritte auf *effective routes* (Effektive Routen).
6. Das Blatt **Effective routes** (Effektive Routen) wird angezeigt, wie in der folgenden Abbildung dargestellt:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)
   
    Wenn der virtuelle Computer nur über eine NIC verfügt, wird diese standardmäßig ausgewählt. Wenn mehrere NICs vorhanden sind, wählen Sie die NIC aus, für die die effektiven Routen angezeigt werden sollen.
   
   > [!NOTE]
   > Wenn der der NIC zugeordnete virtuelle Computer nicht aktiv ist, werden keine effektiven Routen angezeigt. Im Portal werden nur die ersten 200 effektiven Routen angezeigt. Klicken Sie zum Anzeigen der vollständigen Liste auf **Herunterladen**. In der heruntergeladenen CSV-Datei können Sie die Ergebnisse weiter filtern.
   > 
   > 
   
    Beachten Sie die folgenden Angaben in der Ausgabe:
   
   * **Quelle:**gibt den Typ der Route an. Systemrouten werden als *Default* angezeigt, benutzerdefinierte Routen als *User* und Gatewayrouten (statische oder BGP-Routen) als *VPNGateway*.
   * **Status:**gibt den Status der effektiven Route an. Mögliche Werte sind *Aktiv* oder *Ungültig*.
   * **AddressPrefixes:**gibt das Adresspräfix der effektiven Route in CIDR-Notation an. 
   * **nextHopType:**gibt den nächsten Hop für die angegebene Route an. Mögliche Werte sind *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering* oder *Null*. Der Wert *Null* für **nextHopType** in einer benutzerdefinierten Route gibt möglicherweise eine ungültige Route an. Dies ist beispielsweise der Fall, wenn für **nextHopType** der Wert *VirtualAppliance* angegeben ist und der virtuelle Computer des virtuellen Netzwerkgeräts nicht bereitgestellt oder ausgeführt wird. Wenn für **nextHopType** der Wert *VPNGateway* angegeben ist und im angegebenen VNET kein Gateway bereitgestellt oder ausgeführt wird, wird die Route unter Umständen ungültig.
7. In der Abbildung im vorherigen Schritt ist keine Route von *WestUS-VNet1* (Präfix 10.9.0.0/16) zum VNET*WestUS-VNET3* (Präfix 10.10.0.0/16) aufgeführt. In der folgenden Abbildung weist der Peeringlink den Status *Verbindung getrennt* auf:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    Der bidirektionale Link für das Peering ist unterbrochen. Dadurch erklärt sich, warum über VM1 keine Verbindung mit VM3 im VNET *WestUS-VNet3* hergestellt werden konnte.
8. In der folgenden Abbildung werden die Routen nach dem Herstellen des bidirektionalen Peeringlinks angezeigt:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Weitere Szenarios zur Problembehandlung für die Tunnelerzwingung und Routenauswertung finden Sie im Abschnitt [Überlegungen](virtual-network-routes-troubleshoot-portal.md#Considerations) in diesem Artikel.

### <a name="view-effective-routes-for-a-network-interface"></a>Anzeigen der effektiven Routen für eine Netzwerkschnittstelle
Wenn der Netzwerkdatenverkehrsfluss für eine bestimmte Netzwerkschnittstelle (NIC) beeinträchtigt ist, können Sie die vollständige Liste der effektiven Routen für eine NIC direkt anzeigen. Führen Sie die folgenden Schritte aus, um die auf eine NIC angewendeten Aggregatrouten anzuzeigen:

1. Melden Sie sich unter „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf **Weitere Dienste** und dann auf **Netzwerkschnittstellen**.
3. Suchen Sie in der Liste den Namen einer NIC, oder wählen Sie ihn in der angezeigten Liste aus. In diesem Beispiel wird **VM1-NIC1** ausgewählt.
4. Wählen Sie auf dem Blatt **Netzwerkschnittstelle** die Option **Effective routes** (Effektive Routen) aus, wie in der folgenden Abbildung dargestellt:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)
   
    Für **Bereich** ist standardmäßig die ausgewählte Netzwerkschnittstelle festgelegt.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)

### <a name="view-effective-routes-for-a-route-table"></a>Anzeigen der effektiven Routen für eine Routingtabelle
Wenn Sie benutzerdefinierte Routen in einer Routingtabelle ändern, können Sie die Auswirkung der für einen bestimmten virtuellen Computer hinzugefügten Routen überprüfen. Eine Routingtabelle kann beliebig vielen Subnetzen zugeordnet werden. Sie können nun alle effektiven Routen für alle NICs anzeigen, auf die eine bestimmte Routingtabelle angewendet wird, ohne auf dem Blatt der entsprechenden Routingtabelle den Kontext wechseln zu müssen.

In diesem Beispiel wird eine benutzerdefinierte Route (*UDRoute*) in einer Routingtabelle (*UDRouteTable*) angegeben. Über diese Route wird der gesamte Internetdatenverkehr von *Subnet1* im VNET *WestUS-VNet1* über ein virtuelles Netzwerkgerät in das *Subnet2* des gleichen VNet geleitet. Die Route wird in der folgenden Abbildung angezeigt:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Führen Sie die folgenden Schritte aus, um die Aggregatrouten für eine Routingtabelle anzuzeigen:

1. Melden Sie sich unter „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf **Weitere Dienste** und dann auf **Routingtabellen**.
3. Suchen Sie in der Liste die Routingtabelle, für die Sie Aggregatrouten anzeigen möchten, und wählen Sie sie aus. In diesem Beispiel wird **UDRouteTable** ausgewählt. Ein Blatt für die ausgewählte Routingtabelle wird angezeigt (siehe folgende Abbildung):
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)
4. Wählen Sie auf dem Blatt **Routingtabelle** die Option **Effective Routes** (Effektive Routen) aus. Der **Bereich** ist auf die ausgewählte Routingtabelle festgelegt.
5. Eine Routingtabelle kann auf mehrere Subnetze angewendet werden. Wählen Sie in der Liste das **Subnetz** aus, das überprüft werden soll. In diesem Beispiel wird **Subnet1** ausgewählt.
6. Wählen Sie eine **Netzwerkschnittstelle**aus. Es sind alle mit dem ausgewählten Subnetz verbundenen NICs aufgeführt. In diesem Beispiel wird **VM1-NIC1** ausgewählt.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)
   
   > [!NOTE]
   > Wenn die NIC keinem ausgeführten virtuellen Computer zugeordnet ist, werden keine effektiven Routen angezeigt.
   > 
   > 

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
  * Die Regeln von Netzwerksicherheitsgruppen (NSGs) können den Datenverkehrsfluss beeinträchtigen. Weitere Informationen finden Sie im Artikel [Problembehandlung bei Netzwerksicherheitsgruppen](virtual-network-nsg-troubleshoot-portal.md) .

<!--HONumber=Oct16_HO2-->


