1. Navigieren Sie im Portal zu **Neu** > **Netzwerk** > **Virtuelles Netzwerkgateway**. Das Blatt **Virtuelles Netzwerkgateway erstellen** wird geöffnet.

	![Gateway](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. Geben Sie dem Gateway auf dem Blatt **Virtuelles Netzwerkgateway erstellen** im Feld **Name** einen Namen. Dies ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Dies ist der Name des Gatewayobjekts, das Sie erstellen.

3. Passen Sie das Feld **Standort** an, um auf den Standort zu verweisen, an dem sich das virtuelle Netzwerk befindet. Wenn Sie diesen Schritt nicht ausführen, wird das virtuelle Netzwerk nicht in der VNet-Liste angezeigt.
 
4. Wählen Sie als Nächstes das virtuelle Netzwerk aus, dem Sie dieses Gateway hinzufügen möchten. Klicken Sie auf **Virtuelles Netzwerk**, um das Blatt **Virtuelles Netzwerk auswählen** zu öffnen. Wählen Sie das VNet aus. Damit das VNet in der Liste angezeigt wird, muss es bereits über ein gültiges Gatewaysubnetz verfügen.

5. Wählen Sie eine öffentliche IP-Adresse aus. Klicken Sie auf **Öffentliche IP-Adresse**, um das Blatt **Öffentliche IP-Adresse wählen** zu öffnen. Klicken Sie auf **+ Neu erstellen**, um das Blatt **Öffentliche IP-Adresse erstellen** zu öffnen. Geben Sie einen Namen für die öffentliche IP-Adresse ein. Es wird ein Objekt für eine öffentliche IP-Adresse erstellt, dem eine öffentliche IP-Adresse dynamisch zugewiesen wird. <br>Klicken Sie zum Speichern der Änderungen auf **OK**.

5. Wählen Sie unter **Gatewaytyp** den Gatewaytyp aus, der für Ihre Konfiguration angegeben ist.

6. Wählen Sie unter **VPN-Typ** den VPN-Typ aus, der für Ihre Konfiguration angegeben ist.

7. Stellen Sie unter **Abonnement** sicher, dass das richtige Abonnement ausgewählt ist.

8. Die **Ressourcengruppe** wird anhand des virtuellen Netzwerks ermittelt, das Sie ausgewählt haben.

9. Passen Sie den **Standort** nicht mehr an, nachdem Sie die obigen Einstellungen angegeben haben.

10. An diesem Punkt ähnelt das Blatt der Grafik aus Schritt 1. Stellen Sie sicher, dass die Einstellungen mit den Einstellungen für Ihre Konfiguration übereinstimmen. Sie können die Option **An Dashboard anheften** unten auf dem Blatt auswählen, wenn das Gateway auf dem Dashboard angezeigt werden soll.

11. Klicken Sie auf **Erstellen**, um das Gateway zu erstellen. Die Einstellungen werden überprüft, und auf dem Dashboard wird die Kachel mit dem Hinweis angezeigt, dass das Gateway des virtuellen Netzwerks bereitgestellt wird. Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.

	![Gateway](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Nach der Erstellung des Gateways können Sie die zugewiesene IP-Adresse unter dem Virtual Network im Portal anzeigen. Das Gateway wird als verbundenes Gerät angezeigt. Sie können auf das verbundene Gerät (Ihr Virtual Network-Gateway) klicken, um weitere Informationen anzuzeigen.

<!---HONumber=AcomDC_0810_2016-->