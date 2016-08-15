<properties
   pageTitle="Globale Verteilung von Daten mit DocumentDB | Microsoft Azure"
   description="Erfahren Sie mehr über weltweite Georeplikation, Failover und Datenwiederherstellungen mithilfe von globalen Datenbanken mit Azure DocumentDB, einem vollständig verwalteten NoSQL-Datenbankdienst."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="kipandya"/>
   
   
# Globale Verteilung von Daten mit DocumentDB

> [AZURE.NOTE] Die globale Verteilung von DocumentDB-Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, die globale Verteilung für alle vorhandenen Konten zu aktivieren. Wenn Sie jedoch in der Zwischenzeit die globale Verteilung für Ihr Konto aktivieren möchten, [kontaktieren Sie den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), der diesen Schritt für Sie ausführt.

Azure DocumentDB wurde für die Anforderungen von IoT-Anwendungen konzipiert, die aus Millionen von global verteilten Geräten und internetbasierten Anwendungen bestehen, die Benutzern auf der ganzen Welt ein reaktionsschnelles Benutzererlebnis bieten. Mit diesen Datenbanksystemen lässt sich die Herausforderung bewältigen, beim Zugriff auf Anwendungsdaten aus mehreren geografischen Regionen eine sehr niedrige Latenz zu erzielen und gleichzeitig für klar definierte Datenkonsistenz und garantierte Verfügbarkeit zu sorgen. Als global verteiltes Datenbanksystem vereinfacht DocumentDB die globale Verteilung von Daten, indem es vollständig verwaltete Datenbankkonten in mehreren Regionen bietet, die für eine sorgfältig austarierte Balance zwischen Konsistenz, Verfügbarkeit und Leistung mit den entsprechenden Garantien sorgen. DocumentDB-Datenbankkonten bieten hohe Verfügbarkeit, Latenzen im einstelligen Millisekundenbereich, mehrere [klar abgegrenzte Konsistenzebenen][consistency], transparentes regionales Failover mit Multihosting-APIs sowie die Fähigkeit, Durchsatz und Speicher für alle Konten weltweit elastisch zu skalieren.

  
## Konfigurieren von Konten in mehreren Regionen

Sie können Ihr DocumentDB-Konto im Azure-Portal in weniger als einer Minute für eine weltweite Skalierung konfigurieren. Sie müssen nur aus den unterstützten, klar abgegrenzten Konsistenzebenen die richtige Ebene auswählen und Ihrem Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. DocumentDB-Konsistenzebenen bieten eine sorgfältig austarierte Balance zwischen Konsistenzgarantie und Leistung.

![DocumentDB bietet mehrere klar definierte (gelockerte) Konsistenzmodelle.][1]

DocumentDB bietet mehrere klar definierte (gelockerte) Konsistenzmodelle zur Auswahl.

Die Auswahl der richtigen Konsistenzebene richtet sich nach der Datenkonsistenzgarantie, die für Ihre Anwendung erforderlich ist. DocumentDB repliziert Ihre Daten automatisch auf alle angegebenen Regionen und garantiert die Konsistenz, die Sie für Ihr Datenbankkonto ausgewählt haben.


## Verwenden eines Failovers über mehrere Regionen hinweg 

Azure DocumentDB kann ein transparentes Failover für Datenbankkonten über mehrere Azure-Regionen hinweg durchführen. Die neuen [Multihosting-APIs][developingwithmultipleregions] garantieren, dass Ihre App weiterhin einen logischen Endpunkt verwenden kann und durch das Failover nicht unterbrochen wird. Sie steuern das Failover selbst, sodass Sie Ihr Datenbankkonto verlagern können, falls potenzielle Fehlerbedingungen auftreten, einschließlich (echten oder simulierten) Fehlern in Anwendungen, Infrastrukturen, Diensten oder Regionen. Sollte ein regionaler DocumentDB-Fehler auftreten, führt der Dienst ein transparentes Failover Ihres Datenbankkontos durch, und Ihre Anwendung kann weiterhin ohne Verfügbarkeitsverlust auf die Daten zugreifen. DocumentDB bietet [SLAs mit einer Verfügbarkeit von 99,99%][sla]. Sie können die End-to-End-Verfügbarkeitseigenschaften Ihrer Anwendung jedoch testen, indem Sie [programmgesteuert][arm] oder über das Azure-Portal einen regionalen Ausfall simulieren.


