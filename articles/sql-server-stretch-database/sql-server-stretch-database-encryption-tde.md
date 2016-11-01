<properties
   pageTitle="Aktivieren von Transparent Data Encryption (TDE) für SQL Server Stretch-Datenbank über Azure | Microsoft Azure"
   description="Aktivieren von Transparent Data Encryption (TDE) für SQL Server Stretch-Datenbank über Azure"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# Aktivieren von Transparent Data Encryption (TDE) für eine Stretch-Datenbank in Azure
> [AZURE.SELECTOR]
- [Azure-Portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparent Data Encryption (TDE) bietet Schutz vor der Bedrohung durch böswillige Aktivitäten. Hierzu werden die Datenbank, die dazugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt, ohne dass Änderungen der Anwendung erforderlich sind.

TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird. Der Datenbank-Verschlüsselungsschlüssel ist mit einem integrierten Serverzertifikat geschützt. Das integrierte Serverzertifikat ist für jeden Azure-Server einmalig. Microsoft führt für diese Zertifikate nach spätestens 90 Tagen automatisch eine Rotation durch. Eine allgemeine Beschreibung von TDE finden Sie unter [Transparente Datenverschlüsselung (TDE)].

##Aktivieren der Verschlüsselung

Befolgen Sie folgende Schritte zum Aktivieren von TDE für eine Azure-Datenbank, die die Daten speichert, die aus einer SQL Server Datenbank migriert wurden, für die Stretch aktiviert ist:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen**.
3. Wählen Sie die Option **Transparent data encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Ein** aus und wählen Sie anschließend **Speichern** ![][2] aus.


##Deaktivieren der Verschlüsselung

Befolgen Sie folgende Schritte zum Deaktivieren von TDE für eine Azure-Datenbank, die die Daten speichert, die aus einer SQL Server Datenbank migriert wurden, für die Stretch aktiviert ist:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen**.
3. Wählen Sie die Option **Transparten Data Encryption** aus.
4. Wählen Sie die Einstellung **Aus** aus und wählen Sie anschließend **Speichern** aus.




<!--Anchors-->
[Transparente Datenverschlüsselung (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->

<!---HONumber=AcomDC_0810_2016-->