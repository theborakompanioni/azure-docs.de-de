<properties
   pageTitle="Migrieren eines vorhandenen Azure SQL Data Warehouse in Storage Premium | Microsoft Azure"
   description="Anweisungen zum Migrieren eines vorhandenen SQL Data Warehouse in Storage Premium"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="nicw;barbkess;sonyama"/>

# Details zur Migration zu Storage Premium
Für SQL Data Warehouse wurde vor Kurzem [Storage Premium eingeführt, um die Leistung besser vorhersagen zu können][]. Wir sind jetzt bereit für das Migrieren vorhandener Data Warehouses, für die derzeit Storage Standard verwendet wird, zu Storage Premium. Unten folgen weitere Details dazu, wie und wann automatische Migrationen erfolgen und wie Sie die Migration selbst durchführen, um den Zeitpunkt der Ausfallzeiten zu steuern.

Wenn Sie mehr als ein Data Warehouse verwenden, sollten Sie anhand des unten angegebenen [Zeitplans für die automatische Migration][] ermitteln, wann die einzelnen Migrationen durchgeführt werden.

## Bestimmen des Speichertyps
Wenn Sie ein Data Warehouse vor den unten angegebenen Terminen erstellt haben, verwenden Sie derzeit Storage Standard. Für jedes Data Warehouse (DW), bei dem derzeit Storage Standard verwendet wird und bei dem eine Migration ansteht, wird außerdem im [Azure-Portal][] oben im Blatt „Data Warehouse“ ein Hinweis mit folgendem Inhalt angezeigt: „*Ein bevorstehendes Upgrade auf Storage Premium ist mit Ausfallzeiten verbunden. Erfahren Sie mehr ->*“.

| **Region** | **Vor diesem Datum erstelltes DW** |
| :------------------ | :-------------------------------- |
| Australien (Osten) | Storage Premium noch nicht verfügbar |
| Australien (Südost) | Storage Premium noch nicht verfügbar |
| Brasilien Süd | Storage Premium noch nicht verfügbar |
| Kanada, Mitte | 25\. Mai 2016 |
| Kanada, Osten | 26\. Mai 2016 |
| USA (Mitte) | 26\. Mai 2016 |
| China, Osten | Storage Premium noch nicht verfügbar |
| China, Norden | Storage Premium noch nicht verfügbar |
| Ostasien | 25\. Mai 2016 |
| USA (Ost) | 26\. Mai 2016 |
| USA (Ost 2) | 27\. Mai 2016 |
| Indien, Mitte | 27\. Mai 2016 |
| Indien, Süden | 26\. Mai 2016 |
| Indien, Westen | Storage Premium noch nicht verfügbar |
| Japan Ost | Storage Premium noch nicht verfügbar |
| Japan (Westen) | Storage Premium noch nicht verfügbar |
| USA (Mitte/Norden) | Storage Premium noch nicht verfügbar |
| Nordeuropa | Storage Premium noch nicht verfügbar |
| USA (Mitte/Süden) | 27\. Mai 2016 |
| Südostasien | 24\. Mai 2016 |
| Westeuropa | 25\. Mai 2016 |
| USA (West) | 26\. Mai 2016 |

## Details zur automatischen Migration
Standardmäßig migrieren wir Ihre Datenbank zwischen 18:00 und 6:00 Uhr Ortszeit Ihrer Region, und zwar zu einem Zeitpunkt des unten angegebenen [Zeitplans für die automatische Migration][]. Während der Migration können Sie Ihr vorhandenes Data Warehouse nicht nutzen. Die Migration dauert schätzungsweise ca. eine Stunde pro TB an gespeicherten Daten pro Data Warehouse. Außerdem stellen wir sicher, dass für Sie während der Migration keine Kosten anfallen.

> [AZURE.NOTE] Während der Migration können Sie Ihr vorhandenes Data Warehouse nicht nutzen. Nachdem die Migration abgeschlossen ist, ist das Data Warehouse wieder online.

