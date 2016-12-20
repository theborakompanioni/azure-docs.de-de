---
title: Feldzuordnungen in Azure Search-Indexern
description: Konfigurieren von Feldzuordnungen im Azure Search-Indexer zum Ausgleichen von Unterschieden in Feldnamen und Datendarstellungen
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 0325a4de-0190-4dd5-a64d-4e56601d973b
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: fc2f30569acc49dd383ba230271989eca8a14423
ms.openlocfilehash: 57e91f070d9a42882a56e708f12b1ce238ed9191

---

# <a name="field-mappings-in-azure-search-indexers"></a>Feldzuordnungen in Azure Search-Indexern
Bei Verwendung des Azure Search-Indexers gibt es gelegentlich Situationen, in denen die Eingabedaten nicht ganz dem Schema des Zielindexes entsprechen. In diesen Fällen können Sie Ihre Daten mithilfe von **Feldzuordnungen** in das gewünschte Format umwandeln.

Feldzuordnungen sind beispielsweise in folgenden Situationen hilfreich:

* Ihre Datenquelle verfügt über ein Feld `_id`, aber Azure Search lässt keine Feldnamen zu, die mit einem Unterstrich beginnen. Mit einer Feldzuordnung können Sie das Feld „umbenennen“.
* Sie möchten mehrere Felder im Index mit denselben Datenquellendaten auffüllen, da Sie z.B. verschiedene Analysen auf diese Felder anwenden möchten. Mit Feldzuordnungen können Sie ein Datenquellenfeld „verzweigen“.
* Sie müssen Ihre Daten mit Base64 codieren oder decodieren. Feldzuordnungen unterstützen mehrere **Zuordnungsfunktionen**, einschließlich Funktionen für die Base64-Codierung und -Decodierung.   

