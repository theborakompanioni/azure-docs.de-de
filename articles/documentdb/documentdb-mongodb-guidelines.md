<properties 
	pageTitle="Entwicklungsrichtlinien für DocumentDB-Konten mit Protokollunterstützung für MongoDB (Vorschau) | Microsoft Azure" 
	description="Informieren Sie sich über Entwicklungsrichtlinien für DocumentDB-Konten mit Protokollunterstützung für MongoDB (jetzt als Vorschau verfügbar)." 
	services="documentdb" 
	authors="stephbaron" 
	manager="jhubbard" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/31/2016" 
	ms.author="stbaro"/>

# Entwicklungsrichtlinien für DocumentDB-Konten mit Protokollunterstützung für MongoDB (Vorschau)

Sie können über einen beliebigen Open-Source-MongoDB-[Clienttreiber](https://docs.mongodb.org/ecosystem/drivers/) mit Azure DocumentDB kommunizieren. Für die Protokollunterstützung für MongoDB wird davon ausgegangen, dass die MongoDB-Clienttreiber mit einem Serverendpunkt mit MongoDB 2.6 oder höher kommunizieren. DocumentDB unterstützt dies durch Nutzung des MongoDB-[Verbindungsprotokolls](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol/) der Version 2.6. (Beachten Sie, dass das Verbindungsprotokoll der Version 3.2 fast vollständig unterstützt wird, aber bestimmte Clientumgebungen wie MongoDB-Shellsitzungen der Version 3.2 könnten angeben, dass sie „in einen veralteten Modus heruntergestuft werden“.)

DocumentDB unterstützt die MongoDB-API-Kernfunktionen zum Erstellen, Lesen, Aktualisieren und Löschen (Create, Read, Update, Delete, CRUD) von Daten sowie Datenbankabfragen. Die implementierten Funktionen wurden basierend auf den Anforderungen gängiger Plattformen, Frameworks, Tools und Anwendungsmuster priorisiert.

## Sammlungen

> [AZURE.IMPORTANT] DocumentDB verwendet einen reservierten Durchsatz auf Sammlungsebene, um eine garantierte, planbare Leistung zu liefern. Sammlungen sind daher in DocumentDB fakturierbare Entitäten.

Die Leistungsreservierungen werden auf Sammlungsebene angewendet, sodass Anwendungen die Leistung auf der untersten Ebene der Datencontainer im System anpassen können. Die Kosten der Sammlung werden anhand des bereitgestellten Durchsatzes der Sammlung ermittelt, der in Anforderungseinheiten pro Sekunde zusammen mit dem insgesamt verbrauchten Speicher in GB gemessen wird. Der bereitgestellte Durchsatz kann während der gesamten Lebensdauer einer Sammlung angepasst werden, um sie auf die veränderten Verarbeitungsanforderungen und Zugriffsmuster der Anwendung abzustimmen. Weitere Informationen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).

