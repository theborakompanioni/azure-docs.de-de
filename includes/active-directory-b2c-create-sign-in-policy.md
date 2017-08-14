Für die Anmeldung bei Ihrer Anwendung müssen Sie eine Anmelderichtlinie erstellen. Diese Richtlinie beschreibt die Benutzeroberflächen, die Kunden bei der Anmeldung durchlaufen, und den Inhalt der Token, die die Anwendung bei erfolgreichen Anmeldungen erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)] Klicken Sie auf **Anmelderichtlinien**.

Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .

Der **Name** bestimmt den Namen der Anmelderichtlinie, die von der Anwendung verwendet wird. Geben Sie beispielsweise **SiIn** ein.

Klicken Sie auf **Identitätsanbieter**, und wählen Sie **Anmeldung für lokales Konto** aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.

Klicken Sie auf **Anwendungsansprüche**. Hier wählen Sie die Ansprüche aus, die in den Token nach einer erfolgreichen Anmeldung an die Anwendung zurückgegeben werden sollen. Wählen Sie beispielsweise **Anzeigename**, **Identitätsanbieter**, **Postleitzahl** und **Objekt-ID des Benutzers** aus. Klicken Sie auf **OK**.

Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als **B2C_1_SiIn** (das Fragment **B2C\_1\_** wird automatisch hinzugefügt) auf dem Blatt **Anmelderichtlinien** angezeigt wird.

Öffnen Sie die Richtlinie, indem Sie auf **B2C_1_SiIn** klicken.

Wählen Sie in der Dropdownliste **Anwendungen** die App **Contoso B2C** und `https://localhost:44321/` in der Dropdownliste **Reply URL/Redirect URI** aus.

Klicken Sie auf **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Anmeldung bei Ihrer Anwendung durchgehen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
>