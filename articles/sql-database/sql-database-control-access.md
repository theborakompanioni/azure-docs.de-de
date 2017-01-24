---
title: Zugriffssteuerung bei Azure SQL-Datenbank | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zum Steuern des Zugriff auf Microsoft Azure SQL-Datenbank.
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
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/18/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 82508d01f9f1aaee11ebb5a5f936dfc71ace9cad


---
# <a name="azure-sql-database-access-control"></a>Zugriffssteuerung bei Azure SQL-Datenbank
Zur Gewährleistung der Sicherheit steuert SQL-Datenbank den Zugriff mit Firewallregeln, die die Konnektivität nach IP-Adresse einschränken, Authentifizierungsmechanismen, die die Identitätsbestätigung durch Benutzer erfordern, und Autorisierungsmechanismen, die bestimmte Aktionen und Daten für Benutzer eingrenzen. 

> [!IMPORTANT]
> Eine Übersicht über die Sicherheitsfunktionen von SQL-Datenbank finden Sie unter [Securing your SQL Database](sql-database-security-overview.md) (Schützen Ihrer SQL-Datenbank).
>

## <a name="firewall-and-firewall-rules"></a>Firewall und Firewallregeln
Die Microsoft Azure SQL-Datenbank bietet einen relationalen Datenbankdienst für Azure und andere internetbasierte Anwendungen. Zum Schutz Ihrer Daten verhindern Firewalls jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung. Weitere Informationen finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](sql-database-firewall-configure.md).

Der Azure SQL-Datenbank-Dienst ist nur über TCP-Port 1433 verfügbar. Wenn Sie von Ihrem Computer auf eine SQL-Datenbank zugreifen möchten, sollten Sie sicherstellen, dass die Firewall Ihres Clientcomputers die ausgehende TCP-Kommunikation über TCP-Port 1433 zulässt. Sofern nicht für andere Anwendungen benötigt, blockieren Sie eingehende Verbindungen über TCP-Port 1433. 

Im Rahmen des Verbindungsprozesses werden Verbindungen von virtuellen Azure-Computern an eine andere IP-Adresse und einen anderen Port umgeleitet, wobei diese Angaben für jede Workerrolle jeweils eindeutig sind. Die Portnummer liegt im Bereich von 11000 bis 11999. Weitere Informationen zu TCP-Ports finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Authentifizierung

SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

