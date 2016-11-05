---
title: Sicherheitsrichtlinien und Einschränkungen von Azure SQL-Datenbank | Microsoft Docs
description: Enthält Informationen zu den Richtlinien und Einschränkungen von Microsoft Azure SQL-Datenbank, die sich auf die Sicherheit beziehen.
services: sql-database
documentationcenter: ''
author: BYHAM
manager: jhubbard
editor: ''
tags: ''

ms.service: sql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/02/2016
ms.author: rickbyh

---
# Sicherheitsrichtlinien und Einschränkungen von Azure SQL-Datenbank
In diesem Thema werden die Richtlinien und Einschränkungen von Microsoft Azure SQL-Datenbank beschrieben, die sich auf die Sicherheit beziehen. Beachten Sie die folgenden Punkte, wenn Sie die Sicherheit Ihrer Azure SQL-Datenbanken verwalten.

## Zugriff auf die virtuelle Masterdatenbank
In der Regel benötigen nur Administratoren Zugriff auf die Masterdatenbank. Der routinemäßige Zugriff auf jede Benutzerdatenbank muss über Benutzer für eigenständige Datenbanken geschehen, die in jeder Datenbank erstellt werden und nicht über Administratorrechte verfügen. Bei der Verwendung von Benutzern für eine eigenständige Datenbank müssen Sie keine Anmeldungen für die Masterdatenbank erstellen. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

## Firewall
Die SQL Server-Firewall, die auf den gesamten Azure SQL Server ausgelegt ist, wird normalerweise über das Portal konfiguriert und darf nur die von Administratoren verwendeten IP-Adressen zulassen. Stellen Sie eine Verbindung zu einer Benutzerdatenbank her, und verwenden Sie anschließend die Transact-SQL-Anweisung [sp\_set\_database\_firewall\_rule](https://msdn.microsoft.com/library/dn270010.aspx), um eine datenbankweite Firewallregel zu erstellen, die den notwendigen Bereich von IP-Adressen für jede Datenbank öffnet.

Der Azure SQL-Datenbank-Dienst ist nur über TCP-Port 1433 verfügbar. Wenn Sie von Ihrem Computer auf eine SQL-Datenbank zugreifen möchten, sollten Sie sicherstellen, dass die Firewall Ihres Clientcomputers die ausgehende TCP-Kommunikation über TCP-Port 1433 zulässt. Sofern nicht für andere Anwendungen benötigt, blockieren Sie eingehende Verbindungen über TCP-Port 1433.

Im Rahmen des Verbindungsprozesses werden Verbindungen von virtuellen Azure-Computern an eine andere IP-Adresse und einen anderen Port umgeleitet, wobei diese Angaben für jede Workerrolle jeweils eindeutig sind. Die Portnummer liegt im Bereich von 11.000 bis 11.999. Weitere Informationen zu TCP-Ports finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).

## Authentifizierung
Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), wenn Sie die Möglichkeit dazu haben. Informationen zum Konfigurieren der AD-Authentifizierung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) und [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/library/ms144284.aspx) in der SQL Server-Onlinedokumentation.

