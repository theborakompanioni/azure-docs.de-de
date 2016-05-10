<properties
	pageTitle="SQL-Datenbank-Tutorial: Erste Schritte für die Sicherheit"
	description="Erfahren Sie, wie Sie Benutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken erstellen."
	keywords=""
	services="sql-database"
	documentationCenter=""
	authors="carlrabeler"
	manager="jhubbard"
	editor=""/>


<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="05/03/2016"
	ms.author="carlrab"/>

# SQL Datenbank-Tutorial: Erstellen von SQL-Datenbankbenutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken über das Azure-Portal

In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal für Folgendes verwenden:

- Anmelden bei einer SQL-Datenbank mithilfe einer Prinzipalanmeldung auf Serverebene
- Erstellen eines Benutzerkontos für eine SQL-Datenbank
- Erteilen von DBO-Berechtigungen für ein Benutzerkonto einer SQL-Datenbank in einer Benutzerdatenbank
- Herstellen einer Verbindung mit einer SQL-Datenbank mit einem Benutzerkonto, bei dem es sich nicht um einen Prinzipal auf Serverebene handelt 

[AZURE.INCLUDE [Anmeldung](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Erstellen eines logischen SQL-Datenbankservers](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## Nächste Schritte
Nachdem Sie dieses Tutorial zur SQL-Datenbank ausgeführt, ein Benutzerkonto erstellt und dem Benutzerkonto DBO-Berechtigungen erteilt haben, lesen Sie mehr über die [Sicherheit von SQL-Datenbanken](sql-database-manage-logins.md).

<!---HONumber=AcomDC_0504_2016-->