Bei den unten angegebenen Details handelt es sich um Schritte, die Microsoft für Sie ausführt, um die Migration abzuschließen. Hierfür ist kein Eingriff Ihrerseits erforderlich. Stellen Sie sich für dieses Beispiel vor, dass Ihr vorhandenes DW mit Storage Standard derzeit den Namen „MyDW“ hat.

1.	Microsoft benennt „MyDW“ in „MyDW\_DO\_NOT\_USE\_[Timestamp]“ um.
2.	Microsoft hält „MyDW\_DO\_NOT\_USE\_[Timestamp]“ an. Während dieser Zeit erstellt Microsoft eine Sicherung. Es kann zu mehreren Anhaltevorgängen und Fortsetzungen kommen, falls bei diesem Prozess Probleme auftreten.
3.	Microsoft erstellt unter Storage Premium anhand der Sicherung, die oben in Schritt 2 erstellt wurde, ein neues DW mit dem Namen „MyDW“. „MyDW“ wird erst angezeigt, nachdem der Wiederherstellungsvorgang abgeschlossen wurde.
4.	Nach Abschluss der Wiederherstellung wird „MyDW“ zurück auf die gleiche DWU-Ebene und in den angehaltenen oder aktiven Zustand wie vor der Migration versetzt.
5.	Nachdem die Migration abgeschlossen ist, löscht Microsoft das Element „MyDW\_DO\_NOT\_USE\_[Timestamp]“.
	
> [AZURE.NOTE] Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
> 
>	-  Auditing at the Database level will need to be re-enabled
>	-  Firewall rules at the **Database** level will need to be re-added.  Firewall rules at the **Server** level will not be impacted.

### Zeitplan für die automatische Migration
Die automatische Migration wird zwischen 18:00 und 6:00 Uhr (Ortszeit der Region) an einem Punkt des unten angegebenen Ausfallzeitplans durchgeführt.

| **Region** | **Geschätztes Startdatum** | **Geschätztes Enddatum** |
| :------------------ | :--------------------------- | :--------------------------- |
| Australien (Osten) | Noch nicht festgelegt | Noch nicht festgelegt |
| Australien (Südosten) | 10\. August 2016 | 24\. August 2016 |
| Brasilien Süd | 10\. August 2016 | 24\. August 2016 |
| Kanada, Mitte | 23\. Juni 2016 | 1\. Juli 2016 |
| Kanada, Osten | 23\. Juni 2016 | 1\. Juli 2016 |
| USA (Mitte) | 23\. Juni 2016 | 4\. Juli 2016 |
| China, Osten | Noch nicht festgelegt | Noch nicht festgelegt |
| China, Norden | Noch nicht festgelegt | Noch nicht festgelegt |
| Ostasien | 23\. Juni 2016 | 1\. Juli 2016 |
| USA (Ost) | 23\. Juni 2016 | 11\. Juli 2016 |
| USA (Ost 2) | 23\. Juni 2016 | 8\. Juli 2016 |
| Indien, Mitte | 23\. Juni 2016 | 1\. Juli 2016 |
| Indien, Süden | 23\. Juni 2016 | 1\. Juli 2016 |
| Indien, Westen | Noch nicht festgelegt | Noch nicht festgelegt |
| Japan Ost | 10\. August 2016 | 24\. August 2016 |
| Japan (Westen) | Noch nicht festgelegt | Noch nicht festgelegt |
| USA (Mitte/Norden) | Noch nicht festgelegt | Noch nicht festgelegt |
| Nordeuropa | 10\. August 2016 | 24\. August 2016 |
| USA Süd Mitte | 23\. Juni 2016 | 2\. Juli 2016 |
| Südostasien | 23\. Juni 2016 | 1\. Juli 2016 |
| Westeuropa | 23\. Juni 2016 | 8\. Juli 2016 |
| USA (West) | 23\. Juni 2016 | 7\. Juli 2016 |

