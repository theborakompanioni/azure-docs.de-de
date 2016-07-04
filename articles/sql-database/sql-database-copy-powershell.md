<properties 
    pageTitle="Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Azure" 
    description="Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von PowerShell" 
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="06/06/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell


> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-Portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Die folgenden Schritte veranschaulichen, wie Sie eine SQL-Datenbank mit PowerShell auf den gleichen oder einen anderen Server kopieren. Beim Kopieren der Datenbank wird das Cmdlet [Start-AzureSqlDatabaseCopy](https://msdn.microsoft.com/library/dn720220.aspx) verwendet.


Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
- Azure PowerShell. Sie können die Azure PowerShell-Module herunterladen und installieren, indem Sie den [Microsoft-Webplattform-Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409) ausführen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).



## Kopieren der SQL-Datenbank

Es gibt einige Variablen, bei denen Sie die Beispielwerte durch die speziellen Werte für Ihre Datenbank und Ihre Server ersetzen müssen. Ersetzen Sie die Platzhalterwerte durch die Werte für Ihre Umgebung:

    # The name of the server on which the source database resides.
    $ServerName = "sourceServerName"

    # The name of the source database (the database to copy). 
    $DatabaseName = "sourceDatabaseName" 
    
    # The name of the server that hosts the target database. This server must be in the same Azure subscription as the source database server. 
    $PartnerServerName = "partnerServerName"

    # The name of the target database (the name of the copy).
    $PartnerDatabaseName = "partnerDatabaseName" 





### Kopieren einer SQL-Datenbank auf denselben Server

Mit diesem Befehl wird die Anforderung zum Kopieren der Datenbank an den Dienst gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    # Copy a database to the same server
    Start-AzureSqlDatabaseCopy -ServerName $ServerName -DatabaseName $DatabaseName -PartnerDatabase $PartnerDatabaseName

### Kopieren einer SQL-Datenbank auf einen anderen Server

Mit diesem Befehl wird die Anforderung zum Kopieren der Datenbank an den Dienst gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    # Copy a database to a different server
    Start-AzureSqlDatabaseCopy -ServerName $ServerName -DatabaseName $DatabaseName -PartnerServer $PartnerServerName -PartnerDatabase $PartnerDatabaseName
    

## Überwachen des Fortschritts des Kopiervorgangs

Nach dem Ausführen von **Start-AzureSqlDatabaseCopy** können Sie den Status der Kopieranforderung überprüfen. Wenn die Ausführung sofort nach der Anforderung erfolgt, wird normalerweise **Status: Ausstehend** oder **Status: Wird ausgeführt** ausgegeben. Sie können die Ausführung also mehrmals durchführen, bis in der Ausgabe **Status: ABGESCHLOSSEN** angezeigt wird.


    Get-AzureSqlDatabaseOperation -ServerName $ServerName -DatabaseName $DatabaseName

## Auflösen von Anmeldungen

Informationen zum Auflösen von Anmeldungen, nachdem der Kopiervorgang abgeschlossen wurde, finden Sie unter [Auflösen von Anmeldungen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes).


## PowerShell-Beispielskript

    # The name of the server where the source database resides
    $ServerName = "sourceServerName"

    # The name of the source database (the database to copy) 
    $DatabaseName = "sourceDatabaseName" 
    
    # The name of the server to host the database copy. This server must be in the same Azure subscription as the source database server
    $PartnerServerName = "partnerServerName"

    # The name of the target database (the name of the copy)
    $PartnerDatabaseName = "partnerDatabaseName" 


    Add-AzureAccount
    Select-AzureSubscription -SubscriptionName "myAzureSubscriptionName"
      
    # Copy a database to a different server (remove the -PartnerServer parameter to copy to the same server)
    Start-AzureSqlDatabaseCopy -ServerName $ServerName -DatabaseName $DatabaseName -PartnerServer $PartnerServerName -PartnerDatabase $PartnerDatabaseName
    
    # Monitor the status of the copy
    Get-AzureSqlDatabaseOperation -ServerName $ServerName -DatabaseName $DatabaseName
    

## Nächste Schritte

- Einen Überblick über das Kopieren einer Azure SQL-Datenbank finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md).
- Informationen zum Kopieren einer Datenbank mithilfe des Azure-Portals finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md).
- Informationen zum Kopieren einer Datenbank mithilfe von Transact-SQL finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md).
- Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).


## Zusätzliche Ressourcen

- [Verwalten von Anmeldungen](sql-database-manage-logins.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
- [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0622_2016-->