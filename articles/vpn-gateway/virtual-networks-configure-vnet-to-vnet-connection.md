---
title: "Herstellen einer Verbindung für ein virtuelles Azure-Netzwerk mit einem anderen VNet: Klassisch | Microsoft-Dokumentation"
description: Informationen zum Herstellen von Verbindungen zwischen virtuellen Azure-Netzwerken mithilfe von PowerShell und dem klassischen Azure-Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7413827f-233d-4c7c-a133-9c99cf031833
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eadb1f29da69e7f6fcc2c7c19ba67f4e3072c346
ms.openlocfilehash: 863b308125a7a9ae2b9d9104d150bd2474c064b3


---
# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Konfigurieren einer VNet-zu-VNet-Verbindung für das klassische Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Klassisch – Klassisches Portal](virtual-networks-configure-vnet-to-vnet-connection.md)
> 
> 

Dieser Artikel führt Sie durch die Schritte zum Erstellen und Verbinden virtueller Netzwerke mithilfe des klassischen Bereitstellungsmodells (auch als Dienstverwaltung bezeichnet). In den folgenden Schritten wird das klassische Azure-Portal verwendet, um die VNets und Gateways zu erstellen, und PowerShell wird zum Konfigurieren der VNet-zu-VNet-Verbindung verwendet. Sie können die Verbindung nicht im Portal konfigurieren.