## Selbst durchgeführte Migration zu Storage Premium
Wenn Sie den Zeitpunkt der Ausfallzeiten steuern möchten, können Sie die unten angegebenen Schritte ausführen, um ein vorhandenes Data Warehouse unter Storage Standard zu Storage Premium zu migrieren. Falls Sie sich für die selbst durchgeführte Migration entscheiden, müssen Sie diesen Vorgang vor Beginn der automatischen Migration in dieser Region durchführen. So vermeiden Sie das Risiko, dass die automatische Migration einen Konflikt verursacht (siehe [Zeitplan für die automatische Migration][]).

### Anleitung zur selbst durchgeführten Migration
Wenn Sie Ihre Ausfallzeiten selbst steuern möchten, können Sie die Migration für das Data Warehouse per Sicherung und Wiederherstellung selbst durchführen. Der Wiederherstellungsanteil der Migration dauert ca. 1 Stunde pro TB an gespeicherten Daten pro DW. Führen Sie die unten angegebenen Schritte für die [Problemumgehung in Bezug auf die Umbenennung][] aus, wenn Sie nach Abschluss der Migration den gleichen Namen beibehalten möchten.

1.	[Anhalten][]\: Halten Sie das DW an, damit eine automatische Sicherung erstellt wird.
2.	[Wiederherstellung][]\: Führen Sie eine Wiederherstellung aus der letzten Momentaufnahme durch.
3.	Löschen Sie das vorhandene DW unter Storage Standard. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses berechnet.**

> [AZURE.NOTE] Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
> 
>	-  Auditing at the Database level will need to be re-enabled
>	-  Firewall rules at the **Database** level will need to be re-added.  Firewall rules at the **Server** level will not be impacted.

#### Optional: Schritte zum Umbenennen während der Migration 
Zwei Datenbanken auf demselben logischen Server können nicht den gleichen Namen haben. SQL Data Warehouse unterstützt jetzt die Möglichkeit zum Umbenennen eines DW.

Stellen Sie sich für dieses Beispiel vor, dass Ihr vorhandenes DW mit Storage Standard derzeit den Namen „MyDW“ hat.

1.	Benennen Sie „MyDW“ um, indem Sie den ALTER DATABASE-Befehl nach einer Zeichenfolge ähnlich dieser ausführen: MyDW\_BeforeMigration. Damit werden alle vorhandenen Transaktionen gelöscht. Dieser Vorgang muss in der Masterdatenbank ausgeführt werden, um erfolgreich zu sein.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.	[Anhalten][]\: Halten Sie „MyDW\_BeforeMigration“ an, damit eine automatische Sicherung erstellt wird.
3.	[Wiederherstellen][]\: Stellen Sie aus der jüngsten Momentaufnahme eine neue Datenbank mit dem vorherigen Namen wieder her (z.B. „MyDW“).
4.	Löschen Sie „MyDW\_BeforeMigration“. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses berechnet.**

> [AZURE.NOTE] Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
> 
>	-  Auditing at the Database level will need to be re-enabled
>	-  Firewall rules at the **Database** level will need to be re-added.  Firewall rules at the **Server** level will not be impacted.

## Nächste Schritte
Wenn Probleme mit Ihrem Data Warehouse auftreten, können Sie [ein Supportticket erstellen][] und als möglichen Grund „Migration zu Storage Premium“ angeben.

<!--Image references-->

<!--Article references-->
[Zeitplan für die automatische Migration]: #automatic-migration-schedule
[Zeitplans für die automatische Migration]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[ein Supportticket erstellen]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: ./best-practices-availability-paired-regions.md
[main documentation site]: ./services/sql-data-warehouse.md
[Anhalten]: ./sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Wiederherstellen]: ./sql-data-warehouse-manage-database-restore-portal.md
[Wiederherstellung]: ./sql-data-warehouse-manage-database-restore-portal.md
[Problemumgehung in Bezug auf die Umbenennung]: #optional-rename-workaround

<!--MSDN references-->


<!--Other Web references-->
[Storage Premium eingeführt, um die Leistung besser vorhersagen zu können]: https://azure.microsoft.com/de-DE/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure-Portal]: https://portal.azure.com

<!---HONumber=AcomDC_0803_2016-->