<properties
	pageTitle="Gewusst wie: Ausführen von Verwaltungsaufgaben wie etwa Zurücksetzen des Administratorkennworts | Microsoft Azure"
	description="Hier wird beschrieben, wie allgemeine Verwaltungsaufgaben in SQL-Datenbank ausgeführt werden. Sie erfahren beispielsweise, wie Sie das Administratorkennwort zurücksetzen und Zugriff gewähren bzw. entfernen."
	services="sql-database"
	documentationCenter=""
	authors="v-shysun"
	manager="felixwu"
	editor=""
	keywords="Administratorkennwort zurücksetzen"/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="v-shysun"/>

# Gewusst wie: Ausführen allgemeiner Verwaltungsaufgaben in Azure SQL-Datenbank
In diesem Thema finden Sie Kurzanleitungen zum Gewähren und Aufheben des Zugriffs auf eine Azure SQL-Datenbank. Ausführlichere Informationen finden Sie unter:

- [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](sql-database-manage-logins.md)
- [Schützen Ihrer SQL-Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## So setzen Sie das Administratorkennwort für einen logischen Server zurück

- Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Server**, wählen Sie in der Liste den Server aus, und klicken Sie auf **Kennwort zurücksetzen**.

## So stellen Sie sicher, dass nur autorisierte IP-Adressen auf den Server zugreifen dürfen
- Siehe [Konfigurieren der Firewalleinstellungen für Azure SQL-Datenbank](sql-database-configure-firewall-settings.md).

## So erstellen Sie eigenständige Datenbankbenutzer in der Benutzerdatenbank
- Verwenden Sie die [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx)-Anweisung, und lesen Sie [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

## So authentifizieren Sie eigenständige Datenbankbenutzer mithilfe von Azure Active Directory
- Siehe [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).

## So erstellen Sie zusätzliche Anmeldungen für Benutzer mit hohen Berechtigungen in der virtuellen „master“-Datenbank
- Verwenden Sie die [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx)-Anweisung, und lesen Sie den Abschnitt „Verwalten von Anmeldungen“ in [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](sql-database-manage-logins.md), um weitere Informationen zu erhalten.

<!---HONumber=AcomDC_0713_2016-->