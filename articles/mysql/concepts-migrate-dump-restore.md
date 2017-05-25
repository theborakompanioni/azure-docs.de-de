---
title: "Migrieren der MySQL-Datenbank durch Sicherungen und Wiederherstellungen in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Einführung in die Migration von Azure-Datenbank für MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/17/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: c0029e025cf6d0af478d1f21dc6acc7860905a81
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017

---

# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen
In diesem Artikel werden zwei allgemeine Verfahren zum Sichern und Wiederherstellen von Datenbanken in Azure-Datenbank für MySQL beschrieben
- Sichern und Wiederherstellen über die Befehlszeile (mithilfe von „mysqldump“) 
- Sichern und Wiederherstellen mithilfe von PHPMyAdmin 

## <a name="before-you-begin"></a>Voraussetzungen
Um diese Anleitung schrittweise auszuführen, müssen Sie Folgendes durchgeführt haben:
- [Erstellung eines Servers für Azure-Datenbank für MySQL – Azure-Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Installation des Befehlszeilenprogramms [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) auf einem Computer
- Download von MySQL-Workbench über [Herunterladen von MySQL Workbench](https://dev.mysql.com/downloads/workbench/), Toad, Navicat oder einem Drittanbietertool für MySQL

## <a name="use-common-tools"></a>Verwenden von gängigen Tools
Mithilfe von gängigen Tools wie MySQL Workbench, mysqldump, Toad oder Navicat können Sie eine Remoteverbindung zu Azure-Datenbank für MySQL herstellen und Daten in dieser wiederherstellen. Verwenden Sie diese Tools auf Ihrem Clientcomputer mit Internetverbindung, um eine Verbindung zu Azure-Datenbank für MySQL herzustellen. Um eine SSL-verschlüsselte Verbindung gemäß bewährter Sicherheitsmethoden zu verwenden, lesen Sie [Konfigurieren von SSL-Konnektivität in Azure-Datenbank für MySQL](concepts-ssl-connection-security.md). Bei der Migration auf Azure-Datenbank für MySQL müssen Sie Ihre Sicherungsdateien nicht in einen bestimmten Cloudspeicherort verschieben. 

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Erstellen einer Sicherungsdatei über die Befehlszeile „mysqldump“
Um eine vorhandene MySQL-Datenbank lokal oder in einer VM zu sichern, führen Sie folgenden Befehl aus: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Es müssen folgende Parameter bereitgestellt werden:
- [uname] Der Datenbankbenutzername 
- [pass] Das Kennwort für Ihre Datenbank (beachten Sie, dass kein Leerzeichen zwischen „-p“ und dem Kennwort verwendet wird) 
- [dbname] Der Name der Datenbank 
- [backupfile.sql] Der Dateiname für die Datenbanksicherung 
- [--opt] Die mysqldump-Option 

Um eine Datenbank namens „testdb“ mit dem Benutzernamen „testuser“ ohne Kennwort als Datei mit dem Namen „testdb_backup.sql“ zu sichern, verwenden Sie folgenden Befehl. Mit diesem Befehl wird die Datenbank „testdb“ als Datei namens „testdb_backup.sql“ gesichert, die alle SQL-Anweisungen zum Neuerstellen der Datenbank enthält. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Um zur Sicherung bestimmte Tabellen in der Datenbank auszuwählen, listen Sie die Tabellennamen durch Leerzeichen getrennt auf. Um beispielsweise nur die Tabellen „table1“ und „table2“ von „testdb“ zu sichern, führen Sie folgendes Beispiel durch: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

Um mehrere Datenbanken gleichzeitig zu sichern, verwenden Sie den Schalter „--database“ und listen Sie die Datenbanknamen durch Leerzeichen getrennt auf. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Um alle Datenbanken auf dem Server gleichzeitig zu sichern, verwenden Sie die Option „--all-databases“.
```
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-the-target-azure-mysql-server"></a>Erstellen einer Datenbank auf dem Azure-MySQL-Zielserver
Sie müssen auf dem Azure-Datenbank für MySQL-Zielserver, auf den die Daten mit MySQL Workbench, Toad, Navicat oder einem beliebigen Drittanbietertool für MySQL migriert werden sollen, eine leere Datenbank erstellen. Die Datenbank kann denselben Namen aufweisen wie die Datenbank, die die Sicherungsdaten enthält, oder Sie können eine Datenbank mit einem anderen Namen erstellen.

![Azure-Datenbank für MySQL-Verbindungszeichenfolge](./media/concepts-migrate-import-export/p5.png)

![MySQL Workbench-Verbindungszeichenfolge](./media/concepts-migrate-import-export/p4.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Wiederherstellen der MySQL-Datenbank über die Befehlszeile oder MySQL Workbench
Nachdem Sie die Zieldatenbank erstellt haben, können Sie mithilfe des Befehls „mysql“ oder von MySQL Workbench Daten in der jeweiligen neu erstellten Datenbank anhand der Sicherungsdatei wiederherstellen.
```bash
mysql -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
In diesem Beispiel werden die Daten auf dem Zielserver in der neu erstellten Datenbank „testdb3“ wiederhergestellt.
```bash
$ mysql -u root -p testdb3 < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exportieren mithilfe von PHPMyAdmin
Für den Export können Sie das allgemeine Tool phpMyAdmin verwenden, das Sie eventuell bereits lokal in Ihrer Umgebung installiert haben. So exportieren Sie Ihre MySQL-Datenbank mithilfe von PHPMyAdmin:
- Öffnen Sie phpMyAdmin.
- Wählen Sie Ihre Datenbank aus, indem Sie auf der linken Seite des Bildschirms auf den Datenbanknamen in der Liste klicken. 
- Klicken Sie auf den Exportlink. Hierdurch sollte ein neuer Bildschirm angezeigt werden, der besagt, dass die Sicherung der Datenbank angezeigt wird. 
- Klicken Sie im Bereich „Export“ auf den Link „Alle auswählen“, um alle Tabellen in der Datenbank auszuwählen. 
- Klicken Sie im Bereich „SQL-Optionen“ auf die jeweiligen Optionen. 
- Klicken Sie auf die Option „Als Datei speichern“ und die entsprechende Komprimierungsoption. Klicken Sie dann auf die Schaltfläche „Gehe zu“. Es sollte ein Dialogfeld angezeigt werden, in dem Sie dazu aufgefordert werden, die Datei lokal zu speichern.

## <a name="import-using-phpmyadmin"></a>Importieren mithilfe von PHPMyAdmin
Der Import der Datenbank ist mit dem Exportvorgang vergleichbar. Führen Sie folgende Aktionen durch:
- Öffnen Sie phpMyAdmin. 
- Erstellen Sie eine Datenbank mit einem entsprechenden Namen, und wählen Sie sie aus, indem Sie auf der linken Seite des Bildschirms auf den Datenbanknamen in der Liste klicken. Wenn Sie den Import über eine vorhandene Datenbank neu schreiben möchten, klicken Sie auf den Datenbanknamen, aktivieren Sie alle Kontrollkästchen neben den Tabellennamen, und wählen Sie „Löschen“ aus, um alle vorhandenen Tabellen in der Datenbank zu löschen. 
- Klicken Sie auf den SQL-Link. Daraufhin sollte ein neuer Bildschirm angezeigt werden, in dem Sie SQL-Befehle eingeben oder Ihre SQL-Datei hochladen können. 
- Verwenden Sie die Schaltfläche „Durchsuchen“, um nach der Datenbankdatei zu suchen. 
- Klicken Sie auf die Schaltfläche „Gehe zu“. Dies führt zum Export der Sicherung, Ausführen der SQL-Befehle und Neuerstellen Ihrer Datenbank.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md) 
[Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

