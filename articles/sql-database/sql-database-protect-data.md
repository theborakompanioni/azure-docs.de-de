---
title: "Übersicht über den Datenschutz in Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Schützen von Daten in einer Azure SQL-Datenbank."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: secure and protect
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 12/21/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 67b32b224d86e8d0ed06d8248c3a2c5f5569c5cb


---
# <a name="protecting-data-within-your-sql-database"></a>Schützen von Daten in SQL-Datenbank

SQL-Datenbank schützt Ihre Daten mithilfe von Verschlüsselung.  

## <a name="overview"></a>Übersicht

SQL-Datenbank schützt Ihre Daten, indem die Verschlüsselung für Daten in Bewegung per [Transport Layer Security](https://support.microsoft.com/en-us/kb/3135244), für ruhende Daten per [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242) und für verwendete Daten per [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) ermöglicht wird. Eine Beschreibung der Nutzung dieser Funktionen zum Schützen von Daten in SQL-Datenbank finden Sie unter [Protecting data within your SQL Database](sql-database-protect-data.md) (Schützen von Daten in Ihrer SQL-Datenbank).

Es gibt noch weitere Möglichkeiten zum Verschlüsseln Ihrer Daten:

* [Verschlüsselung auf Zellenebene](https://msdn.microsoft.com/library/ms179331.aspx) werden bestimmte Spalten oder sogar Zellen, die Daten enthalten, mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsselt.
* Wenn Sie ein Hardwaresicherheitsmodul oder eine zentrale Verwaltung Ihrer Verschlüsselungsschlüsselhierarchie benötigen, können Sie den [Azure-Schlüsseltresor mit SQL Server in einer Azure-VM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)verwenden.


SQL-Datenbank schützt Ihre Daten, indem Funktionen für die Überwachung und Bedrohungserkennung bereitgestellt werden. 

### <a name="auditing"></a>Überwachung
Bei der Überwachung von SQL-Datenbank werden Datenbankaktivitäten nachverfolgt, und Sie erhalten Unterstützung bei der Einhaltung von gesetzlichen Bestimmungen, indem Datenbankereignisse in Ihrem Azure Storage-Konto in einem Überwachungsprotokoll aufgezeichnet werden. Dank der Überwachung können Sie die derzeitigen Datenbankaktivitäten verstehen und die Verlaufsaktivität analysieren und untersuchen, um potenzielle Bedrohungen oder vermutete Missbrauchsfälle und Sicherheitsverletzungen zu identifizieren. Weitere Informationen finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing-get-started.md).  

### <a name="threat-detection"></a>Bedrohungserkennung
Mit der Bedrohungserkennung wird die Überwachung vervollständigt, indem der Azure SQL-Datenbank-Dienst um eine zusätzliche Security Intelligence-Ebene erweitert wird. Sie ist rund um die Uhr aktiv, um zu lernen, Profile zu erstellen und anomale Datenbankaktivitäten zu erkennen. Sie werden vor verdächtigen Aktivitäten, potenziellen Sicherheitsrisiken, Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbank-Zugriffsmustern gewarnt. Sie können auf Warnungen reagieren, indem Sie die bereitgestellten informativen und direkt umsetzbaren Anweisungen befolgen. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection-get-started.md).  

## <a name="connection-security"></a>Verbindungssicherheit
Verbindungssicherheit bezieht sich darauf, auf welche Weise Sie die Verbindungen zu Ihrer Datenbank mithilfe von Firewall-Regeln und Verbindungsverschlüsselung einschränken und sichern.

