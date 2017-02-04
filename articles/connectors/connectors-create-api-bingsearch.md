---
title: "Hinzufügen des Bing-Suche-Connectors in Logik-Apps | Microsoft Docs"
description: "Übersicht über den Bing-Suche-Connector mit REST-API-Parametern"
services: 
suite: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: a7f530e8-1573-4612-8899-c9c84aa2de6d
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: dbb920f212d46365233e83ba1e0a0ae99917e405


---
# <a name="get-started-with-the-bing-search-connector"></a>Erste Schritte mit dem Bing-Suche-Connector
Verbinden Sie sich mit der Bing-Suche, um Nachrichten, Videos und mehr zu suchen. Die Bing-Suche ermöglicht Folgendes: 

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus einer Suche abgerufen werden. 
* Verwenden von Aktionen, um Bilder, Nachrichten und mehr zu suchen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z. B. ein Video suchen und dann Twitter verwenden, um das Video in einem Twitter-Feed zu posten.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Die Bing-Suche bietet die folgenden Aktionen. Es gibt keine Trigger. 

| Trigger | Aktionen |
| --- | --- |
| Keine |<ul><li>Web durchsuchen</li><li>Videos suchen</li><li>Bilder suchen</li><li>Nachrichten suchen</li><li>Verwandte Informationen suchen</li><li>Schreibweisen suchen</li><li>Alles suchen</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### <a name="search-web"></a>Web durchsuchen
Ruft Websites aus einer Bing-Suche ab.  
```GET: /Web```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |
| webFileType |string |no |query |Keine |Dateityp zum Eingrenzen der Suche (Beispiel: DOC) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="search-videos"></a>Videos suchen
Ruft Videos aus einer Bing-Suche ab.  
```GET: /Video```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |
| videoFilters |string |no |query |Keine |Filtern Sie die Suche basierend auf Größe, Aspekt, Farbe, Format, Gesicht und einer beliebigen Kombination dieser Angaben.  Gültige Werte:  <ul><li>Duration:Short</li><li>Duration:Medium</li><li>Duration:Long</li><li>Aspect:Standard</li><li>Aspect:Widescreen</li><li>Resolution:Low</li><li>Resolution:Medium</li><li>Resolution:High</li></ul> <br/><br/>Beispiel: „Duration:Short+Resolution:High“ |
| videoSortBy |string |no |query |Keine |Sortierreihenfolge für Ergebnisse. Gültige Werte:  <ul><li>Date</li><li>Relevanz</li></ul> <p>Die Sortierung nach Datum erfolgt absteigend.</p> |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="search-images"></a>Bilder suchen
Ruft Bilder aus einer Bing-Suche ab.  
```GET: /Image```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |
| imageFilters |string |no |query |Keine |Filtern Sie die Suche basierend auf Größe, Aspekt, Farbe, Format, Gesicht und einer beliebigen Kombination dieser Angaben. Gültige Werte:  <ul><li>Size:Small</li><li>Size:Medium</li><li>Size:Large</li><li>Size:Width:[Breite]</li><li>Size:Height:[Höhe]</li><li>Aspect:Square</li><li>Aspect:Wide</li><li>Aspect:Tall</li><li>Color:Color</li><li>Color:Monochrome</li><li>Style:Photo</li><li>Style:Graphics</li><li>Face:Face</li><li>Face:Portrait</li><li>Face:Other</li></ul><br/><br/>Beispiel: „Size:Small+Aspect:Square“ |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="search-news"></a>Nachrichten suchen
Ruft neue Ergebnisse aus einer Bing-Suche ab.  
```GET: /News```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |
| newsSortBy |string |no |query |Keine |Sortierreihenfolge für Ergebnisse. Gültige Werte:  <ul><li>Date</li><li>Relevanz</li></ul> <p>Die Sortierung nach Datum erfolgt absteigend.</p> |
| newsCategory |string |no |query | |Kategorie von Nachrichten zum Eingrenzen der Suche (Beispiel: rt_Business) |
| newsLocationOverride |string |no |query |Keine |Erkennung des Standorts durch Bing außer Kraft setzen. Dieser Parameter gilt nur für den Markt „en-US“. Das Format für die Eingabe ist: US./<state /> (Beispiel: „US.WA“) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="search-spellings"></a>Schreibweisen suchen
Ruft Rechtschreibvorschläge ab.  
```GET: /SpellingSuggestions```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="search-related"></a>Verwandte Informationen suchen
Ruft verwandte Suchergebnisse aus einer Bing-Suche ab.  
```GET: /RelatedSearch```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="search-all"></a>Alles suchen
Ruft alle Websites, Videos, Bilder usw. aus einer Bing-Suche ab.  
```GET: /CompositeSearch```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu suchender Text (Beispiel: Xbox) |
| maxResult |integer |no |query |Keine |Maximale Anzahl der zurückzugebenden Ergebnisse |
| startOffset |integer |no |query |Keine |Anzahl zu überspringender Ergebnisse |
| adultContent |string |no |query |Keine |Filter für nicht jugendfreie Inhalte. Gültige Werte:  <ul><li>Aus</li><li>Moderat</li><li>Strict</li></ul> |
| market |string |no |query |Keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: en-US) |
| longitude |number |no |query |Keine |Längengrad (Ost-West-Koordinate) zum Eingrenzen der Suche (Beispiel: 47,603450) |
| latitude |number |no |query |Keine |Breitengrad (Nord-Süd-Koordinate) zum Eingrenzen der Suche (Beispiel: -122,329696) |
| webFileType |string |no |query |Keine |Dateityp zum Eingrenzen der Suche (Beispiel: DOC) |
| videoFilters |string |no |query |Keine |Filtern Sie die Suche basierend auf Größe, Aspekt, Farbe, Format, Gesicht und einer beliebigen Kombination dieser Angaben.  Gültige Werte:  <ul><li>Duration:Short</li><li>Duration:Medium</li><li>Duration:Long</li><li>Aspect:Standard</li><li>Aspect:Widescreen</li><li>Resolution:Low</li><li>Resolution:Medium</li><li>Resolution:High</li></ul> <br/><br/>Beispiel: „Duration:Short+Resolution:High“ |
| videoSortBy |string |no |query |Keine |Sortierreihenfolge für Ergebnisse. Gültige Werte:  <ul><li>Date</li><li>Relevanz</li></ul> <p>Die Sortierung nach Datum erfolgt absteigend.</p> |
| imageFilters |string |no |query |Keine |Filtern Sie die Suche basierend auf Größe, Aspekt, Farbe, Format, Gesicht und einer beliebigen Kombination dieser Angaben. Gültige Werte:  <ul><li>Size:Small</li><li>Size:Medium</li><li>Size:Large</li><li>Size:Width:[Breite]</li><li>Size:Height:[Höhe]</li><li>Aspect:Square</li><li>Aspect:Wide</li><li>Aspect:Tall</li><li>Color:Color</li><li>Color:Monochrome</li><li>Style:Photo</li><li>Style:Graphics</li><li>Face:Face</li><li>Face:Portrait</li><li>Face:Other</li></ul><br/><br/>Beispiel: „Size:Small+Aspect:Square“ |
| newsSortBy |string |no |query |Keine |Sortierreihenfolge für Ergebnisse. Gültige Werte:  <ul><li>Date</li><li>Relevanz</li></ul> <p>Die Sortierung nach Datum erfolgt absteigend.</p> |
| newsCategory |string |no |query |(Keine) |Kategorie von Nachrichten zum Eingrenzen der Suche (Beispiel: rt_Business) |
| newsLocationOverride |string |no |query |Keine |Erkennung des Standorts durch Bing außer Kraft setzen. Dieser Parameter gilt nur für den Markt „en-US“. Das Format für die Eingabe ist: US./<state /> (Beispiel: „US.WA“) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="webresultmodel-bing-web-search-results"></a>WebResultModel: Bing-Ergebnisse für Websuche
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Titel |string |no |
| Beschreibung |string |no |
| DisplayUrl |string |no |
| ID |string |no |
| FullUrl |string |no |

