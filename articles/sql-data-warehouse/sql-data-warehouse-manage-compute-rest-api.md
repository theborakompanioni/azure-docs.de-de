<properties
   pageTitle="Verwalten von Computeleistung in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="PowerShell-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Computeleistung in Azure SQL Data Warehouse (REST)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Passen Sie die Leistung durch horizontales Hochskalieren der Computeressourcen und des Arbeitsspeichers an, um die wechselhaften Anforderungen Ihres Workloads zu erfüllen. Sparen Sie außerhalb von Spitzenzeiten Kosten, indem Sie die zur Verfügung stehenden Ressourcen verringern oder den Computevorgang vollständig pausieren.

Diese Sammlung von Tasks verwendet das Azure-Portal für folgende Aufgaben:

- Skalieren von Computeressourcen
- Anhalten von Computeressourcen
- Fortsetzen von Computeressourcen

Informationen hierzu finden Sie in der [Übersicht über das Verwalten von Computeleistung][].

<a name="scale-performance-bk"></a> <a name="scale-compute-bk"></a>

## Skalieren von Computeleistung

[AZURE.INCLUDE [Beschreibung des Skalierens der DWUs in SQL Data Warehouse](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Verwenden Sie die REST-API zum [Erstellen oder Aktualisieren einer Datenbank][], um die DWUs zu ändern. Im folgenden Beispiel wird der Servicelevel-Zielpunkt für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ gesetzt. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## Anhalten von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Anhaltens in SQL Data Warehouse](../../includes/sql-data-warehouse-pause-description.md)]

Zum Anhalten einer Datenbank verwenden Sie die REST-API zum [Anhalten der Datenbank][]. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ angehalten. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## Fortsetzen von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Fortsetzens in SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie die REST-API zum [Fortsetzen der Datenbank][], um eine Datenbank zu starten. Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ gestartet. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Verwaltungstools für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Verwaltungstools für SQL Data Warehouse]: ./sql-data-warehouse-overview-manage.md
[Übersicht über das Verwalten von Computeleistung]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Anhalten der Datenbank]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Fortsetzen der Datenbank]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Erstellen oder Aktualisieren einer Datenbank]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0810_2016-->