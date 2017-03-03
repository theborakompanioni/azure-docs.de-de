---
title: Erteilen des Zugriffs auf Azure SQL-Datenbank | Microsoft Dokumentation
description: Hier erhalten Sie Informationen zum Erteilen des Zugriffs auf Microsoft Azure SQL-Datenbank.
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/06/2017
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: 8b9b9b70cc7b6a9b89a52523f713b43b74713042
ms.lasthandoff: 02/17/2017

---
# <a name="azure-sql-database-access-control"></a>Zugriffssteuerung bei Azure SQL-Datenbank
Zur Gewährleistung der Sicherheit steuert SQL-Datenbank den Zugriff mit Firewallregeln, die die Konnektivität nach IP-Adresse einschränken, Authentifizierungsmechanismen, die die Identitätsbestätigung durch Benutzer erfordern, und Autorisierungsmechanismen, die bestimmte Aktionen und Daten für Benutzer eingrenzen. 

> [!IMPORTANT]
> Eine Übersicht über die Sicherheitsfunktionen von SQL-Datenbank finden Sie unter [Securing your SQL Database](sql-database-security-overview.md) (Schützen Ihrer SQL-Datenbank). Ein Tutorial mit SQL Server-Authentifizierung finden Sie unter [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md). Ein Tutorial mit Azure Active Directory-Authentifizierung finden Sie unter [Azure AD-Authentifizierung und -Autorisierung](sql-database-control-access-aad-authentication-get-started.md).

## <a name="firewall-and-firewall-rules"></a>Firewall und Firewallregeln
Die Microsoft Azure SQL-Datenbank bietet einen relationalen Datenbankdienst für Azure und andere internetbasierte Anwendungen. Zum Schutz Ihrer Daten verhindern Firewalls jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung. Weitere Informationen finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](sql-database-firewall-configure.md).

Der Azure SQL-Datenbank-Dienst ist nur über TCP-Port 1433 verfügbar. Wenn Sie von Ihrem Computer auf eine SQL-Datenbank zugreifen möchten, sollten Sie sicherstellen, dass die Firewall Ihres Clientcomputers die ausgehende TCP-Kommunikation über TCP-Port 1433 zulässt. Sofern nicht für andere Anwendungen benötigt, blockieren Sie eingehende Verbindungen über TCP-Port 1433. 

Im Rahmen des Verbindungsprozesses werden Verbindungen von virtuellen Azure-Computern an eine andere IP-Adresse und einen anderen Port umgeleitet, wobei diese Angaben für jede Workerrolle jeweils eindeutig sind. Die Portnummer liegt im Bereich von 11000 bis 11999. Weitere Informationen zu TCP-Ports finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Authentifizierung

SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

