---
title: Analysen in Azure Search | Microsoft-Dokumentation
description: Weisen Sie durchsuchbaren Textfeldern in einem Index Analysen zu, um die Lucene-Standardanalyse durch benutzerdefinierte, vordefinierte oder sprachspezifische Alternativen zu ersetzen.
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/03/2017
ms.author: heidist
ms.translationtype: HT
ms.sourcegitcommit: ce0189706a3493908422df948c4fe5329ea61a32
ms.openlocfilehash: 6e6c4491b8f66011340d1246495dbded7caf2903
ms.contentlocale: de-de
ms.lasthandoff: 09/05/2017

---

# <a name="analyzers-in-azure-search"></a>Analysemodule in Azure Search

Eine *Analyse* ist eine Komponente der [Volltextsuchverarbeitung](search-lucene-query-architecture.md) und zuständig für Text-zu-Token-Konvertierungen für Indizierungs- und Abfrage-Workloads. Bei der Indizierung transformiert eine Analyse Text in tokenisierte Benennungen, die in den Index geschrieben werden. Bei Abfragen führt die Analyse dann die gleichen Transformationen (Text in tokenisierte Benennungen) durch, diesmal allerdings für Lesevorgänge während Abfragen. 

Typische Transformationen während der Analyse:

+ Nicht unbedingt benötigte Wörter (Stoppwörter) und Satzzeichen werden entfernt.
+ Benennungen mit Bindestrichen und Ausdrücke werden in Einzelkomponenten unterteilt.
+ Benennungen werden in Kleinbuchstaben umgewandelt.
+ Wörter werden auf ihre jeweilige Stammform reduziert, sodass unabhängig von der Zeitform eine Übereinstimmung gefunden werden kann.

Azure Search bietet eine Standardanalyse. Diese kann feldspezifisch durch eine Alternative ersetzt werden. In diesem Artikel werden das Spektrum der verfügbaren Optionen sowie bewährte Methoden für das Hinzufügen einer Analyse für Suchvorgänge beschrieben. Außerdem finden Sie hier Analysebeispielkonfigurationen für gängige Szenarien.

## <a name="how-analysis-fits-into-full-text-search-processing"></a>Die Funktion der Analyse bei der Verarbeitung der Volltextsuche

Analysen arbeiten mit von der Abfrageanalyse übergebenen Benennungseingaben und geben analysierte Benennungen zurück, die einem Abfragestrukturobjekt hinzugefügt werden.

 ![Diagramm zur Lucene-Abfragearchitektur in Azure Search][1]

Analysen werden nur für Abfragen verwendet, die auf einer einzelnen Benennung oder auf einem Ausdruck basieren. Für Abfragetypen mit unvollständigen Benennungen (Präfixabfragen, Platzhalterabfragen, Abfragen mit regulären Ausdrücken) oder für Fuzzyabfragen werden keine Analysen verwendet. Bei diesen Abfragetypen werden Benennungen unter Umgehung der Analysephase direkt der Abfragestruktur hinzugefügt. Die einzige Transformation, die für die Abfrageausdrücke dieser Typen durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.

## <a name="supported-analyzers"></a>Unterstützte Analysen

Die folgende Liste gibt Aufschluss darüber, welche Analysen in Azure Search unterstützt werden:

