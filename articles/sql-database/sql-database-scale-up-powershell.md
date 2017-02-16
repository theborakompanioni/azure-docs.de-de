---
title: "PowerShell: Ändern des Azure SQL-Datenbank-Tarifs | Microsoft Docs"
description: "Erfahren Sie, wie Sie die Dienstebene und Leistungsstufe einer Azure SQL-Datenbank-Instanz mit PowerShell ändern und wie Sie die Ressourcen und Preise Ihrer SQL-Datenbank zentral hoch- oder herunterskalieren."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 6f016c17-b048-4968-b82b-d2dcec954e54
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: NA
ms.date: 10/12/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 5b1a96149924c6170ea4561b9639f6b85b15ddd3
ms.openlocfilehash: c45d9200dd25d18d45602c377c682875d04c64cb


---
# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank mit PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-scale-up.md)
> * [**PowerShell**](sql-database-scale-up-powershell.md)> 
> 

Dienstebenen und Leistungsstufen beschreiben die für Ihre SQL-Datenbank verfügbaren Funktionen und Ressourcen und können aktualisiert werden, wenn sich die Anforderungen an Ihre Anwendung ändern. Weitere Informationen finden Sie unter [Tarife](sql-database-service-tiers.md).

Beachten Sie, dass durch das Ändern des Diensttarifs und/oder der Leistungsstufe einer Datenbank ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsstufe erstellt wird. Infolge wird die Verbindung zu diesem Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Nur in seltenen Fällen, z.B. wenn im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist dieses Fenster länger.  

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in, aus oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Eine Datenbank der gleichen Größe, die ihre Leistungsstufen innerhalb des Premium-Diensttarifs ändert, sollte dies innerhalb von drei Stunden vollziehen.

* Für ein Downgrade einer Datenbank sollte die Datenbank kleiner als die in der Zieldienstebene maximal zulässige Größe sein. 
* Beim Aktualisieren einer Datenbank, für die [Georeplikation](sql-database-geo-replication-portal.md) aktiviert ist, müssen Sie vor der Aktualisierung der primären Datenbank zunächst die zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe aktualisieren.
* Beim Downgrade von einer Premium-Dienstebene müssen Sie zuerst alle geografischen Replikationsbeziehungen beenden. Sie können die im Thema [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) beschriebenen Schritte verwenden, um den Replikationsprozess zwischen der primären und den aktiven sekundären Datenbanken zu beenden.
* Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der Azure SQL-Datenbank](sql-database-business-continuity.md).
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

**Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:**

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie lediglich oben auf dieser Seite auf **KOSTENLOSES KONTO**. Lesen Sie anschließend diesen Artikel zu Ende.
* Eine Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
* Azure PowerShell.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Ändern der Dienstebene und Leistungsstufe Ihrer SQL-Datenbank
Führen Sie das Cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx) aus, und legen Sie **-RequestedServiceObjectiveName** auf die Leistungsstufe des gewünschten Tarifs fest, z.B. *S0*, *S1*, *S2*, *S3*, *P1*, *P2*...

```
$ResourceGroupName = "resourceGroupName"

$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```






## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>PowerShell-Beispielskript zum Ändern der Dienstebene und Leistungsstufe Ihrer SQL-Datenbank
Ersetzen Sie ```{variables}``` durch Ihre Werte (ohne geschweifte Klammern).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"

$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"

$ServerName = "{server}"
$DatabaseName = "{database}"

$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```



## <a name="next-steps"></a>Nächste Schritte
* [Horizontal hoch- und herunterskalieren](sql-database-elastic-scale-get-started.md)
* [Herstellen einer Verbindung mit einer SQL-Datenbank und Abfragen dieser Datenbank mit SSMS](sql-database-connect-query-ssms.md)
* [Exportieren einer Azure SQL-Datenbank](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbankdokumentation](http://azure.microsoft.com/documentation/services/sql-database/)
* [Azure SQL-Datenbank-Cmdlets](http://msdn.microsoft.com/library/azure/mt574084https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx.aspx)




<!--HONumber=Jan17_HO1-->


