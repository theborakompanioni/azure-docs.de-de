Möglicherweise verwenden Sie ein Azure-Abonnement, für das Sie nicht als Administrator oder Besitzer fungieren (beispielsweise ein unternehmenseigenes Abonnement). Vergewissern Sie sich in diesem Fall mithilfe der Schritte in diesem Artikel, dass Sie über Folgendes verfügen:

* Zugriff für Mitwirkende: Für die Anmeldung bei Azure benötigen Sie mindestens das Zugriffsrecht „Mitwirkender“ für die Azure-Ressourcengruppe. Diese Ressourcengruppe dient zum Erstellen eines HDInsight-Clusters und anderer Azure-Ressourcen.
* Anbieterregistrierung: Ein Benutzer, der für das Azure-Abonnement mindestens über Zugriff vom Typ „Mitwirkender“ verfügt, muss den Anbieter für die von Ihnen verwendete Ressource bereits registriert haben. Die Anbieterregistrierung findet statt, wenn ein Benutzer, der über Abonnementzugriff vom Typ „Mitwirkender“ verfügt, erstmals eine Ressource für das Abonnement erstellt. Das kann auch ohne Erstellung einer Ressource erreicht werden. Führen Sie dazu wie [hier](https://msdn.microsoft.com/library/azure/dn790548.aspx) beschrieben eine REST-basierte Anbieterregistrierung aus.

Weitere Informationen zur Verwendung der Zugriffsverwaltung finden Sie in den folgenden Artikeln:

* [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](../articles/active-directory/role-based-access-control-what-is.md)
* [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../articles/active-directory/role-based-access-control-configure.md)
