1. Navigieren Sie im [Portal](http://portal.azure.com) zum virtuellen Resource Manager-Netzwerk, für das Sie ein virtuelles Netzwerkgateway erstellen möchten.
2. Klicken Sie auf der VNET-Seite im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite **Subnetze** zu erweitern.
3. Klicken Sie auf der Seite **Subnetze** auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen. 

  ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Hinzufügen des Gatewaysubnetzes")
4. Als **Name** für Ihr Subnetz wird automatisch der Wert „GatewaySubnet“ eingefügt. Dieser Wert ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** gemäß Ihren Konfigurationsanforderungen an, und klicken Sie unten auf der Seite auf **OK**, um das Subnetz zu erstellen.

  ![Hinzufügen des Subnetzes](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Hinzufügen des Subnetzes")