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
ms.custom: loading
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: cc5ec6022cede019541d697905aa068b88d25ee4
ms.lasthandoff: 03/22/2017


---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Laden von Beispieldaten in SQL Data Warehouse
Befolgen Sie diese einfachen Schritte, um die Adventure Works-Beispieldatenbank zu laden und abzufragen. Zunächst verwenden diese Skripts „sqlcmd“, um SQL auszuführen und so Tabellen und Ansichten zu erstellen. Nachdem die Tabellen erstellt wurden, verwenden die Skripts „bcp“, um Daten zu laden.  Wenn Sie sqlcmd und bcp noch nicht installiert haben, folgen Sie diesen Links, um [bcp][install bcp] und [sqlcmd][install sqlcmd] zu installieren.

## <a name="load-sample-data"></a>Laden von Beispieldaten
1. Laden Sie die ZIP-Datei mit [Adventure Works-Beispielskripts für SQL Data Warehouse][Adventure Works Sample Scripts for SQL Data Warehouse] herunter.
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
Nachdem Sie einige Beispieldaten in SQL Data Warehouse geladen haben, können Sie schnell einige Abfragen ausführen.  Stellen Sie zum Ausführen einer Abfrage eine Verbindung mit der neu erstellten Adventure Works-Datenbank in Azure SQL Data Warehouse mit Visual Studio und SSDT her, wie im Artikel [Abfragen mit Visual Studio][query with Visual Studio] beschrieben.

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

SQL Data Warehouse unterstützt fast alle T-SQL-Konstrukte, die SQL Server unterstützt.  Alle Unterschiede sind in unserer Dokumentation zum [Migrieren von Code][migrate code] dokumentiert.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Gelegenheit hatten, einige Abfragen mit Beispieldaten auszuprobieren, sehen Sie sich nun an, wie Sie für SQL Data Warehouse [entwickeln][develop], [laden][load] oder [migrieren][migrate] können.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip

