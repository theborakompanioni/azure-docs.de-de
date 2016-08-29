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
   ms.date="08/11/2016"
   ms.author="nicw;barbkess;sonyama"/>

# Details zur Migration zu Storage Premium
Für SQL Data Warehouse wurde vor Kurzem [Storage Premium eingeführt, um die Leistung besser vorhersagen zu können][]. Wir sind jetzt bereit für das Migrieren vorhandener Data Warehouses, für die derzeit Storage Standard verwendet wird, zu Storage Premium. Im Folgenden erhalten Sie weitere Details dazu, wie und wann automatische Migrationen erfolgen und wie Sie die Migration selbst durchführen, um den Zeitpunkt der Ausfallzeiten zu steuern.

Wenn Sie mehr als ein Data Warehouse verwenden, ermitteln Sie anhand des unten angegebenen [Zeitplans für die automatische Migration][], wann die einzelnen Migrationen durchgeführt werden.

## Bestimmen des Speichertyps
Wenn Sie ein Data Warehouse vor den unten angegebenen Terminen erstellt haben, verwenden Sie derzeit Storage Standard. Für jedes Data Warehouse in Storage Standard, das der automatischen Migration unterliegt, wird außerdem im [Azure-Portal][] oben im Blatt „Data Warehouse“ ein Hinweis mit folgendem Inhalt angezeigt: „*Ein bevorstehendes Upgrade auf Storage Premium ist mit Ausfallzeiten verbunden. Erfahren Sie mehr ->*.“

| **Region** | **Vor diesem Datum erstelltes DW** |
| :------------------ | :-------------------------------- |
| Australien (Osten) | Storage Premium noch nicht verfügbar |
| Australien (Südosten) | 5\. August 2016 |
| Brasilien Süd | 5\. August 2016 |
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
| Japan Ost | 5\. August 2016 |
| Japan (Westen) | Storage Premium noch nicht verfügbar |
| USA (Mitte/Norden) | Storage Premium noch nicht verfügbar |
| Nordeuropa | 5\. August 2016 |
| USA (Mitte/Süden) | 27\. Mai 2016 |
| Südostasien | 24\. Mai 2016 |
| Westeuropa | 25\. Mai 2016 |
| USA, Westen-Mitte | Storage Premium noch nicht verfügbar |
| USA (West) | 26\. Mai 2016 |
| USA, Westen 2 | Storage Premium noch nicht verfügbar |

## Details zur automatischen Migration
Standardmäßig migrieren wir Ihre Datenbank zwischen 18:00 und 06:00 Uhr Ortszeit Ihrer Region, und zwar während des unten angegebenen [Zeitplans für die automatische Migration][]. Während der Migration kann Ihr vorhandenes Data Warehouse nicht genutzt werden. Die Migration dauert ca. eine Stunde pro TB an gespeicherten Daten pro Data Warehouse. Außerdem stellen wir sicher, dass während der automatischen Migration keine Kosten für Sie anfallen.

> [AZURE.NOTE] Während der Migration können Sie Ihr vorhandenes Data Warehouse nicht nutzen. Nachdem die Migration abgeschlossen ist, ist das Data Warehouse wieder online.

Bei den unten angegebenen Details handelt es sich um Schritte, die Microsoft für Sie ausführt, um die Migration abzuschließen. Hierfür ist kein Eingriff Ihrerseits erforderlich. Beispiel: Ihr vorhandenes Data Warehouse mit Storage Standard trägt derzeit den Namen „MyDW“.

