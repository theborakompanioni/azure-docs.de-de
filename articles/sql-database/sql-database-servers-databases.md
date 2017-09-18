---
title: Erstellen und Verwalten von Azure SQL-Servern und -Datenbanken | Microsoft-Dokumentation
description: Erfahren Sie mehr zu Azure SQL-Datenbankservern und Datenbankkonzepten sowie zum Erstellen und Verwalten von Servern und Datenbanken mithilfe des Azure-Portals, PowerShell, der Azure CLI, Transact-SQL und der REST-API.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: 4585be186554d5d7aa9bc79029b6f8506b278701
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---

# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Erstellen und Verwalten von Azure SQL-Datenbankservern und -datenbanken

Eine Azure SQL-Datenbank ist eine verwaltete Datenbank in Microsoft Azure, die innerhalb einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) mit einem definierten Satz von [Compute- und Speicherressourcen für verschiedene Workloads](sql-database-service-tiers.md) erstellt wird. Eine Azure SQL-Datenbank ist einem logischen Azure SQL-Datenbankserver zugeordnet, der innerhalb einer bestimmten Azure-Region erstellt wird. 

## <a name="an-azure-sql-database-can-be-a-single-pooled-or-partitioned-database"></a>Eine Azure SQL-Datenbank kann eine einzelne, zusammengelegte oder partitionierte Datenbank sein.

Eine Azure SQL-Datenbank kann Folgendes sein:

- Eine [einzelne Datenbank](sql-database-single-database-resources.md) mit ihrem eigenen Ressourcensatz
- Teil eines [elastischen Pools](sql-database-elastic-pool.md) mit gemeinsamer Nutzung eines Ressourcensatzes
- Teil eines [skalierten Satzes von Sharddatenbanken](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), die entweder einfache oder in einem Pool zusammengefasste Datenbanken sein können
- Teil von Datenbanksätzen, die an einem [mehrinstanzfähigen SaaS-Entwurfsmuster](sql-database-design-patterns-multi-tenancy-saas-applications.md) teilnehmen, und deren Datenbanken entweder einzelne oder in einem Pool zusammengefasste Datenbanken (oder beides) sein können 

> [!TIP]
> Gültige Datenbanknamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). 
>
 
- Die standardmäßige Datenbanksortierung von Microsoft Azure SQL-Datenbank ist **SQL_LATIN1_GENERAL_CP1_CI_AS**, wobei **LATIN1_GENERAL** für Englisch (USA), **CP1** für Codepage 1252, **CI** für keine Unterscheidung von Groß-/Kleinschreibung und **AS** für die Unterscheidung nach Akzent steht. Weitere Informationen zum Festlegen der Sortierung finden Sie unter [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).
- Microsoft Azure SQL-Datenbank unterstützt den TDS-Client (Tabular Data Stream) ab Version 7.3.
- Nur TCP/IP-Verbindungen sind zulässig.

## <a name="what-is-an-azure-sql-logical-server"></a>Was ist ein logischer Azure SQL-Server?

Ein logischer Server fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken, einschließlich [Pools für elastische Datenbanken](sql-database-elastic-pool.md), [Anmeldungen](sql-database-manage-logins.md), [Firewallregeln](sql-database-firewall-configure.md), [Überwachungsregeln](sql-database-auditing.md), [Richtlinien zur Erkennung von Bedrohungen](sql-database-threat-detection.md) und [Failovergruppen](sql-database-geo-replication-overview.md). Ein logischer Server kann sich in einer anderen Region als seine Ressourcengruppe befinden. Der logische Server muss vorhanden sein, bevor Sie die Azure SQL-Datenbank erstellen können. Alle Datenbanken auf einem Server werden innerhalb der gleichen Region wie der logische Server erstellt. 


