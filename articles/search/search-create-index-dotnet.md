---
title: Erstellen eines Azure Search-Indexes mit dem .NET SDK | Microsoft Docs
description: Erstellen Sie einen Index im Code mithilfe des Azure Search .NET SDK.
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 455c4847893175c1091ae21fa22215fd1dd10c53
ms.openlocfilehash: a607ab6bf73f59f55109f9ee60ab69aa15d74db3


---
# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Erstellen eines Azure Search-Index mit dem .NET SDK
> [!div class="op_single_selector"]
> * [Übersicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

In diesem Artikel lernen Sie, wie Sie einen Azure Search-[Index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) mithilfe des [Azure Search-.NET SDK](https://aka.ms/search-sdk) erstellen.

Bevor Sie dieser Anleitung folgen und einen Index erstellen, müssen Sie einen [Azure Search-Dienst erstellen](search-create-service-portal.md).

Beachten Sie, dass der gesamte Beispielcode in diesem Artikel in C# geschrieben wurde. Den vollständigen Quellcode finden Sie [bei GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identifizieren des Admin-API-Schlüssels Ihres Azure Search-Diensts
Nachdem Sie einen Azure Search-Dienst bereitgestellt haben, sind Sie fast bereit, Anforderungen für Ihren Dienstendpunkt mithilfe des .NET SDK auszugeben. Zunächst müssen Sie aber einen der Admin-API-Schlüssel abrufen, die für den bereitgestellten Suchdienst generiert wurden. Das .NET SDK sendet diesen API-Schlüssel für jede Anforderung an Ihren Dienst. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, um die API-Schlüssel für Ihren Dienst zu ermitteln.
2. Wechseln Sie zum Blatt Ihres Azure Search-Diensts.
3. Klicken Sie auf das Schlüsselsymbol.

Der Dienst enthält *Admin-Schlüssel* und *Abfrageschlüssel*.

* Die primären und sekundären *Admin-Schlüssel* gewähren Ihnen Vollzugriff auf alle Vorgänge. Dazu zählen die Dienstverwaltung und das Erstellen und Löschen von Indizes, Indexern und Datenquellen. Ihnen stehen zwei Schlüssel zur Verfügung, damit Sie den sekundären Schlüssel weiterhin nutzen können, wenn Sie den primären Schlüssel neu generieren möchten, und umgekehrt.
* Die *Abfrageschlüssel* gewähren Ihnen Lesezugriff auf Indizes und Dokumente. Diese werden in der Regel auf Clientanwendungen verteilt, die Suchanfragen ausgeben.

Verwenden Sie zum Erstellen eines Indexes entweder den primären oder den sekundären Admin-Schlüssel.

<a name="CreateSearchServiceClient"></a>

## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Erstellen einer Instanz der SearchServiceClient-Klasse
Damit Sie das Azure Search .NET SDK verwenden können, müssen Sie eine Instanz der `SearchServiceClient` -Klasse erstellen. Diese Klasse verfügt über mehrere Konstruktoren. Für den gewünschten Konstruktor werden der Name Ihres Suchdiensts und ein `SearchCredentials` -Objekt als Parameter verwendet. `SearchCredentials` umfasst Ihren API-Schlüssel.

Mit dem folgenden Code wird ein neues `SearchServiceClient`-Element mit den Werten für den Namen des Suchdiensts und den API-Schlüssel erstellt, die in der Konfigurationsdatei der Anwendung (`app.config` oder `web.config`) gespeichert sind:

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient` verfügt über eine `Indexes`-Eigenschaft. Diese Eigenschaft stellt alle Methoden bereit, die Sie benötigen, um Azure Search-Indizes zu erstellen, aufzulisten, zu aktualisieren oder zu löschen.

> [!NOTE]
> Die `SearchServiceClient`-Klasse verwaltet die Verbindungen mit Ihrem Suchdienst. Um zu vermeiden, dass zu viele Verbindungen geöffnet werden, sollten Sie nur eine Instanz von `SearchServiceClient` in Ihrer Anwendung freigeben, sofern dies möglich ist. Die zugehörigen Methoden sind threadsicher und ermöglichen diese Freigabe.
> 
> 

<a name="DefineIndex"></a>

## <a name="iii-define-your-azure-search-index"></a>III. Definieren des Azure Search-Index
Der Index kann mit einem einzigen Aufruf der `Indexes.Create` -Methode erstellt werden. Für diese Methode wird als Parameter ein `Index` -Objekt verwendet, das Ihren Azure Search-Index definiert. Sie müssen ein `Index` -Objekt erstellen und wie folgt initialisieren:

1. Legen Sie die `Name`-Eigenschaft des `Index`-Objekts auf den Namen des Index fest.
2. Legen Sie die `Fields`-Eigenschaft des `Index`-Objekts auf ein Array von `Field`-Objekten fest. Am einfachsten können Sie die `Field`-Objekte durch Aufrufen der `FieldBuilder.BuildForType`-Methode erstellen und dabei eine Modellklasse für den Typparameter übergeben. Eine Modellklasse verfügt über Eigenschaften, die den Felder Ihres Index zugeordnet werden. Dadurch können Sie Dokumente aus Ihrem Suchindex an Instanzen Ihrer Modellklasse binden.

> [!NOTE]
> Falls Sie keine Modellklasse verwenden möchten, können Sie `Field`-Objekte auch direkt erstellen, um Ihren Index zu definieren. Sie können den Namen des Felds zusammen mit dem Datentyp (oder dem Analyzer bei Zeichenfolgenfeldern) für den Konstruktor angeben. Sie können auch andere Eigenschaften festlegen, z. B. `IsSearchable`, `IsFilterable` usw.
>
>

Berücksichtigen Sie beim Gestalten des Index die Benutzerfreundlichkeit und die geschäftlichen Anforderungen, da jedem Feld die [richtigen Eigenschaften](https://docs.microsoft.com/rest/api/searchservice/Create-Index) zugewiesen sein müssen. Über diese Eigenschaften wird gesteuert, welche Suchfunktionen (Filtern, Facettierung, Sortieren der Volltextsuche usw.) für welche Felder gelten. Für jede Eigenschaft, die Sie nicht explizit festlegen, wird mit der `Field`-Klasse automatisch die entsprechende Suchfunktion deaktiviert, bis Sie sie ausdrücklich aktivieren.

In unserem Beispiel hat der Index den Namen „hotels“. Die Felder wurden unter Verwendung einer Modellklasse definiert. Jede Eigenschaft der Modellklasse verfügt über Attribute, die das suchbezogene Verhalten des entsprechenden Indexfelds bestimmen. Die Modellklasse ist wie folgt definiert:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
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

    // ToString() method omitted for brevity...
}
```

Die Attribute für die einzelnen Eigenschaften wurden so gewählt, wie sie unseres Erachtens in einer Anwendung verwendet werden. So ist es beispielsweise wahrscheinlich, dass für Personen, die nach Hotels suchen, Übereinstimmungen mit Schlüsselwörtern im Feld `description` relevant sind. Daher aktivieren wir für dieses Feld die Volltextsuche, indem wir der `Description`-Eigenschaft das `IsSearchable`-Attribut hinzufügen.

In Ihrem Index vom Typ `string` muss genau ein Feld als *key*-Feld festgelegt sein. Hierzu muss das `Key`-Attribut hinzugefügt werden (wie unter `HotelId` im obigen Beispiel zu sehen).

Diese Indexdefinition verwendet für das Feld `description_fr` eine Sprachanalyse, da es für Text in französischer Sprache vorgesehen ist. Weitere Informationen zu Sprachanalysen finden Sie im [Thema zur Sprachunterstützung](https://docs.microsoft.com/rest/api/searchservice/Language-support) sowie im entsprechenden [Blogbeitrag](https://azure.microsoft.com/blog/language-support-in-azure-search/).

> [!NOTE]
> Standardmäßig wird der Name jeder Eigenschaft in Ihrer Modellklasse als Name des entsprechenden Felds im Index verwendet. Wenn Sie alle Eigenschaftennamen Feldnamen in Camel-Case-Schreibweise zuordnen möchten, markieren Sie die Klasse mit dem `SerializePropertyNamesAsCamelCase`-Attribut. Wenn Sie als Zuweisungsziel einen anderen Namen verwenden möchten, können Sie das `JsonProperty`-Attribut wie die `DescriptionFr`-Eigenschaft weiter oben verwenden. Das `JsonProperty`-Attribut hat Vorrang vor dem `SerializePropertyNamesAsCamelCase`-Attribut.
> 
> 

Nach dem Definieren einer Modellklasse können wir nun ganz problemlos eine Indexdefinition erstellen:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="iv-create-the-index"></a>IV. Erstellen des Index
Nachdem Sie nun über ein initialisiertes `Index`-Objekt verfügen, können Sie den Index einfach durch Aufrufen von `Indexes.Create` für das `SearchServiceClient`-Objekt aufrufen:

```csharp
serviceClient.Indexes.Create(definition);
```

Bei einer erfolgreich durchgeführten Anforderung wird die Methode normal zurückgegeben. Wenn ein Problem mit der Anforderung auftritt, z. B. ein ungültiger Parameter vorhanden ist, gibt die Methode `CloudException` zurück.

Wenn Sie einen Index nicht mehr benötigen und ihn löschen möchten, rufen Sie einfach die `Indexes.Delete`-Methode für `SearchServiceClient` auf. Der Index „hotels“ wird beispielsweise wie folgt gelöscht:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Der Beispielcode in diesem Artikel verwendet der Einfachheit halber die synchronen Methoden des Azure Search-.NET-SDK. Für Ihre eigenen Anwendungen empfehlen wir aus Gründen der Skalierbarkeit und Reaktionsfähigkeit die asynchronen Methoden. In den Beispielen oben können Sie `Create` und `Delete` beispielsweise auch durch `CreateAsync` und `DeleteAsync` ersetzen.
> 
> 

## <a name="next"></a>Weiter
Nach dem Erstellen eines Azure Search-Indexes können Sie [Ihre Inhalte in den Index hochladen](search-what-is-data-import.md) und mit dem Durchsuchen der Daten beginnen.




<!--HONumber=Dec16_HO2-->


