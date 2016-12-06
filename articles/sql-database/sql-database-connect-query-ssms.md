---
title: "Verbinden mit der SQL-Datenbank – SQL Server Management Studio | Microsoft Docs"
description: "Erfahren Sie, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer SQL-Datenbank in Azure herstellen. Führen Sie dann eine Beispielabfrage mithilfe von Transact-SQL (T-SQL) aus."
metacanonical: 
keywords: Verbinden mit SQL-Datenbank, SQL Server Management Studio
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0eb25eb76c6c6c2446ac0b2b07c65975c3719db0


---
# <a name="connect-to-sql-database-with-sql-server-management-studio-and-execute-a-sample-tsql-query"></a>Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage
> [!div class="op_single_selector"]
> * [Visual Studio](sql-database-connect-query.md)
> * [SSMS](sql-database-connect-query-ssms.md)
> * [Excel](sql-database-connect-excel.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer Azure SQL-Datenbank herstellen. Nach der erfolgreichen Verbindungsherstellung führen wir eine einfache Transact-SQL-Abfrage (T-SQL) aus, um die Kommunikation mit der Datenbank zu überprüfen.

[!INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[!INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

## <a name="run-sample-queries"></a>Durchführen von Beispielabfragen
Nachdem Sie eine Verbindung mit Ihrem Server hergestellt haben, können Sie eine Verbindung mit einer Datenbank herstellen und eine Beispielabfrage ausführen. Wenn Sie mit dem Schreiben von Abfragen noch nicht vertraut sind, helfen Ihnen die Informationen unter [Schreiben von Transact-SQL-Anweisungen](https://msdn.microsoft.com/library/ms365303.aspx)weiter.

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
4. Klicken Sie auf die Schaltfläche **Ausführen** .
   
    ![Erfolgreich. Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## <a name="next-steps"></a>Nächste Schritte
Mit T-SQL-Anweisungen können Sie Datenbanken in Azure auf gleiche Weise wie mit SQL Server erstellen und verwalten. Wenn Sie mit der Verwendung von T-SQL mit SQL Server vertraut sind, finden Sie unter [Azure SQL-Datenbank – Transact-SQL-Informationen](sql-database-transact-sql-information.md) eine Zusammenfassung der Unterschiede.

Wenn Sie mit T-SQL nicht vertraut sind, siehe [Tutorial: Schreiben von Transact-SQL-Anweisungen](https://msdn.microsoft.com/library/ms365303.aspx) und [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx).

Informationen zu den ersten Schritten zum Erstellen von Datenbankbenutzern und Datenbankbenutzeradministratoren finden Sie unter [SQL Datenbank-Tutorial: Erstellen von SQL-Datenbankbenutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken über das Azure-Portal](sql-database-get-started-security.md)

Weitere Informationen zu SSMS finden Sie unter [Verwenden Sie SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).




<!---HONumber=Nov16_HO2-->


