---
title: Laden von Beispieldaten in SQL Data Warehouse | Microsoft Docs
description: Laden von Beispieldaten in SQL Data Warehouse
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9edad2037787a7a2da4e2a6fcce15ace51d41032


---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Laden von Beispieldaten in SQL Data Warehouse
Befolgen Sie diese einfachen Schritte, um die Adventure Works-Beispieldatenbank zu laden und abzufragen. Zunächst verwenden diese Skripts „sqlcmd“, um SQL auszuführen und so Tabellen und Ansichten zu erstellen. Nachdem die Tabellen erstellt wurden, verwenden die Skripts „bcp“, um Daten zu laden.  Wenn Sie „sqlcmd“ und „bcp“ noch nicht installiert haben, folgen Sie diesen Links, um [install bcp][install bcp] und [sqlcmd][sqlcmd] zu installieren.

## <a name="load-sample-data"></a>Laden von Beispieldaten
1. Laden Sie die ZIP-Datei [Adventure Works-Beispielskripts für SQL Data Warehouse][Adventure Works-Beispielskripts für SQL Data Warehouse] herunter.
2. Extrahieren Sie die Dateien aus der heruntergeladenen ZIP-Datei in ein Verzeichnis auf Ihrem lokalen Computer.
3. Bearbeiten Sie die extrahierte Datei „aw_create.bat“, und legen Sie am Anfang der Datei die folgenden Variablen fest:  Entfernen Sie alle Leerzeichen zwischen dem „=“ und dem Parameter.  Im Folgenden finden Sie Beispiele für die Bearbeitung.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. Führen Sie über die Windows-Befehlszeile die bearbeitete Datei „aw_create.bat“ aus.  Stellen Sie sicher, dass Sie sich in dem Verzeichnis befinden, in dem Sie die bearbeitete Version von „aw_create.bat“ gespeichert haben.
   Mit diesem Skript werden folgende Vorgänge durchgeführt:
   
   * Löschen von Adventure Works-Tabellen oder -Ansichten, die bereits in der Datenbank vorhanden sind
   * Erstellen von Adventure Works-Tabellen und -Ansichten
   * Laden der einzelnen Adventure Works-Tabellen mithilfe von bcp
   * Überprüfen der Zeilenanzahl für jede Adventure Works-Tabelle
   * Erfassen von Statistiken jeder Spalte für jede Adventure Works-Tabelle

## <a name="query-sample-data"></a>Abfragen von Beispieldaten
Nachdem Sie einige Beispieldaten in SQL Data Warehouse geladen haben, können Sie schnell einige Abfragen ausführen.  Stellen Sie zum Ausführen einer Abfrage mithilfe von Visual Studio und SSDT eine Verbindung mit der neu erstellten Adventure Works-Datenbank in Azure SQL Data Warehouse her, wie im Artikel [Abfragen von Azure SQL Data Warehouse (Visual Studio)][Abfragen von Azure SQL Data Warehouse (Visual Studio)] beschrieben.

Beispiel einer einfachen SELECT-Anweisung, um alle Informationen zu den Mitarbeitern abzurufen:

```sql
SELECT * FROM DimEmployee;
```

Beispiel einer komplexeren Abfrage mithilfe von Konstrukten wie GROUP BY, um den Gesamtbetrag für alle Verkäufe an jedem Tag anzuzeigen:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Beispiel einer SELECT-Anweisung mit einer WHERE-Klausel, um Aufträge vor einem bestimmten Datum herauszufiltern:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse unterstützt fast alle T-SQL-Konstrukte, die SQL Server unterstützt.  Alle Unterschiede werden in unserer Dokumentation zum [Migrieren von Code][Migrieren von Code] erläutert.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie jetzt Gelegenheit hatten, einige Abfragen mit Beispieldaten auszuprobieren, sehen Sie sich als Nächstes an, wie das [Entwickeln][Entwickeln], [Laden][Laden] oder [Migrieren][Migrieren] für SQL Data Warehouse funktioniert.

<!--Image references-->

<!--Article references-->
[Migrieren]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Laden]: sql-data-warehouse-overview-load.md
[Abfragen von Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[Migrieren von Code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works-Beispielskripts für SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip



<!--HONumber=Nov16_HO3-->


