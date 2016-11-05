---
title: Verwalten eines Pools für elastische Datenbanken (PowerShell) | Microsoft Docs
description: Erfahren Sie, wie Sie PowerShell zum Verwalten eines Pools für elastische Datenbanken nutzen
services: sql-database
documentationcenter: ''
author: srinia
manager: jhubbard
editor: ''

ms.service: sql-database
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 06/22/2016
ms.author: srinia

---
# Überwachen und Verwalten eines Pools für elastische Datenbanken mit PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-elastic-pool-manage-portal.md)
> * [PowerShell](sql-database-elastic-pool-manage-powershell.md)
> * [C#](sql-database-elastic-pool-manage-csharp.md)
> * [T-SQL](sql-database-elastic-pool-manage-tsql.md)
> 
> 

Verwalten eines [Pools für elastische Datenbanken](sql-database-elastic-pool.md) mit PowerShell-Cmdlets

Häufige Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme](sql-database-develop-error-messages.md).

Werte für Pools finden Sie unter [eDTUs und Speicherbeschränkungen](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

## Voraussetzungen
* Azure PowerShell 1.0 oder höher. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
* Pools für elastische Datenbanken sind nur auf SQL-Datenbank V12-Servern verfügbar. Wenn Sie über einen SQL-Datenbank V11-Server verfügen, können Sie in einem Schritt [mithilfe von PowerShell auf V12 aktualisieren und einen Pool erstellen](sql-database-upgrade-server-portal.md).

## Verschieben einer Datenbank in einen elastischen Pool
Mit dem Cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) können Sie eine Datenbank in einen Pool oder aus diesem verschieben.

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## Ändern Sie die Leistungseinstellungen eines Pools
Bei beeinträchtigter Leistung können Sie die Einstellungen des Pools ändern, um Wachstum zu ermöglichen. Verwenden Sie dazu das Cmdlet [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx). Legen Sie den „-Dtu“-Parameter auf die eDTUs pro Pool fest. Mögliche Werte sind unter [eDTUs und Speicherbeschränkungen](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) aufgeführt.

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## Abrufen des Status der Poolvorgänge
Das Erstellen eines Pools kann einige Zeit in Anspruch nehmen. Sie können den Status der Vorgänge im Pool, einschließlich Erstellung und Updates, mithilfe des Cmdlets [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) nachverfolgen.

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## Abrufen des Status beim Verschieben einer elastischen Datenbank in und aus einem Pool
Das Verschieben einer Datenbank kann einige Zeit in Anspruch nehmen. Den Status bei Verschiebungen können Sie mit dem Cmdlet [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) nachverfolgen.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## Abrufen der Daten zur Ressourcenauslastung für einen Pool
Metriken, die als Prozentsatz des Ressourcenpool-Grenzwerts abgerufen werden können:

| Metrikname | Beschreibung |
|:--- |:--- |
| cpu\_percent |Durchschnittliche Computeauslastung als Prozentwert der maximalen Kapazität des Pools. |
| physical\_data\_read\_percent |Durchschnittliche E/A-Auslastung als Prozentwert der maximalen Kapazität des Pools. |
| log\_write\_percent |Durchschnittliche Auslastung der Schreibressourcen als Prozentwert der maximalen Kapazität des Pools. |
| DTU\_consumption\_percent |Durchschnittliche eDTU-Auslastung als Prozentwert des eDTU-Grenzwerts für den Pool. |
| storage\_percent |Durchschnittliche Speicherauslastung als Prozentwert der Speicherbeschränkung des Pools. |
| workers\_percent |Maximale Anzahl der gleichzeitigen Worker (Anforderungen) als Prozentwert basierend auf der maximalen Kapazität des Pools. |
| sessions\_percent |Maximale Anzahl der gleichzeitigen Sitzungen als Prozentwert basierend auf der maximalen Kapazität des Pools. |
| eDTU\_limit |Aktuelle maximale DTU-Einstellung des elastischen Pools für einen bestimmten elastischen Pool und ein bestimmtes Intervall. |
| storage\_limit |Aktuelle maximale Speicherbeschränkung des elastischen Pools für einen bestimmten elastischen Pool und ein bestimmtes Intervall in MB. |
| eDTU\_used |Durchschnittliche Anzahl der vom Pool in einem bestimmten Intervall genutzten eDTUs. |
| storage\_used |Durchschnittliche Speicherauslastung durch den Pool in einem bestimmten Intervall in Byte. |

**Granularität/Beibehaltungsdauern für die Metriken:**

* Daten werden mit 5-Minuten-Granularität zurückgegeben.
* Die Datenaufbewahrung beträgt 35 Tage.

