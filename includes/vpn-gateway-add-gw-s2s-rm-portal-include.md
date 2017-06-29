1. Klicken Sie im Portal auf der linken Seite auf **+**, und geben Sie „Gateway für virtuelle Netzwerke“ für die Suche ein. Suchen Sie unter **Ergebnisse** nach **Gateway für virtuelle Netzwerke**, und klicken Sie auf den Eintrag.
2. Klicken Sie am unteren Rand des Blatts „Gateway für virtuelle Netzwerke“ auf **Erstellen**. Das Blatt **Virtuelles Netzwerkgateway erstellen** wird geöffnet.

    ![Felder des Blatts „Gateway für virtuelle Netzwerke erstellen“](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Neues Gateway")

3. Geben Sie auf dem Blatt **Gateway für virtuelle Netzwerke erstellen** die Werte für das virtuelle Netzwerkgateway an.

  - **Name**: Benennen Sie Ihr Gateway. Dies ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Hierbei handelt es sich um den Namen des Gatewayobjekts, das Sie erstellen.
  - **Gatewaytyp**: Wählen Sie **VPN** aus. Bei VPN-Gateways wird ein virtuelles Netzwerkgateway vom Typ **VPN** verwendet. 
  - **VPN-Typ**: Wählen Sie den für Ihre Konfiguration angegebenen VPN-Typ aus. Bei den meisten Konfigurationen wird ein routenbasierter VPN-Typ benötigt.
  - **SKU**: Wählen Sie in der Dropdownliste die Gateway-SKU aus. Welche SKUs in der Dropdownliste aufgeführt werden, hängt vom ausgewählten VPN-Typ ab. Weitere Informationen zu Gateway-SKUs finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Standort**: Unter Umständen müssen Sie einen Bildlauf durchführen, um diese Option anzuzeigen. Passen Sie das Feld **Standort** an, um auf den Standort zu verweisen, an dem sich das virtuelle Netzwerk befindet. Wenn der Standort nicht auf die Region verweist, in der sich Ihr virtuelles Netzwerk befindet, wird das virtuelle Netzwerk im nächsten Schritt nicht in der Dropdownliste „Virtuelles Netzwerk auswählen“ angezeigt.
  - **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, dem Sie dieses Gateway hinzufügen möchten. Klicken Sie auf **Virtuelles Netzwerk**, um das Blatt „Virtuelles Netzwerk auswählen“ zu öffnen. Wählen Sie das VNet aus. Sollte Ihr VNet nicht angezeigt werden, vergewissern Sie sich, dass das Feld „Speicherort“ auf die Region verweist, in der sich Ihr virtuelles Netzwerk befindet.
  - **Öffentliche IP-Adresse**: Mit dem Blatt „Öffentliche IP-Adresse erstellen“ wird ein Objekt für eine öffentliche IP-Adresse erstellt. Die öffentliche IP-Adresse wird bei der Erstellung des VPN-Gateways dynamisch zugewiesen. VPN Gateway unterstützt derzeit nur die *dynamische* Zuweisung öffentlicher IP-Adressen. Das bedeutet jedoch nicht, dass sich die IP-Adresse ändert, nachdem sie Ihrem VPN-Gateway zugewiesen wurde. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

    - Klicken Sie zunächst auf **Öffentliche IP-Adresse**, um das Blatt „Öffentliche IP-Adresse wählen“ zu öffnen, und klicken Sie dann auf **+ Neu erstellen**, um das Blatt „Öffentliche IP-Adresse erstellen“ zu öffnen.
    - Geben Sie dann für die öffentliche IP-Adresse einen Wert für **Name** ein, und klicken Sie dann unten auf dem Blatt auf **OK**, um die Änderungen zu speichern.

      ![Erstellen der öffentlichen IP-Adresse](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "Erstellen der PIP")

4. Überprüfen Sie die Einstellungen. Sie können die Option **An Dashboard anheften** unten auf dem Blatt auswählen, wenn das Gateway auf dem Dashboard angezeigt werden soll. 
5. Klicken Sie auf **Erstellen**, um das VPN-Gateway zu erstellen. Die Einstellungen werden überprüft, und auf dem Dashboard wird die Kachel mit dem Hinweis angezeigt, dass das Gateway des virtuellen Netzwerks bereitgestellt wird. Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.

Zeigen Sie nach der Erstellung des Gateways unter dem virtuellen Netzwerk im Portal die zugewiesene IP-Adresse an. Das Gateway wird als verbundenes Gerät angezeigt. Sie können auf das verbundene Gerät (Ihr virtuelles Netzwerkgateway) klicken, um weitere Informationen anzuzeigen.