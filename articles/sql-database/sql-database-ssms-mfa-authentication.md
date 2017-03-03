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
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/23/2017
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 6e68ff56eaab3de8166711c872bf1b5707dbdf56
ms.openlocfilehash: ce77a059a05f83d74e80c71a87a22de6755b2dbb
ms.lasthandoff: 02/16/2017


---
# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse
Azure SQL-Datenbank und Azure SQL Data Warehouse unterstützen jetzt Verbindungen aus SQL Server Management Studio (SSMS) mithilfe der *universellen Active Directory-Authentifizierung*. Die universelle Active Directory-Authentifizierung ist ein interaktiver Workflow, der die *Azure Multi-Factor Authentication* (MFA) unterstützt. Azure MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Diese Lösung ermöglicht eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen: Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Benutzer können ihre bevorzugte Methode wählen. 

* Eine Beschreibung von Multi-Factor Authentication finden Sie unter [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).
* Konfigurationsschritte finden Sie unter [Konfigurieren der Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

## <a name="multi-factor-options"></a>Optionen der Multi-Factor Authentication

SSMS unterstützt jetzt Folgendes:

* Interaktive MFA mit Azure AD mit der Möglichkeit zur Überprüfung über Popupdialogfelder.
* Nicht interaktive Methoden – die Active Directory-Kennwortauthentifizierung und die integrierte Active Directory-Authentifizierung –, die in vielen verschiedenen Anwendungen (ADO.NET, JDBC, ODBC usw.) eingesetzt werden können. Bei diesen beiden Methoden werden niemals Popupdialogfelder angezeigt.

Wenn ein Benutzerkonto für MFA konfiguriert ist, erfordert der interaktive Authentifizierungsworkflow eine Benutzerinteraktion durch Eingaben in Popupdialogfeldern, den Einsatz von Smartcards usw. Wenn ein Benutzerkonto für MFA konfiguriert ist, muss der Benutzer beim Herstellen einer Verbindung die universelle Azure-Authentifizierung auswählen. Wenn das Benutzerkonto keine MFA erfordert, kann der Benutzer weiterhin die beiden anderen Azure Active Directory-Authentifizierungsoptionen verwenden.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Einschränkungen der universellen Authentifizierung bei SQL-Datenbank und SQL Data Warehouse
* SSMS ist das einzige Tool, das derzeit für MFA über die universelle Active Directory-Authentifizierung aktiviert ist.
* Über die universelle Authentifizierung kann sich nur ein einziges Azure Active Directory-Konto bei einer Instanz von SSMS anmelden. Für die Anmeldung mit einem anderen Azure AD-Konto müssen Sie eine andere SSMS-Instanz verwenden. (Diese Einschränkung gilt nur für die universelle Active Directory-Authentifizierung. Mit der Active Directory-Kennwortauthentifizierung, der integrierten Active Directory-Authentifizierung oder der SQL Server-Authentifizierung können Sie sich bei verschiedenen Servern anmelden.)
* SSMS unterstützt die universelle Active Directory-Authentifizierung für den Objekt-Explorer, den Abfrage-Editor und die Abfragespeichervisualisierung.
* Weder DacFx noch der Schema-Designer unterstützen die universelle Authentifizierung.
* MSA-Konten werden für die universelle Active Directory-Authentifizierung nicht unterstützt.
* Die universelle Active Directory-Authentifizierung wird in SSMS nicht für Benutzer unterstützt, die aus anderen Azure Active Directory-Verzeichnissen in das aktuelle Active Directory-Verzeichnis importiert wurden. Diese Benutzer werden nicht unterstützt, da eine Mandanten-ID erforderlich wäre, um die Konten zu überprüfen. Für diese Bereitstellung gibt es aber keinen Mechanismus.
* Es bestehen keine weiteren Softwareanforderungen für die universelle Active Directory-Authentifizierung – mit einer Ausnahme: Sie müssen eine unterstützte SSMS-Version verwenden.



## <a name="next-steps"></a>Nächste Schritte

* Konfigurationsschritte finden Sie unter [Konfigurieren der Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
* Erteilen Sie anderen Zugriff auf Ihre Datenbank: [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md)  
Stellen Sie sicher, dass andere eine Verbindung durch die Firewall herstellen können: [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).



