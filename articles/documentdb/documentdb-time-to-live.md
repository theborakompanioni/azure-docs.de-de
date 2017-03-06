---
title: "Festlegen einer Gültigkeitsdauer für den Ablauf von Daten in DocumentDB | Microsoft Docs"
description: "Mit TTL bietet Microsoft Azure DocumentDB die Möglichkeit, im System vorhandene Dokumente nach einem bestimmten Zeitraum automatisch zu löschen."
services: documentdb
documentationcenter: 
keywords: "Gültigkeitsdauer (Time To Live, TTL)"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 1ad5307054dbd860f9c65db4b82ea5f560a554c8
ms.openlocfilehash: 14a06dd20547f2910b2321372b27d9f777e54cc7
ms.lasthandoff: 01/18/2017


---
# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Festlegen einer Gültigkeitsdauer für den automatischen Ablauf von Daten in DocumentDB-Sammlungen
Anwendungen können Unmengen an Daten generieren und speichern. Einige dieser Daten (etwa vom Computer generierte Ereignisdaten, Protokolle und Benutzersitzungsinformationen) sind allerdings nur für einen begrenzten Zeitraum relevant. Sobald die Daten von der Anwendung nicht mehr benötigt werden, können sie gefahrlos gelöscht werden, um den Speicherbedarf einer Anwendung zu verringern.

Mit der Gültigkeitsdauer (Time To Live, TTL) bietet Microsoft Azure DocumentDB die Möglichkeit, Dokumente nach einem bestimmten Zeitraum automatisch aus der Datenbank endgültig zu löschen. Die standardmäßige Gültigkeitsdauer kann auf Sammlungsebene festgelegt und für individuelle Dokumente überschrieben werden. Nach dem Festlegen der Gültigkeitsdauer (auf Sammlungs- oder Dokumentebene) werden Dokumente, die nach dem in Sekunden angegebenen Zeitraum (beginnend ab der letzten Änderung) vorhanden sind, von DocumentDB automatisch entfernt.

Die Gültigkeitsdauer in DocumentDB basiert auf einem Offset für den letzten Änderungszeitpunkt eines Dokuments. Zu diesem Zweck wird das für jedes Dokument vorhandene Feld `_ts` verwendet. Bei dem Feld „_ts“ handelt es sich um einen Epochenzeitstempel im Unix-Format zur Darstellung von Datum und Uhrzeit. Das Feld `_ts` wird bei jeder Änderung eines Dokuments aktualisiert. 

## <a name="ttl-behavior"></a>TTL-Verhalten
Das TTL-Feature wird über TTL-Eigenschaften auf zwei Ebenen (Sammlungsebene und Dokumentebene) gesteuert. Die Werte werden in Sekunden festgelegt und fungieren als Delta für den letzten Änderungszeitpunkt des Dokuments (aus dem Feld `_ts`).

1. DefaultTTL für die Sammlung
   
   * Wenn diese Angabe fehlt (oder auf Null festgelegt ist), werden Dokumente nicht automatisch gelöscht.
   * Ist die Angabe vorhanden und der Wert auf „-1“ (unendlich) festgelegt, laufen Dokumente standardmäßig nicht ab.
   * Ist die Angabe vorhanden und der Wert auf eine beliebige Zahl (n) festgelegt, laufen Dokumente n Sekunden nach der letzten Änderung ab.
2. TTL für die Dokumente 
   
   * Die Eigenschaft wird nur angewendet, wenn DefaultTTL für die übergeordnete Auflistung vorhanden ist.
   * Überschreibt den DefaultTTL-Wert für die übergeordnete Auflistung.

Abgelaufene Dokumente (`ttl` + `_ts` > = aktuelle Serverzeit) werden als abgelaufen markiert. Ab diesem Zeitpunkt können für diese Dokumente keine Vorgänge mehr ausgeführt werden, und sie werden in Abfrageergebnissen nicht mehr berücksichtigt. Die Dokumente werden im System physisch gelöscht und zu einem späteren Zeitpunkt opportunistisch im Hintergrund gelöscht. Dabei werden keinerlei [Anforderungseinheiten (Request Units, RUs)](documentdb-request-units.md) des Sammlungsbudgets verbraucht.

