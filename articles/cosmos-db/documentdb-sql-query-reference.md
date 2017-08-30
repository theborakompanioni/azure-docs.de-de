---
title: 'Azure Cosmos DB: DocumentDB-API: SQL-Syntax | Microsoft-Dokumentation'
description: "Referenzdokumentation für die DocumentDB-API-SQL-Abfragesprache von Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/13/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 63b2d20c74df4fd6173994ee1a727594ba8afba3
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a>Azure Cosmos DB: DocumentDB-API: SQL-Syntaxreferenz

Die DocumentDB-API von Azure Cosmos DB unterstützt Abfragen von Dokumenten mithilfe einer vertrauten, SQL-ähnlichen (Structured Query Language, strukturierte Abfragesprache) Grammatik für hierarchische JSON-Dokumente, ohne dass ein explizites Schema oder die Erstellung sekundärer Indizes erforderlich ist. Dieses Thema ist die Referenzdokumentation für die DocumentDB-API-SQL-Abfragesprache.

Eine exemplarische Vorgehensweise für die DocumentDB-API-SQL-Abfragesprache finden Sie unter [SQL queries for Azure Cosmos DB DocumentDB API](documentdb-sql-query.md) (SQL-Abfragen für die DocumentDB-API von Azure Cosmos DB).  
  
