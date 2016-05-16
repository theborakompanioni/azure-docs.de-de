<properties
   pageTitle="Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="REST-API-Tasks zum horizontalen Hochskalieren der Leistung für Azure SQL Data Warehouse. Ändern Sie Computeressourcen durch Anpassen der DWUs. Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen."
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

# Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (REST)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-scalability.md)
- [Portal](sql-data-warehouse-manage-scale-out-tasks.md)
- [PowerShell](sql-data-warehouse-manage-scale-out-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-scale-out-tasks-rest-api.md)
- [TSQL](sql-data-warehouse-manage-scale-out-tasks-tsql.md)

Skalieren Sie Computeressourcen und Arbeitsspeicher elastisch horizontal hoch, um die sich ändernden Anforderungen Ihrer Workload zu erfüllen, und sparen Sie Kosten, indem Sie in Zeiten geringerer Auslastung Ressourcen zurück skalieren.

Diese Sammlung von Tasks verwendet REST-APIs für folgende Aufgaben:

- Skalieren der Leistung durch Anpassen von DWUs
- Anhalten von Computeressourcen
- Fortsetzen von Computeressourcen

Informationen hierzu finden Sie unter [Übersicht über die Skalierbarkeit der Leistung][].

<a name="scale-performance-bk"></a>

## Skalieren der Leistung

[AZURE.INCLUDE [Beschreibung des Skalierens der DWUs in SQL Data Warehouse](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie die REST-API zum [Erstellen oder Aktualisieren einer Datenbank][]. Im folgenden Beispiel wird der Servicelevel-Zielpunkt für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ gesetzt. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

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

Andere Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Verwaltungsübersicht]: ./sql-data-warehouse-overview-manage.md
[Übersicht über die Skalierbarkeit der Leistung]: ./sql-data-warehouse-overview-scalability.md

<!--MSDN references-->
[Anhalten der Datenbank]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Fortsetzen der Datenbank]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Erstellen oder Aktualisieren einer Datenbank]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0504_2016-->