Die obige Logik wird in der folgenden Matrix veranschaulicht:

|  | DefaultTTL fehlt/nicht für die Sammlung festgelegt | DefaultTTL = -1 für die Sammlung | DefaultTTL = n für die Sammlung |
| --- |:--- |:--- |:--- |
| TTL für Dokument nicht vorhanden |Auf der Dokumentebene kann nichts überschrieben werden, da das TTL-Konzept weder auf der Dokument- noch auf der Sammlungsebene zur Anwendung kommt. |In dieser Sammlung laufen keine Dokumente ab. |Die Dokumente in dieser Sammlung laufen nach dem Intervall n ab. |
| TTL = -1 für das Dokument |Auf der Dokumentebene kann nichts überschrieben werden, da auf der Sammlungsebene keine DefaultTTL-Eigenschaft definiert ist, die ein Dokument überschreiben könnte. Der TTL-Wert für ein Dokument wird vom System nicht interpretiert. |In dieser Sammlung laufen keine Dokumente ab. |Das Dokument mit TTL = -1 in dieser Auflistung läuft nicht ab. Alle anderen Dokumente laufen nach dem Intervall n ab. |
| TTL = n für das Dokument |Auf der Dokumentebene kann nichts überschrieben werden. Der TTL-Wert für ein Dokument wird vom System nicht interpretiert. |Das Dokument mit TTL = n läuft nach dem Intervall n (in Sekunden) ab. Andere Dokumente erben das Intervall -1 und laufen nie ab. |Das Dokument mit TTL = n läuft nach dem Intervall n (in Sekunden) ab. Andere Dokumente erben das Intervall n von der Sammlung. |

## <a name="configuring-ttl"></a>Konfigurieren von TTL
Die Gültigkeitsdauer ist in DocumentDB-Sammlungen und für alle Dokumente standardmäßig deaktiviert.

## <a name="enabling-ttl"></a>Aktivieren von TTL
Um TTL für eine Sammlung (oder für die Dokumente in einer Sammlung) zu aktivieren, müssen Sie die DefaultTTL-Eigenschaft einer Auflistung entweder auf „-1“ oder auf eine positive Zahl ungleich Null festlegen. Wenn DefaultTTL auf „-1“ festgelegt wird, laufen die Dokumente in der Sammlung standardmäßig nicht ab, der DocumentDB-Dienst überwacht die Sammlung aber auf Dokumente, bei denen diese Standardeinstellung überschrieben wurde.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurieren einer Standardgültigkeitsdauer für eine Sammlung
Sie haben die Möglichkeit, eine Standardgültigkeitsdauer auf Sammlungsebene zu konfigurieren. Wenn Sie die Gültigkeitsdauer für eine Sammlung festlegen möchten, müssen Sie mit einer positiven Zahl ungleich null den Zeitraum angeben, nach dem alle in der Sammlung enthaltenen Dokumente ablaufen (in Sekunden ab dem Zeitstempel der letzten Änderung des Dokuments [`_ts`]). Alternativ können Sie den Standardwert auf „-1“ festlegen. In diesem Fall laufen die in die Sammlung eingefügten Dokumente niemals ab.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>Festlegen einer Gültigkeitsdauer für ein Dokument
Zusätzlich zu einer Standardgültigkeitsdauer für eine Sammlung können Sie eine spezifische Gültigkeitsdauer auf Dokumentebene festlegen. Dadurch wird die Standardeinstellung der Sammlung überschrieben.

* Wenn Sie die Gültigkeitsdauer für ein Dokument festlegen möchten, müssen Sie mit einer positiven Zahl ungleich null den Zeitraum angeben, nach dem das Dokument abläuft (in Sekunden ab dem Zeitstempel der letzten Änderung des Dokuments [`_ts`]).
* Für Dokumente ohne TTL-Feld gilt die Standardeinstellung der Sammlung.
* Wenn die Gültigkeitsdauer auf der Sammlungsebene deaktiviert ist, wird das TTL-Feld für das Dokument ignoriert, bis die Gültigkeitsdauer für die Sammlung wieder aktiviert wird.