1.	Microsoft benennt „MyDW“ in „MyDW\_DO\_NOT\_USE\_[Zeitstempel]“ um.
2.	Microsoft hält „MyDW\_DO\_NOT\_USE\_[Zeitstempel]“ an. Während dieser Zeit wird eine Sicherung erstellt. Es kann zu mehreren Anhaltevorgängen und Fortsetzungen kommen, falls bei diesem Prozess Probleme auftreten.
3.	Microsoft erstellt anhand der in Schritt 2 erstellen Sicherung ein neues Data Warehouse in Storage Premium mit dem Namen „MyDW“. „MyDW“ wird erst angezeigt, nachdem der Wiederherstellungsvorgang abgeschlossen wurde.
4.	Nach Abschluss der Wiederherstellung wird „MyDW“ zurück auf die gleichen DWUs und in den angehaltenen oder aktiven Zustand wie vor der Migration versetzt.
5.	Nachdem die Migration abgeschlossen ist, löscht Microsoft „MyDW\_DO\_NOT\_USE\_[Zeitstempel]“.
	
> [AZURE.NOTE] Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
> 
>	-  Auditing at the Database level needs to be re-enabled
>	-  Firewall rules at the **Database** level need to be readded.  Firewall rules at the **Server** level are not be impacted.

### Zeitplan für die automatische Migration
Automatische Migrationen finden zwischen 18:00 Uhr und 06:00 Uhr (Ortszeit in der jeweiligen Region) und im Rahmen des folgenden Ausfallzeitplans statt.

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
| USA, Westen-Mitte | 14\. August 2016 | 28\. August 2016 |
| USA (West) | 23\. Juni 2016 | 7\. Juli 2016 |
| USA, Westen 2 | 14\. August 2016 | 28\. August 2016 |

## Selbst durchgeführte Migration zu Storage Premium
Wenn Sie steuern möchten, wann genau Ihr Data Warehouse nicht verfügbar sein wird, können Sie die folgenden Schritte ausführen, um ein vorhandenes Data Warehouse von Storage Standard zu Storage Premium zu migrieren. Falls Sie sich für die selbst durchgeführte Migration entscheiden, müssen Sie diesen Vorgang vor Beginn der automatischen Migration in dieser Region durchführen. So vermeiden Sie das Risiko, dass die automatische Migration einen Konflikt verursacht (siehe [Zeitplan für die automatische Migration][]).

### Anleitung zur selbst durchgeführten Migration
Wenn Sie Ihre Ausfallzeiten selbst steuern möchten, können Sie die Migration für das Data Warehouse per Sicherung und Wiederherstellung selbst durchführen. Der Wiederherstellungsanteil der Migration dauert ca. eine Stunde pro TB an gespeicherten Daten pro Data Warehouse. Führen Sie die [Schritte zum Umbenennen während der Migration][] aus, wenn Sie nach Abschluss der Migration den gleichen Namen beibehalten möchten.

1.	[Anhalten][]\: Halten Sie das Data Warehouse an, damit eine automatische Sicherung erstellt wird.
2.	[Wiederherstellung][]\: Führen Sie eine Wiederherstellung aus der letzten Momentaufnahme durch.
3.	Löschen Sie das vorhandene DW unter Storage Standard. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses berechnet.**

> [AZURE.NOTE] Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
> 
>	-  Auditing at the Database level needs to be re-enabled
>	-  Firewall rules at the **Database** level need to be readded.  Firewall rules at the **Server** level are not be impacted.

#### Optional: Schritte zum Umbenennen während der Migration 
Zwei Datenbanken auf demselben logischen Server können nicht den gleichen Namen haben. SQL Data Warehouse unterstützt jetzt die Möglichkeit zum Umbenennen eines DW.

Beispiel: Ihr vorhandenes Data Warehouse mit Storage Standard trägt derzeit den Namen „MyDW“.

1.	Benennen Sie „MyDW“ um, indem Sie den ALTER DATABASE-Befehl nach einer Zeichenfolge ähnlich dieser ausführen: MyDW\_BeforeMigration. Dieser Befehl löscht alle vorhandenen Transaktionen und muss in der Masterdatenbank ausgeführt werden, um erfolgreich zu sein.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.	[Anhalten][]\: Halten Sie „MyDW\_BeforeMigration“ an, damit eine automatische Sicherung erstellt wird.
3.	[Wiederherstellen][]\: Stellen Sie aus der jüngsten Momentaufnahme eine neue Datenbank mit dem vorherigen Namen wieder her (z.B. „MyDW“).
4.	Löschen Sie „MyDW\_BeforeMigration“. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses berechnet.**

