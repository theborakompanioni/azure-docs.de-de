<properties
	pageTitle="Globale DocumentDB-Datenbankreplikation | Microsoft Azure"
	description="Erfahren Sie, wie Sie die globale Replikation Ihres DocumentDB-Kontos über das Azure-Portal verwalten."
	services="documentdb"
    keywords="Globale Datenbank, Replikation"
	documentationCenter=""
	authors="mimig1"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/26/2016"
	ms.author="mimig"/>

# Ausführen einer globalen DocumentDB-Datenbankreplikation mit dem Azure-Portal

Erfahren Sie, wie Sie mit dem Azure-Portal Daten in mehreren Regionen replizieren, um globale Verfügbarkeit von Daten in Azure DocumentDB sicherzustellen.

Informationen zur Funktionsweise der globalen Datenreplikation in DocumentDB finden Sie unter [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md). Informationen zur programmgesteuerten Ausführung einer Datenbankreplikation finden Sie unter [Entwickeln mit regionsübergreifenden DocumentDB-Konten](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Die globale Verteilung von DocumentDB-Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, die globale Verteilung für alle vorhandenen Konten zu aktivieren. Wenn Sie jedoch in der Zwischenzeit die globale Verteilung für Ihr Konto aktivieren möchten, [kontaktieren Sie den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), der diesen Schritt für Sie ausführt.

## <a id="addregion"></a>Hinzufügen von globalen Datenbankregionen

DocumentDB ist in den meisten [Azure-Regionen][azureregions] verfügbar. Nachdem Sie die Standardkonsistenzebene für Ihr Datenbankkonto ausgewählt haben, können Sie dem Konto eine oder mehrere Regionen zuordnen (je nachdem, welche Konsistenzebene Sie ausgewählt haben und welche Anforderungen an eine globale Verteilung bestehen).

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) in der Navigationsleiste auf **DocumentDB-Konten**.
2. Wählen Sie auf dem Blatt **DocumentDB-Konto** das zu ändernde Datenbankkonto aus.
3. Klicken Sie auf dem Kontoblatt im Menü auf **Regionen hinzufügen/entfernen**.
4. Wählen Sie auf dem Blatt **Regionen hinzufügen/entfernen** die Regionen aus, die Sie hinzufügen oder entfernen möchten, und klicken Sie dann auf **OK**. Für das Hinzufügen von Regionen entstehen Kosten. Weitere Informationen hierzu finden Sie auf der Seite mit [Preisinformationen](https://azure.microsoft.com/pricing/details/documentdb/) sowie im Artikel [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md).

    ![Hinzufügen oder Entfernen von Regionen per Klick auf die Regionen auf der Karte][1]

### Auswählen von globalen Datenbankregionen

Wenn Sie zwei oder mehr Regionen konfigurieren, empfiehlt es sich, die Regionen basierend auf den Regionspaaren auszuwählen, die im Artikel [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare][bcdr] beschrieben werden.

Stellen Sie beim Konfigurieren von mehreren Regionen insbesondere sicher, dass Sie für jede Spalte mit einem Regionspaar die gleiche Anzahl von Regionen auswählen (+/-1 für eine ungerade/gerade Anzahl). Wenn Sie also z.B. Daten in vier Regionen in den USA bereitstellen möchten, wählen Sie zwei USA-Regionen aus der linken Spalte und zwei USA-Regionen aus der rechten Spalte. Folgende Auswahl wäre eine geeignete Kombination: „USA, Westen“, „USA, Osten“, „USA, Norden-Mitte“, „USA, Süden-Mitte“.

Diese Anweisungen müssen unbedingt befolgt werden, wenn für Szenarien der Notfallwiederherstellung nur zwei Regionen konfiguriert werden. Wenn Sie mehr als zwei Regionen konfigurieren, empfiehlt es sich, diese Anweisungen zu befolgen, dies ist jedoch nicht kritisch, sofern einige der ausgewählten Regionen zum gleichen Regionspaar gehören.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Nächste Schritte

Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in DocumentDB](documentdb-consistency-levels.md).

Informationen zur Funktionsweise der globalen Datenreplikation in DocumentDB finden Sie unter [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md). Informationen zur programmgesteuerten Datenbankreplikation in mehreren Regionen finden Sie unter [Entwickeln mit regionsübergreifenden DocumentDB-Konten](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/documentdb/

<!---HONumber=AcomDC_0831_2016-->