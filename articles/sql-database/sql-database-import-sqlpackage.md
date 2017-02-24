---
title: 'SqlPackage: Importieren aus einer BACPAC-Datei in Azure SQL-Datenbank | Microsoft Docs'
description: "Dieser Artikel zeigt, wie Sie mithilfe des Befehlszeilen-Hilfsprogramms für die Eingabeaufforderung SqlPackage aus einer BACPAC-Datei in SQL-Datenbank importieren."
keywords: Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, BACPAC-Datei importieren, sqlpackage
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 424afa27-5f13-4ec3-98f6-99a511a6a2df
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 02/08/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e6f0d661465c813ec310b8c69ab1ee06e4f95401
ms.openlocfilehash: 23c5da7b9e171312274aec02262b139ee701deb1


---
# <a name="import-a-database-from-a-bacpac-file-to-azure-sql-database-using-sqlpackage"></a>Importieren einer Datenbank aus einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SqlPackage

Dieser Artikel zeigt, wie Sie aus einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei mithilfe des [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)-Befehlszeilen-Hilfsprogramms für die Eingabeaufforderung in SQL-Datenbank importieren. Dieses Hilfsprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.

> [!NOTE]
> Die folgenden Schritten setzen voraus, dass Sie bereits einen SQL-Datenbankserver bereitgestellt haben, Ihnen die Verbindungsinformationen vorliegen und Sie die Kompatibilität der Quelldatenbank überprüft haben.
> 
> 

## <a name="import-the-database"></a>Importieren der Datenbank
Führen Sie zum Importieren einer kompatiblen SQL Server-Datenbank (oder Azure SQL-Datenbank) aus einer BACPAC-Datei das Befehlszeilenhilfsprogramm [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) aus.

> [!NOTE]
> Die folgenden Schritten setzen voraus, dass Sie bereits einen Azure SQL-Datenbank-Server bereitgestellt haben und Ihnen die Verbindungsinformationen vorliegen.
>  

Führen Sie an einer Eingabeaufforderung im Verzeichnis mit der neuesten Version des Befehlszeilenhilfsprogramm „SqlPackage.exe“ einen Befehl wie den folgenden Beispielbefehl aus, über den eine BACPAC-Datei in eine Azure SQL-Datenbank als „Premium P11“ importiert wird.

```
SqlPackage.exe /a:import /tcs:"Data Source=SERVER;Initial Catalog=DBNAME;User Id=USER;Password=PASSWORD" /sf:C:\db.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P11
```   

## <a name="next-steps"></a>Nächste Schritte

* Eine Erläuterung des gesamten Migrationsprozesses von SQL Server-Datenbanken, einschließlich Empfehlungen zur Leistung, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).
* Referenzinformationen zu SQLPackage finden Sie unter [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx).
* Informationen zum Herunterladen der neuesten Version von SQLPackage finden Sie unter [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876).
* Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).




<!--HONumber=Feb17_HO2-->


