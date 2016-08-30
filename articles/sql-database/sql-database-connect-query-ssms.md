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
	ms.date="08/17/2016"
	ms.author="sstein;carlrab" />

# Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

In diesem Artikel wird beschrieben, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer Azure SQL-Datenbank herstellen. Nach der erfolgreichen Verbindungsherstellung führen wir eine einfache Transact-SQL-Abfrage (T-SQL) aus, um die Kommunikation mit der Datenbank zu überprüfen.

[AZURE.INCLUDE [SSMS-Installation](../../includes/sql-server-management-studio-install.md)]

[AZURE.INCLUDE [SSMS-Verbindung](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


## Durchführen von Beispielabfragen

Nachdem Sie eine Verbindung mit Ihrem Server hergestellt haben, können Sie eine Verbindung mit einer Datenbank herstellen und eine Beispielabfrage ausführen. Wenn Sie mit dem Schreiben von Abfragen noch nicht vertraut sind, helfen Ihnen die Informationen unter [Schreiben von Transact-SQL-Anweisungen](https://msdn.microsoft.com/library/ms365303.aspx) weiter.

1. Navigieren Sie im **Objekt-Explorer** zu einer Datenbank auf dem Server, z.B. der Beispieldatenbank **AdventureWorks**.
2. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie dann **Neue Abfrage**:

	![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. Fügen Sie im Abfragefenster den folgenden Code ein:

		SELECT
		CustomerId
		,Title
		,FirstName
		,LastName
		,CompanyName
		FROM SalesLT.Customer;

4. Klicken Sie auf die Schaltfläche **Ausführen**.

	![Erfolgreich. Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Nächste Schritte

Mit T-SQL-Anweisungen können Sie Datenbanken in Azure auf gleiche Weise wie mit SQL Server erstellen und verwalten. Wenn Sie mit der Verwendung von T-SQL mit SQL Server vertraut sind, finden Sie unter [Azure SQL-Datenbank – Transact-SQL-Informationen](sql-database-transact-sql-information.md) eine Zusammenfassung der Unterschiede.

Wenn Sie mit T-SQL nicht vertraut sind, siehe [Tutorial: Schreiben von Transact-SQL-Anweisungen](https://msdn.microsoft.com/library/ms365303.aspx) und [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx).

Informationen zu den ersten Schritten zum Erstellen von Datenbankbenutzern und Datenbankbenutzeradministratoren finden Sie unter [SQL Datenbank-Tutorial: Erstellen von SQL-Datenbankbenutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken über das Azure-Portal](sql-database-get-started-security.md).

Weitere Informationen zu SSMS finden Sie unter [Verwenden Sie SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).

<!---HONumber=AcomDC_0824_2016-->