---
title: Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
description: Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 12/20/2016
ms.author: sstein;carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: df14225e6c2a1b9bf83623df172b9be9b5777add
ms.openlocfilehash: 33699b00d50c623661292e5a9b21a97726c47611


---
# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-export.md)
> * [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
> * [PowerShell](sql-database-export-powershell.md)
> 

In diesem Artikel wird beschrieben, wie Sie das [Azure-Portal](https://portal.azure.com)verwenden, um Ihre Azure SQL-Datenbank in einer BACPAC-Datei archivieren, die im Azure-Blobspeicher gespeichert ist.

Wenn Sie ein Archiv einer Azure SQL-Datenbank erstellen müssen, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung BACPAC. Eine BACPAC-Datei kann später im Azure-Blobspeicher oder im lokalen Speicher an einem lokalen Standort gespeichert werden und dann zurück in die Azure SQL-Datenbank oder in eine lokale SQL-Serverinstallation importiert werden. 

> [!IMPORTANT]
> Der automatisierte Export von Azure SQL-Datenbanken ist jetzt als Vorschau verfügbar und wird am 1. März 2017 eingestellt. Ab dem 1. Dezember 2016 ist das Konfigurieren des automatisierten Exports für SQL-­Datenbanken nicht mehr möglich. Alle vorhandenen Aufträge für automatisierten Export werden bis zum 1. März 2017 weiterhin ausgeführt. Nach dem 1. Dezember 2016 können Sie die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md) oder [Azure Automation](../automation/automation-intro.md) zum regelmäßigen Archivieren von SQL-Datenbanken mithilfe von PowerShell nach einem Zeitplan Ihrer Wahl verwenden. Als Beispielskript können Sie das [Beispielskript von GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) herunterladen. 
>

## <a name="considerations"></a>Überlegungen

* Damit ein Archiv hinsichtlich der Transaktionen konsistent ist, müssen Sie entweder sicherstellen, dass während des Exports keine Schreibaktivitäten durchgeführt werden, oder den Export von einer [in Hinblick auf Transaktionen konsistenten Kopie](sql-database-copy.md) Ihrer Azure SQL-Datenbank durchführen.
* Die Maximalgröße für eine BACPAC-Datei, die in den Azure-Blobspeicher archiviert wird, beträgt 200 GB. Verwenden Sie das Eingabeaufforderungs-Hilfsprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) , um eine große BACPAC-Datei im lokalen Speicher zu archivieren. Dieses Hilfsprogramm wird mit Visual Studio und SQL Server bereitgestellt. Sie können auch die aktuelle Version der SQL Server Data Tools [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx) , um dieses Hilfsprogramm zu erhalten.
* Das Archivieren in Azure Storage Premium mithilfe einer BACPAC-Datei wird nicht unterstützt.
* Falls der Exportvorgang länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Um die Leistung während des Exports zu erhöhen, können Sie Folgendes tun:
  * Erhöhen Sie vorübergehend Ihr Servicelevel.
  * Verhindern Sie jegliche Lese- und Schreibaktivitäten während des Exports.
  * Verwenden Sie einen [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit Werten ungleich NULL in allen großen Tabellen. Ohne gruppierte Indizes schlägt ein Export, der länger als sechs bis zwölf Stunden dauert, ggf. fehl. Dies liegt daran, dass der Exportdienst einen Tabellenscan durchführen muss, um die gesamte Tabelle zu exportieren. Eine gute Möglichkeit, um zu ermitteln, ob Ihre Tabellen für den Export optimiert sind, besteht darin, **DBCC SHOW_STATISTICS** auszuführen und sicherzustellen, dass *RANGE_HI_KEY* nicht NULL ist und der Wert eine gute Verteilung aufweist. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPAC-Dateien eignen sich nicht für Backup- und Wiederherstellungsvorgänge. Azure SQL-Datenbank erstellt für jede Benutzerdatenbank automatisch Sicherungen. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).  
> 

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement.
* Azure SQL-Datenbank. 
* Ein [Azure-Standardspeicherkonto](../storage/storage-create-storage-account.md) mit einem Blobcontainer, um die BACPAC-Dateien im Standardspeicher zu speichern.

## <a name="export-your-database"></a>Exportieren der Datenbank
Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie exportieren möchten.

> [!IMPORTANT]
> Um eine im Hinblick auf Transaktionen konsistente BACPAC-Datei sicherzustellen, sollten Sie zunächst [eine Kopie Ihrer Datenbank erstellen](sql-database-copy.md) und dann die Datenbankkopie exportieren. 
> 
> 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **SQL-Datenbanken**.
3. Klicken Sie auf die zu archivierende Datenbank
4. Klicken Sie auf dem Blatt „SQL-Datenbank“ auf **Exportieren**, um das Blatt **Datenbank exportieren** zu öffnen:
   
   ![Schaltfläche „Exportieren“][1]
5. Klicken Sie auf **Speicher**, und wählen Sie Ihr Speicherkonto und den Blob-Container aus, in dem die BACPAC-Datei gespeichert wird:
   
   ![Datenbank exportieren][2]
6. Wählen Sie Ihren Authentifizierungstyp aus. 
7. Geben Sie für die Azure SQL Server-Instanz, die die zu exportierende Datenbank enthält, die passenden Authentifizierungsanmeldeinformationen ein.
8. Klicken Sie auf **OK** , um die Datenbank zu archivieren. Durch Klicken auf **OK** wird eine Anforderung zum Exportieren der Datenbank erstellt und an den Dienst gesendet. Die Dauer des Exports hängt von der Größe und Komplexität Ihrer Datenbank und von Ihrem Servicelevel ab. Zeigen Sie die erhaltene Benachrichtigung an.
   
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
* Informationen zum Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank](sql-database-import.md)
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx)

<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png




<!--HONumber=Dec16_HO3-->


