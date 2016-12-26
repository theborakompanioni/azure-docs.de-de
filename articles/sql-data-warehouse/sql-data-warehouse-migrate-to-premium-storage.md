---
title: Migrieren eines vorhandenen Azure SQL Data Warehouse zu Storage Premium | Microsoft Azure
description: Anweisungen zum Migrieren eines vorhandenen SQL Data Warehouse in Storage Premium
services: sql-data-warehouse
documentationcenter: NA
author: happynicolle
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/29/2016
ms.author: rortloff;barbkess
translationtype: Human Translation
ms.sourcegitcommit: b676630e44c49c2ab4006f04408b01cc90c4dc28
ms.openlocfilehash: ef20cf90b54c9b28c70341d7545bb55474feb39f


---
# <a name="migration-to-premium-storage-details"></a>Details zur Migration zu Storage Premium
Für SQL Data Warehouse wurde vor Kurzem [Storage Premium eingeführt, um die Leistung besser vorhersagen zu können][Premium Storage for greater performance predictability].  Wir sind jetzt bereit für das Migrieren vorhandener Data Warehouses, für die derzeit Storage Standard verwendet wird, zu Storage Premium.  Im Folgenden erhalten Sie weitere Details dazu, wie und wann automatische Migrationen erfolgen und wie Sie die Migration selbst durchführen, um den Zeitpunkt der Ausfallzeiten zu steuern.

Wenn Sie über mehr als ein Data Warehouse verfügen, ermitteln Sie anhand des unten angegebenen [Zeitplans für die automatische Migration][automatic migration schedule], wann die einzelnen Migrationen durchgeführt werden.

## <a name="determine-storage-type"></a>Bestimmen des Speichertyps
Wenn Sie ein Data Warehouse vor den unten angegebenen Terminen erstellt haben, verwenden Sie derzeit Storage Standard.

| **Region** | **Vor diesem Datum erstelltes DW** |
|:--- |:--- |
| Australien (Osten) |Storage Premium noch nicht verfügbar |
| China, Osten |1. November 2016 |
| China, Norden |1. November 2016 |
| Deutschland, Mitte |1. November 2016 |
| Deutschland, Nordosten |1. November 2016 |
| Indien, Westen |Storage Premium noch nicht verfügbar |
| Japan (Westen) |Storage Premium noch nicht verfügbar |
| USA (Mitte/Norden) |10. November 2016 |

## <a name="automatic-migration-details"></a>Details zur automatischen Migration
Standardmäßig migrieren wir Ihre Datenbank zwischen 18:00 und 06:00 Uhr Ortszeit Ihrer Region, und zwar während des unten angegebenen [Zeitplans für die automatische Migration][automatic migration schedule].  Während der Migration kann Ihr vorhandenes Data Warehouse nicht genutzt werden.  Die Migration dauert ca. eine Stunde pro TB an gespeicherten Daten pro Data Warehouse.  Außerdem stellen wir sicher, dass während der automatischen Migration keine Kosten für Sie anfallen.

> [!NOTE]
> Während der Migration können Sie Ihr vorhandenes Data Warehouse nicht nutzen.  Nachdem die Migration abgeschlossen ist, ist das Data Warehouse wieder online.
>
>

Bei den unten angegebenen Details handelt es sich um Schritte, die Microsoft für Sie ausführt, um die Migration abzuschließen. Hierfür ist kein Eingriff Ihrerseits erforderlich.  Beispiel: Ihr vorhandenes Data Warehouse mit Storage Standard trägt derzeit den Namen „MyDW“.

1. Microsoft benennt „MyDW“ in „MyDW_DO_NOT_USE_[Zeitstempel]“ um.
2. Microsoft hält „MyDW_DO_NOT_USE_[Zeitstempel]“ an.  Während dieser Zeit wird eine Sicherung erstellt.  Es kann zu mehreren Anhaltevorgängen und Fortsetzungen kommen, falls bei diesem Prozess Probleme auftreten.
3. Microsoft erstellt anhand der in Schritt 2 erstellen Sicherung ein neues Data Warehouse in Storage Premium mit dem Namen „MyDW“.  „MyDW“ wird erst angezeigt, nachdem der Wiederherstellungsvorgang abgeschlossen wurde.
4. Nach Abschluss der Wiederherstellung wird „MyDW“ zurück auf die gleichen DWUs und in den angehaltenen oder aktiven Zustand wie vor der Migration versetzt.
5. Nachdem die Migration abgeschlossen ist, löscht Microsoft „MyDW_DO_NOT_USE_[Zeitstempel]“.

> [!NOTE]
> Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
>
> * Die Überwachung auf Datenbankebene muss erneut aktiviert werden.
> * Firewallregeln auf **Datenbankebene** müssen erneut hinzugefügt werden.  Firewallregeln auf **Serverebene** sind nicht betroffen.
>
>

### <a name="automatic-migration-schedule"></a>Zeitplans für die automatische Migration
Automatische Migrationen finden zwischen 18:00 Uhr und 06:00 Uhr (Ortszeit in der jeweiligen Region) und im Rahmen des folgenden Ausfallzeitplans statt.

