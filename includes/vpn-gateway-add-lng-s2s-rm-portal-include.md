1. Klicken Sie im Portal im Abschnitt **Alle Ressourcen** auf **+Hinzufügen**. 
2. Geben Sie im Suchfeld des Blatts **Alles** den Text **Lokales Netzwerkgateway** ein, und klicken Sie dann auf „Suchen“. Eine Liste wird zurückgegeben. Klicken Sie auf **Lokales Netzwerkgateway**, um das Blatt zu öffnen. Klicken Sie dann auf **Erstellen**, um das Blatt **Lokales Netzwerkgateway erstellen** zu öffnen.

  ![Erstellen eines Gateways für das lokale Netzwerk](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)

3. Geben Sie auf dem Blatt **Lokales Netzwerkgateway erstellen** die Werte für das lokale Netzwerkgateway ein.

  - **Name:** Geben Sie einen Namen für das lokale Netzwerkgatewayobjekt ein.
  - **IP-Adresse:** Dies ist die öffentliche IP-Adresse des VPN-Geräts, mit dem Azure eine Verbindung herstellen soll. Geben Sie eine gültige öffentliche IP-Adresse ein. Die IP-Adresse darf sich nicht hinter einer NAT befinden und muss für Azure erreichbar sein. Wenn Sie zurzeit nicht über die IP-Adresse verfügen, können Sie die Werte im Screenshot verwenden. Sie müssen dann aber später die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse Ihres VPN-Geräts ersetzen. Andernfalls kann Azure keine Verbindung herstellen.
  - **Adressraum** bezieht sich auf die Adressbereiche für das Netzwerk, das dieses lokale Netzwerk darstellt. Sie können mehrere Adressraumbereiche hinzufügen. Achten Sie darauf, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke überschneiden, mit denen Sie eine Verbindung herstellen möchten. Azure leitet den Adressbereich, den Sie angeben, an die lokale IP-Adresse des VPN-Geräts weiter. *Verwenden Sie hier Ihre eigenen Werte und nicht die Werte im Screenshot*.
  - **Abonnement:** Stellen Sie sicher, dass das richtige Abonnement angezeigt wird.
  - **Ressourcengruppe:** Wählen Sie die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine neue Ressourcengruppe erstellen oder eine auswählen, die Sie bereits erstellt haben.
  - **Speicherort:** Wählen Sie den Speicherort aus, an dem dieses Objekt erstellt wird. Es empfiehlt sich unter Umständen, den gleichen Ort auszuwählen, an dem sich auch Ihr VNet befindet, dies ist aber nicht zwingend erforderlich.

4. Wenn Sie die Werte angegeben haben, klicken Sie unten auf dem Blatt auf **Erstellen**, um das lokale Netzwerkgateway zu erstellen.