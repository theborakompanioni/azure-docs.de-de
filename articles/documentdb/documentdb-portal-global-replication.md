---
title: Globale DocumentDB-Datenbankreplikation | Microsoft Docs
description: "Erfahren Sie, wie Sie die globale Replikation Ihres DocumentDB-Kontos über das Azure-Portal verwalten."
services: documentdb
keywords: Globale Datenbank, Replikation
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 7c28fda22a08ea40b15cf69351e1b0aff6bd0a95
ms.openlocfilehash: 41b75cf3059cd3758f1bce149ad8fcad4e5644ac
ms.lasthandoff: 03/07/2017


---
# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Ausführen einer globalen DocumentDB-Datenbankreplikation mit dem Azure-Portal

Erfahren Sie, wie Sie mit dem Azure-Portal Daten in mehreren Regionen replizieren, um globale Verfügbarkeit von Daten in Azure DocumentDB sicherzustellen.

Informationen zur Funktionsweise der globalen Datenreplikation in DocumentDB finden Sie unter [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md). Informationen zur programmgesteuerten Ausführung einer Datenbankreplikation finden Sie unter [Entwickeln mit regionsübergreifenden DocumentDB-Konten](documentdb-developing-with-multiple-regions.md).

## <a id="addregion"></a>Hinzufügen von globalen Datenbankregionen
DocumentDB ist in den meisten [Azure-Regionen][azureregions] verfügbar. Nachdem Sie die Standardkonsistenzebene für Ihr Datenbankkonto ausgewählt haben, können Sie dem Konto eine oder mehrere Regionen zuordnen (je nachdem, welche Konsistenzebene Sie ausgewählt haben und welche Anforderungen an eine globale Verteilung bestehen).

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der linken Leiste auf **NoSQL (DocumentDB)**.
2. Wählen Sie auf dem Blatt **NoSQL (DocumentDB)** das zu ändernde Datenbankkonto aus.
3. Klicken Sie auf dem Blatt „Konto“ im Menü auf **Daten global replizieren**.
4. Wählen Sie auf dem Blatt **Daten global replizieren** die Regionen aus, die Sie hinzufügen oder entfernen möchten, und klicken Sie dann auf **Speichern**. Für das Hinzufügen von Regionen entstehen Kosten. Weitere Informationen hierzu finden Sie auf der Seite mit [Preisinformationen](https://azure.microsoft.com/pricing/details/documentdb/) sowie im Artikel [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md).
   
    ![Hinzufügen oder Entfernen von Regionen per Klick auf die Regionen auf der Karte][1]
    
Nachdem Sie eine zweite Region hinzugefügt haben, wird auf dem Blatt **Daten lokal replizieren** im Portal die Option **Manuelles Failover** aktiviert. Sie können diese Option verwenden, um den Failovervorgang zu testen. Nachdem Sie eine dritte Region hinzugefügt haben, wird auf dem gleichen Blatt die Option **Failoverprioritäten** aktiviert, sodass Sie die Failoverreihenfolge für Lesevorgänge ändern können.  

### <a name="selecting-global-database-regions"></a>Auswählen von globalen Datenbankregionen
Wenn Sie zwei oder mehr Regionen konfigurieren, empfiehlt es sich, die Regionen basierend auf den Regionspaaren auszuwählen, die im Artikel [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare][bcdr] beschrieben werden.

Stellen Sie beim Konfigurieren von mehreren Regionen insbesondere sicher, dass Sie für jede Spalte mit einem Regionspaar die gleiche Anzahl von Regionen auswählen (+/-1 für eine ungerade/gerade Anzahl). Wenn Sie also z.B. Daten in vier Regionen in den USA bereitstellen möchten, wählen Sie zwei USA-Regionen aus der linken Spalte und zwei USA-Regionen aus der rechten Spalte. Folgende Auswahl wäre eine geeignete Kombination: „USA, Westen“, „USA, Osten“, „USA, Norden-Mitte“, „USA, Süden-Mitte“.

Diese Anweisungen müssen unbedingt befolgt werden, wenn für Szenarien der Notfallwiederherstellung nur zwei Regionen konfiguriert werden. Wenn Sie mehr als zwei Regionen konfigurieren, empfiehlt es sich, diese Anweisungen zu befolgen, dies ist jedoch nicht kritisch, sofern einige der ausgewählten Regionen zum gleichen Regionspaar gehören.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **NoSQL (DocumentDB)** blade, select the database account to modify.
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
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/

