---
title: "Sichern und Wiederherstellen in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren und die PostgreSQL-Datenbank aus einer mit pg_dump erstellten Archivdatei in Azure-Datenbank für PostgreSQL wiederherstellen."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 06/14/2017
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 8a399ca2f529568dbe5ffd4b58c3f0f69248b71d
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migrieren der PostgreSQL-Datenbank durch Sichern und Wiederherstellen
Sie können mit [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren und mit [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) die PostgreSQL-Datenbank aus einer mit pg_dump erstellten Archivdatei wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Server für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md) mit Firewallregeln, um den Zugriff und eine Datenbank darunter zu ermöglichen
- Die Befehlszeilenprogramme [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) müssen installiert sein.

Führen Sie zum Sichern und Wiederherstellen der PostgreSQL-Datenbank diese Schritte aus:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Erstellen einer Sicherungsdatei mit pg_dump, die die zu ladenden Daten enthält
Um eine vorhandene PostgreSQL-Datenbank lokal oder auf einem virtuellen Computer zu sichern, führen Sie folgenden Befehl aus:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Beispiel: Lokaler Server und eine Datenbank namens **testdb**
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Wiederherstellen der Daten in der Azure-Zieldatenbank für PostgreSQL mithilfe von pg_restore
Nachdem Sie die Zieldatenbank erstellt haben, können Sie mithilfe des Befehls „pg_restore“ oder des Parameters „-d, --dbname“ die Daten in der Zieldatenbank anhand der Sicherungsdatei wiederherstellen.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
In diesem Beispiel stellen wir die Daten aus der Dumpdatei **testdb.dump** in die Datenbank **mypgsqldb** auf dem Zielserver **mypgserver-20170401.postgres.database.azure.com** wieder her.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Migrieren einer PostgreSQL-Datenbank mithilfe von Export und Import finden Sie unter [Migrieren der PostgreSQL-Datenbank mit Import und Export](howto-migrate-using-export-and-import.md).
