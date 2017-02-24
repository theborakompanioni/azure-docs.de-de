---
title: Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank | Microsoft Docs
description: Erstellen Sie eine Azure SQL-Datenbank, indem Sie eine vorhandene BACPAC-Datei importieren.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 02/07/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 60bcd02d24e2084b9020ce56ef6a9f8268c6b1b5
ms.openlocfilehash: e77a3ba82e7620e23441d9296ebed46d41cb525c


---
# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-using-the-azure-portal"></a>Importieren einer BACPAC-Datei in Azure SQL-Datenbank über das Azure-Portal

Dieser Artikel enthält Anweisungen zum Erstellen einer Azure SQL-Datenbank aus einer BACPAC-Datei mithilfe des [Azure-Portals](https://portal.azure.com).

## <a name="prerequisites"></a>Voraussetzungen

Zum Importieren einer SQL-­Datenbank aus einer BACPAC-Datei benötigen Sie Folgendes:

* Ein Azure-Abonnement. 
* Einen Azure SQL-Datenbank V12-Server Wenn Sie nicht über einen V12-Server verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
* Eine BACPAC-Datei der Datenbank, die Sie in einen Blobcontainer eines [Azure Storage-Kontos (Standard)](../storage/storage-create-storage-account.md) importieren möchten.

> [!IMPORTANT]
> Verwenden Sie beim Importieren einer BACPAC-Datei aus Azure-Blobspeicher den Standardspeicher. Das Importieren einer BACPAC-Datei aus dem Premiumspeicher wird nicht unterstützt.
> 

## <a name="import-the-database"></a>Importieren der Datenbank
Öffnen Sie das Blatt „SQL Server“:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **SQL-Server**.
3. Klicken Sie auf den Server, um die Datenbank darauf wiederherzustellen.
4. Klicken Sie auf dem Blatt „SQL Server“ auf **Datenbank importieren**, um das Blatt **Datenbank importieren** zu öffnen:
   
   ![Datenbank importieren][1]
5. Klicken Sie auf **Storage**, wählen Sie Ihr Speicherkonto, den Blobcontainer und die BACPAC-Datei aus, und klicken Sie auf **OK**.
   
   ![Speicheroptionen konfigurieren][2]
6. Wählen Sie den Tarif für die neue Datenbank aus, und klicken Sie auf **Auswählen**. Das direkte Importieren einer Datenbank in einen elastischen Pool wird nicht unterstützt. Sie können Daten jedoch zuerst in eine eigenständige Datenbank importieren und die Datenbank anschließend in einen Pool verschieben.
   
   ![Tarif auswählen][3]
7. Geben Sie einen **DATENBANKNAMEN** für die Datenbank ein, die Sie aus der BACPAC-Datei erstellen.
8. Wählen Sie den Authentifizierungstyp aus, und geben Sie dann die Authentifizierungsinformationen für den Server an. 
9. Klicken Sie auf **Erstellen** , um die Datenbank aus der BACPAC-Datei zu erstellen.
   
   ![Datenbank erstellen][4]

Durch Klicken auf **Erstellen** wird eine Anforderung zum Importieren der Datenbank an den Dienst gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Importvorgang abgeschlossen ist.

## <a name="monitor-the-progress-of-the-import-operation"></a>Überwachen des Fortschritts des Importvorgangs
1. Klicken Sie auf **SQL-Server**.
2. Klicken Sie auf dem Server, auf dem die Wiederherstellung erfolgt.
3. Klicken Sie auf dem Blatt „SQL Server“ im Bereich „Vorgänge“ auf **Import-/Exportverlauf**:
   
   ![Import-/Exportverlauf][5]
   ![Import-/Exportverlauf][6]

4. Zum Überprüfen, ob die Datenbank auf dem Server aktiv ist, klicken Sie auf **SQL-Datenbanken** und prüfen, ob der Status der neuen Datenbank **Online** ist.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Herstellen einer Verbindung mit einer importierten SQL-Datenbank und zum Abfragen einer solchen Datenbank finden Sie unter [Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md).
* Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).
* Eine Erläuterung des gesamten Migrationsprozesses von SQL Server-Datenbanken, einschließlich Empfehlungen zur Leistung, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).


<!--Image references-->
[1]: ./media/sql-database-import/import-database.png
[2]: ./media/sql-database-import/storage-options.png
[3]: ./media/sql-database-import/pricing-tier.png
[4]: ./media/sql-database-import/create.png
[5]: ./media/sql-database-import/import-history.png
[6]: ./media/sql-database-import/import-status.png



<!--HONumber=Feb17_HO2-->


