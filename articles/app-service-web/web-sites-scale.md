<properties
	pageTitle="Zentrales Hochskalieren einer App in Azure | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine App in Azure App Service zentral hochskalieren, um Kapazität und Features hinzuzufügen."
	services="app-service"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor="mollybos"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/05/2016"
	ms.author="cephalin"/>

# Zentrales Hochskalieren einer App in Azure #

In diesem Artikel wird die Vorgehensweise beim Skalieren Ihrer App in Azure App Service beschrieben. Es gibt zwei Workflows für die Skalierung: zentrales Hochskalieren und horizontales Hochskalieren. Dieser Artikel befasst sich mit dem Workflow zum zentralen Hochskalieren.

- [Zentrales Hochskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling) Bei dieser Methode erhalten Sie mehr CPUs, mehr Arbeitsspeicher und mehr Speicherplatz sowie zusätzliche Features wie dedizierte virtuelle Computer, benutzerdefinierte Domänen und Zertifikate, Stagingslots, automatische Skalierung und mehr. Zum zentralen Hochskalieren muss der Tarif des App Service-Plans geändert werden, zu dem die App gehört.
- [Horizontales Hochskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling) Bei dieser Methode wird die Anzahl von VM-Instanzen erhöht, die Ihre App ausführen. Die Instanzenanzahl kann je nach Tarif auf bis zu 20 erhöht werden. [App Service-Umgebungen](../app-service/app-service-app-service-environments-readme.md) im Tarif **Premium** erhöhen die Anzahl der Instanzen bei horizontalem Hochskalieren weiter auf 50. Weitere Informationen zum horizontalen Hochskalieren finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../azure-portal/insights-how-to-scale.md). Dort erfahren Sie, wie Sie die automatische Skalierung verwenden, d.h. das automatische Skalieren der Instanzenzahl basierend auf vordefinierten Regeln und Zeitplänen.

Diese Skalierungseinstellungen werden innerhalb von Sekunden angewendet und wirken sich auf alle Apps im [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) aus. Dazu müssen Sie weder Ihren Code ändern noch Ihre Anwendung erneut bereitstellen.

Informationen zur Preisgestaltung und zu den Funktionen der einzelnen App Service-Pläne finden Sie unter [App Service Preise](/pricing/details/web-sites/).

> [AZURE.NOTE] Bevor Sie einen App Service-Plan aus dem Tarif **Free** entfernen, müssen Sie zunächst die [Ausgabenlimits](/pricing/spending-limits/) für Ihr Azure-Abonnement aufheben. Informationen zum Anzeigen oder Ändern von Optionen für Ihr Microsoft Azure App Service-Abonnement finden Sie unter [Microsoft Azure-Abonnements][azuresubscriptions].

<a name="scalingsharedorbasic"></a> <a name="scalingstandard"></a>

## Zentrales Hochskalieren Ihres Tarifs

1. Öffnen Sie in Ihrem Browser das [Azure-Portal][portal].

2. Klicken Sie auf dem Blatt Ihrer App auf **Alle Einstellungen** und anschließend auf **Zentral hochskalieren**.

	![Navigieren Sie, um Ihre Azure-App zentral hochzuskalieren.][ChooseWHP]

4. Wählen Sie Ihren Tarif, und klicken Sie auf **Auswählen**.

	Auf der Registerkarte **Benachrichtigungen** wird in grüner Schrift der Text **ERFOLGREICH** angezeigt, nachdem der Vorgang abgeschlossen wurde.

<a name="ScalingSQLServer"></a>
## Skalieren zugehöriger Ressourcen
Wenn Ihre App von anderen Diensten abhängt, beispielsweise von Azure SQL-Datenbank oder Azure Storage, können Sie diese Ressourcen basierend auf Ihren Anforderungen ebenfalls zentral hochskalieren. Diese Ressourcen werden nicht mit dem App Service-Plan skaliert, sondern müssen separat skaliert werden.

1. Klicken Sie in **Grundlegende Informationen** auf den Link **Ressourcengruppe**.

	![Zentrales Hochskalieren der zugehörigen Ressourcen Ihrer Azure-App](./media/web-sites-scale/RGEssentialsLink.png)

