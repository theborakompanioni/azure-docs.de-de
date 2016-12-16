---
title: Globale Verteilung von Daten mit DocumentDB | Microsoft Docs
description: "Erfahren Sie mehr über weltweite Georeplikation, Failover und Datenwiederherstellungen mithilfe von globalen Datenbanken mit Azure DocumentDB, einem vollständig verwalteten NoSQL-Datenbankdienst."
services: documentdb
documentationcenter: 
author: kiratp
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: kipandya
translationtype: Human Translation
ms.sourcegitcommit: 2d833a559b72569983340972ba3b905b9e42e61d
ms.openlocfilehash: 4147bf0a5f4a3c41de53ef5b3e05d8409f9a8c87


---
# <a name="distribute-data-globally-with-documentdb"></a>Globale Verteilung von Daten mit DocumentDB
> [!NOTE]
> Die globale Verteilung von DocumentDB-Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, die globale Verteilung für alle vorhandenen Konten zu aktivieren. Wenn Sie jedoch in der Zwischenzeit die globale Verteilung für Ihr Konto aktivieren möchten, [kontaktieren Sie den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), der diesen Schritt für Sie ausführt.
> 
> 

Azure DocumentDB wurde für die Anforderungen von IoT-Anwendungen konzipiert, die aus Millionen von global verteilten Geräten und internetbasierten Anwendungen bestehen, die Benutzern auf der ganzen Welt ein reaktionsschnelles Benutzererlebnis bieten. Mit diesen Datenbanksystemen lässt sich die Herausforderung bewältigen, beim Zugriff auf Anwendungsdaten aus mehreren geografischen Regionen eine sehr niedrige Latenz zu erzielen und gleichzeitig für klar definierte Datenkonsistenz und garantierte Verfügbarkeit zu sorgen. Als global verteiltes Datenbanksystem vereinfacht DocumentDB die globale Verteilung von Daten, indem es vollständig verwaltete Datenbankkonten in mehreren Regionen bietet, die für eine sorgfältig austarierte Balance zwischen Konsistenz, Verfügbarkeit und Leistung mit den entsprechenden Garantien sorgen. DocumentDB-Datenbankkonten bieten hohe Verfügbarkeit, Latenzen im einstelligen Millisekundenbereich, mehrere [klar abgegrenzte Konsistenzebenen][consistency], transparentes regionales Failover mit Multihosting-APIs sowie die Fähigkeit, Durchsatz und Speicher für alle Konten weltweit elastisch zu skalieren. 

Für die ersten Schritte wird empfohlen, das folgende Video anzusehen, in dem Karthik Raman die geografische Verteilung mit Azure DocumentDB erläutert.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]
> 
> 

## <a name="configuring-multi-region-accounts"></a>Konfigurieren von Konten in mehreren Regionen
Sie können Ihr DocumentDB-Konto im [Azure-Portal](documentdb-portal-global-replication.md)in weniger als einer Minute für eine weltweite Skalierung konfigurieren. Sie müssen nur aus den unterstützten, klar abgegrenzten Konsistenzebenen die richtige Ebene auswählen und Ihrem Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. DocumentDB-Konsistenzebenen bieten eine sorgfältig austarierte Balance zwischen Konsistenzgarantie und Leistung. 

![DocumentDB bietet mehrere klar definierte (gelockerte) Konsistenzmodelle.][1]

DocumentDB bietet mehrere klar definierte (gelockerte) Konsistenzmodelle zur Auswahl.

Die Auswahl der richtigen Konsistenzebene richtet sich nach der Datenkonsistenzgarantie, die für Ihre Anwendung erforderlich ist. DocumentDB repliziert Ihre Daten automatisch auf alle angegebenen Regionen und garantiert die Konsistenz, die Sie für Ihr Datenbankkonto ausgewählt haben. 

## <a name="using-multi-region-failover"></a>Verwenden eines Failovers über mehrere Regionen hinweg
Azure DocumentDB kann ein transparentes Failover für Datenbankkonten über mehrere Azure-Regionen hinweg durchführen. Die neuen [Multihosting-APIs][developingwithmultipleregions] garantieren, dass Ihre App weiterhin einen logischen Endpunkt verwenden kann und durch das Failover nicht unterbrochen wird. Sie steuern das Failover selbst, sodass Sie Ihr Datenbankkonto verlagern können, falls potenzielle Fehlerbedingungen auftreten, einschließlich (echten oder simulierten) Fehlern in Anwendungen, Infrastrukturen, Diensten oder Regionen. Sollte ein regionaler DocumentDB-Fehler auftreten, führt der Dienst ein transparentes Failover Ihres Datenbankkontos durch, und Ihre Anwendung kann weiterhin ohne Verfügbarkeitsverlust auf die Daten zugreifen. DocumentDB bietet [SLAs mit einer Verfügbarkeit von 99,99 %][sla]. Sie können die End-to-End-Verfügbarkeitseigenschaften Ihrer Anwendung jedoch testen, indem Sie [programmgesteuert][arm] oder über das Azure-Portal einen regionalen Ausfall simulieren.

