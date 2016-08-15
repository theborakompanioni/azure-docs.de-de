<properties
   pageTitle="Authentifizierung in Azure SQL Data Warehouse | Microsoft Azure"
   description="Azure Active Directory- (AAD) und SQL Server-Authentifizierung in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="07/29/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# Authentifizierung in Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-connect-overview.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Treiber](sql-data-warehouse-connection-strings.md)

Zum Herstellen einer Verbindung mit SQL Data Warehouse müssen Sie zu Authentifizierungszwecken Sicherheitsanmeldeinformationen übergeben. Beim Herstellen einer Verbindung werden Sie feststellen, dass bestimmte Verbindungseinstellungen im Rahmen der Einrichtung Ihrer Abfragesitzung konfiguriert werden.

Weitere Informationen zur Sicherheit und zum Aktivieren von Verbindungen mit Ihrem Data Warehouse finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse][].

## SQL-Authentifizierung
Zum Herstellen einer Verbindung mit SQL Data Warehouse müssen Sie die folgenden Informationen angeben:

- Vollqualifizierter Servername
- SQL-Authentifizierung
- Benutzername
- Kennwort
- Standarddatenbank (optional)

Standardmäßig wird die Verbindung mit der Masterdatenbank hergestellt, nicht mit Ihrer Benutzerdatenbank. Sie haben zwei Möglichkeiten, um eine Verbindung mit Ihrer Benutzerdatenbank herzustellen:

1. Geben Sie die Standarddatenbank an, wenn Sie Ihren Server mit dem SQL Server-Objekt-Explorer in SSDT oder SSMS registrieren, oder geben Sie die Datenbank in Ihrer Verbindungszeichenfolge für die Anwendung an. Fügen Sie für eine ODBC-Verbindung beispielsweise den InitialCatalog-Parameter ein.
2. Markieren Sie zuerst die Benutzerdatenbank, bevor Sie eine Sitzung in SSDT erstellen.

> [AZURE.NOTE] Eine Anleitung zum Herstellen einer Verbindung mit SQL Data Warehouse über SSDT finden Sie im Artikel [Abfragen mit Visual Studio][].

Hierbei ist es wiederum wichtig zu beachten, dass die Transact-SQL-Anweisung **USE <Ihre Datenbank>** zum Ändern der Datenbank für eine Verbindung nicht unterstützt wird.


## Authentifizierung über Azure Active Directory (AAD)

Die [Azure Active Directory][What is Azure Active Directory]-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit Microsoft Azure SQL Data Warehouse unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Mithilfe der Azure Active Directory-Authentifizierung ist es möglich, die Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten an einer zentralen Stelle zu verwalten. Die zentrale ID-Verwaltung bietet einen Ort, an dem die SQL Data Warehouse-Benutzer verwaltet werden und von vereinfacht die Berechtigungsverwaltung.

### Vorteile

Daraus ergeben sich u. a. die folgenden Vorteile:

- Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
- Es wird einer unkontrollierten Ausbreitung von Benutzeridentitäten über Datenbankserver hinweg Einhalt geboten.
- Es wird eine Kennwortrotation über eine zentrale Stelle ermöglicht.
- Kunden können Datenbankberechtigungen mithilfe von externen Gruppen (AAD) verwalten.
- Durch das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen wird das Speichern von Kennwörtern überflüssig.
- Die Azure Active Directory-Authentifizierung verwendet eigenständige Datenbankbenutzer zum Authentifizieren von Identitäten auf Datenbankebene.
- Azure Active Directory unterstützt die tokenbasierte Authentifizierung für Anwendungen, die sich mit SQL Data Warehouse verbinden.

> [AZURE.IMPORTANT] Die Azure Active Directory-Authentifizierung ist ein Vorschaufeature und unterliegt den Vorschaubestimmungen in Ihrer Lizenzvereinbarung (z. B. Enterprise Agreement, Microsoft Azure-Vertrag oder Microsoft Online-Abonnementvertrag) sowie allen anwendbaren Bestimmungen unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### Konfigurationsschritte

Die Konfigurationsschritte schließen die folgenden Verfahren zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung ein:

1. Erstellen und Auffüllen eines Azure Active Directory-Verzeichnisses
2. Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
3. Erstellen eines Azure Active Directory-Administrators für Azure SQL Data Warehouse
4. Konfigurieren der Clientcomputer
5. Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind
6. Herstellen einer Verbindung mit Ihrem Data Warehouse unter Verwendung von Azure AD-Identitäten

Azure Active Directory-Benutzer werden derzeit nicht im SSDT-Objekt-Explorer angezeigt. Die Benutzer können aber in [sys.database\_principals](https://msdn.microsoft.com/library/ms187328.aspx) angezeigt werden.
  
### Suchen der Details
- Führen Sie die ausführlichen Schritte aus. Die ausführlichen Schritte zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung sind fast identisch für die Azure SQL-Datenbank und für Azure SQL Data Warehouse. Befolgen Sie die ausführlichen Schritte im Thema [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](../sql-database/sql-database-aad-authentication.md).
- Erstellen Sie benutzerdefinierte Datenbankrollen, und fügen Sie den Rollen Benutzer hinzu. Weisen Sie anschließend präzise Berechtigungen für die Rollen zu. Weitere Informationen finden Sie unter [Getting Started with Database Engine Permissions](https://msdn.microsoft.com/library/mt667986.aspx) (Erste Schritte mit Berechtigungen für Datenbankmodule).

## Nächste Schritte

Informationen zum Abfragen Ihres Data Warehouse mit Visual Studio und anderen Anwendungen finden Sie unter [Abfragen mit Visual Studio][].

<!-- Article references -->
[Sichern einer Datenbank in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Abfragen mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md

<!---HONumber=AcomDC_0803_2016-->