> [!IMPORTANT]
> In SQL-Datenbank ist ein Server ein logisches Konstrukt, das von einer SQL Server-Instanz unterschieden wird, mit der Sie möglicherweise in der lokalen Welt vertraut sind. Insbesondere der SQL-Datenbankdienst gibt keine Garantie bezüglich des Speicherorts der Datenbanken in Bezug auf ihre logischen Server, und bietet kein Zugriff oder Funktion auf Instanzebene.
> 

Wenn Sie einen logischen Server erstellen, geben Sie ein Serveranmeldekonto und -kennwort an, mit dem Sie Administratorrechte für die Masterdatenbank auf diesem Server und alle auf diesem Server erstellte Datenbanken haben. Bei diesem anfänglichen Konto handelt es sich um ein SQL-Anmeldekonto. Azure SQL-Datenbank unterstützt für die Authentifizierung die SQL-Authentifizierung und die Azure Active Directory-Authentifizierung. Weitere Informationen zu Anmeldungen und Authentifizierungen finden Sie unter [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](sql-database-manage-logins.md). Windows-Authentifizierung wird nicht unterstützt. 

> [!TIP]
> Gültige Ressourcengruppen- und Servernamen finden Sie unter [Benennungsregeln und Einschränkungen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Ein logischer Azure Datenbankserver:

- wird in einem Azure-Abonnement erstellt, kann aber mit seinen enthaltenen Ressourcen in ein anderes Abonnement verschoben werden
- ist die übergeordnete Ressource für Datenbanken, elastische Pools und Data Warehouses
- bietet einen Namespace für Datenbanken, für Pools für elastische Datenbanken und Data Warehouses
- ist ein logischer Container mit Semantik von hoher Lebensdauer – beim Löschen eines Servers werden die enthaltenen Datenbanken, Pools für elastische Datenbanken und Data Warehouses gelöscht
- beteiligt sich an der [rollenbasierten Zugriffssteuerung (role-based access control, RBAC) in Azure](/active-directory/role-based-access-control-what-is) – Datenbanken, Pools für elastische Datenbanken und Data Warehouses innerhalb eines Servers erben dessen Zugriffsrechte
- ist ein oberes Element der Identität von Datenbanken, Pools für elastische Datenbanken und Data Warehouses für Zwecke der Azure-Ressourcenverwaltung (siehe das URL-Schema für Datenbanken und Pools)
- stellt in einer Region Ressourcen zusammen
- bietet eine Verbindungsendpunkt für den Datenbankzugriff (<serverName>.database.windows.net)
- bietet Zugriff auf Metadaten bezüglich der darin enthaltenen Ressourcen über DMVs (dynamic management views), indem eine Verbindung mit einer Masterdatenbank hergestellt wird 
- stellt den Bereich für Verwaltungsrichtlinien bereit, die für seine Datenbanken gelten – Anmeldungen, Firewall, Überwachung, Bedrohungserkennung usw. 
- ist durch ein Kontingent innerhalb des übergeordneten Abonnements eingeschränkt (standardmäßig sechs Server pro Abonnement – [siehe Grenzwerte für Abonnements hier](../azure-subscription-service-limits.md))
- stellt den Bereich für Datenbank- und DTU-Kontingente für die darin enthaltenen Ressourcen (z.B. 45.000 DTU9) bereit
- ist der Versionsverwaltungsbereich für Funktionen, die in enthaltenen Ressourcen aktiviert wurden 
- Serverebenenprinzipal-Anmeldungen können alle Datenbanken auf einem Server verwalten
- kann Anmeldungen enthalten, die denen in SQL-Instanzen vor Ort ähnlich sind und die über Zugriff auf eine oder mehrere Datenbanken auf dem Server verfügen und denen beschränkte Administratorrechte zugewiesen werden können. Weitere Informationen finden Sie unter [Logins (Anmeldungen)](sql-database-manage-logins.md).

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>Azure SQL-Datenbanken, die von der SQL-Datenbank-Firewall geschützt werden

Zum Schutz Ihrer Daten verhindert eine [SQL-Datenbank-Firewall](sql-database-firewall-configure.md) jeglichen Zugriff auf Ihren Datenbankserver oder eine dessen Datenbanken von außerhalb Ihrer Verbindung zum Server direkt über Ihre Azure-Abonnement-Verbindung. Um zusätzliche Konnektivität zu ermöglichen, müssen Sie [mindestens eine Firewallregel erstellen](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Informationen zum Erstellen und Verwalten von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe des Azure-Portals

Sie können die Ressourcengruppe der Azure SQL-Datenbank im Voraus oder beim Erstellen des Servers selbst erstellen. Es existieren mehrere Methoden zum Erhalten eines Formulars für einen neuen SQL-Server. Sie erhalten ein solches entweder durch das Erstellen eines neuen SQL-Servers oder als Teil der Erstellung einer neuen Datenbank. 

### <a name="create-a-blank-sql-server-logical-server"></a>Erstellen eines leeren SQL-Servers (logischen Servers)

Um mithilfe des [Azure-Portals](https://portal.azure.com) einen Azure SQL-Datenbankserver (ohne eine Datenbank) zu erstellen, navigieren Sie zu einem leeren SQL-Server-Formular (logischer Server).  

### <a name="create-a-blank-or-sample-sql-database"></a>Erstellen einer leeren oder einer Beispiel-SQL-Datenbank

Zum Erstellen einer Azure SQL-Datenbank mithilfe des [Azure-Portals](https://portal.azure.com) navigieren Sie zu einem leeren SQL-Datenbank-Formular, und stellen Sie die erforderlichen Informationen bereit. Sie können die Ressourcengruppe und den logischen Server der Azure SQL-Datenbank im Voraus oder beim Erstellen der Datenbank selbst erstellen. Sie können eine leere Datenbank oder eine Beispieldatenbank basierend auf Adventure Works LT erstellen. 

  ![Datenbankerstellung 1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Informationen zum Auswählen des Tarifs für Ihre Datenbank finden Sie unter [Dienstebenen](sql-database-service-tiers.md).
>

### <a name="manage-an-existing-sql-server"></a>Verwalten eines vorhandenen SQL-Servers

Zum Verwalten eines vorhandenen Servers navigieren Sie mithilfe einer Reihe von Methoden zum Server, z.B. über eine bestimmte Seite der SQL-Datenbank, die Seite der **SQL-Server** oder die Seite **All resources** (Alle Ressourcen). 

Um eine vorhandene Datenbank zu verwalten, navigieren Sie zu der Seite **SQL-Datenbanken**, und klicken Sie auf die Datenbank, die Sie verwalten möchten. Der folgende Screenshot zeigt, wie Sie beginnen, über die Seite **Übersicht** für eine Datenbank eine Firewall auf Serverebene für eine Datenbank festzulegen. 

   ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Informationen zum Konfigurieren von Leistungseigenschaften für eine Datenbank finden Sie unter [Dienstebenen](sql-database-service-tiers.md).
>

> [!TIP]
> Ein Schnellstart-Tutorial zum Azure-Portal finden Sie unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von PowerShell

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von Azure PowerShell die folgenden PowerShell-Cmdlets. Wenn Sie PowerShell installieren oder upgraden müssen, helfen Ihnen die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) weiter. Informationen zum Erstellen und Verwalten von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

| Cmdlet | Beschreibung |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Erstellt eine Datenbank |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Ruft mindestens eine Datenbank ab|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Legt Eigenschaften für eine Datenbank fest oder verschiebt eine vorhandene Datenbank in einen Pool für elastische Datenbanken|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Entfernt eine Datenbank|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Erstellt eine Ressourcengruppe
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Erstellt einen Server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Gibt Informationen zu Servern zurück|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/set-azurermsqlserver)|Ändert die Eigenschaften eines Servers|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Entfernt einen Server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Erstellt eine Firewallregel auf Serverebene |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Ruft Firewallregeln für einen Server ab|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Ändert eine Firewallregel auf einem Server|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Löscht eine Firewallregel von einem Server|

> [!TIP]
> Ein Schnellstart-Tutorial zu PowerShell finden Sie unter [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-portal.md). PowerShell-Beispielskripts finden Sie unter [Erstellen einer einzelnen Azure SQL-­Datenbank und Konfigurieren einer Firewallregel mithilfe von PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) und [Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe der Azure CLI

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe der [Azure CLI](/cli/azure/overview) die folgenden [Azure CLI-SQL-Datenbank](/cli/azure/sql/db)-Befehle. Führen Sie die CLI mithilfe von [Cloud Shell](/azure/cloud-shell/overview) in Ihrem Browser aus, oder [installieren](/cli/azure/install-azure-cli) Sie sie unter macOS, Linux oder Windows. Informationen zum Erstellen und Verwalten von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

| Cmdlet | Beschreibung |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#create) |Erstellt eine Datenbank|
|[az sql db list](/cli/azure/sql/db#list)|Listet alle Datenbanken und Data Warehouses eines Servers oder alle Datenbanken eines Pools für elastische Datenbanken auf|
|[az sql db list-editions](/cli/azure/sql/db#list-editions)|Listet verfügbare Dienstziele und Speicherlimits auf|
|[az sql db list-usages](/cli/azure/sql/db#list-usages)|Gibt Informationen zur Datenbankverwendung zurück|
|[az sql db show](/cli/azure/sql/db#show)|Ruft eine Datenbank oder ein Data Warehouse ab|
|[az sql db update](/cli/azure/sql/db#update)|Aktualisiert eine Datenbank|
|[az sql db delete](/cli/azure/sql/db#delete)|Entfernt eine Datenbank|
|[az group create](/cli/azure/group#create)|Erstellt eine Ressourcengruppe|
|[az sql server create](/cli/azure/sql/server#create)|Erstellt einen Server|
|[az sql server list](/cli/azure/sql/server#list)|Listet Server auf|
|[az sql server list-usages](/cli/azure/sql/server#list-usages)|Gibt Informationen zur Server-Verwendung zurück|
|[az sql server show](/cli/azure/sql/server#show)|Ruft einen Server ab|
|[az sql server update](/cli/azure/sql/server#update)|Aktualisiert einen Server|
|[az sql server delete](/cli/azure/sql/server#delete)|Löscht einen Server|
|[az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule#create)|Erstellt eine Serverfirewallregel|
|[az sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#list)|Listet die Firewallregeln auf einem Server auf|
|[az sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#show)|Zeigt die Details einer Firewallregel an|
|[az sql server firewall-rule update](/cli/azure/sql/server/firewall-rule#update)|Aktualisiert eine Firewallregel|
|[az sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#delete)|Löscht eine Firewallregel|

> [!TIP]
> Ein Schnellstart-Tutorial zur Azure CLI finden Sie unter [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe der Azure CLI](sql-database-get-started-cli.md). Azure CLI-Beispielskripts finden Sie unter [Erstellen einer einzelnen SQL-­Datenbank und Konfigurieren einer Firewallregel mit der Azure CLI](scripts/sql-database-create-and-configure-database-cli.md) und [Überwachen und Skalieren einer einzelnen SQL-­Datenbank mithilfe der CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von Transact-SQL

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von Transact-SQL die folgenden T-SQL-Befehle. Sie können diese Befehle mit dem Azure-Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) oder einem beliebigen anderen Programm ausführen, mit dem eine Verbindung mit einem Azure SQL-Datenbankserver hergestellt und Transact-SQL-Befehle übergeben werden können. Informationen zum Verwalten von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Sie können einen Server mithilfe von Transact-SQL nicht erstellen oder löschen.
>

| Befehl | Beschreibung |
| --- | --- |
|[CREATE DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/create-database-azure-sql-database)|Erstellt eine neue Datenbank. Sie müssen über eine Verbindung mit der Masterdatenbank verfügen, um eine neue Datenbank erstellen zu können.|
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Ändert eine Azure SQL-Datenbank. |
|[ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Ändert ein Azure SQL Data Warehouse.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Löscht eine Datenbank.|
|[sys.database_service_objectives (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Gibt die Edition (Dienstebene), das Dienstziel (Tarif) und den Namen des Pools für elastische Datenbanken, falls vorhanden, für eine Azure SQL-Datenbank oder ein Azure SQL Data Warehouse zurück. Wenn eine Anmeldung an der Masterdatenbank in einem Azure SQL-Datenbankserver besteht, werden Informationen zu allen Datenbanken zurückgegeben. Für Azure SQL Data Warehouse müssen Sie über eine Verbindung mit der Masterdatenbank verfügen.|
|[sys.dm_db_resource_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Gibt die CPU-, E/A- und Arbeitsspeichernutzung für eine Azure SQL-Datenbank zurück. Für alle 15 Sekunden ist eine Zeile vorhanden, selbst wenn keine Aktivität in der Datenbank erfolgt ist.|
|[sys.resource_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Gibt CPU-Nutzungs- und Speicherdaten für eine Azure SQL-Datenbank zurück. Die Daten werden in Intervallen von fünf Minuten gesammelt und aggregiert.|
|[sys.database_connection_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Enthält Statistiken zu Verbindungsereignissen für SQL-Datenbank und eine Übersicht über erfolgreiche und nicht erfolgreiche Datenbankverbindungen. |
|[sys.event_log (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Gibt erfolgreiche Datenbankverbindungen, Verbindungsfehler und Deadlocks für Azure SQL-Datenbank zurück. Sie können diese Informationen nutzen, um die Datenbankaktivität mit SQL-Datenbank nachzuverfolgen oder um Probleme zu beheben.|
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Erstellt oder aktualisiert die Firewalleinstellungen auf Serverebene für Ihre SQL-Datenbankserver. Diese gespeicherte Prozedur ist nur in der Masterdatenbank für die Prinzipalanmeldung auf Serverebene verfügbar. Eine Firewallregel auf Serverebene kann erst mithilfe von Transact-SQL erstellt werden, nachdem die erste Firewallregel auf Serverebene von einem Benutzer mit Azure-Berechtigungen erstellt wurde.|
|[sys.firewall_rules (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Gibt Informationen zu den Firewalleinstellungen auf Serverebene im Zusammenhang mit Ihrer Microsoft Azure SQL-Datenbank zurück.|
|[sp_delete_firewall_rule (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Entfernt Firewalleinstellungen auf Serverebene von Ihrem SQL-Datenbankserver. Diese gespeicherte Prozedur ist nur in der Masterdatenbank für die Prinzipalanmeldung auf Serverebene verfügbar.|
|[sp_set_database_firewall_rule (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Erstellt oder aktualisiert die Firewallregeln auf Datenbankebene für Ihre Azure SQL-Datenbank oder SQL Data Warehouse. Datenbank-Firewallregeln können für die Masterdatenbank und für Benutzerdatenbanken in SQL-Datenbank konfiguriert werden. Datenbank-Firewallregeln sind bei der Verwendung von Benutzern eigenständiger Datenbanken nützlich. |
|[sys.database_firewall_rules (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Gibt Informationen zu den Firewalleinstellungen auf Datenbankebene im Zusammenhang mit Ihrer Microsoft Azure SQL-Datenbank zurück. |
|[sp_delete_database_firewall_rule (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Entfernt die Firewalleinstellung auf Datenbankebene von Ihrer Azure SQL-Datenbank oder SQL Data Warehouse. |


> [!TIP]
> Ein Schnellstart-Tutorial mit SQL Server Management Studio unter Microsoft Windows finden Sie unter [Azure SQL-Datenbank: Verwenden von SQL Server Management Studio zum Herstellen der Verbindung und Abfragen von Daten](sql-database-connect-query-ssms.md). Ein Schnellstart-Tutorial mit Visual Studio Code unter macOS, Linux oder Windows finden Sie unter [Azure SQL-Datenbank: Verwenden von Visual Studio Code zum Herstellen einer Verbindung mit und Abfragen von Daten](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe der REST-API

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls diese REST-API-Anforderungen.

| Befehl | Beschreibung |
| --- | --- |
|[Servers - Create Or Update](/rest/api/sql/servers/createorupdate)|Erstellt oder aktualisiert einen neuen Server.|
|[Servers - Delete](/rest/api/sql/servers/delete)|Löscht eine SQL Server-Instanz.|
|[Servers - Get](/rest/api/sql/servers/get)|Ruft einen Server ab.|
|[Servers - List](/rest/api/sql/servers/list)|Gibt eine Serverliste zurück.|
|[Servers - List By Resource Group](/rest/api/sql/servers/listbyresourcegroup)|Gibt eine Liste aller Server in einer Ressourcengruppe zurück.|
|[Server - Update](/rest/api/sql/servers/update)|Aktualisiert einen vorhandenen Server.|
|[Servers - Sql](/rest/api/sql/servers%20-%20sql)|Bestimmt, ob eine Ressource mit dem angegebenen Namen erstellt werden kann.|
|[Databases - Create Or Update](/rest/api/sql/databases/createorupdate)|Erstellt eine neue Datenbank oder aktualisiert eine bereits vorhandene Datenbank|
|[Datenbanken – Abrufen](/rest/api/sql/databases/get)|Ruft eine Datenbank ab|
|[Datenbanken – Abrufen nach Pool für elastische Datenbanken](/rest/api/sql/databases/getbyelasticpool)|Ruft eine Datenbank in einem Pool für elastische Datenbanken ab|
|[Datenbanken – Abrufen nach empfohlenem Pool für elastische Datenbanken](/rest/api/sql/databases/getbyrecommendedelasticpool)|Ruft eine Datenbank in einem empfohlenen Pool für elastische Datenbanken ab|
|[Datenbanken – Auflisten nach Pool für elastische Datenbanken](/rest/api/sql/databases/listbyelasticpool)|Gibt eine Liste der Datenbanken in einem elastischen Pool zurück.|
|[Datenbanken – Auflisten nach empfohlenem Pool für elastische Datenbanken](/rest/api/sql/databases/listbyrecommendedelasticpool)|Gibt eine Liste von Datenbanken in einem empfohlenen Pool für elastische Datenbanken zurück|
|[Datenbanken – Auflisten nach Server](/rest/api/sql/databases/listbyserver)|Gibt eine Liste der Datenbanken auf einem Server zurück|
|[Datenbanken – Aktualisieren](/api/sql/databases/update)|Aktualisiert eine vorhandene Datenbank.|
|[Firewall Rules - Create Or Update](/rest/api/sql/firewallrules/createorupdate)|Erstellt oder aktualisiert eine Firewallregel.|
|[Firewall Rules - Delete](/rest/api/sql/firewallrules/delete)|Löscht eine Firewallregel.|
|[Firewall Rules - Get](/rest/api/sql/firewallrules/get)|Ruft eine Firewallregel ab.|
|[Firewall Rules - List By Server](/rest/api/sql/firewallrules/listbyserver)|Gibt eine Liste von Firewallregeln zurück.|

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Zusammenlegen von Datenbanken mithilfe von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).
- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL-Datenbank?](sql-database-technical-overview.md).
- Weitere Informationen zum Migrieren einer SQL Server-Datenbank zu Azure finden Sie unter [Migrieren zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).
- Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md).

