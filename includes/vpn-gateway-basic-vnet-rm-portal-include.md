Führen Sie zum Erstellen eines VNet mit dem Azure-Portal die folgenden Schritte aus. Beachten Sie, dass die Screenshots als Beispiele dienen. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Weitere Informationen zur Arbeit mit virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../articles/virtual-network/virtual-networks-overview.md).

1. Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com), und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.

2. Klicken Sie auf **Neu** **>** **Netzwerk** **>** **Virtuelles Netzwerk**.

	![VNetBlade](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal650.png)

3. Wählen Sie unten auf dem Blatt „Virtuelles Netzwerk“ in der Liste **Bereitstellungsmodell auswählen** die Option **Resource Manager**, und klicken Sie dann auf **Erstellen**.


	![„Ressourcen-Manager“ auswählen](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png)

4. Konfigurieren Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die VNet-Einstellungen. In diesem Blatt fügen Sie Ihren ersten Adressraum und einen Adressbereich des Subnetzes hinzu. Nachdem Sie die Erstellung des VNet abgeschlossen haben, können Sie zurückgehen und weitere Subnetze und Adressräume hinzufügen. Dies ist eine aktuelle Beschränkung des Portals. Sie können immer zurückkehren, um diese Werte zu aktualisieren, indem Sie die VNet-Eigenschaften im Portal oder mit PowerShell bearbeiten. Die Werte, die Sie verwenden, richten sich nach der zu erstellenden Konfiguration. Achten Sie darauf, dass Sie Ihre geplanten Konfigurationswerte verwenden.

	![Blatt "Erstellen eines virtuellen Netzwerks"](./media/vpn-gateway-basic-vnet-rm-portal-include/createavnet250.png)

5. Überprüfen Sie, ob es sich um das richtige **Abonnement** handelt. Sie können Abonnements in der Dropdownliste ändern.

6. Klicken Sie auf **Ressourcengruppe**, und wählen Sie entweder eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue, indem Sie einen Namen für die neue Ressourcengruppe eingeben. Geben Sie der Ressourcengruppe beim Erstellen einer neuen Gruppe einen Namen, der zu Ihren geplanten Konfigurationswerten passt. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](resource-group-overview.md#resource-groups).

7. Wählen Sie als Nächstes für das VNet die Einstellungen für **Standort** aus. Beachten Sie, dass mit dem Standort angegeben wird, wo sich die in diesem VNet bereitgestellten Ressourcen befinden. Sie können dies später nicht mehr ändern, ohne die Ressourcen neu bereitzustellen.

8. Wählen Sie **An Dashboard anheften**, wenn das VNet auf dem Dashboard leicht zugänglich sein soll, und klicken Sie dann auf **Erstellen**.
	
	![An Dashboard anheften](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png)


9. Nach dem Klicken auf „Erstellen“ wird auf dem Dashboard eine Kachel angezeigt, mit der der Status des VNet angegeben wird. Die Kachel ändert sich, wenn das VNet erstellt wird.

	![Kachel "Erstellen eines virtuellen Netzwerks"](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png)