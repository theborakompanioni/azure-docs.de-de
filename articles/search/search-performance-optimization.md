<properties 
	pageTitle="Überlegungen zur Leistung und Optimierung von Azure Search | Microsoft Azure" 
	description="Optimieren der Leistung von Azure Search und Konfigurieren der optimalen Skalierung" 
	services="search" 
	documentationCenter="" 
	authors="LiamCavanagh" 
	manager="pablocas" 
	editor=""/>

<tags 
	ms.service="search" 
	ms.devlang="rest-api" 
	ms.workload="search" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.date="06/27/2016" 
	ms.author="liamca"/>

# Überlegungen zur Leistung und Optimierung von Azure Search

Eine hervorragende Suchfunktion ist ein Schlüssel zum Erfolg für viele mobile und Webanwendungen. Vom Immobilienmarkt über Gebrauchtwagenmärkte bis hin zu Onlinekatalogen – schnelle Suchvorgänge und relevante Ergebnisse sind sehr wichtig für die Kundenzufriedenheit. Dieses Dokument stellt bewährte Methoden vor, mit deren Hilfe Sie optimal von Azure Search profitieren können. Dies gilt insbesondere für Szenarien mit hohen Anforderungen an die Skalierbarkeit, die Unterstützung mehrerer Sprachen oder benutzerdefinierte Rangfolgen. Darüber hinaus beschreibt dieses Dokument interne Merkmale und erläutert verschiedene Vorgehensweisen, die in realen Kunden-Apps effektiv funktionieren.

## Optimierung der Leistung und Skalierbarkeit für Search-Dienste

Alle Benutzer sind an Suchmaschinen wie Bing und Google und deren hohe Leistung gewöhnt. Daher erwarten Ihre Benutzer ähnliche Leistungsmerkmale, wenn sie die Suchfunktionen in Ihrer mobilen oder Webanwendung verwenden. Bei der Optimierung der Suchleistung ist eine der besten Vorgehensweisen, sich auf die Latenz zu konzentrieren, also die Zeit, die verstreicht, bis eine Abfrage beendet wird und Ergebnisse zurückgibt. Folgende Aspekte sind bei der Optimierung der Suchlatenz wichtig:

1. Wählen Sie eine Ziellatenz aus (also die maximale Zeitdauer), die bis zum Abschluss einer typischen Suchanforderung vergehen darf.

2. Erstellen und testen Sie eine Workload für Ihren Suchdienst mit einem realistischen Dataset, um diese Latenzraten zu messen.

3. Beginnen Sie mit einer geringen Anzahl von Abfragen pro Sekunde, und erhöhen Sie die im Test ausgeführte Abfrageanzahl, bis die Abfragelatenz die definierte Ziellatenz unterschreitet. Dies ist ein wichtiger Benchmark, mit dem Sie die Skalierung planen können, wenn die Nutzungsrate Ihrer Anwendung steigt.

4. Verwenden Sie HTTP-Verbindungen nach Möglichkeit wieder. Bei Verwendung des Azure Search .NET SDK sollten Sie eine Instanz oder eine [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx)-Instanz wiederverwenden. Wenn Sie die REST-API nutzen, sollten Sie eine einzelne HttpClient-Instanz wiederverwenden.
 
Beim Erstellen dieser Testworkloads müssen Sie einige Merkmale von Azure Search berücksichtigen:

1. Es ist möglich, so viele Suchabfragen gleichzeitig zu senden, dass die im Azure Search-Dienst verfügbaren Ressource überlastet sind. In diesem Fall werden HTTP 503-Antwortcodes angezeigt. Aus diesem Grund empfiehlt es sich, mit Suchanforderungen mit unterschiedlichem Umfang zu beginnen, um die Unterschiede in den Latenzraten zu ermitteln, die sich beim Hinzufügen weiterer Suchanforderungen ergeben.

2. Das Hochladen von Inhalten in Azure Search wirkt sich auf die Leistung und Latenz des Azure Search-Diensts insgesamt aus. Wenn Sie damit rechnen, dass Daten gesendet werden, während Benutzer Suchvorgänge durchführen, müssen Sie diese Workload bei den Tests einbeziehen.

