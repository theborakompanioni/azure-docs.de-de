---
title: 'Azure-Portal: Erstellen und Verwalten einzelner Azure SQL-Datenbanken | Microsoft-Dokumentation'
description: "Kurzübersicht über das Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit dem Azure-Portal"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 02/06/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 319e22e2fb423e0fe1fabf95d0018d4fa3232e0b
ms.openlocfilehash: 5d7f11b9b027f86adf3d2382e82f2d382d71ee51


---
# <a name="create-and-manage-single-azure-sql-databases-with-the-azure-portal"></a>Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit dem Azure-Portal

Sie können einzelne Azure SQL-Datenbanken mit dem [Azure-Portal](https://portal.azure.com/), mit PowerShell, Transact-SQL, der REST-API oder C# erstellen und verwalten. In diesem Thema wird die Verwendung des Azure-Portals erläutert. Informationen zur Verwendung von PowerShell finden Sie unter [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit PowerShell](sql-database-manage-single-databases-powershell.md). Informationen zur Verwendung von Transact-SQL finden Sie unter [Erstellen und Verwalten einzelner Azure SQL-Datenbanken mit Transact-SQL](sql-database-manage-single-databases-tsql.md). 

## <a name="create-a-single-azure-sql-database-with-the-azure-portal"></a>Erstellen einer einzelnen Azure SQL-Datenbank mit dem Azure-Portal

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) das Blatt **SQL-Datenbanken**. 

    ![SQL-Datenbanken](./media/sql-database-get-started/sql-databases.png)
2. Klicken Sie auf dem Blatt „SQL-Datenbanken“ auf **Hinzufügen**.

    ![Hinzufügen der SQL-Datenbank](./media/sql-database-get-started/add-sql-database.png)

> [!TIP]
> Ein Tutorial zum Erstellen einer Datenbank mit dem Azure-Portal finden Sie unter [SQL-Datenbank-Tutorial: Enthält Informationen zu den ersten Schritten mit Azure SQL-Datenbankservern, -Datenbanken und -Firewallregeln mit dem Azure-Portal und SQL Server Management Studio](sql-database-get-started.md).
>    

## <a name="view-and-update-sql-database-settings-using-the-azure-portal"></a>Anzeigen und Aktualisieren von SQL-Datenbankeinstellungen über das Azure-Portal

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) das Blatt **SQL-Datenbanken**. 

    ![SQL-Datenbanken](./media/sql-database-get-started/sql-databases.png)

2. Klicken Sie auf die Datenbank, die Sie verwenden möchten, und klicken Sie dann auf dem Blatt der SQL-Datenbank auf die gewünschte Einstellung.

    ![Blatt für neue Beispieldatenbank](./media/sql-database-get-started/new-sample-db-blade.png)

## <a name="change-the-service-tier-and-performance-level-of-a-single-database"></a>Ändern der Dienstebene und Leistungsstufe einer Einzeldatenbank
Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie zentral hoch- oder herunterskalieren möchten:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL-Datenbanken**.
2. Klicken Sie auf die Datenbank, die Sie ändern möchten.
3. Klicken Sie auf dem Blatt **SQL-Datenbank** auf **Tarif (DTUs skalieren)**:
   
   ![Tarif](./media/sql-database-manage-single-database-portal/new-tier.png)

4. Wählen Sie eine neue Stufe aus, und klicken Sie auf **Auswählen**:
   
   Durch das Klicken auf **Auswählen** wird eine Skalierungsanforderung zum Ändern des Tarifs gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Skalierungsvorgang abgeschlossen ist (siehe Informationen oben in diesem Artikel).
   
   > [!NOTE]
   > Durch das Ändern des Tarifs für Ihre Datenbank ändert sich nicht die maximale Datenbankgröße. Verwenden Sie zum Ändern der maximalen Datenbankgröße [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
   > 
   > 
   
   ![Tarif auswählen](./media/sql-database-manage-single-database-portal/choose-tier.png)
5. Klicken Sie auf das Benachrichtigungssymbol (Glocke), in der oberen rechten Ecke:
   
   ![Benachrichtigungen](./media/sql-database-manage-single-database-portal/scale-notification.png)
   
6. Klicken Sie auf den Benachrichtigungstext, um im Detailbereich öffnen, der den Status der Anforderung anzeigt.

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht über Verwaltungstools finden Sie unter [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mit dem Azure-Portal finden Sie unter [Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von PowerShell finden Sie unter [Verwalten von Azure SQL-Datenbank mithilfe von PowerShell](sql-database-manage-powershell.md).
* Informationen zum Ausführen von Verwaltungsaufgaben mithilfe von SQL Server Management Studio finden Sie unter [Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md).
* Informationen zum SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md). 
* Informationen zu Azure-Datenbankservern und Datenbankfeatures finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md).



<!--HONumber=Feb17_HO2-->


