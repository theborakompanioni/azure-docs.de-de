Für die Durchführung der Schritte für diese Aufgabe benötigen Sie ein VNet mit den folgenden Eigenschaften. Zusätzliche Einstellungen und Namen werden ebenfalls in dieser Liste beschrieben. Wir verwenden diese Liste nicht direkt in einem der Schritte, obwohl wir Variablen basierend auf den Werten in dieser Liste hinzufügen. Sie können diese Liste als Referenz verwenden und die Werte durch Ihre eigenen Werte ersetzen.

Konfiguration der Referenzliste:

* Name des virtuellen Netzwerks = TestVNet
* Adressraum des virtuellen Netzwerks: 192.168.0.0/16
* Ressourcengruppe = TestRG
* Name Subnet1 = FrontEnd 
* Adressraum Subnet1 = 192.168.0.0/16
* Name des Gatewaysubnetzes: GatewaySubnet. Sie müssen ein Gatewaysubnetz immer *GatewaySubnet* nennen.
* Adressraum des Gatewaysubnetzes = 192.168.200.0/26
* Region = USA, Osten
* Name des Gateways = GW
* Name der Gateway-IP = GWIP
* Name der Gateway-IP-Konfiguration = gwipconf
* Typ = ExpressRoute. Dieser Typ ist für eine ExpressRoute-Konfiguration erforderlich.
* Name der öffentlichen Gateway-IP = gwpip

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways
1. Stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her. 
   
        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"
2. Definieren Sie Ihre Variablen für diese Übung. Für dieses Beispiel werden die Variablen im nachstehenden Beispiel verwendet. Achten Sie darauf, dies so zu bearbeiten, dass es den Einstellungen entspricht, die Sie verwenden möchten. 
   
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"
3. Speichern Sie das virtuelle Netzwerkobjekt als Variable.
   
        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
4. Fügen Sie Ihrem virtuellen Netzwerk ein Gatewaysubnetz hinzu. Das Gatewaysubnetz muss den Namen GatewaySubnet tragen. Sie möchten ein Gateway erstellen, das /27 oder größer (/26, /25, etc.) ist.
   
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
5. Legen Sie die Konfiguration fest.
   
            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
6. Speichern Sie das Gatewaysubnetz als Variable.
   
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
7. Fordern Sie eine öffentliche IP-Adresse an. Die IP-Adresse wird angefordert, bevor das Gateway erstellt wird. Sie können die IP-Adresse, die Sie verwenden möchten, nicht selbst angeben. Sie wird dynamisch zugewiesen. Diese IP-Adresse wird im nächsten Konfigurationsabschnitt verwendet. Die AllocationMethod muss dynamisch sein.
   
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
8. Erstellen Sie die Konfiguration für Ihr Gateway. Die Gatewaykonfiguration definiert das zu verwendende Subnetz und die zu verwendende öffentliche IP-Adresse. In diesem Schritt geben Sie die Konfiguration an, die beim Erstellen des Gateways verwendet wird. Dieser Schritt erstellt das Gatewayobjekt nicht tatsächlich. Verwenden Sie das folgende Beispiel, um Ihre Gatewaykonfiguration zu erstellen. 
   
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
9. Erstellen Sie das Gateway. In diesem Schritt ist **-GatewayType** besonders wichtig. Sie müssen den Wert **ExpressRoute**verwenden. Beachten Sie, dass das Erstellen des Gateways nach der Ausführung dieser Cmdlets 20 Minuten oder länger dauern kann.
   
        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Erstellung des Gateways überprüfen
Verwenden Sie den folgenden Befehl, um zu überprüfen, ob das Gateway erstellt wurde.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways
Es gibt eine Reihe von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Sie können jederzeit den folgenden Befehl verwenden, um die Gateway-SKU zu ändern.

> [!IMPORTANT]
> Dieser Befehl kann nicht für das UltraPerformance-Gateway verwendet werden. Wenn Sie das Gateway in ein UltraPerformance-Gateway ändern möchten, entfernen Sie zunächst das vorhandene ExpressRoute-Gateway, und erstellen Sie anschließend ein neues UltraPerformance-Gateway. Wenn Sie das Gateway von einem UltraPerformance-Gateway herabstufen möchten, entfernen Sie zunächst das UltraPerformance-Gateway, und erstellen Sie anschließend ein neues Gateway.
> 
> 

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways
Verwenden Sie den folgenden Befehl, um ein Gateway zu entfernen.

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  


<!--HONumber=Nov16_HO3-->


