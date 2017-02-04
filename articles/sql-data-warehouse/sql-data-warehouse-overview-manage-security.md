---
title: Sichern einer Datenbank in SQL Data Warehouse | Microsoft Docs
description: "Tipps für das Sichern einer Datenbank in Azure SQL Data Warehouse für die Entwicklung von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 8fa2f5ca-4cf5-4418-99a2-4dc745799850
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: rortloff;barbkess
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: febecd5f53077c2e3daa0845964e95025b97893b


---
# <a name="secure-a-database-in-sql-data-warehouse"></a>Sichern einer Datenbank in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Dieser Artikel beschreibt die Grundlagen zum Sichern der Azure SQL Data Warehouse-Datenbank. Insbesondere erhalten Sie in diesem Artikel erste Informationen über Ressourcen zum Einschränken des Zugriffs, zum Schützen von Daten und zum Überwachen von Aktivitäten in einer Datenbank.

## <a name="connection-security"></a>Verbindungssicherheit
Verbindungssicherheit bezieht sich darauf, auf welche Weise Sie die Verbindungen zu Ihrer Datenbank mithilfe von Firewall-Regeln und Verbindungsverschlüsselung einschränken und sichern.

Firewall-Regeln werden vom Server und der Datenbank verwendet, um Verbindungsversuche von IP-Adressen abzuwehren, die nicht explizit der weißen Liste hinzugefügt wurden. Damit von Ihrer Anwendung oder von der öffentlichen IP-Adresse Ihres Clientcomputers aus Verbindungen hergestellt werden können, müssen Sie zuerst über das Azure-Portal, über REST-API oder PowerShell eine Firewallregel auf Serverebene erstellen. Eine bewährte Methode besteht darin, die von der Server-Firewall zugelassenen IP-Adressbereiche so weit wie möglich einzuschränken.  Um von Ihrem lokalen Computer aus auf Azure SQL Data Warehouse zuzugreifen, stellen Sie sicher, dass die Firewall im Netzwerk und auf dem lokalen Computer eine ausgehende Kommunikation an TCP-Port 1433 zulässt.  Weitere Informationen finden Sie unter [Azure SQL-Datenbankfirewall][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule] und [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Verbindungen mit Ihrem SQL Data Warehouse sind standardmäßig verschlüsselt.  Das Ändern von Verbindungseinstellungen zum Deaktivieren der Verschlüsselung wird ignoriert.

## <a name="authentication"></a>Authentifizierung
Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL Data Warehouse unterstützt derzeit die SQL Server-Authentifizierung mit Benutzername und Kennwort sowie Azure Active Directory. 

Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich mittels SQL Server-Authentifizierung bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. „dbo“ (database owner) authentifizieren.

Allerdings sollten die Benutzer in Ihrer Organisation als bewährte Methode ein anderes Konto für die Authentifizierung verwenden. Auf diese Weise können Sie die Berechtigungen für die Anwendung beschränken und die Risiken schädlicher Aktivitäten reduzieren, falls Ihr Anwendungscode für einen SQL-Injection-Angriff anfällig ist. 

Um einen authentifizierten SQL Server-Benutzer zu erstellen, stellen Sie mit den Anmeldeinformationen des Serveradministrators eine Verbindung mit der **Masterdatenbank** auf dem Server her, und erstellen Sie eine neue Serveranmeldung.  Außerdem ist es eine gute Idee, einen Benutzer in der Masterdatenbank für Azure SQL Data Warehouse-Benutzer zu erstellen. Das Erstellen eines Benutzers in der Masterdatenbank ermöglicht einem Benutzer, sich mit Tools wie SSMS ohne Angabe eines Datenbanknamens anzumelden.  Außerdem kann er mit dem Objekt-Explorer alle Datenbanken auf einer SQL Server-Instanz anzeigen.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Stellen Sie dann mit den Anmeldedaten für den Serveradministrator eine Verbindung mit der **SQL Data Warehouse-Datenbank** her, und erstellen Sie einen Datenbankbenutzer basierend auf der gerade erstellten Serveranmeldung.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Wenn Benutzer zusätzliche Vorgänge wie das Erstellen von Anmeldenamen oder neuer Datenbanken durchführen, müssen sie auch den Rollen `Loginmanager` und `dbmanager` in der Masterdatenbank zugewiesen werden. Weitere Informationen zu diesen zusätzlichen Rollen sowie zur Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank][Managing databases and logins in Azure SQL Database].  Weitere Informationen zu Azure AD für SQL Data Warehouse finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse mithilfe der Azure Active Directory-Authentifizierung][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Autorisierung
Autorisierung bezieht sich darauf, welche Aufgaben Sie in einer Azure SQL Data Warehouse-Datenbank ausführen können. Dies wird durch die Rollenmitgliedschaften und Berechtigungen Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Azure SQL Data Warehouse vereinfacht die Verwaltung durch Rollen in T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden.

Es gibt Möglichkeiten, Benutzerberechtigungen für die Azure SQL-Datenbank noch weiter einzuschränken:

* Mithilfe von granularen [Berechtigungen][Permissions] können Sie steuern, welche Aufgaben in einzelnen Spalten, Tabellen, Ansichten, Prozeduren und anderen Objekten in der Datenbank ausgeführt werden dürfen. Verwenden Sie präzise Berechtigungen, um die größtmögliche Kontrolle zu haben und die geringstmöglichen Berechtigungen zu erteilen. Das präzise Berechtigungssystem ist etwas kompliziert, und seine effektive Nutzung erfordert etwas Übung.
* [Datenbankrollen][Database roles] können – mit Ausnahme von „db_datareader“ und „db_datawriter“ – dazu verwendet werden, Anwendungsbenutzerkonten mit mehr Berechtigungen oder Verwaltungskonten mit weniger Berechtigungen zu erstellen. Die integrierten festen Datenbankrollen bieten eine einfache Möglichkeit zum Erteilen von Berechtigungen, können jedoch auch dazu führen, dass mehr Berechtigungen als nötig erteilt werden.
* [Gespeicherte Prozeduren][Stored procedures] können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

Die Verwaltung von Datenbanken und logischen Servern über das klassische Azure-Portal oder mit der Azure-Ressourcen-Manager-API wird durch die Rollenzuweisungen Ihres Portal-Benutzerkontos gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal][Role-based access control in Azure Portal].

