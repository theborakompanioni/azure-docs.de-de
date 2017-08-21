---
title: "Azure SQL Data Warehouse-Sicherungen – Momentaufnahmen (georedundant) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie integrierte SQL Data Warehouse-Datenbanksicherungen Ihnen ermöglichen, ein Azure SQL Data Warehouse auf einen Wiederherstellungspunkt in einer anderen geografischen Region wiederherzustellen."
services: sql-data-warehouse
documentationcenter: 
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: ef2e6e7a19c7ed1730fdec5eca73c941e1b319c4
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="sql-data-warehouse-backups"></a>SQL Data Warehouse-Sicherungen
SQL Data Warehouse bietet sowohl lokale als auch geografische Sicherungen im Rahmen der Data Warehouse-Sicherungsfunktionalität. Hierzu gehören Azure Storage Blob-Momentaufnahmen und georedundante Speicher. Verwenden Sie Data Warehouse-Sicherungen, um Ihr Data Warehouse auf einen Wiederherstellungspunkt in der primären Region oder in einer anderen geografischen Region wiederherzustellen. Dieser Artikel erläutert die Merkmale von Sicherungen in SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Was ist eine Data Warehouse-Sicherung?
Bei einer Data Warehouse-Sicherung handelt es sich um die Daten, die Sie verwenden können, um ein Data Warehouse auf einen bestimmten Zeitpunkt wiederherzustellen.  Da SQL Data Warehouse ein verteiltes System ist, besteht eine Data Warehouse-Sicherung aus vielen Dateien, die in Azure-Blobs gespeichert sind. 

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Datenredundanz
SQL Data Warehouse schützt Ihre Daten durch Speichern der Daten in lokal redundantem Azure Storage Premium-Speicher. Dieses Azure Storage-Feature speichert mehrere synchrone Kopien der Daten im lokalen Rechenzentrum, um bei lokalen Ausfällen einen transparenten Datenschutz sicherzustellen. Die Datenredundanz gewährleistet, dass Ihre Daten gegen Infrastrukturprobleme wie Datenträgerausfälle geschützt sind. Außerdem gewährleistet die Datenredundanz Geschäftskontinuität mit einer fehlertoleranten und hoch verfügbaren Infrastruktur.

Weitere Informationen:

* Azure Premium-Speicher: siehe [Einführung in Azure Storage Premium](../storage/storage-premium-storage.md).
* Lokal redundanter Speicher: siehe [Azure Storage-Replikation](../storage/storage-redundancy.md#locally-redundant-storage).

## <a name="azure-storage-blob-snapshots"></a>Azure Storage Blob-Momentaufnahmen
Azure Storage Premium bietet den Vorteil, dass SQL Data Warehouse Azure Storage Blob-Momentaufnahmen verwenden kann, um das Data Warehouse lokal zu sichern. Sie können ein Data Warehouse auf einen Wiederherstellungspunkt einer Momentaufnahme wiederherstellen. Momentaufnahmen werden mindestens alle acht Stunden gestartet und sind sieben Tage lang verfügbar.  

Weitere Informationen:

* Azure Storage Blob-Momentaufnahmen: siehe [Erstellen einer Momentaufnahme eines Blobs](../storage/storage-blob-snapshots.md).

## <a name="geo-redundant-backups"></a>Georedundante Sicherungen
SQL Data Warehouse speichert das gesamte Data Warehouse alle 24 Stunden in einem Standardspeicher. Das vollständige Data Warehouse wird gemäß der Uhrzeit der letzten Momentaufnahme erstellt. Der Standardspeicher gehört zu einem georedundanten Speicherkonto mit Lesezugriff (RA-GRS). Die RA-GRS-Funktion von Azure Storage repliziert die Sicherungsdateien in einem [gekoppelten Rechenzentrum](../best-practices-availability-paired-regions.md). Diese Georeplikation stellt sicher, dass Sie ein Data Warehouse wiederherstellen können, falls Sie nicht auf die Momentaufnahmen in Ihrer primären Region zugreifen können. 

Dieses Feature ist standardmäßig aktiviert. Wenn Sie georedundante Sicherungen nicht verwenden möchten, können Sie sich [Abmelden] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn). 

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden. 
> 
> 

> [!NOTE]
> Sie können georedundante Sicherungen mit DWU 9000 und DWU 18000 nicht deaktivieren. 
>
> 

Weitere Informationen:

