---
title: Migrieren eines vorhandenen Azure Data Warehouse zu Storage Premium | Microsoft-Dokumentation
description: Anweisungen zum Migrieren eines vorhandenen Data Warehouse zu Storage Premium
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
ms.sourcegitcommit: e66f808da8d301e0adc393ba0ae67ab8618ce814
ms.openlocfilehash: e73e52665dd22e33054745907613c269b6d57915


---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a>Migrieren eines Data Warehouse zu Storage Premium
Für Azure SQL Data Warehouse wurde vor Kurzem [Storage Premium eingeführt, um die Leistung besser vorhersagen zu können][premium storage for greater performance predictability]. Vorhandene Data Warehouses mit derzeit Standardspeicher können nun zu Storage Premium migriert werden. Sie können die automatische Migration nutzen. Wenn Sie aber den Migrationszeitpunkt bestimmen möchten (was eine gewisse Ausfallzeit mit sich bringt), können Sie die Migration selbst vornehmen.

Wenn Sie über mehr als ein Data Warehouse verfügen, ermitteln Sie anhand des [Zeitplans für die automatische Migration][automatic migration schedule], wann die einzelnen Migrationen erfolgen.

## <a name="determine-storage-type"></a>Bestimmen des Speichertyps
Wenn Sie ein Data Warehouse vor den folgenden Terminen erstellt haben, verwenden Sie derzeit Standardspeicher.

| **Region** | **Vor diesem Datum erstelltes Data Warehouse** |
|:--- |:--- |
| Australien (Osten) |Storage Premium noch nicht verfügbar |
| China, Osten |1. November 2016 |
| China, Norden |1. November 2016 |
| Deutschland, Mitte |1. November 2016 |
| Deutschland, Nordosten |1. November 2016 |
| Indien, Westen |Storage Premium noch nicht verfügbar |
| Japan (Westen) |Storage Premium noch nicht verfügbar |
| USA Nord Mitte |10. November 2016 |

## <a name="automatic-migration-details"></a>Details zur automatischen Migration
Standardmäßig migrieren wir Ihre Datenbank zwischen 18:00 und 06:00 Uhr Ortszeit Ihrer Region, und zwar im Rahmen des [Zeitplans für die automatische Migration][automatic migration schedule]. Während der Migration kann Ihr vorhandenes Data Warehouse nicht genutzt werden. Die Migration dauert pro Data Warehouse pro Terabyte an gespeicherten Daten ca. eine Stunde. Im Rahmen der automatischen Migration fallen keine Kosten für Sie an.

> [!NOTE]
> Nach Abschluss der Migration wird das Data Warehouse zur Nutzung wieder online geschaltet.
>
>

Microsoft führt die folgenden Schritte aus, um die Migration abzuschließen (die von Ihnen keine Beteiligung verlangen). Beispiel: Ihr vorhandenes Data Warehouse mit Standardspeicher hat derzeit den Namen „MyDW“.

1. Microsoft benennt „MyDW“ in „MyDW_DO_NOT_USE_[Zeitstempel]“ um.
2. Microsoft hält „MyDW_DO_NOT_USE_[Zeitstempel]“ an. Während dieser Zeit wird eine Sicherung erstellt. Es kann zu mehreren Anhalte- und Fortsetzungsvorgängen kommen, falls bei diesem Prozess Probleme auftreten.
3. Microsoft erstellt anhand der in Schritt 2 erstellen Sicherung ein neues Data Warehouse in Storage Premium mit dem Namen „MyDW“. „MyDW“ wird erst angezeigt, nachdem der Wiederherstellungsvorgang abgeschlossen wurde.
4. Nach Abschluss der Wiederherstellung wird „MyDW“ zurück auf die gleichen Data Warehouse-Einheiten und in den (angehaltenen oder aktiven) Zustand wie vor der Migration versetzt.
5. Nach Abschluss der Migration löscht Microsoft „MyDW_DO_NOT_USE_[Zeitstempel]“.

> [!NOTE]
> Die folgenden Einstellungen werden im Rahmen der Migration nicht übernommen:
>
> * Die Überwachung auf Datenbankebene muss erneut aktiviert werden.
> * Firewallregeln auf Datenbankebene müssen erneut hinzugefügt werden. Firewallregeln auf Serverebene sind nicht betroffen.
>
>

### <a name="automatic-migration-schedule"></a>Zeitplan für die automatische Migration
Automatische Migrationen erfolgen zwischen 18:00 Uhr und 06:00 Uhr (Ortszeit in der jeweiligen Region) und im Rahmen des folgenden Ausfallzeitplans.

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
Wenn Sie steuern möchten, wann genau Ihr Data Warehouse nicht verfügbar sein soll, können Sie die folgenden Schritte ausführen, um ein vorhandenes Data Warehouse von Standardspeicher zu Storage Premium zu migrieren. Wenn Sie sich hierfür entscheiden, müssen Sie die selbst durchgeführte Migration vor Beginn der automatischen Migration in dieser Region abschließen. Dadurch wird sichergestellt, dass Sie alle Risiken aufgrund eines Konflikts mit der automatischen Migration vermeiden (siehe den [Zeitplan für die automatische Migration][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Anleitung zur selbst durchgeführten Migration
Verwenden Sie zum Migrieren Ihres Data Warehouse die Sicherung- und Wiederherstellungsfunktionen. Der Wiederherstellungsanteil der Migration dauert pro Data Warehouse pro TB gespeicherter Daten ca. eine Stunde. Führen Sie die [Schritte zum Umbenennen während der Migration][steps to rename during migration] aus, wenn Sie nach Abschluss der Migration den gleichen Namen beibehalten möchten.

1. [Halten][Pause] Sie Ihr Data Warehouse an. Daraufhin wird eine automatische Sicherung erstellt.
2. Führen Sie eine [Wiederherstellung][Restore] der neuesten Momentaufnahme durch.
3. Löschen Sie das vorhandene Data Warehouse in Standardspeicher. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses in Rechnung gestellt.**

> [!NOTE]
> Die folgenden Einstellungen werden im Rahmen der Migration nicht übernommen:
>
> * Die Überwachung auf Datenbankebene muss erneut aktiviert werden.
> * Firewallregeln auf Datenbankebene müssen erneut hinzugefügt werden. Firewallregeln auf Serverebene sind nicht betroffen.
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a>Umbenennen des Data Warehouse während der Migration (optional)
Zwei Datenbanken auf demselben logischen Server können nicht den gleichen Namen haben. SQL Data Warehouse unterstützt jetzt die Möglichkeit zum Umbenennen eines Data Warehouse.

Beispiel: Ihr vorhandenes Data Warehouse mit Standardspeicher hat derzeit den Namen „MyDW“.

1. Benennen Sie "MyDW" mithilfe des folgenden ALTER DATABASE-Befehls um. (In diesem Beispiel erfolgt eine Umbenennung in „MyDW_BeforeMigration“.) Dieser Befehl beendet alle vorhandenen Transaktionen und muss in der Masterdatenbank ausgeführt werden, um erfolgreich zu sein.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Halten][Pause] Sie „MyDW_BeforeMigration“ an. Daraufhin wird eine automatische Sicherung erstellt.
3. Führen Sie mithilfe der neuesten Momentaufnahme eine [Wiederherstellung][Restore] durch, und erstellen Sie dabei eine neue Datenbank mit dem vorherigen Namen (z.B. „MyDW“).
4. Löschen Sie „MyDW_BeforeMigration“. **Wenn Sie diesen Schritt nicht ausführen, werden Ihnen beide Data Warehouses in Rechnung gestellt.**


## <a name="next-steps"></a>Nächste Schritte
Mit dem Wechsel zu Storage Premium haben wir auch die Anzahl von Datenbank-Blobdateien in der zugrunde liegenden Architektur Ihrer Data Warehouse-Instanz erhöht. Um die Leistungsvorteile dieses Wechsels zu maximieren, erstellen Sie Ihre gruppierten Columnstore-Indizes mit dem folgenden Skript neu. Das Skript erzwingt, dass einige Ihrer Daten in den zusätzlichen Blobs gespeichert werden. Wenn Sie keine Maßnahmen ergreifen, werden die Daten im Laufe der Zeit auf natürliche Weise verteilt, sobald Sie weitere Daten in Ihre Tabellen laden.

**Voraussetzungen:**

- Das Data Warehouse sollte mit 1.000 Data Warehouse-Einheiten oder mehr ausgeführt werden (siehe [Skalieren von Computeleistung][scale compute power]).
- Der Benutzer, der das Skript ausführt, muss die [Rolle „mediumrc“][mediumrc role] oder höher haben. Führen Sie Folgendes aus, um dieser Rolle einen Benutzer hinzuzufügen:     ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table to control index rebuild
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
-- Step 2: Execute index rebuilds. If script fails, the below can be re-run to restart where last left off.
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
-- Step 3: Clean up table created in Step 1
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



<!--HONumber=Dec16_HO3-->


