### <a name="noconnection"></a>So ändern Sie die IP-Adresspräfixe eines Gateways des lokalen Netzwerks: Keine Gatewayverbindung

#### <a name="to-add-additional-address-prefixes"></a>So fügen Sie weitere Adresspräfixe hinzu:

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Fügen Sie den IP-Adressbereich im Feld *Zusätzlichen Adressbereich hinzufügen* hinzu.
3. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

#### <a name="to-remove-address-prefixes"></a>So entfernen Sie Adresspräfixe:

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Klicken Sie auf **...** in der Zeile mit dem Präfix, das Sie entfernen möchten.
3. Klicken Sie auf **Entfernen**.
4. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

### <a name="withconnection"></a>So ändern Sie die IP-Adresspräfixe eines Gateways des lokalen Netzwerks: Vorhandene Gatewayverbindung

Führen Sie die folgenden Schritte in der angegebenen Reihenfolge aus, wenn eine Gatewayverbindung besteht und Sie die IP-Adresspräfixe Ihres lokalen Netzwerkgateways hinzufügen oder entfernen möchten. Dies führt zu Ausfallzeiten bei Ihrer VPN-Verbindung. Beim Ändern von IP-Adresspräfixen müssen Sie das VPN-Gateway nicht löschen. Sie müssen nur die Verbindung entfernen.

#### <a name="1-remove-the-connection"></a>1. Entfernen Sie die Verbindung.

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway im Abschnitt **Einstellungen** auf **Verbindungen**.
2. Klicken Sie in den einzelnen Zeilen für die Verbindungen auf **...** und dann auf **Löschen**.
3. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

#### <a name="2-modify-the-address-prefixes"></a>2. Ändern Sie die Adresspräfixe.

So fügen Sie weitere Adresspräfixe hinzu:

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Fügen Sie den IP-Adressbereich hinzu.
3. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

So entfernen Sie Adresspräfixe:

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Klicken Sie in der Zeile mit dem Präfix, das Sie entfernen möchten, auf **...**.
3. Klicken Sie auf **Entfernen**.
4. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

#### <a name="3-recreate-the-connection"></a>3. Erstellen Sie die Verbindung erneut.

1. Navigieren Sie zum Gateway für virtuelle Netzwerke für Ihr VNet. (Dies ist nicht das lokale Netzwerkgateway.)
2. Klicken Sie für das lokale Netzwerkgateway im Abschnitt **Einstellungen** auf **Verbindungen**.
3. Klicken Sie auf **+ Hinzufügen**, um das Blatt **Verbindung hinzufügen** zu öffnen.
4. Erstellen Sie die Verbindung erneut.
5. Klicken Sie auf **OK**, um die Verbindung zu erstellen.