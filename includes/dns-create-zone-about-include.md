Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt.

Beispiel: Die Domäne „contoso.com“ kann eine Reihe von DNS-Einträgen wie „mail.contoso.com“ (für einen E-Mail-Server) und „www.contoso.com“ (für eine Website) enthalten.

Beim Erstellen einer DNS-Zone in Azure DNS gilt Folgendes:

* Der Name der Zone muss innerhalb der Ressourcengruppe eindeutig sein. Außerdem darf die Zone noch nicht vorhanden sein. Andernfalls ist der Vorgang nicht erfolgreich.
* Der gleiche Zonennamen kann in einer anderen Ressourcengruppe oder einem anderen Azure-Abonnement erneut verwendet werden.
* Wenn mehrere Zonen den gleichen Namen haben, erhält jede Instanz eine andere Adresse für den Namenserver. Mit der Domänennamen-Registrierungsstelle kann nur ein Satz von Adressen konfiguriert werden.

> [!NOTE]
> Sie müssen keinen Domänennamen besitzen, um eine DNS-Zone mit diesem Domänennamen in Azure DNS zu erstellen. Sie müssen jedoch die Domäne besitzen, um die Azure DNS-Namenserver als korrekte Namenserver für den Domänennamen mit der Domänennamen-Registrierungsstelle zu konfigurieren.
> 
> Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](../articles/dns/dns-domain-delegation.md).