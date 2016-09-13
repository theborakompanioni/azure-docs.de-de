<properties
	pageTitle="Tutorial zu SQL-Datenbank: Erstellen einer Datenbank | Microsoft Azure"
	description="Hier erhalten Sie Informationen zum Einrichten von logischen SQL-Datenbankservern, Serverfirewallregeln, SQL-Datenbanken und Beispieldaten. Darüber hinaus erfahren Sie, wie Sie eine Verbindung mit Clienttools herstellen, Benutzer konfigurieren und eine Datenbankfirewallregel einrichten."
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

- Erstellen eines logischen Azure SQL-Datenbankservers zum Hosten von SQL-Datenbanken
- Erstellen einer SQL-Datenbank ohne Daten, mit Beispieldaten oder mit Daten aus einer SQL-Datenbanksicherung
- Erstellen einer Firewallregel auf Serverebene für eine einzelne IP-Adresse oder für einen IP-Adressbereich

Die gleichen Aufgaben können Sie mit [C#](sql-database-get-started-csharp.md) oder [PowerShell](sql-database-get-started-powershell.md) durchführen.

[AZURE.INCLUDE [Anmeldung](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

[AZURE.INCLUDE [Erstellen eines logischen SQL-Datenbankservers](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Nächste Schritte
Nachdem Sie dieses Tutorial zu SQL-Datenbank ausgeführt und eine Datenbank mit Beispieldaten erstellt haben, können Sie nun das Experimentieren mit Ihren Lieblingstools beginnen.

- Wenn Sie mit Transact-SQL und SQL Server Management Studio (SSMS) vertraut sind, informieren Sie sich über das [Herstellen von Verbindungen mit einer SQL-Datenbank und das Abfragen dieser mit SSMS](sql-database-connect-query-ssms.md).

- Wenn Sie mit Excel vertraut sind, informieren Sie sich über das [Herstellen von Verbindungen mit SQL-Datenbank in Azure mit Excel](sql-database-connect-excel.md).

- Wenn Sie mit dem Programmieren beginnen möchten, wählen Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md) Ihre Programmiersprache aus.

- Wenn Sie lokale SQL Server-Datenbanken in Azure verschieben möchten, finden Sie unter [Migrieren von Datenbanken zu SQL-Datenbank](sql-database-cloud-migrate.md) weitere Informationen.

- Wenn Sie einige Daten aus einer CSV-Datei mit dem BCP-Befehlszeilentool in eine neue Tabelle laden möchten, machen Sie sich mit den Informationen unter [Laden von Daten aus einer CSV-Datei in SQL-Datenbank mit BPC](sql-database-load-from-csv-with-bcp.md) vertraut.


## Zusätzliche Ressourcen

[Was ist SQL Database?](sql-database-technical-overview.md)

<!---HONumber=AcomDC_0907_2016-->