---
title: Migrieren zu SQL-Datenbank per Transaktionsreplikation | Microsoft Docs
description: Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, Transaktionsreplikation
services: sql-database
documentationcenter: 
author: jognanay
manager: jhubbard
editor: 
ms.assetid: eebdd725-833d-4151-9b2b-a0303f39e30f
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 12/09/2016
ms.author: carlrab; jognanay;
translationtype: Human Translation
ms.sourcegitcommit: 8baeadbf7ef24e492c4115745c0d384e4f526188
ms.openlocfilehash: 8380925a56d39bd53fe737bed539b862cc835fad


---
# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migrieren von SQL Server-Datenbank zu Azure SQL-Datenbank per Transaktionsreplikation
> [!div class="op_single_selector"]
> * [SSMS-Migrations-Assistent](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Exportieren in eine BACPAC-Datei](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Importieren aus einer BACPAC-Datei](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie eine kompatible SQL Server-Datenbank mithilfe der Transaktionsreplikation von SQL Server mit nur minimalen Ausfallzeiten zu Azure SQL-Datenbank migrieren.

## <a name="understanding-the-transactional-replication-architecture"></a>Verstehen der Architektur der Transaktionsreplikation
Wenn Sie es sich nicht leisten können, Ihre SQL Server-Datenbank während der Migration aus der Produktion herauszunehmen, können Sie die SQL Server-Transaktionsreplikation als Migrationslösung verwenden. Um diese Lösung zu verwenden, können Sie Ihre Azure SQL-Datenbank-Instanz als Abonnenten der lokalen SQL Server-Instanz konfigurieren, die Sie migrieren möchten. Der Verteiler der lokalen Transaktionsreplikation synchronisiert die Daten aus der zu synchronisierenden lokalen Datenbank (dem Herausgeber), während weiterhin neue Transaktionen ausgeführt werden. 

Sie können die Transaktionsreplikation auch zum Migrieren einer Teilmenge Ihrer lokalen Datenbank verwenden. Die Veröffentlichung, die Sie zu Azure SQL-Datenbank replizieren, kann auf eine Teilmenge der Tabellen in der replizierten Datenbank beschränkt werden. Für jede replizierte Tabelle können Sie die Daten auf eine Teilmenge der Zeilen bzw. eine Teilmenge der Spalten beschränken.

Bei der Transaktionsreplikation werden alle Änderungen an Daten oder Schema in Ihrer Azure SQL-Datenbank angezeigt. Wenn die Synchronisierung abgeschlossen ist, und Sie zur Migration bereit sind, ändern Sie die Verbindungszeichenfolgen Ihrer Anwendungen so, dass sie auf Ihre Azure SQL-Datenbank verweisen. Sobald mit der Transaktionsreplikation alle verbleibenden Änderungen Ihrer lokalen Datenbank abgearbeitet wurden, und alle Anwendungen auf Azure DB zeigen, können Sie die Transaktionsreplikation deinstallieren. Ihre Azure SQL-Datenbank ist jetzt Ihr Produktionssystem.

 ![SeedCloudTR-Diagramm](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="how-transactional-replication-works"></a>Funktionsweise der Transaktionsreplikation

Die Transaktionsreplikation umfasst drei Hauptkomponenten: den Herausgeber, den Verteiler und den Abonnenten. Diese Komponenten führen gemeinsam die Replikation durch. Der Verteiler ist für die Steuerung der Prozesse verantwortlich, durch die Ihre Daten zwischen Servern verschoben werden. Wenn Sie die Verteilung einrichten, erstellt SQL eine Verteilungsdatenbank. Jeder Herausgeber muss an eine Verteilungsdatenbank gebunden werden. Die Verteilungsdatenbank enthält die Metadaten für jede zugehörige Veröffentlichung und Daten zum Status jeder Replikation. Bei der Transaktionsreplikation enthält sie alle Transaktionen, die auf dem Abonnenten ausgeführt werden müssen.

Der Herausgeber ist die Datenbank, aus der alle Daten für die Migration stammen. Im Herausgeber können viele Veröffentlichungen vorhanden sein. Die Veröffentlichungen enthalten Artikel, die all den Tabellen und Daten entsprechen, die repliziert werden müssen. Abhängig davon, wie Sie die Veröffentlichung und die Artikel definieren, können Sie entweder Ihre gesamte Datenbank oder einen Teil der Datenbank replizieren. 

Bei der Replikation ist der Abonnent der Server, der alle Daten und Transaktionen aus der Veröffentlichung empfängt. Jede Veröffentlichung kann über viele Replikationen verfügen.

## <a name="transactional-replication-requirements"></a>Anforderungen der Transaktionsreplikation
[Eine aktualisierte Liste von Anforderungen finden Sie unter diesem Link.](https://msdn.microsoft.com/en-US/library/mt589530.aspx)
> [!IMPORTANT]
> Verwenden Sie die neueste Version von SQL Server Management Studio, um immer den Updates von Microsoft Azure und SQL-Datenbank zu entsprechen. Ältere Versionen von SQL Server Management Studio können SQL-Datenbank nicht als Abonnenten einrichten. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 

## <a name="migration-to-sql-database-using-transaction-replication-workflow"></a>Migration zu SQL-Datenbank mithilfe des Workflows der Transaktionsreplikation

1. Einrichten der Verteilung
   -  [Verwenden von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   -  [Verwenden von Transact-SQL](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)
2. Erstellen der Veröffentlichung
   -  [Verwenden von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   -  [Verwenden von Transact-SQL](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Erstellen des Abonnements
   -  [Verwenden von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   -  [Verwenden von Transact-SQL](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

## <a name="some-tips-and-differences-for-migrating-to-sql-database"></a>Einige Tipps und Informationen zu den Unterschieden, die beim Migrieren zu SQL-Datenbank zu beachten sind

1. Verwendung eines lokalen Verteilers: 
   - Ein lokaler Verteiler beeinträchtigt die Leistung des Servers. 
   - Wenn die Leistungsbeeinträchtigung inakzeptabel ist, können Sie einen anderen Server verwenden, dies erhöht jedoch die Komplexität der Verwaltung und Administration.
2. Achten Sie bei der Auswahl eines Momentaufnahmeordners darauf, dass der Ordner ausreichend groß ist, um eine BCP-Datei von jeder zu replizierenden Tabelle darin zu speichern. 
3. Durch die Erstellung der Momentaufnahme werden die zugehörigen Tabellen gesperrt, bis der Vorgang abgeschlossen ist. Bedenken Sie dies bei der Planung Ihrer Momentaufnahme. 
4. In Azure SQL-Datenbank werden nur Pushabonnements unterstützt.
   - Sie können nur Abonnenten von der Seite Ihrer lokalen Datenbank hinzufügen.

## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [SQL Server 2016](https://www.microsoft.com/sql-server/sql-server-2016)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* Weitere Informationen zur Transaktionsreplikation finden Sie unter [Transaktionsreplikation](https://msdn.microsoft.com/library/mt589530.aspx).
* Weitere Informationen zum gesamten Migrationsprozess und zu den Optionen finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL-Datenbank in der Cloud](sql-database-cloud-migrate.md).



<!--HONumber=Dec16_HO2-->


