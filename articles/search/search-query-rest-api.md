---
title: Abfragen eines Azure Search-Indexes mit der REST-API | Microsoft Docs
description: Erstellen Sie eine Suchabfrage in Azure Search, und verwenden Sie Suchparameter zum Filtern und Sortieren von Suchergebnissen.
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 455c4847893175c1091ae21fa22215fd1dd10c53
ms.openlocfilehash: 96e8177f57977f88c5a4a1ec0b9243b5b348f078

---

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Abfragen des Azure Search-Indexes mit der REST-API
> [!div class="op_single_selector"]
>
> * [Übersicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Dieser Artikel beschreibt, wie Sie einen Index mithilfe der [Azure Search-REST-API](https://docs.microsoft.com/rest/api/searchservice/)abfragen.

[Erstellen Sie einen Azure Search-Index](search-what-is-an-index.md) und [füllen Sie ihn mit Daten](search-what-is-data-import.md), bevor Sie mit dieser exemplarischen Vorgehensweise beginnen.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>I. Identifizieren des Abfrage-API-Schlüssel Ihres Azure Search-Diensts
Eine wichtige Komponente jedes Suchvorgangs für die Azure Search-REST-API ist der *API-Schlüssel* , der für den bereitgestellten Dienst generiert wurde. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, um die API-Schlüssel für Ihren Dienst zu ermitteln.
2. Wechseln Sie zum Blatt Ihres Azure Search-Diensts.
3. Klicken Sie auf das Schlüsselsymbol.

Der Dienst enthält *Admin-Schlüssel* und *Abfrageschlüssel*.

* Die primären und sekundären *Admin-Schlüssel* gewähren Ihnen Vollzugriff auf alle Vorgänge. Dazu zählen die Dienstverwaltung und das Erstellen und Löschen von Indizes, Indexern und Datenquellen. Ihnen stehen zwei Schlüssel zur Verfügung, damit Sie den sekundären Schlüssel weiterhin nutzen können, wenn Sie den primären Schlüssel neu generieren möchten, und umgekehrt.
* Die *Abfrageschlüssel* gewähren Ihnen Lesezugriff auf Indizes und Dokumente. Diese werden in der Regel auf Clientanwendungen verteilt, die Suchanfragen ausgeben.

Zum Abfragen eines Indexes können Sie einen der Abfrageschlüssel verwenden. Admin-Schlüssel können auch für Abfragen verwendet werden, Sie sollten jedoch einen Abfrageschlüssel in Ihrem Anwendungscode verwenden, da dies dem [Prinzip der geringsten Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege)besser entspricht.

## <a name="ii-formulate-your-query"></a>II. Formulieren der Abfrage
Es gibt zwei Möglichkeiten, um [den Index mithilfe der REST-API zu durchsuchen](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Eine Möglichkeit besteht darin, eine HTTP POST-Anforderung auszugeben, wobei die Abfrageparameter in ein JSON-Objekt im Anforderungstext definiert werden. Die andere Möglichkeit besteht darin, eine HTTP GET-Anforderung auszugeben, wobei die Abfrageparameter in der Anforderungs-URL definiert werden. Die Beschränkungen in Bezug auf die Größe der Abfrageparameter sind bei POST [geringer](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) als bei GET. Aus diesem Grund empfehlen wir die Verwendung von POST, sofern GET nicht aufgrund bestimmter Umstände praktischer wäre.

Sowohl für POST als auch für GET müssen Sie in der Anforderungs-URL Ihren *Dienstnamen*, den *Indexnamen* sowie die entsprechende *API-Version* (die aktuelle Version der API zum Zeitpunkt der Veröffentlichung dieses Dokuments ist `2016-09-01`) bereitstellen. Für GET befindet sich die *Abfragezeichenfolge* am Ende der URL dort, wo Sie die Abfrageparameter angeben. Das URL-Format finden Sie weiter unten:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

Das Format für POST ist das Gleiche, allerdings mit der API-Version in den Parametern für die Abfragezeichenfolge.

#### <a name="example-queries"></a>Beispielabfragen
Hier finden Sie einige Beispielabfragen für einen Index mit dem Namen „hotels“. Diese Abfragen werden sowohl im GET- als auch im POST-Format dargestellt.

Durchsuchen des gesamten Index nach dem Wort „budget“, zurückgegeben wird nur das Feld `hotelName`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

Anwenden eines Filters auf den Index für die Suche nach Hotels, die weniger als&150; US-Dollar pro Nacht kosten, zurückgegeben werden `hotelId` und `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Durchsuchen des gesamten Index, Sortieren nach einem bestimmten Feld (`lastRenovationDate`) in absteigender Reihenfolge, Auswählen der oberen beiden Ergebnisse, nur Anzeigen von `hotelName` und `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Senden der HTTP-Anforderung
Da Sie nun Ihre Abfrage als Teil Ihrer HTTP-Anforderungs-URL (für GET) oder des Texts (für POST) formuliert haben, können Sie Ihre Anforderungsheader definieren und die Anforderung absenden.

#### <a name="request-and-request-headers"></a>Anforderung und Anforderungsheader
Sie müssen zwei Anforderungsheader für GET bzw. drei für POST definieren:

1. Der Header `api-key` muss auf den Abfrageschlüssel aus Schritt I oben festgelegt werden. Sie können auch den Admin-Schlüssel als Header `api-key` festlegen. Wir empfehlen allerdings, dass Sie einen Abfrageschlüssel verwenden, da dieser ausschließlich Lesezugriff auf Indizes und Dokumente gewährt.
2. Der Header `Accept` muss auf `application/json` festgelegt sein.
3. Nur im Fall von POST muss der Header `Content-Type` auch auf `application/json` festgelegt sein.

Im Folgenden sehen Sie eine HTTP GET-Abfrage zum Durchsuchen des Indexes „hotels“ mithilfe der Azure Search-REST-API mit einer einfachen Abfrage nach dem Begriff „motels“:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

Dies ist die gleiche Beispielabfrage, allerdings unter Verwendung von HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Bei erfolgreicher Abfrageanforderung wird der Statuscode `200 OK` ausgegeben, und die Suchergebnisse werden im Antworttext im JSON-Format ausgegeben. So sehen die Ergebnisse für die o. g. Abfrage aus. Dabei wird davon ausgegangen, dass der Index „hotels“ mit den Beispieldaten aus [Importieren von Daten in Azure Search über die REST-API](search-import-data-rest-api.md) gefüllt wurde (JSON wurde aus Gründen der Übersichtlichkeit formatiert).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Weitere Informationen finden Sie im Abschnitt „Antwort“ unter [Dokumente durchsuchen](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Weitere Informationen zu anderen HTTP-Statuscodes, die bei Fehlern ausgegeben werden, finden Sie unter [HTTP-Statuscodes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).



<!--HONumber=Dec16_HO2-->


