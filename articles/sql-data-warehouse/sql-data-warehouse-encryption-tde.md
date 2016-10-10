<properties
   pageTitle="Transparent Data Encryption in SQL Data Warehouse (Portal) | Microsoft Azure"
   description="Transparent Data Encryption (TDE) in SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# Erste Schritte mit Transparent Data Encryption (TDE) in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## Erforderliche Berechtigungen

Sie müssen Administrator oder ein Mitglied der Rolle „dbmanager“ sein, um Transparent Data Encryption (TDE) zu aktivieren.

## Aktivieren der Verschlüsselung

Führen Sie diese Schritte aus, um TDE für ein SQL Data Warehouse zu aktivieren:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen**.
3. Wählen Sie die Option **Transparent data encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Ein** aus. ![][2]
5. Wählen Sie **Speichern** aus. ![][3]

## Deaktivieren der Verschlüsselung

Führen Sie diese Schritte aus, um TDE für ein SQL Data Warehouse zu deaktivieren:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen**.
3. Wählen Sie die Option **Transparent data encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Aus** aus. ![][4]
5. Wählen Sie **Speichern** aus. ![][5]

## Verschlüsselung-DMVs

Die Verschlüsselung kann mit folgenden DMVs überprüft werden:

- [sys.databases]
- [sys.dm\_pdw\_nodes\_database\_encryption\_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm\_pdw\_nodes\_database\_encryption\_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->

<!---HONumber=AcomDC_0928_2016-->