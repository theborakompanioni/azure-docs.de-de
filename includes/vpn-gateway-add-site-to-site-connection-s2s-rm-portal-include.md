1. Suchen Sie nach Ihrem Gateway für das virtuelle Netzwerk.
2. Klicken Sie auf **Verbindungen**. Klicken Sie oben auf dem Blatt „Verbindungen“ auf **+Hinzufügen**, um das Blatt **Verbindung hinzufügen** zu öffnen.
   
    ![Einrichten einer Standort-zu-Standort-Verbindung](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection.png)
3. Geben Sie auf dem Blatt **Verbindung hinzufügen** einen **Namen** für die Verbindung ein. 
4. Wählen Sie als **Verbindungstyp** die Option **Site-to-Site (IPSec)** aus.
5. Für **Virtuelles Netzwerkgateway**ist der Wert festgelegt, da Sie von diesem Gateway aus die Verbindung herstellen.
6. Klicken Sie unter **Lokales Netzwerkgateway** auf E**in lokales Netzwerkgateway auswählen**, und wählen Sie das lokale Netzwerkgateway aus, das Sie verwenden möchten. 
7. Unter **Gemeinsam verwendeter Schlüssel** muss der hier angegebene Wert mit dem Wert übereinstimmen, den Sie für Ihr lokales VPN-Gerät verwenden. Im Beispiel wurde „abc123“ verwendet, aber Sie können (und sollten) einen komplexeren Wert verwenden. Entscheidend ist Folgendes: Der Wert, den Sie hier angeben, muss dem Wert entsprechen, den Sie beim Konfigurieren Ihres VPN-Geräts angegeben haben.
8. Die verbleibenden Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** wurden korrigiert.
9. Klicken Sie auf **OK** , um die Verbindung zu erstellen. Auf dem Bildschirm blinkt der Hinweis *Verbindung wird erstellt*.
10. Nachdem die Verbindung hergestellt wurde, wird sie auf dem Blatt **Verbindungen** des Gateways für virtuelle Netzwerke angezeigt.
    
    ![Einrichten einer Standort-zu-Standort-Verbindung](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connectionstatus450.png)