Firewall-Regeln werden vom Server und der Datenbank verwendet, um Verbindungsversuche von IP-Adressen abzuwehren, die nicht explizit der weißen Liste hinzugefügt wurden. Damit Ihre Anwendung oder die öffentliche IP-Adresse Ihres Clientcomputers eine Verbindung mit einer neuen Datenbank herstellen kann, müssen Sie zuerst über das klassische Azure-Portal, über REST-API oder PowerShell eine Firewall-Regel auf Serverebene erstellen. Eine bewährte Methode besteht darin, die von der Server-Firewall zugelassenen IP-Adressbereiche so weit wie möglich einzuschränken. Weitere Informationen finden Sie unter [Firewall für die Azure SQL-Datenbank](https://msdn.microsoft.com/library/ee621782).

Für alle Verbindungen zur Azure SQL-Datenbank ist eine Verschlüsselung (SSL/TLS) erforderlich, und zwar zu jedem Zeitpunkt während der Übertragung von Daten in und aus der Datenbank. Sie müssen in der Verbindungszeichenfolge Ihrer Anwendung Parameter zum Verschlüsseln der Verbindung angeben und dürfen dem Serverzertifikat *nicht* vertrauen (dies wird für Sie erledigt, wenn Sie die Verbindungszeichenfolge aus dem klassischen Azure-Portal kopieren). Andernfalls kann die Identität des Servers von der Verbindung nicht überprüft werden, und diese ist anfällig für „Man-in-the-Middle“-Angriffe. Für ADO.NET-Treiber beispielsweise lauten diese Parameter für Verbindungszeichenfolgen **Encrypt=True** und **TrustServerCertificate=False**. Weitere Informationen finden Sie im Artikel zum Thema [Azure SQL-Datenbank – Verbindungsverschlüsselung und Zertifikatüberprüfung](https://msdn.microsoft.com/library/azure/ff394108#encryption).

## <a name="authentication"></a>Authentifizierung
Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

* **SQL-Authentifizierung**, bei der ein Benutzername und ein Kennwort verwendet werden
* **Azure Active Directory-Authentifizierung**, bei der von Azure Active Directory verwaltete Identitäten verwendet werden und die für verwaltete und integrierte Domänen unterstützt wird

Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. "dbo" (database owner) authentifizieren. Wenn Sie die Azure Active Directory-Authentifizierung verwenden möchten, müssen Sie einen weiteren Serveradministrator mit der Bezeichnung "Azure AD Admin" erstellen, der zum Verwalten von Azure Active Directory-Benutzern und -Gruppen berechtigt ist. Dieser Administrator kann außerdem die gleichen Aufgaben wie ein normaler Serveradministrator ausführen. Unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) finden Sie eine exemplarische Vorgehensweise zum Erstellen eines Azure AD-Administrators, um die Azure Active Directory-Authentifizierung zu aktivieren.

Entsprechend einer bewährten Methode sollte Ihre Anwendung ein anderes Konto zur Authentifizierung verwenden. Auf diese Weise können Sie die Berechtigungen für die Anwendung beschränken und die Risiken schädlicher Aktivitäten reduzieren, falls Ihr Anwendungscode für einen SQL-Injection-Angriff anfällig ist. Der empfohlene Ansatz besteht darin, einen [eigenständigen Datenbankbenutzer](https://msdn.microsoft.com/library/ff929188) zu erstellen. Dadurch kann sich Ihre App direkt bei der Datenbank authentifizieren. Sie können einen eigenständigen Datenbankbenutzer erstellen, der die SQL-Authentifizierung verwendet, indem Sie den folgenden T-SQL-Befehl ausführen, während Sie mit Ihrer Benutzerdatenbank über die Server Admin-Anmeldung verbunden sind:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Wenn Sie einen Azure AD-Administrator erstellt haben, können Sie einen eigenständigen Datenbankbenutzer erstellen, der die Azure Active Directory-Authentifizierung verwendet, indem Sie den folgenden T-SQL-Befehl ausführen, während Sie mit Ihrer Benutzerdatenbank über die Server Admin-Anmeldung verbunden sind:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

In beiden Fällen sollte für die Verbindung mit der Datenbank die Verbindungszeichenfolge Ihrer Anwendung diese Benutzeranmeldeinformationen anstelle der Server Admin-Anmeldung angeben.

Weitere Informationen zur Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](sql-database-manage-logins.md).

## <a name="authorization"></a>Autorisierung
Autorisierung bezieht sich darauf, welche Aufgaben Sie in einer Azure SQL-Datenbank ausführen können. Dies wird durch die Rollenmitgliedschaften und Berechtigungen Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Azure SQL-Datenbank vereinfacht die Verwaltung durch Rollen in T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden.

Es gibt Möglichkeiten, Benutzerberechtigungen für die Azure SQL-Datenbank noch weiter einzuschränken:

* [Datenbankrollen](https://msdn.microsoft.com/library/ms189121) können – mit Ausnahme von „db_datareader“ und „db_datawriter“ – dazu verwendet werden, Anwendungsbenutzerkonten mit mehr Berechtigungen oder Verwaltungskonten mit weniger Berechtigungen zu erstellen.
* Mithilfe von granularen [Berechtigungen](https://msdn.microsoft.com/library/ms191291) können Sie steuern, welche Aufgaben in einzelnen Spalten, Tabellen, Ansichten, Prozeduren und anderen Objekten in der Datenbank ausgeführt werden dürfen.
* Mithilfe von [Identitätswechsel](https://msdn.microsoft.com/library/vstudio/bb669087) und [Modulsignierung](https://msdn.microsoft.com/library/bb669102) können Berechtigungen ohne Sicherheitsbedenken vorübergehend erhöht werden.
* [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131) lassen sich die Zeilen beschränken, auf die ein Benutzer Zugriff hat.
* [Daten Masking](sql-database-dynamic-data-masking-get-started.md) kann zum Schutz vor unautorisierter Offenlegung von sensiblen Daten verwendet werden.
* [Gespeicherten Prozeduren](https://msdn.microsoft.com/library/ms190782) können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

Die Verwaltung von Datenbanken und logischen Servern über das klassische Azure-Portal oder mit der Azure-Ressourcen-Manager-API wird durch die Rollenzuweisungen Ihres Portal-Benutzerkontos gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal](../active-directory/role-based-access-control-configure.md).

## <a name="encryption"></a>Verschlüsselung
Durch [transparente Datenverschlüsselung](http://go.microsoft.com/fwlink/?LinkId=526242)kann die Azure SQL-Datenbank helfen, Ihre Daten zu schützen, indem ruhende oder in Datenbankdateien und Sicherungen gespeicherte Daten verschlüsselt werden. Um Ihre Datenbank zu verschlüsseln, stellen Sie als Datenbankbesitzer eine Verbindung her und führen Folgendes aus:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Es gibt noch weitere Möglichkeiten zum Verschlüsseln von sensiblen Daten:

* [Verschlüsselung auf Zellenebene](https://msdn.microsoft.com/library/ms179331.aspx) werden bestimmte Spalten oder sogar Zellen, die Daten enthalten, mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsselt.
* Wenn Sie ein Hardwaresicherheitsmodul oder eine zentrale Verwaltung Ihrer Verschlüsselungsschlüsselhierarchie benötigen, können Sie den [Azure-Schlüsseltresor mit SQL Server in einer Azure-VM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)verwenden.
* [Immer verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx) (in der Vorschau) macht die Verschlüsselung für Anwendungen erkennbar und ermöglicht es Clients, sensible Daten in Clientanwendungen zu verschlüsseln, ohne die Verschlüsselungsschlüssel mit der SQL-Datenbank freizugeben.

## <a name="auditing"></a>Überwachung
Die Überwachung und Nachverfolgung von Datenbankereignissen kann Sie bei der Einhaltung von gesetzlichen Bestimmungen und der Erkennung von verdächtigen Aktivitäten unterstützen. Mit SQL-Datenbanküberwachung können Sie Ereignisse in der Datenbank in einem Überwachungsprotokoll in Ihrem Azure-Speicherkonto aufzeichnen. SQL-Datenbanküberwachung kann auch in Microsoft Power BI integriert werden, um detaillierte Berichte und Analysen leichter zu ermöglichen. Weitere Informationen finden Sie unter [Erste Schritte mit der SQL-Datenbanküberwachung](sql-database-auditing-get-started.md).

Mit der Bedrohungserkennung von SQL-Datenbank wird die Überwachung um eine zusätzliche Sicherheitsebene erweitert. Damit können Sie auf erkannte Bedrohungen reagieren. Zu diesem Zweck werden Sicherheitswarnungen zu anomalen Aktivitäten bereitgestellt. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Compliance
Zusätzlich zu den oben aufgeführten Features und Funktionalitäten, mit denen Ihre Anwendung eine Reihe von Security Compliance-Anforderungen erfüllen kann, wird Azure SQL-Datenbank außerdem regelmäßigen Überprüfungen unterzogen und ist für eine Reihe von Compliance-Standards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), wo die aktuellste Liste von [Compliance-Zertifizierungen für SQL-Datenbank](https://azure.microsoft.com/support/trust-center/services/) angezeigt wird.


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über alle Sicherheitsfunktionen von SQL-Datenbank finden Sie unter [Securing your SQL Database](sql-database-security-overview.md) (Schützen Ihrer SQL-Datenbank).
- Eine Beschreibung der Nutzung von Zugriffssteuerungsfunktionen in SQL-Datenbank finden Sie unter [Steuern des Zugriffs](sql-database-control-access.md).
- Eine Beschreibung der proaktiven Überwachung finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing-get-started.md) und [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection-get-started.md).



<!--HONumber=Dec16_HO4-->


