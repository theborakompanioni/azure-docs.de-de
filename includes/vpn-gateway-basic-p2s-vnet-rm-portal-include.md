Wenn Sie mit dem Azure-Portal ein VNET im Resource Manager-Bereitstellungsmodell erstellen, führen Sie die Schritte unten aus. Die Screenshots dienen als Beispiele. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Weitere Informationen zur Arbeit mit virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../articles/virtual-network/virtual-networks-overview.md).

1. Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie unten auf der Seite auf **+**. Geben Sie im Feld **Marketplace durchsuchen** die Zeichenfolge „Virtual Network“ ein. Klicken Sie in der zurückgegebenen Liste auf **Virtual Network**, um die Seite **Virtual Network** zu öffnen.

  ![Zur Seite mit den Ressourcen des virtuellen Netzwerks navigieren](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/newvnetportal700.png "Zur Seite mit den Ressourcen des virtuellen Netzwerks navigieren")
3. Wählen Sie unten auf der Seite „Virtuelles Netzwerk“ in der Liste **Bereitstellungsmodell auswählen** die Option **Resource Manager** aus, und klicken Sie dann auf **Erstellen**.

  ![Resource Manager auswählen](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Resource Manager auswählen")
4. Konfigurieren Sie auf der Seite **Virtuelles Netzwerk erstellen** die VNET-Einstellungen. Beim Ausfüllen der Felder ändert sich das rote Ausrufezeichen in ein grünes Häkchen, sofern die in das Feld eingegebenen Zeichen zulässig sind. Unter Umständen wurden bestimmte Werte bereits automatisch ausgefüllt. Ersetzen Sie sie in diesem Fall durch Ihre eigenen Werte. Die Seite **Virtuelles Netzwerk erstellen** sieht in etwa wie im folgenden Beispiel aus:

  ![Feldüberprüfung](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/createp2sgvnet.png "Feldüberprüfung")
5. **Name:** Geben Sie den Namen für Ihr virtuelles Netzwerk ein.
6. **Adressraum:** Geben Sie den Adressraum ein. Falls Sie mehrere Adressräume hinzufügen möchten, fügen Sie Ihren ersten Adressraum hinzu. Weitere Adressräume können später (nach Erstellung des VNets) hinzugefügt werden.
7. **Subnetzname:** Fügen Sie Name und Adressbereich des Subnetzes hinzu. Weitere Subnetze können später (nach Erstellung des VNets) hinzugefügt werden.
8. **Abonnement:** Vergewissern Sie sich, dass das richtige Abonnement angegeben ist. Das Abonnement kann über die Dropdownliste geändert werden.
9. **Ressourcengruppe:** Wählen Sie entweder eine bereits vorhandene Ressourcengruppe aus, oder geben Sie einen Namen für eine neue Ressourcengruppe ein. Geben Sie der Ressourcengruppe beim Erstellen einer neuen Gruppe einen Namen, der zu Ihren geplanten Konfigurationswerten passt. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
10. **Standort:** Wählen Sie den Standort für Ihr VNET aus. Der Standort gibt an, wo sich die in diesem VNet bereitgestellten Ressourcen befinden.
11. Wählen Sie **An Dashboard anheften** aus, wenn das VNET komfortabel auf dem Dashboard zur Verfügung stehen soll, und klicken Sie dann auf **Erstellen**.

 ![An Dashboard anheften](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "An Dashboard anheften")
12. Nach dem Klicken auf **Erstellen** wird auf dem Dashboard eine Kachel angezeigt, auf der der Status des VNET angegeben wird. Die Kachel verändert sich, wenn das VNet erstellt wird.

  ![Kachel „Virtuelles Netzwerk wird erstellt“](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Kachel „Virtuelles Netzwerk wird erstellt“")