---
title: Synchronisieren von Daten (Vorschauversion) | Microsoft-Dokumentation
description: "Diese Übersicht enthält eine Einführung in Azure SQL-Datensynchronisierung (Vorschauversion)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
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
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 926938a8ed20167e1f17a9883007cd993897f14a
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung

SQL-Datensynchronisierung ist ein Dienst, der auf Azure SQL-Datenbank basiert und mit dem Sie die ausgewählten Daten bidirektional über mehrere SQL-Datenbanken und SQL Server-Instanzen hinweg synchronisieren können.

Grundlage der Datensynchronisierung ist eine Synchronisierungsgruppe. Eine Synchronisierungsgruppe ist eine Gruppe von Datenbanken, die Sie synchronisieren möchten.

Eine Synchronisierungsgruppe hat die folgenden Eigenschaften:

-   Im **Synchronisierungsschema** wird beschrieben, welche Daten synchronisiert werden.

-   Die **Synchronisierungsrichtung** kann bidirektional oder unidirektional sein. Für die Synchronisierungsrichtung kann also *Vom Hub zum Mitglied*, *Vom Mitglied zum Hub* oder beides gelten.

-   Mit dem **Synchronisierungsintervall** wird die Häufigkeit der Synchronisierung angegeben.

-   Die **Richtlinie zur Konfliktlösung** ist eine Richtlinie auf Gruppenebene, die den Typ *Hub gewinnt* oder *Mitglied gewinnt* haben kann.

Für die Datensynchronisierung wird eine Topologie der Art „Nabe und Speiche“ genutzt, um Daten zu synchronisieren. Sie definieren eine der Datenbanken in der Gruppe als Hub-Datenbank. Die übrigen Datenbanken sind Mitgliedsdatenbanken. Die Synchronisierung erfolgt nur zwischen dem Hub und den einzelnen Mitgliedern.
-   Die **Hub-Datenbank** muss eine Azure SQL-Datenbank sein.
-   Die **Mitgliedsdatenbanken** können entweder SQL-Datenbanken, lokale SQL Server-Datenbanken oder SQL Server-Instanzen auf virtuellen Azure-Computern sein.
-   Die **Synchronisierungsdatenbank** enthält die Metadaten und das Protokoll für die Datensynchronisierung. Bei der Synchronisierungsdatenbank muss es sich um eine Azure SQL-Datenbank handeln, die in derselben Region wie die Hub-Datenbank angeordnet ist. Die Synchronisierungsdatenbank wird vom Kunden erstellt und befindet sich in seinem Besitz.

> [!NOTE]
> Wenn Sie eine lokale Datenbank verwenden, ist es erforderlich, dass Sie einen [lokalen Agent konfigurieren](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync).

