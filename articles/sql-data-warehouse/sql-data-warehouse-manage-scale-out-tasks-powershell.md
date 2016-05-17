<properties
   pageTitle="Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (PowerShell) | Microsoft Azure"
   description="PowerShell-Tasks zum horizontalen Hochskalieren der Leistung für Azure SQL Data Warehouse. Ändern Sie Computeressourcen durch Anpassen der DWUs. Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/28/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (PowerShell)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-scalability.md)
- [Portal](sql-data-warehouse-manage-scale-out-tasks.md)
- [PowerShell](sql-data-warehouse-manage-scale-out-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-scale-out-tasks-rest-api.md)
- [TSQL](sql-data-warehouse-manage-scale-out-tasks-tsql.md)

Skalieren Sie Computeressourcen und Arbeitsspeicher elastisch horizontal hoch, um die sich ändernden Anforderungen Ihrer Workload zu erfüllen, und sparen Sie Kosten, indem Sie in Zeiten geringerer Auslastung Ressourcen zurück skalieren.

Diese Auflistung von Aufgaben verwendet PowerShell-Cmdlets zu folgenden Zwecken:

- Skalieren der Leistung durch Anpassen von DWUs
- Anhalten von Computeressourcen
- Fortsetzen von Computeressourcen

Informationen hierzu finden Sie unter [Übersicht über die Skalierbarkeit der Leistung][].

<a name="before-you-begin-bk"></a>

## Voraussetzungen

### Installieren der neuesten Version von Azure PowerShell

> [AZURE.NOTE]  Damit Sie Azure Powershell mit SQL Data Warehouse verwenden können, benötigen Sie Azure Powershell Version 1.0.3 oder höher. Um Ihre aktuelle Version zu überprüfen, führen Sie den Befehl **Get-Module -ListAvailable -Name Azure** aus. Sie können die neueste Version von [Microsoft Web Platform Installer][] herunterladen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

### Erste Schritte mit Azure PowerShell-Cmdlets

Erste Schritte:

1. Öffnen Sie Azure PowerShell. 
2. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>

## Skalieren der Leistung

[AZURE.INCLUDE [Beschreibung des Skalierens der DWUs in SQL Data Warehouse](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie das PowerShell-Cmdlet [Set AzureRmSqlDatabase][]. Im folgenden Beispiel wird der Servicelevel-Zielpunkt für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ gesetzt.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## Anhalten von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Anhaltens in SQL Data Warehouse](../../includes/sql-data-warehouse-pause-description.md)]

Um eine Datenbank anzuhalten, verwenden Sie das Cmdlet [Suspend-AzureRmSqlDatabase][]. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ angehalten. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

> [AZURE.NOTE] Hinweis: Wenn Ihr Server „foo.database.windows.net“ heißt, verwenden Sie in den PowerShell-Cmdlets für „-ServerName“ den Namen „foo“.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
In einer Variation ruft das nächste Beispiel die Datenbank in das $database-Objekt ab. Das abgerufene Objekt wird dann an [Suspend-AzureRmSqlDatabase][] weitergeleitet. Die Ergebnisse werden in dem Objekt „resultDatabase“ gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## Fortsetzen von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Fortsetzens in SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie das Cmdlet [Resume-AzureRmSqlDatabase][], um eine Datenbank zu starten. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ gestartet. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

In einer Variation ruft das nächste Beispiel die Datenbank in das $database-Objekt ab. Dann wird das Objekt an [Resume-AzureRmSqlDatabase][] weitergeleitet, und die Ergebnisse werden in $resultDatabase gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## Nächste Schritte

Andere Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Verwaltungsübersicht]: ./sql-data-warehouse-overview-manage.md
[Übersicht über die Skalierbarkeit der Leistung]: ./sql-data-warehouse-overview-scalability.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0504_2016-->