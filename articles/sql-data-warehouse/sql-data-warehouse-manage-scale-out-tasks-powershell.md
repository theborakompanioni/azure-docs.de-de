<properties
   pageTitle="Aufgaben zum horizontalen Hochskalieren in Azure SQL Data Warehouse | Microsoft Azure"
   description="Erfahren Sie, wie Sie Computeressourcen für eine Azure SQL Data Warehouse-Datenbank anhalten oder starten (fortsetzen), und wie Sie die DWU-Einstellung für den Servicelevel-Zielpunkt (SLO) horizontal hochskalieren oder zurück skalieren. Zu diesen Aufgaben verwenden Sie die Azure PowerShell-Cmdlets."
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
   ms.date="04/21/2016"
   ms.author="barbkess;sonyama"/>

# Aufgaben zum horizontalen Hochskalieren oder Zurückskalieren von Computeressourcen in Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-manage-scale-out-tasks.md)
- [PowerShell](sql-data-warehouse-manage-scale-out-tasks-powershell.md)


Skalieren Sie Computeressourcen und Arbeitsspeicher elastisch horizontal hoch, um die sich ändernden Anforderungen Ihrer Workload zu erfüllen, und sparen Sie Kosten, indem Sie in Zeiten geringerer Auslastung Ressourcen zurück skalieren.

Diese Auflistung von Aufgaben verwendet PowerShell-Cmdlets zu folgenden Zwecken:

- Anhalten von Computeressourcen
- Fortsetzen von Computeressourcen
- Ändern von Computeressourcen durch Anpassen der DWUs

## Voraussetzungen

### Installieren der neuesten Version von Azure PowerShell

> [AZURE.NOTE]  Damit Sie Azure Powershell mit SQL Data Warehouse verwenden können, benötigen Sie Azure Powershell Version 1.0.3 oder höher. Um Ihre aktuelle Version zu überprüfen, führen Sie den Befehl **Get-Module - ListAvailable – Name Azure** aus. Sie können die neueste Version von [Microsoft Web Platform Installer][] herunterladen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

### Erste Schritte mit Azure PowerShell-Cmdlets

Erste Schritte:

1. Öffnen Sie Azure PowerShell. 
2. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## Aufgabe 1: Anhalten des Rechnens

[AZURE.INCLUDE [Beschreibung des Anhaltens in SQL Data Warehouse](../../includes/sql-data-warehouse-pause-description.md)]

Um eine Datenbank anzuhalten, verwenden Sie das Cmdlet [Suspend-AzureRmSqlDatabase][]. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ angehalten. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

> [AZURE.NOTE] Hinweis: Wenn Ihr Server „foo.database.windows.net“ heißt, verwenden Sie in den PowerShell-Cmdlets für „-ServerName“ den Namen „foo“.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
In einer Variation ruft das nächste Beispiel die Datenbank in das $database-Objekt ab. Das abgerufene Objekt wird dann an [Suspend-AzureRmSqlDatabase][] weitergeleitet. Die Ergebnisse werden in dem Objekt „resultDatabase“ gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## Aufgabe 2: Fortsetzen des Rechnens

[AZURE.INCLUDE [Beschreibung des Fortsetzens in SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie das Cmdlet [Resume-AzureRmSqlDatabase][], um eine Datenbank zu starten. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ gestartet. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

In einer Variation ruft das nächste Beispiel die Datenbank in das $database-Objekt ab. Dann wird das Objekt an [Resume-AzureRmSqlDatabase][] weitergeleitet, und die Ergebnisse werden in $resultDatabase gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```


## Aufgabe 3: Skalieren der DWUs

[AZURE.INCLUDE [Beschreibung des Skalierens der DWUs in SQL Data Warehouse](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie das PowerShell-Cmdlet [Set AzureRmSqlDatabase][]. Im folgenden Beispiel wird der Servicelevel-Zielpunkt für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ gesetzt.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

## Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Verwaltungstools für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Verwaltungstools für SQL Data Warehouse]: ./sql-data-warehouse-overview-manage.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0427_2016-->