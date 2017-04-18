### <a name="noconnection"></a>Gewusst wie: Hinzufügen oder Entfernen von Präfixen ohne Gatewayverbindung
### <a name="to-add-additional-prefixes"></a>So fügen Sie zusätzliche Präfixe hinzu

Verwenden Sie das unten angegebene Beispiel, um einem von Ihnen erstellten lokalen Netzwerkgateway, das noch nicht über eine Gatewayverbindung verfügt, zusätzliche Präfixe hinzuzufügen. Legen Sie die Werte auf Ihre eigenen Werte fest.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```
### <a name="to-remove-an-address-prefix"></a>So entfernen Sie ein Adresspräfix

Verwenden Sie das unten angegebene Beispiel, um ein Adresspräfix aus einem lokalen Netzwerkgateway zu entfernen, das noch nicht über eine VPN-Verbindung verfügt. Lassen Sie die Präfixe weg, die Sie nicht mehr benötigen. In diesem Beispiel wird das Präfix 20.0.0.0/24 (aus dem vorherigen Beispiel) nicht mehr benötigt. Daher wird das lokale Netzwerkgateway aktualisiert und das Präfix weggelassen.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <a name="withconnection"></a>Gewusst wie: Hinzufügen oder Entfernen von Präfixen mit Gatewayverbindung
Führen Sie die folgenden Schritte in der angegebenen Reihenfolge aus, wenn Sie die Gatewayverbindung erstellt haben und die IP-Adresspräfixe Ihres lokalen Netzwerkgateways hinzufügen oder entfernen möchten. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung. Beim Aktualisieren der Präfixe entfernen Sie zuerst die Verbindung und ändern die Präfixe und erstellen anschließend eine neue Verbindung. Legen Sie die Werte in den bereitgestellten Beispielen auf Ihre eigenen Werte fest.

> [!IMPORTANT]
> Löschen Sie nicht das VPN-Gateway. Wenn Sie dies tun, müssen Sie die Schritte zum Erstellen erneut durchführen und das Gateway außerdem auf Ihrem lokalen Router mit den neuen Einstellungen erneut konfigurieren.
> 
> 

1. Entfernen Sie die Verbindung.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Ändern Sie die IP-Adresspräfixe für das lokale Netzwerkgateway.
   
  Legen Sie die Variable für das lokale Netzwerkgateway (LocalNetworkGateway) fest.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Ändern Sie die Präfixe.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Erstellen Sie die Verbindung. In diesem Beispiel konfigurieren wir einen IPsec-Verbindungstyp. Verwenden Sie beim erneuten Erstellen der Verbindung den für Ihre Konfiguration angegebenen Verbindungstyp. Weitere Verbindungstypen finden Sie auf der Seite [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) .
   
  Legen Sie die Variable für das virtuelle Netzwerkgateway (VirtualNetworkGateway) fest.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Erstellen Sie die Verbindung. Beachten Sie, dass in diesem Beispiel die $local-Variable verwendet wird, die Sie im vorherigen Schritt festgelegt haben.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```
