---
title: "Aktivieren von Transparent Data Encryption für Stretch-Datenbank-T-SQL – Azure | Microsoft-Dokumentation"
description: "Aktivieren von Transparent Data Encryption (TDE) für SQL Server Stretch-Datenbank über Azure T-SQL"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
translationtype: Human Translation
ms.sourcegitcommit: bcb0a66425439522e0c9a353798ac70505b91e39
ms.openlocfilehash: fe6a6bfb02dc5d83bd67fdfe59ed87660c364835


---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Aktivieren von Transparent Data Encryption (TDE) für eine Stretch-Datenbank in Azure (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparent Data Encryption (TDE) bietet Schutz vor der Bedrohung durch böswillige Aktivitäten. Hierzu werden die Datenbank, die dazugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt, ohne dass Änderungen der Anwendung erforderlich sind.

TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird. Der Datenbank-Verschlüsselungsschlüssel ist mit einem integrierten Serverzertifikat geschützt. Das integrierte Serverzertifikat ist für jeden Azure-Server einmalig. Microsoft führt für diese Zertifikate nach spätestens 90 Tagen automatisch eine Rotation durch. Eine allgemeine Beschreibung von TDE finden Sie unter [Transparente Datenverschlüsselung (TDE)].

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung
Befolgen Sie folgende Schritte zum Aktivieren von TDE für eine Azure-Datenbank, die die Daten speichert, die aus einer SQL Server Datenbank migriert wurden, für die Stretch aktiviert ist:

1. Stellen Sie eine Verbindung mit der Datenbank *master* auf dem Azure-Server her, auf dem die Datenbank gehostet wird. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung
Befolgen Sie folgende Schritte zum Deaktivieren von TDE für eine Azure-Datenbank, die die Daten speichert, die aus einer SQL Server Datenbank migriert wurden, für die Stretch aktiviert ist:

1. Stellen Sie eine Verbindung mit der Datenbank *master* her. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Überprüfen der Verschlüsselung
Befolgen Sie folgende Schritte zur Überprüfung des Verschlüsselungsstatus für eine Azure-Datenbank, die die Daten speichert, die aus einer SQL Server Datenbank migriert wurden, für die Stretch aktiviert ist:

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

<!--Anchors-->
[Transparente Datenverschlüsselung (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->



<!--HONumber=Jan17_HO4-->