2. Klicken Sie im Bereich **Zusammenfassung** des Blatts **Ressourcengruppe** auf eine Ressource, die Sie skalieren möchten. Der folgende Screenshot zeigt eine SQL-Datenbank-Ressource und eine Azure Storage-Ressource.

	![Navigieren zum Blatt „Ressourcengruppe“, um Ihre Azure-App zentral hochzuskalieren](./media/web-sites-scale/ResourceGroup.png)

3. Klicken Sie für eine SQL-Datenbank-Ressource auf **Einstellungen** > **Tarif**, um den Tarif zu skalieren.

	![Zentrales Hochskalieren des SQL-Datenbank-Back-Ends für Ihre Azure-App](./media/web-sites-scale/ScaleDatabase.png)

	Sie können auch die [Georeplikation](../sql-database/sql-database-geo-replication-overview.md) für Ihre SQL-Datenbankinstanz aktivieren.

    Klicken Sie für eine Azure Storage-Ressource auf **Einstellungen** > **Konfiguration**, um Ihre Speicheroptionen zentral hochzuskalieren.

    ![Zentrales Hochskalieren des Azure Storage-Kontos, das von Ihrer Azure-App verwendet wird](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>
## Informationen zu Entwicklerfunktionen
Je nach Preisstufe stehen die folgenden entwicklungsbezogenen Funktionen zur Verfügung:

### Bitness ###

- Die Preisstufen **Basic**, **Standard** und **Premium** unterstützen 64-Bit- und 32-Bit-Anwendungen.
- Die Preisstufen **Free** und **Shared** unterstützen nur 32-Bit-Anwendungen.

### Debugger-Support ###

- Debugger-Support ist für die Modi **Free**, **Shared** und**Basic** bei einer Verbindung pro App Service-Plan verfügbar.
- Debugger-Support ist für die Modi **Standard** und **Premium** bei fünf gleichzeitigen Verbindungen pro App Service-Plan verfügbar.

<a name="OtherFeatures"></a>
## Informationen zu anderen Features

- Ausführliche Informationen zu allen weiteren Funktionen in den App Service-Plänen, einschließlich Preisgestaltung und allgemein interessanten Funktionen (auch für Entwickler) finden Sie in den [App Service – Preisdetails](/pricing/details/web-sites/).

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto registrieren, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte und keine Verpflichtungen erforderlich.

<a name="Next Steps"></a>
## Nächste Schritte

- Informationen zu den ersten Schritten mit Azure finden Sie unter [Kostenlose Microsoft Azure-Testversion](/pricing/free-trial/).
- Informationen zu Preisen, Support und SLAs erhalten Sie unter den folgenden Links:

	[Preisdetails zur Datenübertragung](/pricing/details/data-transfers/)

	[Supportpläne für Microsoft Azure](/support/plans/)

	[Vereinbarungen zum Servicelevel (SLAs)](/support/legal/sla/)

	[Preisdetails für SQL-Datenbanken](/pricing/details/sql-database/)

	[Größen virtueller Computer und Clouddienste für Microsoft Azure][vmsizes]

	[App-Service-Preisdetails](/pricing/details/app-service/)

	[App-Service-Preisdetails - SSL-Verbindungen](/pricing/details/web-sites/#ssl-connections)

- Informationen zu den Best Practices für Azure App Service, einschließlich dem Aufbau einer skalierbaren, robusten Architektur, finden Sie unter [Best Practices: Azure App Service-Web-Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).

- Videos zum Skalieren von App Service-Apps finden Sie in den folgenden Ressourcen:

	- [Wann sollte man Azure-Websites skalieren? - Mit Stefan Schackow](/documentation/videos/azure-web-sites-free-vs-standard-scaling/)
	- [Automatisches Skalieren von Azure-Websites, CPU-abhängig oder geplant - Mit Stefan Schackow](/documentation/videos/auto-scaling-azure-web-sites/)
	- [Wie sollte man Azure-Websites skalieren? - Mit Stefan Schackow](/documentation/videos/how-azure-web-sites-scale/)


<!-- LINKS -->
[vmsizes]: /pricing/details/app-service/
[SQLaccountsbilling]: http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]: http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png

<!---HONumber=AcomDC_0803_2016-->