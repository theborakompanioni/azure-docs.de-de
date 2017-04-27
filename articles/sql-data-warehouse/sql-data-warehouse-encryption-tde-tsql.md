---
title: Transparent Data Encryption in SQL Data Warehouse (T-SQL) | Microsoft Docs
description: Transparent Data Encryption (TDE) in SQL Data Warehouse (T-SQL)
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 74c9032aababdce91ed617cd7a4c628915b42504
ms.lasthandoff: 04/03/2017


---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Erste Schritte mit Transparent Data Encryption (TDE)
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Erforderliche Berechtigungen
Sie müssen Administrator oder ein Mitglied der Rolle „dbmanager“ sein, um Transparent Data Encryption (TDE) zu aktivieren.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung
Führen Sie die folgenden Schritte aus, um TDE für ein SQL Data Warehouse zu aktivieren:

1. Stellen Sie eine Verbindung mit der Datenbank *master* auf dem Server her, auf dem die Datenbank gehostet wird. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung
Führen Sie die folgenden Schritte aus, um TDE für ein SQL Data Warehouse zu deaktivieren:

1. Stellen Sie eine Verbindung mit der Datenbank *master* her. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Ein angehaltenes SQL Data Warehouse muss vor dem Ändern der TDE-Einstellungen fortgesetzt werden.
> 
> 

## <a name="verifying-encryption"></a>Überprüfen der Verschlüsselung
Führen Sie die folgenden Schritte aus, um den Verschlüsselungsstatus für ein SQL Data Warehouse zu überprüfen:

1. Stellen Sie eine Verbindung mit der Datenbank *master* oder einer Instanzdatenbank her. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Das Ergebnis ```1``` steht für eine verschlüsselte Datenbank, und mit ```0``` wird eine nicht verschlüsselte Datenbank angegeben.

## <a name="encryption-dmvs"></a>Verschlüsselung-DMVs
* [sys.databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->