![Synchronisieren von Daten zwischen Datenbanken](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Verwenden der Datensynchronisierung

Die Datensynchronisierung ist in Fällen nützlich, in denen Daten über mehrere Azure SQL-Datenbanken oder SQL Server-Datenbanken aktuell gehalten werden müssen. Hier sind die wichtigsten Anwendungsfälle für die Datensynchronisierung aufgeführt:

-   **Hybrid-Datensynchronisierung:** Bei der Datensynchronisierung können Sie Daten zwischen Ihren lokalen Datenbanken und Azure SQL-Datenbanken synchron halten, um Hybridanwendungen zu ermöglichen. Diese Funktion ist unter Umständen gut für Kunden geeignet, die eine Umstellung auf die Cloud erwägen und einen Teil ihrer Anwendung in Azure anordnen möchten.

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
Für die Datensynchronisierung werden Auslöser für Einfügen, Aktualisieren und Löschen verwendet, um Änderungen nachzuverfolgen. In der Benutzerdatenbank werden Nebentabellen für die Änderungsnachverfolgung erstellt. Diese Aktivitäten zur Änderungsnachverfolgung haben Auswirkungen auf Ihre Datenbankworkload. Bewerten Sie Ihren Tarif, und aktualisieren Sie ihn bei Bedarf.

### <a name="eventual-consistency"></a>Letztliche Konsistenz
Die Transaktionskonsistenz ist nicht garantiert, da die Datensynchronisierung auf Auslösern basiert. Microsoft gewährleistet, dass alle Änderungen letztlich vorgenommen werden und dass es bei der Datensynchronisierung nicht zu Datenverlusten kommt.

### <a name="unsupported-data-types"></a>Nicht unterstützte Datentypen

-   FileStream

-   SQL/CLR UDT

-   XMLSchemaCollection (XML unterstützt)

-   Cursor, Timestamp, Hierarchyid

### <a name="requirements"></a>Anforderungen

-   Jede Tabelle muss über einen Primärschlüssel verfügen.

-   Eine Tabelle kann keine Identitätsspalte enthalten, die kein Primärschlüssel ist.

-   Die Namen von Objekten (Datenbanken, Tabellen und Spalten) dürfen nicht die druckbaren Zeichen Punkt (.), linke eckige Klammer ([) oder rechte eckige Klammer (]) enthalten.

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

## <a name="common-questions"></a>Häufig gestellte Fragen

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Wie häufig kann die Datensynchronisierung für meine Daten erfolgen? 
Die Mindesthäufigkeit ist alle fünf Minuten.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Kann ich die Datensynchronisierung verwenden, um Daten ausschließlich für lokale SQL Server-Datenbanken synchronisieren zu lassen? 
Nicht direkt. Sie können Daten zwischen lokalen SQL Server-Datenbanken jedoch indirekt synchronisieren, indem Sie in Azure eine Hub-Datenbank erstellen und anschließend die lokalen Datenbanken der Synchronisierungsgruppe hinzufügen.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>Kann ich mithilfe der Datensynchronisierung ein Seeding für Daten aus meiner Produktionsdatenbank in eine leere Datenbank ausführen und diese dann synchron halten? 
Ja. Erstellen Sie das Schema in der neuen Datenbank mithilfe eines Skripts manuell, das sich am Original orientiert. Nachdem Sie das Schema erstellt haben, fügen Sie Tabellen einer Synchronisierungsgruppe hinzu, um die Daten zu kopieren und synchron zu halten.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Warum sehe ich Tabellen, die ich nicht erstellt habe?  
Bei der Datensynchronisierung werden Nebentabellen für die Änderungsnachverfolgung erstellt. Löschen Sie diese nicht, da ansonsten die Datensynchronisierung nicht mehr funktioniert.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a>Ich habe eine Fehlermeldung erhalten, die besagt, dass ich den Wert NULL nicht in die Spalte \<Spalte\> einfügen kann, da die Spalte keine NULL-Werte zulässt. Was bedeutet dies, und wie kann ich das Problem beheben? 
Diese Fehlermeldung besagt, dass eines der beiden folgenden Probleme vorliegt:
1.  Es gibt möglicherweise eine Tabelle ohne Primärschlüssel. Um dieses Problem zu beheben, fügen Sie allen Tabellen, die Sie synchronisieren möchten, einen Primärschlüssel hinzu.
2.  Es gibt möglicherweise in der CREATE INDEX-Anweisung eine WHERE-Klausel. Die Synchronisierung beseitigt diese Bedingung nicht. Um dieses Problem zu beheben, entfernen Sie die WHERE-Klausel, oder nehmen Sie die Änderungen an allen Datenbanken manuell vor. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Wie geht die Datensynchronisierung mit Zirkelbezügen um? Diese liegen vor, wenn dieselben Daten in mehreren Synchronisierungsgruppen synchronisiert werden und sich daher ständig ändern.
Die Datensynchronisierung beseitigt Zirkelbezüge nicht. Vermeiden Sie sie deshalb unbedingt. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   [Erste Schritte mit der SQL-Datensynchronisierung](sql-database-get-started-sql-data-sync.md)

-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:
    -   [Verwenden von PowerShell zum Synchronisieren von Daten zwischen mehreren Azure SQL-­Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Download der gesamten technischen Dokumentation zur SQL-Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)

-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)

-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)

