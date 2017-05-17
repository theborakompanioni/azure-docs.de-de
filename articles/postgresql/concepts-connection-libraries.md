---
title: "Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Auflistung der einzelnen Bibliotheken bzw. Treiber, mit denen Clientprogramme Verbindungen zu Azure-Datenbank für PostgreSQL herstellen können"
keywords: Azure Cloud PostgreSQL postgres
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql - database
ms.tgt_pltfrm: portal
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6bb3f393a8e9533b86919e9a1ebe3490ab838714
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL
In diesem Thema werden die einzelnen Bibliotheken bzw. Treiber aufgelistet, mit denen Clientprogramme Verbindungen zu Azure-Datenbank für PostgreSQL herstellen können.

## <a name="client-interfaces"></a>Clientschnittstellen
Die meisten Clientsprachbibliotheken, die eine Verbindung zum PostgreSQL-Server herstellen, sind externe Projekte, und werden unabhängig verteilt. Diese werden auf Windows-, Linux- und Mac-Plattformen unterstützt. Einige der gängigen Clienttreiber werden nachfolgend aufgeführt.
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
- Einen Überblick über den Dienst finden Sie unter [Azure-Datenbank für PostgreSQL – Überblick](overview.md).
- Weitere Informationen zu Servern finden Sie unter [Azure-Datenbank für PostgreSQL-Server](concepts-servers.md).
- Um Ihren ersten PostgreSQL-Server zu erstellen, lesen Sie [Erstellen eines Servers für Azure-Datenbank für PostgreSQL im Azure-Portal](quickstart-create-server-database-portal.md).

