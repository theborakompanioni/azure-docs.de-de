---
title: Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern | Microsoft Docs
description: Erfahren Sie, wie Sie Daten aus SQL Azure-Datenbank mithilfe von Indexern in einen Azure Search-Index abrufen.
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: e9bbf352-dfff-4872-9b17-b1351aae519f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: 096fcd2a7415da03714f05bb1f29ceac6f186eda
ms.openlocfilehash: dba7cd466d94cb68896ee9270bc765fe822ca00e

---

# <a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern
Azure Search ist ein gehosteter Cloudsuchdienst, mit dem Sie eine überzeugende Suchumgebung einfach bereitstellen können. Bevor Sie suchen können, müssen Sie einen Azure Search-Index mit Daten auffüllen. Wenn sich die Daten in einer Azure SQL-Datenbank befinden, können Sie mit dem neuen Feature **Azure Search-Indexer für Azure SQL-Datenbank** (oder kurz **Azure SQL-Indexer**) den Indizierungsprozess automatisieren. Dies bedeutet, dass Sie weniger Code schreiben und weniger Infrastruktur berücksichtigen müssen.

In diesem Artikel wird die Verwendung von Indexern behandelt, es werden aber auch die Features beschrieben, die nur bei Azure SQL-Datenbanken verfügbar sind (z.B. die integrierte Änderungsnachverfolgung). Azure Search unterstützt auch andere Datenquellen, z.B. Azure DocumentDB, Blob Storage und Table Storage. Wenn Sie sich Unterstützung für zusätzliche Datenquellen wünschen, geben Sie dazu im [Feedback-Forum für Azure Search](https://feedback.azure.com/forums/263029-azure-search/) Feedback.

## <a name="indexers-and-data-sources"></a>Indexer und Datenquellen
Sie können einen Azure SQL-Indexer wie folgt einrichten und konfigurieren:

* Mit dem Datenimport-Assistenten im [Azure-Portal](https://portal.azure.com)
* Mit dem Azure Search [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
* Mit der Azure Search [REST-API](http://go.microsoft.com/fwlink/p/?LinkID=528173)

In diesem Artikel wird die REST-API verwendet, um zu veranschaulichen, wie Sie **Indexer** und **Datenquellen** erstellen und verwalten können.

Eine **Datenquelle** gibt an, welche Daten indiziert werden müssen. Sie legt außerdem die Anmeldeinformationen für den Zugriff auf die Daten sowie die Richtlinien fest, mit denen Änderungen an den Daten effizient identifiziert werden können (z.B. neue, geänderte oder gelöschte Zeilen). Die Datenquelle wird als unabhängige Ressource definiert, sodass sie von mehreren Indexern verwendet werden kann.

Ein **Indexer** ist die Ressource, die Datenquellen mit Zielsuchindizes verbindet. Ein Indexer kann folgendermaßen verwendet werden:

* Eine einmalige Kopie der Daten zum Auffüllen eines Indexes ausführen.
* Einen Index mit Änderungen an der Datenquelle nach einem Zeitplan aktualisieren.
* Ausführung bei Bedarf, um den Index je nach Notwendigkeit zu aktualisieren.

## <a name="when-to-use-azure-sql-indexer"></a>Gründe für die Verwendung von Azure SQL-Indexern
Abhängig von verschiedenen Faktoren, die mit den Daten zusammenhängen, kann die Verwendung von Azure SQL-Indexern angebracht oder nicht angebracht sein. Wenn Ihre Daten die folgenden Anforderungen erfüllen, können Sie Azure SQL-Indexer verwenden:

* Alle Daten stammen aus einer einzelnen Tabelle oder Sicht.
  * Wenn die Daten auf mehrere Tabellen verteilt sind, können Sie eine Sicht erstellen und diese Ansicht dann mit dem Indexer verwenden. Bei Verwendung einer Sicht können Sie jedoch nicht die in SQL Server integrierte Änderungserkennung nutzen. Weitere Informationen finden Sie in [diesem Abschnitt](#CaptureChangedRows).
* Die in der Datenquelle verwendeten Datentypen werden vom Indexer unterstützt. Es werden die meisten, aber nicht alle SQL-Typen unterstützt. Weitere Informationen finden Sie unter [Zuordnen von Datentypen in Azure Search](http://go.microsoft.com/fwlink/p/?LinkID=528105).
* Bei Zeilenänderungen werden keine sehr zeitnahen Aktualisierungen des Index benötigt.
  * Der Indexer kann die Tabelle höchstens alle 5 Minuten erneut indizieren. Wenn sich Ihre Daten häufig ändern und die Änderungen innerhalb von Sekunden oder weniger Minuten im Index widergespiegelt werden müssen, sollten Sie direkt die [Index-API von Azure Search](https://msdn.microsoft.com/library/azure/dn798930.aspx) verwenden.
* Wenn Sie ein großes Dataset haben und den Indexer nach einem Zeitplan ausführen möchten, können wir anhand Ihres Schemas effizient geänderte (und ggf. gelöschte) Zeilen identifizieren. Weitere Informationen finden Sie unter "Erfassen geänderter und gelöschter Zeilen" weiter unten.
* Die Größe der indizierten Felder in einer Zeile überschreitet nicht die maximale Größe einer Azure Search-Indizierungsanforderung, die 16 MB beträgt .

## <a name="create-and-use-an-azure-sql-indexer"></a>Erstellen und Verwenden eines Azure SQL-Indexers
Erstellen Sie zunächst die Datenquelle:

    POST https://myservice.search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Sie können die Verbindungszeichenfolge mit der Option `ADO.NET connection string` aus dem klassischen [Azure-Portal](https://portal.azure.com) abrufen.

Dann erstellen Sie den Azure Search-Zielindex, sofern Sie bislang über keinen Index verfügen. Sie können einen Index über die [Portal-Benutzeroberfläche](https://portal.azure.com) oder mit der [Create Index API](https://msdn.microsoft.com/library/azure/dn798941.aspx) (API zur Indexerstellung) erstellen. Stellen Sie sicher, dass das Schema des Zielindexes mit dem Schema der Quelltabelle kompatibel ist – siehe dazu [Zuordnung zwischen SQL-Datentypen und Azure Search-Datentypen](#TypeMapping).

Erstellen Sie schließlich den Indexer, indem Sie ihm einen Namen geben und einen Verweis auf die Datenquelle und den Zielindex hinzufügen:

    POST https://myservice.search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Ein auf diese Weise erstellter Indexer verfügt über keinen Zeitplan. Er wird automatisch einmal ausgeführt, wenn er erstellt wird. Sie können ihn jederzeit mithilfe der Anforderung **run indexer** erneut ausführen:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2016-09-01
    api-key: admin-key

Sie können einige Aspekte des Indexerverhaltens anpassen, z.B. die Batchgröße, und wie viele Dokumente übersprungen werden können, bevor bei einer Indexerausführung ein Fehler auftritt. Weitere Informationen finden Sie unter [Create Indexer API](https://msdn.microsoft.com/library/azure/dn946899.aspx) (API zum Erstellen eines Indexers).

Möglicherweise müssen Sie Azure Services erlauben, eine Verbindung mit der Datenbank herzustellen. Anleitungen dazu finden Sie unter [Verbinden von Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Verwenden Sie zum Überwachen des Indexerstatus und Ausführungsverlaufs (Anzahl der indizierten Elemente, Fehler usw.) die Anforderung **indexer status** :

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2016-09-01
    api-key: admin-key

Die Antwort sollte etwa wie folgt aussehen:

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Der Ausführungsverlauf enthält bis zu 50 der zuletzt abgeschlossenen Ausführungen. Diese sind in umgekehrter chronologischer Reihenfolge sortiert (somit wird die neueste Ausführung als Erstes in der Antwort aufgelistet).
Weitere Informationen zur Antwort finden Sie unter [Abrufen des Indexerstatus](http://go.microsoft.com/fwlink/p/?LinkId=528198).

## <a name="run-indexers-on-a-schedule"></a>Ausführen von Indexern nach einem Zeitplan
Sie können den Indexer auch so konfigurieren, dass er regelmäßig nach einem Zeitplan ausgeführt wird. Dazu fügen Sie die **schedule**-Eigenschaft beim Erstellen oder Aktualisieren des Indexers hinzu. Das folgende Beispiel zeigt eine PUT-Anforderung den Indexer, um den zu aktualisieren:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Der Parameter **interval** ist erforderlich. Das Intervall bezieht sich auf den Zeitraum zwischen dem Start von zwei aufeinander folgenden Indexerausführungen. Das kleinste zulässige Intervall beträgt 5 Minuten. Das längste ist ein Tag. Es muss als XSD-Wert „dayTimeDuration“ formatiert sein (eine eingeschränkte Teilmenge eines [ISO 8601-Zeitwerts](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration)). Das Muster hierfür lautet wie folgt: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` = alle 15 Minuten, `PT2H` = alle 2 Stunden.

Der optionale Parameter **startTime** gibt an, wann die geplanten Ausführungen beginnen sollen. Wird er weggelassen, wird die aktuelle UTC-Zeit verwendet. Diese Zeitangabe kann in der Vergangenheit liegen. In diesem Fall wird die erste Ausführung so geplant, als wäre der Indexer seit der mit startTime angegebenen Startzeit kontinuierlich ausgeführt worden.  

Nur eine Ausführung eines Indexers kann zu einem gegebenen Zeitpunkt ausgeführt werden. Wenn ein Indexer zu dem Zeitpunkt ausgeführt wird, für den seine Ausführung geplant ist, wird die Ausführung auf den nächsten geplanten Zeitpunkt verschoben.

Betrachten wir ein Beispiel, um dies zu konkreter veranschaulichen. Angenommen Sie, wir haben den folgenden stündlichen Zeitplan konfiguriert:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Hier geschieht Folgendes:

1. Die erste Indexerausführung beginnt am 1. März 2015 um oder gegen 12:00 Uhr UTC.
2. Angenommen, diese Ausführung dauert 20 Minuten (oder hat eine andere Dauer unter 1 Stunde).
3. Die zweite Ausführung beginnt am 1. März 2015 um oder gegen 1:00 Uhr.
4. Nehmen wir nun an, dass diese Ausführung länger als eine Stunde dauert, z.B. 70 Minuten, sodass sie etwa um 2:10 Uhr abgeschlossen ist.
5. Es ist jetzt 2:00 Uhr und an der Zeit, die dritte Ausführung zu starten. Da die zweite Ausführung von 1:00 Uhr aber noch ausgeführt wird, wird die dritte Ausführung übersprungen. Die dritte Ausführung beginnt um 3 Uhr morgens.

Sie können mithilfe einer **PUT Indexer** -Anforderung einen Zeitplan für einen vorhandenen Indexer hinzufügen, ändern oder löschen.

<a name="CaptureChangedRows"></a>

## <a name="capturing-new-changed-and-deleted-rows"></a>Erfassen neuer, geänderter und gelöschter Zeilen
Wenn die Tabelle viele Zeilen umfasst, sollten Sie eine Richtlinie zur Erkennung von Datenänderungen verwenden. Die Änderungserkennung ermöglicht den effizienten Abruf nur der neuen oder geänderten Zeilen, ohne dass die gesamte Tabelle neu indiziert werden muss.

### <a name="sql-integrated-change-tracking-policy"></a>Richtlinie für die integrierte SQL-Änderungsnachverfolgung
Wenn die SQL-Datenbank die [Änderungsnachverfolgung](https://msdn.microsoft.com/library/bb933875.aspx)unterstützt, wird empfohlen, die **Richtlinie für die integrierte SQL-Änderungsnachverfolgung**zu verwenden. Dies ist die effizienteste Richtlinie. Zudem sorgt sie dafür, dass Azure Search gelöschte Zeilen identifiziert, ohne dass Sie eine explizite „Vorläufig löschen“-Spalte in Ihrer Tabelle angeben müssen.

Die integrierte Änderungsverfolgung wird , ab den folgenden SQL Server-Datenbankversionen unterstützt:

* SQL Server 2008 R2 und höher bei Verwendung von SQL Server auf Azure-VMs.
* Azure SQL-Datenbank V12 bei Verwendung von Azure SQL-Datenbank.

Wenn Sie die Richtlinie für die integrierte SQL-Änderungsnachverfolgung verwenden, geben Sie keine separate Richtlinie für das Erkennen gelöschter Daten an. Die Identifizierung gelöschter Zeilen wird von der Richtlinie bereits unterstützt.

Diese Richtlinie kann nur mit Tabellen verwendet werden; sie kann nicht mit Ansichten verwendet werden. Sie müssen die Änderungsnachverfolgung für die verwendete Tabelle aktivieren, bevor Sie diese Richtlinie verwenden können. Anweisungen finden Sie unter [Aktivieren und Deaktivieren der Änderungsnachverfolgung](https://msdn.microsoft.com/library/bb964713.aspx) .

Um diese Richtlinie zu verwenden, erstellen oder aktualisieren Sie die Datenquelle wie folgt:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Richtlinie zum Erkennen von Änderungen mit oberem Grenzwert
Obwohl sich die Verwendung der Richtlinie für die integrierte SQL- Änderungsnachverfolgung empfiehlt, kann die Richtlinie lediglich mit Tabellen, jedoch nicht mit Sichten verwendet werden. Bei Verwendung einer Sicht sollten Sie die Richtlinie zum Erkennen von Änderungen mit oberem Grenzwert einsetzen. Diese Richtlinie kann verwendet werden, wenn die Tabelle oder Sicht eine Spalte enthält, die die folgenden Kriterien erfüllt:

* Alle Einfügungen geben einen Wert für die Spalte an.
* Alle Updates für ein Element ändern auch den Wert der Spalte.
* Der Wert dieser Spalte wird bei jeder Änderung erhöht.
* Abfragen mit den folgenden WHERE- und ORDER BY-Klauseln können effizient ausgeführt werden: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

Beispielsweise ist eine indizierte **rowversion** -Spalte ein idealer Kandidat für die Spalte mit dem oberen Grenzwert.
Um diese Richtlinie zu verwenden, erstellen oder aktualisieren Sie die Datenquelle wie folgt:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]"
      }
    }

> [!WARNING]
> Wenn die Quelltabelle keinen Index für die Spalte mit dem oberen Grenzwert enthält, kann das Zeitlimit für die vom SQL-Indexer verwendeten Abfragen überschritten werden. Insbesondere die Klausel `ORDER BY [High Water Mark Column]` erfordert für die effiziente Ausführung einen Index, wenn die Tabelle viele Zeilen enthält.
>
>

Wenn Timeoutfehler auftreten, können Sie mithilfe der Indexer-Konfigurationseinstellung `queryTimeout` das Abfragetimeout auf einen höheren Wert als das Standardtimeout von 5 Minuten festlegen. Um das Timeout z.B. auf 10 Minuten festzulegen, erstellen oder aktualisieren Sie den Indexer mit der folgenden Konfiguration:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Sie können auch die Klausel `ORDER BY [High Water Mark Column]` deaktivieren. Dies wird jedoch nicht empfohlen, da bei Unterbrechung der Indexer-Ausführung aufgrund eines Fehlers der Indexer alle Zeilen erneut verarbeiten muss, wenn er später ausgeführt wird, auch wenn zum Zeitpunkt der Unterbrechung bereits nahezu alle Zeilen verarbeitet wurden. Zum Deaktivieren der Klausel `ORDER BY` verwenden Sie die Einstellung `disableOrderByHighWaterMarkColumn` in der Indexer-Definition:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Richtlinie zum Erkennen von Löschungen anhand der Spalte „Vorläufig löschen“
Wenn Zeilen aus der Quelltabelle gelöscht werden, sollten Sie diese Zeilen auch aus dem Suchindex löschen. Wenn Sie die Richtlinie für die integrierte SQL-Änderungsnachverfolgung verwenden, müssen Sie sich nicht darum kümmern, weil es automatisch geschieht. Die Richtlinie zum Erkennen von Änderungen mit oberem Grenzwert hilft Ihnen allerdings nicht beim Löschen von Zeilen. Vorgehensweise

Wenn Zeilen physisch aus der Tabelle entfernt werden, gibt es in Azure Search keine Möglichkeit, das Vorhandensein von Datensätzen, die nicht mehr vorhanden sind, abzuleiten.  Mit der Methode des „vorläufigen Löschens“ können Sie jedoch Zeilen logisch löschen, ohne sie aus der Tabelle zu entfernen. Fügen Sie dazu der Tabelle eine Spalte hinzu, um Zeilen unter Verwendung dieser Spalte anzuzeigen und als gelöscht zu markieren.

Wenn Sie die Methode des "vorläufigen Löschens" verwenden, können Sie die Richtlinie zum Erkennen von Löschungen anhand der "Vorläufig löschen"-Spalte wie folgt beim Erstellen oder Aktualisieren der Datenquelle angeben:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]"
        }
    }

**softDeleteMarkerValue** muss eine Zeichenfolge sein. Verwenden Sie die Zeichenfolgendarstellung des tatsächlichen Werts. Wenn Sie z.B. über eine „integer“-Spalte verfügen, in der gelöschte Zeilen durch den Wert 1 gekennzeichnet sind, verwenden Sie `"1"`. Wenn Sie über eine „BIT“-Spalte verfügen, in der gelöschte Zeilen durch den booleschen Wert „TRUE“ markiert sind, verwenden Sie `"True"`.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Zuordnung zwischen SQL-Datentypen und Azure Search-Datentypen
| SQL-Datentyp | Zulässige Ziel-Index-Feldtypen | Hinweise |
| --- | --- | --- |
| Bit |Edm.Boolean, Edm.String | |
| int, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String | |
| bigint |Edm.Int64, Edm.String | |
| real, float |Edm.Double, Edm.String | |
| Smallmoney, money decimal numeric |Edm.String |Azure Search unterstützt nicht die Konvertierung von Dezimaltypen in Edm.Double-Typen, da es hierbei zu Genauigkeitsverlusten kommt. |
| char, nchar, varchar, nvarchar |Edm.String<br/>Collection(Edm.String) |Eine SQL-Zeichenfolge kann zum Auffüllen eines Collection(Edm.String)-Felds verwendet werden, wenn die Zeichenfolge ein JSON-Array von Zeichenfolgen darstellt: `["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, date, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| geography |Edm.GeographyPoint |Es werden nur Geography-Instanzen vom Typ POINT mit SRID 4326 (Standard) unterstützt. |
| rowversion |N/V |rowversion-Spalten können nicht im Suchindex gespeichert werden, sie können jedoch für die Änderungsnachverfolgung verwendet werden. |
| time, timespan, binary, varbinary, image, xml, geometry, CLR types |N/V |Nicht unterstützt |

## <a name="configuration-settings"></a>Konfigurationseinstellungen
SQL-Indexer verfügt über mehrere Konfigurationseinstellungen:

| Einstellung | Datentyp | Zweck | Standardwert |
| --- | --- | --- | --- |
| queryTimeout |string |Legt das Timeout für die Ausführung einer SQL-Abfrage fest |5 Minuten („00:05:00“) |
| disableOrderByHighWaterMarkColumn |bool |Bewirkt, dass bei der von der Richtlinie zum Erkennen von Änderungen mit oberem Grenzwert verwendeten SQL-Abfrage die ORDER BY-Klausel weggelassen wird. Siehe [Richtlinie zum Erkennen von Änderungen mit oberem Grenzwert](#HighWaterMarkPolicy). |false |

Diese Einstellungen werden im `parameters.configuration`-Objekt in der Indexer-Definition verwendet. Um das Abfragetimeout z.B. auf 10 Minuten festzulegen, erstellen oder aktualisieren Sie den Indexer mit der folgenden Konfiguration:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
**F:** Kann ich Azure SQL-Indexer mit SQL-Datenbanken verwenden, die auf IaaS-VMs in Azure ausgeführt werden?

A: Ja. Sie müssen jedoch die Verbindung des Suchdiensts mit der Datenbank ermöglichen. Weitere Informationen finden Sie im Artikel [Konfigurieren einer Verbindung eines Azure Search-Indexers mit SQL Server auf einer Azure-VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**F:** Kann ich Azure SQL-Indexer mit SQL-Datenbanken verwenden, die lokal ausgeführt werden?

A: Dies wird von uns weder empfohlen noch unterstützt, da Sie die Datenbanken dann für den Internetverkehr öffnen müssten.

**F:** Kann ich Azure SQL-Indexer mit anderen Datenbanken als SQL Server in IaaS auf Azure verwenden?

A: Wir unterstützen dieses Szenario nicht, da wir den Indexer mit keinen anderen Datenbanken als SQL Server getestet haben.  

**F:** Kann ich mehrere Indexer erstellen, die nach einem Zeitplan ausgeführt werden?

A: Ja. Allerdings kann zu einem gegebenen Zeitpunkt nur ein Indexer auf einem Knoten ausgeführt werden. Wenn mehrere Indexer gleichzeitig ausgeführt werden sollen, sollten Sie den Suchdienst skalieren und mehrere Sucheinheiten einsetzen.

**F:** Wirkt sich die Ausführung eines Indexers auf meine Abfragearbeitsauslastung aus?

A: Ja. Indexer werden auf einem der Knoten im Suchdienst ausgeführt, und die Ressourcen dieses Knotens werden für die Indizierung und das Bearbeiten des Datenverkehrs für Abfragen und andere API-Anforderungen gemeinsam genutzt. Wenn Sie intensive Indizierungs- und Abfragearbeitsauslastungen ausführen und viele 503-Fehler oder zunehmend längere Antwortzeiten auftreten, sollten Sie Ihren Suchdienst skalieren.



<!--HONumber=Nov16_HO3-->