## <a name="encryption"></a>Verschlüsselung
Azure SQL Data Warehouse Transparent Data Encryption (TDE) ist ein zusätzlicher Schutz vor der Bedrohung durch schädliche Aktivitäten. Hierzu werden die Schritte für die Echtzeitverschlüsselung und -entschlüsselung der ruhenden Daten ausgeführt.  Wenn Sie Ihre Datenbank verschlüsseln, werden zugehörige Sicherungen und Transaktionsprotokolle verschlüsselt, ohne dass Änderungen an Ihren Anwendungen erforderlich sind. TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird. In SQL-Datenbank wird der Datenbankverschlüsselungsschlüssel mit einem integrierten Serverzertifikat geschützt. Das integrierte Serverzertifikat ist für jeden SQL-Datenbank-Server eindeutig. Microsoft führt für diese Zertifikate nach spätestens 90 Tagen automatisch eine Rotation durch. Der von SQL Data Warehouse verwendete Verschlüsselungsalgorithmus ist AES-256. Eine allgemeine Beschreibung von TDE finden Sie unter [Transparente Datenverschlüsselung (TDE)][Transparent Data Encryption].

Sie können Ihre Datenbank mit dem [Azure-Portal][Encryption with Portal] oder mit [T-SQL][Encryption with TSQL] verschlüsseln.

## <a name="next-steps"></a>Nächste Schritte
Informationen und Beispiele zum Herstellen einer Verbindung mit SQL Data Warehouse anhand von verschiedenen Protokollen finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse][Connect to SQL Data Warehouse].

<!--Image references-->

<!--Article references-->
[Connect to SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Database roles]: https://msdn.microsoft.com/library/ms189121.aspx
[Managing databases and logins in Azure SQL Database]: https://msdn.microsoft.com/library/ee336235.aspx
[Permissions]: https://msdn.microsoft.com/library/ms191291.aspx
[Stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Role-based access control in Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure



<!--HONumber=Dec16_HO2-->


