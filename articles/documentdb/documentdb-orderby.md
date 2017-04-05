---
redirect_url: https://azure.microsoft.com/services/documentdb/
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 0ca716857733290fad4278e3be5059408bb75393
ms.lasthandoff: 03/28/2017


---
# <a name="sorting-documentdb-data-using-order-by"></a>Sortieren von DocumentDB-Daten mit "Order By"
Microsoft Azure DocumentDB unterstützt Dokumentabfragen mit SQL über JSON-Dokumente. Abfrageergebnisse können mit der ORDER BY-Klausel in SQL-Abfrageanweisungen sortiert werden.

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten: 

* Wie erstelle ich Abfragen mit "Order By"?
* Wie konfiguriere ich eine Indexrichtlinie für "Order By"?
* Neue Entwicklungen

[Beispiele](#samples) und [häufig gestellte Fragen](#faq) werden ebenfalls bereitgestellt.

Eine vollständige Referenz zu SQL-Abfragen finden Sie unter der [DocumentDB-Abfragelernprogramm](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Erstellen von Abfragen mit "Order By"
Wie in ANSI-SQL können Sie jetzt bei DocumentDB-Abfragen eine optionale "Order By"-Klausel in SQL-Anweisungen einschließen. Die Klausel kann ein optionales ASC/DESC-Argument enthalten, um die Reihenfolge anzugeben, in der Ergebnisse abgerufen werden sollen. 

### <a name="ordering-using-sql"></a>Sortieren mit SQL
Nachstehend finden Sie z.B. eine Abfrage zum Abrufen der Top 10 der beliebtesten Bücher in absteigender Reihenfolge nach Titeln. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Sortieren mit SQL und Filtern
Verwenden eine geschachtelte Eigenschaft innerhalb von Dokumenten wie Books.ShippingDetails.Weight für das Sortieren verwenden, und Sie können zusätzliche Filter in der WHERE-Klausel in Kombination mit "Order By" wie in diesem Beispiel angeben:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Sortieren mithilfe des LINQ-Anbieters für .NET
Mit .NET SDK, Version 1.2.0 und höher, können Sie auch die "OrderBy()"- oder "OrderByDescending()"-Klausel in LINQ-Abfragen verwenden, wie in diesem Beispiel gezeigt wird:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB unterstützt die Sortierung mit einer einzelnen numerischen, Zeichenfolgen- oder booleschen Eigenschaft pro Abfrage. Weitere Abfragetypen werden in Kürze zur Verfügung stehen. Weitere Informationen finden Sie unter [Neue Entwicklungen](#Whats_coming_next).

## <a name="configure-an-indexing-policy-for-order-by"></a>Konfigurieren einer Indexrichtlinie für "Order By"
Denken Sie daran, dass DocumentDB zwei Arten von Indizes (Hash und Bereich) unterstützt, die für unterschiedliche Pfade/Eigenschaften, Datentypen (Zeichenfolgen/Zahlen) und auf verschiedene Präzisionswerte (maximale Genauigkeit oder einen festen Genauigkeitswert) festgelegt werden können. Da DocumentDB standardmäßig Hash-Indizes verwendet, müssen Sie eine neue Sammlung mit einer benutzerdefinierten Indizierungsrichtlinie mit „Bereich“ für Zahlen, Zeichenfolgen oder beide Optionen erstellen, um „Order By“ zu verwenden. 

> [!NOTE]
> Indizes für Zeichenfolgenbereiche wurden am 7. Juli 2015 mit der REST-API-Version 2015-06-03 eingeführt. Um Richtlinien für „Order By“ für Zeichenfolgen zu erstellen, müssen Sie die SDK-Version 1.2.0 des .NET-SDK oder Version 1.1.0 des Python-, Node.js- oder Java-SDK verwenden.
> 
> Vor der REST-API-Version 2015-06-03 war „Hash“ die Standardsammlung der Indizierungsrichtlinie für Zeichenfolgen und Zahlen. Dies wurde in „Hash“ für Zeichenfolgen und „Bereich“ für Zahlen geändert. 
> 
> 

Weitere Informationen finden Sie unter [DocumentDB-Indizierungsrichtlinien](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indizierung für „Order By“ für alle Eigenschaften
Hier erfahren Sie, wie Sie eine Sammlung mit „All Range“-Indizierung für „Order By“ für alle bzw. alle numerischen Eigenschaften oder Zeichenfolgeneigenschaften erstellen, die in JSON-Dokumenten angezeigt werden. Hier legen wir für String-Werte „Range“ statt dem standardmäßig eingestellten Indextyp fest und verwenden die maximale Genauigkeit (-1).

    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

> [!NOTE]
> Beachten Sie, dass „Order By“ nur Ergebnisse der Datentypen (Zeichenfolge und Anzahl) ausgibt, die mit einem RangeIndex indiziert werden. Verwenden Sie z. B. die Standardindizierungsrichtlinie, die nur RangeIndex für Zahlen verwendet, wird „Order By“ für einen Pfad mit Zeichenfolgenwerten keine Dokumente zurückgeben.
> 
> 

### <a name="indexing-for-order-by-for-a-single-property"></a>Indizierung für "Order By" für eine einzelne Eigenschaft
Hier erfahren Sie, wie Sie eine Sammlung mit der Indizierung für „Order By“ für die „Title“-Eigenschaft erstellen können, bei der es sich um eine Zeichenfolge handelt. Hierfür gibt es zwei Pfade – einen für die „Title“-Eigenschaft (/Title/?) mit der Bereichsindizierung und den anderen für jede andere Eigenschaft mit dem Standardindizierungsschema („Hash“ für Zeichenfolgen und „Bereich“ für Zahlen).                    

    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Beispiele
Sehen Sie sich dieses [GitHub-Beispielprojekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) an, das veranschaulicht, wie „Order By“ verwendet wird, einschließlich Erstellung von Richtlinien für die Indizierung und Paging mit „Order By“. Diese Beispiele sind Open-Source-basiert, und wir freuen uns, wenn Sie Pullanforderungen mit Beiträgen senden, von denen andere DocumentDB-Entwickler profitieren können. In den [Anleitungen für Beiträge](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) finden Sie Informationen dazu, wie Sie beitragen können.  

## <a name="faq"></a>Häufig gestellte Fragen
**Was ist der erwartete Anforderungseinheitsverbrauch (Request Unit, RU) der "Order By"-Abfragen?**

Da "Order By" den DocumentDB-Index für Suchvorgänge verwendet, wird die Anzahl der Anforderungseinheiten, die von "Order By"-Abfragen verwendet werden, ungefähr den Abfragen ohne "Order By" entsprechen. Die Anzahl der Anforderungseinheiten hängt wie bei jedem anderen Vorgang für DocumentDB von den Größen/Formen Dokumente sowie von der Komplexität der Abfrage ab. 

**Was ist der erwartete Indizierungsaufwand für "Order By"?**

Die Indizierungsspeicheraufwand verhält sich proportional zur Anzahl der Eigenschaften. Im schlimmsten Fall beträgt der Indizierungsaufwand 100 % der Daten. Es gibt keinen Unterschied beim Durchsatz (Anforderungseinheiten) zwischen Bereichs-/"Order By"-Indizierung und der Standardhashindizierung.

**Wie frage ich vorhandene Daten in DocumentDB mit "Order By" ab?**

Um die Ergebnisse der Abfrage mit „Order By“ zu sortieren, muss die Indizierungsrichtlinie der Sammlung so geändert werden, dass ein Bereichsindextyp auf die Eigenschaft angewendet wird, nach der sortiert wird. Weitere Informationen finden Sie unter [Ändern der Indizierungsrichtlinie](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Was sind die aktuellen Einschränkungen von "Order By"?**

„Order By“ kann nur für eine Eigenschaft angegeben werden (numerisch oder Zeichenfolgen), wenn der Bereich mit der maximalen. Genauigkeit (-1) indiziert wird.

Sie können Folgendes nicht durchführen:

* "Order By" mit internen Zeichenfolgeneigenschaften wie id, _rid und _self (demnächst verfügbar).
* "Order By" mit Eigenschaften, die vom Ergebnis einer dokumentinternen Verknüpfung abgeleitet werden (demnächst verfügbar).
* "Order By" mit mehreren Eigenschaften (demnächst verfügbar).
* „Order By“ mit Abfragen für Datenbanken, Sammlungen, Benutzer, Berechtigungen oder Anlagen (in Kürze verfügbar).
* "Order By" mit berechneten Eigenschaften, z. B. das Ergebnis eines Ausdrucks oder eine UDF-Funktion/integrierte Funktion.

„Order By“ wird derzeit nicht für partitionsübergreifende Abfragen unterstützt, wenn der Abfrage-Explorer im Azure-Portal verwendet wird.

## <a name="troubleshooting"></a>Problembehandlung
Wenn eine Fehlermeldung angezeigt wird, dass „Order By“ nicht unterstützt wird, stellen Sie sicher, dass Sie eine Version des [SDK](documentdb-sdk-dotnet.md) verwenden, die „Order By“ unterstützt. 

## <a name="next-steps"></a>Nächste Schritte
Verzweigen Sie das [GitHub-Beispielprojekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries), und starten Sie die Sortierung Ihrer Daten. 

## <a name="references"></a>Referenzen
* [DocumentDB-Abfragereferenz](documentdb-sql-query.md)
* [DocumentDB-Indizierungsrichtlinienreferenz](documentdb-indexing-policies.md)
* [DocumentDB-SQL-Referenz](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB-"Order By"-Beispiele](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)


