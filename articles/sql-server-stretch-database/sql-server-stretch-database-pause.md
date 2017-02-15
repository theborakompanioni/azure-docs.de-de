---
title: Anhalten und Fortsetzen der Datenmigration (Stretch Database) | Microsoft Docs
description: "Erfahren Sie Einzelheiten über das Anhalten oder Fortsetzen der Datenmigration in Azure."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: d45c05ad-254e-4950-a652-3d5cc40ed967
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f8b5f4461a20c1407ba2ce834ec6dd970f4e7544


---
# <a name="pause-and-resume-data-migration-stretch-database"></a>Anhalten und Fortsetzen der Datenmigration (Stretch Database)
Wählen Sie zum Anhalten oder Fortsetzen der Migration von Daten in Azure für eine Tabelle in SQL Server Management Studio **Stretch** aus, und wählen Sie dann **Anhalten**, um die Datenmigration anzuhalten, oder **Fortsetzen**, um die Datenmigration fortzusetzen. Sie können zum Anhalten bzw. Fortsetzen der Datenmigration auch Transact\-SQL verwenden.

Die Datenmigration lässt sich für einzelne Tabellen anhalten, um Probleme auf dem lokalen Server zu behandeln oder um die verfügbare Netzwerkbandbreite zu maximieren.

## <a name="pause-data-migration"></a>Anhalten der Datenmigration
### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Verwenden von SQL Server Management Studio zum Anhalten der Datenmigration
1. Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Stretch\-fähige Tabelle aus, deren Migration angehalten werden soll.
2. Wählen Sie durch Klicken mit der rechten Maustaste **Stretch** aus, und wählen Sie dann **Anhalten** aus.

### <a name="use-transact-sql-to-pause-data-migration"></a>Verwenden von Transact\-SQL zum Anhalten der Datenmigration
Führen Sie den folgenden Befehl aus:

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Fortsetzen der Datenmigration
### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Verwenden von SQL Server Management Studio zum Fortsetzen der Datenmigration
1. Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Stretch\-fähige Tabelle aus, deren Migration fortgesetzt werden soll.
2. Wählen Sie durch Klicken mit der rechten Maustaste **Stretch** aus, und wählen Sie dann **Fortsetzen** aus.

### <a name="use-transact-sql-to-resume-data-migration"></a>Verwenden von Transact\-SQL zum Fortsetzen der Datenmigration
Führen Sie den folgenden Befehl aus:

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Überprüfen, ob die Migration aktiv ist oder angehalten wurde
### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Verwenden von SQL Server Management Studio, um zu überprüfen, ob die Migration aktiv ist oder angehalten wurde
Öffnen Sie in SQL Server Management Studio **Stretch Database Monitor**, und überprüfen Sie den Wert der Spalte **Migrationsstatus**. Weitere Informationen finden Sie unter [Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Verwenden von Transact-SQL zum Überprüfen, ob die Migration aktiv ist oder angehalten wurde
Fragen Sie die Katalogansicht **sys.remote_data_archive_tables** ab, und überprüfen Sie den Wert der Spalte **is_migration_paused**. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Weitere Informationen
[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)](sql-server-stretch-database-monitor.md)




<!--HONumber=Nov16_HO3-->


