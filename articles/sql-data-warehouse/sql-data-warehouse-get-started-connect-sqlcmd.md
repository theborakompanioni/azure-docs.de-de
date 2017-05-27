---
title: Verbinden mit Azure SQL Data Warehouse mithilfe von sqlcmd | Microsoft-Dokumentation
description: Verwenden Sie das Befehlszeilenprogramm [sqlcmd][sqlcmd], um eine Verbindung mit einer Azure SQL Data Warehouse-Instanz herzustellen und diese abzufragen.
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 3508cdf6dcfa3d7122e1e3b635f3cd37863dbf62
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017


---
# <a name="connect-to-sql-data-warehouse-with-sqlcmd"></a>Verbinden mit SQL Data Warehouse per sqlcmd
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Verwenden Sie das Befehlszeilenprogramm [sqlcmd][sqlcmd], um eine Verbindung mit einer Azure SQL Data Warehouse-Instanz herzustellen und diese abzufragen.  

## <a name="1-connect"></a>1. Verbinden
Öffnen Sie zur Verwendung von [sqlcmd][sqlcmd] die Eingabeaufforderung, und geben Sie **sqlcmd** sowie die Verbindungszeichenfolge für Ihre SQL Data Warehouse-Datenbank ein. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

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
Ausführlichere Informationen zu den verfügbaren Optionen für „sqlcmd“ finden Sie in der [Dokumentation zu „sqlcmd“][sqlcmd].

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->

