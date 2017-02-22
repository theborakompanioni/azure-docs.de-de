---
title: Arbeiten mit Daten in Azure DocumentDB | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure DocumentDB mit Daten arbeiten.
services: documentdb
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 3c07980e09362eabdfc80483213b62912e64ecf6
ms.openlocfilehash: b57dc0b699d7f7e6d539d6622b3d2769adf4257d


---
# <a name="working-with-dates-in-azure-documentdb"></a>Arbeiten mit Daten in Azure DocumentDB
DocumentDB bietet Schemaflexibilität und eine umfassende Indizierung über ein natives [JSON](http://www.json.org)-Datenmodell. Alle DocumentDB-Ressourcen mit Datenbanken, Sammlungen, Dokumenten und gespeicherten Prozeduren werden als JSON-Dokumente modelliert und gespeichert. Aufgrund der erforderlichen Portierbarkeit unterstützt JSON (ebenso wie DocumentDB) nur einen kleinen Satz grundlegender Typen: String, Number, Boolean, Array, Object und Null. JSON ist jedoch flexibel und ermöglicht es Entwicklern und Frameworks, komplexere Daten darzustellen, indem deren Grundtypen verwendet und als Objekte oder Arrays zusammengestellt werden. 

Zusätzlich zu den grundlegenden Typen benötigen viele Anwendungen den [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx)-Typ, um Daten und Zeitstempel darzustellen. Dieser Artikel beschreibt, wie Entwickler Daten in DocumentDB über das .NET SDK speichern, abrufen und abfragen können.

## <a name="storing-datetimes"></a>Speichern von DateTime-Werten
Standardmäßig serialisiert das [DocumentDB SDK](documentdb-sdk-dotnet.md) DateTime-Werte als [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874)-Zeichenfolgen. Die meisten Anwendungen können die standardmäßige Zeichenfolgendarstellung aus folgenden Gründen für DateTime verwenden:

* Zeichenfolgen können verglichen werden, und die relative Reihenfolge der DateTime-Werte wird beibehalten, wenn diese in Zeichenfolgen umgewandelt werden. 
* Für diesen Ansatz sind weder benutzerdefinierter Code noch Attribute für die JSON-Konvertierung erforderlich.
* Die als JSON gespeicherten Daten sind visuell lesbar.
* Bei dieser Vorgehensweise können Sie den DocumentDB-Index für eine schnelle Abfrageleistung nutzen.

Der folgende Codeausschnitt beispielsweise speichert ein `Order`-Objekt mit zwei DateTime-Eigenschaften – `ShipDate` und `OrderDate` – als Dokument mit dem .NET SDK:

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

Dieses Dokument wird folgendermaßen in DocumentDB gespeichert:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Alternativ dazu können Sie DateTime-Werte auch als UNIX-Zeitstempel speichern, also als Zahl, die die Anzahl der seit dem 1. Januar 1970 verstrichenen Sekunden darstellt. Die interne Timestamp-Eigenschaft von DocumentDB (`_ts`) folgt diesem Ansatz. Sie können die [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx)-Klasse verwenden, um DateTime-Werte als Zahlen zu serialisieren. 

## <a name="indexing-datetimes-for-range-queries"></a>Indizieren von DateTime-Werten für Bereichsabfragen
Bereichsabfragen werden bei DateTime-Werten häufig ausgeführt. Wenn Sie z.B. alle seit gestern erstellten oder in den letzten fünf Minuten ausgelieferten Aufträge finden möchten, müssen Sie Bereichsabfragen ausführen. Um diese Abfragen effizient auszuführen, müssen Sie Ihre Sammlung für die Bereichsindizierung von Zeichenfolgen konfigurieren.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

Weitere Informationen zum Konfigurieren von Indizierungsrichtlinien finden Sie unter [DocumentDB-Indizierungsrichtlinien](documentdb-indexing-policies.md).

## <a name="querying-datetimes-in-linq"></a>Abfragen von DateTime-Werten in LINQ
Das DocumentDB .NET SDK unterstützt automatisch die Abfrage von Daten, die über LINQ in DocumentDB gespeichert sind. Der folgende Codeausschnitt zeigt z.B. eine LINQ-Abfrage, die Aufträge filtert, die in den letzten drei Tagen ausgeliefert wurden.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on DocumentDB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

Weitere Informationen über die SQL-Abfragesprache von DocumentDB und den LINQ-Anbieter finden Sie unter [Abfragen von DocumentDB](documentdb-sql-query.md).

In diesem Artikel wurde erläutert, wie DateTime-Werte in DocumentDB gespeichert, indiziert und abgefragt werden.

## <a name="next-steps"></a>Nächste Schritte
* Herunterladen und Ausführen der [Codebeispiele auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Weitere Informationen zu [DocumentDB-Abfragen](documentdb-sql-query.md)
* Weitere Informationen zu [DocumentDB-Indizierungsrichtlinien](documentdb-indexing-policies.md)



<!--HONumber=Dec16_HO4-->


