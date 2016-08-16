### So überprüfen Sie Ihre Verbindung mithilfe von PowerShell

Mit dem `Get-AzureRmVirtualNetworkGatewayConnection`-Cmdlet (mit oder ohne `-Debug`) können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde.

1. Verwenden Sie das folgende Cmdlet-Beispiel, und konfigurieren Sie die Werte so, dass sie Ihren eigenen Werten entsprechen. Wählen Sie ggf. die Option „A“ für „Alle ausführen“ aus. In dem Beispiel verweist `-Name` auf den Namen der Verbindung, die Sie erstellt haben und testen möchten.

		Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Sehen Sie sich nach Abschluss des Cmdlets die Werte an. Im Beispiel weiter unten ist der Verbindungsstatus als „Connected“ (Verbunden) angegeben, und Sie sehen die Eingangs- und Ausgangsbytes.

		Body:
		{
		  "name": "MyGWConnection",
		  "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
		  "properties": {
		    "provisioningState": "Succeeded",
		    "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
		    "virtualNetworkGateway1": {
		      "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
		teways/vnetgw1"
		    },
		    "localNetworkGateway2": {
		      "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
		ways/LocalSite"
		    },
		    "connectionType": "IPsec",
		    "routingWeight": 10,
		    "sharedKey": "abc123",
		    "connectionStatus": "Connected",
		    "ingressBytesTransferred": 33509044,
		    "egressBytesTransferred": 4142431
		  }

### So überprüfen Sie Ihre Verbindung mithilfe des Azure-Portals

Navigieren Sie im Azure-Portal zur gewünschten Verbindung, um den Verbindungsstatus anzuzeigen. Dazu gibt es verschiedene Möglichkeiten. Im Anschluss folgt eine davon.

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zu **Virtuelle Netzwerkgateways**. Klicken Sie auf den Namen Ihres Gateways.
2. Klicken Sie im Bereich unter **Einstellungen** auf **Verbindungen**. Der Status der einzelnen Verbindungen wird angezeigt.
3. Klicken Sie auf den Namen der Verbindung, um weitere Informationen zur Verbindung anzuzeigen. Beachten Sie den **Verbindungsstatus** auf der Zusammenfassungsseite für die Verbindung. Der Status lautet „Erfolgreich“ und „Verbunden“, wenn die Verbindung erfolgreich hergestellt wurde. Die Angaben unter **Eingehende Daten** und **Ausgehende Daten** geben Aufschluss über den Datenfluss.

	Im folgenden Beispiel lautet der Verbindungsstatus**Nicht verbunden**:

	![Überprüfen der Verbindung](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)

<!---HONumber=AcomDC_0810_2016-->