* **SQL-Authentifizierung**, bei der ein Benutzername und ein Kennwort verwendet werden. Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. "dbo" (database owner) authentifizieren. 
* **Azure Active Directory-Authentifizierung**, bei der von Azure Active Directory verwaltete Identitäten verwendet werden und die für verwaltete und integrierte Domänen unterstützt wird. Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), [sofern dies möglich ist](https://msdn.microsoft.com/library/ms144284.aspx). Wenn Sie die Azure Active Directory-Authentifizierung verwenden möchten, müssen Sie einen weiteren Serveradministrator mit der Bezeichnung "Azure AD Admin" erstellen, der zum Verwalten von Azure Active Directory-Benutzern und -Gruppen berechtigt ist. Dieser Administrator kann außerdem die gleichen Aufgaben wie ein normaler Serveradministrator ausführen. Unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) finden Sie eine exemplarische Vorgehensweise zum Erstellen eines Azure AD-Administrators, um die Azure Active Directory-Authentifizierung zu aktivieren.

Das Datenbankmodul schließt Verbindungen, die sich länger als 30 Minuten im Leerlauf befinden. Für die Verbindung muss dann eine erneute Anmeldung durchgeführt werden, bevor sie verwendet werden kann. Ständig aktive Verbindungen mit SQL-Datenbank erfordern mindestens alle zehn Stunden eine erneute Autorisierung (per Datenbankmodul). Das Datenbankmodul versucht, die Autorisierung erneut durchzuführen, indem das ursprünglich übermittelte Kennwort verwendet wird. Im Erfolgsfall ist dann keine Benutzereingabe erforderlich. Wenn ein Kennwort in SQL-Datenbank zurückgesetzt wird, wird die Verbindung aus Leistungsgründen nicht erneut authentifiziert. Dies gilt auch, wenn die Verbindung aufgrund des Verbindungspoolings zurückgesetzt wird. Dies unterscheidet sich vom Verhalten einer lokalen SQL Server-Instanz. Wenn das Kennwort seit der ersten Autorisierung der Verbindung geändert wurde, muss die Verbindung beendet und mit dem neuen Kennwort eine neue Verbindung hergestellt werden. Ein Benutzer mit der Berechtigung KILL DATABASE CONNECTION kann eine Verbindung mit SQL-Datenbank explizit beenden, indem er den Befehl [KILL](https://msdn.microsoft.com/library/ms173730.aspx) verwendet.

Benutzerkonten können in der Masterdatenbank erstellt werden und Berechtigungen in allen Datenbanken auf dem Server erhalten. Alternativ können sie in der Datenbank selbst erstellt werden (sogenannte eigenständige Benutzer). Informationen zum Erstellen und Verwalten von Anmeldungen finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md). Verwenden Sie eigenständige Datenbankbenutzer, um die Portabilität und Skalierbarkeit zu verbessern. Weitere Informationen zu eigenständigen Benutzern finden Sie unter [Eigenständige Datenbankbenutzer - machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx) und [Eigenständige Datenbanken](https://technet.microsoft.com/library/ff929071.aspx).

Entsprechend einer bewährten Methode sollte Ihre Anwendung ein anderes Konto zur Authentifizierung verwenden. Auf diese Weise können Sie die Berechtigungen für die Anwendung beschränken und die Risiken schädlicher Aktivitäten reduzieren, falls Ihr Anwendungscode für einen SQL-Injection-Angriff anfällig ist. Der empfohlene Ansatz besteht darin, einen [eigenständigen Datenbankbenutzer](https://msdn.microsoft.com/library/ff929188) zu erstellen. Dadurch kann sich Ihre App direkt bei der Datenbank authentifizieren. Sie können einen eigenständigen Datenbankbenutzer erstellen, der die SQL-Authentifizierung verwendet, indem Sie den folgenden T-SQL-Befehl ausführen, während Sie mit Ihrer Benutzerdatenbank über die Server Admin-Anmeldung verbunden sind:

## <a name="authorization"></a>Autorisierung

Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer in einer Azure SQL-Datenbank ausführen kann. Dies wird durch die [Datenbank-Rollenmitgliedschaften](https://msdn.microsoft.com/library/ms189121) und [Objektebenenberechtigungen](https://msdn.microsoft.com/library/ms191291.aspx) Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden. Weitere Informationen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).

In der Regel benötigen nur Administratoren Zugriff auf die Masterdatenbank. Der routinemäßige Zugriff auf jede Benutzerdatenbank muss über Benutzer für eigenständige Datenbanken geschehen, die in jeder Datenbank erstellt werden und nicht über Administratorrechte verfügen. Bei der Verwendung von Benutzern für eine eigenständige Datenbank müssen Sie keine Anmeldungen für die Masterdatenbank erstellen. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

Darüber hinaus können diese Funktionen zum Beschränken oder Erweitern von Berechtigungen verwendet werden.

* Mithilfe von [Identitätswechsel](https://msdn.microsoft.com/library/vstudio/bb669087) und [Modulsignierung](https://msdn.microsoft.com/library/bb669102) können Berechtigungen ohne Sicherheitsbedenken vorübergehend erhöht werden.
* [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131) lassen sich die Zeilen beschränken, auf die ein Benutzer Zugriff hat.
* [Daten Masking](sql-database-dynamic-data-masking-get-started.md) kann zum Schutz vor unautorisierter Offenlegung von sensiblen Daten verwendet werden.
* [Gespeicherten Prozeduren](https://msdn.microsoft.com/library/ms190782) können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über alle Sicherheitsfunktionen von SQL-Datenbank finden Sie unter [Securing your SQL Database](sql-database-security-overview.md) (Schützen Ihrer SQL-Datenbank).
- Weitere Informationen zu Firewallregeln finden Sie unter [Firewall für die Azure SQL-Datenbank](sql-database-firewall-configure.md).
- Weitere Informationen zu Benutzern und Anmeldungenen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md). 
- Eine Beschreibung der Nutzung von Funktionen zum Schützen von Daten in SQL-Datenbank finden Sie unter [Protecting data within your SQL Database](sql-database-protect-data.md) (Schützen von Daten in Ihrer SQL-Datenbank).
- Eine Beschreibung der proaktiven Überwachung finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing-get-started.md) und [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection-get-started.md).




<!--HONumber=Dec16_HO4-->