![VNet-zu-VNet-Konnektivitätsdiagramm](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und -methoden für VNet-zu-VNet-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle enthält die derzeit verfügbaren Bereitstellungsmodelle und -methoden für VNet-zu-VNet-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Über VNet-zu-VNet-Verbindungen
Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines virtuellen Netzwerks mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPSec/IKE bereitzustellen. 

Die VNets, die Sie verbinden, können sich in verschiedenen Abonnements und Regionen befinden. Sie können die VNet-zu-VNet-Kommunikation mit Konfigurationen für mehrere Standorte kombinieren. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.

### <a name="why-connect-virtual-networks"></a>Gründe für Verbindungen zwischen virtuellen Netzwerken
Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

* **Regionsübergreifende Georedundanz und Geopräsenz**
  
  * Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
  * Mit dem Azure Load Balancer und Clustertechnologie von Microsoft oder Drittanbietern können Sie Workloads mit hoher Verfügbarkeit mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.
* **Regionale Anwendungen mit mehreren Ebenen mit starker Isolierungsgrenze**
  
  * Innerhalb der gleichen Region können Sie Anwendungen mit mehreren Ebenen mit mehreren verbundenen VNets einrichten, die starke Isolierung und sichere Kommunikation zwischen den Ebenen aufweisen.
* **Abonnementübergreifende Kommunikation zwischen Organisationen in Azure**
  
  * Wenn Sie über mehrere Azure-Abonnements verfügen, können Sie Workloads aus verschiedenen Abonnements sicher zwischen virtuellen Netzwerken verbinden.
  * Für Unternehmen und Dienstanbieter können Sie die organisationsübergreifende Kommunikation mit sicherer VPN-Technologie in Azure aktivieren.

### <a name="vnet-to-vnet-considerations-for-classic-vnets"></a>Informationen zu VNet-zu-VNet-Verbindungen bei klassischen VNets
* Die virtuellen Netzwerke können sich im gleichen Abonnement oder in verschiedenen Abonnements befinden.
* Die virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen (Standorte) befinden.
* Ein Clouddienst oder Endpunkt mit Lastenausgleich darf auch dann nicht mehrere virtuelle Netzwerke umfassen, wenn diese verbunden sind.
* Für das Verbinden mehrerer virtueller Netzwerke sind keine VPN-Geräte erforderlich.
* VNet-zu-VNet unterstützt das Verbinden virtueller Azure-Netzwerke. Nicht unterstützt werden hingegen Verbindungen virtueller Computer oder Clouddienste, die nicht in einem virtuellen Netzwerk bereitgestellt wurden.
* Für VNet-zu-VNet sind Gateways mit dynamischem Routing erforderlich. Azure-Gateways mit statischem Routing werden nicht unterstützt.
* Virtuelle Netzwerkverbindungen können simultan mit VPNs mit mehreren Standorten verwendet werden. Maximal 10 VPN-Tunnel können für ein VPN-Gateway des virtuellen Netzwerks verwendet werden, das Verbindungen mit anderen virtuellen Netzwerken oder lokalen Standorten herstellt.
* Die Adressräume der virtuellen Netzwerke und der lokalen Netzwerkstandorte dürfen sich nicht überschneiden. Wenn sich Adressbereiche überschneiden, tritt beim Erstellen eines virtuellen Netzwerks oder beim Hochladen von NETCFG-Konfigurationsdateien ein Fehler auf.
* Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden nicht unterstützt.
* Alle VPN-Tunnel des VNet (einschließlich P2S-VPNs) verwenden die verfügbare Bandbreite für das VPN-Gateway und die gleiche SLA für die Verfügbarkeit des VPN-Gateways in Azure gemeinsam.
* VNet-zu-VNet-Datenverkehr wird über den Azure-Backbone übertragen.

## <a name="a-namestep1astep-1---plan-your-ip-address-ranges"></a><a name="step1"></a>Schritt 1: Planen der IP-Adressbereiche
Eine wichtige Entscheidung betrifft die Bereiche, die Sie zum Konfigurieren Ihrer virtuellen Netzwerke verwenden. Bei dieser Konfiguration müssen Sie sicherstellen, dass sich die VNet-Bereiche weder untereinander überlappen, noch mit lokalen Netzwerken, mit denen sie eine Verbindung herstellen.

Die folgende Tabelle zeigt ein Beispiel zur Definition von VNets. Verwenden Sie die aufgeführten Bereiche nur als Richtlinie. Notieren Sie sich die Bereiche für Ihre virtuellen Netzwerke. Sie benötigen diese Informationen in den späteren Schritten.

**Beispieleinstellungen**

| Virtual Network | Adressraum | Region | Verbindung mit dem lokalen Netzwerkstandort |
|:--- |:--- |:--- |:--- |
| VNet1 |VNet1 (10.1.0.0/16) |USA, Westen |VNet2Local (10.2.0.0/16) |
| VNet2 |VNet2 (10.2.0.0/16) |Japan Ost |VNet1Local (10.1.0.0/16) |

## <a name="step-2---create-vnet1"></a>Schritt 2: Erstellen von VNet1
In diesem Schritt erstellen wir VNet1. Wenn Sie eines der Beispiele verwenden, achten Sie darauf, dass Sie Ihre eigenen Werte einsetzen. Wenn Ihr VNet bereits vorhanden ist, müssen Sie diesen Schritt nicht ausführen. Aber Sie müssen sicherstellen, dass die IP-Adressbereiche sich nicht mit den Bereichen für das zweite VNet oder mit einem der anderen VNets überlappen, mit denen Sie eine Verbindung herstellen möchten.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an. In diesem Artikel verwenden wir das klassische Portal, da einige der erforderlichen Konfigurationseinstellungen noch nicht im Azure-Portal verfügbar sind.
2. Klicken Sie in der unteren linken Ecke des Bildschirms auf **Neu** > **Netzwerkdienste** > **Virtuelles Netzwerk** > **Benutzerdefiniert erstellen**, um den Konfigurations-Assistenten zu starten. Während Sie den Assistenten bearbeiten, fügen Sie die angegebenen Werte auf jeder Seite hinzu.

### <a name="virtual-network-details"></a>Details zum virtuellen Netzwerk
Geben Sie auf der Seite Details des virtuellen Netzwerks die folgenden Informationen ein:

  ![Details zum virtuellen Netzwerk](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

* **Name** – Der Name des virtuellen Netzwerks. Beispiel: VNet1
* **Standort**: Wenn Sie ein virtuelles Netzwerk erstellen, ordnen Sie dieses einem Azure-Standort (einer Region) zu. Wenn Sie z. B. wünschen, dass Ihre virtuellen Computer, die Sie für Ihr virtuelles Netzwerk bereitgestellt haben, physisch in "USA West" vorhanden sind, wählen Sie diesen Standort aus. Sie können den Ihrem virtuellen Netzwerk zugeordnete Standort nach dem Erstellen nicht mehr ändern.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS-Server und VPN-Konnektivität
Geben Sie auf der Seite DNS Servers and VPN Connectivity die folgenden Informationen ein, und klicken Sie dann in der unteren rechten Ecke auf den Weiter-Pfeil.

  ![DNS-Server und VPN-Konnektivität](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

* **DNS-Server** – Geben Sie den Namen und die IP-Adresse des DNS-Servers ein, oder wählen Sie einen zuvor registrierten DNS-Server in der Dropdown-Liste aus. Mit dieser Einstellung wird kein DNS-Server erstellt. Sie bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Wenn Sie Namensauflösung zwischen Ihren virtuellen Netzwerken wünschen, müssen Sie Ihren eigenen DNS-Server konfigurieren, anstatt die Namensauflösung zu verwenden, die von Azure bereitgestellt wird.
* Aktivieren Sie keines der Kontrollkästchen für die P2S- oder S2S-Verbindungen. Klicken Sie unten rechts auf den Pfeil, um zum nächsten Bildschirm zu gelangen.

### <a name="virtual-network-address-spaces"></a>Adressräume des virtuellen Netzwerks
Geben Sie auf der Seite „Adressräume des virtuellen Netzwerks“ die Adressbereiche an, die für das virtuelle Netzwerk verwendet werden sollen. Dies sind die dynamischen IP-Adressen (DIPS), die den virtuellen Computern und anderen Rolleninstanzen zugewiesen werden, die Sie für dieses virtuelle Netzwerk bereitstellen. 

Wenn Sie ein VNet erstellen, das auch mit Ihrem lokalen Netzwerk verbunden ist, ist es besonders wichtig, einen Bereich auszuwählen, der nicht mit den Bereichen überlappt, die für das lokale Netzwerk verwendet werden. Sprechen Sie sich in diesem Fall mit Ihrem Netzwerkadministrator ab. Der Netzwerkadministrator muss ggf. einen Bereich von IP-Adressen aus dem Adressraum Ihres lokalen Netzwerks reservieren, den Sie für Ihr VNet verwenden können.

  ![Seite "Adressräume des virtuellen Netzwerks"](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

* **Adressraum** – Umfasst Start-IP und die Anzahl Adressen. Stellen Sie sicher, dass die angegebenen Adressräume und die Adressräume im lokalen Netzwerk nicht überlappen. In diesem Beispiel verwenden Sie 10.1.0.0/16 für VNet1.
* **Subnetz hinzufügen** – Umfasst Start-IP und Anzahl Adressen. Zusätzliche Subnetze sind nicht erforderlich, aber Sie können eine getrenntes Subnetz für virtuelle Computer erstellen, die über statische DIPs verfügen sollen. Vielleicht möchten Sie jedoch auch Ihre virtuellen Computer in einem Subnetz zusammenfassen, das von Ihren anderen Rolleninstanzen getrennt ist.

**Klicken Sie auf das Häkchen** rechts unten auf der Seite, damit das virtuelle Netzwerk erstellt wird. Sobald der Vorgang abgeschlossen ist, wird auf der Seite „Netzwerke“ unter „Status“ der Eintrag „Erstellt“ angezeigt.

## <a name="step-3---create-vnet2"></a>Schritt 3: Erstellen von VNet2
Wiederholen Sie anschließend die oben aufgeführten Schritte, um ein weiteres VNet zu erstellen. In einem späteren Schritt verbinden Sie die beiden VNets. Sie können auf die [Beispieleinstellungen](#step1) in Schritt 1 zurückgreifen. Wenn Ihr VNet bereits vorhanden ist, müssen Sie diesen Schritt nicht ausführen. Aber Sie müssen sicherstellen, dass die IP-Adressbereiche sich nicht mit den Bereichen der anderen VNets oder lokalen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen möchten.

## <a name="step-4---add-the-local-network-sites"></a>Schritt 4: Hinzufügen der lokalen Netzwerkstandorte
Wenn Sie eine VNet-zu-VNet-Konfiguration erstellen, müssen Sie lokale Netzwerkstandorte konfigurieren, die auf der Seite **Lokale Netzwerke** des Portals angezeigt werden. Azure verwendet die für die einzelnen lokalen Netzwerkstandorte angegebenen Einstellungen, um zu bestimmen, wie Datenverkehr zwischen den VNets weitergeleitet wird. Sie legen den Namen fest, den Sie zum Verweisen auf die einzelnen lokalen Netzwerkstandorte verwenden möchten. Sie sollten einen aussagekräftigen Namen verwenden, da Sie den Wert in späteren Schritten aus einer Dropdownliste auswählen.

Beispielsweise stellt VNet1 eine Verbindung mit einem lokalen Netzwerkstandort her, den Sie mit dem Namen „VNet2Local“ erstellt haben. Die Einstellungen für VNet2Local enthalten die Adresspräfixe für VNet2 und eine öffentliche IP-Adresse für das VNet2-Gateway. VNet2 stellt eine Verbindung mit einem lokalen Netzwerkstandort her, den Sie mit dem Namen „VNet1Local“ erstellt haben, und der die Adresspräfixe für VNet1 und die öffentliche IP-Adresse für das VNet1-Gateway enthält.

### <a name="a-namelocalnetaadd-the-local-network-site-vnet1local"></a><a name="localnet"></a>Hinzufügen des lokalen Netzwerkstandorts VNet1Local
1. Klicken Sie in der unteren linken Ecke des Bildschirms auf **Neu** > **Netzwerkdienste** > **Virtuelles Netzwerk** > **Lokales Netzwerk hinzufügen**.
2. Geben Sie auf der Seite **Details zum lokalen Netzwerk angeben** unter **Name** den Namen ein, den Sie für das Netzwerk verwenden möchten, mit dem Sie eine Verbindung herstellen möchten. In diesem Beispiel können Sie „VNet1Local“ verwenden, um auf die IP-Adressbereiche und das Gateway für VNet1 zu verweisen.
3. Geben Sie in **IP-Adresse des VPN-Geräts (optional)**eine beliebige gültige öffentliche IP-Adresse an. Normalerweise verwenden Sie die tatsächliche externe IP-Adresse für ein VPN-Gerät. Verwenden Sie für VNet-zu-VNet-Konfigurationen die öffentliche IP-Adresse, die dem Gateway für das VNet zugewiesen ist. Wenn Sie das Gateway jedoch noch nicht erstellt haben, können Sie eine beliebige gültige öffentliche IP-Adresse als Platzhalter eingeben. Lassen Sie dieses Feld nicht leer, es ist für diese Konfiguration nicht optional. In einem späteren Schritt kehren Sie zu diesen Einstellungen zurück und konfigurieren sie dann mit den entsprechenden Gateway-IP-Adressen, nachdem Azure diese generiert hat. Klicken Sie auf den Pfeil, um zum nächsten Bildschirm zu gelangen.
4. Geben Sie auf der Seite **Adresse angeben**den tatsächlichen IP-Adressbereich und die Adressenanzahl für VNet1 ein. Diese Angaben müssen genau dem Bereich entsprechen, den Sie für VNet1 konfiguriert haben. Azure verwendet den IP-Adressbereich, den Sie angeben, um den Datenverkehr für VNet1 weiterzuleiten. Klicken Sie auf das Häkchen, um das lokale Netzwerk zu erstellen.

### <a name="add-the-local-network-site-vnet2local"></a>Hinzufügen des lokalen Netzwerkstandorts VNet2Local
Verwenden Sie die oben aufgeführten Schritte, um den lokalen Netzwerkstandort „VNet2Local“ zu erstellen. Sie können bei Bedarf auf die Werte in den [Beispieleinstellungen](#step1) in Schritt 1 zurückgreifen.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Konfigurieren der einzelnen VNets, sodass sie auf ein lokales Netzwerk verweisen
Jedes VNet muss auf das entsprechende lokale Netzwerk verweisen, an das Sie Datenverkehr weiterleiten möchten. 

**Für VNet1**

1. Navigieren Sie zur Seite **Konfigurieren** für das virtuelle Netzwerk **VNet1**. 
2. Wählen Sie unter „Site-to-Site-Konnektivität“ die Option „Eine Verbindung mit dem lokalen Netzwerk herstellen“ aus, und wählen Sie dann **VNet2Local** als lokales Netzwerk aus der Dropdownliste aus. 
3. Speichern Sie die Einstellungen.

**Für VNet2**

1. Navigieren Sie zur Seite **Konfigurieren** für das virtuelle Netzwerk **VNet2**. 
2. Wählen Sie unter „Site-to-Site-Konnektivität“ die Option „Eine Verbindung mit dem lokalen Netzwerk herstellen“ aus, und wählen Sie dann **VNet1Local** als lokales Netzwerk aus der Dropdownliste aus. 
3. Speichern Sie die Einstellungen.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Schritt 5: Konfigurieren eines Gateways für jedes VNet
Konfigurieren Sie ein Gateway mit dynamischem Routing für jedes virtuelle Netzwerk. Diese Konfiguration unterstützt keine Gateways mit statischem Routing. Wenn Sie VNets verwenden, die zuvor konfiguriert wurden und die bereits über Gateways mit dynamischem Routing verfügen, müssen Sie diesen Schritt nicht ausführen. Wenn Sie Gateways mit statischem Routing haben, müssen Sie diese löschen und dann erneut als Gateways mit dynamischem Routing erstellen. Beim Löschen eines Gateways wird die zugewiesene öffentliche IP-Adresse freigegeben. Sie müssen dann lokale Netzwerke und VPN-Geräte mit der neuen öffentlichen IP-Adresse für das neue Gateway neu konfigurieren.

1. Stellen Sie auf der Seite **Netzwerke** sicher, dass in der Statusspalte für Ihr virtuelles Netzwerk **Erstellt** angezeigt wird.
2. Klicken Sie in der Spalte **Name** auf den Namen Ihres virtuellen Netzwerks. In diesem Beispiel verwenden wir „VNet1“.
3. Vergewissern Sie sich auf der Seite **Dashboard** , dass dieses VNet noch kein konfiguriertes Gateway besitzt. Diese Statusänderung wird angezeigt, wenn Sie die Konfigurationsschritte für Ihr Gateway absolvieren.
4. Klicken Sie unten auf der Seite auf **Gateway erstellen** und **Dynamisches Routing**. Wenn Sie aufgefordert werden zu bestätigen, dass das Gateway erstellt werden soll, klicken Sie auf "Ja".
   
      ![Gatewaytyp](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  
5. Während der Gatewayerstellung wechselt die Gatewaygrafik auf der Seite zu gelb und zeigt „Gateway wird erstellt“ an. Die Erstellung des Gateways nimmt normalerweise 30 Minuten in Anspruch.
6. Wiederholen Sie die gleichen Schritte für VNet2. Das erste VNet-Gateway muss nicht fertiggestellt sein, wenn Sie mit dem Erstellen des Gateways für das andere VNet beginnen.
7. Wenn sich der Gatewaystatus in „Verbinden“ ändert, wird die öffentliche IP-Adresse für jedes Gateway im Dashboard angezeigt. Notieren Sie sich die IP-Adressen, die für jedes VNet gelten. Gehen Sie dabei sorgfältig vor, um sie nicht zu verwechseln. Diese IP-Adressen werden verwendet, wenn Sie die IP-Platzhalteradressen für das VPN-Gerät für jedes lokale Netzwerk bearbeiten.

## <a name="step-6---edit-the-local-network"></a>Schritt 6: Bearbeiten des lokalen Netzwerks
1. Klicken Sie auf der Seite **Lokale Netzwerke** auf den Namen des lokalen Netzwerks, den Sie bearbeiten möchten, und klicken Sie dann unten auf der Seite auf **Bearbeiten**. Geben Sie als **IP-Adresse des VPN-Geräts** die IP-Adresse des Gateways ein, das dem VNet entspricht. Geben Sie z.B. für VNet1Local die Gateway-IP-Adresse ein, die VNet1 zugewiesen ist. Klicken Sie unten auf der Seite auf den Pfeil.
2. Klicken Sie auf der Seite **Den Adressraum angeben** unten rechts auf das Häkchen, ohne Änderungen vorzunehmen.

## <a name="step-7---create-the-vpn-connection"></a>Schritt 7: Erstellen der VPN-Verbindung
Legen Sie nach Abschluss aller zuvor beschriebenen Schritte die vorinstallierten IPSec-/IKE-Schlüssel fest, und stellen Sie die Verbindung her. Diese Schritte werden mit PowerShell ausgeführt und können nicht im Portal konfiguriert werden. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) . Stellen Sie sicher, dass Sie die neueste Version der Dienstverwaltungs-Cmdlets (SM) herunterladen. 

1. Öffnen Sie Windows PowerShell, und melden Sie sich an.
   
        Add-AzureAccount
2. Wählen Sie das Abonnement aus, in dem sich Ihre VNets befinden.
   
        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"
3. Erstellen Sie die Verbindungen. Beachten Sie in den folgenden Beispielen, dass der Wert für „SharedKey“ identisch ist. Dieser Wert muss immer übereinstimmen.

    VNet1-zu-VNet2-Verbindung

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2-zu-VNet1-Verbindung

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Warten Sie, bis die Verbindung initialisiert wird. Nach der Initialisierung entspricht das Gateway der folgenden Grafik.
   
    ![Gatewaystatus – verbunden](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  
   
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Nächste Schritte
Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Dokumentation zu Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .




<!--HONumber=Jan17_HO4-->