> [AZURE.NOTE] Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
> 
>	-  Auditing at the Database level needs to be re-enabled
>	-  Firewall rules at the **Database** level need to be readded.  Firewall rules at the **Server** level are not be impacted.

## Nächste Schritte
Mit der Änderung zu Storage Premium haben wir auch die Anzahl von Datenbank-Blobdateien in der zugrunde liegenden Architektur Ihrer Data Warehouse-Instanz erhöht. Falls Leistungsprobleme auftreten, empfehlen wir, die gruppierten Columnstore-Indizes mit dem folgenden Skript neu zu erstellen. Das Skript erzwingt, dass einige Ihrer Daten in den zusätzlichen Blobs gespeichert werden. Wenn Sie keine Maßnahmen ergreifen, werden die Daten im Laufe der Zeit auf natürliche Weise verteilt, sobald Sie weitere Daten in die Data Warehouse-Tabellen laden.

**Voraussetzungen:**

1.	Data Warehouse sollte mit 1.000 DWUs oder mehr ausgeführt werden (siehe [Skalieren von Computeleistung][]).
2.	Benutzer, die das Skript ausführen, sollten über die [Rolle „mediumrc“][] oder höher verfügen.
	1.	Führen Sie Folgendes aus, um dieser Rolle einen Benutzer hinzuzufügen:
		1.	````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Schritt 1: Erstellen der Tabelle zum Steuern der Indexneuerstellung
-- Ausführung als Benutzer unter „mediumrc“ oder höher
--------------------------------------------------------------------------------
create table sql\_statements WITH (distribution = round\_robin) as select 'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement, row\_number() over (order by s.name, t.name) as sequence from sys.schemas s inner join sys.tables t on s.schema\_id = t.schema\_id where is\_external = 0 ; go
 
--------------------------------------------------------------------------------
-- Schritt 2: Ausführen von Indexneuerstellungen: Wenn das Skript fehlschlägt, kann der unten angegebene Code erneut ausgeführt werden, um ab dem letzten Punkt fortzufahren.
-- Ausführung als Benutzer unter „mediumrc“ oder höher
--------------------------------------------------------------------------------

declare @nbr\_statements int = (select count(*) from sql\_statements) declare @i int = 1 while(@i <= @nbr\_statements) begin declare @statement nvarchar(1000)= (select statement from sql\_statements where sequence = @i) print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement exec (@statement) delete from sql\_statements where sequence = @i set @i += 1 end;
go
-------------------------------------------------------------------------------
-- Schritt 3: Bereinigen der in Schritt 1 erstellten Tabelle
--------------------------------------------------------------------------------
drop table sql\_statements; go ````

Wenn Probleme mit Ihrem Data Warehouse auftreten, können Sie [ein Supportticket erstellen][] und als möglichen Grund „Migration zu Storage Premium“ angeben.

<!--Image references-->

<!--Article references-->
[Zeitplan für die automatische Migration]: #automatic-migration-schedule
[Zeitplans für die automatische Migration]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[ein Supportticket erstellen]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Anhalten]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Wiederherstellen]: sql-data-warehouse-restore-database-portal.md
[Wiederherstellung]: sql-data-warehouse-restore-database-portal.md
[Schritte zum Umbenennen während der Migration]: #optional-steps-to-rename-during-migration
[Skalieren von Computeleistung]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[Rolle „mediumrc“]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Storage Premium eingeführt, um die Leistung besser vorhersagen zu können]: https://azure.microsoft.com/de-DE/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure-Portal]: https://portal.azure.com

<!---HONumber=AcomDC_0817_2016-->