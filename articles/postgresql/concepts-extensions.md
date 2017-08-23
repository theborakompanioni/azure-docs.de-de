---
title: "Verwendung von PostgreSQL-Erweiterungen in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Beschreibung der Möglichkeit zum Erweitern der Funktionalität von Datenbanken mithilfe von Erweiterungen in Azure-Datenbank für PostgreSQL"
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 4f85e27d01341d3d9d61d1984add32e72a53b6ac
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-Erweiterungen in Azure-Datenbank für PostgreSQL
PostgreSQL bietet die Möglichkeit, die Funktionalität Ihrer Datenbank mithilfe von Erweiterungen zu erweitern. Durch Erweiterungen können mehrere SQL-bezogene Objekte in ein einziges Paket gebündelt, und über einen einzigen Befehl in die Datenbank geladen oder aus dieser entfernt werden. Werden Erweiterungen einmal in die Datenbank geladen, können sie wie ihre integrierten Funktionen eingesetzt werden. Weitere Informationen zu PostgreSQL-Erweiterungen finden Sie unter [Packen von zugehörigen Objekten in einer Erweiterung](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Wie werden PostgreSQL-Erweiterungen verwendet?
Bevor Sie PostgreSQL-Erweiterungen verwenden können, müssen diese für Ihre Datenbank installiert werden. Um eine bestimmte Erweiterung zu installieren, führen Sie zum Laden der gepackten Objekte in Ihrer Datenbank den Befehl [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) über das psql-Tool aus.

Azure-Datenbank für PostgreSQL unterstützt eine Teilmenge der wichtigsten hier aufgeführten Erweiterungen. Andere Erweiterungen werden neben den hier aufgeführten nicht unterstützt. Mit dem Azure-Datenbank für PostgreSQL-Dienst können keine eigenen Erweiterungen erstellt werden.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Unterstützte Erweiterungen in Azure-Datenbank für PostgreSQL
In den folgenden Tabellen werden die standardmäßigen PostgreSQL-Erweiterungen aufgeführt, die derzeit von Azure-Datenbank für PostgreSQL unterstützt werden. Sie können diese Informationen auch abrufen, indem Sie pg\_available\_extensions abfragen. 

### <a name="data-types-extensions"></a>Datentypenerweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Gibt einen Zeichenfolgentyp an, bei dem Groß-/Kleinschreibung beachtet werden muss |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Gibt den Datentyp zum Speichern von Schlüssel-/Wertpaaren an |

### <a name="functions-extensions"></a>Funktionserweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Stellt mehrere Funktionen bereit, um Ähnlichkeiten und den Abstand zwischen Zeichenfolgen zu ermitteln |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Stellt Funktionen und Operatoren bereit, um Arrays mit Ganzzahlen ohne Nullen zu bearbeiten |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Stellt kryptografische Funktionen bereit |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Verwaltet partitionierte Tabellen nach Zeit oder ID |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Stellt Funktionen und Operatoren bereit, um die Ähnlichkeit von alphanumerischen Texten basierend auf übereinstimmenden Trigrammen zu ermitteln |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Generiert universell eindeutige Bezeichner (UUIDs) |

### <a name="full-text-search-extensions"></a>Erweiterungen für die Volltextsuche

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Ein Wörterbuch für die Textsuche, das Akzente (diakritische Zeichen) aus Lexemen entfernt |

### <a name="index-types-extensions"></a>Indextypenerweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [btree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Gibt Beispielklassen für den GIN-Operator an, der das B-Struktur-ähnliche Verhalten für bestimmte Datentypen implementiert |
| [btree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Gibt Operatorklassen für den GiST-Index an, der die B-Struktur implementiert |

### <a name="language-extensions"></a>Spracherweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | Ladbare prozedurale Sprache, PL/pgSQL |

### <a name="miscellaneous-extensions"></a>Verschiedene Erweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Bietet eine Möglichkeit, um die Vorgänge im freigegebenen Puffercache in Echtzeit mitzuverfolgen |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Bietet eine Möglichkeit, um Relationsdaten in den Puffercache zu laden |
| [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Bietet eine Möglichkeit, um Ausführungsstatistiken zu allen SQL-Anweisungen nachzuverfolgen, die von einem Server ausgeführt werden |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Wrapper für programmfremde Daten, um auf in externen PostgreSQL-Servern gespeicherten Daten zuzugreifen |

### <a name="postgis"></a>PostGIS

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | Räumliche und geografische Objekte für PostgreSQL |
| address\_standardizer, address\_standardizer\_data\_us | Wird verwendet, um eine Adresse in dessen zugehörige Elemente zu analysieren. Wird als Unterstützung für den Normalisierungsschritt zur Geocodierung von Adressen verwendet. |
| [pgrouting](http://pgrouting.org/) | Erweitert die PostGIS/PostgreSQL-Geodatenbank um die Bereitstellung von Funktionen zum räumlichen Routing |

## <a name="next-steps"></a>Nächste Schritte
Fehlt eine Erweiterung, die Sie verwenden möchten? Dann informieren Sie uns darüber. Stimmen Sie in unserem [Kundenfeedback-Forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) über vorhandene Anfragen ab, oder teilen Sie uns weitere Feedbacks und Wünsche mit.

