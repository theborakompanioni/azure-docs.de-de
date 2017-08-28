---
title: "Azure SQL-Datenbank – Dienstebenen und Leistungsstufen | Microsoft-Dokumentation"
description: "Enthält einen Vergleich von SQL-Datenbank-Dienstebenen und -Leistungsebenen für Einzeldatenbanken und eine Einführung in elastische SQL-Pools."
keywords: Datenbankoptionen, Datenbankleistung
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/30/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 760543dc3880cb0dbe14070055b528b94cffd36b
ms.openlocfilehash: b25ff5331f119efd44c61808f7d1d5decb226bd6
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="what-performance-options-are-available-for-an-azure-sql-database"></a>Verfügbare Leistungsoptionen für eine Azure SQL-Datenbank

[Azure SQL-Datenbank](sql-database-technical-overview.md) verfügt über vier Dienstebenen für Einzeldatenbanken und Datenbanken in [Pools](sql-database-elastic-pool.md). Diese Dienstebenen lauten: **Basic**, **Standard**, **Premium** und **Premium RS**. Jede Dienstebene verfügt über mehrere Leistungsebenen ([DTUs](sql-database-what-is-a-dtu.md)) und Speicheroptionen zum Behandeln verschiedener Workloads und Datengrößen. Bei höheren Leistungsebenen stehen zusätzliche Compute- und Speicherressourcen zur Verfügung, um einen höheren Durchsatz und eine höhere Kapazität zu ermöglichen. Sie können die Dienstebenen, Leistungsebenen und den Speicher dynamisch und ohne Ausfallzeiten ändern. 
- Die Dienstebenen **Basic**, **Standard** und **Premium** haben jeweils eine Betriebszeit-SLA von 99,99 Prozent und bieten flexible Optionen für Geschäftskontinuität, Sicherheitsfeatures und stündliche Abrechnung. 
- Die Tarifebene **Premium RS** verfügt über die gleichen Leistungsebenen wie die Ebene „Premium“, aber es gilt eine reduzierte SLA. Der Grund ist, dass eine geringere Anzahl von redundanten Kopien als bei einer Datenbank der anderen Ebenen ausgeführt wird. Im Falle eines Dienstfehlers müssen Sie daher Ihre Datenbank unter Umständen aus einer Sicherung mit einem Rückstand von bis zu 5 Minuten wiederherstellen.

> [!IMPORTANT]
> Eine Azure SQL-Datenbank erhält einen garantierten Ressourcensatz, und die erwarteten Leistungsmerkmale Ihrer Datenbank werden von den anderen Datenbanken in Azure nicht beeinträchtigt. 

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene
In der folgenden Tabelle sind Beispiele für Dienstebenen aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Dienstebene | Zielworkloads |
| :--- | --- |
| **Basic** | Eignet sich am besten für eine kleine Datenbank, in der normalerweise ein einzelner aktiver Vorgang zu einem bestimmten Zeitpunkt unterstützt wird. Beispiele hierfür sind Datenbanken, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| **Standard** |Optimale Option für Cloudanwendungen mit geringen bis mittleren E/A-Leistungsanforderungen. Unterstützt mehrere parallele Abfragen. Beispiele hierfür sind Arbeitsgruppen-oder Webanwendungen. |
| **Premium** | Konzipiert für hohes Transaktionsvolumen mit hohen E/A-Leistungsanforderungen. Unterstützt zahlreiche gleichzeitige Benutzer. Beispiele hierfür sind Datenbanken für unternehmenskritische Anwendungen. |
| **Premium RS** | Speziell für E/A-intensive Workloads, die nicht die höchsten Verfügbarkeitsgarantien benötigen. Beispiele hierfür sind das Testen von Hochleistungsworkloads oder eine analytische Workload, wo die Datenbank nicht das bevorzugte System ist. |
|||

Sie können Einzeldatenbanken mit dedizierten Ressourcen einer Dienstebene auf einer bestimmten [Leistungsebene](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) erstellen, oder Sie können Datenbanken in einem [Pool für elastische SQL-Datenbanken](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) erstellen. In einem Pool für elastische SQL-Datenbanken werden die Compute- und Speicherressourcen auf einem einzelnen logischen Server über mehrere Datenbanken hinweg gemeinsam genutzt. 

