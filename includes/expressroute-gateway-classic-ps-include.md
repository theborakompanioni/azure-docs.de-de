Sie müssen zuerst ein VNet und ein Gatewaysubnetz erstellen, bevor Sie an den folgenden Aufgaben arbeiten. Weitere Informationen finden Sie im Artikel [Konfigurieren eines Virtual Network mit dem klassischen Portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) .   

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways
Verwenden Sie den folgenden Befehl, um ein Gateway zu erstellen. Achten Sie darauf, dass Sie die vorhandenen Werte durch Ihre eigenen Werte ersetzen.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Erstellung des Gateways überprüfen
Verwenden Sie den folgenden Befehl, um zu überprüfen, ob das Gateway erstellt wurde. Mit diesem Befehl wird auch die Gateway-ID abgerufen, die Sie für andere Vorgänge benötigen.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways
Es gibt eine Reihe von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Sie können jederzeit den folgenden Befehl verwenden, um die Gateway-SKU zu ändern.

> [!IMPORTANT]
> Dieser Befehl kann nicht für das UltraPerformance-Gateway verwendet werden. Wenn Sie das Gateway in ein UltraPerformance-Gateway ändern möchten, entfernen Sie zunächst das vorhandene ExpressRoute-Gateway, und erstellen Sie anschließend ein neues UltraPerformance-Gateway. Wenn Sie das Gateway von einem UltraPerformance-Gateway herabstufen möchten, entfernen Sie zunächst das UltraPerformance-Gateway, und erstellen Sie anschließend ein neues Gateway. 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways
Verwenden Sie den folgenden Befehl, um ein Gateway zu entfernen.

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>