Verwenden Sie Benutzer für eigenständige Datenbanken, um die Skalierbarkeit zu verbessern. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer - machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx) und [Eigenständige Datenbanken](https://technet.microsoft.com/library/ff929071.aspx).

Das Datenbankmodul schließt Verbindungen, die sich länger als 30 Minuten im Leerlauf befinden. Für die Verbindung muss dann eine erneute Anmeldung durchgeführt werden, bevor sie verwendet werden kann.

Ständig aktive Verbindungen mit SQL-Datenbank erfordern mindestens alle 10 Stunden eine erneute Autorisierung (per Datenbankmodul). Das Datenbankmodul versucht, die Autorisierung erneut durchzuführen, indem das ursprünglich übermittelte Kennwort verwendet wird. Im Erfolgsfall ist dann keine Benutzereingabe erforderlich. Wenn ein Kennwort in SQL-Datenbank zurückgesetzt wird, wird die Verbindung aus Leistungsgründen nicht erneut authentifiziert. Dies gilt auch, wenn die Verbindung aufgrund des Verbindungspoolings zurückgesetzt wird. Dies unterscheidet sich vom Verhalten einer lokalen SQL Server-Instanz. Wenn das Kennwort seit der ersten Autorisierung der Verbindung geändert wurde, muss die Verbindung beendet und mit dem neuen Kennwort eine neue Verbindung hergestellt werden. Ein Benutzer mit der Berechtigung KILL DATABASE CONNECTION kann eine Verbindung mit SQL-Datenbank explizit beenden, indem er den Befehl [KILL](https://msdn.microsoft.com/library/ms173730.aspx) verwendet.

## Anmeldungen und Benutzer
Für die Verwaltung von Anmeldungen und Benutzern in SQL-Datenbank gelten Einschränkungen.

* Sie müssen mit der Masterdatenbank (der Datenbank namens **master**) verbunden sein, wenn Sie die ``CREATE/ALTER/DROP DATABASE``-Anweisungen ausführen. – Der Datenbankbenutzer in der Masterdatenbank, der dem Anmeldeprinzipal auf Serverebene entspricht, kann nicht geändert oder verworfen werden.
* US-Englisch ist die Standardsprache des Anmeldeprinzipals auf Serverebene.
* Für das Zugreifen auf die Datenbank **master** muss jede Anmeldung einem Benutzerkonto in der Datenbank **master** zugeordnet sein. Die Datenbank **master** unterstützt keine Benutzer für eigenständige Datenbanken.
* Nur die Serverebenenprinzipal-Anmeldung und die Mitglieder der Datenbankrolle **dbmanager** in der Datenbank **master** verfügen über die Berechtigung zum Ausführen der Anweisungen ``CREATE DATABASE`` und ``DROP DATABASE``.
* Sie müssen über eine Verbindung mit der Datenbank „master“ verfügen, wenn Sie die ``CREATE/ALTER/DROP LOGIN``-Anweisungen ausführen. Von der Verwendung von Anmeldungen wird jedoch abgeraten. Verwenden Sie stattdessen Benutzer von eigenständigen Datenbanken.
* Sie müssen den Namen der Datenbank in der Verbindungszeichenfolge angeben, um eine Verbindung mit einer Benutzerdatenbank herzustellen.
* Nur die Serverebenenprinzipal-Anmeldung und die Mitglieder der Datenbankrolle **loginmanager** in der Datenbank **master** verfügen über die Berechtigung zum Ausführen der Anweisungen ``CREATE LOGIN``, ``ALTER LOGIN`` und ``DROP LOGIN``.
* Beim Ausführen der Anweisungen ``CREATE/ALTER/DROP LOGIN`` und ``CREATE/ALTER/DROP DATABASE`` in einer ADO.NET-Anwendung ist die Verwendung von parametrisierten Befehlen nicht zulässig. Weitere Informationen finden Sie unter [Befehle und Parameter](https://msdn.microsoft.com/library/ms254953.aspx).
* Beim Ausführen der Anweisungen ``CREATE/ALTER/DROP DATABASE`` und ``CREATE/ALTER/DROP LOGIN`` muss es sich jeweils um die einzige Anweisung in einem Transact-SQL-Batch handeln. Andernfalls tritt ein Fehler auf. Beispielsweise wird mit dem folgenden Transact-SQL-Code überprüft, ob die Datenbank vorhanden ist. Wenn ja, wird die Anweisung ``DROP DATABASE`` aufgerufen, um die Datenbank zu entfernen. Da die Anweisung ``DROP DATABASE`` nicht die einzige Anweisung im Batch ist, führt das Ausführen der folgenden Transact-SQL-Anweisung zu einem Fehler.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

* Beim Ausführen der Anweisung ``CREATE USER`` mit der Option ``FOR/FROM LOGIN`` muss sie die einzige Anweisung in einem Transact-SQL-Batch sein.
* Beim Ausführen der Anweisung ``ALTER USER`` mit der Option ``WITH LOGIN`` muss sie die einzige Anweisung in einem Transact-SQL-Batch sein.
* Für ``CREATE/ALTER/DROP`` benötigt ein Benutzer die Berechtigung ``ALTER ANY USER`` für die Datenbank.
* Wenn der Besitzer einer Datenbankrolle versucht, einen anderen Datenbankbenutzer dieser Datenbankrolle hinzuzufügen oder ihn daraus zu entfernen, tritt ggf. ein Fehler der folgenden Art auf: **Benutzer oder Rolle "Name" ist in dieser Datenbank nicht vorhanden.** Dieser Fehler tritt auf, da der Benutzer für den Besitzer nicht sichtbar ist. Gewähren Sie dem Rollenbesitzer die Berechtigung ``VIEW DEFINITION`` für den Benutzer, um dieses Problem zu beheben.

Weitere Informationen zu Anmeldungen und Benutzern finden Sie unter [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](sql-database-manage-logins.md).

## Weitere Informationen
[Firewall für die Azure SQL-Datenbank](sql-database-firewall-configure.md)

[Konfigurieren der Firewalleinstellungen (Azure SQL-Datenbank)](sql-database-configure-firewall-settings.md)

[Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](sql-database-manage-logins.md)

[Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!---HONumber=AcomDC_0803_2016-->