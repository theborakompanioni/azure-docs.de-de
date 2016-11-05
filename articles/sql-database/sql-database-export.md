---
title: Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
description: Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: ''

ms.service: sql-database
ms.devlang: NA
ms.date: 08/15/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA

---
# Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-export.md)
> * [PowerShell](sql-database-export-powershell.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie das [Azure-Portal](https://portal.azure.com) verwenden, um Ihre Azure SQL-Datenbank in einer BACPAC-Datei archivieren, die im Azure-Blobspeicher gespeichert ist.

Wenn Sie ein Archiv einer Azure SQL-Datenbank erstellen müssen, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung BACPAC. Eine BACPAC-Datei kann später im Azure-Blobspeicher oder im lokalen Speicher an einem lokalen Standort gespeichert werden und dann zurück in die Azure SQL-Datenbank oder in eine lokale SQL-Serverinstallation importiert werden.

***Überlegungen***

* Damit ein Archiv hinsichtlich der Transaktionen konsistent ist, müssen Sie entweder sicherstellen, dass während des Exports keine Schreibaktivitäten durchgeführt werden, oder den Export von einer [in Hinblick auf Transaktionen konsistenten Kopie](sql-database-copy.md) Ihrer Azure SQL-Datenbank durchführen.
* Die Maximalgröße für eine BACPAC-Datei, die in den Azure-Blobspeicher archiviert wird, beträgt 200 GB. Verwenden Sie das Eingabeaufforderungs-Hilfsprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx), um eine große BACPAC-Datei im lokalen Speicher zu archivieren. Dieses Hilfsprogramm wird mit Visual Studio und SQL Server bereitgestellt. Sie können auch die aktuelle Version der SQL Server Data Tools [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx), um dieses Hilfsprogramm zu erhalten.
* Das Archivieren in Azure Storage Premium mithilfe einer BACPAC-Datei wird nicht unterstützt.
* Falls der Exportvorgang länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Um die Leistung während des Exports zu erhöhen, können Sie Folgendes tun:
  * Erhöhen Sie vorübergehend Ihr Servicelevel.
  * Verhindern Sie jegliche Lese- und Schreibaktivitäten während des Exports.
  * Verwenden Sie einen [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx)mit Werten ungleich NULL in allen großen Tabellen. Ohne gruppierte Indizes schlägt ein Export, der länger als sechs bis zwölf Stunden dauert, ggf. fehl. Dies liegt daran, dass der Exportdienst einen Tabellenscan durchführen muss, um die gesamte Tabelle zu exportieren. Eine gute Möglichkeit, zu ermitteln, ob Ihre Tabellen für den Export optimiert sind, ist es, **DBCC SHOW\_STATISTICS** auszuführen und sicherzustellen dass *RANGE\_HI\_KEY* nicht NULL ist und der Wert eine gute Verteilung aufweist. Weitere Informationen finden Sie unter [DBCC SHOW\_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPAC-Dateien eignen sich nicht für Backup- und Wiederherstellungsvorgänge. Azure SQL-Datenbank erstellt für jede Benutzerdatenbank automatisch Sicherungen. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
> 
> 

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement.
* Azure SQL-Datenbank.
* Ein [Azure-Standardspeicherkonto](../storage/storage-create-storage-account.md) mit einem Blobcontainer, um die BACPAC-Dateien im Standardspeicher zu speichern.

## Exportieren der Datenbank
Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie exportieren möchten.

> [!IMPORTANT]
> Um eine im Hinblick auf Transaktionen konsistente BACPAC-Datei sicherzustellen, sollten Sie zunächst [eine Kopie Ihrer Datenbank erstellen](sql-database-copy.md) und dann die Datenbankkopie exportieren.
> 
> 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **SQL-Datenbanken**.
3. Klicken Sie auf die zu archivierende Datenbank
4. Klicken Sie im Blatt „SQL-Datenbank“ auf **Exportieren**, um das Blatt **Datenbank exportieren** zu öffnen:
   
   ![Schaltfläche „Exportieren“][1]
5. Klicken Sie auf **Storage**, und wählen Sie Ihr Speicherkonto und den Blob-Container aus, in dem die BACPAC-Datei gespeichert wird:
   
   ![Datenbank exportieren][2]
6. Wählen Sie Ihren Authentifizierungstyp aus.
7. Geben Sie für die Azure SQL Server-Instanz, die die zu exportierende Datenbank enthält, die passenden Authentifizierungsanmeldeinformationen ein.
8. Klicken Sie auf **OK**, um die Datenbank zu archivieren. Durch Klicken auf **OK** wird eine Anforderung zum Exportieren der Datenbank erstellt und an den Dienst gesendet. Die Dauer des Exports hängt von der Größe und Komplexität Ihrer Datenbank und von Ihrem Servicelevel ab. Sie erhalten eine Benachrichtigung.
   
   ![Benachrichtigung über Anfrage zum Datenbankexport][3]

## Überwachen des Fortschritts des Exportvorgangs
1. Klicken Sie auf **SQL-Server**.
2. Klicken Sie auf den Server mit der ursprünglichen (Quell)-Datenbank, die Sie gerade archiviert haben.
3. Scrollen Sie zu den Vorgängen runter.
4. Klicken Sie im Blatt „SQL Server“ auf **Import/Export-Verlauf**:
   
   ![Import/Export-Verlauf][4]

## Sicherstellen, dass sich die BACPAC-Datei im Speichercontainer befindet
1. Klicken Sie auf **Speicherkonten**.
2. Klicken Sie auf das Speicherkonto, in dem Sie das BACPAC-Archiv gespeichert haben.
3. Klicken Sie auf **Container** und wählen Sie den Container, in den Sie die Datenbank exportiert haben, um sich Details anzeigen zu lassen (Sie können die BACPAC-Datei hier herunterladen und speichern).
   
   ![Details der BACPAC-Datei][5]

## Nächste Schritte
* Informationen zum Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank](sql-database-import.md).
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx).

<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

<!---HONumber=AcomDC_0817_2016-->