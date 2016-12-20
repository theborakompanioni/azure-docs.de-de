---
title: Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank | Microsoft Docs
description: Erstellen Sie eine Azure SQL-Datenbank, indem Sie eine vorhandene BACPAC-Datei importieren.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.devlang: NA
ms.date: 08/31/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26364edba47c4ac77b2125e067b99f87eefb434c


---
# <a name="import-a-bacpac-file-to-create-an-azure-sql-database"></a>Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank
**Einzeldatenbank**

> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-import.md)
> * [PowerShell](sql-database-import-powershell.md)
> * [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
> 
> 

Dieser Artikel enthält Anweisungen zum Erstellen einer Azure SQL-Datenbank aus einer BACPAC-Datei mithilfe des [Azure-Portals](https://portal.azure.com).

Ein „BACPAC“ ist eine BACPAC-Datei, die ein Datenbankschema und Daten enthält. Die Datenbank wird aus einer BACPAC-Datei erstellt, die aus einem Azure-Speicherblobcontainer importiert wurde. Wenn im Azure-Speicher keine BACPAC-Datei vorhanden ist, können Sie eine erstellen, indem Sie die in [Erstellen und Exportieren der BACPAC-Datei einer Azure SQL-Datenbank](sql-database-export.md)beschriebenen Schritte ausführen.

> [!NOTE]
> Azure SQL-Datenbank erstellt und verwaltet automatisch Sicherungen für jede Benutzerdatenbank, die Sie wiederherstellen können. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
> 
> 

Zum Importieren einer SQL-­Datenbank aus einer BACPAC-Datei benötigen Sie Folgendes:

* Ein Azure-Abonnement. 
* Einen Azure SQL-Datenbank V12-Server Wenn Sie nicht über einen V12-Server verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
* Eine BACPAC-Datei der Datenbank, die Sie in einen Blobcontainer eines [Azure Storage-Kontos (Standard)](../storage/storage-create-storage-account.md) importieren möchten.

> [!IMPORTANT]
> Verwenden Sie beim Importieren einer BACPAC-Datei aus Azure-Blobspeicher den Standardspeicher. Das Importieren einer BACPAC-Datei aus dem Premiumspeicher wird nicht unterstützt.
> 
> 

## <a name="select-the-server-to-host-the-database"></a>Auswählen des Servers zum Hosten der Datenbank
Öffnen Sie das Blatt „SQL Server“:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **SQL-Server**.
3. Klicken Sie auf den Server, um die Datenbank darauf wiederherzustellen.
4. Klicken Sie auf dem Blatt „SQL Server“ auf **Datenbank importieren**, um das Blatt **Datenbank importieren** zu öffnen:
   
   ![Datenbank importieren][1]
5. Klicken Sie auf **Storage**, wählen Sie Ihr Speicherkonto, den Blobcontainer und die BACPAC-Datei aus, und klicken Sie auf **OK**.
   
   ![Speicheroptionen konfigurieren][2]
6. Wählen Sie den Tarif für die neue Datenbank aus, und klicken Sie auf **Auswählen**. Das direkte Importieren einer Datenbank in einen elastischen Pool wird nicht unterstützt. Sie können Daten jedoch zuerst in eine Einzeldatenbank importieren und die Datenbank anschließend in einen Pool verschieben.
   
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

## <a name="verify-the-database-is-live-on-the-server"></a>Sicherstellen, dass die Datenbank auf dem Server aktiv ist
1. Klicken Sie auf **SQL-Datenbanken**, und überprüfen Sie, ob die neue Datenbank **online** ist.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Herstellen einer Verbindung mit einer importierten SQL-Datenbank und zum Abfragen einer solchen Datenbank finden Sie unter [Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)

<!--Image references-->
[1]: ./media/sql-database-import/import-database.png
[2]: ./media/sql-database-import/storage-options.png
[3]: ./media/sql-database-import/pricing-tier.png
[4]: ./media/sql-database-import/create.png
[5]: ./media/sql-database-import/import-history.png
[6]: ./media/sql-database-import/import-status.png



<!--HONumber=Nov16_HO3-->