| Kategorie | Beschreibung |
|----------|-------------|
| [Lucene-Standardanalyse](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Standard. Wird automatisch für die Indizierung und für Abfragen verwendet. Muss nicht angegeben oder konfiguriert werden. Diese allgemeine Analyse eignet sich für die meisten Sprachen und Szenarien.|
| Vordefinierte Analysen | Werden als fertiges Produkt angeboten und in der Regel unverändert oder nur mit minimalen Anpassungen verwendet. <br/>Es gibt spezialisierte und sprachspezifische Typen. Sie sind „vordefiniert“, da Sie ihren Namen angeben und sie nicht weiter anpassen. <br/><br/>[Spezialisierte (sprachunabhängige) Analysen](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) für Texteingaben, die eine spezielle oder eine minimale Verarbeitung erfordern. Zu sprachunabhängigen vordefinierten Analysen zählen **Asciifolding**, **Keyword**, **Pattern**, **Simple**, **Stop** und **Whitespace**.<br/><br/>[Sprachanalysen](https://docs.microsoft.com/rest/api/searchservice/language-support) bieten umfassende linguistische Unterstützung für individuelle Sprachen. Azure Search unterstützt 35 Lucene-Sprachanalysen und 50 Microsoft-Analysen für die Verarbeitung natürlicher Sprache. |
|[Benutzerdefinierte Analysen](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Eine benutzerdefinierte Konfiguration einer Kombination vorhandener Elemente, bestehend aus einem Tokenizer (erforderlich) und optionalen Filtern („char“ oder „token“).|

Sie können eine vordefinierte Analyse (beispielsweise **pattern** oder **stop**) anpassen, um alternative, in der [Referenz zu vordefinierten Analysen](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) dokumentierte Optionen zu verwenden. Nur wenige der vordefinierten Analysen verfügen über konfigurierbare Optionen. Versehen Sie Ihre neue Konfiguration wie üblich mit einem Namen (etwa *myPatternAnalyzer*), damit er von der Lucene-Musteranalyse zu unterscheiden ist.

## <a name="how-to-specify-analyzer"></a>Angeben von Analysen

1. Erstellen Sie für benutzerdefinierte Analysen eine Definition vom Typ `analyzer` für den Index. Weitere Informationen finden Sie unter [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) (Erstellen des Index) sowie unter [Create a custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search#create-a-custom-analyzer) (Erstellen einer benutzerdefinierten Analyse).

2. Legen Sie bei jedem Feld, für das Sie die Analyse verwenden möchten, die Eigenschaft `analyzer` auf den Namen der Zielanalyse für eine [Felddefinition im Index](https://docs.microsoft.com/rest/api/searchservice/create-index) fest. Hierbei können Sie eine vordefinierte Analyse, eine Sprachanalyse oder eine benutzerdefinierte Analyse verwenden, die zuvor im Indexschema definiert wurde.

 Anstelle einer einzelnen Eigenschaft vom Typ `analyzer` können Sie mithilfe der Feldparameter `indexAnalyzer` und `searchAnalyzer` auch verschiedene Analysen für Indizierung und Abfragen festlegen. 

3. Erstellen Sie den Index neu, um das neue Textverarbeitungsverhalten aufzurufen.

## <a name="best-practices"></a>Bewährte Methoden

Dieser Abschnitt enthält Tipps zur effizienteren Verwendung von Analysen.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Eine einzelne Analyse für Lese-/Schreibvorgänge, sofern keine besonderen Anforderungen gelten

In Azure Search können Sie verschiedene Analysen für das Indizieren und Suchen nach zusätzlichen Feldparametern vom Typ `indexAnalyzer` und `searchAnalyzer` angeben. Wenn keine Angabe vorhanden ist, wird die Analyse, die mit der `analyzer`-Eigenschaft festgelegt wird, sowohl für die Indizierung als auch für die Suche verwendet. Ohne Angabe von `analyzer` wird die Standardanalyse von Lucene verwendet.

Allgemeine Regel: Verwenden Sie für die Indizierung und für Abfragen die gleiche Analyse, es sei denn, es müssen besondere Anforderungen erfüllt werden. In der Regel ist es effizienter, wenn die Analyse, die das Token erstellt, die gleiche ist, die später bei Abfragen auch für die Tokensuche verwendet wird. 

### <a name="test-during-active-development"></a>Testen während der aktiven Entwicklung

Wenn Sie die Standardanalyse überschreiben, muss der Index neu erstellt werden. Überlegen Sie sich möglichst während der aktiven Entwicklung, welche Analysen Sie verwenden möchten, bevor Sie einen Index in die Produktionsumgebung einbringen.

### <a name="compare-analyzers-side-by-side"></a>Gegenüberstellen von Analysen

Wir empfehlen die Verwendung der [Analyse-API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Die Antwort besteht aus tokenisierten Benennungen, die von einer spezifischen Analyse für den von Ihnen angegebenen Text generiert werden. 

> [!Tip]
> Die [Search Analyzer Demo](http://alice.unearth.ai/) zeigt eine Gegenüberstellung der Lucene-Standardanalyse, der Lucene-Englischanalyse und der Verarbeitung natürlicher Sprache (Englisch) von Microsoft. Die Ergebnisse für Ihre Sucheingaben werden für die einzelnen Analysen nebeneinander angezeigt.

## <a name="examples"></a>Beispiele

Die folgenden Beispiele zeigen Analysedefinitionen für einige gängige Szenarien.

<a name="Example1"></a>
### <a name="example-1-custom-options"></a>Beispiel 1: Benutzerdefinierte Optionen

Dieses Beispiel zeigt eine Analysedefinition mit benutzerdefinierten Optionen. Benutzerdefinierte Optionen für char-Filter, Tokenizer und token-Filter werden separat als benannte Konstrukte angegeben, auf die dann in der Analysedefinition verwiesen wird. Vordefinierte Elemente werden unverändert verwendet und einfach über ihren Namen angegeben.

Das Beispiel im Detail:

* Analysen sind eine Eigenschaft der Feldklasse für ein durchsuchbares Feld.
* Eine benutzerdefinierte Analyse ist Teil einer Indexdefinition. Sie kann geringfügig (etwa durch Anpassung einer einzelnen Option in einem einzelnen Filter) oder an mehreren Stellen angepasst werden.
* In diesem Fall ist „my_analyzer“ die benutzerdefinierte Analyse, die wiederum den angepassten Standard-Tokenizer „my_standard_tokenizer“ und zwei token-Filter („lowercase“ und den angepassten asciifolding-Filter „my_asciifolding“) verwendet.
* Darüber hinaus wird der benutzerdefinierte char-Filter „map_dash“ definiert, um vor der Tokenisierung sämtliche Bindestriche durch Unterstriche zu ersetzen. (Der Standard-Tokenizer unterbricht bei einem Bindestrich, aber nicht bei einem Unterstrich.)

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizer",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Example2"></a>
### <a name="example-2-override-the-default-analyzer"></a>Beispiel 2: Überschreiben der Standardanalyse

Die Standardanalyse ist die Standardeinstellung. Angenommen, Sie möchten die Standardeinstellung durch eine andere vordefinierte Analyse (beispielsweise „pattern“) ersetzen. Wenn Sie keine benutzerdefinierten Optionen festlegen, müssen Sie nur den Namen in der Felddefinition angeben.

Das Element „analyzer“ überschreibt die Standardanalyse für das jeweilige Feld. Es findet keine globale Überschreibung statt. In diesem Beispiel verwendet `text1` die Analyse „pattern“ und `text2` die Standardanalyse, da hierfür keine Analyse angegeben ist.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Example3"></a>
### <a name="example-3-different-analyzers-for-indexing-and-search-operations"></a>Beispiel 3: Unterschiedliche Analysen für Indizierung und Suchvorgänge

Die Vorschau-APIs enthalten zusätzliche Indexattribute, die es ermöglichen, unterschiedliche Analysen für Indizierung und Suchvorgänge anzugeben. Die Attribute `searchAnalyzer` und `indexAnalyzer` müssen als Paar angegeben werden und ersetzen das Einzelattribut `analyzer`.


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Example4"></a>
### <a name="example-4-language-analyzer"></a>Beispiel 4: Sprachanalyse

Für Felder mit Zeichenfolgen in unterschiedlichen Sprachen kann eine Sprachanalyse verwendet werden, während für andere Felder weiterhin die Standardanalyse (oder eine andere vordefinierte oder benutzerdefinierte Analyse) verwendet wird. Bei Verwendung einer Sprachanalyse muss sie sowohl für die Indizierung als auch für Suchvorgänge verwendet werden. Bei Feldern mit Sprachanalyse können für Indizierung und Suche keine unterschiedlichen Analysen verwendet werden.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "IndexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="next-steps"></a>Nächste Schritte

+ Lesen Sie unsere umfassende Erläuterung der [Funktionsweise der Volltextsuche in Azure Search](search-lucene-query-architecture.md). In diesem Artikel werden anhand von Beispielen Verhaltensweisen erklärt, die auf den ersten Blick vielleicht nicht intuitiv erscheinen.

+ Probieren Sie andere Abfragesyntax in den Beispielen unter [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) (Suchen nach Dokumenten) oder unter [Simple query syntax in Azure Search](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) (Einfache Abfragesyntax in Azure Search) im Suchexplorer im Portal aus.

+ Informieren Sie sich darüber, wie Sie [sprachspezifische lexikalische Analysen](https://docs.microsoft.com/rest/api/searchservice/language-support) anwenden.

+ [Konfigurieren Sie benutzerdefinierte Analysen](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) für eine minimale oder besondere Verarbeitung einzelner Felder.

+ Vergleichen Sie auf [dieser Demowebsite](http://alice.unearth.ai/) die Standard- und die Englischanalyse in einer Gegenüberstellung. 

## <a name="see-also"></a>Weitere Informationen

 [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Suchen nach Dokumenten (Azure Search Service-REST-API)) 

 [Einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

 [Vollständige Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 
 [Verarbeiten von Suchergebnissen](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png

