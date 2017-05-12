### <a name="gwipnoconnection"></a> So ändern Sie die Angabe für „GatewayIpAddress“ für ein Gateway des lokalen Netzwerks: Keine Gatewayverbindung

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen. Ändern Sie das Gateway eines lokalen Netzwerks, für das keine Gatewayverbindung besteht, anhand des Beispiels.

Bei dieser Gelegenheit können Sie auch die Adresspräfixe ändern. Achten Sie darauf, dass Sie den Namen des Gateways des lokalen Netzwerks verwenden, um die aktuellen Einstellungen zu überschreiben. Wenn Sie einen anderen Namen verwenden, wird nicht das bereits vorhandene Gateway des lokalen Netzwerks überschrieben, sondern ein neues erstellt.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>So ändern Sie die Angabe für „GatewayIpAddress“ für ein Gateway des lokalen Netzwerks: Vorhandene Gatewayverbindung

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen. Ist bereits eine Gatewayverbindung vorhanden, muss sie zuerst entfernt werden. Nachdem die Verbindung entfernt wurde, können Sie die Gateway-IP-Adresse ändern und eine neue Verbindung erstellen. Bei dieser Gelegenheit können Sie auch die Adresspräfixe ändern. Dies führt zu Ausfallzeiten bei Ihrer VPN-Verbindung. Beim Ändern der Gateway-IP-Adresse müssen Sie das VPN-Gateway nicht löschen. Sie müssen nur die Verbindung entfernen.
 

1. Entfernen Sie die Verbindung. Den Namen Ihrer Verbindung können Sie mithilfe des Cmdlets „Get-AzureRmVirtualNetworkGatewayConnection“ ermitteln.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Ändern Sie den GatewayIpAddress-Wert. Bei dieser Gelegenheit können Sie auch die Adresspräfixe ändern. Achten Sie darauf, dass Sie den Namen Ihres lokalen Netzwerkgateways verwenden, um die aktuellen Einstellungen zu überschreiben. Andernfalls wird nicht das bereits vorhandene lokale Netzwerkgateway überschrieben, sondern ein neues erstellt.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Erstellen Sie die Verbindung. In diesem Beispiel konfigurieren wir einen IPsec-Verbindungstyp. Verwenden Sie beim erneuten Erstellen der Verbindung den für Ihre Konfiguration angegebenen Verbindungstyp. Weitere Verbindungstypen finden Sie auf der Seite [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) .  Der VirtualNetworkGateway-Name kann mithilfe des Cmdlets „Get-AzureRmVirtualNetworkGateway“ abgerufen werden.
   
    Legen Sie die Variablen fest.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Erstellen Sie die Verbindung.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```