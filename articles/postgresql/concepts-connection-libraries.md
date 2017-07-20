---
title: "Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "In diesem Artikel werden verschiedene Bibliotheken und Treiber beschrieben, die Entwickler beim Schreiben von Anwendungen zum Erstellen von Verbindungen mit und zum Abfragen von Azure-Datenbank für PostgreSQL verwenden können."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 06/15/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 7099e26fd749820ab6869c0262e1f97d993ab7b2
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017

---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL
In diesem Thema werden Bibliotheken und Treiber beschrieben, die Entwickler beim Programmieren von Anwendungen zum Herstellen von Verbindungen mit und zum Abfragen von Azure-Datenbank für PostgreSQL verwenden können.

## <a name="client-interfaces"></a>Clientschnittstellen
Die meisten Clientsprachbibliotheken, die eine Verbindung zum PostgreSQL-Server herstellen, sind externe Projekte, und werden unabhängig verteilt. Diese werden auf Windows-, Linux- und Mac-Plattformen unterstützt. Einige der gängigen Clienttreiber werden aufgeführt:

| **Sprache** | **Clientschnittstelle** | **Zusätzliche Informationen** | **Download** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0-konform | [Download](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://php.net/manual/en/book.pgsql.php) | Datenbankerweiterung | [Installieren](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Pg npm package](https://www.npmjs.com/package/pg) | Reiner nicht blockierender JavaScript-Client | [Installieren](https://www.npmjs.com/package/pg) |
| Java | [JDBC](http://jdbc.postgresql.org/) | JDBC-Treiber vom Typ 4 | [Download](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg gem](https://deveiate.org/code/pg/) | Ruby-Schnittstelle | [Download](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Go | [Package pq](https://godoc.org/github.com/lib/pq) | Reiner Go-Postgres-Treiber | [Installieren](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](http://www.npgsql.org/) | ADO.NET-Datenanbieter | [Download](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC-Treiber | [Download](http://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Primäre Schnittstelle der C-Sprache | Included |
| C++ | [libpqxx](http://pqxx.org/) | Schnittstelle C++ mit neuer Formatvorlage | [Download](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie diese Schnellstarts, in denen beschrieben wird, wie Azure-Datenbank für PostgreSQL mit der gewünschten Sprache verbunden und abgefragt wird:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [.NET (C#)](./connect-csharp.md)