Dieses Cmdlet und die API beschränken die Anzahl der Zeilen, die in einem Aufruf abgerufen werden können, auf 1000 Zeilen (ca. drei Tage Daten bei 5-Minuten-Granularität). Dieser Befehl kann jedoch mehrmals mit verschiedenen Anfangs- und Endzeitintervallen aufgerufen werden, um mehr Daten abzurufen.

So rufen Sie die Metriken ab

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## Abrufen der Daten zur Ressourcenauslastung für eine elastische Datenbank
Diese APIs sind identisch mit den aktuellen APIs (V12), die für die Überwachung der Ressourcenverwendung einer eigenständigen Datenbank verwendet werden, mit Ausnahme der folgenden semantischen Unterschiede:

Die für diese API abgerufenen Metriken werden als Prozentsatz des Maximalwerts für eDTUs (oder der entsprechenden Obergrenze für die zugrunde liegende Metrik wie CPU, E/A usw.) ausgedrückt, der für diesen Pool festgelegt wurde. Beispielsweise zeigen 50 % Auslastung bei einer dieser Metriken an, dass der spezifische Ressourcenverbrauch der Obergrenze pro Datenbank für diese Ressource im übergeordneten Pool bei 50 % liegt.

So rufen Sie die Metriken ab

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## Eine Warnung zu einer Poolressource hinzufügen
Sie können Regeln zu Ressourcen hinzufügen, um E-Mail-Benachrichtigungen oder Warnzeichenfolgen an [URL-Endpunkte](https://msdn.microsoft.com/library/mt718036.aspx) zu senden, wenn die Ressource einen von Ihnen eingerichteten Nutzungsschwellenwert erreicht. Verwenden Sie das Cmdlet Add-AzureRmMetricAlertRule.

> [!IMPORTANT]
> Bei der Überwachung der Ressourcenverwendung für elastische Pools gibt es eine Verzögerung von mindestens 20 Minuten. Das Festlegen von Warnungen von weniger als 30 Minuten für elastische Pools wird derzeit nicht unterstützt. Warnungen für elastische Pools mit einem Zeitraum (Parameter namens „-WindowSize“ in der PowerShell-API) von weniger als 30 Minuten werden möglicherweise nicht ausgelöst. Stellen Sie sicher, dass alle Warnungen, die Sie für elastische Pools definieren, einen Zeitraum (WindowSize) von 30 Minuten oder mehr verwenden.
> 
> 

Dieses Beispiel fügt eine Warnung hinzu, damit eine Benachrichtigung gesendet wird, wenn die eDTU-Nutzung eines Pools einen bestimmten Schwellenwert überschreitet.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## Hinzufügen von Warnungen zu allen Datenbanken in einem Pool
Sie können Warnungsregeln zu allen Datenbanken in einem elastischen Pool hinzufügen, um E-Mail-Benachrichtigungen oder Warnzeichenfolgen an [URL-Endpunkte](https://msdn.microsoft.com/library/mt718036.aspx) zu senden, wenn eine Ressource einen durch die Warnung eingerichteten Nutzungsschwellenwert erreicht.

> [!IMPORTANT]
> Bei der Überwachung der Ressourcenverwendung für elastische Pools gibt es eine Verzögerung von mindestens 20 Minuten. Das Festlegen von Warnungen von weniger als 30 Minuten für elastische Pools wird derzeit nicht unterstützt. Warnungen für elastische Pools mit einem Zeitraum (Parameter namens „-WindowSize“ in der PowerShell-API) von weniger als 30 Minuten werden möglicherweise nicht ausgelöst. Stellen Sie sicher, dass alle Warnungen, die Sie für elastische Pools definieren, einen Zeitraum (WindowSize) von 30 Minuten oder mehr verwenden.
> 
> 

Dieses Beispiel fügt jeder Datenbank in einem Pool eine Warnung hinzu, damit eine Benachrichtigung gesendet wird, wenn die DTU-Nutzung einer Datenbank einen bestimmten Schwellenwert überschreitet.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## Sammeln und Überwachen von Ressourcennutzungsdaten über mehrere Pools in einem Abonnement hinweg
Wenn Sie über eine große Anzahl von Datenbanken in einem Abonnement verfügen, ist es mühselig, jeden elastischen Pool einzeln zu überwachen. Stattdessen können PowerShell-Cmdlets und T-SQL-Abfragen für SQL-Datenbank kombiniert werden, um Ressourcennutzungsdaten aus mehreren Pools und ihren Datenbanken zu sammeln, um eine Überwachung und Analyse der Ressourcennutzung durchzuführen. Eine [Beispielimplementierung](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) eines solchen PowerShell-Skriptsatzes steht im GitHub-Repository für SQL Server-Beispiele zur Verfügung. Hier finden Sie außerdem eine Dokumentation zur Funktionsweise und Verwendung der Beispiele.

Um diese Beispielimplementierung zu verwenden, führen Sie die nachfolgend aufgeführten Schritte aus.

1. Laden Sie [Skripts und die Dokumentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) herunter:
2. Ändern Sie die Skripts für Ihre Umgebung ab. Geben Sie einen oder mehrere Server an, auf denen elastische Pools gehostet werden.
3. Geben Sie eine Telemetriedatenbank an, in der die gesammelten Metriken gespeichert werden.
4. Passen Sie das Skript an, um die Dauer der Skriptausführung festzulegen.

Im Allgemeinen führen die Skripts folgende Aufgaben aus:

* Auflisten aller Server in einem vorgegebenen Azure-Abonnement (oder einer angegebenen Serverliste).
* Ausführen eines Hintergrundauftrags für jeden Server. Der Auftrag wird in regelmäßigen Intervallen in einer Schleife ausgeführt und sammelt Telemetriedaten für alle Pools auf dem Server. Anschließend werden die gesammelten Daten in die angegebene Telemetriedatenbank geladen.
* Auflisten der Datenbanken in jedem Pool, um Daten zur Datenbank-Ressourcennutzung zu sammeln. Anschließend werden die gesammelten Daten in die Telemetriedatenbank geladen.

Die gesammelten Metriken in der Telemetriedatenbank können analysiert werden, um die Integrität der elastischen Pools und der darin enthaltenen Datenbanken zu überwachen. Die Skripts installieren außerdem eine vordefinierte Tabellenwertfunktion (Table-Value Function, TVF) in der Telemetriedatenbank, um die Aggregation der Metriken für ein bestimmtes Zeitfenster zu ermöglichen. Beispielsweise können die Ergebnisse der TVF zur Anzeige der „Top-N der elastischen Pools mit der höchsten eDTU-Nutzung in einem vorgegebenen Zeitfenster“ verwendet werden. Optional können Sie Analysetools wie Excel oder Power BI zur Abfrage und Analyse der gesammelten Daten verwenden.

## Beispiel: Abrufen der Ressourcenverbrauchsmetriken für einen Pool und die zugehörigen Datenbanken
In diesem Beispiel werden die Verbrauchsmetriken für einen bestimmten elastischen Pool und alle zugehörigen Datenbanken abgerufen. Die gesammelten Daten werden formatiert und in eine Datei im CSV-Format geschrieben. Die Datei kann mit Excel durchsucht werden.

    $subscriptionId = '<Azure subscription id>'          # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name

    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC

    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }

    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }

    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }

    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## Latenzzeit der elastischen Poolvorgänge