| **Region** | **Geschätztes Startdatum** | **Geschätztes Enddatum** |
|:--- |:--- |:--- |
| Australien (Osten) |Noch nicht festgelegt |Noch nicht festgelegt |
| China, Osten |9. Januar 2017 |13. Januar 2017 |
| China, Norden |9. Januar 2017 |13. Januar 2017 |
| Deutschland, Mitte |9. Januar 2017 |13. Januar 2017 |
| Deutschland, Nordosten |9. Januar 2017 |13. Januar 2017 |
| Indien, Westen |Noch nicht festgelegt |Noch nicht festgelegt |
| Japan (Westen) |Noch nicht festgelegt |Noch nicht festgelegt |
| USA (Mitte/Norden) |9. Januar 2017 |13. Januar 2017 |

## <a name="self-migration-to-premium-storage"></a>Selbst durchgeführte Migration zu Storage Premium
Wenn Sie steuern möchten, wann genau Ihr Data Warehouse nicht verfügbar sein wird, können Sie die folgenden Schritte ausführen, um ein vorhandenes Data Warehouse von Storage Standard zu Storage Premium zu migrieren.  Falls Sie sich für die selbst durchgeführte Migration entscheiden, müssen Sie diesen Vorgang vor Beginn der automatischen Migration in dieser Region durchführen. So vermeiden Sie das Risiko, dass die automatische Migration einen Konflikt verursacht (siehe [Zeitplan für die automatische Migration][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Anleitung zur selbst durchgeführten Migration
Wenn Sie Ihre Ausfallzeiten selbst steuern möchten, können Sie die Migration für das Data Warehouse per Sicherung und Wiederherstellung selbst durchführen.  Der Wiederherstellungsanteil der Migration dauert ca. eine Stunde pro TB an gespeicherten Daten pro Data Warehouse.  Führen Sie die [Schritte zum Umbenennen während der Migration][steps to rename during migration] aus, wenn Sie nach Abschluss der Migration den gleichen Namen beibehalten möchten.

1. [Anhalten][Pause]: Halten Sie das Data Warehouse an, damit eine automatische Sicherung erstellt wird.
2. [Wiederherstellung][Restore]: Führen Sie eine Wiederherstellung aus der letzten Momentaufnahme durch.
3. Löschen Sie das vorhandene DW unter Storage Standard. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses berechnet.**

> [!NOTE]
> Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
>
> * Die Überwachung auf Datenbankebene muss erneut aktiviert werden.
> * Firewallregeln auf **Datenbankebene** müssen erneut hinzugefügt werden.  Firewallregeln auf **Serverebene** sind nicht betroffen.
>
>

#### <a name="optional-steps-to-rename-during-migration"></a>Optional: Schritte zum Umbenennen während der Migration
Zwei Datenbanken auf demselben logischen Server können nicht den gleichen Namen haben. SQL Data Warehouse unterstützt jetzt die Möglichkeit zum Umbenennen eines DW.

Beispiel: Ihr vorhandenes Data Warehouse mit Storage Standard trägt derzeit den Namen „MyDW“.

1. Benennen Sie „MyDW“ um, indem Sie den ALTER DATABASE-Befehl nach einer Zeichenfolge ähnlich dieser ausführen: MyDW_BeforeMigration.  Dieser Befehl löscht alle vorhandenen Transaktionen und muss in der Masterdatenbank ausgeführt werden, um erfolgreich zu sein.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Anhalten][Pause]: Halten Sie „MyDW_BeforeMigration“ an, damit eine automatische Sicherung erstellt wird.
3. [Wiederherstellen][Restore]: Stellen Sie aus der jüngsten Momentaufnahme eine neue Datenbank mit dem vorherigen Namen wieder her (z.B. „MyDW“).
4. Löschen Sie „MyDW_BeforeMigration“.  **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses berechnet.**

> [!NOTE]
> Diese Einstellungen werden im Rahmen der Migration nicht übernommen:
>
> * Die Überwachung auf Datenbankebene muss erneut aktiviert werden.
> * Firewallregeln auf **Datenbankebene** müssen erneut hinzugefügt werden.  Firewallregeln auf **Serverebene** sind nicht betroffen.
>
>

## <a name="next-steps"></a>Nächste Schritte
Mit der Änderung zu Storage Premium haben wir auch die Anzahl von Datenbank-Blobdateien in der zugrunde liegenden Architektur Ihrer Data Warehouse-Instanz erhöht.  Um die Leistungsvorteile dieser Änderung zu maximieren, empfehlen wir Ihnen, die gruppierten Columnstore-Indizes mit dem folgenden Skript neu zu erstellen.  Das Skript erzwingt, dass einige Ihrer Daten in den zusätzlichen Blobs gespeichert werden.  Wenn Sie keine Maßnahmen ergreifen, werden die Daten im Laufe der Zeit auf natürliche Weise verteilt, sobald Sie weitere Daten in die Data Warehouse-Tabellen laden.

**Voraussetzungen:**

1. Data Warehouse sollte mit 1.000 DWUs oder mehr ausgeführt werden (siehe [Skalieren von Computeleistung][scale compute power]).
2. Benutzer, die das Skript ausführen, sollten über die [Rolle „mediumrc“][mediumrc role] oder höher verfügen.
   1. Führen Sie Folgendes aus, um dieser Rolle einen Benutzer hinzuzufügen:
      1. ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Wenn Probleme mit Ihrem Data Warehouse auftreten, können Sie [ein Supportticket erstellen][create a support ticket] und als möglichen Grund „Migration zu Storage Premium“ angeben.

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com



<!--HONumber=Nov16_HO5-->