Wir laden Sie auch zum Besuch des [Query Playground](http://www.documentdb.com/sql/demo) (Abfragetestplatz) ein, wo Sie Azure Cosmos DB ausprobieren und SQL-Abfragen an unserem Dataset durchführen können.  
  
## <a name="select-query"></a>SELECT-Abfrage  
Ruft JSON-Dokumente aus der Datenbank ab. Unterstützt die Auswertung von Ausdrücken, Projektionen, Filtern und Verknüpfungen.  Die Konventionen zum Beschreiben von SELECT-Anweisungen sind im Abschnitt zu Syntaxkonventionen tabellarisch dargestellt.  
  
**Syntax**  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Hinweise:**  
  
 Weitere Informationen zu den einzelnen Klauseln finden Sie in den folgenden Abschnitten:  
  
-   [SELECT-Klausel](#bk_select_query)  
  
-   [FROM-Klausel](#bk_from_clause)  
  
-   [WHERE-Klausel](#bk_where_clause)  
  
-   [ORDER BY-Klausel](#bk_orderby_clause)  
  
Die Klauseln in der SELECT-Anweisung müssen wie oben gezeigt sortiert werden. Jede der optionalen Klauseln kann ausgelassen werden. Aber wenn optionale Klauseln verwendet werden, müssen sie in der richtigen Reihenfolge stehen.  
  
**Logische Verarbeitungsreihenfolge der SELECT-Anweisung**  
  
Die Klauseln werden in folgender Reihenfolge verarbeitet:  

1.  [FROM-Klausel](#bk_from_clause)  
2.  [WHERE-Klausel](#bk_where_clause)  
3.  [ORDER BY-Klausel](#bk_orderby_clause)  
4.  [SELECT-Klausel](#bk_select_query)  

Beachten Sie, dass sich dies von der Reihenfolge unterscheidet, in der sie in der Syntax auftreten. Die Reihenfolge ist, dass alle von einer verarbeiteten Klausel eingeführten neuen Symbole sichtbar sind und in Klauseln verwendet werden können, die zu einem späteren Zeitpunkt verarbeitet werden. So können z.B. WHERE- und SELECT-Klauseln auf Aliase zugreifen, die in einer FROM-Klausel deklariert werden.  

**Leerzeichen und Kommentare**  

Alle Leerzeichen, die nicht Teil einer in Anführungszeichen gesetzten Zeichenfolge oder eines in Anführungszeichen gesetzten Bezeichners sind, sind nicht Bestandteil der Sprachgrammatik und werden während der Analyse ignoriert.  

Die Abfragesprache unterstützt Kommentare im T-SQL-Stil wie  

-   SQL-Anweisung `-- comment text [newline]`  

Obwohl Leerzeichen und Kommentare in der Grammatik keine Bedeutung haben, müssen sie zum Trennen von Tokens verwendet werden. Beispiel: `-1e5` ist ein einzelnes Zahlentoken, während `: – 1 e5` aus dem Minuszeichentoken gefolgt von der Zahl 1 und dem Bezeichner „e5“ zusammengesetzt ist.  

##  <a name="bk_select_query"></a> SELECT-Klausel  
Die Klauseln in der SELECT-Anweisung müssen wie oben gezeigt sortiert werden. Jede der optionalen Klauseln kann ausgelassen werden. Aber wenn optionale Klauseln verwendet werden, müssen sie in der richtigen Reihenfolge stehen.  

**Syntax**  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argumente**  
  
 `<select_specification>`  
  
 Eigenschaften oder Wert, die für das Resultset ausgewählt werden.  
  
 `'*'`  
  
Gibt an, dass der Wert abgerufen werden sollte, ohne Änderungen vorzunehmen. Insbesondere, wenn der verarbeitete Wert ein Objekt ist, werden alle Eigenschaften abgerufen.  
  
 `<object_property_list>`  
  
Gibt die Liste der abzurufenden Eigenschaften an. Jeder zurückgegebene Wert wird ein Objekt mit den angegebenen Eigenschaften sein.  
  
`VALUE`  
  
Gibt an, dass der JSON-Wert anstelle des vollständigen JSON-Objekts abgerufen werden sollte. Hiermit wird im Gegensatz zu `<property_list>` der projizierte Wert nicht in ein Objekt gehüllt.  
  
`<scalar_expression>`  
  
Ausdruck, der den zu berechnenden Wert darstellt. Weitere Informationen finden Sie im Abschnitt [Skalarausdrücke](#bk_scalar_expressions).  
  
**Hinweise:**  
  
Die `SELECT *`-Syntax ist nur gültig, wenn die FROM-Klausel genau einen Alias deklariert hat. `SELECT *` bietet eine Identitätsprojektion, die hilfreich sein kann, wenn keine Projektion erforderlich ist. SELECT * ist nur gültig, wenn die FROM-Klausel angegeben und nur eine einzelne Eingabequelle eingeführt ist.  
  
Beachten Sie, dass `SELECT <select_list>` und `SELECT *` syntaktische Erleichterungen sind und auch, wie unten gezeigt, mithilfe einfacher SELECT-Anweisungen ausgedrückt werden können.  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     entspricht:  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     entspricht:  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Weitere Informationen**  
  
[Skalarausdrücke](#bk_scalar_expressions)  
[SELECT-Klausel](#bk_select_query)  
  
##  <a name="bk_from_clause"></a> FROM-Klausel  
Gibt die Quelle oder verknüpfte Quellen an. Die FROM-Klausel ist optional. Wenn nicht angegeben, werden andere Klauseln weiterhin so ausgeführt, als ob die FROM-Klausel ein einzelnes Dokument bereitstellen würde.  
  
**Syntax**  
  
```  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <collection_expression> [[AS] input_alias]  
        | input_alias IN <collection_expression>  
  
<collection_expression> ::=   
        ROOT   
     | collection_name  
     | input_alias  
     | <collection_expression> '.' property_name  
     | <collection_expression> '[' "property_name" | array_index ']'  
```  
  
**Argumente**  
  
`<from_source>`  
  
Gibt eine Datenquelle mit oder ohne Alias an. Wenn der Alias nicht angegeben wird, wird er mithilfe der folgenden Regeln aus dem `<collection_expression>` abgeleitet:  
  
-   Wenn der Ausdruck ein „collection_name“ ist, wird „collection_name“ als Alias verwendet.  
  
-   Wenn der Ausdruck `<collection_expression>` ist, wird „property_name“ als Alias verwendet. Wenn der Ausdruck ein „collection_name“ ist, wird „collection_name“ als Alias verwendet.  
  
AS `input_alias`  
  
Gibt an, dass der `input_alias` ein Satz von Werten ist, die von dem zugrunde liegenden Sammlungsausdruck zurückgegeben werden.  
 
`input_alias` IN  
  
Gibt an, dass der `input_alias` den Satz von durch Iteration über alle Arrayelemente jedes Arrays gewonnenen Werten darstellen sollte, die vom zugrunde liegenden Sammlungsausdruck zurückgegeben werden. Jeder vom zugrunde liegenden Sammlungsausdruck zurückgegebene Wert, der kein Array ist, wird ignoriert.  
  
`<collection_expression>`  
  
Gibt den Sammlungsausdruck an, der zum Abrufen der Dokumente verwendet wird.  
  
`ROOT`  
  
Gibt an, dass das Dokument von der standardmäßigen, derzeit verbundenen Sammlung abgerufen werden sollte.  
  
`collection_name`  
  
Gibt an, dass das Dokument von der bereitgestellten Sammlung abgerufen werden sollte. Der Name der Sammlung muss mit dem Namen der Sammlung übereinstimmen, zu der derzeit eine Verbindung besteht.  
  
`input_alias`  
  
Gibt an, dass das Dokument von der anderen, durch den bereitgestellten Alias definierten Quelle abgerufen werden sollte.  
  
`<collection_expression> '.' property_`  
  
Gibt an, dass das Dokument durch den Zugriff auf die Eigenschaft `property_name` oder das Arrayelement „array_index“ für alle Dokumente, die durch den angegebenen Sammlungsausdruck abgerufen werden, abgerufen werden sollte.  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
Gibt an, dass das Dokument durch den Zugriff auf die Eigenschaft `property_name` oder das Arrayelement „array_index“ für alle Dokumente, die durch den angegebenen Sammlungsausdruck abgerufen werden, abgerufen werden sollte.  
  
**Hinweise:**  
  
Alle in den `<from_source>(`s) bereitgestellten oder abgeleiteten Aliase müssen eindeutig sein. Die Syntax `<collection_expression>.`property_name ist identisch mit `<collection_expression>' ['"property_name"']'`. Die letztgenannte Syntax kann jedoch verwendet werden, wenn ein Eigenschaftenname Nicht-ID-Zeichen enthält.  
  
**Fehlende Eigenschaften, fehlende Arrayelemente, Verarbeitung undefinierter Werte**  
  
Wenn ein Sammlungsausdruck auf Eigenschaften oder Arrayelemente zugreift, und dieser Wert nicht vorhanden ist, wird dieser Wert ignoriert und nicht weiter verarbeitet.  
  
**Kontextbereich des Sammlungsausdrucks**  
  
Ein Sammlungsausdruck kann sammlungsbezogen oder dokumentbezogen sein:  
  
-   Ein Ausdruck ist sammlungsbezogen, wenn die zugrunde liegende Quelle des Sammlungsausdrucks entweder ROOT oder `collection_name` ist. Ein solcher Ausdruck stellt einen Satz von Dokumenten dar, die direkt aus der Sammlung abgerufen werden, und ist nicht von der Verarbeitung anderer Sammlungsausdrücke abhängig.  
  
-   Ein Ausdruck ist dokumentbezogen, wenn die zugrunde liegende Quelle des Sammlungsausdrucks ein früher in der Abfrage eingeführter `input_alias` ist. Ein solcher Ausdruck stellt eine Reihe von Dokumenten dar, die durch das Auswerten des Sammlungsausdrucks im Bereich der einzelnen Dokumente gewonnen werden, die zu dem Satz gehören, der der Aliassammlung zugeordnet ist.  Das Resultset ist eine Vereinigung von Sätzen, die durch die Auswertung des Sammlungsausdrucks für jedes Dokument im zugrunde liegenden Satz gewonnen werden.  
  
**Verknüpfungen**  
  
In der aktuellen Version unterstützt Azure Cosmos DB innere Verknüpfungen. Weitere Verknüpfungsfunktionen sind in Planung.

Aus inneren Verknüpfungen resultiert ein komplettes Kreuzungsprodukt der an der Verknüpfung beteiligten Sätze. Das Ergebnis einer N-Way-Verknüpfung ist ein Satz von N-Element-Tupeln, wo jeder Wert im Tupel dem an der Verknüpfung beteiligten Aliassatz zugeordnet ist, und der Zugriff auf den Wert durch Verweis auf diesen Alias in anderen Klauseln erfolgen kann.  
  
Die Auswertung der Verknüpfung hängt vom Kontextbereich der beteiligten Sätze ab:  
  
-  Eine Verknüpfung zwischen Sammlungssatz A und sammlungsbezogenem Satz B führt zu einem Kreuzprodukt aller Elemente in den Sätzen A und B.
  
-   Eine Verknüpfung zwischen Satz A und dokumentbezogenem Satz B führt zu einer Vereinigung aller durch die Auswertung des dokumentbezogenen Satzes B für jedes Dokument aus Satz A gewonnenen Sätze.  
  
 In der aktuellen Version wird höchstens ein sammlungsbezogener Ausdruck vom Abfrageprozessor unterstützt.  
  
**Beispiele für Verknüpfungen:**  
  
Betrachten Sie die folgende FROM-Klausel: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Jede Quelle definiert `input_alias1, input_alias2, …, input_aliasN`. Diese FROM-Klausel gibt einen Satz von N-Tupeln (Tupel mit N-Werten) zurück. Jedes Tupel enthält Werte, die durch Iteration aller Sammlungsaliase über deren jeweilige Sätze entstanden sind.  
  
*JOIN-Beispiel 1 mit 2 Quellen:*  
  
- `<from_source1>` ist sammlungsbezogen und stellt den Satz {A, B, C} dar.  
  
- `<from_source2>` ist dokumentbezogen, verweist auf „input_alias1“ und stellt folgende Sätze dar:  
  
    {1, 2} für `input_alias1 = A,`  
  
    {3} für `input_alias1 = B,`  
  
    {4, 5} für `input_alias1 = C,`  
  
- Die FROM-Klausel `<from_source1> JOIN <from_source2>` führt zu folgenden Tupeln:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
*JOIN-Beispiel 2 mit 3 Quellen:*  
  
- `<from_source1>` ist sammlungsbezogen und stellt den Satz {A, B, C} dar.  
  
- `<from_source2>` ist dokumentbezogen, verweist auf `input_alias1` und stellt folgende Sätze dar:  
  
    {1, 2} für `input_alias1 = A,`  
  
    {3} für `input_alias1 = B,`  
  
    {4, 5} für `input_alias1 = C,`  
  
- `<from_source3>` ist dokumentbezogen, verweist auf `input_alias2` und stellt folgende Sätze dar:  
  
    {100, 200} für `input_alias2 = 1,`  
  
    {300} für `input_alias2 = 3,`  
  
- Die FROM-Klausel `<from_source1> JOIN <from_source2> JOIN <from_source3>` führt zu folgenden Tupeln:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
> [!NOTE]
> Fehlende Tupel für andere Werte von `input_alias1`, `input_alias2`, für die `<from_source3>` keine Werte zurückgegeben hat.  
  
*JOIN-Beispiel 3 mit 3 Quellen:*  
  
- <from_source1> ist sammlungsbezogen und stellt den Satz {A, B, C} dar.  
  
- `<from_source1>` ist sammlungsbezogen und stellt den Satz {A, B, C} dar.  
  
- <from_source2> ist dokumentbezogen, verweist auf „input_alias1“ und stellt folgende Sätze dar:  
  
    {1, 2} für `input_alias1 = A,`  
  
    {3} für `input_alias1 = B,`  
  
    {4, 5} für `input_alias1 = C,`  
  
- `<from_source3>` ist auf `input_alias1` bezogen und stellt folgende Sätze dar:  
  
    {100, 200} für `input_alias2 = A,`  
  
    {300} für `input_alias2 = C,`  
  
- Die FROM-Klausel `<from_source1> JOIN <from_source2> JOIN <from_source3>` führt zu folgenden Tupeln:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300), (C, 5, 300)  
  
> [!NOTE]
> Dies führt zu einem Kreuzprodukt zwischen `<from_source2>` und `<from_source3>`, da beide auf die gleiche `<from_source1>` bezogen sind.  Dies führt zu 4 (2 x 2) Tupeln mit dem Wert A, 0 Tupeln mit dem Wert B (1 x 0) und 2 (2 x 1) Tupeln mit dem Wert C.  
  
**Weitere Informationen**  
  
 [SELECT-Klausel](#bk_select_query)  
  
##  <a name="bk_where_clause"></a> WHERE-Klausel  
 Gibt die Suchbedingung für die Dokumente an, die von der Abfrage zurückgegeben werden.  
  
 **Syntax**  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argumente**  
  
-   `<filter_condition>`  
  
     Gibt die Bedingung an, die erfüllt sein muss, damit die Dokumente zurückgegeben werden.  
  
-   `<scalar_expression>`  
  
     Ausdruck, der den zu berechnenden Wert darstellt. Weitere Informationen finden Sie im Abschnitt [Skalarausdrücke](#bk_scalar_expressions).  
  
 **Hinweise:**  
  
 Damit das Dokument zurückgegeben wird, muss ein als Filterbedingung angegebener Ausdruck mit „true“ ausgewertet werden. Nur der boolesche Wert „true“ erfüllt die Bedingung, jeder andere Wert – undefiniert, NULL, false, Number, Array oder Object – erfüllt die Bedingung nicht.  
  
##  <a name="bk_orderby_clause"></a> ORDER BY-Klausel  
 Gibt die Sortierreihenfolge für die von der Abfrage zurückgegebenen Ergebnisse an.  
  
 **Syntax**  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Argumente**  
  
-   `<sort_specification>`  
  
     Gibt eine Eigenschaft oder einen Ausdruck an, wonach das Resultset der Abfrage sortiert werden soll. Eine Sortierspalte kann als Name oder Spaltenalias angegeben werden.  
  
     Es können mehrere Sortierspalten angegeben werden. Spaltennamen müssen eindeutig sein. Die Sequenz der Sortierspalten in der ORDER BY-Klausel definiert die Anordnung des sortierten Resultsets. Dies bedeutet: Das Resultset wird nach der ersten Eigenschaft sortiert, und dann wird diese sortierte Liste nach der zweiten Eigenschaft sortiert usw.  
  
     Die Spaltennamen, auf die in der ORDER BY-Klausel verwiesen wird, müssen entweder einer Spalte in der Auswahlliste oder einer Spalte entsprechen, die in einer Tabelle definiert ist, die in der FROM-Klausel eindeutig angegeben ist.  
  
-   `<sort_expression>`  
  
     Gibt eine einzelne Eigenschaft oder einen Ausdruck an, wonach das Resultset der Abfrage sortiert werden soll.  
  
-   `<scalar_expression>`  
  
     Weitere Informationen finden Sie im Abschnitt [Skalarausdrücke](#bk_scalar_expressions).  
  
-   `ASC | DESC`  
  
     Gibt an, dass die Werte in der angegebenen Spalte in aufsteigender oder absteigender Reihenfolge sortiert werden sollen. ASC sortiert vom niedrigsten Wert zum höchsten Wert. DESC sortiert vom höchsten Wert zum niedrigsten Wert. ASC ist die Standardsortierreihenfolge. NULL-Werte werden als die niedrigsten möglichen Werte behandelt.  
  
 **Hinweise:**  
  
 Die Abfragegrammatik unterstützt mehrere ORDER BY-Eigenschaften, die Abfragelaufzeit von Azure Cosmos DB hingegen nur die Sortierung für eine einzelne Eigenschaft, und nur für Eigenschaftennamen, d.h. nicht für berechnete Eigenschaften. Das Sortieren erfordert auch, dass die Indizierungsrichtlinie einen Bereichsindex für die Eigenschaft und den angegebenen Typ mit der maximalen Genauigkeit enthält. Weitere Informationen finden Sie in der Dokumentation zur Indizierungsrichtlinie.  
  
##  <a name="bk_scalar_expressions"></a> Skalarausdrücke  
 Ein skalarer Ausdruck ist eine Kombination aus Symbolen und Operatoren, die ausgewertet werden können, um einen einzelnen Wert zu erhalten. Einfache Ausdrücke können Konstanten, Eigenschaftenverweise, Arrayelementverweise, Aliasverweise oder Funktionsaufrufe sein. Einfache Ausdrücke können mit Operatoren in komplexen Ausdrücken kombiniert werden.  
  
 Ausführliche Informationen zu Werten, die skalare Ausdrücke aufweisen können, finden Sie im Abschnitt [Konstanten](#bk_constants).  
  
 **Syntax**  
  
```  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Argumente**  
  
-   `<constant>`  
  
     Stellt einen konstanten Wert dar. Details hierzu finden Sie im Abschnitt [Konstanten](#bk_constants).  
  
-   `input_alias`  
  
     Stellt einen Wert dar, der von dem `input_alias` definiert wird, der in der `FROM` Klausel eingeführt wird.  
    Dieser Wert ist garantiert nicht **undefiniert** –**undefinierte** Werte in der Eingabe werden übersprungen.  
  
-   `<scalar_expression>.property_name`  
  
     Stellt einen Wert der Eigenschaft eines Objekts dar. Wenn die Eigenschaft nicht vorhanden ist, oder über einen Wert, der kein Objekt ist, auf die Eigenschaft verwiesen wird, ergibt der Ausdruck den Wert **undefiniert**.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Stellt einen Wert der Eigenschaft mit dem Namen `property_name` oder ein Arrayelement mit Index `array_index` eines Objekts/Arrays dar. Wenn die Eigenschaft/der Arrayindex nicht vorhanden ist, oder über einen Wert, der kein Objekt/Array ist, auf die Eigenschaft/den Arrayindex verwiesen wird, wird der Ausdruck als undefinierter Wert ausgewertet.  
  
-   `unary_operator <scalar_expression>`  
  
     Stellt einen Operator dar, der auf einen einzelnen Wert angewendet wird. Details hierzu finden Sie im Abschnitt [Operatoren](#bk_operators).  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     Stellt einen Operator dar, der auf zwei Werte angewendet wird. Details hierzu finden Sie im Abschnitt [Operatoren](#bk_operators).  
  
-   `<scalar_function_expression>`  
  
     Stellt einen Wert dar, der durch ein Ergebnis eines Funktionsaufrufs definiert wird.  
  
-   `udf_scalar_function`  
  
     Name der benutzerdefinierten skalaren Funktion.  
  
-   `builtin_scalar_function`  
  
     Name der integrierten skalaren Funktion.  
  
-   `<create_object_expression>`  
  
     Stellt einen Wert dar, der durch Erstellen eines neuen Objekts mit festgelegten Eigenschaften und ihren Werten gewonnen wird.  
  
-   `<create_array_expression>`  
  
     Stellt einen Wert dar, der durch Erstellen eines neuen Arrays mit festgelegten Werten als Elemente gewonnen wird.  
  
-   `parameter_name`  
  
     Stellt einen Wert des angegebenen Parameternamens dar. Das erste Zeichen eines Parameternamens muss ein einzelnes „@“ sein.  
  
 **Hinweise:**  
  
 Beim Aufrufen einer integrierten oder benutzerdefinierten skalaren Funktion müssen alle Argumente definiert werden. Wenn eines der Argumente undefiniert ist, wird die Funktion nicht aufgerufen, und das Ergebnis ist undefiniert.  
  
 Beim Erstellen eines Objekts wird jede Eigenschaft, der ein undefinierter Wert zugewiesen ist, übersprungen und nicht in das erstellte Objekt einbezogen.  
  
 Beim Erstellen eines Arrays wird jeder Elementwert, dem ein **undefinierter** Wert zugewiesen ist, übersprungen und nicht in das erstellte Objekt einbezogen. Daraufhin nimmt das nächste definierte Element seinen Platz ein, sodass das erstellte Array keine übersprungenen Indizes aufweist.  
  
##  <a name="bk_operators"></a> Operatoren  
 Dieser Abschnitt beschreibt die unterstützten Operatoren. Jeder Operator kann genau einer Kategorie zugewiesen werden.  
  
 In der folgenden Tabelle **Operatorkategorien** finden Sie weitere Informationen zur Behandlung **undefinierter** Werte, zu Typanforderungen für Eingabewerte sowie zur Behandlung von Werten mit nicht übereinstimmenden Typen.  
  
 **Operatorkategorien:**  
  
|**Kategorie**|**Details**|  
|-|-|  
|**Arithmetisch**|Der Operator erwartet (eine) Zahl(en) als Eingabe(n). Die Ausgabe ist ebenfalls eine Zahl. Wenn eine der Eingaben **undefiniert** ist oder einen anderen Typ als „Number“ aufweist, ist das Ergebnis **undefiniert**.|  
|**Bitweise**|Der Operator erwartet (eine) 32-Bit-Ganzzahl(en) mit Vorzeichen als Eingabe(n). Die Ausgabe ist ebenfalls eine 32-Bit-Ganzzahl mit Vorzeichen.<br /><br /> Jeder nicht ganzzahlige Wert wird gerundet. Positive Werte werden abgerundet, negative Werte aufgerundet.<br /><br /> Jeder Wert, der außerhalb des 32-Bit-Ganzzahlbereichs liegt, wird unter Verwendung der letzten 32 Bits seiner Zweierkomplement-Schreibweise konvertiert.<br /><br /> Wenn eine der Eingaben **undefiniert** ist oder einen anderen Typ als „Number“ aufweist, ist das Ergebnis **undefiniert**.<br /><br /> **Hinweis:** das oben beschriebene Verhalten ist kompatibel mit dem JavaScript-Verhalten des bitweisen Operators.|  
|**Logisch**|Der Operator erwartet (eine) boolesche Eingabe(n). Die Ausgabe ist ebenfalls boolesch.<br />Wenn eine der Eingaben **undefiniert** ist oder einen anderen Typ als „boolesch“ aufweist, ist das Ergebnis **undefiniert**.|  
|**Vergleich**|Der Operator erwartet, dass die Eingabe(n) vom gleichen Typ und nicht undefiniert ist (sind). Die Ausgabe ist ein boolescher Wert.<br /><br /> Wenn eine der Eingaben **undefiniert** ist oder sie verschiedene Typen aufweisen, ist das Ergebnis **undefiniert**.<br /><br /> In der Tabelle **Reihenfolge der Werte für den Vergleich** finden Sie weitere Informationen zur Reihenfolge der Werte.|  
|**string**|Der Operator erwartet String(s) als Eingabe(n). Die Ausgabe ist ebenfalls ein String.<br />Wenn eine der Eingaben **undefiniert** ist oder einen anderen Typ als „String“ aufweist, ist das Ergebnis **undefiniert**.|  
  
 **Unäre Operatoren:**  
  
|**Name**|**Operator**|**Details**|  
|-|-|-|  
|**Arithmetisch**|+<br /><br /> -|Gibt den Zahlenwert zurück.<br /><br /> Bitweise Negation. Gibt den negierten Zahlenwert zurück.|  
|**Bitweise**|~|Einerkomplement. Gibt ein Komplement eines Zahlenwerts zurück.|  
|**Logisch**|**NOT**|Negation. Gibt den negierten booleschen Wert zurück.|  
  
 **Binäre Operatoren:**  
  
|**Name**|**Operator**|**Details**|  
|-|-|-|  
|**Arithmetisch**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Addition.<br /><br /> Subtraktion.<br /><br /> Multiplikation.<br /><br /> Division.<br /><br /> Modulation.|  
|**Bitweise**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bitweises OR.<br /><br /> Bitweises AND.<br /><br /> Bitweises XOR.<br /><br /> Verschiebung nach links.<br /><br /> Verschiebung nach rechts.<br /><br /> Nullauffüllung, Verschiebung nach rechts.|  
|**Logisch**|**AND**<br /><br /> **OR**|Logische Konjunktion. Gibt **true** zurück, wenn beide Argumente **true** sind, andernfalls **false**.<br /><br /> Logische Konjunktion. Gibt **true** zurück, wenn beide Argumente **true** sind, andernfalls **false**.|  
|**Vergleich**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Gleich. Gibt **true** zurück, wenn die Argumente gleich sind, andernfalls **false**.<br /><br /> Ungleich. Gibt **true** zurück, wenn die Argumente nicht gleich sind, andernfalls **false**.<br /><br /> Größer als. Gibt **true** zurück, wenn das erste Argument größer als das zweite Argument ist, andernfalls **false**.<br /><br /> Größer als oder gleich. Gibt **true** zurück, wenn das erste Argument größer als das zweite Argument oder ihm gleich ist, andernfalls **false**.<br /><br /> Kleiner als. Gibt **true** zurück, wenn das erste Argument kleiner als das zweite Argument ist, andernfalls **false**.<br /><br /> Kleiner als oder gleich. Gibt **true** zurück, wenn das erste Argument kleiner als das zweite Argument oder ihm gleich ist, andernfalls **false**.<br /><br /> Zusammenfügen. Gibt das zweite Argument zurück, wenn das erste Argument ein **undefinierter** Wert ist.|  
|**String**|**&#124;&#124;**|Verkettung. Gibt eine Verkettung der beiden Argumente zurück.|  
  
 **Ternäre Operatoren:**  
  
|Ternärer Operator|?|Gibt das zweite Argument zurück, wenn die Auswertung des ersten Arguments **true** ergibt; gibt andernfalls das dritte Argument zurück.|  
|-|-|-|  
  
 **Reihenfolge der Werte für den Vergleich**  
  
|**Typ**|**Reihenfolge der Werte**|  
|-|-|  
|**Undefiniert**|Nicht vergleichbar.|  
|**NULL**|Einzelner Wert: **NULL**|  
|**Number**|Natürliche reelle Zahl.<br /><br /> Der negative Unendlichkeitswert ist kleiner als jeder andere Number-Wert.<br /><br /> Der positive Unendlichkeitswert ist größer als jeder andere Number-Wert. **NaN**-Wert ist nicht vergleichbar. Vergleich mit **NaN** führt zu **undefiniertem** Wert.|  
|**String**|Lexikografische Reihenfolge.|  
|**Array**|Keine Reihenfolge, aber gleichberechtigt.|  
|**Object**|Keine Reihenfolge, aber gleichberechtigt.|  
  
 **Hinweise:**  
  
 In Azure Cosmos DB sind die Typen von Werten oft nicht bekannt, bis sie tatsächlich aus der Datenbank abgerufen werden. Um die effiziente Ausführung von Abfragen zu ermöglichen, haben die meisten Operatoren strikte Typanforderungen. Außerdem führen Operatoren selbst keine impliziten Konvertierungen aus.  
  
 Dies bedeutet, dass eine Abfrage wie „SELECT * FROM ROOT r WHERE r.Age = 21“ nur Dokumente zurückgibt, deren Eigenschaft „Age“ der Zahl 21 entspricht. Dokumente, deren Eigenschaft „Age“ der Zeichenfolge „21“ oder der Zeichenfolge „0021“ entspricht, stimmen nicht überein, da der Ausdruck „21“ = 21 als undefiniert ausgewertet wird. Dies ermöglicht eine bessere Verwendung von Indizes, da die Suche nach einem bestimmten Wert (d.h. der Zahl 21) schneller ist als die Suche nach einer unbestimmten Anzahl möglicher Übereinstimmungen (d.h. Zahl 21 oder Zeichenfolgen „21“, „021“, „21,0“... ). Dies unterscheidet sich von der Art, in der JavaScript Operatoren für Werte verschiedener Typen auswertet.  
  
 **Arrays und Objekte – Gleichheit und Vergleich**  
  
 Das Ergebnis des Vergleichs von Array- und Object-Werten mithilfe von Bereichsoperatoren (>, > =, <, < =) ist undefiniert, weil für Object- oder Array-Werte keine Reihenfolge definiert ist. Die Verwendung von Gleichheits-/Ungleichheitsoperatoren (=, ! =, <>) wird jedoch unterstützt, und die Werte werden strukturell verglichen.  
  
 Arrays sind gleich, wenn beide Arrays die gleiche Anzahl von Elementen aufweisen, und Elemente an übereinstimmenden Positionen ebenfalls gleich sind. Wenn das Ergebnis des Vergleichs eines beliebigen Paars von Elementen undefiniert ist, ist das Ergebnis des Arrayvergleichs undefiniert.  
  
 Objekte sind gleich, wenn für beide Objekte die gleichen Eigenschaften definiert sind, und wenn Werte von übereinstimmenden Eigenschaften auch gleich sind. Wenn das Ergebnis des Vergleichs eines beliebigen Paars von Eigenschaftswerten undefiniert ist, ist das Ergebnis des Objektvergleichs undefiniert.  
  
##  <a name="bk_constants"></a> Konstanten  
 Eine Konstante, auch bekannt als Literal oder skalarer Wert, ist ein Symbol, das einen bestimmten Datenwert darstellt. Das Format einer Konstante hängt vom Datentyp des Werts ab, den sie darstellt.  
  
 **Unterstützte skalare Datentypen:**  
  
|**Typ**|**Reihenfolge der Werte**|  
|-|-|  
|**Undefiniert**|Einzelner Wert: **undefiniert**|  
|**NULL**|Einzelner Wert: **NULL**|  
|**Boolescher Wert**|Werte: **false**, **true**.|  
|**Number**|Eine Gleitkommazahl mit doppelter Genauigkeit, IEEE 754-Standard.|  
|**String**|Eine Sequenz von null (0) oder mehr Unicode-Zeichen. Zeichenfolgen müssen in einfache oder doppelte Anführungszeichen eingeschlossen sein.|  
|**Array**|Eine Sequenz von null (0) oder mehr Elementen. Jedes Element kann einen Wert eines beliebigen skalaren Datentyps mit Ausnahme von „Undefiniert“ aufweisen.|  
|**Object**|Ungeordneter Satz von null (0) oder mehr Name/Wert-Paaren. Der Name ist eine Unicode-Zeichenfolge, der Wert kann einen beliebigen skalaren Datentyp mit Ausnahme von **Undefiniert** aufweisen.|  
  
 **Syntax**  
  
```  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Argumente**  
  
1.  `<undefined_constant>; undefined`  
  
     Stellt einen undefinierten Wert vom Typ „Undefiniert“ dar.  
  
2.  `<null_constant>; null`  
  
     Stellt einen **NULL**-Wert des Typs **NULL** dar.  
  
3.  `<boolean_constant>`  
  
     Stellt eine Konstante des Typs „Boolesch“ dar.  
  
4.  `false`  
  
     Stellt einen **false**-Wert des Typs „Boolesch“ dar.  
  
5.  `true`  
  
     Stellt einen **true**-Wert des Typs „Boolesch“ dar.  
  
6.  `<number_constant>`  
  
     Stellt eine Konstante dar.  
  
7.  `decimal_literal`  
  
     Dezimalliterale sind Zahlen, die mithilfe von Dezimal- oder wissenschaftlicher Schreibweise dargestellt werden.  
  
8.  `hexadecimal_literal`  
  
     Hexadezimalliterale sind Zahlen, die mit Präfix „0x“ gefolgt von einer oder mehreren hexadezimalen Ziffern dargestellt werden.  
  
9. `<string_constant>`  
  
     Stellt eine Konstante des Typs „String“ dar.  
  
10. `string _literal`  
  
     Zeichenfolgenliterale sind Unicode-Zeichenfolgen, die durch eine Sequenz aus null oder mehr Unicode-Zeichen oder Escapesequenzen dargestellt werden. Zeichenfolgenliterale werden in einfache Anführungszeichen (Apostrophen: ') oder doppelte Anführungszeichen (Anführungszeichen: ") eingeschlossen.  
  
 Folgende Escapesequenzen sind zulässig:  
  
|**Escapesequenz**|**Beschreibung**|**Unicode-Zeichen**|  
|-|-|-|  
|\\'|Apostroph (')|U+0027|  
|\\"|Anführungszeichen (")|U+0022|  
|\\\|umgekehrter Schrägstrich (\\)|U+005C|  
|\\/|Schrägstrich (/)|U+002F|  
|\b|Rücktaste|U+0008|  
|\f|Seitenvorschub|U+000C|  
|\n|Zeilenvorschub|U+000A|  
|\r|Wagenrücklauf|U+000D|  
|\t|Tabulatorzeichen|U+0009|  
|\uXXXX|Ein Unicode-Zeichen, das durch 4 Hexadezimalstellen definiert wird.|U+XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a> Leistungsrichtlinien zur Abfrage  
 Damit eine Abfrage für eine große Sammlung effizient ausgeführt werden kann, sollte sie Filter verwenden, die über einen oder mehrere Indizes bedient werden können.  
  
 Die folgenden Filter werden für die Indexsuche berücksichtigt:  
  
-   Verwenden des Gleichheitsoperators (=) mit einem Dokumentenpfadausdruck und einer Konstanten.  
  
-   Verwenden von Bereichsoperatoren (<, \<=, >, >=) mit einem Dokumentenpfadausdruck und Zahlenkonstanten.  
  
-   Dokumentenpfadausdruck steht für jeden Ausdruck, der einen Konstantenpfad in den Dokumenten aus der referenzierten Datenbanksammlung identifiziert.  
  
 **Dokumentenpfadausdruck**  
  
 Dokumentenpfadausdrücke sind Ausdrücke, die ein Eigenschaftenpfad oder Arrayindexer für ein Dokument auswertet, das aus Datenbanksammlungs-Dokumenten stammt. Dieser Pfad kann zur Identifizierung des Speicherorts von Werten verwendet werden, auf die in einem Filter direkt innerhalb der Dokumente in der Datenbanksammlung verwiesen wird.  
  
 Für einen Ausdruck, der als Dokumentenpfadausdruck in Betracht kommt, sollten folgende Voraussetzungen gelten:  
  
1.  Direktes Verweisen auf den Stamm der Sammlung.  
  
2.  Verweisen auf Eigenschaften- oder Konstantenarrayindexer eines Dokumentenpfadausdrucks.  
  
3.  Verweisen auf einen Alias, der einen Dokumentenpfadausdruck darstellt.  
  
     **Syntaxkonventionen**  
  
     Die folgende Tabelle beschreibt die Konventionen zum Beschreiben der Syntax in der Referenz der Abfragesprache der DocumentDB-API.  
  
    |**Konvention**|**Verwendung**|  
    |-|-|    
    |GROSSBUCHSTABEN|Schlüsselwörter, bei denen Groß-/Kleinschreibung nicht beachtet werden muss.|  
    |Kleinbuchstaben|Schlüsselwörter, bei denen Groß-/Kleinschreibung beachtet werden muss.|  
    |\<nichtterminal>|Nichtterminal, separat definiert.|  
    |\<nichtterminal> ::=|Syntaxdefinition von „nichtterminal“.|  
    |other_terminal|Terminal (Token), ausführlich in Wörtern beschrieben.|  
    |Bezeichner|Bezeichner. Ermöglicht nur die folgenden Zeichen: a-z, A-Z 0-9 _Erstes Zeichen darf keine Ziffer sein.|  
    |„String“|Zeichenfolge in Anführungszeichen. Kann jede gültige Zeichenfolge ein. Siehe Beschreibung von „string_literal“.|  
    |'Symbol'|Literalsymbol, das Teil der Syntax ist.|  
    |&#124; (senkrechter Strich)|Alternativen für Syntaxelemente. Sie können nur eines der angegebenen Elemente verwenden.|  
    |[] /(Klammern)|Klammern schließen ein oder mehrere optionale Elemente ein.|  
    |[ ,...n ]|Gibt an, dass das vorherige Element n-Mal wiederholt werden kann. Die einzelnen Vorkommen werden durch Kommas getrennt.|  
    |[ ...n ]|Gibt an, dass das vorherige Element n-Mal wiederholt werden kann. Die einzelnen Vorkommen werden durch Leerzeichen getrennt.|  
  
##  <a name="bk_built_in_functions"></a> Integrierte Funktionen  
 Azure Cosmos DB bietet viele integrierte SQL-Funktionen. Die Kategorien der integrierten Funktionen sind unten aufgeführt.  
  
|Funktion|Beschreibung|  
|--------------|-----------------|  
|[Mathematische Funktionen](#bk_mathematical_functions)|Jede mathematische Funktion führt eine Berechnung durch, üblicherweise basierend auf Eingabewerten, die als Argument bereitgestellt werden, und gibt einen numerischen Wert zurück.|  
|[Funktionen für die Typüberprüfung](#bk_type_checking_functions)|Mit den Funktionen für die Typprüfung können Sie den Typ eines Ausdrucks in SQL-Abfragen prüfen.|  
|[Zeichenfolgenfunktionen](#bk_string_functions)|Die folgenden Stringfunktionen führen einen Vorgang für einen Zeichenfolgen-Eingabewert durch und geben eine Zeichenfolge, einen numerischen Wert oder einen booleschen Wert zurück.|  
|[Arrayfunktionen](#bk_array_functions)|Die Arrayfunktionen führen einen Vorgang für einen Arrayeingabewert durch und geben einen numerischen Wert, booleschen Wert oder Arraywert zurück.|  
|[Räumliche Funktionen](#bk_spatial_functions)|Die räumlichen Funktionen führen einen Vorgang für einen Raumobjekt-Eingabewert durch und geben einen numerischen oder booleschen Wert zurück.|  
  
###  <a name="bk_mathematical_functions"></a> Mathematische Funktionen  
 Die folgenden Funktionen führen jeweils eine Berechnung durch, üblicherweise basierend auf Eingabewerten, die als Argumente bereitgestellt werden, und geben einen numerischen Wert zurück.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[CEILING](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DEGREES](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOG](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[POWER](#bk_power)|  
|[RADIANS](#bk_radians)|[ROUND](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[SQUARE](#bk_square)|[SIGN](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a> ABS  
 Gibt den absoluten (positiven) Wert des angegebenen numerischen Ausdrucks zurück.  
  
 **Syntax**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Ergebnisse der Anwendung der ABS-Funktion auf drei verschiedene Zahlen.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Kosinus der angegebene numerische Ausdruck ist. Wird auch als Arkuskosinus bezeichnet.  
  
 **Syntax**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den ACOS von -1 zurück.  
  
```  
SELECT ACOS(-1)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Sinus der angegebene numerische Ausdruck ist. Wird auch als Arkussinus bezeichnet.  
  
 **Syntax**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den ASIN für -1 zurück.  
  
```  
SELECT ASIN(-1)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Tangens der angegebene numerische Ausdruck ist. Wird auch als Arkustangens bezeichnet.  
  
 **Syntax**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den ATAN des angegebenen Werts zurück.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Gibt den Hauptwert des Arcustangens von y/x zurück, ausgedrückt in Bogenmaß.  
  
 **Syntax**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel berechnet ATN2 für die angegebenen x- und y-Komponenten.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> CEILING  
 Gibt den kleinsten ganzzahligen Wert zurück, der größer oder gleich dem angegebenen numerischen Ausdruck ist.  
  
 **Syntax**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die positiven numerischen, negativen und Nullwerte mit der CEILING-Funktion an.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Gibt den trigonometrischen Kosinus des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen Ausdruck zurück.  
  
 **Syntax**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel berechnet den COS des angegebenen Winkels.  
  
```  
SELECT COS(14.78)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Gibt den trigonometrischen Kotangens des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen numerischen Ausdruck zurück.  
  
 **Syntax**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird der COT des angegebenen Winkels berechnet.  
  
```  
SELECT COT(124.1332)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> DEGREES  
 Gibt den entsprechenden Winkel in Grad für einen in „rad“ (Bogenmaß) angegebenen Winkel zurück.  
  
 **Syntax**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt die Gradzahl in einem Winkel des PI/2-Bogenmaßes zurück.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a> FLOOR  
 Gibt den größten ganzzahligen Wert zurück, der kleiner oder gleich dem angegebenen numerischen Ausdruck ist.  
  
 **Syntax**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die positiven numerischen, negativen und Nullwerte mit der FLOOR-Funktion an.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Gibt den Exponentialwert des angegebenen numerischen Ausdrucks zurück.  
  
 **Syntax**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Hinweise:**  
  
 Die Konstante **e** (2,718281...) ist die Basis des natürlichen Logarithmus.  
  
 Der Exponent einer Zahl ist die Konstante **e** potenziert mit der Zahl. Beispiel: EXP(1,0) = e^1,0 = 2,71828182845905 und EXP(10) = e^10 = 22026,4657948067.  
  
 Der Exponentialwert des natürlichen Logarithmus einer Zahl ist die Zahl selbst: EXP (LOG (n)) = n. Und der natürliche Logarithmus des Exponentialwerts einer Zahl ist die Zahl selbst: LOG (EXP (n)) = n.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird eine Variable deklariert und der Exponentialwert der angegebenen Variablen zurückgegeben (10).  
  
```  
SELECT EXP(10)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 Das folgende Beispiel gibt den Exponentialwert des natürlichen Logarithmus von 20 und den natürlichen Logarithmus des Exponentialwerts von 20 zurück. Da diese Funktionen Umkehrfunktionen der jeweils anderen sind, beträgt der Rückgabewert mit Rundung für die Gleitkommaberechnung in beiden Fällen 20.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a> LOG  
 Gibt den natürlichen Logarithmus des angegebenen numerischen Ausdrucks zurück.  
  
 **Syntax**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
-   `base`  
  
     Optionales numerisches Argument, das die Basis des Logarithmus festlegt.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Hinweise:**  
  
 Standardmäßig gibt LOG() den natürlichen Logarithmus zurück. Sie können die Basis des Logarithmus mithilfe des optionalen base-Parameters in einen anderen Wert ändern.  
  
 Der natürliche Logarithmus ist der Logarithmus zur Basis **e**, wobei **e** eine Irrationale Konstante ist, die ungefähr 2,718281828 entspricht.  
  
 Der natürliche Logarithmus des Exponentialwerts einer Zahl ist die Zahl selbst: LOG( EXP(n) ) = n. Und der Exponentialwert des natürlichen Logarithmus einer Zahl ist die Zahl selbst: EXP( LOG (n) ) = n.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird eine Variable deklariert und der Logarithmuswert der angegebenen Variablen zurückgegeben (10).  
  
```  
SELECT LOG(10)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 Im folgenden Beispiel wird der LOG für den Exponenten einer Zahl berechnet.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Gibt den dekadischen Logarithmus des angegebenen numerischen Ausdrucks zurück.  
  
 **Syntax**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Hinweise:**  
  
 Die Funktionen LOG10 und POWER sind umgekehrt miteinander verknüpft. Beispiel: 10 ^ LOG10(n) = n.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird eine Variable deklariert und der LOG10-Wert der angegebenen Variablen zurückgegeben (100).  
  
```  
SELECT LOG10(100)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 Gibt den konstanten Wert von PI zurück.  
  
 **Syntax**  
  
```  
PI ()  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den Wert von PI zurück.  
  
```  
SELECT PI()  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> POWER  
 Gibt den Wert des angegebenen Ausdrucks gemäß der angegebenen Potenz zurück.  
  
 **Syntax**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
-   `y`  
  
     Ist der Wert, mit der `numeric_expression` potenziert wird.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt das Potenzieren einer Zahl mit 3 (dritte Potenz der Zahl).  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADIANS  
 Gibt das Bogenmaß zurück, wenn ein numerischer Ausdruck in Grad eingegeben wird.  
  
 **Syntax**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel werden einige Winkel eingegeben und ihre entsprechenden Bogenmaßwerte zurückgegeben.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Hier ist das Resultset.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> ROUND  
 Gibt einen numerischen Wert zurück, gerundet auf den nächsten ganzzahligen Wert.  
  
 **Syntax**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel werden die folgenden positiven und negativen Zahlen auf die nächste ganze Zahl gerundet.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a> SIGN  
 Gibt das positive Vorzeichen (+1), null (0) oder das negative Vorzeichen (-1) des angegebenen numerischen Ausdrucks zurück.  
  
 **Syntax**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel werden die SIGN-Werte der Zahlen von -2 bis 2 zurückgegeben.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Gibt den trigonometrischen Sinus des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen Ausdruck zurück.  
  
 **Syntax**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel berechnet den SIN des angegebenen Winkels.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> SQRT  
 Gibt die Quadratwurzel des angegebenen numerischen Werts zurück.  
  
 **Syntax**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel werden die Quadratwurzeln der Zahlen von 1 bis 3 zurückgegeben.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> SQUARE  
 Gibt das Quadrat des angegebenen numerischen Werts zurück.  
  
 **Syntax**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel werden die Quadrate der Zahlen von 1 bis 3 zurückgegeben.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Gibt den Tangens des angegebenen Winkels in Bogenmaß im angegebenen Ausdruck zurück.  
  
 **Syntax**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel berechnet den Tangens von PI()/2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> TRUNC  
 Gibt einen numerischen Wert zurück, gekürzt auf den nächsten ganzzahligen Wert.  
  
 **Syntax**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumente**  
  
-   `numeric_expression`  
  
     Ist ein numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel werden die folgenden positiven und negativen Zahlen auf den nächsten ganzzahligen Wert abgeschnitten.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a> Funktionen für die Typüberprüfung  
 Die folgenden Funktionen unterstützen die Typüberprüfung für Eingabewerte und geben jeweils einen booleschen Wert zurück.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Array“ ist.  
  
 **Syntax**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_ARRAY-Funktion.  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „boolesch“ ist.  
  
 **Syntax**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_BOOL-Funktion.  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Gibt einen booleschen Wert zurück, um anzugeben, ob der Eigenschaft ein Wert zugewiesen wurde.  
  
 **Syntax**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft das Vorhandensein einer Eigenschaft im angegebenen JSON-Dokument. Die erste Überprüfung gibt "true" zurück, da „a“ vorhanden ist, aber die zweite „false“, da „b“ nicht vorhanden ist.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Hier ist das Resultset.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „NULL“ ist.  
  
 **Syntax**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_NULL-Funktion.  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Zahl“ ist.  
  
 **Syntax**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_NULL-Funktion.  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „JSON-Objekt“ ist.  
  
 **Syntax**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_OBJECT-Funktion.  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck ein Grundtyp ist (Zeichenfolge, boolesch, numerisch oder NULL).  
  
 **Syntax**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_PRIMITIVE-Funktion.  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Zeichenfolge“ ist.  
  
 **Syntax**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumente**  
  
-   `expression`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft JSON-Objekte des Typs boolesch, Number, String, NULL, Object, Array und undefiniert mithilfe der IS_STRING-Funktion.  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a> Zeichenfolgenfunktionen  
 Die folgenden Skalarfunktionen führen einen Vorgang für einen Zeichenfolgen-Eingabewert durch und geben eine Zeichenfolge, einen numerischen Wert oder einen booleschen Wert zurück.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[CONTAINS](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTH](#bk_length)|  
|[LOWER](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICATE](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[SUBSTRING](#bk_substring)|  
|[UPPER](#bk_upper)|||  
  
####  <a name="bk_concat"></a> CONCAT  
 Gibt eine Zeichenfolge zurück, die das Ergebnis der Verkettung von zwei oder mehr Zeichenfolgenwerten darstellt.  
  
 **Syntax**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt die verkettete Zeichenfolge der angegebenen Werte zurück.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> CONTAINS  
 Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck den zweiten enthält.  
  
 **Syntax**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird überprüft, ob „abc“ „ab“ und „d“ enthält.  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten endet.  
  
 **Syntax**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird zurückgegeben, ob „abc“ mit „b“ und „bc“ endet.  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 Gibt die Anfangsposition des ersten Vorkommens des zweiten Zeichenfolgenausdrucks innerhalb des ersten angegebenen Zeichenfolgenausdrucks zurück, oder -1, wenn die Zeichenfolge nicht gefunden wird.  
  
 **Syntax**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den Index der verschiedenen Teilzeichenfolgen innerhalb von „abc“ zurück.  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a> LEFT  
 Gibt den linken Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen zurück.  
  
 **Syntax**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
-   `num_expr`  
  
     Ist ein beliebiger gültiger numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den linken Teil von „abc“ für verschiedene Längenwerte zurück.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "a", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a> LENGTH  
 Gibt die Anzahl der Zeichen im angegebenen Zeichenfolgenausdruck zurück.  
  
 **Syntax**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt die Länge einer Zeichenfolge zurück.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a> LOWER  
 Gibt eine Zeichenfolge zurück, nachdem Großbuchstaben in Kleinbuchstaben konvertiert wurden.  
  
 **Syntax**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von LOWER in einer Abfrage.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Gibt einen Zeichenfolgenausdruck zurück, nachdem vorangestellte Leerzeichen entfernt wurden.  
  
 **Syntax**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von LTRIM in einer Abfrage.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> REPLACE  
 Ersetzt alle Vorkommen eines angegebenen Zeichenfolgenwerts durch einen anderen Zeichenfolgenwert.  
  
 **Syntax**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von REPLACE in einer Abfrage.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> REPLICATE  
 Wiederholt einen Zeichenfolgenwert mit der angegebenen Häufigkeit.  
  
 **Syntax**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
-   `num_expr`  
  
     Ist ein beliebiger gültiger numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von REPLICATE in einer Abfrage.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> REVERSE  
 Gibt einen Zeichenfolgenwert in umgekehrter Reihenfolge zurück.  
  
 **Syntax**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von REVERSE in einer Abfrage.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a> RIGHT  
 Gibt den rechten Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen zurück.  
  
 **Syntax**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
-   `num_expr`  
  
     Ist ein beliebiger gültiger numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt den rechten Teil von „abc“ für verschiedene Längenwerte zurück.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Gibt einen Zeichenfolgenausdruck zurück, nachdem vorangestellte Leerzeichen entfernt wurden.  
  
 **Syntax**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von RTRIM in einer Abfrage.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt.  
  
 **Syntax**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel überprüft, ob die Zeichenfolge „abc“ mit „b“ und „a“ beginnt.  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a> SUBSTRING  
 Gibt einen Teil eines Zeichenfolgenausdrucks zurück. Das angegebene Zeichen ist der Nullpunkt, von dem ab die Teilzeichenfolge in angegebener Länge bzw. bis zum Ende der Zeichenfolge zurückgegeben wird.  
  
 **Syntax**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
-   `num_expr`  
  
     Ist ein beliebiger gültiger numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel gibt die Teilzeichenfolge „abc“ zurück, beginnend bei 1 und für eine Länge von 1 Zeichen.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "b"}]  
```  
  
####  <a name="bk_upper"></a> UPPER  
 Gibt eine Zeichenfolge zurück, nachdem Kleinbuchstaben in Großbuchstaben konvertiert wurden.  
  
 **Syntax**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumente**  
  
-   `str_expr`  
  
     Ist ein beliebiger gültiger Zeichenfolgenausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Zeichenfolgenausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Verwendung von UPPER in einer Abfrage.  
  
```  
SELECT UPPER("Abc")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a> Arrayfunktionen  
 Die folgenden Skalarfunktionen führen einen Vorgang für einen Arrayeingabewert durch und geben einen numerischen Wert, booleschen Wert oder Arraywert zurück.  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 Gibt ein Array zurück, das das Ergebnis der Verkettung von zwei oder mehr Arraywerten darstellt.  
  
 **Syntax**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumente**  
  
-   `arr_expr`  
  
     Ist ein beliebiger gültiger Arrayausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen Arrayausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird veranschaulicht, wie zwei Arrays verkettet werden.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
 Gibt einen booleschen Wert zurück, um anzugeben, ob das Array den angegebenen Wert enthält.  
  
 **Syntax**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 **Argumente**  
  
-   `arr_expr`  
  
     Ist ein beliebiger gültiger Arrayausdruck.  
  
-   `expr`  
  
     Ist ein beliebiger gültiger Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Wert zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Überprüfung auf Mitgliedschaft in einem Array mit ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Gibt die Anzahl der Elemente des angegebenen Arrayausdrucks zurück.  
  
 **Syntax**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumente**  
  
-   `arr_expr`  
  
     Ist ein beliebiger gültiger Arrayausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück.  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird die Länge eines Arrays mit ARRAY_LENGTH abgerufen.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Hier ist das Resultset.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Gibt einen Teil eines Arrayausdrucks zurück.
  
 **Syntax**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumente**  
  
-   `arr_expr`  
  
     Ist ein beliebiger gültiger Arrayausdruck.  
  
-   `num_expr`  
  
     Ist ein beliebiger gültiger numerischer Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Wert zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt, wie ein Teil eines Arrays mit ARRAY_SLICE abgerufen wird.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 Hier ist das Resultset.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <a name="bk_spatial_functions"></a> Räumliche Funktionen  
 Die folgenden Skalarfunktionen führen einen Vorgang für einen Raumobjekt-Eingabewert durch und geben einen numerischen oder booleschen Wert zurück.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 Gibt den Abstand zwischen den beiden GeoJSON-Punkt-, Polygon- oder LineString-Ausdrücken zurück.  
  
 **Syntax**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumente**  
  
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Objektausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen numerischen Ausdruck zurück, der den Abstand enthält. Dieser wird für das Standardreferenzsystem in Metern angegeben.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Rückgabe aller Familiendokumente, die sich innerhalb von 30 km von der angegebenen Position befinden. Dazu wird die integrierte ST_DISTANCE-Funktion verwendet. zu erstellen und zu verwalten.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Hier ist das Resultset.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 Gibt einen booleschen Ausdruck zurück, der angibt, ob das im ersten Argument angegebene GeoJSON-Objekt (Punkt, Polygon oder LineString) im zweiten Argument im GeoJSON-Objekt (Punkt, Polygon oder LineString) enthalten ist.  
  
 **Syntax**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumente**  
  
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Objektausdruck.  
 
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Objektausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Wert zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt, wie alle Familiendokumente mit ST_WITHIN innerhalb eines Polygons gefunden werden.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier ist das Resultset.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 Gibt einen booleschen Ausdruck zurück, der angibt, ob das im ersten Argument angegebene GeoJSON-Objekt (Punkt, Polygon oder LineString) sich mit dem GeoJSON-Objekt (Punkt, Polygon oder LineString) im zweiten Argument überschneidet.  
  
 **Syntax**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumente**  
  
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Objektausdruck.  
 
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Objektausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Wert zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt, wie alle Bereiche gefunden werden, die sich mit dem angegebenen Polygon überschneiden.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier ist das Resultset.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Gibt einen booleschen Wert zurück, der angibt, ob der/das angegebene GeoJSON-Punkt, -Polygon, oder -LineString gültig ist.  
  
 **Syntax**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumente**  
  
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt-, -Polygon- oder -LineString-Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen booleschen Ausdruck zurück.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt die Überprüfung mit ST_VALID, ob ein Punkt gültig ist.  
  
 Dieser Punkt hat z.B. einen Breitengradwert, der nicht im gültigen Wertebereich [-90, 90] liegt, sodass die Abfrage „false“ zurückgibt.  
  
 Für Polygone erfordert die GeoJSON-Spezifikation, dass das letzte angegebene Koordinatenpaar mit dem ersten identisch sein muss, um eine geschlossene Form zu bilden. Punkte innerhalb eines Polygons müssen gegen den Uhrzeigersinn nacheinander angegeben werden. Ein Polygon, das im Uhrzeigersinn angegeben wird, stellt die Umkehrung der darin enthaltenen Region dar.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Hier ist das Resultset.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Gibt einen JSON-Wert mit einem booleschen Wert zurück, wenn der angegebene GeoJSON-Punkt-, -Polygon- oder -LineString-Ausdruck gültig ist. Falls ungültig, wird außerdem der Grund als Zeichenfolge zurückgegeben.  
  
 **Syntax**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumente**  
  
-   `spatial_expr`  
  
     Ist ein beliebiger gültiger GeoJSON-Punkt- oder -Polygon-Ausdruck.  
  
 **Rückgabetypen**  
  
 Gibt einen JSON-Wert mit einem booleschen Wert zurück, wenn der angegebene GeoJSON-Punkt oder -Polygon-Ausdruck gültig ist. Falls ungültig, wird außerdem der Grund als Zeichenfolge zurückgegeben.  
  
 **Beispiele**  
  
 Das folgende Beispiel zeigt das Überprüfen der Gültigkeit (mit Details) mit ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Hier ist das Resultset.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Nächste Schritte  
 [SQL-Syntax und SQL-Abfrage für Azure Cosmos DB](documentdb-sql-query.md)   
 [Dokumentation für Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  

