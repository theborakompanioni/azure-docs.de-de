---
title: "Übersicht über die Sicherheit der Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Sicherheit von Azure SQL-Datenbank und SQL Server, einschließlich der Unterschiede zwischen einem SQL-Server in der Cloud und einem SQL-Server vor Ort hinsichtlich Authentifizierung, Autorisierung, Verbindungssicherheit, Verschlüsselung und Compliance."
services: sql-database
documentationcenter: 
author: tmullaney
manager: jhubbard
editor: 
ms.assetid: a012bb85-7fb4-4fde-a2fc-cf426c0a56bb
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/05/2017
ms.author: thmullan;jackr
ms.translationtype: HT
ms.sourcegitcommit: 07e5e15f4f4c4281a93c8c3267c0225b1d79af45
ms.openlocfilehash: de2b70a6a6bc557902fbe1fe1391696ec9998d70
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="securing-your-sql-database"></a>Sichern der SQL-Datenbank

Dieser Artikel beschreibt die Grundlagen zum Sichern der Datenebene einer Anwendung mit der Azure SQL-Datenbank. In diesem Artikel erhalten Sie vor allem die ersten Informationen zu den Ressourcen zum Schützen von Daten, zum Steuern des Zugriffs und für die proaktive Überwachung. 

Eine vollständige Übersicht der für alle Varianten von SQL verfügbaren Sicherheitsfunktionen finden Sie unter [Sicherheitscenter für SQL Server Database Engine und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589). Weitere Informationen finden Sie auch im [technischen Whitepaper zu Sicherheit und Azure SQL-Datenbank](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>Schützen von Daten
SQL-Datenbank schützt Ihre Daten, indem die Verschlüsselung für Daten in Bewegung per [Transport Layer Security](https://support.microsoft.com/kb/3135244), für ruhende Daten per [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) und für verwendete Daten per [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) ermöglicht wird. 

> [!IMPORTANT]
>Für alle Verbindungen zur Azure SQL-Datenbank ist eine Verschlüsselung (SSL/TLS) erforderlich, und zwar zu jedem Zeitpunkt während der Übertragung von Daten in und aus der Datenbank. Sie müssen in der Verbindungszeichenfolge Ihrer Anwendung Parameter zum Verschlüsseln der Verbindung angeben und dürfen dem Serverzertifikat *nicht* vertrauen (dies wird für Sie erledigt, wenn Sie die Verbindungszeichenfolge aus dem klassischen Azure-Portal kopieren). Andernfalls wird die Identität des Servers von der Verbindung nicht überprüft und diese ist anfällig für „Man-in-the-Middle“-Angriffe. Für ADO.NET-Treiber beispielsweise lauten diese Parameter für Verbindungszeichenfolgen **Encrypt=True** und **TrustServerCertificate=False**. 

Es gibt noch weitere Möglichkeiten zum Verschlüsseln Ihrer Daten:

* [Verschlüsselung auf Zellenebene](https://msdn.microsoft.com/library/ms179331.aspx) werden bestimmte Spalten oder sogar Zellen, die Daten enthalten, mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsselt.
* Wenn Sie ein Hardwaresicherheitsmodul oder eine zentrale Verwaltung Ihrer Verschlüsselungsschlüsselhierarchie benötigen, können Sie den [Azure-Schlüsseltresor mit SQL Server in einer Azure-VM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)verwenden.

## <a name="control-access"></a>Steuern des Zugriffs
SQL-Datenbank schützt Ihre Daten, indem der Zugriff auf Ihre Datenbank beschränkt wird. Hierfür wird Folgendes verwendet: Firewallregeln, Authentifizierungsmechanismen, bei denen Benutzer ihre Identität nachweisen müssen, und Datenautorisierung über rollenbasierte Mitgliedschaften und Berechtigungen sowie mithilfe von Sicherheit auf Zeilenebene und dynamischer Datenmaskierung. Eine Beschreibung der Nutzung von Zugriffssteuerungsfunktionen in SQL-Datenbank finden Sie unter [Steuern des Zugriffs](sql-database-control-access.md).

> [!IMPORTANT]
> Die Verwaltung von Datenbanken und logischen Servern in Azure wird über die Rollenzuweisungen in Ihrem Portalbenutzerkonto gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal](../active-directory/role-based-access-control-what-is.md).
>

### <a name="firewall-and-firewall-rules"></a>Firewall und Firewallregeln
Als Schutz für Ihre Daten verhindern Firewalls jeglichen Zugriff auf Ihren Datenbankserver, bis Sie mit [Firewallregeln](sql-database-firewall-configure.md) angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.

### <a name="authentication"></a>Authentifizierung
Die SQL-Datenbank-Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

* **SQL-Authentifizierung**, bei der ein Benutzername und ein Kennwort verwendet werden. Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. "dbo" (database owner) authentifizieren. 
* **Azure Active Directory-Authentifizierung**, bei der von Azure Active Directory verwaltete Identitäten verwendet werden und die für verwaltete und integrierte Domänen unterstützt wird. Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), [sofern dies möglich ist](https://msdn.microsoft.com/library/ms144284.aspx). Wenn Sie die Azure Active Directory-Authentifizierung verwenden möchten, müssen Sie einen weiteren Serveradministrator mit der Bezeichnung "Azure AD Admin" erstellen, der zum Verwalten von Azure Active Directory-Benutzern und -Gruppen berechtigt ist. Dieser Administrator kann außerdem die gleichen Aufgaben wie ein normaler Serveradministrator ausführen. Unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) finden Sie eine exemplarische Vorgehensweise zum Erstellen eines Azure AD-Administrators, um die Azure Active Directory-Authentifizierung zu aktivieren.

### <a name="authorization"></a>Autorisierung
Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer in einer Azure SQL-Datenbank ausführen kann. Dies wird durch die Datenbank-Rollenmitgliedschaften und Objektebenenberechtigungen Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden.

### <a name="row-level-security"></a>Sicherheit auf Zeilenebene
Bei der Sicherheit auf Zeilenebene können Kunden den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (z.B. Gruppenmitgliedschaft oder Ausführungskontext). Weitere Informationen finden Sie unter [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131).

### <a name="data-masking"></a>Datenmaskierung 
Die dynamische Datenmaskierung für SQL-Datenbank schränkt die Offenlegung vertraulicher Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Mit der dynamischen Datenmaskierung werden in Azure SQL-Datenbank automatisch potenziell sensible Daten ermittelt und direkt umsetzbare Empfehlungen bereitgestellt, um diese Felder mit minimalen Auswirkungen auf die Anwendungsschicht zu maskieren. Hierzu werden die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern verborgen, ohne dass die Daten in der Datenbank geändert werden. Weitere Informationen finden Sie in den [ersten Schritten mit der dynamischen Datenmaskierung für SQL-Datenbanken](sql-database-dynamic-data-masking-get-started.md). Die Datenmaskierung kann zum Schutz vor unautorisierter Offenlegung von sensiblen Daten verwendet werden.

## <a name="proactive-monitoring"></a>Proaktive Überwachung
SQL-Datenbank schützt Ihre Daten, indem Funktionen für die Überwachung und Bedrohungserkennung bereitgestellt werden. 

### <a name="auditing"></a>Überwachung
Bei der Überwachung von SQL-Datenbank werden Datenbankaktivitäten nachverfolgt, und Sie erhalten Unterstützung bei der Einhaltung von gesetzlichen Bestimmungen, indem Datenbankereignisse in Ihrem Azure Storage-Konto in einem Überwachungsprotokoll aufgezeichnet werden. Dank der Überwachung können Sie die derzeitigen Datenbankaktivitäten verstehen und die Verlaufsaktivität analysieren und untersuchen, um potenzielle Bedrohungen oder vermutete Missbrauchsfälle und Sicherheitsverletzungen zu identifizieren. Weitere Informationen finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing.md).  

### <a name="threat-detection"></a>Bedrohungserkennung
Mit der Bedrohungserkennung wird die Überwachung vervollständigt, indem der Azure SQL-Datenbank-Dienst um eine zusätzliche Security Intelligence-Ebene erweitert wird, die ungewöhnliche und eventuell schädliche Versuche erkennt, auf Datenbanken zuzugreifen oder diese zu missbrauchen. Sie werden vor verdächtigen Aktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbank-Zugriffsmustern gewarnt. Warnungen der Bedrohungserkennung können in [Azure Security Center](https://azure.microsoft.com/services/security-center/) angezeigt werden und bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann. Die Bedrohungserkennung kostet 15 $/Server/Monat. Sie ist für die ersten 60 Tage kostenlos. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).
 
### <a name="data-masking"></a>Datenmaskierung 
Die dynamische Datenmaskierung für SQL-Datenbank schränkt die Offenlegung vertraulicher Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Mit der dynamischen Datenmaskierung werden in Azure SQL-Datenbank automatisch potenziell sensible Daten ermittelt und direkt umsetzbare Empfehlungen bereitgestellt, um diese Felder mit minimalen Auswirkungen auf die Anwendungsschicht zu maskieren. Hierzu werden die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern verborgen, ohne dass die Daten in der Datenbank geändert werden. Weitere Informationen finden Sie unter [Dynamische Datenmaskierung für SQL-Datenbank](sql-database-dynamic-data-masking-get-started.md).
 
## <a name="compliance"></a>Compliance
Zusätzlich zu den oben aufgeführten Features und Funktionalitäten, mit denen Ihre Anwendung eine Reihe von Sicherheitsanforderungen erfüllen kann, wird Azure SQL-Datenbank außerdem regelmäßigen Überprüfungen unterzogen und ist für eine Reihe von Kompatibilitätsstandards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), wo die aktuellste Liste von [Compliance-Zertifizierungen für SQL-Datenbank](https://azure.microsoft.com/support/trust-center/services/) angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

- Eine Beschreibung der Nutzung von Zugriffssteuerungsfunktionen in SQL-Datenbank finden Sie unter [Steuern des Zugriffs](sql-database-control-access.md).
- Eine Erörterung zur Datenbanküberwachung finden Sie unter [Überwachung von SQL-Datenbank](sql-database-auditing.md).
- Eine Erörterung zur Bedrohungserkennung finden Sie unter [Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).