## <a name="setting-up-field-mappings"></a>Einrichten von Feldzuordnungen
Sie können Feldzuordnungen beim Erstellen eines neuen Indexers über die API [Indexer erstellen](https://msdn.microsoft.com/library/azure/dn946899.aspx) hinzufügen. Sie können Feldzuordnungen für einen Indexer mithilfe der API [Indexer aktualisieren](https://msdn.microsoft.com/library/azure/dn946892.aspx) verwalten.

Eine Feldzuordnung besteht aus drei Teilen:

1. Einem `sourceFieldName`, der ein Feld in der Datenquelle darstellt. Diese Eigenschaft ist obligatorisch.
2. Einem optionalen `targetFieldName`, der ein Feld im Suchindex darstellt. Wenn dieser nicht angegeben wird, wird derselbe Name wie in der Datenquelle verwendet.
3. Einer optionalen `mappingFunction`, die die Daten mit einer von mehreren vordefinierten Funktionen transformieren kann. Die vollständige Liste der Funktionen finden Sie [unten](#mappingFunctions).

Feldzuordnungen werden dem `fieldMappings` -Array in der Indexerdefinition hinzugefügt.

Beispielsweise können Sie Unterschiede in Feldnamen folgendermaßen ausgleichen:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

Ein Indexer kann mehrere Feldzuordnungen aufweisen. Hier sehen Sie beispielsweise, wie Sie ein Feld „verzweigen“ können:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" },
]
```

> [!NOTE]
> Azure Search verwendet einen von Groß- und Kleinschreibung unabhängigen Vergleich zum Auflösen von Feld- und Funktionsnamen in Feldzuordnungen. Dies ist zwar praktisch, weil Sie die Groß- und Kleinschreibung nicht berücksichtigen müssen, bedeutet aber, dass die Datenquelle oder der Index keine Felder aufweisen dürfen, die sich nur in der Groß- und Kleinschreibung unterscheiden.  
>
>

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Feldzuordnungsfunktionen
Diese Funktionen werden derzeit unterstützt:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode"></a>base64Encode
Führt eine *URL-sichere* Base64-Codierung der Eingabezeichenfolge durch. Geht davon aus, dass die Eingabe mit UTF-8 codiert ist.

#### <a name="sample-use-case"></a>Beispiel für einen Anwendungsfall
Nur URL-sichere Zeichen können in einem Azure Search-Dokumentschlüssel enthalten sein (da Kunden das Dokument z.B. über die Lookup-API aufrufen können müssen). Wenn Ihre Daten URL-unsichere Zeichen enthalten und Sie damit ein Schlüsselfeld in Ihrem Suchindex auffüllen möchten, verwenden Sie diese Funktion.   

#### <a name="example"></a>Beispiel
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Path",
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

<a name="base64DecodeFunction"></a>

### <a name="base64decode"></a>base64Decode
Führt die Base64-Decodierung der Eingabezeichenfolge durch. Es wird davon ausgegangen, dass es sich bei der Eingabe um eine *URL-sichere* Base64-codierte Zeichenfolge handelt.

#### <a name="sample-use-case"></a>Beispiel für einen Anwendungsfall
Benutzerdefinierte Blob-Metadatenwerte müssen ASCII-codiert sein. Sie können die Base64-Codierung verwenden, um beliebige Unicode-Zeichenfolgen in benutzerdefinierten Blobmetadaten darzustellen. Um die Suche aussagekräftig zu gestalten, können Sie mit dieser Funktion die codierten Daten beim Auffüllen des Suchindexes wieder in „normale“ Zeichenfolgen umwandeln.  

#### <a name="example"></a>Beispiel
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" }
  }]
```

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition"></a>extractTokenAtPosition
Teilt ein Zeichenfolgenfeld mithilfe des angegebenen Trennzeichens und wählt das Token an der angegebenen Position in der resultierenden Teilung aus.

Wenn die Eingabe beispielsweise `Jane Doe` lautet, `" "` (Leerzeichen) als Trennzeichen (`delimiter`) dient und die `position` 0 ist, ist das Ergebnis `Jane`. Ist die `position` 1, ist das Ergebnis `Doe`. Wenn die Position auf ein Token verweist, das nicht vorhanden ist, wird ein Fehler zurückgegeben.

#### <a name="sample-use-case"></a>Beispiel für einen Anwendungsfall
Die Datenquelle enthält ein Feld `PersonName`, und Sie möchten es als zwei separate Felder `FirstName` und `LastName` indizieren. Mit dieser Funktion können Sie die Eingabe mit dem Leerzeichen als Trennzeichen unterteilen.

#### <a name="parameters"></a>Parameter
* `delimiter`: eine Zeichenfolge, die beim Teilen der Eingabezeichenfolge als Trennzeichen verwendet werden soll.
* `position`: eine ganzzahlige nullbasierte Position des Tokens, das nach der Teilung der Eingabezeichenfolge ausgewählt werden soll.    

#### <a name="example"></a>Beispiel
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection
Wandelt eine Zeichenfolge, die als JSON-Zeichenfolgenarray formatiert ist, in ein Zeichenfolgenarray um, das zum Auffüllen eines Felds `Collection(Edm.String)` im Index verwendet werden kann.

Wenn die Eingabezeichenfolge beispielsweise `["red", "white", "blue"]` lautet, wird das Zielfeld vom Typ `Collection(Edm.String)` mit drei Werten gefüllt: `red`, `white` und `blue`. Für Eingabewerte, die nicht als JSON-Zeichenfolgenarrays analysiert werden können, wird ein Fehler zurückgegeben.

#### <a name="sample-use-case"></a>Beispiel für einen Anwendungsfall
Die Azure SQL-Datenbank verfügt über keinen integrierten Datentyp, der `Collection(Edm.String)` -Feldern in Azure Search natürlich zugeordnet werden kann. Um Zeichenfolgen-Sammlungsfelder aufzufüllen, formatieren Sie Ihre Quelldaten als JSON-Zeichenfolgenarray, und verwenden Sie diese Funktion.

#### <a name="example"></a>Beispiel
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns bei der Verbesserung von Azure Search
Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/)mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.



<!--HONumber=Nov16_HO3-->