* **SQL-Authentifizierung**, bei der ein Benutzername und ein Kennwort verwendet werden. Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. "dbo" (database owner) authentifizieren. 
* **Azure Active Directory-Authentifizierung**, bei der von Azure Active Directory verwaltete Identitäten verwendet werden und die für verwaltete und integrierte Domänen unterstützt wird. Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), [sofern dies möglich ist](https://msdn.microsoft.com/library/ms144284.aspx). Wenn Sie die Azure Active Directory-Authentifizierung verwenden möchten, müssen Sie einen weiteren Serveradministrator mit der Bezeichnung "Azure AD Admin" erstellen, der zum Verwalten von Azure Active Directory-Benutzern und -Gruppen berechtigt ist. Dieser Administrator kann außerdem die gleichen Aufgaben wie ein normaler Serveradministrator ausführen. Unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) finden Sie eine exemplarische Vorgehensweise zum Erstellen eines Azure AD-Administrators, um die Azure Active Directory-Authentifizierung zu aktivieren.

Das Datenbankmodul schließt Verbindungen, die sich länger als 30 Minuten im Leerlauf befinden. Für die Verbindung muss dann eine erneute Anmeldung durchgeführt werden, bevor sie verwendet werden kann. Ständig aktive Verbindungen mit SQL-Datenbank erfordern mindestens alle zehn Stunden eine erneute Autorisierung (per Datenbankmodul). Das Datenbankmodul versucht, die Autorisierung erneut durchzuführen, indem das ursprünglich übermittelte Kennwort verwendet wird. Im Erfolgsfall ist dann keine Benutzereingabe erforderlich. Wenn ein Kennwort in SQL-Datenbank zurückgesetzt wird, wird die Verbindung aus Leistungsgründen nicht erneut authentifiziert. Dies gilt auch, wenn die Verbindung aufgrund des Verbindungspoolings zurückgesetzt wird. Dies unterscheidet sich vom Verhalten einer lokalen SQL Server-Instanz. Wenn das Kennwort seit der ersten Autorisierung der Verbindung geändert wurde, muss die Verbindung beendet und mit dem neuen Kennwort eine neue Verbindung hergestellt werden. Ein Benutzer mit der Berechtigung `KILL DATABASE CONNECTION` kann eine Verbindung mit SQL-Datenbank mithilfe des Befehls [KILL](https://msdn.microsoft.com/library/ms173730.aspx) explizit beenden.

Benutzerkonten können in der Masterdatenbank erstellt werden und Berechtigungen in allen Datenbanken auf dem Server erhalten. Alternativ können sie in der Datenbank selbst erstellt werden (sogenannte eigenständige Benutzer). Informationen zum Erstellen und Verwalten von Anmeldungen finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md). Verwenden Sie Benutzer für eigenständige Datenbanken, um die Portabilität und Skalierbarkeit zu verbessern. Weitere Informationen zu eigenständigen Benutzern finden Sie unter [Eigenständige Datenbankbenutzer - machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx) und [Eigenständige Datenbanken](https://technet.microsoft.com/library/ff929071.aspx).

Es empfiehlt sich, für die Anwendung ein dediziertes Konto für die Authentifizierung zu verwenden. Auf diese Weise können Sie die Berechtigungen für die Anwendung beschränken und die Risiken schädlicher Aktivitäten reduzieren, falls Ihr Anwendungscode für einen Angriff durch Einschleusung von SQL-Befehlen anfällig ist. Der empfohlene Ansatz besteht darin, einen [eigenständigen Datenbankbenutzer](https://msdn.microsoft.com/library/ff929188) zu erstellen. Dadurch kann sich Ihre App direkt bei der Datenbank authentifizieren. 

## <a name="authorization"></a>Autorisierung

Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer in einer Azure SQL-Datenbank ausführen kann. Dies wird durch die [Datenbank-Rollenmitgliedschaften](https://msdn.microsoft.com/library/ms189121) und [Objektebenenberechtigungen](https://msdn.microsoft.com/library/ms191291.aspx) Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden. Weitere Informationen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).

In der Regel benötigen nur Administratoren Zugriff auf die `master`-Datenbank. Der routinemäßige Zugriff auf jede Benutzerdatenbank muss über Benutzer für eigenständige Datenbanken geschehen, die in jeder Datenbank erstellt werden und nicht über Administratorrechte verfügen. Bei Verwendung von Benutzern für eine eigenständige Datenbank müssen Sie keine Anmeldungen für die `master`-Datenbank erstellen. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

Machen Sie sich mit den folgenden Features zum Einschränken oder Erweitern von Berechtigungen vertraut:   
* Mithilfe von [Identitätswechsel](https://msdn.microsoft.com/library/vstudio/bb669087) und [Modulsignierung](https://msdn.microsoft.com/library/bb669102) können Berechtigungen ohne Sicherheitsbedenken vorübergehend erhöht werden.
* [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131) lassen sich die Zeilen beschränken, auf die ein Benutzer Zugriff hat.
* [Daten Masking](sql-database-dynamic-data-masking-get-started.md) kann zum Schutz vor unautorisierter Offenlegung von sensiblen Daten verwendet werden.
* [Gespeicherten Prozeduren](https://msdn.microsoft.com/library/ms190782) können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Sicherheitsfunktionen von SQL-Datenbank finden Sie unter [Securing your SQL Database](sql-database-security-overview.md) (Schützen Ihrer SQL-Datenbank).
- Weitere Informationen zu Firewallregeln finden Sie unter [Firewallregeln](sql-database-firewall-configure.md).
- Weitere Informationen zu Benutzern und Anmeldungenen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md). 
- Eine Beschreibung der proaktiven Überwachung finden Sie unter [Datenbanküberwachung](sql-database-auditing.md) und [Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).
- Ein Tutorial mit SQL Server-Authentifizierung finden Sie unter [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md).
- Ein Tutorial mit Azure Active Directory-Authentifizierung finden Sie unter [Azure AD-Authentifizierung und -Autorisierung](sql-database-control-access-aad-authentication-get-started.md).

