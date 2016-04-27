<properties
	pageTitle="Verbinden mit der SQL-Datenbank – SQL Server Management Studio | Microsoft Azure"
	description="Erfahren Sie, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer SQL-Datenbank in Azure herstellen. Führen Sie dann eine Beispielabfrage mithilfe von Transact-SQL (T-SQL) aus."
	metaCanonical=""
	keywords="Verbinden mit SQL-Datenbank, SQL Server Management Studio"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor="" />

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="03/25/2016"
	ms.author="sstein;carlrab" />

# Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage

> [AZURE.SELECTOR]
- [C#](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

In diesem Artikel werden das Herstellen einer Verbindung mit einer Azure SQL-Datenbank mithilfe der neuesten Version von SQL Server Management Studio (SSMS) und das anschließende Durchführen einer einfachen Abfrage mithilfe von Transact-SQL-Anweisungen (T-SQL) veranschaulicht.

[AZURE.INCLUDE [Anmelden](../../includes/azure-getting-started-portal-login.md)]

[AZURE.INCLUDE [SSMS-Installation](../../includes/sql-server-management-studio-install.md)]

[AZURE.INCLUDE [SSMS-Verbindung](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

Informationen zur Firewallregeln finden Sie unter [Konfigurieren der Firewalleinstellungen für SQL-Datenbank mit dem Azure-Portal](sql-database-configure-firewall-settings.md).

## Durchführen von Beispielabfragen

Nachdem Sie eine Verbindung mit Ihrem logischen Server hergestellt haben, können Sie eine Verbindung mit einer Datenbank herstellen und eine Beispielabfrage ausführen.

1. Navigieren Sie im **Objekt-Explorer** zu einer Datenbank auf dem Server, für die Sie Berechtigungen besitzen, etwa die Beispieldatenbank **AdventureWorks**.
2. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie dann **Neue Abfrage** aus.

	![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. Kopieren Sie den folgenden Code, und fügen Sie ihn im Abfragefenster ein.

		SELECT
		CustomerId
		,Title
		,FirstName
		,LastName
		,CompanyName
		FROM SalesLT.Customer;

4. Klicken Sie auf die Schaltfläche **Ausführen**. Der folgende Screenshot zeigt eine erfolgreiche Abfrage.

	![Erfolgreich Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Nächste Schritte

Mit T-SQL-Anweisungen können Sie Datenbanken in Azure auf gleiche Weise wie mit SQL Server erstellen und verwalten. Wenn Sie mit der Verwendung von T-SQL mit SQL Server vertraut sind, finden Sie unter [Azure SQL-Datenbank – Transact-SQL-Informationen](sql-database-transact-sql-information.md) eine Zusammenfassung der Unterschiede.

Wenn Sie mit T-SQL nicht vertraut sind, siehe [Tutorial: Schreiben von Transact-SQL-Anweisungen](https://msdn.microsoft.com/library/ms365303.aspx) und [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx).

Informationen zu den ersten Schritten zum Erstellen von Datenbankbenutzern und Datenbankbenutzeradministratoren finden Sie unter [Get Started with Azure SQL Database security](sql-database-get-started-security.md) (Erste Schritte bei der Sicherheit für Azure SQL-Datenbanken).

<!---HONumber=AcomDC_0420_2016-->