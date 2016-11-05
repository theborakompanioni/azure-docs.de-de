---
title: 'SQL-Datenbank-Tutorial: Erste Schritte für die Sicherheit'
description: Erfahren Sie, wie Sie Benutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken erstellen.
keywords: ''
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: ''

ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: carlrab

---
# SQL Datenbank-Tutorial: Erstellen von SQL-Datenbankbenutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken
> [!div class="op_single_selector"]
> * [Lernprogramm zu den ersten Schritten](sql-database-get-started-security.md)
> * [Gewähren von Zugriff](sql-database-manage-logins.md)
> 
> 

In diesem Tutorial erfahren Sie, wie Sie SQL Server Management Studio (SSMS) für folgende Zwecke verwenden:

* Anmelden bei einer SQL-Datenbank mithilfe einer Prinzipalanmeldung auf Serverebene
* Erstellen eines Benutzerkontos für eine SQL-Datenbank
* Erteilen von [„db\_owner“-Berechtigungen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0) für den Benutzer einer SQL-Datenbank
* Herstellen einer Verbindung mit einer SQL-Datenbank mit einem Benutzerkonto, bei dem es sich nicht um einen Prinzipal auf Serverebene handelt

[!INCLUDE [Anmeldung](../../includes/azure-getting-started-portal-login.md)]

[!INCLUDE [Erstellen eines logischen SQL-Datenbankservers](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

[!INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-database-user.md)]

[!INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-grant-database-user-dbo-permissions.md)]

[!INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-sql-server-management-studio-connect-user.md)]

## Nächste Schritte
Nachdem Sie dieses Tutorial zur SQL-Datenbank ausgeführt, ein Benutzerkonto erstellt und dem Benutzerkonto DBO-Berechtigungen erteilt haben, können Sie sich weiter über die [Sicherheit von SQL-Datenbanken](sql-database-manage-logins.md) informieren.

<!---HONumber=AcomDC_0824_2016-->