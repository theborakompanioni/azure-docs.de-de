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
ms.custom: security-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/23/2017
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: b134999d407195aaf44babb3e4862b96cc1dc1ed
ms.openlocfilehash: b36f0cf8cbf0dfb310d6dd534906ee5391ce4cd5
ms.lasthandoff: 03/02/2017


---
# <a name="configure-azure-sql-database-multi-factor-authentication-for-sql-server-management-studio"></a>Konfigurieren der Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio

In diesem Thema wird beschrieben, wie Sie die Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio konfigurieren. 

Eine Übersicht über die Multi-Factor Authentication in Azure SQL-Datenbank finden Sie unter [Übersicht über die Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Konfigurationsschritte

1. **Konfigurieren eines Azure Active Directory-Verzeichnisses**: Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Hinzufügen eigener Domänennamen zu Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure now supports federation with Windows Server Active Directory (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory)](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx) sowie [Verwalten von Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurieren von MFA**: Eine ausführliche Anleitung finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 
3. **Konfigurieren von SQL-Datenbank oder SQL Data Warehouse für die Azure AD-Authentifizierung**: Eine ausführliche Anleitung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
4. **Herunterladen von SSMS**: Laden Sie die neueste SSMS-Version (mind. vom August 2016) von [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) auf den Clientcomputer herunter.

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

* Eine Übersicht über die Multi-Factor Authentication in Azure SQL-Datenbank finden Sie unter [Übersicht über die Multi-Factor Authentication in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication.md).
* Erteilen Sie anderen Zugriff auf Ihre Datenbank: [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md)  
Stellen Sie sicher, dass andere eine Verbindung durch die Firewall herstellen können: [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).

[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png


