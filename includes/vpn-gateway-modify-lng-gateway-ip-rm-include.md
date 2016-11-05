Verwenden Sie zum Ändern der Gateway-IP-Adresse das `New-AzureRmVirtualNetworkGatewayConnection`-Cmdlet. Wenn Sie den Namen des lokalen Netzwerkgateways unverändert lassen, werden die Einstellungen überschrieben. Das Ändern der Gateway-IP-Adresse wird vom Cmdlet „Set“ derzeit nicht unterstützt.

### <a name="gwipnoconnection"></a>Gewusst wie: Ändern der Die Gateway-IP-Adresse ohne Gatewayverbindung
Verwenden Sie das weiter unten angegebene Beispiel, um die Gateway-IP-Adresse für ein lokales Netzwerkgateway ohne Verbindung zu aktualisieren. Bei dieser Gelegenheit können Sie auch die Adresspräfixe aktualisieren. Die vorhandenen Einstellungen werden mit den angegebenen Einstellungen überschrieben. Achten Sie darauf, den Namen Ihres lokalen Netzwerkgateways zu verwenden. Andernfalls wird nicht das bereits vorhandene lokale Netzwerkgateway überschrieben, sondern ein neues erstellt.

Verwenden Sie das folgende Beispiel, und ersetzen Sie dabei die Werte durch Ihre eigenen:

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Gewusst wie: Ändern der Die Gateway-IP-Adresse mit Gatewayverbindung
Ist bereits eine Gatewayverbindung vorhanden, muss sie zuerst entfernt werden. Danach können Sie dann die Gateway-IP-Adresse ändern und eine neue Verbindung erstellen. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung.

> [!IMPORTANT]
> Löschen Sie nicht das VPN-Gateway. Andernfalls müssen Sie die Schritte zum Erstellen erneut ausführen und das Gateway außerdem auf Ihrem lokalen Router mit der IP-Adresse konfigurieren, die dem neu erstellten Gateway zugewiesen wird.
> 
> 

1. Entfernen Sie die Verbindung. Den Namen der Verbindung können Sie mithilfe des `Get-AzureRmVirtualNetworkGatewayConnection`-Cmdlets ermitteln.
   
        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName
2. Ändern Sie den GatewayIpAddress-Wert. Bei dieser Gelegenheit können Sie ggf. auch die Adresspräfixe ändern. Beachten Sie, dass dadurch die vorhandenen Einstellungen des lokalen Netzwerkgateways überschrieben werden. Verwenden Sie zum Vornehmen von Änderungen den vorhandenen Namen des lokalen Netzwerkgateways, damit die Einstellungen überschrieben werden. Andernfalls wird nicht das bereits vorhandene lokale Netzwerkgateway geändert, sondern ein neues erstellt.
   
        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
3. Erstellen Sie die Verbindung. In diesem Beispiel konfigurieren wir einen IPsec-Verbindungstyp. Verwenden Sie beim erneuten Erstellen der Verbindung den für Ihre Konfiguration angegebenen Verbindungstyp. Weitere Verbindungstypen finden Sie auf der Seite [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx). Der VirtualNetworkGateway-Name kann mithilfe des `Get-AzureRmVirtualNetworkGateway`-Cmdlets abgerufen werden.
   
    Legen Sie die Variablen fest:
   
        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
   
    Erstellen Sie die Verbindung:
   
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

<!---HONumber=AcomDC_0810_2016-->