DocumentDB-Sammlungen mit Protokollunterstützung für MongoDB werden standardmäßig mit dem Standard-Tarif mit 1.000 RU/s des bereitgestellten Durchsatzes erstellt. Sie können den bereitgestellten Durchsatz aller Ihrer Sammlungen anpassen, wie unter [Ändern von Leistungsstufen mit dem Azure-Portal](documentdb-performance-levels.md#changing-performance-levels-using-the-azure-portal) beschrieben.

## CRUD-Vorgänge

Die MongoDB-Vorgänge „Insert“, „Read“, „Update“, „Replace“ und „Delete“ werden vollständig unterstützt. Dazu gehört die Unterstützung für Feld-, Array- und Isolationsaktualisierungen sowie bitweiser Aktualisierungen, wie in der MongoDB-Spezifikation zum [Update-Operator](https://docs.mongodb.org/manual/reference/operator/update/) beschrieben. Für die Update-Operatoren, die mehrere Dokumentänderungen erfordern, enthält DocumentDB die vollständige ACID-Semantik mit Isolation von Momentaufnahmen.

## Abfragevorgänge

DocumentDB unterstützt mit wenigen Ausnahmen die vollständige Grammatik von MongoDB-Abfragen. Abfragen für einen JSON-kompatiblen Satz von [BSON-Typen](https://docs.mongodb.org/manual/reference/bson-types/) werden zur Unterstützung des Datum/Uhrzeit-Formats von MongoDB ebenfalls unterstützt. Für Abfragen, die keine von JSON-Typen abhängigen Operatoren erfordern, unterstützt DocumentDB GUID-Datentypen. Die folgende Tabelle enthält die unterstützten und die nicht unterstützten Aspekte der MongoDB-Abfragegrammatik.

## Aggregation

DocumentDB unterstützt die MongoDB-Aggregationspipeline oder MapReduce-Vorgänge nicht. Die Aggregationspipeline dient normalerweise zum Verarbeiten von Dokumenten über einen mehrstufigen Satz von Filtern und Transformationen, z.B. Abgleichen und Gruppieren von Ergebnissen. DocumentDB bietet native Unterstützung für benutzerdefinierte JavaScript-Funktionen und gespeicherte Prozeduren. Darüber hinaus kann DocumentDB mithilfe von Azure HDInsight über den DocumentDB [Hadoop-Connector](documentdb-run-hadoop-with-hdinsight.md) ganz einfach als Quelle und Senke für Hive-, Pig- und MapReduce-Aufträge dienen.

## Portalfunktion
Die Benutzeroberfläche des Azure-Portals für die für das MongoDB-Protokoll aktivierten Konten unterscheidet sich ein wenig von der Benutzeroberfläche des Portals für standardmäßige DocumentDB-Konten. Wir möchten die Benutzeroberfläche erweitern, aber wir benötigen Ihr [Feedback](mailto:askdocdb@microsoft.com?subject=DocumentDB%20Protocol%20Support%20for%20MongoDB%20Preview%20Portal%20Experience) im Hinblick auf die für Sie nützlichsten Portalfunktionen.

## Unterstützungsmatrix


### CRUD- und Abfragevorgänge

Funktion|Unterstützt|Wird unterstützt|Nicht unterstützt 
---|---|---|---
Einfügen|InsertOne| | 
 |InsertMany| | 
 |Einfügen| | 
Aktualisieren|UpdateOne| | 
 |UpdateMany| | 
 |Aktualisieren| | 
Feld aktualisieren|$inc, $mul, $rename, $set, $unset, $min, $max|$currentDate| 
Array aktualisieren| |-alle-| 
Bitweise| |-alle-| 
Isolation| |-alle-| 
Replace|ReplaceOne| |
Löschen|DeleteOne | |
 |DeleteMany| | 
 |Remove| | 
BulkWrite| |bulkWrite()| 
Vergleich|-alle-| | 
Logisch|-alle-| | 
Elementabfrage| |-alle-| 
Auswertung|$mod|$regex, $text, $where| 
GeoSpatial|2dsphere, 2d, polygon|Alle anderen| 
Array|$all, $size|$elemMatch| 
Bitweise| |-alle-| 
Kommentar|-alle-| | 
Projektion| |-alle-| 


### Datenbankbefehle

Funktion|Unterstützt|Wird unterstützt|Nicht unterstützt 
---|---|---|---
Aggregation|Count| |aggregate, distinct, group, mapreduce
GeoSpatial| |-alle-| 
Abfragen und Schreiben|find, insert, update, delete, getLastError, getMore, findAndModify| |Eval, parallelCollectionScan, getPrevError, resetError
QueryPlan-Cache| | |-alle-
Authentifizierung|getnonce, logout, authenticate| |Copydbgetnone, authschemaUpgrade
Benutzerverwaltung| | |-alle-
Rollenverwaltung| | |-alle-
Replikation| | |-alle-
Verwaltung|createIndex, listIndexes, dropIndexes, connectionStatus, reIndex| |Sonstige Befehle. Für Indizes keine Unterstützung für: Unique, expireAfterSeconds, storageEngine, weights, default\_language, textIndexVersion, min, max, bucketSize
Diagnose|listDatabases, collStats, dbStats| |Alle anderen

## Nächste Schritte

- Erfahren Sie, wie Sie [MongoChef](documentdb-mongodb-mongochef.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden.
- Untersuchen Sie DocumentDB mit Protokollunterstützung für MongoDB anhand von [Beispielen](documentdb-mongodb-samples.md).

 

<!---HONumber=AcomDC_0720_2016-->