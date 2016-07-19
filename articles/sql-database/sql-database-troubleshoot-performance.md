<properties
	pageTitle="Gewusst wie: Auswerten und Verbessern der Datenbankleistung in Azure SQL-Datenbank"
	description="Informationen zum Auswerten und Verbessern der Datenbankleistung"
	services="sql-database"
	documentationCenter=""
	authors="v-shysun"
	manager="felixwu"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/03/2016"
	ms.author="v-shysun"/>

# Gewusst wie: Auswerten und Verbessern der Datenbankleistung in Azure SQL-Datenbank
Sie können jederzeit den [Tarif](sql-database-service-tiers.md) einer Einzeldatenbank ändern oder die Anzahl der eDTUs eines Pools für elastische Datenbanken erhöhen, um die Leistung zu verbessern. Doch zuvor sollten Sie zunächst Möglichkeiten zum Verbessern und Optimieren der Abfrageleistung bestimmen. Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen einer schlechten Datenbankleistung.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Schritte zum Bewerten und Optimieren der Datenbankleistung
1.	Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Datenbanken**, wählen Sie die Datenbank aus, und suchen Sie dann mithilfe des Diagramms „Überwachung“ Ressourcen, die sich ihrem Maximum annähern. „DTU-Verbrauch“ wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten**, um den Zeitraum und die angezeigten Werte zu ändern.
2.	Verwenden Sie [Query Performance Insight](sql-database-query-performance.md) zum Auswerten der Abfragen mittels DTUs und dann den [SQL-Datenbankratgeber](sql-database-advisor.md), um Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen anzuzeigen.
3.	Mithilfe von dynamischen Verwaltungsansichten (DMVs), erweiterten Ereignissen (Xevents) und des Abfragespeichers in SSMS können Sie Leistungsparameter in Echtzeit abrufen. Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie detaillierte Tipps zur Überwachung und Optimierung.


    > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## Schritte zur Verbesserung der Datenbankleistung mit mehr Ressourcen
1.	Für Einzeldatenbanken können Sie die [Diensttarife bedarfsgesteuert ändern](sql-database-scale-up.md), um die Datenbankleistung zu steigern.
2.	Ziehen Sie bei mehreren Datenbanken [Pool für elastische Datenbanken](sql-database-elastic-pool-guidance.md) in Betracht, um Ressourcen automatisch zu skalieren.

<!---HONumber=AcomDC_0706_2016-->