## <a name="scaling-across-the-planet"></a>Weltweite Skalierung
Mit DocumentDB können Sie für alle DocumentDB-Sammlungen jeder Größenordnung unabhängig voneinander Durchsatz bereitstellen und Speicherplatz nutzen, und zwar global über alle Regionen hinweg, die Ihrem Datenbankkonto zugeordnet sind. Eine DocumentDB-Sammlung wird automatisch global verteilt und über alle Regionen hinweg verwaltet, die Ihrem Datenbankkonto zugeordnet sind. Sammlungen in Ihrem Datenbankkonto können auf alle Azure-Regionen verteilt werden, in denen der [DocumentDB-Dienst][serviceregions] verfügbar ist. 

Der erworbene Durchsatz und der genutzte Speicher für jede DocumentDB-Sammlung werden automatisch gleichmäßig über alle Regionen hinweg bereitgestellt. So kann Ihre Anwendung nahtlos auf der ganzen Welt skaliert werden, und [Sie zahlen nur für den pro Stunde genutzten Durchsatz und Speicher][pricing]. Wenn Sie z.B. 2 Millionen Anforderungseinheiten (Request Units, RUs) für eine DocumentDB-Sammlung bereitgestellt haben, erhält jede Region, die Ihrem Datenbankkonto zugeordnet ist, 2 Millionen Anforderungseinheiten für diese Sammlung. Dies ist im Folgenden dargestellt:

![Skalieren des Durchsatzes für eine DocumentDB-Sammlung über vier Regionen hinweg][2]

DocumentDB garantiert bei P99 Latenzen von < 10 ms für Lesevorgänge und < 15 ms für Schreibvorgänge. Die Leseanforderungen erstrecken sich nie über Rechenzentrumsgrenzen hinweg, um die [von Ihnen ausgewählten Konsistenzanforderungen][consistency] zu garantieren. Schreibvorgänge werden immer lokal im Quorum zugesichert, bevor sie für die Clients bestätigt werden. Jedes Datenbankkonto wird mit einer Priorität für die Schreibregion konfiguriert. Die Region mit der höchsten Priorität fungiert als aktuelle Schreibregion für das Konto. Alle SDKs leiten Schreibvorgänge des Datenbankkontos transparent an die aktuelle Schreibregion weiter. 

Da DocumentDB ganz [ohne Schemas][vldb] arbeitet, müssen Sie sich nicht um die Verwaltung/Aktualisierung von Schemas oder sekundären Indizes über mehrere Rechenzentren hinweg kümmern. Ihre [SQL-Abfragen][sqlqueries] funktionieren weiterhin, während Ihre Anwendungs- und Datenmodelle sich weiterentwickeln. 

## <a name="enabling-global-distribution"></a>Aktivieren der globalen Verteilung
Sie können Ihre Daten lokal bereitstellen oder global verteilen, indem Sie einem DocumentDB-Datenbankkonto eine oder mehrere Azure-Regionen zuordnen. Sie können Ihrem Datenbankkonto jederzeit Regionen hinzufügen oder Regionen daraus entfernen. Informationen zum Aktivieren der globalen Verteilung über das Portal finden Sie unter [Ausführen einer globalen DocumentDB-Datenbankreplikation mit dem Azure-Portal](documentdb-portal-global-replication.md). Informationen zur programmgesteuerten Aktivierung der globalen Verteilung finden Sie unter [Entwickeln mit regionsübergreifenden DocumentDB-Konten](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung von Daten mit DocumentDB:

* [Bereitstellung von Durchsatz und Speicher für eine Sammlung][throughputandstorage]
* [Multihosting-APIs über REST- .NET-, Java-, Python- und Node-SDKs][developingwithmultipleregions]
* [Konsistenzebenen in DocumentDB][consistency]
* [Verfügbarkeits-SLAs][sla]
* [Verwalten eines Datenbankkontos][manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md




<!--HONumber=Nov16_HO3-->


