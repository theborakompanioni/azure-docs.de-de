---
title: Synchronisieren von Daten (Vorschauversion) | Microsoft-Dokumentation
description: "Diese Übersicht enthält eine Einführung in Azure SQL-Datensynchronisierung (Vorschauversion)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 075b5563688158289d51f2f0b5da4a3441ddd13a
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung

SQL-Datensynchronisierung ist ein Dienst, der auf Azure SQL-Datenbank basiert und mit dem Sie die ausgewählten Daten bidirektional über mehrere SQL-Datenbanken und SQL Server-Instanzen hinweg synchronisieren können.

Grundlage der Datensynchronisierung ist eine Synchronisierungsgruppe. Eine Synchronisierungsgruppe ist eine Gruppe von Datenbanken, die Sie synchronisieren möchten.

Eine Synchronisierungsgruppe verfügt über mehrere Eigenschaften, z.B.:

-   Im **Synchronisierungsschema**  wird beschrieben, welche Daten synchronisiert werden.

-   Die **Synchronisierungsrichtung** kann bidirektional oder unidirektional sein. Für die Synchronisierungsrichtung kann also *Vom Hub zum Mitglied*, *Vom Mitglied zum Hub* oder beides gelten.

-   Mit dem **Synchronisierungsintervall** wird die Häufigkeit der Synchronisierung angegeben.

-   Die **Richtlinie zur Konfliktlösung** ist eine Richtlinie auf Gruppenebene, die den Typ *Hub gewinnt* oder *Mitglied gewinnt* haben kann.

Für die Datensynchronisierung wird eine Topologie der Art „Nabe und Speiche“ genutzt, um Daten zu synchronisieren. Sie müssen eine der Datenbanken in der Gruppe als Hub-Datenbank definieren. Die übrigen Datenbanken sind Mitgliedsdatenbanken. Die Synchronisierung erfolgt nur zwischen dem Hub und den einzelnen Mitgliedern.
-   Die **Hub-Datenbank** muss eine Azure SQL-Datenbank sein.
-   Die **Mitgliedsdatenbanken** können entweder SQL-Datenbanken, lokale SQL Server-Datenbanken oder SQL Server-Instanzen auf virtuellen Azure-Computern sein.
-   Die **Synchronisierungsdatenbank** enthält die Metadaten und das Protokoll für die Datensynchronisierung. Bei der Synchronisierungsdatenbank muss es sich um eine Azure SQL-Datenbank handeln, die in derselben Region wie die Hub-Datenbank angeordnet ist. Die Synchronisierungsdatenbank wird vom Kunden erstellt und befindet sich in seinem Besitz.

> [!NOTE]
> Wenn Sie eine lokale Datenbank verwenden, ist es erforderlich, dass Sie einen [lokalen Agent konfigurieren](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync).

