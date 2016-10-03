<properties 
	pageTitle="Entwicklungsrichtlinien für DocumentDB-Konten mit Protokollunterstützung für MongoDB (Vorschau) | Microsoft Azure" 
	description="Informieren Sie sich über Entwicklungsrichtlinien für DocumentDB-Konten mit Protokollunterstützung für MongoDB (jetzt als Vorschau verfügbar)." 
	services="documentdb" 
	authors="andrewhoh" 
	manager="jhubbard" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/15/2016" 
	ms.author="anhoh"/>

# Entwicklungsrichtlinien für DocumentDB-Konten mit Protokollunterstützung für MongoDB

Sie können über einen beliebigen Open-Source-MongoDB-[Clienttreiber](https://docs.mongodb.org/ecosystem/drivers/) mit Azure DocumentDB kommunizieren. Für die Protokollunterstützung für MongoDB wird davon ausgegangen, dass die MongoDB-Clienttreiber mit einem Serverendpunkt mit MongoDB 2.6 oder höher kommunizieren. DocumentDB unterstützt dies durch Nutzung des MongoDB-[Verbindungsprotokolls](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol/) der Version 2.6.

DocumentDB unterstützt die MongoDB-API-Kernfunktionen zum Erstellen, Lesen, Aktualisieren und Löschen (Create, Read, Update, Delete, CRUD) von Daten sowie Datenbankabfragen. Die implementierten Funktionen wurden basierend auf den Anforderungen gängiger Plattformen, Frameworks, Tools und Anwendungsmuster priorisiert.

## Sammlungen

> [AZURE.IMPORTANT] DocumentDB verwendet einen reservierten Durchsatz auf Sammlungsebene, um eine garantierte, planbare Leistung zu liefern. Sammlungen sind daher in DocumentDB fakturierbare Entitäten.

Die Leistungsreservierungen werden auf Sammlungsebene angewendet, sodass Anwendungen die Leistung auf der untersten Ebene der Datencontainer im System anpassen können. Der Preis einer Sammlung wird anhand des bereitgestellten Durchsatzes der Sammlung ermittelt, der in Anforderungseinheiten pro Sekunde zusammen mit dem insgesamt verbrauchten Speicher in GB gemessen wird. Der bereitgestellte Durchsatz kann während der gesamten Lebensdauer einer Sammlung angepasst werden, um sie auf die veränderten Verarbeitungsanforderungen und Zugriffsmuster der Anwendung abzustimmen. Weitere Informationen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).

DocumentDB-Sammlungen mit Protokollunterstützung für MongoDB werden standardmäßig mit dem Standard-Tarif mit 1.000 RU/s des bereitgestellten Durchsatzes erstellt. Sie können den bereitgestellten Durchsatz aller Ihrer Sammlungen anpassen, wie unter [Ändern von Leistungsstufen mit dem Azure-Portal](documentdb-performance-levels.md#changing-performance-levels-using-the-azure-portal) beschrieben.

## CRUD-Vorgänge

Die MongoDB-Vorgänge „Insert“, „Read“, „Update“, „Replace“ und „Delete“ werden vollständig unterstützt. Dazu gehört die Unterstützung für Feld-, Array- und Isolationsaktualisierungen sowie bitweiser Aktualisierungen, wie in der MongoDB-Spezifikation zum [Update-Operator](https://docs.mongodb.org/manual/reference/operator/update/) beschrieben. Für die Update-Operatoren, die mehrere Dokumentänderungen erfordern, enthält DocumentDB die vollständige ACID-Semantik mit Isolation von Momentaufnahmen.

## Abfragevorgänge

DocumentDB unterstützt mit nur wenigen Ausnahmen die vollständige Grammatik von MongoDB-Abfragen. Abfragen für einen JSON-kompatiblen Satz von [BSON-Typen](https://docs.mongodb.org/manual/reference/bson-types/) werden zur Unterstützung des Datum/Uhrzeit-Formats von MongoDB ebenfalls unterstützt. Für Abfragen, die keine von JSON-Typen abhängigen Operatoren erfordern, unterstützt DocumentDB GUID-Datentypen.

## Portalfunktion
Die Azure-Portalumgebung für MongoDB-protokollfähige Konten ist speziell auf solche Konten zugeschnitten. Wir möchten die Benutzeroberfläche erweitern, aber wir benötigen Ihr [Feedback](mailto:askdocdb@microsoft.com?subject=DocumentDB%20Protocol%20Support%20for%20MongoDB%20Preview%20Portal%20Experience) im Hinblick auf die für Sie nützlichsten Portalfunktionen.

## Unterstützungsmatrix


### CRUD- und Abfragevorgänge

Feature|Unterstützt|Wird unterstützt
---|---|---
Einfügen|InsertOne| 
 |InsertMany| 
 |Einfügen| 
Aktualisieren|UpdateOne| 
 |UpdateMany| 
 |Aktualisieren| 
Feld aktualisieren|$inc, $mul, $rename, $set, $unset, $min, $max|$currentDate| 
Array aktualisieren| |-alle-
Bitweise| |-alle-
Isolation| |-alle-
Ersetzen von|ReplaceOne| 
Löschen|DeleteOne | 
 |DeleteMany| 
 |Spalten| 
BulkWrite| |bulkWrite()
Vergleich|-alle-| 
Logisch|-alle-| 
Elementabfrage| |-alle-
Auswertung|$mod, $regex |$text, $where
GeoSpatial|2dsphere, 2d, polygon|Alle anderen
Array|$all, $size, $elemMatch|
Bitweise| |-alle-
Kommentar|-alle-| 
Projektion| |-alle-


### Datenbankbefehle

Feature|Unterstützt|Wird unterstützt
---|---|---
Aggregation|Count| 
GeoSpatial| |-alle-
Abfragen und Schreiben|find, insert, update, delete, getLastError, getMore, findAndModify| 
Authentifizierung|getnonce, logout, authenticate| 
Verwaltung|createIndex, listIndexes, dropIndexes, connectionStatus, reIndex| 
Diagnose|listDatabases, collStats, dbStats| 

## Nächste Schritte

- Erfahren Sie, wie Sie [MongoChef](documentdb-mongodb-mongochef.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden.
- Untersuchen Sie DocumentDB mit Protokollunterstützung für MongoDB anhand von [Beispielen](documentdb-mongodb-samples.md).

 

<!---HONumber=AcomDC_0921_2016-->