Für Einzeldatenbanken verfügbare Ressourcen werden als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) bezeichnet, und Ressourcen für einen Pool für elastische SQL-Datenbanken werden als elastische DTUs bzw. eDTUs (elastic Database Transaction Units) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-what-is-a-dtu.md).

Bestimmen Sie zu Beginn die Datenbankfeatures, die Sie mindestens benötigen:

| **Dienstebenenfeatures** | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximale Größe von Einzeldatenbanken | 2 GB | 250 GB | 4TB*  | 500 GB  |
| Maximale Größe des Pools für elastische Datenbanken | 156 GB | 2,9 TB | 4TB* | 750 GB |
| Maximale Datenbankgröße in einem Pool für elastische Datenbanken | 2 GB | 250 GB | 500 GB | 500 GB |
| Maximale Anzahl von Datenbanken pro Pool | 500  | 500 | 100 | 100 |
| Maximale Einzeldatenbanken-DTUs | 5 | 100 | 4000 | 1000 |
| Maximale DTUs pro Datenbank in einem Pool für elastische Datenbanken | 5 | 3000 | 4000 | 1000 |
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 Tage | 35 Tage | 35 Tage | 35 Tage |
||||||

> [!IMPORTANT]
> Speicher von bis zu 4 TB ist derzeit in den folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. Informationen finden Sie unter [Aktuelle Einschränkungen von P11- und P15-Datenbanken mit einem MAXSIZE-Wert von 4 TB](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize).
>

Nach der Ermittlung der geeigneten Dienstebene können Sie die Leistungsebene (Anzahl von DTUs) und die Speichermenge für die Datenbank bestimmen. 

- Die Leistungsebenen S2 und S3 des Tarifs **Standard** sind meist ein guter Ausgangspunkt. 
- Für Datenbanken mit hohen CPU- oder E/A-Anforderungen empfehlen sich dagegen eher die Leistungsebenen des Tarifs **Premium**. 
- Der Tarif **Premium** bietet im Vergleich zur höchsten Leistungsebene des Tarifs **Standard** mehr CPU- und eine mindestens zehnmal höhere E/A-Leistung.
- Der Tarif **PremiumRS** verfügt über die Leistung des Tarifs **Premium** zu geringeren Kosten, aber die SLA ist reduziert.

> [!IMPORTANT]
> Lesen Sie sich das Thema zu [elastischen SQL-Pools](sql-database-elastic-pool.md) durch, um ausführliche Informationen zum Gruppieren von Datenbanken in elastischen SQL-Pools zur gemeinsamen Nutzung von Compute- und Speicherressourcen zu erhalten. Im restlichen Teil dieses Themas geht es um Dienstebenen und Leistungsebenen für Einzeldatenbanken.
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Tarife und Leistungsebenen für Einzeldatenbanken
Bei Einzeldatenbanken gibt es mehrere Leistungsebenen und Speichermengen auf den einzelnen Dienstebenen. 

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Zentrales Hoch- oder Herunterskalieren einer einzelnen Datenbank

Nach dem Auswählen eines Tarifs und einer Leistungsebene können Sie eine einzelne Datenbank dynamisch und bedarfsgerecht zentral hoch- oder herunterskalieren.  

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Die Dauer der Umstellung kann variieren, aber sie liegt im Allgemeinen unter vier Sekunden und in 99% der Fälle unter 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, kann die Umstellung unter Umständen auch länger dauern.  

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in, aus oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Eine Datenbank der gleichen Größe, die ihre Leistungsebenen innerhalb des Premium-Diensttarifs ändert, sollte dies innerhalb von drei Stunden vollziehen.

> [!TIP]
> Um den Status eines laufenden Skalierungsvorgangs für SQL-Datenbank zu überprüfen, verwenden Sie die folgende Abfrage: ```select * from sys.dm_operation_status```.
>

* Wenn Sie ein Upgrade auf eine höhere Dienst- oder Leistungsebene durchführen, wird die maximale Datenbankgröße nicht erhöht, sofern Sie nicht ausdrücklich eine höhere Maximalgröße angeben.
* Für ein Downgrade einer Datenbank muss die Datenbank kleiner als die maximal zulässige Größe der Zieldienstebene sein. 
* Bei einem Upgrade einer Datenbank, für die [Georeplikation](sql-database-geo-replication-portal.md) aktiviert ist, führen Sie vor dem Upgrade der primären Datenbank zunächst ein Upgrade der zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Anleitung). Bei einem Upgrade auf eine andere Edition muss zunächst ein Upgrade der sekundären Datenbank durchgeführt werden. 
* Bei einem Downgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Downgraden der sekundären Datenbank zunächst ein Downgrade der zugehörigen primären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Anleitung). Bei einem Downgrade auf eine andere Edition muss zunächst ein Downgrade der primären Datenbank durchgeführt werden. 

* Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade auf den Tarif **Basic** durchführen, verfügen Sie über einen kürzeren Aufbewahrungszeitraum von Sicherungen. Weitere Informationen finden Sie unter [Azure SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.


## <a name="current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize"></a>Aktuelle Einschränkungen von P11- und P15-Datenbanken mit einem MAXSIZE-Wert von 4 TB

In einigen Regionen wird (wie zuvor beschrieben) eine Maximalgröße von 4 TB für P11- und P15-Datenbanken unterstützt. Die folgenden Aspekte und Einschränkungen gelten für P11- und P15-Datenbanken mit einer maximalen Größe von 4 TB:

- Wenn Sie beim Erstellen einer Datenbank die Maximalgröße von 4 TB wählen (Wert 4 TB oder 4096 GB), tritt für den Erstellungsbefehl ein Fehler auf, falls die Datenbank in einer nicht unterstützten Region bereitgestellt wird.
- Für vorhandene P11- und P15-Datenbanken, die sich in einer der unterstützten Regionen befinden, können Sie den MAXSIZE-Wert auf 4TB erhöhen. Dies kann mithilfe von [SELECT DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) oder durch Überprüfen der Datenbankgröße im Azure-Portal überprüft werden. Die Aktualisierung einer vorhandenen P11- oder P15-Datenbank kann nur mithilfe einer Prinzipalanmeldung auf Serverebene oder von Mitgliedern der Datenbankrolle „dbmanager“ ausgeführt werden. 
- Bei Ausführung eines Upgradevorgangs in einer unterstützten Region wird die Konfiguration umgehend aktualisiert. Die Datenbank bleibt während des Upgradevorgangs online. Allerdings können Sie nicht die vollständigen 4 TB an Speicher nutzen, bis die tatsächlichen Datenbankdateien auf den neuen MAXSIZE-Wert aktualisiert wurden. Die erforderliche Dauer hängt von der Größe der zu aktualisierenden Datenbank ab.  
- Beim Erstellen oder Aktualisieren einer P11- oder P15-Datenbank können Sie nur zwischen einem MAXSIZE-Wert von 1 TB und 4 TB wählen. Zwischengrößen werden zurzeit nicht unterstützt. Beim Erstellen einer P11-/P15-Datenbank ist die Standardspeicheroption von 1 TB vorausgewählt. Für Datenbanken, die sich in einer der unterstützten Regionen befinden, können Sie den Speicher für eine neue oder vorhandene Einzeldatenbank auf maximal 4 TB erhöhen. Für alle anderen Regionen kann die Maximalgröße nicht mehr als 1 TB betragen. Der Preis ändert sich nicht, wenn Sie 4TB Speicher wählen.
- Der MAXSIZE-Wert der 4-TB-Datenbank kann nicht in 1 TB geändert werden, auch wenn der tatsächlich genutzte Speicher unter 1 TB liegt. Daher können Sie eine P11-Datenbank mit 4 TB oder eine P15-Datenbank mit 4 TB nicht auf eine P11-Datenbank mit 1 TB, eine P15-Datenbank mit 1 TB oder eine niedrigere Leistungsstufe (z.B. P1–P6) herunterstufen, bis zusätzliche Speicheroptionen für die restlichen Leistungsstufen bereitgestellt wurden. Diese Einschränkung gilt auch für Wiederherstellungs- und Kopierszenarien, einschließlich Point-in-Time- und Geowiederherstellung, langfristige Aufbewahrung von Sicherungen und Datenbankkopiervorgänge. Sobald eine Datenbank mit der 4-TB-Option konfiguriert wird, muss für Wiederherstellungsvorgänge dieser Datenbank ein P11-/P15-Ziel mit einem MAXSIZE-Wert von 4 TB ausgeführt werden.
- Beim Erstellen oder Aktualisieren einer P11-/P15-Datenbank in einer nicht unterstützten Region, tritt beim Erstellen oder Aktualisieren ein Fehler mit der folgenden Fehlermeldung auf: **P11 and P15 database with up to 4TB of storage are available in US East2, West US, US Gov Virginia, West Europe, Germany Central, South East Asia, Japan East, Australia East, Canada Central, and Canada East.** (P11- und P15-Datenbanken mit einem Speicher von bis zu 4 TB sind in den folgenden Regionen verfügbar: USA, Osten 2; USA, Westen, USA Reg-Bez. Virginia; Europa, Westen; Deutschland, Mitte; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten.)
- Szenarien für aktive Georeplikation:
   - Einrichten einer Georeplikationsbeziehung: Falls es sich bei der primären Datenbank um eine P11- oder P15-Datenbank handelt, müssen auch die sekundären Datenbanken vom Typ „P11“ oder „P15“ sein. Niedrigere Leistungsstufen werden als sekundäre Datenbanken abgelehnt, da sie 4 TB nicht unterstützen können.
   - Aktualisieren der primären Datenbank in einer Georeplikationsbeziehung: Die Änderung des MAXSIZE-Werts für eine primäre Datenbank in 4 TB löst die gleiche Änderung für die sekundäre Datenbank aus. Beide Upgrades müssen erfolgreich ausgeführt werden, damit die Änderung für die primäre Datenbank wirksam wird. Für die Option mit 4 TB gelten Regionseinschränkungen (siehe oben). Wenn sich die sekundäre Datenbank in einer Region befindet, die 4 TB nicht unterstützt, wird die primäre Datenbank nicht aktualisiert.
- Die Verwendung des Import/Export-Diensts zum Laden von Datenbanken des Typs „P11-4TB“ oder „P15-4TB“ wird nicht unterstützt. Verwenden Sie „SqlPackage.exe“, um Daten zu [importieren](sql-database-import.md) und [exportieren](sql-database-export.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-the-azure-portal"></a>Verwalten der Dienst- und Leistungsebenen von Einzeldatenbanken mit dem Azure-Portal

Gehen Sie wie folgt vor, um die Dienstebene, Leistungsebene oder Speichermenge für eine neue oder vorhandene Azure SQL-Datenbank mit dem Azure-Portal festzulegen oder zu ändern: Öffnen Sie das Fenster **Leistung konfigurieren** für Ihre Datenbank, indem Sie wie im folgenden Screenshot gezeigt auf **Tarif (DTUs skalieren)** klicken. 

- Legen Sie die Dienstebene fest bzw. ändern Sie sie, indem Sie die Dienstebene für Ihre Workload auswählen. 
- Legen Sie die Leistungsebene (**DTUs**) in einer Dienstebene mit dem Schieberegler **DTU** fest bzw. ändern Sie sie.
- Legen Sie die Speichermenge für die Leistungsebene mit dem Schieberegler **Speicher** fest bzw. ändern Sie sie. 

  ![Konfigurieren von Dienst- und Leistungsebene](./media/sql-database-service-tiers/service-tier-performance-level.png)

> [!IMPORTANT]
> Wenn Sie eine P11- oder P15-Dienstebene auswählen, helfen Ihnen die Informationen unter [Aktuelle Einschränkungen von P11- und P15-Datenbanken mit einem MAXSIZE-Wert von 4 TB](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) weiter.
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-powershell"></a>Verwalten der Dienst- und Leistungsebenen von Einzeldatenbanken mit PowerShell

Verwenden Sie die folgenden PowerShell-Cmdlets, um Dienstebenen, Leistungsebenen und Speichermengen für Azure SQL-Datenbank mit PowerShell festzulegen und zu ändern. Wenn Sie PowerShell installieren oder aktualisieren müssen, helfen Ihnen die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) weiter. 

| Cmdlet | Beschreibung |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Erstellt eine Datenbank |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Ruft mindestens eine Datenbank ab|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Legt Eigenschaften für eine Datenbank fest oder verschiebt eine vorhandene Datenbank in einen Pool für elastische Datenbanken|


> [!TIP]
> Ein PowerShell-Beispielskript, mit dem die Leistungsmetriken einer Datenbank überwacht werden, die Skalierung auf eine höhere Leistungsebene durchgeführt wird und eine Warnregel für eine der Leistungsmetriken erstellt wird, finden Sie unter [Überwachen und Skalieren einer einzelnen SQL-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-the-azure-cli"></a>Verwalten der Dienst- und Leistungsebenen von Einzeldatenbanken mit der Azure CLI

Verwenden Sie die folgenden Befehle der [Azure CLI-SQL-Datenbank](/cli/azure/sql/db), um Dienstebenen, Leistungsebenen und die Speichermenge von Azure SQL-Datenbanken mit der Azure CLI festzulegen oder zu ändern. Führen Sie die CLI mithilfe von [Cloud Shell](/azure/cloud-shell/overview) in Ihrem Browser aus, oder [installieren](/cli/azure/install-azure-cli) Sie sie unter macOS, Linux oder Windows. Informationen zum Erstellen und Verwalten von Pools für elastische SQL-Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

| Cmdlet | Beschreibung |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#create) |Erstellt eine Datenbank|
|[az sql db list](/cli/azure/sql/db#list)|Listet alle Datenbanken und Data Warehouses eines Servers oder alle Datenbanken eines Pools für elastische Datenbanken auf|
|[az sql db list-editions](/cli/azure/sql/db#list-editions)|Listet verfügbare Dienstziele und Speicherlimits auf|
|[az sql db list-usages](/cli/azure/sql/db#list-usages)|Gibt Informationen zur Datenbankverwendung zurück|
|[az sql db show](/cli/azure/sql/db#show)|Ruft eine Datenbank oder ein Data Warehouse ab|
|[az sql db update](/cli/azure/sql/db#update)|Aktualisiert eine Datenbank|

> [!TIP]
> Ein Azure CLI-Beispielskript, mit dem eine Azure SQL-Einzeldatenbank auf eine andere Leistungsebene skaliert wird, nachdem die Größeninformationen der Datenbank abgerufen wurden, finden Sie unter [Überwachen und Skalieren einer einzelnen SQL-Datenbank mit der Azure CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-transact-sql"></a>Verwalten der Dienst- und Leistungsebenen von Einzeldatenbanken per Transact-SQL

Verwenden Sie die folgenden T-SQL-Befehle, um Dienstebenen, Leistungsebenen und Speichermengen für Azure SQL-Datenbanken mit Transact-SQL festzulegen und zu ändern. Sie können diese Befehle mit dem Azure-Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) oder einem beliebigen anderen Programm ausführen, mit dem eine Verbindung mit einem Azure SQL-Datenbankserver hergestellt und Transact-SQL-Befehle übergeben werden können. 

| Befehl | Beschreibung |
| --- | --- |
|[CREATE DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/create-database-azure-sql-database)|Erstellt eine neue Datenbank. Sie müssen über eine Verbindung mit der Masterdatenbank verfügen, um eine neue Datenbank erstellen zu können.|
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Ändert eine Azure SQL-Datenbank. |
|[sys.database_service_objectives (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Gibt die Edition (Dienstebene), das Dienstziel (Tarif) und den Namen des Pools für elastische Datenbanken, falls vorhanden, für eine Azure SQL-Datenbank oder ein Azure SQL Data Warehouse zurück. Wenn eine Anmeldung an der Masterdatenbank in einem Azure SQL-Datenbankserver besteht, werden Informationen zu allen Datenbanken zurückgegeben. Für Azure SQL Data Warehouse müssen Sie über eine Verbindung mit der Masterdatenbank verfügen.|
|[sys.database_usage (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Listet Anzahl, Typ und Dauer von Datenbanken auf einem Azure SQL-Datenbankserver auf.|

Das folgende Beispiel zeigt den MAXSIZE-Wert der mit dem Befehl ALTER DATABASE geändert wird:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-databases-using-the-rest-api"></a>Verwalten von Einzeldatenbanken mit der REST-API

Informationen zum Festlegen oder Ändern der Dienstebenen, Leistungsebenen und Speichermengen von Azure SQL-Datenbanken finden Sie unter [Azure SQL Database REST API](/rest/api/sql/) (Azure SQL-Datenbank – REST-API).

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [DTUs](sql-database-what-is-a-dtu.md).
* Informationen zur Überwachung der DTU-Nutzung finden Sie unter [Tipps zur Optimierung der SQL-Datenbankleistung](sql-database-troubleshoot-performance.md).


