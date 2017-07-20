---
title: "Bedingter Zugriff – Azure SQL-Datenbank und Data Warehouse | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie der bedingte Zugriff für Azure SQL-Datenbank und Data Warehouse konfiguriert wird."
services: sql-database
author: BYHAM
manager: jhubbard
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: rickbyh
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 0dcec61c03a84197e2c351761c743683caa98a06
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017

---


# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Bedingter Zugriff (MFA) mit Azure SQL-Datenbank und Data Warehouse  

Sowohl die SQL-Datenbank als auch SQL Data Warehouse unterstützen den bedingten Zugriff von Microsoft. Die folgenden Schritte zeigen, wie die SQL-Datenbank zum Erzwingen einer Richtlinie für bedingten Zugriff konfiguriert wird.  

## <a name="prerequisites"></a>Voraussetzungen  
- Sie müssen die SQL-Datenbank oder SQL Data Warehouse zur Unterstützung für die Azure Active Directory-Authentifizierung konfigurieren. Informationen zu den jeweiligen Schritten finden Sie unter [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit der SQL-Datenbank oder SQL Data Warehouse](sql-database-aad-authentication-configure.md).  
- Wenn die mehrstufige Authentifizierung aktiviert ist, müssen Sie eine Verbindung mit einem unterstützten Tool wie der neuesten Version von SSMS herstellen. Weitere Informationen finden Sie unter [Konfigurieren der mehrstufigen Authentifizierung in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Konfigurieren der Zertifizierungsstelle für Azure SQL DB/DW  
1.  Melden Sie sich beim Portal an, und wählen Sie **Azure Active Directory** und dann **Bedingter Zugriff**. Weitere Informationen finden Sie unter [Technische Referenz zum bedingten Azure Active Directory-Zugriff](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference).  
  ![Blatt „Bedingter Zugriff“](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2.  Klicken Sie auf dem Blatt **Richtlinien für bedingten Zugriff** auf **Neue Richtlinie**, geben Sie einen Namen ein, und klicken Sie dann auf **Regeln konfigurieren**.  
3.  Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**, aktivieren Sie **Benutzer und Gruppen auswählen**, und wählen Sie dann den Benutzer oder die Gruppe für den bedingten Zugriff aus. Klicken Sie auf **Auswählen** und dann auf **Fertig**, um Ihre Auswahl zu bestätigen.  
  ![Auswählen von Benutzern und Gruppen](./media/sql-database-conditional-access/select-users-and-groups.png)  

4.  Wählen Sie **Cloud-Apps**, und klicken Sie auf **Apps auswählen**. Alle für den bedingten Zugriff verfügbaren Apps werden angezeigt. Wählen Sie **Azure SQL-Datenbank**, und klicken Sie unten auf **Auswählen** und dann auf **Fertig**.  
  ![Auswählen der SQL-Datenbank](./media/sql-database-conditional-access/select-sql-database.png)  
  Wenn Sie die im dritten Screenshot dargestellte Option **Azure SQL-Datenbank** nicht finden können, führen Sie folgende Schritte durch:   
  - Melden Sie sich über SSMS mit einem AAD-Administratorkonto bei Ihrer Azure SQL DB/DW-Instanz an.  
  - Führen Sie `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER` aus.  
  - Melden Sie sich bei AAD an und stellen Sie sicher, dass Azure SQL-Datenbank und Data Warehouse in den Anwendungen in AAD aufgeführt sind.  

5.  Wählen Sie **Zugriffskontrollen** und **Gewähren**, und aktivieren Sie dann die Richtlinie, die angewendet werden soll. In diesem Beispiel wählen wir **Mehrstufige Authentifizierung anfordern**.  
  ![Auswählen von „Zugriff gewähren“](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Zusammenfassung  
Die ausgewählte Anwendung (Azure SQL-Datenbank), durch die eine Verbindung mit Azure SQL DB/DW über Azure AD Premium hergestellt werden kann, erzwingt nun die ausgewählte Richtlinie für bedingten Zugriff. Hierfür muss die Option **Mehrstufige Authentifizierung anfordern** aktiviert werden.  
Bei Fragen zu Azure SQL-Datenbank und Data Warehouse bezüglich der mehrstufigen Authentifizierung wenden Sie sich unter MFAforSQLDB@microsoft.com an uns.  

## <a name="next-steps"></a>Nächste Schritte  

Ein Tutorial finden Sie unter [Schützen von Azure SQL-Datenbank](sql-database-security-tutorial.md).

