### <a name="to-verify-your-connection-by-using-powershell"></a>So überprüfen Sie Ihre Verbindung mithilfe von PowerShell
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

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>So überprüfen Sie Ihre Verbindung mithilfe des Azure-Portals
Navigieren Sie im Azure-Portal zur gewünschten Verbindung, um den Verbindungsstatus anzuzeigen. Dazu gibt es verschiedene Möglichkeiten. Die folgenden Schritte zeigen eine Möglichkeit, zu Ihrer Verbindung zu navigieren und sie zu überprüfen.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com) auf **All resources** (Alle Ressourcen), und navigieren Sie zu Ihrem Gateway für virtuelle Netzwerke.
2. Klicken Sie auf dem Blatt für das Gateway für virtuelle Netzwerke auf **Verbindungen**. Der Status der einzelnen Verbindungen wird angezeigt.
3. Klicken Sie auf den Namen der zu überprüfenden Verbindung, um **Zusammenfassung** zu öffnen. Unter „Zusammenfassung“ können Sie weitere Informationen zu Ihrer Verbindung anzeigen. Der **Status** lautet „Erfolgreich“ und „Verbunden“, wenn die Verbindung erfolgreich hergestellt wurde.
   
    ![Überprüfen der Verbindung](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)



<!--HONumber=Nov16_HO2-->


