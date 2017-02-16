---
title: Upgrade auf Version 1.1 des Azure Search .NET SDK | Microsoft Docs
description: "Upgrade auf Version 1.1 des Azure Search .NET SDK"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 66f89958-a320-4a24-87f9-69315848909f
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 3e2ad6b466ba4885ae14576b83d4c0f3010bab67
ms.openlocfilehash: 9782454e3bfc697b63cde8aa28a14be0c393c36b


---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Upgrade auf Version 3 des Azure Search .NET SDK
Wenn Sie die Version „2.0-preview“ oder eine frühere Version des [Azure Search .NET SDK](https://aka.ms/search-sdk) verwenden, unterstützt dieser Artikel Sie beim Upgrade Ihrer Anwendung auf Version 3.0.

Eine allgemeinere exemplarische Vorgehensweise für das SDK sowie Beispiele finden Sie unter [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md).

Version 3 des Azure Search .NET SDK enthält einige Änderungen zu früheren Versionen. Dabei handelt es sich hauptsächlich um kleinere Änderungen, sodass zum Ändern Ihres Codes nur ein geringer Aufwand erforderlich sein sollte. Anweisungen zum Ändern Ihres Codes, um die neue SDK-Version zu verwenden, finden Sie unter [Schritte zum Upgrade](#UpgradeSteps) .

> [!NOTE]
> Wenn Sie Version 1.0.2-preview oder eine frühere Version verwenden, sollten Sie zuerst auf Version 1.1 und dann auf Version 3 upgraden. Anweisungen finden Sie unter [Anhang: Schritte zum Upgrade auf Version 1.1](#UpgradeStepsV1) .
>
> Ihre Azure Search-Dienstinstanz unterstützt mehrere REST-API-Versionen, einschließlich der neuesten. Sie können auch ältere Versionen weiterhin verwenden, aber es wird empfohlen, den Code zur neuesten Version zu migrieren. Wenn Sie die REST-API verwenden, müssen Sie die API-Version bei jeder Anforderung über den api-version-Parameter angeben. Wenn Sie das .NET SDK verwenden, bestimmt die Version des von Ihnen verwendeten SDK die Version der REST-API. Wenn Sie ein älteres SDK verwenden, können Sie diesen Code weiterhin ohne Änderungen ausführen, selbst wenn der Dienst aktualisiert wird, um eine neuere Version der API zu unterstützen.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Neuerungen in Version 3
Version 3 des Azure Search .NET SDK ist für die neueste allgemein verfügbare Version der Azure Search-REST-API (2016-09-01) konzipiert. Dadurch können viele neue Features von Azure Search in einer .NET-Anwendung verwendet werden. Hierzu zählen etwa folgende:

* [Benutzerdefinierte Analysen](https://aka.ms/customanalyzers)
* Unterstützung von Indexern für [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) und [Azure Table Storage](search-howto-indexing-azure-tables.md)
* Anpassen von Indexern über [Feldzuordnungen](search-indexer-field-mappings.md)
* ETags-Unterstützung für sichere gleichzeitige Aktualisierung von Indexdefinitionen, Indexern und Datenquellen
* Unterstützung der deklarativen Erstellung von Indexfelddefinitionen mittels Dekorierung Ihrer Modellklasse und Verwendung der neuen `FieldBuilder`-Klasse.
* Unterstützung für .NET Core und .NET Portable Profile 111

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Schritte zum Upgrade
Aktualisieren Sie zunächst die NuGet-Referenz für `Microsoft.Azure.Search` , indem Sie entweder die NuGet-Paket-Manager-Konsole verwenden oder mit der rechten Maustaste auf die Projektverweise klicken und in Visual Studio „NuGet-Pakete verwalten...“ auswählen.

Nachdem NuGet die neuen Pakete und deren Abhängigkeiten heruntergeladen hat, erstellen Sie Ihr Projekt neu. Je nachdem, wie Ihr Code strukturiert ist, verläuft die Neuerstellung erfolgreich. Wenn dies der Fall ist, sind Sie startbereit!

Wenn beim Erstellen ein Fehler auftritt, wird etwa folgende Buildfehlermeldung angezeigt:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Der nächste Schritt besteht darin, diesen Buildfehler zu beheben. Unter [Wichtige Änderungen in Version 3](#ListOfChanges) finden Sie Informationen zur Fehlerursache und -behebung.

Möglicherweise werden zusätzliche Buildwarnungen im Zusammenhang mit veralteten Methoden oder Eigenschaften angezeigt. Die Warnung enthält Anweisungen dazu, was Sie anstelle der veralteten Funktion verwenden sollten. Wenn Ihre Anwendung z.B. die `IndexingParameters.Base64EncodeKeys`-Eigenschaft verwendet, sollten Sie folgende Warnung erhalten: `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Sobald Sie alle Buildfehler behoben haben, können Sie Änderungen an Ihrer Anwendung vornehmen, um nach Bedarf die neue Funktionalität zu nutzen. Neue Funktionen im SDK werden ausführlich unter [Neuerungen in Version 3](#WhatsNew) behandelt.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Wichtige Änderungen in Version 3
Version 3 enthält nur einige wenige wichtige Änderungen, die neben der Neuerstellung der Anwendung ggf. auch Codeänderungen erfordern.

### <a name="indexesgetclient-return-type"></a>Rückgabetyp Indexes.GetClient
Die `Indexes.GetClient` -Methode hat einen neuen Rückgabetyp. Zuvor wurde `SearchIndexClient` zurückgegeben, dies wurde in Version 2.0-preview jedoch in `ISearchIndexClient` geändert, und diese Änderung wird auch in Version 3 übernommen. Dies dient zur Unterstützung von Kunden, die die `GetClient`-Methode für Komponententests durch Rückgabe einer simulierten Implementierung von `ISearchIndexClient` simulieren möchten.

#### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>„AnalyzerName“, „DataType“ und andere können nicht mehr implizit in Zeichenfolgen konvertiert werden.
Viele Typen im Azure Search .NET SDK werden von `ExtensibleEnum` abgeleitet. Zuvor konnten diese Typen alle implizit in den Typ `string` konvertiert werden. In der `Object.Equals`-Implementierung für diese Klassen wurde allerdings ein Fehler entdeckt, dessen Behebung die Deaktivierung dieser impliziten Konvertierung erforderlich machte. Die explizite Konvertierung in `string` ist jedoch weiterhin zulässig.

#### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Veraltete Member wurden entfernt.

Unter Umständen treten Buildfehler im Zusammenhang mit Methoden oder Eigenschaften auf, die in Version 2.0-preview als veraltet markiert waren und nun in Version 3 entfernt wurden. Solche Fehler lassen sich wie folgt beheben:

- Falls Sie den Konstruktor `ScoringParameter(string name, string value)` verwendet haben, verwenden Sie stattdessen Folgendes: `ScoringParameter(string name, IEnumerable<string> values)`.
- Falls Sie die `ScoringParameter.Value`-Eigenschaft verwendet haben, verwenden Sie stattdessen die `ScoringParameter.Values`-Eigenschaft oder die `ToString`-Methode.
- Falls Sie die `SearchRequestOptions.RequestId`-Eigenschaft verwendet haben, verwenden Sie stattdessen die `ClientRequestId`-Eigenschaft.

### <a name="removed-preview-features"></a>Entfernte Vorschaufeatures

Hinweis für Benutzer, die ein Upgrade von Version 2.0-preview auf Version 3 durchführen: Die JSON- und CSV-Analyseunterstützung für Blob-Indexer wurde entfernt, da sich dieses Feature noch in der Vorschauphase befindet. Im Einzelnen wurden folgende Methoden der `IndexingParametersExtensions`-Klasse entfernt:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Falls bei Ihrer Anwendung eine harte Abhängigkeit von diesen Features besteht, können Sie kein Upgrade auf Version 3 des Azure Search .NET SDK durchführen. Sie können weiterhin Version 2.0-preview verwenden. Beachten Sie jedoch, dass die Verwendung von Vorschau-SDKs in Produktionsanwendungen **nicht empfehlenswert** ist. Vorschaufeatures dienen ausschließlich zur Evaluierung und ändern sich unter Umständen.

## <a name="conclusion"></a>Zusammenfassung
Weitere Informationen zur Verwendung des Azure Search .NET SDK finden Sie in unseren kürzlich aktualisierten [Hilfe und Anleitung](search-howto-dotnet-sdk.md)-Artikeln.

Wir freuen uns auf Ihr Feedback zum SDK! Wenn Probleme auftreten, können Sie sich gerne über das [Azure Search-MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch)an uns wenden. Wenn Sie einen Fehler finden, können Sie das Problem im [Azure .NET SDK-GitHub-Repository](https://github.com/Azure/azure-sdk-for-net/issues)melden. Stellen Sie sicher, dass Sie dem Titel des Problems „Search SDK: “ voranstellen.

Vielen Dank für die Nutzung von Azure Search!

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-to-upgrade-to-version-11"></a>Anhang: Schritte zum Upgrade auf Version 1.1
> [!NOTE]
> Dieser Abschnitt betrifft nur Benutzer der Azure Search .NET SDK-Version 1.0.2-preview und älterer Versionen.
> 
> 

Aktualisieren Sie zunächst die NuGet-Referenz für `Microsoft.Azure.Search` , indem Sie entweder die NuGet-Paket-Manager-Konsole verwenden oder mit der rechten Maustaste auf die Projektverweise klicken und in Visual Studio „NuGet-Pakete verwalten...“ auswählen.

Nachdem NuGet die neuen Pakete und deren Abhängigkeiten heruntergeladen hat, erstellen Sie Ihr Projekt neu.

Wenn Sie zuvor die Versionen 1.0.0-preview, 1.0.1-preview oder 1.0.2-preview verwendet haben, sollte die Erstellung erfolgreich sein, und Sie können direkt anfangen!

Wenn Sie zuvor Version 0.13.0-preview oder älter verwendet haben, sollten Erstellungsfehler ähnlich den folgenden angezeigt werden:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Der nächste Schritt besteht darin, die Buildfehler einzeln zu beheben. Für die meisten müssen einige Klassen- und Methodennamen geändert werden, die im SDK umbenannt wurden. [Liste mit wichtigen Änderungen in Version 1.1](#ListOfChangesV1) enthält eine Liste mit diesen Namensänderungen.

Wenn Sie benutzerdefinierte Klassen zum Modellieren von Dokumenten verwenden und diese Klassen Eigenschaften mit primitiven Datentypen umfassen, die keine NULL-Werte zulassen (z.B. `int` oder `bool` in C#), sollten Sie eine Programmfehlerbehebung in der Version 1.1 des SDK kennen. Weitere Details finden Sie unter [Fehlerbehebungen in Version 1.1](#BugFixesV1).

Sobald Sie alle Buildfehler behoben haben, können Sie Änderungen an Ihrer Anwendung vornehmen, um nach Bedarf die neue Funktionalität zu nutzen.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Liste mit wichtigen Änderungen in Version 1.1
Die folgende Liste ist nach der Wahrscheinlichkeit sortiert, dass die Änderung sich auf den Anwendungscode auswirkt.

#### <a name="indexbatch-and-indexaction-changes"></a>Änderungen an IndexBatch und IndexAction
`IndexBatch.Create` wurde in `IndexBatch.New` umbenannt und umfasst kein `params`-Argument mehr. Sie können `IndexBatch.New` für Batches verwenden, in denen verschiedene Typen von Aktionen (Zusammenführen, Löschen usw.) kombiniert werden. Darüber hinaus stehen neue statische Methoden zum Erstellen von Batches zur Verfügung, in denen alle Aktionen identisch sind: `Delete`, `Merge`, `MergeOrUpload` und `Upload`.

`IndexAction` verfügt nicht länger über öffentliche Konstruktoren, und die zugehörigen Eigenschaften sind jetzt unveränderlich. Sie sollten die neuen statischen Methoden zum Erstellen von Aktionen für verschiedene Zwecke verwenden: `Delete`, `Merge`, `MergeOrUpload` und `Upload`. `IndexAction.Create` wurde entfernt. Wenn Sie die Überladung verwendet haben, die nur ein Dokument akzeptiert, müssen Sie stattdessen `Upload` verwenden.

##### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Wenn Sie möchten, können Sie ihn weiter vereinfachen:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>Änderungen an IndexBatchException
Die `IndexBatchException.IndexResponse`-Eigenschaft wurde umbenannt in `IndexingResults`, und ihr Typ ist jetzt `IList<IndexingResult>`.

##### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>Änderungen an der Vorgangsmethode
Jeder Vorgang im Azure Search .NET SDK wird als eine Reihe von Methodenüberladungen für synchrone und asynchrone Aufrufer verfügbar gemacht. Die Signaturen und die Faktorisierung dieser Methodenüberladungen wurden in der Version 1.1 geändert.

Durch den Vorgang „Indexstatistiken abrufen“ in älteren Versionen des SDK wurden beispielsweise folgende Signaturen verfügbar gemacht:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Die Methodensignaturen für denselben Vorgang sehen in Version 1.1 folgendermaßen aus:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Ab Version 1.1 werden Vorgangsmethoden in Azure Search .NET SDK anders organisiert:

* Optionale Parameter werden jetzt als Standardparameter statt als zusätzliche Methodenüberladungen modelliert. Dadurch verringert sich die Anzahl an Methodenüberladungen zum Teil erheblich.
* Bei den Erweiterungsmethoden werden jetzt viele irrelevante HTTP-Details für den Aufrufer ausgeblendet. Ältere Versionen des SDK gaben zum Beispiel ein Antwortobjekt mit einem HTTP-Statuscode zurück, den Sie häufig nicht überprüfen mussten, weil Vorgangsmethoden für jeden Statuscode, der einen Fehler angibt, `CloudException` auslösen. Die neuen Erweiterungsmethoden geben nur Modellobjekte zurück, sodass Sie sie nicht extra in Ihrem Code entpacken müssen.
* Umgekehrt machen die Kernschnittstellen jetzt Methoden verfügbar, die Ihnen mehr Steuerungsmöglichkeiten auf der HTTP-Ebene ermöglichen, wenn Sie sie benötigen. Sie können nun benutzerdefinierte HTTP-Header in Anforderungen übergeben, und der neue `AzureOperationResponse<T>`-Rückgabetyp bietet Ihnen direkten Zugriff auf die Elemente `HttpRequestMessage` und `HttpResponseMessage` für den Vorgang. `AzureOperationResponse` wird im `Microsoft.Rest.Azure`-Namespace definiert und ersetzt `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters-Änderungen
Eine neue Klasse mit dem Namen `ScoringParameter` wurde im letzten SDK hinzugefügt, damit es einfacher wird, für Bewertungsprofile in einer Suchabfrage Parameter anzugeben. Bisher hatte die `ScoringProfiles`-Eigenschaft der `SearchParameters`-Klasse die Typisierung `IList<string>`. Jetzt lautet die Typisierung `IList<ScoringParameter>`.

##### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Änderungen an Modellklassen
Aufgrund der Signaturänderungen, die unter [Änderungen an der Vorgangsmethode](#OperationMethodChanges) beschrieben werden, wurden viele Klassen im `Microsoft.Azure.Search.Models`-Namespace umbenannt oder entfernt. Beispiel:

* `IndexDefinitionResponse` wurde durch `AzureOperationResponse<Index>` ersetzt.
* `DocumentSearchResponse` wurde in `DocumentSearchResult` umbenannt.
* `IndexResult` wurde in `IndexingResult` umbenannt.
* `Documents.Count()` gibt jetzt `long` mit der Dokumentanzahl anstelle von `DocumentCountResponse` zurück.
* `IndexGetStatisticsResponse` wurde in `IndexGetStatisticsResult` umbenannt.
* `IndexListResponse` wurde in `IndexListResult` umbenannt.

Zusammenfassung: Es wurden von `OperationResponse`abgeleitete Klassen entfernt, die nur zum Umschließen eines Modellobjekts dienten. Bei den übrigen Klassen wurde das Suffix von `Response` in `Result` geändert.

##### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Antwortklassen und IEnumerable
Eine weitere Änderung, die sich auf Ihren Code auswirken kann, besteht darin, dass `IEnumerable<T>`von Antwortklassen mit darin enthaltenen Auflistungen nicht mehr implementiert wird. Stattdessen können Sie direkt auf die Auflistungseigenschaft zugreifen. Angenommen, Sie verwenden folgenden Code:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Sonderfall für Webanwendungen
Wenn Sie eine Webanwendung verwenden, die `DocumentSearchResponse` direkt serialisiert, um Suchergebnisse an den Browser zu senden, müssen Sie zum korrekten Serialisieren der Ergebnisse den Code ändern. Angenommen, Sie verwenden folgenden Code:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Sie können ihn durch Abrufen der `.Results` -Eigenschaft der Suchantwort ändern, um das Rendering der Suchergebnisse zu beheben:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Nach solchen Fällen müssen Sie selbst in Ihrem Code suchen. **Sie erhalten keine Warnung vom Compiler**, weil `JsonResult.Data` vom Typ `object` ist.

#### <a name="cloudexception-changes"></a>Änderungen an CloudException
Die `CloudException`-Klasse wurde vom `Hyak.Common`-Namespace in den `Microsoft.Rest.Azure`-Namespace verschoben. Darüber hinaus wurde die `Error`-Eigenschaft in `Body` umbenannt.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Änderungen an SearchServiceClient und SearchIndexClient
Der Typ der `Credentials`-Eigenschaft wurde von `SearchCredentials` in die Basisklasse `ServiceClientCredentials` geändert. Wenn Sie auf die `SearchCredentials` eines `SearchIndexClient` oder `SearchServiceClient` zugreifen müssen, ist es ratsam, die neue `SearchCredentials`-Eigenschaft zu verwenden.

In älteren Versionen des SDK besaßen `SearchServiceClient` und `SearchIndexClient` Konstruktoren, die einen `HttpClient`-Parameter akzeptierten. Diese wurden durch Konstruktoren ersetzt, die einen `HttpClientHandler` und ein Array mit `DelegatingHandler`-Objekten akzeptieren. Dies erleichtert die Installation benutzerdefinierter Ereignishandler für die Vorverarbeitung von HTTP-Anforderungen, falls erforderlich.

Schließlich wurden die Konstruktoren, die `Uri` und `SearchCredentials` akzeptierten, geändert. Angenommen, Sie verwenden folgenden Code:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Beachten Sie auch, dass der Typ des Parameters mit den Anmeldeinformationen in `ServiceClientCredentials`geändert wurde. Dies wird sich wahrscheinlich nicht auf Ihren Code auswirken, weil `SearchCredentials` von `ServiceClientCredentials` abgeleitet wird.

#### <a name="passing-a-request-id"></a>Übergeben eine Anforderungs-ID
In früheren Versionen des SDK konnten Sie eine Anforderungs-ID für `SearchServiceClient` oder `SearchIndexClient` festlegen, die dann in jede Anforderung an die REST-API integriert wurde. Dies ist für die Behandlung von Problemen mit Ihrem Suchdienst nützlich, wenn Sie sich an den Support wenden müssen. Es ist jedoch noch nützlicher, eine eindeutige Anforderungs-ID für jeden Vorgang festzulegen, statt dieselbe ID für alle Vorgänge zu verwenden. Aus diesem Grund wurden die `SetClientRequestId`-Methoden von `SearchServiceClient` und `SearchIndexClient` entfernt. Über den optionalen `SearchRequestOptions` -Parameter können Sie stattdessen für jede Vorgangsmethode eine Anforderungs-ID übergeben.

> [!NOTE]
> In einer zukünftigen Version des SDK fügen wir einen neuen Mechanismus für die globale Festlegung einer Anforderungs-ID für die Clientobjekte hinzu. Dieser entspricht dem Ansatz, der von anderen Azure SDKs verwendet wird.
> 
> 

#### <a name="example"></a>Beispiel
Angenommen, Sie verwenden folgenden Code:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Zum Beheben von Buildfehlern können Sie diesen Code folgendermaßen ändern:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Änderungen an Schnittstellennamen
Die Schnittstellennamen der Vorgangsgruppen wurden alle geändert und stimmen jetzt mit ihren entsprechenden Eigenschaftsnamen überein:

* Der Typ von `ISearchServiceClient.Indexes` wurde von `IIndexOperations` in `IIndexesOperations` umbenannt.
* Der Typ von `ISearchServiceClient.Indexers` wurde von `IIndexerOperations` in `IIndexersOperations` umbenannt.
* Der Typ von `ISearchServiceClient.DataSources` wurde von `IDataSourceOperations` in `IDataSourcesOperations` umbenannt.
* Der Typ von `ISearchIndexClient.Documents` wurde von `IDocumentOperations` in `IDocumentsOperations` umbenannt.

Diese Änderung wird sich wahrscheinlich nicht auf Ihren Code auswirken, es sei denn, Sie haben Modelle dieser Schnittstellen zu Testzwecken erstellt.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Fehlerbehebungen in Version 1.1
In älteren Versionen des Azure Search .NET SDK lag ein Fehler in Bezug auf die Serialisierung benutzerdefinierter Modellklassen vor. Der Fehler konnte auftreten, wenn Sie eine benutzerdefinierte Modellklasse mit einer Eigenschaft erstellt haben, deren Typ keine NULL-Werte zuließ.

#### <a name="steps-to-reproduce"></a>Schritte zum Reproduzieren
Erstellen Sie eine benutzerdefinierte Modellklasse mit einer Eigenschaft, deren Typ keine NULL-Werte zulässt. Fügen Sie z.B. eine öffentliche `UnitCount`-Eigenschaft vom Typ `int` anstelle von `int?` hinzu.

Wenn Sie ein Dokument mit dem Standardwert dieses Typs indizieren (z.B. 0 für `int`), lautet das Feld in Azure Search NULL. Wenn Sie anschließend nach diesem Dokument suchen, gibt der `Search`-Aufruf `JsonSerializationException` mit der Begründung aus, dass `null` nicht in `int` konvertiert werden kann.

Filter funktionieren möglicherweise auch nicht wie erwartet, da anstelle des gewünschten Werts NULL in den Index geschrieben wurde.

#### <a name="fix-details"></a>Details zur Korrektur
Wir haben dieses Problem in Version 1.1 des SDK behoben. Wenn Sie jetzt eine Modellklasse wie die folgende verwenden:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

und `IntValue` auf 0 setzen, wird dieser Wert jetzt bei der Übertragung richtig als 0 serialisiert und auch als 0 im Index gespeichert. Auch der Roundtrip funktioniert erwartungsgemäß.

Bei diesem Ansatz gibt es ein potenzielles Problem, das Sie beachten sollten: Wenn Sie einen Modelltyp mit einer Eigenschaft verwenden, die keine NULL-Werte zulässt, müssen Sie **garantieren** , dass keine Dokumente im Index für das entsprechende Feld einen NULL-Wert enthalten. Weder das SDK noch die Azure Search-REST-API hilft Ihnen, dies durchzusetzen.

Dieser Aspekt ist nicht nur hypothetischer Art: Stellen Sie sich ein Szenario vor, bei dem Sie ein neues Feld einem vorhandenen Index vom Typ `Edm.Int32`hinzufügen. Nach dem Aktualisieren der Indexdefinition besitzen alle Dokumente einen NULL-Wert für das neue Feld (da in Azure Search alle Typen NULL-Werte zulassen). Wenn Sie für dieses Feld anschließend eine Modellklasse mit einer `int`-Eigenschaft verwenden, die keine NULL-Werte zulässt, erhalten Sie beim Abrufen von Dokumenten folgendes `JsonSerializationException`-Element:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund empfehlen wir als bewährte Methode in den Modellklassen die Verwendung von Typen, die NULL-Werte zulassen.

Weitere Informationen zu diesem Fehler und der entsprechenden Korrektur finden Sie auf [GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).




<!--HONumber=Jan17_HO2-->


