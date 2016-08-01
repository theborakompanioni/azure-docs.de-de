<properties 
    pageTitle="Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank mithilfe von PowerShell" 
    description="In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank mit PowerShell zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank mit PowerShell." 
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="07/19/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank mit PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


Dienstebenen und Leistungsstufen beschreiben die für Ihre SQL-Datenbank verfügbaren Funktionen und Ressourcen und können aktualisiert werden, wenn sich die Anforderungen an Ihre Anwendung ändern. Weitere Informationen finden Sie unter [Tarife](sql-database-service-tiers.md).

Beachten Sie, dass durch das Ändern des Diensttarifs und/oder der Leistungsstufe einer Datenbank ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsstufe erstellt wird. Infolge wird die Verbindung zu diesem Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Nur in seltenen Fällen, z.B. wenn im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist dieses Fenster länger.

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in, aus oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Für eine Datenbank der gleichen Größe, die ihre Leistungsstufen innerhalb des Premium-Diensttarifs ändert, sollte er innerhalb von drei Stunden abgeschlossen sein.


- Für ein Downgrade einer Datenbank sollte die Datenbank kleiner als die in der Zieldienstebene maximal zulässige Größe sein.
- Beim Aktualisieren einer Datenbank, für die [Georeplikation](sql-database-geo-replication-portal.md) aktiviert ist, müssen Sie vor der Aktualisierung der primären Datenbank zunächst die zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe aktualisieren.
- Beim Downgrade von einer Premium-Dienstebene müssen Sie zuerst alle geografischen Replikationsbeziehungen beenden. Sie können die im Thema [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) beschriebenen Schritte verwenden, um den Replikationsprozess zwischen der primären und den aktiven sekundären Datenbanken zu beenden.
- Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der Azure SQL-Datenbank](sql-database-business-continuity.md).
- Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.



**Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:**

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenloses Konto** klicken. Lesen Sie anschließend diesen Artikel zu Ende.
- Eine Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]



## Ändern der Dienstebene und Leistungsstufe Ihrer SQL-Datenbank

Führen Sie das Cmdlet **Set-AzureRmSqlDatabase** aus, und legen Sie **-RequestedServiceObjectiveName** auf die Leistungsstufe des gewünschten Tarifs fest, z. B. *S0*, *S1*, *S2*, *S3*, *P1*, *P2*, ...

    $ResourceGroupName = "resourceGroupName"
    
    $ServerName = "serverName"
    $DatabaseName = "databaseName"

    $NewEdition = "Standard"
    $NewPricingTier = "S2"

    $ScaleRequest = Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier


  

   


## PowerShell-Beispielskript zum Ändern der Dienstebene und Leistungsstufe Ihrer SQL-Datenbank

    

    
    $SubscriptionId = "4cac86b0-1e56-bbbb-aaaa-000000000000"
    
    $ResourceGroupName = "resourceGroupName"
    $Location = "Japan West"
    
    $ServerName = "serverName"
    $DatabaseName = "databaseName"
    
    $NewEdition = "Standard"
    $NewPricingTier = "S2"
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId
    
    $ScaleRequest = Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
    
    $ScaleRequest
    
        


## Nächste Schritte

- [Horizontal hoch- und herunterskalieren](sql-database-elastic-scale-get-started.md)
- [Herstellen einer Verbindung mit einer SQL-Datenbank und Abfragen dieser Datenbank mit SSMS](sql-database-connect-query-ssms.md)
- [Exportieren einer Azure SQL-Datenbank](sql-database-export-powershell.md)

## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](http://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL-Datenbank-Cmdlets](http://msdn.microsoft.com/library/mt574084.aspx)

<!---HONumber=AcomDC_0720_2016-->