* Änderungen der minimalen oder maximalen Anzahl der eDTUs pro Datenbank werden in der Regel in maximal 5 Minuten durchgeführt.
* Änderungen der eDTUs pro Pool hängen von der Gesamtmenge des Speicherplatzes aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte, von allen Datenbanken im Pool verwendete Speicherplatz 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

## Migration von V11- auf V12-Server
PowerShell-Cmdlets sind verfügbar, um ein Upgrade auf Azure SQL-Datenbank V12 von V11 oder einer anderen niedrigeren Version als V12 zu starten, zu beenden oder zu überwachen.

* [Upgrade auf SQL-Datenbank V12 mithilfe von PowerShell](sql-database-upgrade-server-powershell.md)

Referenzdokumentation zu diesen PowerShell-Cmdlets finden Sie unter:

* [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
* [Start-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
* [Stop-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)

Mit dem Cmdlet „Stop-“ wird der Vorgang abgebrochen, nicht angehalten. Ein Upgrade kann nach einem Abbruch nicht fortgesetzt werden, es muss wieder von vorne begonnen werden. Das Cmdlet „Stop-“ führt eine Bereinigung durch und gibt alle entsprechenden Ressourcen frei.

## Nächste Schritte
* [Erstellen elastischer Aufträge](sql-database-elastic-jobs-overview.md): Elastische Aufträge ermöglichen die Ausführung von T-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool.
* Unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md) finden Sie Informationen zur Verwendung elastischer Datenbanktools für die horizontale Hochskalierung, zum Verschieben von Daten, für die Abfrage oder zum Erstellen von Transaktionen.

<!---HONumber=AcomDC_0706_2016-->