---
title: 'Verbinden von klassischen virtuellen Netzwerken mit Azure Resource Manager-VNETs: Portal | Microsoft Docs'
description: Hier erfahren Sie, wie Sie mithilfe von VPN Gateway und des Portals eine VPN-Verbindung zwischen klassischen VNets und Resource Manager-VNets erstellen.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 10985eaebd9148ef4fbd22eac0ba50076ca71ef5
ms.openlocfilehash: c20d72f25571e582310ae65ad1d887dbdb7b011c


---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Verbinden von virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen über das Portal
> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

In Azure sind zurzeit zwei Verwaltungsmodelle verfügbar: klassisches Modell und RM-Modell (Resource Manager). Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. In diesem Artikel wird Schritt für Schritt erläutert, wie Sie klassische VNets mit Resource Manager-VNets verbinden, damit die Ressourcen in den separaten Bereitstellungsmodellen über eine Gatewayverbindung miteinander kommunizieren können. 

Sie können auch eine Verbindung zwischen VNets in unterschiedlichen Abonnements und Regionen erstellen. Es ist auch möglich, VNets zu verbinden, die bereits über Verbindungen mit lokalen Netzwerken verfügen, sofern sie mit einem dynamischen oder routenbasierten Gateway konfiguriert wurden. Weitere Informationen zu VNET-zu-VNET-Verbindungen finden Sie am Ende dieses Artikels im Abschnitt [Informationen zu VNET-zu-VNET-Verbindungen](#faq).

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und -methoden für VNet-zu-VNet-Verbindungen
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Die folgende Tabelle wird aktualisiert, wenn neue Artikel und weitere Tools für diese Konfiguration verfügbar werden. Wenn ein Artikel verfügbar ist, fügen wir in der Tabelle einen direkten Link dazu ein.<br><br>

**VNet-zu-VNet**

[!INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

**VNet-Peering**

[!INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="a-namebeforeabefore-beginning"></a><a name="before"></a>Vorbereitungen
Die folgenden Schritte führen Sie durch die erforderlichen Einstellungen zum Konfigurieren eines dynamischen oder routenbasierten Gateways für jedes VNet und die Erstellung einer VPN-Verbindung zwischen den Gateways. Diese Konfiguration unterstützt keine statischen oder richtlinienbasierten Gateways. 

In diesem Artikel verwenden wir das Azure-Portal und PowerShell. PowerShell ist erforderlich, um die Verbindungen zwischen den virtuellen Netzwerken zu erstellen. Die Verbindungen für diese Konfiguration können nicht über das Verwaltungsportal erstellt werden.

### <a name="prerequisites"></a>Voraussetzungen
* Beide VNets wurden bereits erstellt.
* Die Adressbereiche für die VNets überlappen weder einander noch die Bereiche für andere Verbindungen, mit denen die Gateways verbunden werden können.
* Sie haben die aktuellen PowerShell-Cmdlets installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) . Installieren Sie sowohl die SM-Cmdlets (Dienstverwaltung) als auch die RM-Cmdlets (Resource Manager). 

### <a name="a-namevaluesaexample-settings"></a><a name="values"></a>Beispieleinstellungen
Sie können die Beispieleinstellungen als Referenz verwenden.

**Einstellungen für das klassische VNet**

VNet-Name = ClassicVNet  <br>
Adressraum = 10.0.0.0/24 <br>
Subnet-1 = 10.0.0.0/27 <br>
Ressourcengruppe = ClassicRG <br>
Standort = USA, Westen <br>
GatewaySubnet = 10.0.0.32/28 <br>
Lokaler Standort = RMVNetLocal <br>

**Einstellungen für das Resource Manager-VNet**

VNet-Name = RMVNet  <br>
Adressraum = 192.168.0.0/16 <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Ressourcengruppe = RG1 <br>
Standort = USA, Osten <br>
Name des Gateways für virtuelle Netzwerke = RMGateway <br>
Gatewaytyp = VPN <br>
VPN-Typ = Routenbasiert <br>
Name der öffentlichen Gateway-IP = gwpip <br>
Lokales Netzwerkgateway = ClassicVNetLocal <br>

## <a name="a-namecreatesmgwasection-1-configure-classic-vnet-settings"></a><a name="createsmgw"></a>Abschnitt 1: Konfigurieren der Einstellungen für das klassische VNet
In diesem Abschnitt erstellen wir das lokale Netzwerk (den lokalen Standort) und das Gateway des virtuellen Netzwerks für das klassische VNET. In den Anweisungen in diesem Abschnitt wird das Azure-Portal verwendet. In den folgenden Schritten wird davon ausgegangen, dass kein VPN-Gateway für das klassische VNET erstellt wurde. Wenn Sie bereits über ein Gateway verfügen, stellen Sie sicher, dass es sich um ein dynamisches Gateway handelt. Ist es ein statisches, müssen Sie zuerst das VPN-Gateway löschen und anschließend mit den folgenden Schritten fortfahren.

### <a name="part-1---configure-the-local-site"></a>Teil 1 – Konfigurieren des lokalen Standorts

Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

1. Navigieren Sie zu **Alle Ressourcen**, und suchen Sie das klassische virtuelle Netzwerk.
2. Klicken Sie auf dem Blatt **Übersicht** im Abschnitt **VPN-Verbindungen** auf die Grafik **Gateway**, um ein Gateway zu erstellen.

    ![Konfigurieren von VPN-Gateways](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "Konfigurieren von VPN-Gateways")
3. Wählen Sie auf dem Blatt **Neue VPN-Verbindung** unter **Verbindungstyp** die Option **Standort-zu-Standort** aus.
4. Klicken Sie unter **Lokaler Standort** auf **Erforderliche Einstellungen konfigurieren**. Das Blatt **Lokaler Standort** wird geöffnet.
5. Erstellen Sie auf dem Blatt **Lokaler Standort** einen Namen, den Sie für Verweise auf das Resource Manager-VNET verwenden möchten. Beispiel: „RMVNetLocal“.
6. Wenn das VPN-Gateway für das Resource Manager-VNET bereits eine öffentliche IP-Adresse aufweist, verwenden Sie diesen Wert für das Feld **IP-Adresse des VPN-Gateways**. Wenn Sie noch kein Gateway des virtuellen Netzwerks für das Resource Manager-VNET erstellt haben, können Sie eine Platzhalter-IP-Adresse verwenden. Stellen Sie sicher, dass die Platzhalter-IP-Adresse ein gültiges Format aufweist. Später müssen Sie die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse des Gateways für das virtuelle Resource Manager-Netzwerk ersetzen.
7. Verwenden Sie für **Clientadressraum** die Werte für die IP-Adressräume des virtuellen Netzwerks für das Resource Manager-VNET. Diese Einstellung wird verwendet, um die Adressräume für die Weiterleitung an das RM-VNET anzugeben.
8. Klicken Sie auf **OK**, um die Werte zu speichern und zum Blatt **Neue VPN-Verbindung** zurückzukehren.

### <a name="part-2---create-the-virtual-network-gateway"></a>Teil 2 – Erstellen des Gateways für das virtuelle Netzwerk
1. Aktivieren Sie auf dem Blatt **Neue VPN-Verbindung** das Kontrollkästchen **Gateway sofort erstellen**, und klicken Sie auf **Optionale Gatewaykonfiguration**, um das Blatt **Gatewaykonfiguration** zu öffnen. 

    ![Öffnen des Blatts „Gatewaykonfiguration“](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "Öffnen des Blatts „Gatewaykonfiguration“")
2. Klicken Sie auf **Subnetz – Erforderliche Einstellungen konfigurieren**, um das Blatt **Subnetz hinzufügen** zu öffnen. Auf diesem Blatt sehen Sie, dass der Name bereits mit den erforderlichen Wert **GatewaySubnet** konfiguriert ist.
3. **Adressbereich** bezieht sich auf den Bereich für das Gatewaysubnetz. Sie können zwar ein Gatewaysubnetz mit dem Adressbereich /29 erstellen (drei Adressen), es wird jedoch empfohlen, ein Gatewaysubnetz zu erstellen, das mehr verfügbare IP-Adressen enthält, um für mögliche zukünftige Konfigurationen vorzusorgen, die mehr verfügbare IP-Adressen benötigen. Verwenden Sie möglichst /27 oder /28. Klicken Sie auf **OK**, um das Gatewaysubnetz zu erstellen.
4. Auf dem Blatt **Gatewaykonfiguration** bezieht sich **Größe** auf die Gateway-SKU. Wählen Sie die Gateway-SKU für das VPN-Gateway aus.
5. Vergewissern Sie sich, dass für **Routingtyp** der Wert **Dynamisch** festgelegt wurde, und klicken Sie auf **OK**, um zum Blatt **Neue VPN-Verbindung** zurückzukehren.
6. Klicken Sie auf dem Blatt **Neue VPN-Verbindung** auf **OK**, um mit dem Erstellen des VPN-Gateways zu beginnen. Dies kann bis zu 45 Minuten dauern.


### <a name="a-nameipapart-3---copy-the-virtual-network-gateway-public-ip-address"></a><a name="ip"></a>Teil 3 – Kopieren der öffentlichen IP-Adresse für das Gateway des virtuellen Netzwerks
Nachdem das Gateway für das virtuelle Netzwerk erstellt wurde, können Sie die Gateway-IP-Adresse anzeigen. 

1. Navigieren Sie zum klassischen VNET, und klicken Sie auf **Übersicht**.
2. Klicken Sie auf **VPN-Verbindungen**, um das Blatt „VPN-Verbindungen“ zu öffnen. Auf dem Blatt „VPN-Verbindungen“ können Sie die öffentliche IP-Adresse anzeigen. Dies ist die öffentliche IP-Adresse, die dem Gateway des virtuellen Netzwerks zugewiesen ist. Notieren Sie die IP-Adresse, oder kopieren Sie sie. Sie verwenden sie in späteren Schritten bei der Arbeit mit den Konfigurationseinstellungen für das lokale Resource Manager-Netzwerkgateway. Sie können auch den Status der Gatewayverbindungen anzeigen. Beachten Sie, dass der lokale Netzwerkstandort als „Verbindung wird hergestellt“ aufgeführt wird. Der Status ändert sich nach dem Erstellen der Verbindungen. Schließen Sie das Blatt, nachdem Sie die IP-Adresse des Gateways kopiert haben.

## <a name="a-namecreatermgwasection-2-configure-resource-manager-vnet-settings"></a><a name="creatermgw"></a>Abschnitt 2: Konfigurieren der Einstellungen für das Resource Manager-VNet
In diesem Abschnitt erstellen wir das Gateway für das virtuelle Netzwerk und das lokale Netzwerk für Ihr Resource Manager-VNET. Die Screenshots dienen lediglich zur Veranschaulichung. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Falls Sie die Konfiguration zu Übungszwecken erstellen, können Sie [diese Werte](#values)verwenden.

### <a name="part-1---create-a-gateway-subnet"></a>Teil 1 – Erstellen eines Gatewaysubnetzes
Bevor Sie ein Gateway für ein virtuelles Netzwerk erstellen, müssen Sie das Gatewaysubnetz erstellen. Erstellen Sie ein Gatewaysubnetz mit einem CIDR-Wert von /28 oder einem größeren Wert (/27, /26 usw.).

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Teil 2 – Erstellen eines virtuellen Netzwerkgateways
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

Die Erstellung eines Gateways für das virtuelle Netzwerk kann bis zu 45 Minuten dauern. Sie können warten, bis das Gateway das virtuellen Resource Manager-Netzwerks fertig erstellt wurde, oder Sie können mit [Teil 3 – Erstellen eines lokalen Netzwerkgateways](#createlng) fortfahren. Nachdem das Gateway erstellt wurde, wird ihm eine öffentliche IP-Adresse zugewiesen. In späteren Schritten wird diese IP-Adresse verwendet, um die Platzhalter-IP-Adressinformationen in den Einstellungen des lokalen Standorts für das klassische VNET, die Sie in Abschnitt 1 erstellt haben, zu ersetzen. 

### <a name="a-namecreatelngapart-3---create-a-local-network-gateway"></a><a name="createlng"></a>Teil 3 – Erstellen eines lokalen Netzwerkgateways

Das Gateway des lokalen Netzwerks legt den Adressbereich und die öffentliche IP-Adresse fest, die Ihrem klassischen VNET und Gateway des zugehörigen virtuellen Netzwerks zugeordnet sind.

- Verwenden Sie die öffentliche IP-Adresse, die dem Gateway Ihres klassischen VNET im [vorherigen Abschnitt](#ip)zugewiesen wurde. 
- Geben Sie für das lokale Netzwerkgateway einen Namen ein, anhand dessen Azure darauf verweisen kann. Beispiel: „ClassicVNetLocal“.
- Verwenden Sie den Adressraum, den Sie dem klassischen VNET (nicht nur dem Subnetz) zugewiesen haben.

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


## <a name="section-3-modify-the-classic-vnet-local-site-settings"></a>Abschnitt 3: Ändern der Einstellungen für den lokalen Standort des klassischen VNET

In diesem Abschnitt Arbeiten Sie mit dem klassischen VNET. Sie ersetzen die Platzhalter-IP-Adresse, die Sie beim Angeben der Einstellungen für den lokalen Standort verwendet haben. In diesem Abschnitt werden die klassischen PowerShell-Cmdlets (SM) verwendet. Wenn dies noch nicht geschehen ist, zeigen Sie das Gateway des virtuellen Netzwerks für das Resource Manager-VNET an, und kopieren Sie die öffentliche IP-Adresse. Dies ist die IP-Adresse, durch die Sie die Platzhalter-IP-Adresse ersetzen.

Stellen Sie sicher, dass Sie die neueste Version dieser Cmdlets heruntergeladen haben, wie im Abschnitt [Vorbereitungen](#before) beschrieben.

1. Navigieren Sie im Azure-Portal zum klassischen virtuellen Netzwerk.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk auf **Übersicht**.
3. Klicken Sie unter **VPN-Verbindungen** auf den Namen des lokalen Standorts in der Abbildung.

    ![VPN-Verbindungen](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-Verbindungen")
4. Klicken Sie auf dem Blatt **Standort-zu-Standort-VPN-Verbindungen** auf den Namen des Standorts.

    ![Standortname](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "Lokaler Standortname")
5. Klicken Sie auf dem Blatt „Verbindung“ für den lokalen Standort auf den Namen des lokalen Standorts, um das Blatt **Lokaler Standort** zu öffnen.

    ![Öffnen von „Lokaler Standort“](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "Öffnen von „Lokaler Standort“")
6. Ersetzen Sie auf dem Blatt **Lokaler Standort** die VPN-Gateway-IP-Adresse durch die IP-Adresse des Resource Manager-Gateways.

    ![Gateway-IP-Adresse](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "Gateway-IP-Adresse")
7. Klicken Sie auf **OK**, um die IP-Adresse zu aktualisieren.


## <a name="a-nameconnectasection-4-create-the-connection"></a><a name="connect"></a>Abschnitt 4: Erstellen der Verbindung
In diesem Abschnitt erstellen Sie die Verbindung zwischen den VNETs. Für diese Schritte ist PowerShell erforderlich. Die Verbindung kann in keinem der Portale erstellen werden. Stellen Sie sicher, dass Sie sowohl die SM-PowerShell-Cmdlets (klassisch) als auch die RM-PowerShell-Cmdlets (Resource Manager) heruntergeladen und installiert haben.

### <a name="part-1---log-in-to-your-azure-account"></a>Schritt 1 – Anmelden bei Ihrem Azure-Konto

1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und melden Sie sich bei Ihrem Azure-Konto an. Das folgende Cmdlet fordert Sie zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto auf. Nachdem Sie sich angemeldet haben, werden Ihre Kontoeinstellungen heruntergeladen, sodass sie in Azure PowerShell verfügbar sind.
   
        Login-AzureRmAccount 
   
2. Falls Sie mehrere Abonnements haben, benötigen Sie eine Liste Ihrer Azure-Abonnements.
   
        Get-AzureRmSubscription
   
3. Geben Sie das Abonnement an, das Sie verwenden möchten. 
   
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Fügen Sie Ihr Azure-Konto hinzu, um die klassischen PowerShell-Cmdlets zu verwenden. Dazu können Sie den folgenden Befehl verwenden:
   
        Add-AzureAccount

### <a name="part-2---download-your-network-configuration-file"></a>Teil 2 – Herunterladen der Netzwerkkonfigurationsdatei

In einigen Fällen werden die Namen für klassische VNETs und lokale Netzwerkstandorte in der Netzwerkkonfigurationsdatei geändert, wenn klassische VNETs im Azure-Portal erstellt werden. Ursache sind die Unterschiede in den Bereitstellungsmodellen. Mit dem Azure-Portal haben wir das klassische VNET beispielsweise „Classic VNet“ genannt und in einer Ressourcengruppe mit dem Namen „ClassicRG“ erstellt. Der in der Netzwerkkonfigurationsdatei enthaltene Name wird in „Group ClassicRG Classic VNet“ umgewandelt. Wenn Sie einen VNET-Namen angeben, der Leerzeichen enthält, verwenden Sie Anführungszeichen um den Wert.

1. Exportieren Sie Ihre Azure-Netzwerkkonfigurationsdatei, indem Sie den folgenden Befehl ausführen. Sie können den Speicherort der zu exportierenden Datei bei Bedarf ändern.
   
        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
3. Verwenden Sie einen Text-Editor wie z.B. Editor zum Öffnen der XML-Datei, und zeigen Sie den Inhalt an. Überprüfen Sie die Werte für die Namen des klassischen virtuellen Netzwerks und des lokalen Netzwerkstandorts.

### <a name="part-3---set-the-shared-key"></a>Teil 3 – Festlegen des gemeinsam verwendeten Schlüssels

Legen Sie den gemeinsam verwendeten Schlüssel für die Verbindung aus dem klassischen VNET zum Resource Manager-VNET fest. Überprüfen Sie bei der Verwendung dieser Cmdlets die Werte für `-VNetName` und `-LocalNetworkSiteName` mithilfe der Netzwerkkonfigurationsdatei. Wenn Sie Namen angeben, die Leerzeichen enthalten, verwenden Sie Anführungszeichen um den Wert. 

- In diesem Beispiel ist `-VNetName` der Name des klassischen VNET. 
- `-LocalNetworkSiteName` ist der Name, den Sie für den lokalen Standort angegeben haben.
- `-SharedKey` ist ein Wert, den Sie generieren und angeben. In diesem Beispiel wurde *abc123* verwendet, aber Sie können etwas Komplexeres generieren. In erster Linie muss der hier angegebene Wert mit dem Wert identisch sein, den Sie im nächsten Schritt beim Erstellen der Verbindung angeben.

Legen Sie den gemeinsam verwendeten Schlüssel in der PowerShell-Konsole durch Ausführen des folgenden Beispiels fest, und ersetzen Sie dabei die Werte durch eigene. Für dieses Beispiel sollte der Status **Erfolgreich**zurückgegeben werden.
   
        Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
        -LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
### <a name="part-4---create-the-vpn-connection-by-running-the-following-commands"></a>Teil 4 – Erstellen der VPN-Verbindung durch Ausführen der folgenden Befehle:
   
1. Legen Sie die Variablen fest.
   
        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
   
2. Erstellen Sie die Verbindung. In diesem Beispiel ist `-Name` der Name, den Sie Ihrer Verbindung geben möchten, nicht etwa ein bereits erstellter. Das folgende Beispiel erstellt eine Verbindung mit dem Namen „RM-Classic“. Beachten Sie, dass `-ConnectionType` „IPsec“ ist, nicht „Vnet2Vnet“, und dass `-SharedKey` dem Schlüssel entspricht, den Sie zuvor festgelegt haben.
   
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="section-5-verify-your-connections"></a>Abschnitt 5: Überprüfen der Verbindungen
Sie können die Verbindung im Azure-Portal oder mit PowerShell überprüfen. Beim Überprüfen müssen Sie möglicherweise eine Minute warten, während die Verbindung erstellt wird. Wenn eine Verbindung erfolgt ist, wechselt der Verbindungsstatus von „Verbindung wird hergestellt“ zu „Verbunden“.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>So überprüfen Sie die Verbindung vom klassischen VNET zum Resource Manager-VNET

####<a name="verify-your-connection-using-powershell"></a>Überprüfen der Verbindung mithilfe von PowerShell


[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

####<a name="verify-your-connection-using-the-azure-portal"></a>Überprüfen der Verbindung mithilfe des Azure-Portals

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>So überprüfen Sie die Verbindung vom Resource Manager-VNET zum klassischen VNET

####<a name="verify-your-connection-using-powershell"></a>Überprüfen der Verbindung mithilfe von PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

####<a name="verify-your-connection-using-the-azure-portal"></a>Überprüfen der Verbindung mithilfe des Azure-Portals

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="a-namefaqavnet-to-vnet-considerations"></a><a name="faq"></a>VNET-zu-VNET-Verbindungen

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]




<!--HONumber=Jan17_HO4-->


