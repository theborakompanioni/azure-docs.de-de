---
title: "Multi-Factor Authentication – Azure SQL | Microsoft-Dokumentation"
description: "Verwenden Sie die Multi-Factor Authentication mit SSMS für SQL-Datenbank und SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: fbd6e644-0520-439c-8304-2e4fb6d6eb91
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 04/07/2017
ms.author: rickbyh
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: 33282f1ce0c31056524359aeb42399be550334e2
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)
Azure SQL-Datenbank und Azure SQL Data Warehouse unterstützen Verbindungen aus SQL Server Management Studio (SSMS) mithilfe der *universellen Active Directory-Authentifizierung*. 

- Die universelle Active Directory-Authentifizierung unterstützt die zwei nicht interaktiven Authentifizierungsmethoden (Active Directory-Kennwortauthentifizierung und integrierte Active Directory-Authentifizierung). Die nicht interaktiven Methoden – die Active Directory-Kennwortauthentifizierung und die integrierte Active Directory-Authentifizierung – können in vielen verschiedenen Anwendungen (ADO.NET, JDBC, ODBC usw.) eingesetzt werden. Bei diesen beiden Methoden werden niemals Popupdialogfelder angezeigt.

- Die universelle Active Directory-Authentifizierung ist eine interaktive Methode, die auch die *Azure Multi-Factor Authentication* (MFA) unterstützt. Azure MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Diese Lösung ermöglicht eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen: Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Benutzer können ihre bevorzugte Methode wählen. Bei der interaktiven MFA mit Azure AD kann ein Popupdialogfeld zur Überprüfung geöffnet werden.

Eine Beschreibung von Multi-Factor Authentication finden Sie unter [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).
Konfigurationsschritte finden Sie unter [Konfigurieren der Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Name der Azure AD-Domäne oder Mandanten-ID-Parameter   

Ab [SSMS Version 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) können Benutzer, die aus anderen Azure Active Directorys in Active Directory importiert wurden, beim Herstellen einer Verbindung den Namen der Azure AD-Domäne oder die Mandanten-ID angeben. Dadurch kann bei der **universellen Active Directory-Authentifizierung** die richtige Authentifizierungsstelle identifiziert werden. Diese Option ist für die Unterstützung von Microsoft-Konten (MSA) wie outlook.com, hotmail.com oder live.com erforderlich. Alle diese Benutzer, die mithilfe der universellen Authentifizierung authentifiziert werden sollen, müssen den Namen ihrer Azure AD-Domäne oder ihre Mandanten-ID eingeben. Dieser Parameter stellt den aktuellen Namen der Azure AD-Domäne/die aktuelle Mandanten-ID dar, mit denen der Azure-Server verknüpft ist. Wenn Azure Server beispielsweise mit der Azure AD-Domäne `contosotest.onmicrosoft.com` verbunden ist, in der Benutzer `joe@contosodev.onmicrosoft.com` als importierter Benutzer aus Azure AD-Domäne `contosodev.onmicrosoft.com` gehostet wird, lautet der erforderliche Domänenname für die Authentifizierung dieses Benutzers `contosotest.onmicrosoft.com`. Wenn der Benutzer ein nativer Benutzer des mit Azure Server verknüpften Azure AD ist und kein MSA-Konto darstellt, sind weder Domänenname noch Mandanten-ID erforderlich. Füllen Sie zur Eingabe des Parameters (ab SSMS Version 17) im Dialogfeld **Datenbank verbinden** das Dialogfeld aus. Wählen Sie dazu **Universelle Active Directory-Authentifizierung** aus, klicken Sie auf **Optionen**, klicken Sie auf die Registerkarte **Verbindungseigenschaften**, aktivieren Sie das Kontrollkästchen **AD-Domänenname oder Mandanten-ID**, und geben Sie die Authentifizierungsstelle an, z.B. den Domänennamen (**contosotest.onmicrosoft.com**) oder die GUID der Mandanten-ID.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Einschränkungen der universellen Authentifizierung bei SQL-Datenbank und SQL Data Warehouse
* SSMS ist das einzige Tool, das derzeit für MFA über die universelle Active Directory-Authentifizierung aktiviert ist.
* Über die universelle Authentifizierung kann sich nur ein einziges Azure Active Directory-Konto bei einer Instanz von SSMS anmelden. Für die Anmeldung mit einem anderen Azure AD-Konto müssen Sie eine andere SSMS-Instanz verwenden. (Diese Einschränkung gilt nur für die universelle Active Directory-Authentifizierung. Mit der Active Directory-Kennwortauthentifizierung, der integrierten Active Directory-Authentifizierung oder der SQL Server-Authentifizierung können Sie sich bei verschiedenen Servern anmelden.)
* SSMS unterstützt die universelle Active Directory-Authentifizierung für den Objekt-Explorer, den Abfrage-Editor und die Abfragespeichervisualisierung.
* Weder DacFx noch der Schema-Designer unterstützen die universelle Authentifizierung.
* Es bestehen keine weiteren Softwareanforderungen für die universelle Active Directory-Authentifizierung – mit einer Ausnahme: Sie müssen eine unterstützte SSMS-Version verwenden.


## <a name="next-steps"></a>Nächste Schritte

* Konfigurationsschritte finden Sie unter [Konfigurieren der Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
* Erteilen Sie anderen Zugriff auf Ihre Datenbank: [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md)  
Stellen Sie sicher, dass andere eine Verbindung durch die Firewall herstellen können: [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).
* Informationen zur Azure AD-Konfiguration und -Verwaltung finden Sie unter [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](sql-database-aad-authentication-configure.md).