## Weltweite Skalierung
Mit DocumentDB können Sie für alle DocumentDB-Sammlungen jeder Größenordnung unabhängig voneinander Durchsatz bereitstellen und Speicherplatz nutzen, und zwar global über alle Regionen hinweg, die Ihrem Datenbankkonto zugeordnet sind. Eine DocumentDB-Sammlung wird automatisch global verteilt und über alle Regionen hinweg verwaltet, die Ihrem Datenbankkonto zugeordnet sind. Sammlungen in Ihrem Datenbankkonto können auf alle Azure-Regionen verteilt werden, in denen der [DocumentDB-Dienst verfügbar ist][serviceregions].

Der erworbene Durchsatz und der genutzte Speicher für jede DocumentDB-Sammlung werden automatisch gleichmäßig über alle Regionen hinweg bereitgestellt. So kann Ihre Anwendung nahtlos auf der ganzen Welt skaliert werden, und [Sie zahlen nur für den pro Stunde genutzten Durchsatz und Speicher][pricing]. Wenn Sie z.B. 2 Millionen Anforderungseinheiten (Request Units, RUs) für eine DocumentDB-Sammlung bereitgestellt haben, erhält jede Region, die Ihrem Datenbankkonto zugeordnet ist, 2 Millionen Anforderungseinheiten für diese Sammlung. Dies ist im Folgenden dargestellt:

![Skalieren des Durchsatzes für eine DocumentDB-Sammlung über vier Regionen hinweg][2]

DocumentDB garantiert bei P99 Latenzen von < 10 ms für Lesevorgänge und < 15 ms für Schreibvorgänge. Die Leseanforderungen erstrecken sich nie über Rechenzentrumsgrenzen hinweg, um die [von Ihnen ausgewählten Konsistenzanforderungen][consistency] zu garantieren. Schreibvorgänge werden immer lokal im Quorum zugesichert, bevor sie für die Clients bestätigt werden. Jedes Datenbankkonto wird mit einer Priorität für die Schreibregion konfiguriert. Die Region mit der höchsten Priorität fungiert als aktuelle Schreibregion für das Konto. Alle SDKs leiten Schreibvorgänge des Datenbankkontos transparent an die aktuelle Schreibregion weiter.

Da DocumentDB ganz [ohne Schemas arbeitet][vldb], müssen Sie sich nicht um die Verwaltung/Aktualisierung von Schemas oder sekundären Indizes über mehrere Rechenzentren hinweg kümmern. Ihre [SQL-Abfragen][sqlqueries] funktionieren weiterhin, während Ihre Anwendungs- und Datenmodelle sich weiterentwickeln.


## Ermöglichen der globalen Verteilung 

Sie können Ihre Daten lokal bereitstellen oder global verteilen, indem Sie einem DocumentDB-Datenbankkonto eine oder mehrere Azure-Regionen zuordnen. Sie können Ihrem Datenbankkonto jederzeit Regionen hinzufügen oder Regionen daraus entfernen.

## Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung von Daten mit DocumentDB:

* [Bereitstellung von Durchsatz und Speicher für eine Sammlung][throughputandstorage]
* [Multihosting-APIs über REST- .NET-, Java-, Python- und Node-SDKs][developingwithmultipleregions]
* [Konsistenzebenen in DocumentDB][consistency]
* [Verfügbarkeits-SLAs][sla]
* [Verwalten eines Datenbankkontos][manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: https://azure.microsoft.com/documentation/articles/documentdb-partition-data/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: https://azure.microsoft.com/documentation/articles/documentdb-developing-with-multiple-regions/
[createaccount]: https://azure.microsoft.com/documentation/articles/documentdb-create-account/
[manageaccount]: https://azure.microsoft.com/documentation/articles/documentdb-manage-account/
[manageaccount-consistency]: https://azure.microsoft.com/documentation/articles/documentdb-manage-account/#consistency
[manageaccount-addregion]: https://azure.microsoft.com/documentation/articles/documentdb-manage-account/#addregion
[throughputandstorage]: https://azure.microsoft.com/documentation/articles/documentdb-manage/
[arm]: https://azure.microsoft.com/de-DE/documentation/articles/documentdb-automation-resource-manager-cli/
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/de-DE/regions/#services
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: https://azure.microsoft.com/documentation/articles/documentdb-sql-query/

<!---HONumber=AcomDC_0803_2016-->