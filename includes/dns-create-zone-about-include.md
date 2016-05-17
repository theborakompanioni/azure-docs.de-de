Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Um mit dem Hosten der Domäne zu beginnen, müssen Sie eine DNS-Zone erstellen. Alle DNS-Einträge, die für eine bestimmte Domäne erstellt wurden, befinden sich in einer DNS-Zone für die Domäne.

Beispiel: Die Domäne „contoso.com“ kann eine Reihe von DNS-Einträgen wie „mail.contoso.com“ (für einen E-Mail-Server) und „www.contoso.com“ (für eine Website) enthalten.


## <a name="names"></a>Informationen zu DNS-Zonennamen
 
- Der Name der Zone muss innerhalb der Ressourcengruppe eindeutig sein. Außerdem darf die Zone noch nicht vorhanden sein. Andernfalls schlägt der Vorgang fehl.

- Der gleiche Zonennamen kann in einer anderen Ressourcengruppe oder einem anderen Azure-Abonnement erneut verwendet werden.

- Wenn mehrere Zonen denselben Namen haben, werden jeder Instanz verschiedene Namensserveradressen zugewiesen, und nur eine Instanz kann von der übergeordneten Domäne delegiert werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](../articles/dns/dns-domain-delegation.md).