#### <a name="videoresultmodel-bing-video-search-results"></a>VideoResultModel: Bing-Suchergebnisse für Videos
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Titel |string |no |
| DisplayUrl |string |no |
| ID |string |no |
| MediaUrl |string |no |
| Laufzeit |integer |no |
| Miniaturansicht |nicht definiert |no |

#### <a name="thumbnailmodel-thumbnail-properties-of-the-multimedia-element"></a>ThumbnailModel: Miniaturansichteigenschaften des Multimediaelements
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| MediaUrl |string |no |
| ContentType |string |no |
| Breite |integer |no |
| Höhe |integer |no |
| FileSize |integer |no |

#### <a name="imageresultmodel-bing-image-search-results"></a>ImageResultModel: Bing-Suchergebnisse für Bilder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Titel |string |no |
| DisplayUrl |string |no |
| ID |string |no |
| MediaUrl |string |no |
| SourceUrl |string |no |
| Miniaturansicht |nicht definiert |no |

#### <a name="newsresultmodel-bing-news-search-results"></a>NewsResultModel: Bing-Suchergebnisse für Nachrichten
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Titel |string |no |
| Beschreibung |string |no |
| DisplayUrl |string |no |
| ID |string |no |
| Quelle |string |no |
| Date |string |no |

#### <a name="spellresultmodel-bing-spelling-suggestions-results"></a>SpellResultModel: Bing-Suchergebnisse für Schreibweisen
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| Wert |string |no |

#### <a name="relatedsearchresultmodel-bing-related-search-results"></a>RelatedSearchResultModel: Bing-Suchergebnisse für verwandte Informationen
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Titel |string |no |
| ID |string |no |
| BingUrl |string |no |

#### <a name="compositesearchresultmodel-bing-composite-search-results"></a>CompositeSearchResultModel: Zusammengesetzte Bing-Suchergebnisse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| WebResultsTotal |integer |no |
| ImageResultsTotal |integer |no |
| VideoResultsTotal |integer |no |
| NewsResultsTotal |integer |no |
| SpellSuggestionsTotal |integer |no |
| WebResults |array |no |
| ImageResults |array |no |
| VideoResults |array |no |
| NewsResults |array |no |
| SpellSuggestionResults |array |no |
| RelatedSearchResults |array |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

Gehen Sie zur [Liste der APIs](apis-list.md)zurück.




<!--HONumber=Jan17_HO3-->


