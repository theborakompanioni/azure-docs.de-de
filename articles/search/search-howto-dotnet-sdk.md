---
title: Verwenden von Azure Search aus einer .NET-Anwendung | Microsoft Docs
description: Verwenden von Azure Search aus einer .NET-Anwendung
services: search
documentationcenter: 
author: brjohnstmsft
manager: jlembicz
editor: 
ms.assetid: 93653341-c05f-4cfd-be45-bb877f964fcb
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.translationtype: Human Translation
ms.sourcegitcommit: 125f05f5dce5a0e4127348de5b280f06c3491d84
ms.openlocfilehash: 552a7ab193e12d2e72da494166d774e974c85d47
ms.contentlocale: de-de
ms.lasthandoff: 05/22/2017


---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Verwenden von Azure Search aus einer .NET-Anwendung
In diesem Artikel erfahren Sie, wie Sie Ihr [Azure Search-.NET-SDK](https://aka.ms/search-sdk)schnell betriebsbereit machen. Dank .NET-SDK erhalten Sie in Ihrer Anwendung die vielfältigen Suchfunktionen von Azure Search.

## <a name="whats-in-the-azure-search-sdk"></a>Inhalt des Azure Search-SDK
Das SDK enthält die Clientbibliothek `Microsoft.Azure.Search`. Mit dem SDK können Sie Ihre Indizes, Datenquellen und Indexer verwalten, Dokumente hochladen und verwalten und Abfragen ausführen, ohne sich mit den Details von HTTP und JSON befassen zu müssen.

Die Clientbibliothek definiert Klassen wie `Index`, `Field` und `Document` sowie Operationen wie `Indexes.Create` und `Documents.Search` für die Klassen `SearchServiceClient` und `SearchIndexClient`. Diese Klassen sind in die folgenden Namespaces aufgeteilt:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

Die aktuelle Version des Azure Search .NET-SDK ist nun allgemein verfügbar. Wir freuen uns sehr über Ihr Feedback, das wir in der nächsten Version des Programms zu berücksichtigen versuchen. Sie können uns dieses über die [Seite „Feedback“](https://feedback.azure.com/forums/263029-azure-search/) bereitstellen.

Das .NET-SDK unterstützt Version `2016-09-01` der [Azure Search-REST-API](https://docs.microsoft.com/rest/api/searchservice/). Diese Version unterstützt jetzt benutzerdefinierte Analysen sowie Azure-Blob- und Azure-Tabellenindexer. Vorschaufeatures, die in dieser Version *nicht* enthalten sind, z.B. Unterstützung für die Indizierung von JSON- und CSV-Dateien, gehören zur [Vorschau](search-api-2015-02-28-preview.md) und sind über die ältere [Version 2.0-Vorschau von .NET SDK](https://aka.ms/search-sdk-preview) verfügbar.

Dieses SDK unterstützt keine [Verwaltungsvorgänge](https://docs.microsoft.com/rest/api/searchmanagement/) wie etwa das Erstellen und Skalieren von Suchdiensten und das Verwalten von API-Schlüsseln. Wenn Sie Ihre Ressourcen für die Suche in einer .NET-Anwendung verwalten müssen, können Sie das [Azure Search-.NET Management SDK](https://aka.ms/search-mgmt-sdk) verwenden.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Aktualisieren auf die neueste Version des SDK
Wenn Sie bereits eine ältere Version des Azure Search-.NET SDK nutzen und auf die neue allgemein verfügbare Version aktualisieren möchten, finden Sie [in diesem Artikel](search-dotnet-sdk-migration.md) eine entsprechende Anleitung.

## <a name="requirements-for-the-sdk"></a>Anforderungen für das SDK
1. Visual Studio 2017
2. Ihr eigener Azure Search-Dienst. Zur Verwendung des SDK benötigen Sie den Namen Ihres Dienstes und mindestens einen API-Schlüssel. [Create a service in the portal](search-create-service-portal.md) (+++Erstellen eines Dienstes im Portal).
3. Laden Sie das [NuGet-Paket](http://www.nuget.org/packages/Microsoft.Azure.Search) mit dem Azure Search-.NET-SDK in Visual Studio mithilfe von „NuGet-Pakete verwalten“ herunter. Suchen Sie unter NuGet.org nach dem Paketnamen `Microsoft.Azure.Search` .

Das Azure Search .NET SDK unterstützt Anwendungen für .NET Framework 4.6 und .NET Core.

## <a name="core-scenarios"></a>Schlüsselszenarien
In Ihrer Suchanwendung müssen Sie verschiedene Aktionen ausführen. In diesem Zusammenhang werden in diesem Lernprogramm die folgenden Schlüsselszenarien behandelt:

* Erstellen eines Index
* Füllen des Index mit Dokumenten
* Suche nach Dokumenten mit Volltextsuche und Filtern

Im nachfolgenden Beispielcode werden all diese Aktionen vorgeführt. Die Codeausschnitte können Sie gerne in Ihrer eigenen Anwendung übernehmen.

### <a name="overview"></a>Übersicht
Die nachfolgend untersuchte Beispielanwendung erstellt einen neuen Index mit dem Namen „hotels“, füllt diesen mit Dokumenten und führt danach einige Suchabfragen aus. Hier das Hauptprogramm mit dem allgemeinen Ablauf:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> Den vollständigen Quellcode der in diesem Artikel besprochenen Beispielanwendung finden Sie auf [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

Dieses gehen wir nun Schritt für Schritt durch. Zunächst muss das Objekt `SearchServiceClient`erstellt werden. Mit diesem Objekt können Indizes verwaltet werden. Zur Erstellung dieses Objekts müssen Sie Ihren Azure Search-Dienstnamen sowie einen Admin-API-Schlüssel angeben. Sie können diese Informationen in der Datei `appsettings.json` der [Beispielanwendung](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) eingeben.

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> Wenn Sie einen falschen Schlüssel angeben (z. B. einen Abfrageschlüssel, statt eines Admin-Schlüssels), gibt `SearchServiceClient` beim ersten Aufruf einer Operationsmethode (z. B. eines `Indexes.Create`) eine `CloudException` mit der Fehlermeldung „Forbidden“ (Unzulässig) aus. Überprüfen Sie in diesem Fall unsere API-Schlüssel.
> 
> 

Durch die nächsten Zeilen werden Methoden zum Erstellen eines Index mit dem Namen „hotels“ erstellt, wobei dieser, sofern er bereits vorhanden ist, zuerst gelöscht wird. Auf diese Methoden gehen wir später ein.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

Danach muss der Index gefüllt werden. Dazu benötigen wir einen `SearchIndexClient`. Dieses Objekt erhalten Sie entweder, indem Sie es erstellen oder indem Sie für `SearchServiceClient` `Indexes.GetClient` aufrufen. Der Einfachheit halber verwenden wir die zweite Methode.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> In einer typischen Suchanwendung erfolgen Verwaltung und Füllung des Index getrennt von Suchabfragen. `Indexes.GetClient` eignet sich zum Auffüllen eines Indexes, da Sie keine weiteren `SearchCredentials` bereitstellen müssen. Vielmehr wird der Admin-Schlüssel, den Sie zum Erstellen des Objekts `SearchServiceClient` verwendet haben, auf das neue `SearchIndexClient`-Objekt übertragen. In dem Teil der Anwendung, in dem Abfragen ausgeführt werden, sollte das Objekt `SearchIndexClient` hingegen direkt erstellt werden, damit anstelle des Admin-Schlüssels ein Abfrageschlüssel übergeben werden kann. Dies entspricht dem Prinzip der geringsten Rechte und trägt dazu bei, die Anwendung sicherer zu machen. Weitere Informationen zu Admin- und Abfrageschlüsseln erhalten Sie [hier](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Nachdem das Objekt `SearchIndexClient`erstellt ist, kann der Index gefüllt werden. Dies erfolgt durch eine weitere Methode, die wir später besprechen.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Zum Schluss führen wir einige Suchabfragen aus und zeigen die Ergebnisse an. Dieses Mal verwenden wir einen anderen `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Wir werden uns die Methode `RunQueries` später genauer ansehen. Hier ist der Code zum Erstellen des neuen `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Diesmal verwenden wir einen Abfrageschlüssel, da wir keinen Schreibzugriff auf den Index benötigen. Sie können diese Informationen in der Datei `appsettings.json` der [Beispielanwendung](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) eingeben.

Wenn Sie diese Anwendung mit einem gültigen Dienstnamen und API-Schlüsseln ausführen, sollte die Ausgabe wie folgt aussehen:

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
    Search the entire index for the term 'budget' and return only the hotelName field:
    
    Name: Roach Motel
    
    Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:
    
    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river
    
    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:
    
    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00
    
    Search the entire index for the term 'motel':
    
    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
    
    Complete.  Press any key to end application...

Der vollständige Quellcode der Anwendung wird am Ende dieses Artikels bereitgestellt.

Nun sehen wir uns die von `Main`aufgerufenen Methoden näher an.

### <a name="creating-an-index"></a>Erstellen eines Index
Nachdem das Objekt `SearchServiceClient` erstellt ist, löscht `Main` den Index „hotels“, sofern dieser bereits vorhanden ist. Dies erfolgt mit folgender Methode:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Diese Methode verwendet das bereitgestellte Objekt `SearchServiceClient` , um zu überprüfen, ob der Index vorhanden ist, und löscht ihn gegebenenfalls.

> [!NOTE]
> Der Beispielcode in diesem Artikel verwendet der Einfachheit halber die synchronen Methoden des Azure Search-.NET-SDK. Für Ihre eigenen Anwendungen empfehlen wir aus Gründen der Skalierbarkeit und Reaktionsfähigkeit die asynchronen Methoden. In der oben gezeigten Methode können Sie `Exists` und `Delete` zum Beispiel auch durch `ExistsAsync` und `DeleteAsync` ersetzen.
> 
> 

Als Nächstes ruft `Main` die folgende Methode auf, um einen neuen Index mit dem Namen „hotels“ zu erstellen:

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

Diese Methode erstellt ein neues `Index`-Objekt mit einer Liste von `Field`-Objekten, die das Schema des neuen Index definiert. Jedes Feld weist einen Namen, einen Datentyp und mehrere Attribute auf, die das Suchverhalten des Felds definieren. Die Klasse `FieldBuilder` verwendet Reflektion, um durch Untersuchen der öffentlichen Eigenschaften und Attribute der entsprechenden `Hotel`-Modellklasse eine Liste von `Field`-Objekten für den Index zu erstellen. Wir werden uns die Klasse `Hotel` später genauer ansehen.

> [!NOTE]
> Sie können die Liste der `Field`-Objekte immer direkt erstellen, anstatt `FieldBuilder` bei Bedarf zu verwenden. Sie möchten z.B. vielleicht keine Modellklasse verwenden oder Sie müssen möglicherweise eine vorhandene Modellklasse verwenden, die Sie nicht durch Hinzufügen von Attributen ändern möchten.
>
> 

Neben Feldern können Sie dem Index auch Wertungsprofile, Vorschläge oder CORS-Optionen hinzufügen, die im Beispiel fehlen, um es einfach zu halten. Weitere Informationen zum Indexobjekt und seinen Bestandteilen finden Sie in der [SDK-Referenz](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index) sowie in der [Azure Search-REST-API-Referenz](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>Füllen des Index
Im nächsten Schritt in `Main` wird der neu erstellte Index gefüllt. Dies erfolgt mit folgender Methode:

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        { 
            HotelId = "1", 
            BaseRate = 199.0, 
            Description = "Best hotel in town",
            DescriptionFr = "Meilleur hôtel en ville",
            HotelName = "Fancy Stay",
            Category = "Luxury", 
            Tags = new[] { "pool", "view", "wifi", "concierge" },
            ParkingIncluded = false, 
            SmokingAllowed = false,
            LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero), 
            Rating = 5, 
            Location = GeographyPoint.Create(47.678581, -122.131577)
        },
        new Hotel()
        { 
            HotelId = "2", 
            BaseRate = 79.99,
            Description = "Cheapest hotel in town",
            DescriptionFr = "Hôtel le moins cher en ville",
            HotelName = "Roach Motel",
            Category = "Budget",
            Tags = new[] { "motel", "budget" },
            ParkingIncluded = true,
            SmokingAllowed = true,
            LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
            Rating = 1,
            Location = GeographyPoint.Create(49.678581, -122.131577)
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close to town hall and the river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

Diese Methode besteht aus vier Teilen. Im ersten Teil wird ein `Hotel` -Objektarray erstellt, das die Eingabedaten für den Upload in den Index bereitstellt. Diese Daten sind der Einfachheit halber fest codiert. In Ihrer eigenen Anwendung stammen die Daten vermutlich aus einer externen Datenquelle, beispielsweise aus einer SQL-­Datenbank.

Im zweiten Teil wird ein `IndexBatch` mit den Dokumenten erstellt. Sie geben den Vorgang an, den Sie auf den Batch zum Zeitpunkt seiner Erstellung anwenden möchten, in diesem Fall durch Aufruf von `IndexBatch.Upload`. Der Batch wird dann durch die Methode `Documents.Index` in den Azure Search-Index hochgeladen.

> [!NOTE]
> In diesem Beispiel werden nur Dokumente hochgeladen. Wenn Sie z.B. Änderungen in vorhandenen Dokumenten zusammenführen oder Dokumente löschen möchten, können Sie zum Erstellen von Batches `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` oder `IndexBatch.Delete` aufrufen. Sie können auch verschiedene Vorgänge in einem einzigen Batch kombinieren, indem Sie `IndexBatch.New` aufrufen. Dieses akzeptiert eine Auflistung von `IndexAction`-Objekten, und jedes dieser Objekte weist Azure Search an, einen bestimmten Vorgang für das Dokument auszuführen. Sie können jede `IndexAction` mit ihrem eigenen Vorgang erstellen, indem Sie die entsprechende Methode aufrufen, z.B. `IndexAction.Merge`, `IndexAction.Upload` usw.
> 
> 

Der dritte Teil dieser Methode ist ein Catch-Block, der einen wichtigen Fehlerfall bei der Indizierung abfängt. Falls der Azure Search-Dienst Dokumente des Batch nicht indizieren kann, löst `Documents.Index` eine `IndexBatchException` aus. Dies kann bei der Indizierung von Dokumenten geschehen, wenn der Dienst stark ausgelastet ist. **Es wird dringend empfohlen, diesen Fall in Ihrem Code explizit zu behandeln.** Zum Beispiel kann die Indizierung der zuvor nicht indizierten Dokumente nach einer Weile wieder aufgenommen werden oder der Vorgang kann, wie im Beispiel gezeigt, nach der Aufzeichnung des Fehlers fortgeführt werden. Je nach Datenkonsistenzanforderungen Ihrer Anwendung sind aber auch andere Lösungen möglich.

> [!NOTE]
> Sie können die Methode `FindFailedActionsToRetry` zum Erstellen eines neuen Batches verwenden, der nur die Aktionen enthält, die in einem vorherigen Aufruf von `Index` zu einem Fehler geführt haben. Die Methode wird [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) behandelt, und es gibt eine Erläuterung zu ihrer ordnungsgemäßen Verwendung [auf StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Der letzte Teil der Methode `UploadDocuments` fügt eine Verzögerung von zwei Sekunden hinzu. Da die Indizierung in Ihrem Azure Search-Dienst asynchron erfolgt, muss die Beispielanwendung einen Augenblick warten, damit sichergestellt ist, dass die Dokumente für Suchen zur Verfügung stehen. Verzögerungen wie diese sind in der Regel nur in Demos, Tests und Beispielanwendungen erforderlich.

#### <a name="how-the-net-sdk-handles-documents"></a>Behandeln von Dokumenten durch das .NET-SDK
Vielleicht fragen Sie sich, wie das Azure Search-.NET-SDK Instanzen einer benutzerdefinierten Klasse wie `Hotel` in einen Index hochladen kann. Um diese Frage zu beantworten, sehen wir uns die Klasse `Hotel` an:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Das Erste, was Sie sehen, ist, dass jede öffentliche Eigenschaft von `Hotel` einem Feld in der Indexdefinition entspricht, allerdings mit einem bedeutsamen Unterschied: die Namen der Felder beginnen mit einem Kleinbuchstaben („camel-Schreibweise“), während die Namen der öffentlichen Eigenschaften von `Hotel` mit einem Großbuchstaben („Pascal-Schreibweise“) beginnen. Dies ist ein typisches Szenario für .NET-Anwendungen, die Datenbindungen durchführen, wenn das Zielschema außerhalb der Kontrolle des Anwendungsentwicklers liegt. Anstatt nun die Namenskonventionen von .NET mit Eigenschaftsnamen in camel-Schreibweise zu verletzen, können Sie das SDK mit dem Attribut `[SerializePropertyNamesAsCamelCase]` anweisen, Eigenschaftsnamen automatisch der camel-Schreibweise zuzuordnen.

> [!NOTE]
> Das Azure Search .NET SDK verwendet die [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) -Bibliothek, um die Objekte Ihres benutzerdefinierten Modells nach JSON zu serialisieren und aus JSON zu deserialisieren. Sie können diese Serialisierung bei Bedarf anpassen. Weitere Informationen finden Sie unter [Benutzerdefinierte Serialisierung mit JSON.NET](#JsonDotNet).
> 
> 

Zu beachten sind außerdem die Attribute, z.B. `IsFilterable`, `IsSearchable`, `Key` und `Analyzer`, die jede öffentliche Eigenschaft ergänzen. Diese Attribute sind den [entsprechenden Attributen des Azure Search-Indexes](https://docs.microsoft.com/rest/api/searchservice/create-index#request) direkt zugeordnet. Die Klasse `FieldBuilder` verwendet diese, um Felddefinitionen für den Index zu erstellen.

Der dritte bemerkenswerte Punkt an der Klasse `Hotel` sind die Datentypen der öffentlichen Eigenschaften. Die .NET-Typen dieser Eigenschaften stimmen mit den entsprechenden Feldtypen in der Indexdefinition überein. Die Zeichenfolgeeigenschaft `Category` passt zum Beispiel zum Feld `category`, das den Typ `Edm.String` hat. Ähnliche Zuordnungen bestehen auch zwischen `bool?` und `Edm.Boolean`, `DateTimeOffset?`, `Edm.DateTimeOffset` usw. Die jeweiligen Regeln für die Zuordnung eines Typs sind in der [Referenz zum Azure Search .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) unter der `Documents.Get`-Methode dokumentiert. Die Klasse `FieldBuilder` übernimmt diese Zuordnung für Sie, aber es kann dennoch hilfreich sein, den Vorgang zu verstehen, falls Sie Serialisierungsprobleme beheben müssen.

Diese Möglichkeit, eigene Klassen als Dokumente zu verwenden, funktioniert in beide Richtungen. Denn ebenso können Sie das SDK, wie im nächsten Abschnitt gezeigt, beim Abrufen von Suchergebnissen anweisen, diese automatisch in einen Typ Ihrer Wahl zu deserialisieren.

> [!NOTE]
> Das Azure Search-.NET-SDK unterstützt durch die Klasse `Document`, einer Schlüssel-/Wertzuordnung von Feldnamen zu Feldwerten, auch dynamisch typisierte Dokumente. Diese Art der Typisierung eignet sich für Szenarien, in denen Sie bei der Entwicklung noch nicht wissen, wie das Indexschema aussehen soll, oder in denen eine Bindung an bestimmte Modellklassen unpassend ist. Alle Methoden des SDK, die Dokumente verarbeiten, verfügen über Überladungen, welche die Klasse `Document` verwenden, wie auch stark typisierte Überladungen, die einen generischen Typparameter verwenden. Nur letztere Art von Überladung wird im Beispielcode dieses Lernprogramms verwendet. Die Klasse `Document` erbt von `Dictionary<string, object>`. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).
> 
> 

**Argumente für die Verwendung von Nullable-Datentypen**

Beim Entwerfen eigener Modellklassen für die Zuordnung zum Azure Search-Index wird empfohlen, Eigenschaften von Werttypen, z. B. `bool` und `int`, als Eigenschaften zu deklarieren, die NULL-Werte zulassen (z. B. `bool?` anstelle von `bool`). Wenn Sie eine Eigenschaft verwenden, die keine NULL-Werte zulässt, müssen Sie **garantieren**, dass keine Dokumente im Index einen NULL-Wert für das entsprechende Feld enthalten. Weder das SDK noch der Azure Search-Dienst helfen Ihnen, dies durchzusetzen.

Dieser Aspekt ist nicht nur hypothetischer Art: Stellen Sie sich ein Szenario vor, bei dem Sie ein neues Feld einem vorhandenen Index vom Typ `Edm.Int32` hinzufügen. Nach dem Aktualisieren der Indexdefinition besitzen alle Dokumente einen NULL-Wert für das neue Feld (da in Azure Search alle Typen NULL-Werte zulassen). Wenn Sie für dieses Feld anschließend eine Modellklasse mit einer `int`-Eigenschaft verwenden, die keine NULL-Werte zulässt, erhalten Sie beim Abrufen von Dokumenten folgendes `JsonSerializationException`-Element:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund empfehlen wir als bewährte Methode, in Ihren Modellklassen Typen zu verwenden, die NULL-Werte zulassen.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Benutzerdefinierte Serialisierung mit JSON.NET
Das SDK verwendet JSON.NET zum Serialisieren und Deserialisieren von Dokumenten. Bei Bedarf können Sie die Serialisierung und Deserialisierung anpassen, indem Sie einen eigenen `JsonConverter` oder `IContractResolver` definieren (weitere Informationen finden Sie in der [JSON.NET-Dokumentation](http://www.newtonsoft.com/json/help/html/Introduction.htm)). Dies kann nützlich sein, wenn Sie eine vorhandene Modellklasse aus der Anwendung für die Verwendung mit Azure Search und andere fortgeschrittenere Szenarien anpassen möchten. Bei einer benutzerdefinierten Serialisierung bieten sich zum Beispiel folgende Möglichkeiten:

* Ein- oder Ausschließen bestimmter Eigenschaften der Modellklasse bei der Speicherung als Dokumentfelder
* Zuordnen zwischen Eigenschaftennamen im Code und Feldnamen im Index
* Erstellen Sie benutzerdefinierte Attribute, die für die Zuordnung von Eigenschaften zu Dokumentfelder verwendet werden können.

Beispiele für die Implementierung der benutzerdefinierten Serialisierung in den Unittests für das Azure Search .NET SDK finden Sie auf GitHub. Ein guter Ausgangspunkt ist [dieser Ordner](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Er enthält Klassen, die in den Tests für die benutzerdefinierte Serialisierung verwendet werden.

### <a name="searching-for-documents-in-the-index"></a>Suchen nach Dokumenten im Index
Im letzten Schritt der Beispielanwendung wird nach Dokumenten im Index gesucht. Dazu wird folgende Methode verwendet:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "hotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("budget", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "lastRenovationDate desc" },
            Select = new[] { "hotelName", "lastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'motel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);
}
```

Jedes Mal, wenn eine Abfrage ausgeführt wird, erstellt diese Methode zunächst ein neues `SearchParameters`-Objekt. Damit werden zusätzliche Abfrageoptionen wie Sortierung, Filter, Paging und Facettierung festgelegt. Bei dieser Methode legen die Eigenschaften `Filter`, `Select`, `OrderBy` und `Top` für verschiedene Abfragen fest. Alle `SearchParameters`-Eigenschaften wird [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters) behandelt.

Im nächsten Schritt wird die Suchabfrage bereits ausgeführt. Dazu wird die Methode `Documents.Search` verwendet: Für jede Abfrage übergeben wir den zu verwendenden Suchtext als Zeichenfolge (bzw. `"*"`, wenn kein Suchtext vorliegt) sowie die zuvor erstellten Suchparameter. Außerdem geben wir `Hotel` als Typparameter für `Documents.Search` an, wodurch das SDK angewiesen wird, die Dokumente im Suchergebnis in Objekte des Typs `Hotel` zu deserialisieren.

> [!NOTE]
> Weitere Informationen zur Syntax von Suchabfrageausdrücken finden Sie [hier](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Schließlich geht diese Methode nach jeder Abfrage durch alle Übereinstimmungen im Suchergebnis, wobei sie jedes Dokument auf der Konsole ausgibt:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Lassen Sie uns die einzelnen Abfragen einmal näher betrachten. Hier ist der Code zum Ausführen der ersten Abfrage:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

In diesem Fall suchen wir nach Hotels, für die das Wort „Budget“ gefunden wird. Nur die Hotelnamen entsprechend den Angaben des Parameters `Select` sollen zurückgegeben werden. Hier sehen Sie die Ergebnisse:

    Name: Roach Motel

Als Nächstes möchten wir die Hotels mit einem Übernachtungspreis von weniger als 150 € suchen, und nur die Hotel-ID und Beschreibung sollen zurückgegeben werden:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Diese Abfrage verwendet einen OData `$filter`-Ausdruck, `baseRate lt 150`, um die Dokumente im Index zu filtern. Weitere Informationen zur von Azure Search unterstützten OData-Syntax finden Sie [hier](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Hier sehen Sie die Ergebnisse der Abfrage:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

Anschließend möchten wir die zwei besten, in jüngster Zeit renovierten Hotels suchen und deren Hotelnamen und das Datum der letzten Renovierung anzeigen. Hier folgt der Code: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

In diesem Fall verwenden wir erneut OData-Syntax, um den Parameter `OrderBy` auf `lastRenovationDate desc` festzulegen. Wir legen zudem auch `Top` auf 2 fest, um sicherzustellen, dass wir nur die zwei obersten Dokumente abrufen. Wie zuvor geben wir `Select` an, um festzulegen, welche Felder zurückgegeben werden sollen.

Hier sehen Sie die Ergebnisse:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Zum Schluss möchten wir alle Hotels suchen, für die das Wort „Motel“ zutrifft:

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Dies sind die Ergebnisse mit allen Feldern, da wir die Eigenschaft `Select` nicht festgelegt haben:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Mit diesem Schritt ist das Lernprogramm abgeschlossen. Gerne können Sie sich aber noch weiter mit dem Programm vertraut machen. Im Abschnitt **Nächste Schritte** finden Sie eine Zusammenstellung weiterer Ressourcen zur Einarbeitung in Azure Search.

## <a name="next-steps"></a>Nächste Schritte
* Nutzen Sie die Referenzen für das [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) und die [REST-API](https://docs.microsoft.com/rest/api/searchservice/).
* Vertiefen Sie Ihre Kenntnisse mithilfe von [Videos und anderen Beispielen und Lernprogrammen](search-video-demo-tutorial-list.md).
* Lesen Sie den Abschnitt [Namenskonventionen](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) , um sich mit den Namensregeln für Objekte vertraut zu machen.
* Lesen Sie den Abschnitt zu den in Azure Search [unterstützten Datentypen](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) .

