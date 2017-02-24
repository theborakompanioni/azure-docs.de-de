---
title: 'Azure-Portal: Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei | Microsoft-Dokumentation'
description: Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 02/07/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 3d04be3d2427bc59d24bfaad227730991b61265b
ms.openlocfilehash: ed983134e468a69c0e89387a3211479630a278e3


---
# <a name="export-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals

In diesem Artikel wird beschrieben, wie Sie das [Azure-Portal](https://portal.azure.com) verwenden, um Ihre Azure SQL-Datenbank in eine BACPAC-Datei zu exportieren, die im Azure-Blobspeicher gespeichert ist. Einen Überblick über den Export in eine BACPAC-Datei finden Sie unter [Exportieren in eine BACPAC-Datei](sql-database-export.md).

> [!NOTE]
> Sie können Ihre Azure SQL-Datenbankdatei auch über [SQL Server Management Studio](sql-database-export-ssms.md), [PowerShell](sql-database-export-powershell.md) oder [SQLPackage](sql-database-export-sqlpackage.md) in eine BACPAC-Datei exportieren.
>

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement.
* Azure SQL-Datenbank. 
* Ein [Azure-Standardspeicherkonto](../storage/storage-create-storage-account.md) mit einem Blobcontainer, um die BACPAC-Dateien im Standardspeicher zu speichern.

## <a name="export-your-database"></a>Exportieren der Datenbank

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie exportieren möchten.
3. Stellen Sie sicher, dass keine Transaktionen während des Exports erfolgen. 

   > [!IMPORTANT]
   > Um eine im Hinblick auf Transaktionen konsistente BACPAC-Datei sicherzustellen, empfiehlt es sich, zunächst [eine Kopie Ihrer Datenbank zu erstellen](sql-database-copy.md) und dann die Datenbankkopie zu exportieren. 
   > 

4. Klicken Sie auf **SQL-Datenbanken**.
5. Klicken Sie auf die zu archivierende Datenbank
6. Klicken Sie auf dem Blatt „SQL-Datenbank“ auf **Exportieren**, um das Blatt **Datenbank exportieren** zu öffnen:
   
   ![Schaltfläche „Exportieren“][1]
7. Klicken Sie auf **Speicher**, und wählen Sie Ihr Speicherkonto und den Blob-Container aus, in dem die BACPAC-Datei gespeichert wird:
   
   ![Datenbank exportieren][2]
8. Wählen Sie Ihren Authentifizierungstyp aus. 
9. Geben Sie für die Azure SQL Server-Instanz, die die zu exportierende Datenbank enthält, die passenden Authentifizierungsanmeldeinformationen ein.
10. Klicken Sie auf **OK**, um die Datenbank zu exportieren. Durch Klicken auf **OK** wird eine Anforderung zum Exportieren der Datenbank erstellt und an den Dienst gesendet. Die Dauer des Exports hängt von der Größe und Komplexität Ihrer Datenbank und von Ihrem Servicelevel ab. 
11. Zeigen Sie die erhaltene Benachrichtigung an.
   
   ![Benachrichtigung über Anfrage zum Datenbankexport][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen des Fortschritts des Exportvorgangs
1. Klicken Sie auf **SQL-Server**.
2. Klicken Sie auf den Server mit der ursprünglichen (Quell)-Datenbank, die Sie archiviert haben.
3. Scrollen Sie zu den Vorgängen runter.
4. Klicken Sie im Blatt „SQL Server“ auf **Import/Export-Verlauf**:
   
   ![Import/Export-Verlauf][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Sicherstellen, dass sich die BACPAC-Datei im Speichercontainer befindet
1. Klicken Sie auf **Speicherkonten**.
2. Klicken Sie auf das Speicherkonto, in dem Sie das BACPAC-Archiv gespeichert haben.
3. Klicken Sie auf **Container** und wählen Sie den Container, in den Sie die Datenbank exportiert haben, um sich Details anzeigen zu lassen (Sie können die BACPAC-Datei hier herunterladen und speichern).
   
   ![Details der BACPAC-Datei][5]    

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur langfristigen Archivierung von Azure SQL-Datenbanksicherungen als Alternative zum Exportieren einer Datenbank für Archivzwecke finden Sie unter [Langfristige Archivierung von Sicherungen](sql-database-long-term-retention.md).
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx)

<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png




<!--HONumber=Feb17_HO2-->


