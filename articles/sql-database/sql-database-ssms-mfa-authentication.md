---
title: "SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse | Microsoft Docs"
description: "Verwenden Sie die Multi-Factor Authentication mit SSMS für SQL-Datenbank und SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: fbd6e644-0520-439c-8304-2e4fb6d6eb91
ms.service: sql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/04/2016
ms.author: rick.byham@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3281ce8eba82c2cbfcb34415aa2145419c91332d


---
# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse
Azure SQL-Datenbank und Azure SQL Data Warehouse unterstützen jetzt Verbindungen aus SQL Server Management Studio (SSMS) mithilfe der *universellen Active Directory-Authentifizierung*. Die universelle Active Directory-Authentifizierung ist ein interaktiver Workflow, der die *Azure Multi-Factor Authentication* (MFA) unterstützt. Azure MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Diese Lösung ermöglicht eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen: Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Benutzer können ihre bevorzugte Methode wählen. Eine Beschreibung von Multi-Factor Authentication finden Sie unter [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).

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

## <a name="configuration-steps"></a>Konfigurationsschritte
Die Implementierung der Multi-Factor Authentication erfordert vier grundlegende Schritte.

1. **Konfigurieren eines Active Directory-Verzeichnisses**: Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Hinzufügen eigener Domänennamen zu Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure now supports federation with Windows Server Active Directory (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory)](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx) sowie [Verwalten von Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurieren von MFA** : Eine Schrittanleitung finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 
3. **Konfigurieren von SQL-Datenbank oder SQL Data Warehouse für die Azure AD-Authentifizierung** : Eine Schrittanleitung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
4. **Herunterladen von SSMS** : Laden Sie neueste SSMS-Version (August 2016) von [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)auf den Clientcomputer herunter.

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Herstellen einer Verbindung mit SSMS mithilfe der universellen Authentifizierung
Die folgenden Schritte zeigen, wie Sie unter Verwendung der neuesten Version von SSMS eine Verbindung mit SQL-Datenbank oder SQL Data Warehouse herstellen.

1. Um eine Verbindung mithilfe der universellen Authentifizierung herzustellen, wählen Sie im Dialogfeld **Connect to Server** (Mit Server verbinden) die Option **Universelle Active Directory-Authentifizierung** aus.
   ![1mfa-universal-connect][1]
2. Wie bei SQL-Datenbank und SQL Data Warehouse üblich, müssen Sie auf **Optionen** klicken und im Dialogfeld **Optionen** die Datenbank angeben. Klicken Sie auf **Verbinden**.
3. Wenn das Dialogfeld **Bei Ihrem Konto anmelden** angezeigt wird, geben Sie den Kontonamen und das Kennwort Ihrer Azure Active Directory-Identität ein.
   ![2mfa-sign-in][2]
   
   > [!NOTE]
   > Bei der universellen Authentifizierung mit einem Konto, das keine MFA erfordert, stellen Sie an diesem Punkt die Verbindung her. Für Benutzer, für die MFA erforderlich ist, fahren Sie mit den folgenden Schritten fort.
   > 
   > 
4. Es werden möglicherweise zwei Dialogfelder für die Einrichtung von MFA angezeigt. Dieser einmalige Vorgang richtet sich nach der Administratoreinstellung für MFA und ist daher möglicherweise optional. In einer Domäne mit aktivierter MFA ist dieser Schritt manchmal vordefiniert (beispielsweise muss ein Benutzer sich in der Domäne möglicherweise mit einer Smartcard und einer PIN authentifizieren).  
   ![3mfa-setup][3]
5. Das zweite möglicherweise einmalig angezeigte Dialogfeld ermöglicht es Ihnen, die Details Ihrer Authentifizierungsmethode auszuwählen. Die möglichen Optionen werden von Ihrem Administrator konfiguriert.
   ![4mfa-verify-1][4]
6. Das Azure Active Directory-Verzeichnis sendet die Bestätigungsinformationen an Sie. Wenn Sie den Prüfcode erhalten haben, geben Sie ihn in das Feld **Prüfcode eingeben** ein, und klicken Sie auf **Anmelden**.
   ![5mfa-verify-2][5]

Wenn die Überprüfung abgeschlossen ist, stellt SSMS normalerweise eine Verbindung her, vorausgesetzt, die Anmeldeinformationen sind gültig und der Firewallzugriff ist möglich.

## <a name="next-steps"></a>Nächste Schritte
Erteilen Sie anderen Zugriff auf Ihre Datenbank: [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md)  
Stellen Sie sicher, dass andere eine Verbindung durch die Firewall herstellen können: [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).

[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png




<!--HONumber=Nov16_HO3-->