3. Nicht alle Suchabfragen werden auf der gleichen Leistungsstufe durchgeführt. Eine Dokumentsuche oder ein Suchvorschlag beispielsweise werden in der Regel schneller durchgeführt als eine Abfrage mit einer großen Anzahl von Facets und Filtern. Beim Erstellen der Tests sollten Sie daher die verschiedenen Arten von Abfragen berücksichtigen, die Sie für Ihre App erwarten.

4. Variierende Suchanforderungen sind wichtig, denn wenn Sie immer wieder die gleichen Suchanforderungen ausführen, führt die Zwischenspeicherung von Daten dazu, dass die Leistung besser aussieht als bei ungleichartigen Abfragen.

> [AZURE.NOTE] [Visual Studio Load Testing](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) eignet sich sehr gut zum Ausführen Ihrer Benchmarktests, da Sie damit HTTP-Anforderungen so ausführen können, wie es für Suchabfragen bei Azure Search notwendig ist. Außerdem lassen sich Suchanforderungen parallelisieren.

## Skalieren von Azure Search für hohe Abfrageraten und gedrosselte Anforderungen

Wenn Sie zu viele gedrosselte Anforderungen empfangen oder die Ziellatenzraten aufgrund einer erhöhten Abfragelast überschreiten, können Sie die Latenzraten auf zwei Arten senken:

1. **Anzahl der Replikate erhöhen:** Ein Replikat ist eine Kopie Ihrer Daten und ermöglicht Azure Search, die Anforderungslast auf die verschiedenen Kopien zu verteilen. Der gesamte Lastenausgleich und die Replikation der Daten auf die Kopien werden von Azure Search verwaltet. Sie können die Anzahl der Replikate, die Ihrem Dienst zugeordnet sind, jederzeit ändern. In einem Suchdienst mit Tarif „Standard“ können Sie bis zu 12 Replikate zuordnen, im Tarif „Basic“ bis zu drei. Replikate können über das [Azure-Portal](search-create-service-portal.md) oder mithilfe der [Azure Search-Management-API](search-get-started-management-api.md) angepasst werden.

