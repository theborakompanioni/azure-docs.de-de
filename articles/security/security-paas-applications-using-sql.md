---
title: "Schützen webbasierter und mobiler PaaS-Anwendungen mit SQL-Datenbank und SQL Data Warehouse | Microsoft-Dokumentation"
description: " Erfahren Sie etwas zu den Best Practices im Hinblick auf die Sicherheit bei Azure SQL-Datenbank und SQL Data Warehouse zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: be00c1427d57b96506ec8b0ac881b7c1bd09e4de
ms.lasthandoff: 03/22/2017


---
# <a name="securing-paas-web-and-mobile-applications-using-sql-database-and-sql-data-warehouse"></a>Schützen webbasierter und mobiler PaaS-Anwendungen mit SQL-Datenbank und SQL Data Warehouse

In diesem Artikel erläutern wir eine Sammlung empfohlener Vorgehensweisen im Hinblick auf die Sicherheit bei [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) und [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit Azure und den Erfahrungen von Kunden wie Ihnen abgeleitet.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL-Datenbank und SQL Data Warehouse
[Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md) und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) stellen einen relationalen Datenbankdienst für Ihre internetbasierten Anwendungen bereit. Sehen wir uns nun Dienste an, mit deren Hilfe Anwendungen und Daten bei Verwendung von Azure SQL-Datenbank und SQL Data Warehouse in einer PaaS-Bereitstellung geschützt werden:

- Azure Active Directory-Authentifizierung (anstelle von SQL Server-Authentifizierung)
- Azure SQL-Firewall
- Transparent Data Encryption (TDE)

## <a name="best-practices"></a>Bewährte Methoden

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Verwenden eines zentralisierten Identitätsrepositorys zur Authentifizierung und Autorisierung

Azure SQL-Datenbanken können so konfiguriert werden, dass sie einen von zwei Authentifizierungstypen verwenden:

- Die **SQL-Authentifizierung** verwendet einen Benutzernamen und ein Kennwort. Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich bei jeder Datenbank auf diesem Server als Datenbankbesitzer authentifizieren.

- **Azure Active Directory-Authentifizierung** verwendet von Azure Active Directory verwaltete Identitäten und wird für verwaltete und integrierte Domänen unterstützt. Zur Verwendung der Azure Active Directory-Authentifizierung müssen Sie einen weiteren Serveradministrator mit der Bezeichnung „Azure AD-Administrator“ erstellen, der zum Verwalten von Azure Active Directory-Benutzern und -Gruppen berechtigt ist. Dieser Administrator kann außerdem die gleichen Aufgaben wie ein normaler Serveradministrator ausführen.

Die [Azure Active Directory](../active-directory/develop/active-directory-authentication-scenarios.md)-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit Azure SQL-Datenbank und SQL Data Warehouse unter Verwendung von Identitäten in Azure Active Directory (AD). Azure AD stellt eine Alternative zur SQL Server-Authentifizierung bereit, damit Sie die Verbreitung von Benutzeridentitäten über Datenbankserver hinweg beenden können. Die Azure AD-Authentifizierung ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten. Die zentrale ID-Verwaltung ermöglicht eine einheitliche Verwaltung von Datenbankbenutzern und vereinfacht die Berechtigungsverwaltung.  

Vorteile der Verwendung der Azure AD-Authentifizierung anstelle der SQL-Authentifizierung:

- Über eine zentrale Stelle wird eine Kennwortrotation ermöglicht.
- Datenbankberechtigungen können mithilfe externer Azure AD-Gruppen verwaltet werden.
- Das Aktivieren der integrierten Windows-Authentifizierung und anderer von Azure AD unterstützte Authentifizierungsformen machen das Speichern von Kennwörtern überflüssig.
- Eigenständige Datenbankbenutzer werden zum Authentifizieren von Identitäten auf Datenbankebene verwendet.
- Unterstützt tokenbasierte Authentifizierung für Anwendungen, die eine Verbindung mit SQL-Datenbank herstellen.
- Unterstützt ADFS (Domänenverbund) sowie native Benutzer-/Kennwortauthentifizierung für eine lokale Azure AD-Instanz ohne Domänensynchronisierung.
- Unterstützt Verbindungen von SQL Server Management Studio, bei denen universelle Active Directory-Authentifizierungsverfahren verwendet werden, zu denen auch die [Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) gehört. MFA bietet eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen – Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Weitere Informationen finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Weitere Informationen zur Azure AD-Authentifizierung erhalten Sie unter:

- [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](../sql-database/sql-database-aad-authentication.md)
- [Authentifizierung in Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Tokenbasierte Authentifizierungsunterstützung für Azure SQL DB mithilfe der Azure AD-Authentifizierung](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Um sicherzustellen, dass Azure Active Directory für Ihre Umgebung geeignet ist, lesen Sie [Funktionen und Einschränkungen von Azure AD](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), und beachten Sie insbesondere den Abschnitt „Zusätzliche Überlegungen“.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Beschränken des Zugriffs auf Grundlage der IP-Adresse
Sie können Firewallregeln erstellen, die die Bereiche der zulässigen IP-Adressen festlegen. Diese Regeln können auf Server- und auf Datenbankebene ausgerichtet werden. Wir empfehlen, nach Möglichkeit Firewallregeln auf Datenbankebene zu verwenden, um die Sicherheit und die Portabilität der Datenbank zu verbessern. Firewallregeln auf Serverebene eignen sich am besten für Administratoren und bei vielen Datenbanken mit identischen Zugriffsanforderungen, wenn Sie die Datenbanken nicht einzeln konfigurieren möchten.

Die standardmäßigen Einschränkungen der Quell-IP-Adresse von SQL-Datenbank erlauben den Zugriff von allen Azure-Adressen (einschließlich anderer Abonnements und Mandanten). Sie können dies einschränken und nur Ihren IP-Adressen den Zugriff auf die Instanz erlauben. Auch mit den Einschränkungen Ihrer SQL-Firewall und der IP-Adresse ist dennoch eine strenge Authentifizierung erforderlich. Weitere Informationen finden Sie in den Empfehlungen weiter oben in diesem Artikel.

Weitere Informationen zu Azure SQL-Firewall- und IP-Einschränkungen finden Sie unter:

- [Zugriffssteuerung bei Azure SQL-Datenbank](../sql-database/sql-database-control-access.md)
- [Konfigurieren von Firewallregeln für Azure SQL-Datenbank – Übersicht](../sql-database/sql-database-firewall-configure.md)
- [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten
[Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/azure/bb934049) verschlüsselt SQL Server-, Azure SQL-Datenbank- und Azure SQL Data Warehouse-Datendateien. Dies wird als Verschlüsselung ruhender Daten bezeichnet. Sie können verschiedene Vorsichtsmaßnahmen zum Schützen der Datenbank treffen, z.B. Entwerfen eines sicheren Systems, Verschlüsseln vertraulicher Datenbestände und Erstellen einer Firewall für die Datenbankserver. In einem Szenario, in dem physische Medien (wie etwa Festplatten oder Sicherungsbänder) gestohlen werden, kann eine böswillige Partei jedoch die Datenbank wiederherstellen oder anfügen und die Daten durchsuchen. Eine Lösung besteht darin, die vertraulichen Daten in der Datenbank zu verschlüsseln und die Schlüssel zu schützen, die zum Verschlüsseln der Daten mit einem Zertifikat verwendet werden. Dadurch wird verhindert, dass Personen ohne Schlüssel die Daten verwenden. Diese Art des Schutzes muss allerdings im Voraus geplant werden.

TDE schützt ruhende Daten, also die Daten und die Protokolldateien. Sie ermöglicht die Einhaltung von Gesetzen, Bestimmungen und Richtlinien, die in vielen Branchen etabliert sind. Dadurch können Softwareentwickler Daten mithilfe von in der Branche üblichen Verschlüsselungsalgorithmen ohne Änderung vorhandener Anwendungen verschlüsseln.

TDE muss verwendet werden, wenn Bestimmungen eine derartige Verschlüsselung explizit festlegen. Beachten Sie jedoch, dass dies einen Angreifer, der den normalen Zugriffspfad verwendet, nicht aufhalten wird. TDE wird zum Schutz vor dem höchst unwahrscheinlichen Fall eingesetzt, dass Sie eine zusätzliche Verschlüsselung auf Anwendungsebene verwenden müssen, entweder über eine mit Azure SQL bereitgestellte Verschlüsselung für Zeilen und Spalten oder über eine Verschlüsselung auf Anwendungsebene.

Die Verschlüsselung auf Anwendungsebene muss auch für selektive Daten verwendet werden. Sorgen in Bezug auf die Datenhoheit können durch das Verschlüsseln von Daten mit einem Schlüssel verringert werden, der im richtigen Land aufbewahrt wird. Dadurch wird sogar verhindert, dass versehentliche Datenübertragungen Probleme verursachen, da die Daten ohne den Schlüssel nicht entschlüsselt werden können. Voraussetzung dafür ist jedoch, dass ein starker Algorithmus (z.B. AES-256) verwendet wird.

Die in Azure SQL bereitgestellte Verschlüsselung für Zeilen und Spalten kann nur ausgeführt werden, um autorisierten Benutzern ([RBAC](../active-directory/role-based-access-built-in-roles.md)) Zugriff zu erteilen, und verhindert, dass Benutzer mit niedrigeren Rechten Spalten oder Zeilen anzeigen können.

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel stellt eine Sammlung empfohlener Vorgehensweisen im Hinblick auf die Sicherheit bei der SQL-Datenbank und SQL Data Warehouse zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen vor. Weitere Informationen zum Schutz Ihrer PaaS-Bereitstellungen finden Sie unter:

- [Schützen von PaaS-Bereitstellungen](security-paas-deployments.md)
- [Securing PaaS web and mobile applications using Azure App Services](security-paas-applications-using-app-services.md) (Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Services)

