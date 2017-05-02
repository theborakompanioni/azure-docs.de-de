Wechseln Sie zurück zum Browserfenster mit dem Azure-Portal.

#### <a name="add-hostname"></a>Hostnamen hinzufügen

Klicken Sie auf das **+**-Symbol neben **Hostnamen hinzufügen**.

![Hostnamen hinzufügen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

#### <a name="validate-hostname"></a>Hostnamen überprüfen

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie zuvor den CNAME-Eintrag konfiguriert haben (z.B. `www.contoso.com`), und klicken Sie dann auf **Überprüfen**.

Wählen Sie unter der Überschrift **Typ des Hostnamenseintrags** die Option **CNAME (www.beispiel.com oder eine beliebige Unterdomäne)**.

Klicken Sie auf **Hostnamen hinzufügen**, um den DNS-Namen zu Ihrer App hinzuzufügen.

![DNS-Namen zur App hinzufügen](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Möglicherweise dauert es eine Weile, bis der neue Hostnamen auf der Seite **Benutzerdefinierte Domänen** Ihrer App berücksichtigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.