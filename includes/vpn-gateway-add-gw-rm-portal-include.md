1. Wechseln Sie im Portal zu **Neu** und dann zu **Netzwerk**. Wählen Sie in der Liste den Eintrag **Gateway von Virtual Network**.

	![Gateway](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. Geben Sie dem Gateway im Blatt **Gateway von Virtual Network erstellen** im Feld **Name** einen Namen. Dies ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Dies ist der Name des Gatewayobjekts.
 
3. Wählen Sie als Nächstes das virtuelle Netzwerk aus, in dem Sie dieses Gateway bereitstellen möchten. Klicken Sie auf den Pfeil, um das Blatt **Virtual Network auswählen** zu öffnen, und klicken Sie dann auf das VNet. Damit das VNet in der Liste angezeigt wird, muss es bereits über ein gültiges Gatewaysubnetz verfügen.

4. Wählen Sie eine öffentliche IP-Adresse aus. Klicken Sie auf den Pfeil, um das Blatt **Öffentliche IP-Adresse auswählen** zu öffnen. Klicken Sie dann auf **Neu erstellen**, um das Blatt **Öffentliche IP-Adresse erstellen** zu öffnen. Geben Sie einen Namen für die öffentliche IP-Adresse ein. Beachten Sie, dass hierbei nicht nach einer IP-Adresse gefragt wird. Die IP-Adresse wird dynamisch zugewiesen. Hierbei handelt es sich um den Namen des IP-Adressenobjekts, dem die Adresse zugewiesen wird. Klicken Sie zum Speichern der Änderungen auf **OK**.

5. Wählen Sie unter **Gatewaytyp** den Gatewaytyp aus, der für Ihre Konfiguration angegeben ist.

6. Wählen Sie unter **VPN-Typ** den VPN-Typ aus, der für Ihre Konfiguration angegeben ist.

7. Stellen Sie unter **Abonnement** sicher, dass das richtige Abonnement ausgewählt ist.

8. Für **Ressourcengruppe** wird die Ressourcengruppe anhand des Virtual Network ermittelt, das Sie ausgewählt haben.

9. Stellen Sie unter **Standort** sicher, dass der Standort angegeben ist, an dem sich sowohl die Ressourcengruppe als auch das VNet befinden.

10. Sie können die Option **An Dashboard anheften** auswählen, wenn das Gateway auf dem Dashboard angezeigt werden soll. Klicken Sie auf **Erstellen**, um das Gateway zu erstellen. Die Kachel „Virtual Network-Gateway bereitstellen“ wird auf dem Dashboard angezeigt. Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Im Hintergrund werden viele Vorgänge durchgeführt. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.

	
	![Gateway](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Nach der Erstellung des Gateways können Sie die zugewiesene IP-Adresse unter dem Virtual Network im Portal anzeigen. Das Gateway wird als verbundenes Gerät angezeigt. Sie können auf das verbundene Gerät (Ihr Virtual Network-Gateway) klicken, um weitere Informationen anzuzeigen.



