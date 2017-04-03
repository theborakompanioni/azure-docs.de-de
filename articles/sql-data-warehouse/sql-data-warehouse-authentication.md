---
title: Authentifizierung in Azure SQL Data Warehouse | Microsoft Docs
description: Azure Active Directory- (AAD) und SQL Server-Authentifizierung in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: 
author: byham
manager: jhubbard
editor: 
tags: 
ms.assetid: fefaaa75-2d0c-4e5d-aadb-410342d1ad73
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.custom: security
ms.date: 03/21/2017
ms.author: rickbyh;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 9f38d57029e962d1bbe8adae68232baa8a4c575d
ms.lasthandoff: 03/22/2017


---
# <a name="authentication-to-azure-sql-data-warehouse"></a>Authentifizierung in Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Zum Herstellen einer Verbindung mit SQL Data Warehouse müssen Sie zu Authentifizierungszwecken Sicherheitsanmeldeinformationen übergeben. Beim Herstellen einer Verbindung werden bestimmte Verbindungseinstellungen im Rahmen der Einrichtung Ihrer Abfragesitzung konfiguriert.  

Weitere Informationen zur Sicherheit und zum Aktivieren von Verbindungen mit Ihrem Data Warehouse finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL-Authentifizierung
Zum Herstellen einer Verbindung mit SQL Data Warehouse müssen Sie die folgenden Informationen angeben:

* Vollqualifizierter Servername
* SQL-Authentifizierung
* Benutzername
* Kennwort
* Standarddatenbank (optional)

Standardmäßig wird die Verbindung mit der *Masterdatenbank* hergestellt, nicht mit Ihrer Benutzerdatenbank. Sie haben zwei Möglichkeiten, um eine Verbindung mit Ihrer Benutzerdatenbank herzustellen:

* Geben Sie die Standarddatenbank an, wenn Sie Ihren Server mit dem SQL Server-Objekt-Explorer in SSDT oder SSMS registrieren, oder geben Sie die Datenbank in Ihrer Verbindungszeichenfolge für die Anwendung an. Beziehen Sie für eine ODBC-Verbindung beispielsweise den InitialCatalog-Parameter ein.
* Markieren Sie die Benutzerdatenbank, bevor Sie eine Sitzung in SSDT erstellen.

> [!NOTE]
> Die Transact-SQL-Anweisung **USE MyDatabase;** wird nicht zum Ändern der Datenbank für eine Verbindung unterstützt. Eine Anleitung zum Herstellen einer Verbindung mit SQL Data Warehouse über SSDT finden Sie im Artikel [Abfragen mit Visual Studio][Query with Visual Studio].
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Authentifizierung über Azure Active Directory (AAD)
Die [Azure Active Directory][What is Azure Active Directory]-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit Microsoft Azure SQL Data Warehouse unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Mithilfe der Azure Active Directory-Authentifizierung können Sie die Identitäten von Datenbankbenutzern und andere Microsoft-Dienste an einer zentralen Stelle verwalten. Die zentrale ID-Verwaltung bietet einen Ort, an dem die SQL Data Warehouse-Benutzer verwaltet werden und von vereinfacht die Berechtigungsverwaltung. 

### <a name="benefits"></a>Vorteile
Azure Active Directory bietet folgende Vorteile:

* Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
* Es wird einer unkontrollierten Ausbreitung von Benutzeridentitäten über Datenbankserver hinweg Einhalt geboten.
* Es wird eine Kennwortrotation über eine zentrale Stelle ermöglicht.
* Datenbankberechtigungen können mithilfe externer Gruppen (AAD) verwaltet werden.
* Das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen machen das Speichern von Kennwörtern überflüssig.
* Eigenständige Datenbankbenutzer werden zum Authentifizieren von Identitäten auf Datenbankebene verwendet.
* Die tokenbasierte Authentifizierung wird für Anwendungen unterstützt, die sich mit SQL Data Warehouse verbinden.
* Die Multi-Factor Authentication über die universelle Active Directory-Authentifizierung wird für SQL Server Management Studio unterstützt. Eine Beschreibung der Multi-Factor Authentication finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Azure Active Directory ist immer noch relativ neu und weist einige Einschränkungen auf. Um sicherzustellen, dass Azure Active Directory für Ihre Umgebung geeignet ist, lesen Sie [Features und Einschränkungen von Azure AD][Azure AD features and limitations], und beachten Sie insbesondere den Abschnitt „Zusätzliche Überlegungen“.
> 
> 

### <a name="configuration-steps"></a>Konfigurationsschritte
Befolgen Sie diese Schritte zum Konfigurieren der Azure Active Directory-Authentifizierung.

1. Erstellen und Auffüllen eines Azure Active Directory-Verzeichnisses
2. Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
3. Erstellen eines Azure Active Directory-Administrators für Azure SQL Data Warehouse
4. Konfigurieren der Clientcomputer
5. Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind
6. Herstellen einer Verbindung mit Ihrem Data Warehouse unter Verwendung von Azure AD-Identitäten

Azure Active Directory-Benutzer werden derzeit nicht im SSDT-Objekt-Explorer angezeigt. Sie können die Benutzer in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx) anzeigen, um dieses Problem zu umgehen.

### <a name="find-the-details"></a>Suchen der Details
* Führen Sie die ausführlichen Schritte aus. Die ausführlichen Schritte zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung sind fast identisch für die Azure SQL-Datenbank und für Azure SQL Data Warehouse. Befolgen Sie die ausführlichen Schritte im Thema [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](../sql-database/sql-database-aad-authentication.md).
* Erstellen Sie benutzerdefinierte Datenbankrollen, und fügen Sie den Rollen Benutzer hinzu. Weisen Sie anschließend präzise Berechtigungen für die Rollen zu. Weitere Informationen finden Sie unter [Erste Schritte mit Berechtigungen für das Datenbankmodul](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Abfragen Ihres Data Warehouse mit Visual Studio und anderen Anwendungen finden Sie unter [Abfragen mit Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations

