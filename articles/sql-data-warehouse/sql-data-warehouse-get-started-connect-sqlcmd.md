---
title: Abfragen von Azure SQL Data Warehouse (sqlcmd) | Microsoft Docs
description: Es wird beschrieben, wie Sie Azure SQL Data Warehouse mit dem Befehlszeilenprogramm sqlcmd abfragen.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1f2cf8003e46a1df30810a2594bc1d380bc13bcf


---
# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Abfragen von Azure SQL Data Warehouse (sqlcmd)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In dieser exemplarischen Vorgehensweise wird das Befehlszeilenprogramm [sqlcmd][sqlcmd] verwendet, um ein Azure SQL Data Warehouse abzufragen.  

## <a name="1-connect"></a>1. Verbinden
Öffnen Sie zur Nutzung von [sqlcmd][sqlcmd] zuerst die Befehlseingabeaufforderung, und geben Sie **sqlcmd** gefolgt von der Verbindungszeichenfolge für Ihre SQL Data Warehouse-Datenbank ein. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

* **Server (-S):** Server in Form von `<`Servername`>`.database.windows.net.
* **Datenbank (-D)** : Datenbankname.
* **Bezeichner in Anführungszeichen aktivieren (-I)** : Bezeichner müssen in Anführungszeichen eingeschlossen sein, um die Verbindung mit einer SQL Data Warehouse-Instanz herzustellen.

Zur Verwendung der SQL Server-Authentifizierung müssen Sie die Parameter für Benutzername und Kennwort hinzufügen:

* **Benutzer (-U):** Serverbenutzer im Format `<`Benutzer`>`.
* **Kennwort (-P)** : Das Kennwort des Benutzers.

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Um die in Azure Active Directory integrierte Authentifizierung verwenden zu können, müssen Sie die Azure Active Directory-Parameter hinzufügen:

* **Azure Active Directory-Authentifizierung (-G):** Azure Active Directory für die Authentifizierung verwenden

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Sie müssen [Azure Active Directory-Authentifizierung aktivieren](sql-data-warehouse-authentication.md) , um die Authentifizierung mithilfe von Active Directory zu ermöglichen.
> 
> 

## <a name="2-query"></a>2. Abfrage
Nach dem Herstellen der Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen für die Instanz ausgeben.  In diesem Beispiel werden Abfragen im interaktiven Modus gesendet.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Die nächsten Beispiele zeigen, wie Sie Ihre Abfragen im Batchmodus ausführen können, indem Sie die Option „-Q“ oder für den SQL-Code ein Pipe-Zeichen zur Verknüpfung mit „sqlcmd“ verwenden.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Nächste Schritte
Ausführliche Informationen zu den in sqlcmd verfügbaren Optionen finden Sie in der [Dokumentation zu sqlcmd][sqlcmd] .

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure-Portal]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=Nov16_HO2-->