* Informationen zum georedundanten Speichern finden Sie unter [Azure Storage-Replikation](../storage/storage-redundancy.md).
* Informationen zum RA-GRS-Speicher finden Sie unter [Georedundanter Speicher mit Lesezugriff](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Sicherungszeitplan und Aufbewahrungsdauer für ein Data Warehouse
SQL Data Warehouse erstellt alle vier bis acht Stunden Momentaufnahmen in Ihren Online-Data Warehouses und bewahrt jede Momentaufnahme sieben Tage lang auf. Sie können Ihre Onlinedatenbank auf einen der Wiederherstellungspunkte der vergangenen sieben Tage wiederherstellen. 

Führen Sie die folgende Abfrage in Ihrer SQL Data Warehouse-Online-Instanz aus, um zu ermitteln, wann die letzte Momentaufnahme gestartet wurde. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Wenn Sie eine Momentaufnahme länger als sieben Tage aufbewahren möchten, können Sie einen Wiederherstellungspunkt in einem neuen Data Warehouse wiederherstellen. Nach Abschluss der Wiederherstellung beginnt SQL Data Warehouse damit, Momentaufnahmen im neuen Data Warehouse zu erstellen. Wenn Sie keine Änderungen am neuen Data Warehouse vornehmen, bleiben die Momentaufnahmen leer, daher fallen nur minimale Kosten für die Momentaufnahmen an. Sie können das Data Warehouse auch anhalten, um zu verhindern, dass SQL Data Warehouse Momentaufnahmen erstellt.

### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Was passiert mit der Sicherungsaufbewahrung, während das Data Warehouse angehalten ist?
Während ein Data Warehouse angehalten ist, erstellt SQL Data Warehouse keine Momentaufnahmen, und Momentaufnahmen laufen nicht ab. Das Alter der Momentaufnahmen ändert sich nicht, während das Data Warehouse angehalten ist. Die Aufbewahrung einer Momentaufnahme basiert auf der Anzahl von Tagen, die das Data Warehouse online ist, nicht auf Kalendertagen.

Wenn eine Momentaufnahme z.B. am 1. Oktober um 16 Uhr gestartet wurde und das Data Warehouse am 3. Oktober um 16 Uhr angehalten wird, ist die Momentaufnahme zwei Tage alt. Unabhängig davon, wann das Data Warehouse wieder online geschaltet wird – die Momentaufnahme bleibt zwei Tage alt. Wenn das Data Warehouse am 5. Oktober um 16 Uhr wieder online geschaltet wird, ist die Momentaufnahme zwei Tage alt und wird weitere fünf Tage lang aufbewahrt.

Wenn das Data Warehouse wieder online geschaltet wird, setzt SQL Data Warehouse die Erstellung neuer Momentaufnahmen fort, und Momentaufnahmen mit Daten, die älter sind als sieben Tage, laufen ab.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Wie lang ist die Aufbewahrungsdauer für ein gelöschtes Data Warehouse?
Wenn ein Data Warehouse gelöscht wird, werden Data Warehouse und Momentaufnahmen sieben Tage lang gespeichert und dann entfernt. Sie können das Data Warehouse auf jeden der gespeicherten Wiederherstellungspunkte wiederherstellen.

> [!IMPORTANT]
> Wenn Sie eine logische SQL Server-Instanz löschen, werden auch alle Datenbanken der Instanz gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.
> 
> 

## <a name="data-warehouse-backup-costs"></a>Kosten für Data Warehouse-Sicherungen
Die Gesamtkosten für Ihr primäres Data Warehouse und sieben Tage Azure Blob-Momentaufnahmen werden auf die nächsten TB aufgerundet. Wenn Ihr Data Warehouse z.B. 1,5 TB umfasst und die Momentaufnahmen 100 GB verwenden, werden Ihnen 2 TB Daten zu Azure Storage Premium-Sätzen in Rechnung gestellt. 

> [!NOTE]
> Jede Momentaufnahme ist anfangs leer und wächst, wenn Sie Änderungen am primären Data Warehouse vornehmen. Alle Momentaufnahmen werden größer, wenn das Data Warehouse geändert wird. Daher steigen auch die Speicherkosten für Momentaufnahmen entsprechend der Änderungsrate.
> 
> 

Bei Verwendung von georedundantem Speicher wird Ihnen dieser separat in Rechnung gestellt. Der georedundante Speicher wird zum Standardsatz für georedundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) berechnet.

Weitere Informationen zu den Preisen für SQL Data Warehouse finden Sie in der [Preisübersicht für SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Verwenden von Datenbanksicherungen
Der Hauptzweck von SQL Data Warehouse-Sicherungen ist es, das Data Warehouse auf einen der Wiederherstellungspunkte innerhalb des Aufbewahrungszeitraums wiederherstellen zu können.  

* Informationen zum Wiederherstellen von SQL Data Warehouse finden Sie unter [Wiederherstellen einer SQL Data Warehouse-Instanz](sql-data-warehouse-restore-database-overview.md).

## <a name="related-topics"></a>Verwandte Themen
### <a name="scenarios"></a>Szenarien
* Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](../sql-database/sql-database-business-continuity.md).

<!-- ### Tasks -->

* Informationen zum Wiederherstellen eines Data Warehouse finden Sie unter [Wiederherstellen einer SQL Data Warehouse-Instanz](sql-data-warehouse-restore-database-overview.md).

<!-- ### Tutorials -->


