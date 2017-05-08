Verwenden Sie das Cmdlet „New-AzureRmVirtualNetworkGatewayConnection“, um die Gateway-IP-Adresse zu ändern. Das Ändern der Gateway-IP-Adresse wird vom Cmdlet „Set“ derzeit nicht unterstützt.

### <a name="gwipnoconnection"></a>Ändern der Gateway-IP-Adresse – keine Gatewayverbindung
Verwenden Sie das weiter unten angegebene Beispiel, um die Gateway-IP-Adresse für ein lokales Netzwerkgateway ohne Verbindung zu ändern. Bei dieser Gelegenheit können Sie auch die Adresspräfixe ändern. Achten Sie darauf, dass Sie den Namen Ihres lokalen Netzwerkgateways verwenden, um die aktuellen Einstellungen zu überschreiben. Andernfalls wird nicht das bereits vorhandene lokale Netzwerkgateway überschrieben, sondern ein neues erstellt.

Verwenden Sie das folgende Beispiel, und ersetzen Sie dabei die Werte durch Ihre eigenen:

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>Ändern der Gateway-IP-Adresse – vorhandene Gatewayverbindung
Ist bereits eine Gatewayverbindung vorhanden, muss sie zuerst entfernt werden. Nachdem die Verbindung entfernt wurde, können Sie die Gateway-IP-Adresse ändern und eine neue Verbindung erstellen. Bei dieser Gelegenheit können Sie auch die Adresspräfixe ändern. Dies führt zu Ausfallzeiten bei Ihrer VPN-Verbindung.

> [!IMPORTANT]
> Löschen Sie nicht das VPN-Gateway. Ansonsten müssen Sie die Schritte erneut ausführen, um es neu zu erstellen. Außerdem müssen Sie Ihr lokales VPN-Gerät mit der neuen IP-Adresse des VPN-Gateways aktualisieren.
> 
> 

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