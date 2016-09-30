1. Navigieren Sie im Azure-Portal zu **Neu** > **Netzwerk** > **Lokales Netzwerkgateway**.

	![Erstellen eines Gateways für das lokale Netzwerk](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Geben Sie auf dem Blatt **Lokales Netzwerkgateway erstellen** unter **Name** einen Namen für das Objekt Ihres lokalen Netzwerkgateways ein.
 
3. Geben Sie für das VPN-Gerät oder das virtuelle Netzwerkgateway, mit dem Sie eine Verbindung herstellen möchten, eine gültige öffentliche **IP-Adresse** ein.<br>Falls es sich bei diesem lokalen Netzwerk um einen lokalen Ort handelt, ist dies die öffentliche IP-Adresse des VPN-Geräts, mit dem Sie eine Verbindung herstellen möchten. Sie darf sich nicht hinter einer NAT befinden und muss für Azure erreichbar sein.<br>Falls es sich bei diesem lokalen Netzwerk um ein anderes VNet handelt, geben Sie die öffentliche IP-Adresse an, die dem virtuellen Netzwerkgateway für dieses VNet zugewiesen wurde.<br>

4. **Adressraum** bezieht sich auf die Adressbereiche für das Netzwerk, das dieses lokale Netzwerk darstellt. Sie können mehrere Adressraumbereiche hinzufügen. Achten Sie darauf, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke überschneiden, mit denen Sie eine Verbindung herstellen möchten.
 
5. Stellen Sie unter **Abonnement** sicher, dass das richtige Abonnement angezeigt wird.

6. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine neue Ressourcengruppe erstellen oder eine auswählen, die Sie bereits erstellt haben.

7. Wählen Sie unter **Speicherort** den Ort aus, an dem dieses Objekt erstellt wird. Es empfiehlt sich unter Umständen, den gleichen Ort auszuwählen, an dem sich auch Ihr VNet befindet, dies ist aber nicht zwingend erforderlich.

8. Klicken Sie auf **Erstellen**, um das lokale Netzwerkgateway zu erstellen.

<!-----HONumber=AcomDC_0810_2016-->