Der folgende Codeausschnitt veranschaulicht das Festlegen des Ablaufs der Gültigkeitsdauer (TTL) für ein Dokument:

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a>Verlängern der Gültigkeitsdauer für ein vorhandenes Dokument
Die Gültigkeitsdauer für ein Dokument kann durch Ausführen eines beliebigen Schreibvorgangs für das Dokument zurückgesetzt werden. Dadurch wird `_ts` auf die aktuelle Zeit festgelegt, und der Countdown für den mittels `ttl` festgelegten Dokumentablauf beginnt von vorn. Wenn Sie die `ttl` eines Dokuments ändern möchten, können Sie das Feld genau wie jedes andere festlegbare Feld aktualisieren.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a>Entfernen der Gültigkeitsdauer von einem Dokument
Falls für ein Dokument eine Gültigkeitsdauer festgelegt wurde, das Dokument nun aber nicht mehr ablaufen soll, können Sie das Dokument abrufen, das TTL-Feld entfernen und das Dokument anschließend auf dem Server ersetzen. Nach dem Entfernen des TTL-Felds aus dem Dokument gilt wieder der Standardwert der Sammlung. Wenn ein Dokument nicht mehr ablaufen und auch nicht die Einstellung der Sammlung erben soll, muss der TTL-Wert auf „-1“ festgelegt werden.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a>Deaktivieren von TTL
Wenn Sie TTL für eine Sammlung vollständig deaktivieren und den Hintergrundprozess für die Suche nach abgelaufenen Dokumenten beenden möchten, muss die DefaultTTL-Eigenschaft für die Sammlung gelöscht werden. Das Löschen dieser Eigenschaft ist nicht das Gleiche wie das Festlegen der Eigenschaft auf „-1“. Bei Verwendung der Einstellung „-1“ laufen neue Dokumente, die der Sammlung hinzugefügt werden, nicht ab, dies kann jedoch für individuelle Dokumente in der Sammlung überschrieben werden. Wenn Sie die Eigenschaft vollständig aus der Sammlung entfernen, läuft ebenfalls keines der Dokumente ab, dies gilt aber auch für Dokumente, bei denen explizit ein vorheriger Standardwert überschrieben wurde.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a>Häufig gestellte Fragen
**Was kostet TTL?**

Die Verwendung von TTL für ein Dokument ist mit keinerlei Zusatzkosten verbunden.

**Wie lange dauert es nach dem Aktivieren von TTL, bis mein Dokument gelöscht wird?**

Die Dokumente sind sofort abgelaufen, sobald die Gültigkeitsdauer (TTL) verstrichen ist, und ein Zugriff über CRUD oder Abfrage-APIs ist nicht möglich. 

**Hat die Gültigkeitsdauer für ein Dokument Auswirkung auf die Gebühren für Anforderungseinheiten?**

Nein, Löschungen gemäß Gültigkeitsdauer (TTL) abgelaufener Dokumente in DocumentDB haben keine Auswirkungen auf RU-Gebühren.

**Gilt das TTL-Feature nur für vollständige Dokumente oder kann ich einen Ablauf für individuelle Dokumenteigenschaftswerte konfigurieren?**

TTL gilt für das gesamte Dokument. Soll nur ein Teil eines Dokuments ablaufen, empfiehlt es sich, den Teil aus dem Hauptdokument in ein separates, verknüpftes Dokument zu extrahieren und die Gültigkeitsdauer für das extrahierte Dokument zu verwenden.

**Gelten für das TTL-Feature bestimmte Indizierungsanforderungen?**

Ja. Als [Festlegung der Indizierungsrichtlinie](documentdb-indexing-policies.md) der Sammlung kommt „Lazy“ oder „Consistent“ infrage. Beim Versuch, DefaultTTL für eine Sammlung mit der Indizierung „None“ festzulegen, tritt ein Fehler auf. Gleiches gilt, wenn Sie versuchen, die Indizierung für eine Auflistung zu deaktivieren, für die DefaultTTL bereits festgelegt wurde.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure DocumentDB finden Sie auf der [*Dokumentationsseite*](https://azure.microsoft.com/documentation/services/documentdb/) des Diensts.


