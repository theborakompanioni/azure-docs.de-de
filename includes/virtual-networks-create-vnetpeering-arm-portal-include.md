## Erstellen von VNet-Peering im Azure-Portal
Gehen Sie wie folgt vor, um mit dem Azure-Portal ein VNet-Peering basierend auf dem oben beschriebenen Szenario zu erstellen.

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Zum Einrichten des VNet-Peerings müssen Sie zwischen zwei VNets zwei Links erstellen, also einen für jede Richtung. Sie können den VNet-Peeringlink zuerst für VNet1 zu VNet2 erstellen. Klicken Sie im Portal auf **Durchsuchen** > **Virtuelle Netzwerke**.
   
    ![VNet-Peering im Azure-Portal erstellen](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure01.png)
3. Wählen Sie auf dem Blatt „Virtuelle Netzwerke“ die Option „VNet1“ aus, und klicken Sie auf „Peerings“ und dann auf „Hinzufügen“.
   
    ![Peering auswählen](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure02.png)
4. Vergeben Sie auf dem Blatt „Peering hinzufügen“ für das Peering den Linknamen „LinkToVnet2“, wählen Sie das Abonnement und das zu verknüpfende virtuelle Netzwerk „VNet2“ aus, und klicken Sie auf „OK“.
   
    ![Link zu VNet](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure03.png)
5. Gehen Sie nach der Erstellung dieses VNet-Peeringlinks wie unten angegeben vor. Der Linkstatus wird wie folgt angezeigt:
   
    ![Linkstatus](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure04.png)
6. Erstellen Sie als Nächstes den VNet-Peeringlink für VNet2 zu VNet1. Wählen Sie auf dem Blatt „Virtuelle Netzwerke“ die Option „VNet2“ aus, und klicken Sie auf „Peerings“ und dann auf „Hinzufügen“.
   
    ![Peer aus anderem VNet](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure05.png)
7. Vergeben Sie auf dem Blatt „Peering hinzufügen“ für das Peering den Linknamen „LinkToVnet1“, wählen Sie das Abonnement und das zu verknüpfende virtuelle Netzwerk aus, und klicken Sie auf „OK“.
   
    ![Kachel "Erstellen eines virtuellen Netzwerks"](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure06.png)
8. Gehen Sie nach der Erstellung dieses VNet-Peeringlinks wie unten angegeben vor. Der Linkstatus wird wie folgt angezeigt:
   
    ![Endgültiger Linkstatus](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure07.png)
9. Überprüfen Sie den Status von „LinkToVnet2“, der sich jetzt auch in „Verbunden“ ändert.
   
    ![Endgültiger Linkstatus 2](./media/virtual-networks-create-vnetpeering-arm-pportal-include/figure08.png)
10. HINWEIS: Das VNet-Peering wird nur eingerichtet, wenn beide Links verbunden sind.

<!---HONumber=AcomDC_0803_2016-->