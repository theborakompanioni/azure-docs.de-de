---
title: "Konfigurieren von Multi-Factor Authentication – Azure SQL | Microsoft-Dokumentation"
description: "Verwenden Sie die Multi-Factor Authentication mit SSMS für SQL-Datenbank und SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/17/2017
ms.author: rickbyh
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: fd78b34e8bbefdaa79a73d69ff2a0e3c1c342e98
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Konfigurieren der mehrstufigen Authentifizierung für SQL Server Management Studio und Azure AD

In diesem Thema wird beschrieben, wie Sie die mehrstufige Authentifizierung von Azure Active Directory mit SQL Server Management Studio verwenden. Die mehrstufige Azure AD-Authentifizierung kann bei Verbindungen von SSMS oder SqlPackage.exe mit SQL-Datenbank und Azure SQL Data Warehouse verwendet werden.

Einen Überblick über Multi-Factor Authentication in Azure SQL-Datenbank finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Konfigurationsschritte

1. **Konfigurieren eines Azure Active Directory-Verzeichnisses:** Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Hinzufügen eigener Domänennamen zu Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure now supports federation with Windows Server Active Directory (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory)](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/) und [Verwalten von Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurieren der mehrstufigen Authentifizierung:** Eine ausführliche Anleitung finden Sie unter [Was ist mehrstufige Azure-Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md) und [Bedingter Zugriff (mehrstufige Authentifizierung) mit Azure SQL-Datenbank und Data Warehouse](sql-database-conditional-access.md). (Für eine vollständige Zugangsberechtigung ist Azure Active Directory (Azure AD) im Tarif Premium erforderlich. Begrenzte mehrstufige Authentifizierung ist mit Azure AD im Tarif Standard verfügbar.)
3. **Konfigurieren von SQL-Datenbank oder SQL Data Warehouse für die Azure AD-Authentifizierung**: Eine ausführliche Anleitung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
4. **Herunterladen von SSMS:** Laden Sie die neueste SSMS-Version von [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) auf den Clientcomputer herunter. Verwenden Sie für alle Features in diesem Thema mindestens die Version von Juli 2017, Version 17.2.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Herstellen einer Verbindung mit SSMS mithilfe der universellen Authentifizierung

Die folgenden Schritte zeigen, wie Sie unter Verwendung der neuesten Version von SSMS eine Verbindung mit SQL-Datenbank oder SQL Data Warehouse herstellen.

1. Um eine Verbindung mithilfe der universellen Authentifizierung herzustellen, wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** die Option **Active Directory: universell mit MFA-Unterstützung** aus. (Wenn **Universelle Active Directory-Authentifizierung** angezeigt wird, verwenden Sie nicht die aktuelle Version von SSMS.)  
   ![1mfa-universal-connect][1]  
2. Geben Sie im Feld **Benutzername** die Azure Active Directory-Anmeldeinformationen im Format `user_name@domain.com` ein.  
   ![1mfa-universal-connect-user](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Wenn Sie eine Verbindung als Gastbenutzer herstellen, müssen Sie auf **Optionen** klicken und dann im Dialogfeld **Verbindungseigenschaften** das Feld **AD-Domänenname oder Mandanten-ID** ausfüllen. Weitere Informationen finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).
   ![mfa-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. Wie bei SQL-Datenbank und SQL Data Warehouse üblich, müssen Sie auf **Optionen** klicken und im Dialogfeld **Optionen** die Datenbank angeben. (Wenn der verbundene Benutzer ein Gastbenutzer ist (d.h. joe@outlook.com), müssen Sie das Kontrollkästchen aktivieren und den aktuellen AD-Domänennamen oder die Mandanten-ID als Teil der Optionen hinzufügen. Weitere Informationen finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)]()(sql-database-ssms-mfa-authentication.md). Klicken Sie auf **Verbinden**.  
5. Wenn das Dialogfeld **Bei Ihrem Konto anmelden** angezeigt wird, geben Sie den Kontonamen und das Kennwort Ihrer Azure Active Directory-Identität ein. Es ist kein Kennwort erforderlich, wenn ein Benutzer Mitglied einer Domäne im Verbund mit Azure AD ist.  
   ![2mfa-sign-in][2]  

   > [!NOTE]
   > Bei der universellen Authentifizierung mit einem Konto, das keine MFA erfordert, stellen Sie an diesem Punkt die Verbindung her. Fahren Sie für Benutzer, für die MFA erforderlich ist, mit den folgenden Schritten fort:
   >  
   
6. Es werden möglicherweise zwei Dialogfelder für die Einrichtung von MFA angezeigt. Dieser einmalige Vorgang richtet sich nach der Administratoreinstellung für MFA und ist daher möglicherweise optional. In einer Domäne mit aktivierter MFA ist dieser Schritt manchmal vordefiniert (beispielsweise muss ein Benutzer sich in der Domäne möglicherweise mit einer Smartcard und einer PIN authentifizieren).  
   ![3mfa-setup][3]  
7. Das zweite möglicherweise einmalig angezeigte Dialogfeld ermöglicht es Ihnen, die Details Ihrer Authentifizierungsmethode auszuwählen. Die möglichen Optionen werden von Ihrem Administrator konfiguriert.  
   ![4mfa-verify-1][4]  
8. Das Azure Active Directory-Verzeichnis sendet die Bestätigungsinformationen an Sie. Wenn Sie den Prüfcode erhalten haben, geben Sie ihn in das Feld **Prüfcode eingeben** ein, und klicken Sie auf **Anmelden**.  
   ![5mfa-verify-2][5]  

Wenn die Überprüfung abgeschlossen ist, stellt SSMS normalerweise eine Verbindung her, vorausgesetzt, die Anmeldeinformationen sind gültig und der Firewallzugriff ist möglich.

## <a name="next-steps"></a>Nächste Schritte

* Eine Übersicht über die mehrstufige Authentifizierung in Azure SQL-Datenbank finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).
* Erteilen Sie anderen Zugriff auf Ihre Datenbank: [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md)  
Stellen Sie sicher, dass andere eine Verbindung durch die Firewall herstellen können: [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png