![Synchronisieren von Daten zwischen Datenbanken](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Verwenden der Datensynchronisierung

Die Datensynchronisierung ist in Fällen nützlich, in denen Daten über mehrere Azure SQL-Datenbanken oder SQL Server-Datenbanken aktuell gehalten werden müssen. Hier sind die wichtigsten Anwendungsfälle für die Datensynchronisierung aufgeführt:

-   **Hybrid-Datensynchronisierung:** Bei der Datensynchronisierung können Sie Daten zwischen Ihren lokalen Datenbanken und Azure SQL-Datenbanken synchron halten, um Hybridanwendungen mit Datenebene in SQL zu ermöglichen. Diese Funktion ist unter Umständen gut für Kunden geeignet, die eine Umstellung auf die Cloud erwägen und einen Teil ihrer Anwendung in Azure anordnen möchten.

-   **Verteilte Anwendungen:** In vielen Fällen ist es vorteilhaft, unterschiedliche Workloads auf verschiedene Datenbanken aufzuteilen. Wenn Sie beispielsweise über eine große Produktionsdatenbank verfügen, aber gleichzeitig die Berichterstellung oder Analyse für diese Daten durchführen müssen, ist für diese zusätzliche Workload die Verwendung einer zweiten Datenbank hilfreich. Bei diesem Ansatz werden die Auswirkungen auf die Leistung Ihrer Produktionsworkload reduziert. Sie können die Datensynchronisierung nutzen, um diese beiden Datenbanken synchron zu halten.

-   **Global verteilte Anwendungen:** Viele Unternehmen sind in mehreren Regionen oder Ländern ansässig. Es ist ratsam, die Daten jeweils in einer Region in der Nähe vorzuhalten, um die Netzwerklatenz zu verringern. Mit der Datensynchronisierung können Sie Datenbanken in den Regionen weltweit synchron halten.

Für die folgenden Szenarien ist die Datensynchronisierung nicht zu empfehlen:

-   Notfallwiederherstellung

-   Leseskalierung

-   ETL (OLTP zu OLAP)

-   Migration von einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank

## <a name="how-does-data-sync-work"></a>Wie funktioniert die Datensynchronisierung? 

-   **Nachverfolgen von Datenänderungen:** Bei der Datensynchronisierung werden Änderungen mithilfe von Auslösern für Einfügen, Aktualisieren und Löschen nachverfolgt. Die Änderungen werden in der Benutzerdatenbank in einer Nebentabelle aufgezeichnet.

-   **Synchronisieren von Daten:** Für die Datensynchronisierung wird ein Modell der Art „Nabe und Speiche“ genutzt. Der Hub (Nabe) wird einzeln mit jedem Mitglied synchronisiert. Änderungen auf dem Hub werden für das Mitglied heruntergeladen, und anschließend werden Änderungen vom Mitglied auf den Hub hochgeladen.

-   **Beheben von Konflikten:** Die Datensynchronisierung bietet zwei Optionen für die Lösung von Konflikten, und zwar *Hub gewinnt* und *Mitglied gewinnt*.
    -   Wenn Sie *Hub gewinnt* wählen, werden die Änderungen auf dem Mitglied immer durch die Änderungen des Hub überschrieben.
    -   Bei Auswahl von *Mitglied gewinnt* werden die Änderungen auf dem Hub durch die Änderungen auf dem Mitglied überschrieben. Falls mehr als ein Mitglied vorhanden ist, hängt der endgültige Wert davon ab, welches Mitglied zuerst synchronisiert wird.

## <a name="limitations-and-considerations"></a>Einschränkungen und Aspekte

### <a name="performance-impact"></a>Auswirkungen auf die Leistung
Für die Datensynchronisierung werden Auslöser für Einfügen, Aktualisieren und Löschen verwendet, um Änderungen nachzuverfolgen. In der Benutzerdatenbank werden Nebentabellen erstellt. Da sich diese Aktivitäten auf Ihre Datenbankworkload auswirken, sollten Sie Ihre Dienstebene überprüfen und bei Bedarf ein Upgrade durchführen.

### <a name="eventual-consistency"></a>Letztliche Konsistenz
Die Transaktionskonsistenz ist nicht garantiert, da die Datensynchronisierung auf Auslösern basiert. Microsoft gewährleistet, dass alle Änderungen letztlich vorgenommen werden und dass es bei der Datensynchronisierung nicht zu Datenverlusten kommt.

### <a name="unsupported-data-types"></a>Nicht unterstützte Datentypen

-   FileStream

-   SQL/CLR UDT

-   XMLSchemaCollection (XML unterstützt)

-   Cursor, Timestamp, Hierarchyid

### <a name="requirements"></a>Anforderungen

-   Jede Tabelle muss über einen Primärschlüssel verfügen.

-   Eine Tabelle kann keine Identitätsspalten enthalten, die nicht dem Primärschlüssel unterliegen.

-   Ein Datenbankname darf keine Sonderzeichen enthalten.

### <a name="limitations-on-service-and-database-dimensions"></a>Einschränkungen von Dienst- und Datenbankdimensionen

|                                                                 |                        |                             |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| **Dimensionen**                                                      | **Begrenzung**              | **Problemumgehung**              |
| Maximale Anzahl von Synchronisierungsgruppen, denen eine Datenbank angehören kann       | 5                      |                             |
| Maximale Anzahl von Endpunkten einer einzelnen Synchronisierungsgruppe              | 30                     | Erstellen mehrerer Synchronisierungsgruppen |
| Maximale Anzahl von lokalen Endpunkten in einer einzelnen Synchronisierungsgruppe | 5                      | Erstellen mehrerer Synchronisierungsgruppen |
| Namen von Datenbanken, Tabellen, Schemas und Spalten                       | 50 Zeichen pro Name |                             |
| Tabellen in einer Synchronisierungsgruppe                                          | 500                    | Erstellen mehrerer Synchronisierungsgruppen |
| Spalten in einer Tabelle einer Synchronisierungsgruppe                              | 1000                   |                             |
| Größe von Datenzeilen in einer Tabelle                                        | 24 Mb                  |                             |
| Minimales Synchronisierungsintervall                                           | 5 Minuten              |                             |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL-Datenbank und zur SQL-Datensynchronisierung finden Sie unter:

-   [Erste Schritte mit der SQL-Datensynchronisierung](sql-database-get-started-sql-data-sync.md)

-   [Download der gesamten technischen Dokumentation zur SQL-Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)

-   [Download: Dokumentation zur SQL-Datensynchronisierung-REST-API](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)

-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)



