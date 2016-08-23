<properties
	pageTitle="Tutorial zu SQL-Datenbank: Erstellen einer Datenbank | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie einen logischen SQL-Datenbankserver, eine Firewallregel auf Serverebene, eine SQL-Datenbank und Beispieldaten einrichten, eine Verbindung mit Clienttools herstellen sowie Benutzer und eine Firewallregel auf Datenbankebene konfigurieren."
	keywords="Tutorial zu SQL-Datenbank, Erstellen einer SQL-Datenbank"
	services="sql-database"
	documentationCenter=""
	authors="CarlRabeler"
	manager="jhubbard"
	editor=""/>


<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="07/05/2016"
	ms.author="carlrab"/>

# SQL-Datenbank-Tutorial: Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal

**Einzeldatenbank**

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal für Folgendes verwenden:

- Erstellen eines logischen SQL-Datenbankservers zum Hosten von SQL-Datenbanken
- Erstellen einer SQL-Datenbank ohne Daten, mit Beispieldaten oder mit Daten aus einer SQL-Datenbanksicherung
- Erstellen einer Firewallregel auf Serverebene für eine einzelne IP-Adresse oder für einen IP-Adressbereich

Verwenden Sie diese Links, um die gleichen Aufgaben mit [C#](sql-database-get-started-csharp.md) oder mit [PowerShell](sql-database-get-started-powershell.md) durchzuführen.

[AZURE.INCLUDE [Anmeldung](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

[AZURE.INCLUDE [Erstellen eines logischen SQL-Datenbankservers](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Nächste Schritte
Nachdem Sie dieses Tutorial zu SQL-Datenbank ausgeführt und eine Datenbank mit Beispieldaten erstellt haben, können Sie nun das Experimentieren mit Ihren Lieblingstools beginnen.

- Wenn Sie mit Transact-SQL und SQL Server Management Studio vertraut sind, informieren Sie sich über das [Herstellen von Verbindungen mit einer SQL-Datenbank und das Abfragen dieser mit SSMS](sql-database-connect-query-ssms.md).

- Wenn Sie mit Excel vertraut sind, informieren Sie sich über das [Herstellen von Verbindungen mit SQL-Datenbank mit Excel](sql-database-connect-excel.md).

- Wenn Sie mit dem Programmieren beginnen möchten, wählen Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md) Ihre Programmiersprache aus.

- Wenn Sie lokale SQL Server-Datenbanken in Azure verschieben möchten, finden Sie unter [Migrieren von Datenbanken zu Azure SQL-Datenbank](sql-database-cloud-migrate.md) weitere Informationen.

- Wenn Sie einige Daten aus einer CSV-Datei mit BCP in eine neue Tabelle laden möchten, machen Sie sich mit den Informationen unter [Laden von Daten aus einer CSV-Datei in Azure SQL Data Warehouse (Flatfiles)](sql-database-load-from-csv-with-bcp.md) vertraut.


## Zusätzliche Ressourcen

[Was ist SQL Database?](sql-database-technical-overview.md)

<!---HONumber=AcomDC_0817_2016-->