2. **In höheren Search-Tarif wechseln:** Für Azure Search stehen [verschiedene Tarife](https://azure.microsoft.com/pricing/details/search/) zur Verfügung, von denen jeder eine andere Leistungsstufe bietet. Zuweilen treten so viele Abfragen auf, dass Ihr aktueller Tarif keine ausreichend niedrigen Latenzraten mehr bieten kann, selbst wenn Sie bereits die maximale Anzahl von Replikaten zugewiesen haben. In diesem Fall sollten Sie in Erwägung ziehen, in einen höheren Tarif zu wechseln, wie z.B. zu Azure Search S3, der sich sehr gut für Szenarien mit einer großen Anzahl von Dokumenten und extrem hohen Abfrageworkloads eignet.

## Skalieren von Azure Search für langsame Einzelabfragen

Ein weiterer Grund für hohe Latenzraten kann darin liegen, dass eine einzelne Abfrage zu lange dauert. In diesem Fall lässt sich die Latenzrate nicht durch Hinzufügen weiterer Replikate verbessern. Für diesen Fall sind zwei Optionen verfügbar:

1. **Anzahl der Partitionen erhöhen:** Eine Partition ist ein Mechanismus, mit dem Ihre Daten auf zusätzliche Ressourcen aufgeteilt werden. Wenn Sie eine zweite Partition hinzufügen, werden Ihre Daten und der Index in zwei Teile geteilt. Bei einer dritten Partition werden Daten und Index in drei Teile geteilt usw. Dies führt auch dazu, dass aufgrund der Parallelisierung langsame Abfragen in einigen Fällen schneller ausgeführt werden. Es gibt einige Beispiele dafür, dass diese Parallelisierung extrem gut funktioniert, etwa bei Abfragen mit geringer Selektivität. Hierbei handelt es sich um Abfragen, denen viele Dokumente entsprechen, oder um Abfragen, bei denen aufgrund von Facets Zählungen über eine große Anzahl von Dokumenten erfolgen müssen. Da für die Bewertung der Relevanz von Dokumenten oder die Ermittlung der Anzahl von Dokumenten ein hoher Berechnungsaufwand erforderlich ist, kann das Hinzufügen weiterer Partitionen für zusätzliche Berechnungsressourcen sorgen.

   In einem Suchdienst mit Tarif „Standard“ können maximal 12 Partitionen eingerichtet werden, im Tarif „Basic“ nur eine. Partitionen können über das [Azure-Portal](search-create-service-portal.md) oder mithilfe der [Azure Search-Verwaltungs-API](search-get-started-management-api.md) angepasst werden.

2. **Felder mit hoher Kardinalität begrenzen:** Ein Feld mit hoher Kardinalität ist ein Feld, in dem Facets oder Filter verwendet werden können und das eine beträchtliche Anzahl von eindeutigen Werten besitzt. Daher erfordert die Berechnung von Ergebnissen für dieses Feld viele Ressourcen. Wenn Sie z.B. ein Feld mit einer Produkt-ID oder einer Beschreibung zur Verwendung von Facets oder Filtern einrichten, ist die Kardinalität hoch, da die meisten Werte für jedes Dokument eindeutig sind. Verwenden Sie so wenig Felder mit Kardinalität wie irgend möglich.

3. **In höheren Search-Tarif wechseln:** Der Wechsel in einen anderen Azure Search-Tarif ist eine weitere Möglichkeit, die Leistung langsamer Abfragen zu verbessern. Jeder höhere Tarif bietet auch schnellere CPUs und mehr Arbeitsspeicher, was sich positiv auf die Abfrageleistung auswirken kann.

## Skalieren zur Erhöhung der Verfügbarkeit

Replikate können nicht nur zur Verringerung von Abfragelatenzen beitragen, sondern auch die Verfügbarkeit steigern. Mit einem einzigen Replikat müssen Sie mit regelmäßigen Ausfallzeiten aufgrund von Serverneustarts nach Softwareupdates oder anderen Wartungsereignissen rechnen. Daher müssen Sie überlegen, ob Ihre Anwendung eine hohe Verfügbarkeit sowohl für Suchvorgänge (Abfragen) als auch für Schreibvorgänge (Indizierungsereignisse) erfordert. Azure Search bietet SLA-Optionen in allen kostenpflichtigen Tarifen mit den folgenden Attributen:

- 2 Replikate für hohe Verfügbarkeit von schreibgeschützten Workloads (Abfragen)
- 3 oder mehr Replikate für hohe Verfügbarkeit von Lese-/ Schreibworkloads (Abfragen und Indizierung)

Weitere Informationen hierzu finden Sie unter [SLA für Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Bei Replikaten handelt es sich, wie bereits gesagt, um Kopien Ihrer Daten. Wenn Sie also mehrere Replikate einrichten, kann Azure Search Computerneustarts und Wartungsaktivitäten für ein Replikat durchführen, während gleichzeitig Abfragen weiterhin in den anderen Replikaten ausgeführt werden können. Aus diesem Grund müssen Sie auch bedenken, wie sich solche Ausfallzeiten auf Abfragen auswirken, wenn für diese eine Datenkopie weniger zur Verfügung steht.

## Skalieren von geografisch verteilten Workloads und Bereitstellen von Georedundanz

Bei geografisch verteilten Workloads werden Sie feststellen, dass bei Benutzern, deren Standort sich weit von dem Rechenzentrum entfernt befindet, an dem Ihr Azure Search-Dienst gehostet wird, höhere Latenzraten auftreten. Daher ist es wichtig, über mehrere Suchdienste in den verschiedenen Regionen zu verfügen, die sich in größerer geografischer Nähe zu diesen Benutzern befinden. Azure Search stellt derzeit keine automatisierte Methode zur Georeplikation von Azure Search-Indizes über Regionen hinweg bereit. Es gibt allerdings einige Verfahren, mit denen sich ein solcher Prozess einfach implementieren und verwalten lässt. Diese werden in den nächsten Abschnitten beschrieben.

Das Ziel bei der Einrichtung geografisch verteilter Suchdienste ist es, über mindestens zwei Indizes in mindestens zwei Regionen zu verfügen, sodass Benutzer zu dem Azure Search-Dienst weitergeleitet werden können, der die geringste Latenz bietet. Das folgende Beispiel veranschaulicht das Konzept:

   ![Tabelle der Dienste nach Region][1]

### Synchronisieren von Daten über mehrere Azure Search-Dienste hinweg

Es gibt zwei Optionen für die Synchronisierung Ihrer verteilten Suchdienste: der [Azure Search-Indexer](search-indexer-overview.md) und die Push-API (auch als [Azure Search-REST-API](https://msdn.microsoft.com/library/dn798935.aspx) bezeichnet).

### Azure Search-Indexer

Wenn Sie den Azure Search-Indexer verwenden, importieren Sie Datenänderungen bereits aus einem zentralen Datenspeicher wie z.B. Azure SQL-Datenbank oder DocumentDB. Beim Erstellen eines neuen Suchdiensts erstellen Sie einfach auch einen neuen Azure Search-Indexer für diesen Dienst, der auf den gleichen Datenspeicher zeigt. Auf diese Weise werden neue Änderungen von den verschiedenen Indexern indiziert, sobald sie sich im Datenspeicher befinden.

Diese Architektur würde in etwa wie folgt aussehen.

   ![Einzelne Datenquelle mit verteilten Indexer- und Dienstkombinationen][2]


### Push-API 
Wenn Sie die Azure Search-Push-API zum [Aktualisieren von Inhalten in Ihrem Azure Search-Index](https://msdn.microsoft.com/library/dn798930.aspx) verwenden, können Sie die verschiedenen Suchdienste synchronisieren, indem Sie bei jedem Update per Push alle Änderungen an alle Suchdienste übertragen. Hierbei müssen Sie sicherstellen, dass auch Fälle richtig verarbeitet werden, bei denen bei der Aktualisierung eines Suchdienst ein Fehler auftritt, andere Updates jedoch erfolgreich sind.

## Arbeiten mit Azure Traffic Manager

Mit [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) können Sie Anfragen an mehrere Websites an verschiedenen geografischen Standorten weiterleiten, an denen dann mehrere Azure Search-Dienste zum Einsatz kommen. Traffic Manager kann testen, ob Azure Search verfügbar ist, und Benutzer während eines Ausfalls an alternative Suchdienste weiterleiten – dies ist ein großer Vorteil. Wenn Sie Suchanfragen über Azure-Websites weiterleiten, ermöglicht Azure Traffic Manager zudem den Lastenausgleich in Fällen, in denen die Website erreichbar ist, aber nicht Azure Search. Hier finden Sie ein Beispiel für eine Architektur mit Traffic Manager.

   ![Tabelle der Dienste nach Region mit Traffic Manager im Zentrum][3]

## Leistungsüberwachung

Azure Search bietet die Möglichkeit, die Leistung Ihres Dienst über [Search Traffic Analytics (STA)](search-traffic-analytics.md) zu analysieren und zu überwachen. Mit STA können Sie optional die einzelnen Suchvorgänge sowie aggregierte Metriken in einem Azure Storage-Konto protokollieren, das dann zur Analyse verarbeitet oder in Power BI visualisiert werden kann. Mithilfe von STA-Metriken können Sie Leistungsstatistiken wie beispielsweise die durchschnittliche Anzahl von Abfragen oder die Antwortzeiten für Abfragen überprüfen. Darüber hinaus können Sie mit der Vorgangsprotokollierung Details bestimmter Suchoperationen anzeigen.

STA ist ein wertvolles Tool, um die Latenzraten aus der Perspektive von Azure Search zu verstehen. Da die protokollierten Leistungsmetriken für Abfragen auf dem Zeitraum basieren, der für die vollständige Verarbeitung einer Abfrage in Azure Search erforderlich ist (von dem Zeitpunkt, zu dem eine Antwort angefordert wurde, bis zu dem Zeitpunkt, zu dem sie gesendet wird), können Sie anhand dieser Metriken ermitteln, ob Latenzprobleme seitens des Azure Search-Diensts oder außerhalb des Diensts auftreten (z.B. aufgrund einer Netzwerklatenz).

## Nächste Schritte

Weitere Informationen zu den Tarifen und den Grenzwerte für jeden Tarif finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md).

Weitere Informationen zu Partitions- und Replikatkombinationen finden Sie unter [Kapazitätsplanung](search-capacity-planning.md).

Das folgende Video zeigt weitere Details zur Leistung und veranschaulicht, wie die in diesem Artikel beschriebenen Optimierungen implementiert werden können:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png

<!---HONumber=AcomDC_0629_2016-->