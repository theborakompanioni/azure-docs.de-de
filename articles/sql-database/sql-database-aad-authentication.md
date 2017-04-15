---
title: "Azure Active Directory-Authentifizierung – Azure SQL (Übersicht) | Microsoft Docs"
description: "Erfahren Sie, wie Sie Azure Active Directory für die Authentifizierung mit SQL-Datenbank und SQL Data Warehouse verwenden."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/23/2017
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: 154cd7eec65559ec2263a12b8977ec0af0a78743
ms.lasthandoff: 04/10/2017


---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-or-sql-data-warehouse"></a>Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse
Die Azure Active Directory-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit Microsoft Azure SQL-Datenbank und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Mithilfe der Azure AD-Authentifizierung können Sie die Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten an einer zentralen Stelle verwalten. Die zentrale ID-Verwaltung ermöglicht eine einheitliche Verwaltung von Datenbankbenutzern und vereinfacht die Berechtigungsverwaltung. Daraus ergeben sich u. a. die folgenden Vorteile:

* Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
* Es wird einer unkontrollierten Ausbreitung von Benutzeridentitäten über Datenbankserver hinweg Einhalt geboten.
* Es wird eine Kennwortrotation über eine zentrale Stelle ermöglicht.
* Kunden können Datenbankberechtigungen mithilfe von externen Gruppen (AAD) verwalten.
* Durch das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen wird das Speichern von Kennwörtern überflüssig.
* Die Azure Active Directory-Authentifizierung verwendet eigenständige Datenbankbenutzer zum Authentifizieren von Identitäten auf Datenbankebene.
* Die Azure Active Directory-Authentifizierung unterstützt tokenbasierte Authentifizierung für Anwendungen, die sich mit der SQL-Datenbank verbinden.
* Die Azure Active Directory-Authentifizierung unterstützt AD FS (Domänenverbund) sowie native Benutzer-/Kennwortauthentifizierung für ein lokales Azure Active Directory ohne Domänensynchronisierung.  
* Azure AD unterstützt Verbindungen von SQL Server Management Studio, bei denen universelle Active Directory-Authentifizierungsverfahren verwendet werden, zu denen auch die Multi-Factor Authentication (MFA) gehört.  MFA bietet eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen – Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Weitere Informationen finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

Die Konfigurationsschritte schließen die folgenden Verfahren zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung ein:

1. Erstellen und Auffüllen von Azure AD.
2. Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
3. Erstellen eines Azure Active Directory-Administrators für Azure SQL Server oder [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)
4. Konfigurieren der Clientcomputer
5. Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind
6. Herstellen einer Verbindung mit Ihrer Datenbank unter Verwendung von Azure AD-Identitäten

