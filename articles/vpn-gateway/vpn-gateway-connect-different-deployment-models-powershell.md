<properties 
   pageTitle="Verbinden von klassischen virtuellen Netzwerken mit virtuellen Resource Manager-Netzwerken mit PowerShell | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie mithilfe von VPN Gateway und PowerShell eine VPN-Verbindung zwischen klassischen VNets und Resource Manager-VNets erstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="cherylmc" />

# Verbinden von virtuellen Netzwerken aus verschiedenen Bereitstellungsmodellen mit PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

In Azure sind zurzeit zwei Verwaltungsmodelle verfügbar: klassisches Modell und RM-Modell (Resource Manager). Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. In diesem Artikel wird Schritt für Schritt erläutert, wie Sie klassische VNets mit Resource Manager-VNets verbinden, damit die Ressourcen in den separaten Bereitstellungsmodellen über eine Gatewayverbindung miteinander kommunizieren können.

Sie können auch eine Verbindung zwischen VNets in unterschiedlichen Abonnements, Regionen und Bereitstellungsmodellen erstellen. Es ist auch möglich, VNets zu verbinden, die bereits über Verbindungen mit lokalen Netzwerken verfügen, sofern sie mit einem dynamischen oder routenbasierten Gateway konfiguriert wurden. Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen](#faq). 
[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## Vorbereitungen

Die folgenden Schritte führen Sie durch die erforderlichen Einstellungen zum Konfigurieren eines dynamischen oder routenbasierten Gateways für jedes VNet und die Erstellung einer VPN-Verbindung zwischen den Gateways. Diese Konfiguration unterstützt keine statischen oder richtlinienbasierten Gateways.

Überprüfen Sie vor Beginn Folgendes:

 - Beide VNets wurden bereits erstellt.
 - Die Adressbereiche für die VNets überlappen weder einander noch die Bereiche für andere Verbindungen, mit denen die Gateways verbunden werden können.
 - Sie haben die aktuellen PowerShell-Cmdlets (1.0.2 oder höher) installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Installieren Sie sowohl die SM-Cmdlets (Dienstverwaltung) als auch die RM-Cmdlets (Resource Manager).

### <a name="exampleref"></a>Beispieleinstellungen

Sie können die Beispieleinstellungen als Referenz verwenden, wenn Sie in den folgenden Schritten die PowerShell-Cmdlets ausführen.

**Einstellungen für das klassische VNet**

VNet-Name = ClassicVNet <br> Standort = USA, Westen <br> Adressräume von Virtual Network = 10.0.0.0/24 <br> Subnetz-1 = 10.0.0.0/27 <br> Gatewaysubnetz = 10.0.0.32/29 <br> Name des lokalen Netzwerks = RMVNetLocal <br> Gatewaytyp = DynamicRouting

**Einstellungen für das Resource Manager-VNet**

VNet-Name = RMVNet <br> Ressourcengruppe = RG1 <br> IP-Adressräume von Virtual Network = 192.168.1.0/16 <br> Subnetz-1 = 192.168.1.0/24 <br> Gatewaysubnetz = 192.168.0.0/26 <br> Standort = USA, Osten <br> Öffentlicher IP-Name des Gateways = gwpip <br> Lokales Netzwerkgateway = ClassicVNetLocal <br> Name des virtuellen Netzwerkgateways = RMGateway <br> Gateway-IP-Adressierungskonfiguration = gwipconfig


## <a name="createsmgw"></a>Abschnitt 1 – Konfigurieren des klassischen VNet

### Teil 1 – Herunterladen der Netzwerkkonfigurationsdatei

1. Melden Sie sich in der PowerShell-Konsole bei Ihrem Azure-Konto an. Sie werden von diesem Cmdlet zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert. Nach dem Anmelden werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen. Für diesen Teil der Konfiguration verwenden Sie die SM-PowerShell-Cmdlets.

		Add-AzureAccount

2. Exportieren Sie Ihre Azure-Netzwerkkonfigurationsdatei, indem Sie den folgenden Befehl ausführen. Sie können den Speicherort der zu exportierenden Datei bei Bedarf ändern. Sie bearbeiten die Datei und importieren sie anschließend in Azure.

		Get-AzureVNetConfig -ExportToFile c:\AzureNet\NetworkConfig.xml

3. Öffnen Sie die heruntergeladene XML-Datei, um sie zu bearbeiten. Ein Beispiel für die Netzwerkkonfigurationsdatei finden Sie unter [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx) (Netzwerkkonfigurationsschema).
 

### Teil 2 – Überprüfen des Gatewaysubnetzes

Fügen Sie Ihrem VNet im Element **VirtualNetworkSites** ein Gatewaysubnetz hinzu, sofern noch keines erstellt wurde. Bei der Verwendung der Netzwerkkonfigurationsdatei MUSS für das Gatewaysubnetz der Name „GatewaySubnet“ verwendet werden. Andernfalls kann es von Azure nicht als Gatewaysubnetz erkannt und verwendet werden.
	
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### Teil 3 – Hinzufügen des lokalen Netzwerkstandorts

Der lokale Netzwerkstandort, den Sie hinzufügen, stellt das RM-VNet dar, mit dem Sie eine Verbindung herstellen möchten. Sofern noch nicht vorhanden, müssen Sie der Datei ein Element **LocalNetworkSites** hinzufügen. An diesem Punkt der Konfiguration kann für die „VPNGatewayAddress“ eine beliebige gültige öffentliche IP-Adresse verwendet werden, weil wir das Gateway für das Resource Manager-VNet noch nicht erstellt haben. Nachdem wir das Gateway erstellt haben, ersetzen wir diese Platzhalter-IP-Adresse durch die richtige öffentliche IP-Adresse, die dem RM-Gateway zugewiesen wurde.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.1.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### Teil 4 – Zuordnen des VNet zum lokalen Netzwerkstandort

In diesem Abschnitt geben wir den lokalen Netzwerkstandort an, mit dem Sie das VNet verbinden möchten. In diesem Fall handelt es sich dabei um das Resource Manager-VNet, auf das Sie zuvor verwiesen haben. Stellen Sie sicher, dass die Namen übereinstimmen. In diesem Schritt wird kein Gateway erstellt. Er dient zum Angeben des lokalen Netzwerks, mit dem das Gateway eine Verbindung herstellt.

		<Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### Teil 5 – Speichern und Hochladen der Datei

Speichern Sie die Datei, und importieren Sie sie dann in Azure, indem Sie den folgenden Befehl ausführen. Stellen Sie sicher, dass Sie den Dateipfad wie für Ihre Umgebung erforderlich ändern.

		Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\ClassicVNet.xml

Das Ergebnis, durch das der erfolgreiche Abschluss des Imports bestätigt wird, sollte in etwa wie folgt aussehen.

		OperationDescription        OperationId                      OperationStatus                                                
		--------------------        -----------                      ---------------                                                
		Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### Teil 6 – Erstellen des virtuellen Netzwerkgateways 

Sie können das virtuelle Netzwerkgateway im klassischen Portal oder mit PowerShell erstellen. Derzeit ist es nicht möglich, ein klassisches Gateway im Azure-Portal zu erstellen.

#### So erstellen Sie das Gateway im klassischen Portal

1. Navigieren Sie im [klassischen Portal](https://manage.windowsazure.com) zu **Netzwerke**, und klicken Sie auf das klassische VNet, für das Sie ein virtuelles Netzwerkgateway erstellen möchten. Die Hauptseite für Ihr VNet wird geöffnet.
2. Klicken Sie oben auf der Seite auf **Dashboard**, um zur Seite „Dashboard“ zu wechseln.
3. Klicken Sie unten auf der Seite „Dashboard“ auf **Gateway erstellen** und dann auf **Dynamisches Routing**.
4. Klicken Sie auf **Ja**, um mit der Erstellung des Gateways zu beginnen.
5. Warten Sie, bis das Gateway erstellt wurde. Dieser Vorgang kann manchmal 45 Minuten oder sogar länger dauern.
6. Nachdem das Gateway erstellt wurde, können Sie die Gateway-IP-Adresse auf der Seite **Dashboard** anzeigen. Dies ist die öffentliche IP-Adresse Ihres Gateways. Notieren Sie die öffentliche IP-Adresse, oder kopieren Sie sie. Sie verwenden diese Adresse später beim Erstellen des lokalen Netzwerks für die Resource Manager-VNet-Konfiguration.
7. Optional können Sie das folgende Cmdlet verwenden, um die Gatewayeinstellungen abzurufen und den Status des Gateways zu überprüfen: `Get-AzureVirtualNetworkGateway`

#### So erstellen Sie das Gateway mit PowerShell

Verwenden Sie das folgende Beispiel, um das Gateway mithilfe von PowerShell zu erstellen.

	New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting


## <a name="creatermgw"></a>Abschnitt 2: Konfigurieren des RM-VNet-Gateways

Gehen Sie wie im Folgenden beschrieben vor, um ein VPN-Gateway für das RM-VNet zu erstellen. Bevor Sie diese Schritte ausführen, müssen Sie die öffentliche IP-Adresse für das Gateway des klassischen VNet abrufen.

1. Melden Sie sich in der PowerShell-Konsole bei Ihrem **Azure-Konto** an. Das folgende Cmdlet fordert Sie zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto auf. Nachdem Sie sich angemeldet haben, werden Ihre Kontoeinstellungen heruntergeladen, sodass sie in Azure PowerShell verfügbar sind.

		Login-AzureRmAccount 

 	Falls Sie mehrere Abonnements haben, benötigen Sie eine Liste Ihrer Azure-Abonnements.

		Get-AzureRmSubscription

	Geben Sie das Abonnement an, das Sie verwenden möchten.

		Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Erstellen Sie ein lokales Netzwerkgateway**. In einem virtuellen Netzwerk ist mit dem Gateway für das lokale Netzwerk in der Regel Ihr lokaler Standort gemeint. In diesem Fall ist mit dem lokalen Netzwerkgateway Ihr klassisches VNet gemeint. Geben Sie für das Gateway einen Namen ein, anhand dessen Azure darauf verweisen kann, und geben Sie auch das Präfix für den Adressraum an. Azure verwendet das IP-Adresspräfix, um zu ermitteln, welcher Datenverkehr an Ihren lokalen Standort gesendet werden soll. Falls Sie diese Informationen später (vor dem Erstellen des Gateways) anpassen müssen, können Sie die Werte ändern und das Beispiel erneut ausführen.<br><br>
	- `-Name` ist der Name, den Sie dem lokalen Netzwerkgateway zuweisen möchten.<br>
	- `-AddressPrefix` ist der Adressraum für Ihr klassisches VNet.<br>
	- `-GatewayIpAddress` ist die öffentliche IP-Adresse des Gateways für das klassische VNet. Denken Sie daran, das folgende Beispiel mit der richtigen IP-Adresse zu ändern.
	
			New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
			-Location "West US" -AddressPrefix "10.0.0.0/24" `
			-GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Fordern Sie eine öffentliche IP-Adresse an**, die dem VPN-Gateway für Ihr Azure Resource Manager-VNet zugeordnet wird. Es ist nicht möglich, die IP-Adresse selbst anzugeben. Sie wird Ihrem Gateway dynamisch zugewiesen. Das bedeutet jedoch nicht, dass sich die IP-Adresse ändert. Die IP-Adresse des Azure-VPN-Gateways ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das Azure-VPN-Gateway durchgeführt werden.<br>In diesem Schritt legen wir auch eine Variable fest, die in einem späteren Schritt verwendet wird.


		$ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
		-ResourceGroupName RG1 -Location 'EastUS' `
		-AllocationMethod Dynamic

4. **Überprüfen Sie, ob Ihr virtuelles Netzwerk über ein Gatewaysubnetz verfügt**. Falls kein Gatewaysubnetz vorhanden ist, fügen Sie es hinzu. Stellen Sie sicher, dass für das Gatewaysubnetz der Name *GatewaySubnet* angegeben ist.

5. **Rufen Sie das Subnetz ab**, das für das Gateway verwendet wird, indem Sie den folgenden Befehl ausführen. In diesem Schritt legen wir auch eine Variable fest, die im nächsten Schritt verwendet wird.
	- `-Name` ist der Name Ihres Resource Manager-VNet.
	- `-ResourceGroupName` ist die Ressourcengruppe, der das VNet zugeordnet ist. Das Gatewaysubnetz für dieses VNet muss bereits vorhanden sein und den Namen *GatewaySubnet* haben, damit es einwandfrei funktioniert.


			$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
			-VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Erstellen der Gateway-IP-Adressierungskonfiguration**. Die Gatewaykonfiguration definiert das zu verwendende Subnetz und die zu verwendende öffentliche IP-Adresse. Verwenden Sie das folgende Beispiel, um Ihre Gatewaykonfiguration zu erstellen.<br>In diesem Schritt müssen die ID-Eigenschaft aus dem Subnetz bzw. die IP-Adressobjekte an die Parameter `-SubnetId` und `-PublicIpAddressId` übergeben werden. Sie können keine einfache Zeichenfolge verwenden. Diese Variablen werden im Schritt zum Anfordern einer öffentlichen IP-Adresse und im Schritt zum Abrufen des Subnetzes festgelegt.

		$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
		-Name gwipconfig -SubnetId $subnet.id `
		-PublicIpAddressId $ipaddress.id
	
7. **Erstellen Sie das Gateway für das Resource Manager-VNet**, indem Sie den folgenden Befehl ausführen. `-VpnType` muss auf *RouteBased* festgelegt sein.

		New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
		-Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
		-IpConfigurations $gwipconfig `
		-EnableBgp $false -VpnType RouteBased

8. **Kopieren Sie die öffentliche IP-Adresse**, nachdem das VPN-Gateway erstellt wurde. Diese Adresse verwenden Sie beim Konfigurieren der lokalen Netzwerkeinstellungen für Ihr klassisches VNet. Sie können das folgende Cmdlet verwenden, um die öffentliche IP-Adresse abzurufen. Die öffentliche IP-Adresse ist in der Ausgabe als *IpAddress* aufgeführt.

		Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## Abschnitt 3: Ändern des lokalen Netzwerks für das klassische VNet

Zum Ausführen dieses Schritts können Sie entweder die Netzwerkkonfigurationsdatei exportieren, bearbeiten, speichern und wieder in Azure importieren oder die Einstellung im klassischen Portal ändern.

### So nehmen Sie die Änderung im Portal vor

1. Öffnen Sie das [klassische Portal](https://manage.windowsazure.com).

2. Scrollen Sie im klassischen Portal auf der linken Seite nach unten, und klicken Sie auf **Netzwerke**. Klicken Sie oben auf der Seite **Netzwerke** auf **Lokale Netzwerke**.

3. Wählen Sie auf der Seite **Lokale Netzwerke** das lokale Netzwerk aus, das Sie in Teil 1 konfiguriert haben, und klicken Sie dann unten auf der Seite auf **Bearbeiten**.

4. Ersetzen Sie auf der Seite **Details zum lokalen Netzwerk angeben** die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse für das Resource Manager-Gateway, das Sie im vorherigen Abschnitt erstellt haben. Klicken Sie auf den Pfeil, um zum nächsten Abschnitt zu gelangen. Überprüfen Sie, ob der **Adressraum** korrekt ist, und klicken Sie dann auf das Häkchen, um die Änderungen zu übernehmen.

### So nehmen Sie die Änderung mithilfe der Netzwerkkonfigurationsdatei vor

1. Exportieren Sie die Netzwerkkonfigurationsdatei.
2. Ändern Sie in einem Text-Editor die IP-Adresse des VPN-Gateways.

	    <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Speichern Sie die Änderungen, und importieren Sie die bearbeitete Datei wieder in Azure.


## <a name="connect"></a>Abschnitt 4: Erstellen einer Verbindung zwischen den Gateways

Zum Erstellen einer Verbindung zwischen den Gateways ist PowerShell erforderlich. Sie müssen Ihr Azure-Konto hinzufügen, um die klassischen PowerShell-Cmdlets zu verwenden. Dazu können Sie das folgende Cmdlet verwenden:
		
	Add-AzureAccount

1. **Legen Sie Ihren gemeinsam verwendeten Schlüssel fest**, indem Sie den folgenden Befehl ausführen. In diesem Beispiel ist `-VNetName` der Name des klassischen VNet und `-LocalNetworkSiteName` der Name, den Sie bei der Konfiguration des lokalen Netzwerks im klassischen Portal für das lokale Netzwerk angegeben haben. `-SharedKey` ist ein Wert, den Sie generieren und angeben können. Der hier angegebene Wert muss mit dem Wert identisch sein, den Sie im nächsten Schritt beim Erstellen der Verbindung angeben. Für dieses Beispiel sollte der Status „Succesful“ zurückgegeben werden.

		Set-AzureVNetGatewayKey -VNetName ClassicVNet `
		-LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Erstellen Sie die VPN-Verbindung, indem Sie die folgenden Befehle ausführen.

	**Festlegen der Variablen**

		$vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
		$vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

	**Erstellen der Verbindung**<br> Beachten Sie, dass `-ConnectionType` „IPsec“ ist, und nicht „Vnet2Vnet“.
		
		New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
		-Location "East US" -VirtualNetworkGateway1 `
		$vnet02gateway -LocalNetworkGateway2 `
		$vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Sie können die Verbindung im Portal oder mit dem Cmdlet `Get-AzureRmVirtualNetworkGatewayConnection` anzeigen.

		Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen

Zeigen Sie die Details zu den häufig gestellten Fragen an, um zusätzliche Informationen zu VNet-zu-VNet-Verbindungen zu erhalten.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

<!---HONumber=AcomDC_0810_2016-->
