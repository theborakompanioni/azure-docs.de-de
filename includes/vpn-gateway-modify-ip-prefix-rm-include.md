### <a name="noconnection"></a>Gewusst wie: Hinzufügen oder Entfernen von Präfixen ohne VPN Gateway-Verbindung

- Verwenden Sie das unten angegebene Beispiel, um zusätzliche Adresspräfixe einem lokalen Netzwerkgateway **hinzuzufügen**, das Sie erstellt haben, aber das noch nicht über eine Verbindung per VPN Gateway verfügt.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- Verwenden Sie das unten angegebene Beispiel, um ein Adresspräfix aus einem lokalen Netzwerkgateway **zu entfernen**, das noch nicht über eine VPN-Verbindung verfügt. Lassen Sie die Präfixe weg, die Sie nicht mehr benötigen. In diesem Beispiel wird das Präfix 20.0.0.0/24 (aus dem vorherigen Beispiel) nicht mehr benötigt. Daher wird das lokale Netzwerkgateway aktualisiert und das Präfix weggelassen.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Gewusst wie: Hinzufügen oder Entfernen von Präfixen mit VPN Gateway-Verbindung

Wenn Sie die VPN-Verbindung erstellt haben und die IP-Adresspräfixe Ihres lokalen Netzwerkgateways hinzufügen oder entfernen möchten, müssen Sie die folgenden Schritte der Reihenfolge nach ausführen. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung. Beim Aktualisieren der Präfixe entfernen Sie zuerst die Verbindung und ändern die Präfixe und erstellen anschließend eine neue Verbindung.

>[AZURE.IMPORTANT] Löschen Sie nicht das VPN-Gateway. Wenn Sie dies tun, müssen Sie die Schritte zum Erstellen erneut durchführen und das Gateway außerdem auf Ihrem lokalen Router mit den neuen Einstellungen erneut konfigurieren.
 
1. Geben Sie die Variablen an.

		$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

2. Entfernen Sie die Verbindung.

		Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

3. Ändern Sie die Präfixe.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
		-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Erstellen Sie die Verbindung. In diesem Beispiel konfigurieren wir einen IPsec-Verbindungstyp. Weitere Verbindungstypen finden Sie auf der Seite [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx).
	
		New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
		-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec `
		-RoutingWeight 10 -SharedKey 'abc123'

<!---HONumber=AcomDC_0803_2016-->