> [!NOTE]
> Informationen zum Erstellen und Auffüllen von Azure AD und zum anschließenden Konfigurieren von Azure AD mit Azure SQL-Datenbank und SQL Data Warehouse finden Sie unter [Konfigurieren von Azure AD mit Azure SQL-Datenbank](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Architektur von Vertrauensstellungen
Das folgende Diagramm bietet eine Übersicht über die Lösungsarchitektur für die Azure AD-Authentifizierung mit Azure SQL-Datenbank. Die gleichen Konzepte gelten für SQL Data Warehouse. Zur Unterstützung des nativen Benutzerkennworts von Azure AD werden nur der Cloudanteil und Azure AD/Azure SQL-Datenbank berücksichtigt. Zur Unterstützung der Verbundauthentifizierung (oder von Benutzername und Kennwort für Windows-Anmeldeinformationen) ist die Kommunikation mit dem AD FS-Block erforderlich. Die Pfeile zeigen die Kommunikationswege.

![Diagramm zur AAD-Authentifizierung][1]

Das folgende Diagramm zeigt die Verbund-, Vertrauensstellungs- und Hostbeziehungen, die einem Client durch Übermittlung eines Tokens die Verbindungsherstellung mit einer Datenbank ermöglichen. Das Token wird von einem Azure Active Directory authentifiziert, und die Datenbank vertraut dem Token. Bei „Customer1“ kann es sich um ein Azure Active Directory mit nativen Benutzern oder um ein Azure AD mit Verbundbenutzern handeln. „Customer2“ stellt eine mögliche Lösung einschließlich importierter Benutzer dar. In diesem Beispiel stammen diese aus einem dem Verbund angehörenden Azure Active Directory, und AD FS wird mit Azure Active Directory synchronisiert. Wichtig: Für den Zugriff auf eine Datenbank mithilfe der Azure AD-Authentifizierung muss das Abonnement, das als Host fungiert, dem Azure AD zugeordnet sein. Das gleiche Abonnement muss auch verwendet werden, um die SQL Server-Instanz zu erstellen, auf der die Azure SQL-Datenbank oder das SQL Data Warehouse gehostet werden.

![Abonnementbeziehung][2]

## <a name="administrator-structure"></a>Administratorstruktur
Bei Verwendung der Azure AD-Authentifizierung sind zwei Administratorkonten für den SQL-Datenbank-Server vorhanden: der ursprüngliche SQL Server-Administrator und der Azure AD-Administrator. Die gleichen Konzepte gelten für SQL Data Warehouse. Nur der auf einem Azure AD-Konto basierende Administrator kann den ersten eigenständigen Azure AD-Datenbankbenutzer in einer Benutzerdatenbank erstellen. Das Konto für die Azure AD-Administratoranmeldung kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn es sich bei dem Administrator um ein Gruppenkonto handelt, kann es von einem beliebigen Gruppenmitglied verwendet werden, sodass mehrere Azure AD-Administratoren die SQL Server-Instanz verwalten können. Die Verwendung eines Gruppenkontos für den Administrator ermöglicht es Ihnen, Gruppenmitglieder in Azure AD zentral hinzuzufügen und zu entfernen, ohne die Benutzer oder Berechtigungen in SQL-Datenbank zu ändern. Es kann jeweils nur ein Azure AD-Administrator (ein Benutzer oder eine Gruppe) konfiguriert werden.

![Administratorstruktur][3]

## <a name="permissions"></a>Berechtigungen
Um neue Benutzer zu erstellen, müssen Sie über die Berechtigung `ALTER ANY USER` in der Datenbank verfügen. Die Berechtigung `ALTER ANY USER` kann jedem Datenbankbenutzer gewährt werden. Die Berechtigung `ALTER ANY USER` haben auch Serveradministratorkonten inne, ebenso wie Datenbankbenutzer mit der Berechtigung `CONTROL ON DATABASE` oder `ALTER ON DATABASE` für diese Datenbank sowie Mitglieder der Datenbankrolle `db_owner`.

Für die Erstellung eines eigenständigen Datenbankbenutzers in Azure SQL-Datenbank oder SQL Data Warehouse müssen Sie unter Verwendung einer Azure AD-Identität eine Verbindung mit der Datenbank herstellen. Um den ersten eigenständigen Datenbankbenutzer zu erstellen, müssen Sie unter Verwendung eines Azure AD-Administrators (dieser ist der Besitzer der Datenbank) eine Verbindung mit der Datenbank herstellen. Dies wird nachstehend in den Schritten 4 und 5 gezeigt. Eine Azure AD-Authentifizierung ist nur möglich, wenn der Azure AD-Administrator für den Azure SQL-Datenbankserver oder den SQL Data Warehouse-Server erstellt wurde. Falls der Azure Active Directory-Administrator vom Server entfernt wurde, können vorhandene Azure Active Directory-Benutzer, die zuvor in SQL Server erstellt wurden, nicht mehr mithilfe ihrer Azure Active Directory-Anmeldeinformationen auf die Datenbank zugreifen.

## <a name="azure-ad-features-and-limitations"></a>Funktionen und Einschränkungen von Azure AD
Die folgenden Elemente von Azure AD können in Azure SQL Server und SQL Data Warehouse bereitgestellt werden:

* Systemeigene Elemente: ein in Azure AD erstelltes Element in der verwalteten Domäne oder einer Kundendomäne. Weitere Informationen finden Sie unter [Verwenden von benutzerdefinierten Domänennamen zum Vereinfachen des Anmeldevorgangs für Benutzer](../active-directory/active-directory-add-domain.md).
* Mitglieder der Verbunddomäne: in Azure AD mit einer Verbunddomäne erstellte Mitglieder. Weitere Informationen finden Sie unter [Microsoft Azure now supports federation with Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/)(Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory).
* Aus anderen Azure AD-Instanzen importierte Mitglieder, die native Mitglieder oder Mitglieder der Verbunddomäne sind.
* Active Directory-Gruppen, die als Sicherheitsgruppen erstellt wurden.

Microsoft-Konten (beispielsweise "outlook.com", "hotmail.com", "live.com") oder andere Gastkonten (z. B. "gmail.com", "yahoo.com") werden nicht unterstützt. Wenn Sie sich mit dem Konto und Kennwort bei [https://login.live.com](https://login.live.com) anmelden können, verwenden Sie ein Microsoft-Konto. Dies wird für die Azure AD-Authentifizierung für Azure SQL-Datenbank oder Azure SQL Data Warehouse nicht unterstützt.

## <a name="connecting-using-azure-ad-identities"></a>Herstellen einer Verbindung mit Azure AD-Identitäten

Die Azure Active Directory-Authentifizierung unterstützt die folgenden Methoden der Verbindungsherstellung mit einer Datenbank unter Verwendung von Azure AD-Identitäten:

* Integrierte Windows-Authentifizierung
* Azure AD-Prinzipalname und Kennwort
* Anwendungstokenauthentifizierung

### <a name="additional-considerations"></a>Zusätzliche Überlegungen

* Um die Verwaltungsmöglichkeiten zu verbessern, wird empfohlen, eine dedizierte Azure AD-Gruppe als Administrator bereitzustellen.   
* Es kann immer nur ein einzelner Azure AD-Administrator (Benutzer oder Gruppe) für eine Azure SQL Server- oder Azure SQL Data Warehouse-Instanz konfiguriert werden.   
* Nur ein Azure AD-Administrator für SQL Server kann unter Verwendung eines Azure Active Directory-Kontos erstmals eine Verbindung mit der Azure SQL Server- oder Azure SQL Data Warehouse-Instanz herstellen. Der Active Directory-Administrator kann weitere Azure AD-Datenbankbenutzer konfigurieren.   
* Es wird empfohlen, das Verbindungstimeout auf 30 Sekunden festzulegen.   
* SQL Server 2016 Management Studio und SQL Server Data Tools für Visual Studio 2015 (ab Version 14.0.60311.1April 2016) unterstützen die Azure Active Directory-Authentifizierung. (Die Azure AD-Authentifizierung wird vom **.NET Framework-Datenanbieter für SqlServer**ab .NET Framework 4.6 unterstützt.) Daher können die neuesten Versionen dieser Tools und Datenebenenanwendungen (DAC und BACPAC) die Azure AD-Authentifizierung verwenden.   
* [ODBC Version 13.1`bcp.exe` unterstützt die Azure Active Directory-Authentifizierung, aber für ](https://www.microsoft.com/download/details.aspx?id=53339) kann keine Verbindung per Azure Active Directory-Authentifizierung hergestellt werden, da hierfür ein älterer ODBC-Anbieter verwendet wird.   
* `sqlcmd` unterstützt die Azure Active Directory-Authentifizierung ab Version 13.1, verfügbar im [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server Data Tools für Visual Studio 2015 erfordert mindestens die Data Tools-Version von April 2016 (Version 14.0.60311.1). Azure AD-Benutzer werden derzeit nicht im SSDT-Objekt-Explorer angezeigt. Sie können die Benutzer in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx) anzeigen, um dieses Problem zu umgehen.   
* [Microsoft JDBC-Treiber 6.0 für SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) unterstützt die Azure AD-Authentifizierung. Siehe auch [Einstellen der Verbindungseigenschaften](https://msdn.microsoft.com/library/ms378988.aspx).   
* PolyBase kann sich nicht per Azure AD-Authentifizierung authentifizieren.   
* Die Azure AD-Authentifizierung wird für SQL-Datenbank auf den Blättern **Datenbank importieren** und **Datenbank exportieren**des Azure-Portals unterstützt. Import- und Exportvorgänge mit Azure AD-Authentifizierung werden auch per PowerShell-Befehl unterstützt.   

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Erstellen und Auffüllen von Azure AD und zum anschließenden Konfigurieren von Azure AD mit Azure SQL-Datenbank finden Sie unter [Konfigurieren von Azure AD mit Azure SQL-Datenbank](sql-database-aad-authentication-configure.md).
- Eine Übersicht über den Zugriff und die Steuerung in SQL-Datenbank finden Sie unter [Azure SQL Database access control](sql-database-control-access.md) (SQL-Datenbank – Zugriffssteuerung).
- Eine Übersicht über Anmeldungen, Benutzer und Datenbankrollen in SQL-Datenbank finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md).
- Weitere Informationen zu Datenbankprinzipalen finden Sie unter [Prinzipale](https://msdn.microsoft.com/library/ms181127.aspx).
- Weitere Informationen zu Datenbankrollen finden Sie unter [Datenbankrollen](https://msdn.microsoft.com/library/ms189121.aspx).
- Weitere Informationen zu Firewallregeln in SQL-Datenbank finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](sql-database-firewall-configure.md).
- Ein Tutorial mit SQL Server-Authentifizierung finden Sie unter [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md).
- Ein Tutorial mit Azure Active Directory-Authentifizierung finden Sie unter [Azure AD-Authentifizierung und -Autorisierung](sql-database-control-access-aad-authentication-get-started.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png


