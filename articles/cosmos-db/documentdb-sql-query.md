---
title: "SQL-Abfragen für Azure Cosmos DB-DocumentDB-API | Microsoft-Dokumentation"
description: "Informationen zu SQL-Syntax, Datenbankkonzepten und SQL-Abfragen für Azure Cosmos DB. SQL kann als JSON-Abfragesprache in Azure Cosmos DB verwendet werden."
keywords: SQL-Syntax, SQL-Abfrage, SQL-Abfragen, JSON-Abfragesprache, Datenbankkonzepte und SQL-Abfragen, Aggregatfunktionen
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 862594bcbd6df8a2c62a12340ceb8096fb6bd691
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a>SQL-Abfragen für Azure Cosmos DB-DocumentDB-API
Microsoft Azure Cosmos DB unterstützt Dokumentabfragen mit SQL (Structured Query Language) als JSON-Abfragesprache. Azure Cosmos DB funktioniert ohne Schema. Die direkte Integration des JSON-Datenmodells in das Datenbankmodul ermöglicht eine automatische Indexierung von JSON-Dokumenten ohne explizite Schemas oder die Erstellung sekundärer Indizes. 

Bei der Entwicklung der Abfragesprache für Cosmos DB hatten wir zwei Hauptziele:

* Wir wollten SQL unterstützen, anstatt eine neue JSON-Abfragesprache zu entwickeln. SQL ist eine der meistverwendeten und beliebtesten Abfragesprachen. Cosmos DB-SQL bietet ein formelles Programmiermodell zur Durchführung umfassender Abfragen für JSON-Dokumente.
* Als JSON-Dokumentdatenbank, die JavaScript direkt im Datenbankmodul ausführen kann, wollten wir das JavaScript-Programmiermodell als Fundament für unsere Abfragesprache verwenden. Der SQL-Dialekt von DocumentDB-API verwendet Typsystem, Ausdrucksauswertung und Funktionsaufrufe von JavaScript. Dies wiederum bietet ein natürliches Programmiermodell für relationale Projektionen, eine hierarchische Navigation in JSON-Dokumenten, Selbstverknüpfungen, Abfragen für räumliche Daten, den Aufruf vollständig in JavaScript geschriebener, benutzerdefinierter Funktionen (User Defined Functions, UDFs) und andere Features. 

Diese Funktionen sind unserer Ansicht nach der Schlüssel zur Minimierung der Reibung zwischen Anwendung und Datenbank und sind entscheidend für die Produktivität von Entwicklern.

Für den Einstieg empfiehlt sich folgendes Video, in dem Aravind Ramachandran die Abfragefunktionen von Cosmos DB veranschaulicht. Testen Sie außerdem unseren [Query Playground](http://www.documentdb.com/sql/demo), in dem Sie Cosmos DB ausprobieren und SQL-Abfragen für unser Dataset ausführen können.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

Kehren Sie anschließend zu diesem Artikel zurück. Dort werden im Rahmen eines SQL-Abfragetutorials zunächst einige einfache JSON-Dokumente und SQL-Befehle behandelt.

## <a id="GettingStarted"></a>Erste Schritte mit SQL-Befehlen in Cosmos DB
Um die Cosmos DB-SQL-Sprache in Aktion zu sehen, beginnen wir mit einigen einfachen JSON-Dokumenten und betrachten einige einfache Abfragen für diese Dokumente. Betrachten Sie diese beiden JSON-Dokumente über zwei Familien. In Cosmos DB müssen keine Schemas oder sekundäre Indizes explizit erstellt werden. Wir fügen einfach die JSON-Dokumente in eine Cosmos DB-Sammlung ein und führen anschließend Abfragen aus. Hier haben wir ein einfaches JSON-Dokument für die Familie Andersen mit Eltern, Kindern (und deren Haustieren), Adresse und Registrierungsinformationen. Das Dokument enthält Zeichenfolgen, Zahlen, boolesche Werte, Arrays und verschachtelte Eigenschaften. 

**Dokument**  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Nun folgt ein zweites Dokument mit einem kleinen Unterschied: `givenName` und `familyName` werden anstelle von `firstName` und `lastName` verwendet.

**Dokument**  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Wir werden nun einige Abfragen an die Daten ausführen, um einige der Schlüsselaspekte des SQL-Dialekts der DocumentDB-API besser zu verstehen. Die folgende Abfrage gibt z.B. die Dokumente zurück, in denen das ID-Feld den Text `AndersenFamily` enthält. Da es sich um `SELECT *` handelt, ist die Rückgabe der Abfrage das komplette JSON-Dokument:

**Abfragen**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Betrachten wir nun einen Fall, in dem wir die JSON-Ausgabe nach unseren Wünschen formatieren möchten. Diese Abfrage projiziert ein neues JSON-Objekt mit zwei ausgewählten Feldern (Name und Stadt), wenn die Stadt in der Adresse denselben Namen wie der Staat hat. In diesem Fall stimmt "NY, NY" überein.

**Abfragen**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Ergebnisse**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Die nächste Abfrage gibt alle Vornamen von Kindern der Familie zurück, deren ID `WakefieldFamily` entspricht, und zwar geordnet nach dem Wohnort.

**Abfragen**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Ergebnisse**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Beachten Sie einige der bemerkenswerten Aspekte der Cosmos DB-Abfragesprache, die wir in den bisherigen Beispielen gesehen haben:  

* Da der SQL-Dialekt der DocumentDB-API mit JSON-Werten arbeitet, werden baumförmige Entitäten anstelle von Spalten und Zeilen verarbeitet. Daher können Sie auf Knoten in der Baumstruktur in beliebiger Tiefe verweisen, z. B. `Node1.Node2.Node3…..Nodem`, ähnlich wie relationale SQL mit einem zweiteiligen Verweis auf `<table>.<column>`.   
* Die strukturierte Abfragesprache arbeitet mit schemalosen Daten. Daher muss das Typsystem dynamisch gebunden werden. Derselbe Ausdruck kann unterschiedliche Typen in unterschiedlichen Dokumenten ergeben. Das Ergebnis einer Abfrage ist ein gültiger JSON-Wert, aber nicht garantiert innerhalb eines festen Schemas.  
* Cosmos DB unterstützt nur strikte JSON-Dokumente. Typsystem und Ausdrücke sind also auf JSON-Typen beschränkt. Weitere Informationen finden Sie unter [JSON-Spezifikation](http://www.json.org/) .  
* Eine Cosmos DB-Sammlung ist ein schemaloser Container mit JSON-Dokumenten. Die Beziehungen in Datenentitäten innerhalb und zwischen Dokumenten in einer Sammlung werden implizit durch Einschluss erfasst, und nicht durch Beziehungen von primären Schlüsseln und Fremdschlüsseln. Dieser Aspekt ist wichtig angesichts der später in diesem Artikel besprochenen dokumentinternen Verknüpfungen.

## <a id="Indexing"></a>Cosmos DB-Indizierung
Bevor wir auf die SQL-Syntax der DocumentDB-API eingehen, empfiehlt es sich, dass Sie sich zunächst mit dem Indizierungsdesign in Cosmos DB vertraut machen. 

Datenbankindizes dienen zur Ausführung von Abfragen verschiedenster Arten und Formen mit minimaler Ressourcennutzung (CPU, E/A) sowie mit gutem Durchsatz und niedriger Latenz. Die Auswahl der richtigen Indizes für Datenbankabfragen erfordert oft viel Planungs- und Testaufwand. Dieser Ansatz ist eine Herausforderung für schemalose Datenbanken, in denen die Daten nicht einem strikten Schema folgen und sich laufend verändern. 

Daher haben wir uns für die Entwicklung des Indizierungs-Untersystems von Cosmos DB die folgenden Ziele gesetzt:

* Indizierung von Dokumenten ohne Schema: Das Indizierungsuntersystem benötigt keine Schemainformationen und stellt keinerlei Annahmen über das Schema der Dokumente an. 
* Unterstützung für effiziente, umfassende hierarchische und relationale Abfragen: Der Index unterstützt die Cosmos DB-Abfragesprache auf effiziente Weise und bietet Unterstützung für hierarchische und relationale Projektionen.
* Unterstützung für konsistente Abfragen auch bei steigendem Volumen an Schreibvorgängen: Bei Workloads mit hohem Schreibaufwand und konsistenten Abfragen wird der Index inkrementell, effizient und online aktualisiert, um mit dem hohen Aufkommen an Schreibvorgängen Schritt zu halten. Die konsistente Indexaktualisierung ist entscheidend, um Abfragen auf der Konsistenzebene zu bieten, mit der die Benutzer den Dokumentdienst konfiguriert haben.
* Unterstützung für Mehrinstanzenfähigkeit: Beim reservierungsbasierten Modell für mandantenübergreifende Ressourcenverwaltung werden Indexupdates innerhalb des Budgets der pro Replikat verfügbaren Systemressourcen (CPU, Speicher und E/A-Vorgänge pro Sekunde) durchgeführt. 
* Speichereffizienz: Aus Kostengründen ist der zusätzlich benötigte Speicherplatz für den Index beschränkt und vorhersagbar. Dies ist entscheidend, da Entwickler mit Cosmos DB kostenbasierte Kompromisse zwischen Indexmehraufwand und Abfrageleistung treffen können.  

Beispiele zum Konfigurieren der Indexrichtlinie für Sammlungen finden Sie in den [Azure Cosmos DB-Beispielen](https://github.com/Azure/azure-documentdb-net) auf der MSDN-Website. Wenden wir uns nun den Details der Azure Cosmos DB-SQL-Syntax zu.

## <a id="Basics"></a>Grundlagen von Azure Cosmos DB-SQL-Abfragen
Jede Abfrage besteht aus einer SELECT-Klausel und optionalen FROM- und WHERE-Klauseln nach ANSI-SQL-Standards. Normalerweise wird in jeder Abfrage die jeweilige Quelle in der From-Klausel aufgelistet. Anschließend wird in der WHERE-Klausel ein Filter auf die Quelle angewendet, um eine Teilmenge der JSON-Dokumente zurückzugeben. Zuletzt wird die SELECT-Klausel verwendet, um die abgefragten JSON-Werte in die ausgewählte Liste zu projizieren.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>Die FROM-Klausel
Die `FROM <from_specification>` -Klausel ist optional, es sei denn, die Quelle wird später in der Abfrage gefiltert oder projiziert. Mit dieser Klausel wird die Datenquelle angegeben, auf der die Abfrage operiert. Normalerweise dient die gesamte Sammlung als Quelle, stattdessen kann aber auch eine Teilmenge der Sammlung angegeben werden. 

Abfragen wie `SELECT * FROM Families` geben an, dass die gesamte Families-Sammlung als Quelle dient, die durchlaufen werden soll. Der Sonderbezeichner "ROOT" kann anstelle des Sammlungsnamens verwendet werden, um die Sammlung darzustellen. Die folgende Liste enthält die Regeln, die pro Abfrage erzwungen werden:

* Die Sammlung kann Aliase enthalten, z. B. `SELECT f.id FROM Families AS f` oder einfach `SELECT f.id FROM Families f`. Hier ist `f` das Äquivalent von `Families`. `AS` ist ein optionales Schlüsselwort, das als Alias für den Bezeichner fungiert.
* Nach der Aliasverwendung kann die Originalquelle nicht mehr gebunden werden. `SELECT Families.id FROM Families f` ist beispielsweise syntaktisch ungültig, da der Bezeichner „Families“ nicht mehr aufgelöst werden kann.
* Alle referenzierten Eigenschaften müssen vollqualifiziert sein. Dies ist ohne strikte Schemaverwendung notwendig, um mehrdeutige Bindungen zu vermeiden. Daher ist `SELECT id FROM Families f` syntaktisch ungültig, da die `id`-Eigenschaft nicht gebunden ist.

### <a name="subdocuments"></a>Unterdokumente
Die Quelle kann auch auf eine kleinere Teilmenge reduziert werden. Wenn Sie z.B. nur eine Teilstruktur in jedem Dokument auflisten möchten, kann der Unterstamm der Teilstruktur zur Quelle werden, wie im folgenden Beispiel gezeigt:

**Abfragen**

    SELECT * 
    FROM Families.children

**Ergebnisse**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Während im obigen Beispiel ein Array als Quelle verwendet wurde, kann auch ein Objekt als Quelle verwendet werden. Dies wird im folgenden Beispiel veranschaulicht: Gültige JSON-Werte (nicht undefiniert), die in der Quelle gefunden werden können, werden für die Aufnahme im Ergebnis der Abfrage berücksichtigt. Familien ohne `address.state`-Wert werden aus dem Abfrageergebnis ausgeschlossen.

**Abfragen**

    SELECT * 
    FROM Families.address.state

**Ergebnisse**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>WHERE-Klausel
Die WHERE-Klausel (**`WHERE <filter_condition>`**) ist optional. Sie gibt die Bedingungen an, die die in der Quelle angegebenen JSON-Dokumente erfüllen müssen, um als Teil des Ergebnisses zurückgegeben zu werden. Jedes JSON-Dokument muss die angegebenen Bedingungen erfüllen, um in das Ergebnis einbezogen zu werden. Die WHERE-Klausel wird von der Indexebene verwendet, um die absolut kleinste Teilmenge von Quelldokumenten zu bestimmen, die Teil des Ergebnisses sein können. 

Die folgende Abfrage fordert Dokumente an, die eine „name“-Eigenschaft enthalten, deren Eigenschaftswert gleich `AndersenFamily`ist. Alle anderen Dokumente, die keine „name“-Eigenschaft aufweisen oder deren Wert nicht gleich `AndersenFamily` ist, werden ausgeschlossen. 

**Abfragen**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Das vorstehende Beispiel zeigt eine einfache Gleichheitsabfrage. DocumentDB-API-SQL unterstützt außerdem eine Vielzahl skalarer Ausdrücke. Die am häufigsten verwendeten Ausdrücke sind binäre und unäre Ausdrücke. Eigenschaftsverweise aus dem JSON-Quellobjekt sind ebenfalls gültige Ausdrücke. 

Die folgenden binären Operatoren werden momentan unterstützt und können in Abfragen verwendet werden, wie in den folgenden Beispielen gezeigt:  

<table>
<tr>
<td>Arithmetik</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitweise</td>    
<td>|, &, ^, <<, >>, >>> (Nullauffüllung, Verschiebung nach rechts)</td>
</tr>
<tr>
<td>Logisch</td>
<td>AND, OR, NOT</td>
</tr>
<tr>
<td>Vergleich</td>    
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>String</td>    
<td>|| (Verkettung)</td>
</tr>
</table>  


Betrachten wir nun einige Abfragen mit binären Operatoren.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Die unären Operatoren "+,-, ~ und NOT" werden ebenfalls unterstützt und können in Abfragen verwendet werden, wie im folgenden Beispiel gezeigt:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Neben binären und unären Operatoren sind auch Eigenschaftsverweise zulässig. Beispielsweise gibt `SELECT * FROM Families f WHERE f.isRegistered` das JSON-Dokument zurück, das die `isRegistered`-Eigenschaft enthält und dessen Eigenschaftswert gleich dem JSON-Wert `true` ist. Alle anderen Werte (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>` usw.) führen dazu, dass das Quelldokument aus dem Ergebnis ausgeschlossen wird. 

### <a name="equality-and-comparison-operators"></a>Gleichheits- und Vergleichsoperatoren
Die folgende Tabelle zeigt die Ergebnisse für Gleichheitsvergleiche in DocumentDB-API-SQL zwischen den einzelnen JSON-Typen.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Undefined</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Boolean</strong>
         </td>
         <td valign="top">
            <strong>Number</strong>
         </td>
         <td valign="top">
            <strong>String</strong>
         </td>
         <td valign="top">
            <strong>Object</strong>
         </td>
         <td valign="top">
            <strong>Array</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Undefined<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Boolean<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Number<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>String<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Object<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Undefined </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Array<strong>
         </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
Undefined </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
      </tr>
   </tbody>
</table>

Für andere Vergleichsoperatoren wie >, >=, !=, < und <= gelten die folgenden Regeln:   

* Vergleiche zwischen zwei unterschiedlichen Typen ergeben Undefined.
* Vergleiche zwischen zwei Objekten oder zwei Arrays ergeben Undefined.   

Wenn das Ergebnis eines skalaren Ausdrucks im Filter Undefined ist, wird das entsprechende Dokument aus dem Ergebnis ausgeschlossen, da Undefined logisch nicht gleich "true" ist.

### <a name="between-keyword"></a>Schlüsselwort BETWEEN
Mit BETWEEN können Abfragen für Wertebereiche wie ANSI SQL ausgedrückt werden. BETWEEN kann für Zeichenfolgen oder Zahlen verwendet werden.

Diese Abfrage gibt beispielsweise alle Familiendokumente zurück, in denen das erste Kind die Klassen 1-5 besucht. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Anders als in ANSI-SQL, kann die BETWEEN-Klausel wie im folgenden Beispiel in der FROM-Klausel verwendet werden.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Für schnellere Ausführungszeiten von Abfragen müssen Sie eine Indexrichtlinie erstellen, die einen Bereichindextyp für alle in der BETWEEN-Klausel gefilterten numerischen Eigenschaften/Pfade verwendet. 

Der Hauptunterschied zwischen BETWEEN in DocumentDB-API und ANSI SQL besteht darin, dass Bereichsabfragen für Eigenschaften gemischte Typen enthalten können. "grade" kann beispielsweise in einigen Dokumenten eine Zahl (5) sein, in anderen eine Zeichenfolge ("grade4"). In diesen Fällen gibt, wie in JavaScript, ein Vergleich zwischen zwei unterschiedlichen Typen "undefined" zurück, und das Dokument wird übersprungen.

### <a name="logical-and-or-and-not-operators"></a>Logische Operatoren (AND, OR und NOT)
Logische Operatoren arbeiten mit booleschen Werten. Es folgt eine Liste der logischen Wahrheitstabellen für diese Operatoren.

| OR | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Undefined |
| Undefined |True |Undefined |Undefined |

| AND | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |False |Undefined |
| False |False |False |False |
| Undefined |Undefined |False |Undefined |

| NOT |  |
| --- | --- |
| True |False |
| False |True |
| Undefined |Undefined |

### <a name="in-keyword"></a>IN-Schlüsselwort
Mit dem Schlüsselwort „IN“ kann überprüft werden, ob ein angegebener Wert mit einem Wert in einer Liste übereinstimmt. Bei dieser Abfrage werden beispielsweise alle Familiendokumente zurückgegeben, bei denen die ID entweder „WakefieldFamily“ oder „AndersenFamily“ lautet. 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

In diesem Beispiel werden alle Dokumente zurückgegeben, in denen der Bundesstaat mit einem der angegebenen Werte übereinstimmt.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternäre (?) und koaleszierte (??) Operatoren
Ternäre und koaleszierte Operatoren können ähnlich wie in den gängigen Programmiersprachen wie C# und JavaScript zum Erstellen von bedingten Ausdrücken verwendet werden. 

Der ternäre (?) Operator kann beim schnellen Erstellen von neuen JSON-Eigenschaften sehr nützlich sein. Sie können nun z. B. Abfragen zum Klassifizieren von Klassenstufen in einem für Menschen lesbaren Format wie Anfänger/Fortgeschrittene/Profis wie unten dargestellt erstellen.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Sie können auch die Aufrufe vom Operator wie in der folgenden Abfrage verschachteln.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Wie bei anderen Abfrageoperatoren werden Dokumente mit fehlenden verweisenden Eigenschaften im bedingten Ausdruck oder Dokumente mit unterschiedlichen verglichenen Typen in den Abfrageergebnissen ausgeschlossen.

Der koaleszierte (??) Operator kann zur effizienten Prüfung des Vorkommens einer Eigenschaft in einem Dokument verwendet werden. Damit wird also überprüft, ob die Eigenschaft definiert ist. Dies ist bei Abfragen für teilweise strukturiert oder gemischte Datentypen nützlich. Diese Abfrage gibt z. B. "LastName", falls vorhanden, oder "Surname", falls nicht vorhanden zurück.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Eigenschaftenaccessor in Anführungszeichen
Sie können auf Eigenschaften auch zugreifen, indem Sie den Anführungszeichenoperator `[]`für Eigenschaften verwenden. `SELECT c.grade` and `SELECT c["grade"]` gleichwertig. Diese Syntax ist hilfreich, wenn Sie eine Eigenschaft mit Escapezeichen versehen müssen, die Leerzeichen oder Sonderzeichen enthält oder zufällig den gleichen Namen wie ein SQL-Schlüsselwort oder ein reserviertes Wort hat.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>Die SELECT-Klausel
Die SELECT-Klausel (**`SELECT <select_list>`**) ist obligatorisch und gibt an, welche Werte von der Abfrage zurückgegeben werden, wie auch in ANSI-SQL. Die gefilterte Teilmenge aus den Quelldokumenten wird an die Projektionsphase übergeben, in der die angegebenen JSON-Werte abgerufen werden und ein neues JSON-Objekt für jede übergebene Eingabe erstellt wird. 

Das folgende Beispiel zeigt eine typische SELECT-Abfrage: 

**Abfragen**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Verschachtelte Eigenschaften
Im folgenden Beispiel werden zwei verschachtelte Eigenschaften projiziert: `f.address.state` and `f.address.city` verwendet.

**Abfragen**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Die Projektion unterstützt auch JSON-Ausdrücke, wie im folgenden Beispiel gezeigt wird:

**Abfragen**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Beachten Sie die Rolle von `$1`. Die `SELECT`-Klausel muss ein JSON-Objekt erstellen. Da kein Schlüssel angegeben ist, verwenden wir implizite Argumentvariablennamen beginnend mit `$1`. Diese Abfrage gibt z. B. zwei implizite Argumentvariablen mit den Bezeichnungen `$1` und `$2` zurück.

**Abfragen**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliase
Wir werden das obige Beispiel nun erweitern und explizite Aliase für Werte verwenden. AS ist das Schlüsselwort für die Aliasvergabe. Dieses Schlüsselwort ist optional, wie Sie bei der Projektion des zweiten Werts als `NameInfo` sehen. 

Wenn eine Abfrage zwei Eigenschaften mit demselben Namen hat, müssen Aliase verwendet werden, um mindestens eine der Eigenschaften umzubenennen, sodass diese im projizierten Ergebnis eindeutig sind.

**Abfragen**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalare Ausdrücke
Neben Eigenschaftsverweisen unterstützt die SELECT-Klausel auch skalare Ausdrücke wie Konstanten, arithmetische Ausdrücke, logische Ausdrücke usw. Hier sehen Sie z. B. eine einfache „Hello World“-Abfrage.

**Abfragen**

    SELECT "Hello World"

**Ergebnisse**

    [{
      "$1": "Hello World"
    }]


Hier ist ein komplexeres Beispiel mit einem skalaren Ausdruck:

**Abfragen**

    SELECT ((2 + 11 % 7)-2)/3    

**Ergebnisse**

    [{
      "$1": 1.33333
    }]


Das Ergebnis des skalaren Ausdrucks im folgenden Beispiel ist ein Boolean-Wert.

**Abfragen**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Ergebnisse**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Objekt- und Arrayerstellung
Eine weitere Schlüsselfunktion von DocumentDB-API-SQL ist die Objekt- und Arrayerstellung. Im vorherigen Beispiel haben wir ein neues JSON-Objekt erstellt. Auf dieselbe Weise können Sie Arrays erstellen, wie in den folgenden Beispielen gezeigt:

**Abfragen**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Ergebnisse**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a id="ValueKeyword"></a>VALUE-Schlüsselwort
Das **VALUE**-Schlüsselwort ermöglicht die Rückgabe von JSON-Werten. Die folgende Abfrage gibt z. B. den skalaren Wert `"Hello World"` anstelle von `{$1: "Hello World"}` zurück.

**Abfragen**

    SELECT VALUE "Hello World"

**Ergebnisse**

    [
      "Hello World"
    ]


Die folgende Abfrage gibt den JSON-Wert ohne die `"address"` -Bezeichnung in den Ergebnissen zurück.

**Abfragen**

    SELECT VALUE f.address
    FROM Families f    

**Ergebnisse**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Das folgende Beispiel zeigt, wie Sie primitive JSON-Werte zurückgeben können (auf der Blattebene der JSON-Baumstruktur). 

**Abfragen**

    SELECT VALUE f.address.state
    FROM Families f    

**Ergebnisse**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* Operator
Der Sonderoperator (*) wird unterstützt, um das Dokument unverändert zu projizieren. Wenn dieser Operator verwendet wird, dürfen keine weiteren projizierten Felder existieren. So sind beispielsweise Abfragen wie `SELECT * FROM Families f` gültig, `SELECT VALUE * FROM Families f ` und `SELECT *, f.id FROM Families f ` aber nicht.

**Abfragen**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <a id="TopKeyword"></a>TOP-Operator
Das TOP-Schlüsselwort kann verwendet werden, um die Anzahl der Werte aus einer Abfrage zu beschränken. Wenn TOP in Verbindung mit der ORDER BY-Klausel verwendet wird, ist das Resultset auf die ersten N der geordneten Werte beschränkt. Andernfalls werden die ersten N der Ergebnisse in einer nicht definierten Reihenfolge zurückgegeben. Es hat sich bewährt, in einer SELECT-Anweisung immer eine ORDER BY-Klausel mit der TOP-Klausel zu verwenden. Dies ist die einzige Möglichkeit, zuverlässig anzugeben, welche Zeilen von TOP betroffen sind. 

**Abfragen**

    SELECT TOP 1 * 
    FROM Families f 

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

TOP kann mit einem konstanten Wert (wie oben gezeigt) oder einen Variablenwert mithilfe von parametrisierten Abfragen verwendet werden. Weitere Informationen finden Sie unten zu parametrisierte Abfragen.

### <a id="Aggregates"></a>Aggregatfunktionen
Außerdem können Sie in der `SELECT`-Klausel Aggregationen ausführen. Aggregatfunktionen führen eine Berechnung für eine Gruppe von Werten durch und geben einen einzelnen Wert zurück. Bei dieser Abfrage werden beispielsweise alle Familiendokumente in der Sammlung zurückgegeben.

**Abfragen**

    SELECT COUNT(1) 
    FROM Families f 

**Ergebnisse**

    [{
        "$1": 2
    }]

Sie können auch den skalaren Wert des Aggregats zurückgeben, indem Sie das Schlüsselwort `VALUE` verwenden. Bei dieser Abfrage wird beispielsweise die Anzahl der Werte als einzelne Zahl zurückgegeben:

**Abfragen**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Ergebnisse**

    [ 2 ]

Sie können auch Aggregate in Kombination mit Filtern ausführen. Bei dieser Abfrage wird beispielsweise die Anzahl von Dokumenten mit der Adresse im Bundesstaat Washington zurückgegeben.

**Abfragen**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Ergebnisse**

    [ 1 ]

Die folgende Tabelle zeigt die Liste der unterstützten Aggregatfunktionen in der DocumentDB-API. `SUM`und `AVG` werden über numerische Werte ausgeführt, wohingegen `COUNT`, `MIN` und `MAX` über Zahlen, Zeichenfolgen, boolesche Werte und NULL-Werte ausgeführt werden können. 

| Verwendung | Beschreibung |
|-------|-------------|
| COUNT | Gibt die Anzahl der Elemente im Ausdruck zurück. |
| SUM   | Gibt die Summe aller Werte im Ausdruck zurück. |
| MIN   | Gibt den Mindestwert im Ausdruck zurück. |
| MAX   | Gibt den maximalen Wert im Ausdruck zurück. |
| DURCHSCHN.   | Gibt den Durchschnitt aller Werte im Ausdruck zurück. |

Aggregate können auch über die Ergebnisse einer Array-Iteration ausgeführt werden. Weitere Informationen finden Sie unter [Arrayiteration in Abfragen](#Iteration).

> [!NOTE]
> Beachten Sie bei der Verwendung des Abfrage-Explorers des Azure-Portals, dass Aggregationsabfragen möglicherweise die teilweise aggregierten Ergebnisse über eine Abfrageseite zurückgeben. Die SDKs erzeugen einen einzelnen kumulativen Wert für alle Seiten. 
> 
> Zum Durchführen von Aggregationsabfragen mithilfe von Code benötigen Sie .NET SDK 1.12.0, .NET Core SDK 1.1.0 oder Java SDK 1.9.5 oder höher.    
>

## <a id="OrderByClause"></a>ORDER BY-Klausel
Wie bei ANSI-SQL auch, können Sie beim Abfragen eine optionale Order By-Klausel einbinden. Die Klausel kann ein optionales ASC/DESC-Argument enthalten, um die Reihenfolge anzugeben, in der Ergebnisse abgerufen werden sollen.

Dies ist beispielsweise eine Abfrage, mit der Familien sortiert nach dem Namen des Wohnorts abgerufen werden.

**Abfragen**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Ergebnisse**

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

Und mit dieser Abfrage werden Familien sortiert nach dem Erstellungsdatum abgerufen. Dieser Wert wird als Zahl gespeichert, der die „Epoche“ repräsentiert, also die seit dem 1. Januar 1970 verstrichene Zeit in Sekunden.

**Abfragen**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Ergebnisse**

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <a id="Advanced"></a>Erweiterte Datenbankkonzepte und SQL-Abfragen

### <a id="Iteration"></a>Iteration
Es wurde ein neues Konstrukt mit dem **IN** -Schlüsselwort in DocumentDB-API-SQL eingeführt, mit dem JSON-Arrays durchlaufen werden können. Die FROM-Quelle bietet Unterstützung für Iterationen. Beginnen wir mit dem folgenden Beispiel:

**Abfragen**

    SELECT * 
    FROM Families.children

**Ergebnisse**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Betrachten wir nun eine andere Abfrage, die untergeordnete Elemente in der Sammlung durchläuft. Beachten Sie den Unterschied im Ausgabe-Array. Dieses Beispiel teilt `children` und fügt das Ergebnis in ein einzelnes Array ein.  

**Abfragen**

    SELECT * 
    FROM c IN Families.children

**Ergebnisse**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Mit dieser Funktion können Sie über einzelne Einträge im Array filtern, wie im folgenden Beispiel gezeigt:

**Abfragen**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Ergebnisse**  

    [{
      "givenName": "Lisa"
    }]

Sie können auch eine Aggregation über das Ergebnis der Array-Iteration ausführen. Die folgende Abfrage zählt beispielsweise die Anzahl der Kinder in allen Familien.

**Abfragen**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Ergebnisse**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Joins
Die Möglichkeit, Tabellen zu verknüpfen, ist wichtig in relationalen Datenbanken. Es ist die logische Grundvoraussetzung für die Planung normalisierter Schemas. Im Gegensatz dazu arbeitet die DocumentDB-API mit denormalisierten Datenmodellen schemafreier Dokumente. Dies ist das logische Äquivalent zu "Selbstverknüpfungen".

Die Sprache unterstützt die folgende Syntax: <from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>. Dabei wird ein Satz von **N**-Tupeln (Tupel mit **N**-Werten) zurückgegeben. Jedes Tupel enthält Werte, die durch Iteration aller Sammlungsaliase über deren jeweilige Sätze entstanden sind. In anderen Worten, dies ist ein komplettes Kreuzungsprodukt der an der Verknüpfung beteiligten Sätze.

Die folgenden Beispiele veranschaulichen die Funktionsweise der JOIN-Klausel. Das Ergebnis im folgenden Beispiel ist leer, da das Kreuzprodukt der einzelnen Quelldokumente und einem leeren Satz leer ist.

**Abfragen**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Ergebnisse**  

    [{
    }]


Im folgenden Beispiel findet die Verknüpfung zwischen dem Dokumentstamm und dem Unterstamm `children` statt. Es handelt sich um ein Kreuzprodukt zweier JSON-Objekte. Die Tatsache, dass die untergeordnete Struktur ein Array ist, macht für die Verknüpfung keinen Unterschied, da wir auf einem einzigen Stammknoten arbeiten, der das untergeordnete Array darstellt. Das Ergebnis enthält also nur zwei Ergebnisse, da das Kreuzprodukt der einzelnen Dokumente mit dem Array jeweils genau ein Dokument ergibt.

**Abfragen**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Ergebnisse**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Das folgende Beispiel zeigt eine gewöhnlichere Verknüpfung:

**Abfragen**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Ergebnisse**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Beachten Sie zunächst, dass `from_source` für die **JOIN** -Klausel ein Iterator ist. Der Fluss verläuft in diesem Fall also wie folgt:  

* Alle untergeordneten Elemente **c** im Array erweitern.
* Kreuzprodukt mit dem Stammknoten des Dokuments **f** mit den einzelnen untergeordneten Elementen vom Typ **c** anwenden, die im ersten Schritt vereinfacht wurden.
* Zuletzt wird die Namenseigenschaft des Stammobjekts **f** alleine projiziert. 

Das erste Dokument (`AndersenFamily`) enthält nur ein untergeordnetes Element. Daher enthält der Ergebnissatz für dieses Dokument auch nur ein einzelnes Objekt. Das zweite Dokument (`WakefieldFamily`) enthält zwei untergeordnete Elemente. Daher ergibt das Kreuzprodukt ein separates Objekt für jedes untergeordnete Element und das Ergebnis enthält zwei Objekte, je eines pro untergeordnetem Element im Dokument. Die Stammfelder sind in beiden Dokumenten gleich, wie Sie es bei einem Kreuzprodukt erwarten würden.

Der wahre Nutzen von JOIN ist die Bildung von Tupeln aus dem Kreuzprodukt auf eine Weise, die auf anderen Wegen schwer zu projizieren ist. Außerdem könnten Sie Filter auf die Kombination von Tupeln anwenden, mit denen Benutzer eine Bedingung auswählen können, die von der Gesamtheit der Tupel erfüllt sein muss, wie im folgenden Beispiel gezeigt.

**Abfragen**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Ergebnisse**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Dieses Beispiel ist eine natürliche Erweiterung des vorherigen Beispiels und zeigt eine doppelte Verknüpfung. Das Kreuzprodukt kann daher als eine Form des folgenden Pseudocodes betrachtet werden:

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily` hat ein Kind mit einem Haustier. Das Kreuzprodukt ergibt also eine Zeile (1\*1\*1) für diese Familie. WakefieldFamily hat dagegen zwei Kinder, von denen allerdings nur "Jesse" Haustiere hat. Jesse hat jedoch zwei Haustiere. Das Kreuzprodukt ergibt also 1\*1\*2 = 2 Zeilen für diese Familie.

Das nächste Beispiel verwendet einen zusätzlichen Filter für `pet`. Damit werden alle Tupel ausgeschlossen, bei denen der Haustiername nicht "Shadow" ist. Wir können also Tupel aus Arrays bilden, beliebige Elemente der Tupel filtern und beliebige Kombinationen aus Elementen projizieren. 

**Abfragen**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Ergebnisse**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>JavaScript-Integration
Azure Cosmos DB bietet ein Programmiermodell zur Ausführung JavaScript-basierter Anwendungslogik direkt auf die Sammlungen über gespeicherte Prozeduren und Trigger. Damit ist Folgendes möglich:

* Transaktionale CRUD-Vorgänge und Abfragen für Dokumente in einer Sammlung mit hoher Leistung dank der umfassenden Integration der JavaScript-Laufzeit direkt im Datenbankmodul. 
* Eine natürliche Modellierung von Kontrollfluss, Variablen-Bereichssteuerung, Zuweisung und Integration der Ausnahmebehandlung für Datenbanktransaktionen. Weitere Informationen zur Azure Cosmos DB-Unterstützung für die JavaScript-Integration finden Sie in der Dokumentation für serverseitige JavaScript-Programmierung.

### <a id="UserDefinedFunctions"></a>Benutzerdefinierte Funktionen (User Defined Functions, UDFs)
Neben den bereits in diesem Artikel definierten Typen bietet DocumentDB-API-SQL auch Unterstützung für benutzerdefinierte Funktionen (UDF). Insbesondere skalare UDFs werden unterstützt, bei denen Entwickler null oder mehrere Argumente übergeben und ein einziges Argumentergebnis zurückerhalten können. Für diese Argumente wird geprüft, ob es sich um gültige JSON-Werte handelt.  

Die Syntax von DocumentDB-API-SQL wurde erweitert und unterstützt nun benutzerdefinierte Anwendungslogik mit diesen benutzerdefinierten Funktionen (User-Defined Functions, UDFs). UDFs können bei der DocumentDB-API registriert und anschließend als Teil einer SQL-Abfrage referenziert werden. UDFs dienen in der Tat ausschließlich dazu, in Abfragen aufgerufen zu werden. Als Nebeneffekt haben UDFs im Gegensatz zu den anderen JavaScript-Typen (gespeicherte Prozeduren und Trigger) keinen Zugriff auf das Kontextobjekt. Da Abfragen schreibgeschützt ausgeführt werden, können sie entweder auf primären oder auf sekundären Replikaten ausgeführt werden. Daher werden UDFs normalerweise im Gegensatz zu anderen JavaScript-Typen für die Ausführung auf sekundären Replikaten entwickelt.

Hier sehen Sie ein Beispiel für die Registrierung einer UDF in der Cosmos DB-Datenbank, speziell unterhalb einer Dokumentensammlung.

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

Im vorherigen Beispiel wird eine UDF mit dem Namen `REGEX_MATCH` erstellt. Es werden die beiden JSON-Zeichenfolgenwerte `input` und `pattern` akzeptiert, und es wird überprüft, ob der erste Wert mit dem im zweiten Wert angegebenen Muster übereinstimmt. Hierfür wird die string.match()-JavaScript-Funktion verwendet.

Wir können diese UDF nun in einer Abfrage in einer Projektion verwenden. UDFs müssen mit dem Präfix „udf.“ qualifiziert werden (unter Berücksichtigung der Groß-/Kleinschreibung), wenn sie innerhalb von Abfragen aufgerufen werden. 

> [!NOTE]
> Vor dem 17.03.2015 wurden von Cosmos DB UDF-Aufrufe ohne das Präfix „udf.“ unterstützt. Beispiel: SELECT REGEX_MATCH(). Dieses Aufrufmuster ist veraltet.  
> 
> 

**Abfragen**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Ergebnisse**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

Die UDF kann auch in einem Filter verwendet werden, wie im folgenden Beispiel gezeigt (ebenfalls mit dem Präfix „udf.“ qualifiziert):

**Abfragen**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


UDFs sind gültige skalare Ausdrücke und können sowohl in Projektionen als auch in Filtern verwendet werden. 

Wir erweitern den Funktionsumfang von UDFs im folgenden Beispiel um konditionale Logik:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


Hier sehen Sie ein Verwendungsbeispiel für die UDF.

**Abfragen**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

**Ergebnisse**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Wie im vorherigen Beispiel gezeigt, vereinen UDFs den Funktionsumfang der JavaScript-Sprache mit DocumentDB-API-SQL zu einer umfassenden, programmierbaren Schnittstelle für komplexe verfahrensorientierte und konditionale Logik mithilfe der Fähigkeiten der integrierten JavaScript-Laufzeit.

DocumentDB-API-SQL übergibt die Argumente an die UDFs für die einzelnen Quelldokumente für die aktuelle Phase (WHERE-Klausel oder SELECT-Klausel) der UDF-Ausführung. Das Ergebnis wird nahtlos in die Gesamt-Ausführungspipeline aufgenommen. Wenn die von den UDF-Parametern referenzierten Eigenschaften nicht im JSON-Wert verfügbar sind, wird der Parameter als "Undefined" betrachtet und der UDF-Aufruf daher komplett übersprungen. Wenn das Ergebnis der UDF "Undefined" ist, wird es ebenfalls nicht in das Ergebnis aufgenommen. 

UDFs sind also hervorragende Werkzeuge, um komplexe Geschäftslogik als Teil der Abfragen zu integrieren.

### <a name="operator-evaluation"></a>Operatorauswertung
Cosmos DB ist eine JSON-Datenbank und enthält daher Parallelen zu JavaScript-Operatoren und deren Auswertungslogik. Obwohl Cosmos DB versucht, die JavaScript-Logik für die JSON-Unterstützung zu erhalten, weicht die Auswertung von Operationen doch in manchen Fällen davon ab.

In DocumentDB-API-SQL ist der Typ eines Werts im Gegensatz zu herkömmlichem SQL oft nicht bekannt, bevor der Wert aus der Datenbank abgerufen wird. Um effiziente Abfragen zu ermöglichen, haben die meisten Operatoren strikte Typanforderungen. 

DocumentDB-API-SQL führt im Gegensatz zu JavaScript keine impliziten Konvertierungen durch. Eine Abfrage wie `SELECT * FROM Person p WHERE p.Age = 21` stimmt beispielsweise mit Dokumenten überein, die eine Age-Eigenschaft mit dem Wert 21 enthalten. Alle weiteren Dokumente, deren Age-Eigenschaft mit der Zeichenfolge "21" übereinstimmt, oder sonstige Variationen wie "021", "21,0", "0021", "00021" usw. werden nicht zurückgegeben. Bei JavaScript würden diese Zeichenfolgenwerte dagegen implizit in Zahlen umgewandelt (je nach Operator, z. B.: ==). Dieser Unterschied ist wichtig für die effiziente Indexierung in DocumentDB-API-SQL. 

## <a name="parameterized-sql-queries"></a>Parametrisierte SQL-Abfragen
Cosmos DB unterstützt Abfragen mit Parametern, die mit der bekannten @-Notation ausgedrückt werden. Parametrisiertes SQL bietet stabile Fehlerbehandlung und Schutz von Benutzereingaben, wodurch eine versehentliche Offenlegung von Daten durch SQL-Injektion verhindert wird. 

Sie können z. B. eine Abfrage erstellen, die "last name" und "adress state" als Parameter verwendet, und sie dann für unterschiedliche "last name"- und "address state"-Werte auf Grundlage von Benutzereingaben ausführen.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Diese Anforderung kann dann an Cosmos DB als parametrisierte JSON-Abfrage wie im Folgenden dargestellt gesendet werden.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Das Argument für TOP kann, wie unten gezeigt, mit parametrisierte Abfragen festgelegt werden.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwerte können alle gültigen JSON-Werte sein (Zeichenfolgen, Zahlen, boolesche Werte, Null, Arrays oder verschachteltes JSON). Da Cosmos DB über weniger Schemata verfügt, werden Parameter nicht für jeden Typ überprüft.

## <a id="BuiltinFunctions"></a>Integrierte Funktionen
Cosmos DB unterstützt auch eine Reihe von integrierten Funktionen für häufige Vorgänge, die wie benutzerdefinierte Funktionen (User-Defined Functions, UDFs) in Abfragen verwendet werden können.

| Funktionsgruppe          | Vorgänge                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Mathematische Funktionen  | ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN und TAN |
| Funktionen für die Typprüfung | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED und IS_PRIMITIVE                                                           |
| Zeichenfolgenfunktionen        | CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING und UPPER       |
| Arrayfunktionen         | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH und ARRAY_SLICE                                                                                         |
| Räumliche Funktionen       | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID und ST_ISVALIDDETAILED                                                                           | 

Wenn Sie derzeit eine benutzerdefinierte Funktion (UDF) verwenden, für die jetzt eine entsprechende integrierte Funktion verfügbar ist, sollten Sie diese integrierte Funktion verwenden, da sie schneller und effizienter ausgeführt werden kann. 

### <a name="mathematical-functions"></a>Mathematische Funktionen
Jede mathematische Funktion führt eine Berechnung durch, basierend auf Eingabewerten, die als Argument bereitgestellt werden, und gibt einen numerischen Wert zurück. Im Folgenden finden Sie eine Tabelle der unterstützten integrierten mathematischen Funktionen.


| Verwendung | Beschreibung |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Gibt den absoluten (positiven) Wert des angegebenen numerischen Ausdrucks zurück. |
| [CEILING (num_expr)](#bk_ceiling) | Gibt den kleinsten ganzzahligen Wert zurück, der größer oder gleich dem angegebenen numerischen Ausdruck ist. |
| [FLOOR (num_expr)](#bk_floor) | Gibt den größten ganzzahligen Wert zurück, der kleiner oder gleich dem angegebenen numerischen Ausdruck ist. |
| [EXP (num_expr)](#bk_exp) | Gibt den Exponenten des angegebenen numerischen Ausdrucks zurück. |
| [LOG (num_expr [,base])](#bk_log) | Gibt den natürlichen Logarithmus des angegebenen numerischen Ausdrucks oder den Logarithmus basierend auf der angegebenen Basis zurück. |
| [LOG10 (num_expr)](#bk_log10) | Gibt den dekadischen Logarithmuswert des angegebenen numerischen Ausdrucks zurück. |
| [ROUND (num_expr)](#bk_round) | Gibt einen numerischen Wert zurück, gerundet auf den nächsten ganzzahligen Wert. |
| [TRUNC (num_expr)](#bk_trunc) | Gibt einen numerischen Wert zurück, gekürzt auf den nächsten ganzzahligen Wert. |
| [SQRT (num_expr)](#bk_sqrt) | Gibt die Quadratwurzel des angegebenen numerischen Ausdrucks zurück. |
| [SQUARE (num_expr)](#bk_square) | Gibt das Quadrat des angegebenen numerischen Ausdrucks zurück. |
| [POWER (num_expr, num_expr)](#bk_power) | Gibt die Potenz des angegebenen numerischen Ausdrucks über den angegebenen Wert zurück. |
| [SIGN (num_expr)](#bk_sign) | Gibt den Vorzeichenwert (-1, 0, 1) des angegebenen numerischen Ausdrucks zurück. |
| [ACOS (num_expr)](#bk_acos) | Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Kosinus der angegebene numerische Ausdruck ist. Wird auch als Arkuskosinus bezeichnet. |
| [ASIN (num_expr)](#bk_asin) | Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Sinus der angegebene numerische Ausdruck ist. Wird auch als Arkussinus bezeichnet. |
| [ATAN (num_expr)](#bk_atan) | Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Tangens der angegebene numerische Ausdruck ist. Wird auch als Arkustangens bezeichnet. |
| [ATN2 (num_expr)](#bk_atn2) | Gibt den Winkel in „rad“ (Bogenmaß) zwischen der positiven X-Achse und dem Strahl vom Ursprung zum Punkt (y, x) zurück, wobei x und y die Werte der beiden angegebenen float-Ausdrücke sind. |
| [COS (num_expr)](#bk_cos) | Gibt den trigonometrischen Kosinus des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen Ausdruck zurück. |
| [COT (num_expr)](#bk_cot) | Gibt den trigonometrischen Kotangens des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen numerischen Ausdruck zurück. |
| [DEGREES (num_expr)](#bk_degrees) | Gibt den entsprechenden Winkel in Grad für einen in „rad“ (Bogenmaß) angegebenen Winkel zurück. |
| [PI ()](#bk_pi) | Gibt den konstanten Wert von PI zurück. |
| [RADIANS (num_expr)](#bk_radians) | Gibt das Bogenmaß zurück, wenn ein numerischer Ausdruck in Grad eingegeben wird. |
| [SIN (num_expr)](#bk_sin) | Gibt den trigonometrischen Sinus des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen Ausdruck zurück. |
| [TAN (num_expr)](#bk_tan) | Gibt den Tangens des Eingabeausdrucks im angegebenen Ausdruck zurück. |

Sie können nun beispielsweise Abfragen wie die folgende ausführen:

**Abfragen**

    SELECT VALUE ABS(-4)

**Ergebnisse**

    [4]

Der Hauptunterschied zwischen den Cosmos DB-Funktionen und ANSI SQL besteht darin, dass die Funktionen sich gut für Daten ohne und mit gemischtem Schema einsetzen lassen. Wenn Sie beispielsweise ein Dokument haben, in dem die "Size"-Eigenschaft fehlt oder einen nicht numerischen Wert wie "unbekannt" aufweist, wird dieses Dokument übersprungen anstatt einen Fehler zurückzugeben.

### <a name="type-checking-functions"></a>Funktionen für die Typprüfung
Mit den Funktionen für die Typprüfung können Sie den Typ eines Ausdrucks in SQL-Abfragen prüfen. Typprüfungsfunktionen können verwendet werden, um den Typ von Eigenschaften innerhalb von Dokumenten während der Übertragung zu ermitteln, wenn dieser variabel oder unbekannt ist. Im Folgenden finden Sie eine Tabelle der unterstützten integrierten Typprüfungsfunktionen.

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der Wert vom Typ "Array" ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der Wert vom Typ "boolesch" ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der Wert vom Typ "Null" ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der Wert vom Typ "Zahl" ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der Wert vom Typ "JSON-Objekt" ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der Wert vom Typ "Zeichenfolge" ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, um anzugeben, ob der Eigenschaft ein Wert zugewiesen wurde.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></td>
  <td>Gibt einen booleschen Wert zurück, um anzugeben, ob der Typ des Werts eine Zeichenfolge, Zahl, ein boolescher Wert oder Null ist.</td>
</tr>

</table>

Mithilfe dieser Funktionen können Sie nun Abfragen wie die folgende ausführen:

**Abfragen**

    SELECT VALUE IS_NUMBER(-4)

**Ergebnisse**

    [true]

### <a name="string-functions"></a>Zeichenfolgenfunktionen
Die folgenden Skalarfunktionen führen einen Vorgang für einen Zeichenfolgen-Eingabewert durch und geben eine Zeichenfolge, einen numerischen Wert oder einen booleschen Wert zurück. Diese Tabelle enthält integrierte Zeichenfolgenfunktionen:

| Verwendung | Beschreibung |
| --- | --- |
| [LENGTH (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Gibt die Anzahl der Zeichen im angegebenen Zeichenfolgenausdruck zurück. |
| [CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |Gibt eine Zeichenfolge zurück, die das Ergebnis der Verkettung von zwei oder mehr Zeichenfolgenwerten darstellt. |
| [SUBSTRING (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Gibt einen Teil eines Zeichenfolgenausdrucks zurück. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt. |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt. |
| [CONTAINS (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck den zweiten enthält. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |Gibt die Anfangsposition des ersten Vorkommens des zweiten Zeichenfolgenausdrucks innerhalb des ersten angegebenen Zeichenfolgenausdrucks zurück, oder -1, wenn die Zeichenfolge nicht gefunden wird. |
| [LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Gibt den linken Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen zurück. |
| [RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Gibt den rechten Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen zurück. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Gibt einen Zeichenfolgenausdruck zurück, nachdem vorangestellte Leerzeichen entfernt wurden. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Gibt einen Zeichenfolgenausdruck zurück, nachdem alle nachgestellten Leerzeichen entfernt wurden. |
| [LOWER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Gibt eine Zeichenfolge zurück, nachdem Großbuchstaben in Kleinbuchstaben konvertiert wurden. |
| [UPPER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Gibt eine Zeichenfolge zurück, nachdem Kleinbuchstaben in Großbuchstaben konvertiert wurden. |
| [REPLACE (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Ersetzt alle Vorkommen eines angegebenen Zeichenfolgenwerts durch einen anderen Zeichenfolgenwert. |
| [REPLICATE (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |Wiederholt einen Zeichenfolgenwert mit der angegebenen Häufigkeit. |
| [REVERSE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Gibt einen Zeichenfolgenwert in umgekehrter Reihenfolge zurück. |

Mithilfe dieser Funktionen können Sie nun Abfragen wie die folgende ausführen: Beispielsweise können Sie den Familiennamen wie folgt in Großbuchstaben zurückgeben:

**Abfragen**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Ergebnisse**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Oder Zeichenfolgen wie in diesem Beispiel verketten:

**Abfragen**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Zeichenfolgenfunktionen können auch in der WHERE-Klausel zum Filtern von Ergebnissen verwendet werden, wie im folgenden Beispiel:

**Abfragen**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Arrayfunktionen
Die folgenden Skalarfunktionen führen einen Vorgang für einen Arrayeingabewert durch und geben einen numerischen Wert, booleschen Wert oder Arraywert zurück. Diese Tabelle enthält integrierte Arrayfunktionen:

| Verwendung | Beschreibung |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Gibt die Anzahl der Elemente des angegebenen Arrayausdrucks zurück. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |Gibt ein Array zurück, das das Ergebnis der Verkettung von zwei oder mehr Arraywerten darstellt. |
| [ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Gibt einen booleschen Wert zurück, um anzugeben, ob das Array den angegebenen Wert enthält. Kann angeben, ob es sich um eine vollständige oder teilweise Übereinstimmung handelt. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Gibt einen Teil eines Arrayausdrucks zurück. |

Arrayfunktionen können verwendet werden, um Arrays in JSON zu bearbeiten. Mit dieser Abfrage werden beispielsweise alle Dokumente zurückgegeben, in denen ein Elternteil „Robin Wakefield“ ist. 

**Abfragen**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Sie können ein Teilfragment für übereinstimmende Elemente innerhalb des Arrays angeben. Die folgende Abfrage sucht alle übergeordneten Elemente mit `givenName` von `Robin`.

**Abfragen**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]


Dies ist ein weiteres Beispiel für die Verwendung von ARRAY_LENGTH. Hierbei wird die Anzahl der Kinder pro Familie abgerufen.

**Abfragen**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Räumliche Funktionen
Cosmos DB unterstützt die folgenden integrierten OGC-Funktionen (Open Geospatial Consortium ) für das Abfragen von Geodaten. 

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Gibt den Abstand zwischen den beiden GeoJSON-Punkt-, Polygon- oder LineString-Ausdrücken zurück.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Gibt einen booleschen Ausdruck zurück, der angibt, ob das erste GeoJSON-Objekt (Punkt, Polygon oder LineString) innerhalb des zweiten GeoJSON-Objekts (Punkt, Polygon oder LineString) enthalten ist.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Gibt einen booleschen Ausdruck, der angibt, ob die beiden angegebenen GeoJSON-Objekte (Punkt, Polygon oder LineString) sich überschneiden.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Gibt einen booleschen Wert zurück, der angibt, ob der/das angegebene GeoJSON-Punkt, -Polygon, oder -LineString gültig ist.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Gibt einen JSON-Wert mit einem booleschen Wert zurück, wenn der angegebene GeoJSON-Punkt-, -Polygon- oder -LineString-Ausdruck gültig ist. Falls ungültig, wird außerdem der Grund als Zeichenfolge zurückgegeben.</td>
</tr>
</table>

Räumliche Funktionen können verwendet werden, um Entfernungsabfragen auf räumliche Daten anzuwenden. Hier ist z. B. eine Abfrage, die alle Familiendokumente zurückgibt, die sich innerhalb von 30 km von der angegebenen Position befinden. Dazu wird die integrierte ST_DISTANCE-Funktion verwendet. 

**Abfragen**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Weitere Informationen zur Unterstützung von Geodaten in Cosmos DB finden Sie unter [Arbeiten mit Geodaten in Azure Cosmos DB](geospatial.md). Dies sind alle Informationen zu den räumlichen Funktionen und zur SQL-Syntax für Cosmos DB. Als Nächstes sehen wir uns an, wie LINQ-Abfragen funktionieren und wie sie mit der bereits behandelten Syntax interagieren.

## <a id="Linq"></a>LINQ zu DocumentDB-API-SQL
LINQ ist ein .NET-Programmiermodell, das Berechnungen als Abfragen auf Streams von Objekten darstellt. Cosmos DB bietet eine clientseitige Bibliothek als Schnittstelle zu LINQ und vereinfacht die Konvertierung zwischen JSON- und .NET-Objekten sowie die Zuordnung einer Teilmenge von LINQ-Abfragen zu Cosmos DB-Abfragen. 

Die folgende Abbildung zeigt die Architektur für die Unterstützung von LINQ-Abfragen in Cosmos DB.  Mit dem Cosmos DB-Client können Entwickler ein **IQueryable** -Objekt erstellen, das den Cosmos DB-Abfrageanbieter direkt abfragt, der die LINQ-Abfrage wiederum in eine Cosmos DB-Abfrage übersetzt. Anschließend wird die Abfrage an den Cosmos DB-Server übergeben, um einen Ergebnissatz im JSON-Format abzurufen. Die zurückgegebenen Ergebnisse werden clientseitig in einen Stream von .NET-Objekten deserialisiert.

![Architektur für die Unterstützung von LINQ-Abfragen mit DocumentDB-API: SQL-Syntax, JSON-Abfragesprache, Datenbankkonzepte und SQL-Abfragen][1]

### <a name="net-and-json-mapping"></a>.NET- und JSON-Zuordnung
Die Zuordnung zwischen .NET- und JSON-Objekten erfolgt auf natürliche Weise. Jedes Datenelementfeld wird einem JSON-Objekt zugeordnet. Dabei wird der Feldname in den "key"-Teil des Objekts übernommen, und der "value"-Teil wird rekursiv zur Wertkomponente des Objekts zugeordnet. Beachten Sie das folgende Beispiel: Das erstellte „Family“-Objekt wird wie hier gezeigt zum JSON-Dokument zugeordnet. Umgekehrt wird das JSON-Dokument zurück zu einem .NET-Objekt zugeordnet.

**C#-Klasse**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ zu SQL-Übersetzung
Der Cosmos DB-Abfrageanbieter führt eine bestmögliche Zuordnung von LINQ-Abfragen in Cosmos DB-SQL-Abfragen durch. Die folgende Beschreibung geht davon aus, das Sie mit den Grundsätzen von LINQ vertraut sind.

Für das Typsystem werden alle primitiven JSON-Typen unterstützt: numerische Typen, boolesche Typen, String und null. Andere JSON-Typen werden nicht unterstützt. Die folgenden skalaren Ausdrücke werden unterstützt.

* Konstante Werte – dazu gehören konstante Werte der primitiven Datentypen zum Zeitpunkt der Ausführung der Abfrage.
* Indexausdrücke auf Eigenschaften/Arrays – diese Ausdrücke beziehen sich auf die Eigenschaft eines Objekts oder eines Arrayelements.
  
     family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n ist eine INT-Variable
* Arithmetische Ausdrücke - dazu gehören arithmetische Ausdrücke auf numerische und boolesche Werte. Eine vollständige Liste finden Sie in der SQL-Dokumentation.
  
     2 * family.children[0].grade;    x + y;
* Zeichenfolgenvergleiche - dazu gehören Vergleiche von Zeichenfolgenwerten mit konstanten Zeichenfolgen.  
  
     mother.familyName == "Smith";    child.givenName == s; //s ist eine Zeichenfolgenvariable
* Erstellungsausdrücke für Objekte/Arrays - diese Ausdrücke geben ein Objekt mit einem Verbundwerttyp oder anonymen Typ bzw. ein Array solcher Objekte zurück. Diese Werte können verschachtelt sein.
  
     new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //ein anonymer Typ mit zwei Feldern              
     new int[] { 3, child.grade, 5 };

### <a id="SupportedLinqOperators"></a>Liste der unterstützten LINQ-Operatoren
Es folgt eine Liste der unterstützten LINQ-Operatoren im LINQ-Anbieter, die im DocumentDB .NET SDK enthalten sind.

* **Select**: Projektionen einschließlich Objektkonstruktion werden in SQL SELECT übersetzt.
* **Where**: Filter werden in SQL WHERE übersetzt und unterstützen die Übersetzung von &&, || und ! in die SQL-Operatoren
* **SelectMany**: Ermöglicht das Entladen von Arrays in die SQL-JOIN-Klausel. Kann zum Verketten/Verschachteln von Ausdrücken in Arrayelemente verwendet werden.
* **OrderBy und OrderByDescending**: Auf-/absteigende Übersetzung in ORDER BY
* Die Operatoren **Count**, **Sum**, **Min**, **Max** und **Average** für die Aggregation und deren asynchrone Entsprechungen **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync** und **AverageAsync**.
* **CompareTo**: Übersetzung in Bereichsvergleiche. Wird häufig für Zeichenfolgen verwendet werden, da sie nicht in .NET vergleichbar sind.
* **Take**: Übersetzung in SQL TOP, um Ergebnisse aus einer Abfrage einzuschränken.
* **Mathematische Funktionen**: Unterstützt die Übersetzung von „Abs“, „Acos“, „Asin“, „Atan“, „Ceiling“, „Cos“, „Exp“, „Floor“, „Log“, „Log10“, „Pow“, „Round“, „Sign“, „Sin“, „Sqrt“, „Tan“, „Truncate“ aus .NET in die entsprechenden SQL-integrierten Funktionen.
* **Zeichenfolgenfunktionen**: Unterstützt die Übersetzung von „Concat“, „Contains“, „EndsWith“, „IndexOf“, „Count“, „ToLower“, „TrimStart“, „Replace“, „Reverse“, „TrimEnd“, „StartsWith“, „SubString“, „ToUpper“ aus .NET in die entsprechenden SQL-integrierten Funktionen.
* **Arrayfunktionen**: Unterstützt die Übersetzung von „Concat“, „Contains“ und „Count“ aus .NET in die entsprechenden SQL-integrierten Funktionen.
* **Geospatial-Erweiterungsfunktionen**: Unterstützt die Übersetzung aus den Stubmethoden „Distance“, „Within“, „IsValid“ und „IsValidDetailed“ in die entsprechenden SQL-integrierten Funktionen.
* **Erweiterungsfunktion für benutzerdefinierte Funktion**: Unterstützt die Übersetzung aus der Stubmethode „UserDefinedFunctionProvider.Invoke“ in die entsprechende benutzerdefinierte Funktion.
* **Sonstiges**: Unterstützt die Übersetzung der Zusammenfügungs- und bedingten Operatoren. Kann „Contains“ je nach Kontext in die Zeichenfolge CONTAINS, ARRAY_CONTAINS oder SQL IN übersetzen.

### <a name="sql-query-operators"></a>SQL-Abfrageoperatoren
Die folgenden Beispiele zeigen, wie einige der Standard-LINQ-Abfrageoperatoren in Cosmos DB-Abfragen übersetzt werden.

#### <a name="select-operator"></a>Select-Operator
Die Syntax ist `input.Select(x => f(x))`, wobei `f` ein skalarer Ausdruck ist.

**LINQ Lambda-Ausdruck**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ Lambda-Ausdruck**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ Lambda-Ausdruck**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany-Operator
Die Syntax ist `input.SelectMany(x => f(x))`, wobei `f` ein skalarer Ausdruck ist, der einen Sammlungstyp zurückgibt.

**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Where-Operator
Die Syntax ist `input.Where(x => f(x))`, wobei `f` ein skalarer Ausdruck ist, der einen booleschen Wert zurückgibt.

**LINQ Lambda-Ausdruck**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ Lambda-Ausdruck**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Zusammengesetzte SQL-Abfragen
Die obigen Operatoren können zu komplexeren Abfragen zusammengesetzt werden. Da Cosmos DB verschachtelte Sammlungen unterstützt, kann die Zusammensetzung entweder verkettet oder verschachtelt werden.

#### <a name="concatenation"></a>Verkettung
Die Syntax ist `input(.|.SelectMany())(.Select()|.Where())*`. Eine verkettete Abfrage kann mit einer optionalen `SelectMany`-Abfrage beginnen, gefolgt von mehreren `Select`- oder `Where`-Operatoren.

**LINQ Lambda-Ausdruck**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ Lambda-Ausdruck**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ Lambda-Ausdruck**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Verschachtelung
Die Syntax ist `input.SelectMany(x=>x.Q())`, wobei „Q“ ein `Select`-, `SelectMany`- oder `Where`-Operator ist.

In verschachtelten Abfragen wird die innere Abfrage auf jedes Element der äußeren Abfrage angewendet. Hierbei ist wichtig, dass die innere Abfrage auf die Felder der Elemente in der äußeren Abfrage wie bei Selbstverknüpfungen verweisen kann.

**LINQ Lambda-Ausdruck**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ Lambda-Ausdruck**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a id="ExecutingSqlQueries"></a>Ausführen von SQL-Abfragen
Cosmos DB stellt Ressourcen über eine REST-API zur Verfügung, die in jeder Sprache aufgerufen werden kann, die HTTP/HTTPS-Anfragen unterstützt. Zusätzlich bietet Cosmos DB Programmierbibliotheken für einige beliebte Sprachen an, wie .NET, Node.js, JavaScript und Python. Die REST-API und die verschiedenen Bibliotheken unterstützen allesamt SQL-Abfragen. Das .NET SDK unterstützt LINQ-Abfragen zusätzlich zu SQL.

Die folgenden Beispiele zeigen, wie Sie eine Abfrage erstellen und auf einem Cosmos DB-Datenbankkonto ausführen können.

### <a id="RestAPI"></a>REST-API
Cosmos DB bietet ein RESTful-Programmiermodell über HTTP. Datenbankkonten können im Rahmen eines Azure-Abonnements erstellt werden. Das Ressourcenmodell von Cosmos DB besteht aus einem Satz von Ressourcen in einem Datenbankkonto, die jeweils über einen logischen und beständigen URI adressiert werden können. Ein Ressourcensatz wird in diesem Dokument als Feed bezeichnet. Ein Datenbankkonto besteht aus einem Satz von Datenbanken, die je mehrere Sammlungen enthalten, die wiederum Dokumente, UDFs und andere Ressourcentypen enthalten.

Die Interaktion mit diesen Ressourcen erfolgt über die HTTP-Verben GET, PUT, POST und DELETE mit deren Standardinterpretation. Das POST-Verb dient zur Erstellung neuer Ressourcen, zur Ausführung gespeicherter Prozeduren oder zum Ausführen von Cosmos DB-Abfragen. Abfragen werden immer als schreibgeschützte Operationen ohne Nebeneffekte ausgeführt.

Die folgenden Beispiele zeigen POST für eine API-Abfrage der DocumentDB-API an eine Sammlung, die die beiden bisher behandelten Beispieldokumente enthält. Die Abfrage enthält einen einfachen Filter auf die JSON-Eigenschaft name. Beachten Sie die Verwendung der `x-ms-documentdb-isquery`- und Content-Type: `application/query+json`-Header, um anzugeben, dass es sich bei diesem Vorgang um eine Abfrage handelt.

**Anforderung**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


**Ergebnisse**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Das zweite Beispiel zeigt eine komplexere Abfrage, die mehrere Ergebnisse der Verknüpfung zurückgibt.

**Anforderung**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Ergebnisse**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Wenn die Abfrageergebnisse nicht auf eine einzelne Ergebnisseite passen, gibt die REST-API ein Fortsetzungstoken im `x-ms-continuation-token` -Antwortheader zurück. Clients können einzelne Ergebnisseiten abfragen, indem sie den Header in nachfolgenden Abfragen angeben. Die Anzahl der Ergebnisse pro Seite kann über den `x-ms-max-item-count` -number-Header gesteuert werden. Wenn die angegebene Abfrage eine Aggregatfunktion wie `COUNT` hat, gibt die Abfrageseite möglicherweise einen teilweise aggregierten Wert über die Seite mit Ergebnissen zurück. Die Clients müssen eine Aggregation auf zweiter Ebene über diese Ergebnisse ausführen, um die Endergebnisse zu erzeugen, z.B. die Summe über die auf den einzelnen Seiten zurückgegebenen Anzahlen, um die Gesamtanzahl zurückzugeben.

Für die Verwaltung der Datenkonsistenzrichtlinie für Abfragen verwenden Sie den `x-ms-consistency-level` -Header, wie in allen REST-API-Anfragen. Für die Sitzungskonsistenz muss außerdem der letzte `x-ms-session-token` -Cookie-Header in der Abfrageanforderung wiederholt werden. Die Indizierungsrichtlinie der abgefragten Sammlung kann auch die Konsistenz der Abfrageergebnisse beeinflussen. Mit den Standardeinstellungen für Indizierungsrichtlinien stimmt der Index einer Sammlung immer mit dem aktuellen Dokumentinhalt überein, und die Abfrageergebnisse liefern die Daten in der gewünschten Konsistenz zurück. Wenn eine verzögerte Indexierungsrichtlinie verwendet wird, kann es passieren, dass veraltete Ergebnisse zurückgegeben werden. Weitere Informationen finden Sie unter [Konsistenzebenen in Azure Cosmos DB][consistency-levels].

Wenn die konfigurierte Indizierungsrichtlinie der Sammlung die angegebene Abfrage nicht unterstützen kann, gibt der Azure Cosmos DB-Server "400 Bad Request" zurück. Dies wird z. B. für Bereichsabfragen auf Pfade zurückgegeben, die für Hash (Gleichheits)-Suchvorgänge konfiguriert sind, oder für Pfade, die ausdrücklich von der Indexierung ausgeschlossen wurden. Mit dem `x-ms-documentdb-query-enable-scan`-Header kann angegeben werden, dass eine Abfrage einen Scan durchführen darf, wenn kein Index verfügbar ist.

Sie können ausführliche Metriken zur Abfrageausführung abrufen, indem Sie den `x-ms-documentdb-populatequerymetrics`-Header auf `True` festlegen. Weitere Informationen finden Sie unter [SQL-Abfragemetriken für die Azure Cosmos DB-DocumentDB-API](documentdb-sql-query-metrics.md).

### <a id="DotNetSdk"></a>C# (.NET) SDK
Das .NET SDK unterstützt Abfragen per LINQ und SQL. Das folgende Beispiel zeigt, wie Sie die weiter oben in diesem Dokument gezeigte einfache Filterabfrage ausführen können.

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Das Beispiel vergleicht zwei Eigenschaften auf Gleichheit in den einzelnen Dokumenten und verwendet anonyme Projektionen. 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Das nächste Beispiel zeigt Verknüpfungen mithilfe von LINQ-SelectMany.

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Der .NET-Client durchläuft automatisch alle Seiten der Abfrageergebnisse in den foreach-Blöcken, wie oben gezeigt. Die im Abschnitt zur REST-API vorgestellten Abfrageoptionen sind auch im .NET SDK über die Klassen `FeedOptions` and `FeedResponse` in der CreateDocumentQuery-Methode verfügbar. Die Anzahl der Ergebnisse pro Seite kann über die `MaxItemCount` -Einstellung gesteuert werden. 

Sie können die Seitenaufteilung steuern, indem Sie mithilfe des `IQueryable`-Objekts ein `IDocumentQueryable`-Element erstellen, die ` ResponseContinuationToken`-Werte lesen und sie anschließend als `RequestContinuationToken` in `FeedOptions` zurückgeben. `EnableScanInQuery` kann festgelegt werden, um Suchvorgänge zu ermöglichen, wenn die Abfrage nicht von der konfigurierten Indizierungsrichtlinie unterstützt werden kann. Für partitionierte Sammlungen können Sie `PartitionKey` verwenden, um die Abfrage für eine einzelne Partition auszuführen (auch wenn Cosmos DB dies automatisch aus dem Abfragetext extrahieren kann). Mit `EnableCrossPartitionQuery` können Sie Abfragen ausführen, die unter Umständen für mehrere Partitionen ausgeführt werden müssen. 

Weitere Beispiele, die Abfragen enthalten, finden Sie unter [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-documentdb-net) . 

### <a id="JavaScriptServerSideApi"></a>Serverseitige JavaScript-API
Cosmos DB bietet ein Programmiermodell zur Ausführung JavaScript-basierter Anwendungslogik direkt auf die Sammlungen über gespeicherte Prozeduren und Trigger. Die auf Sammlungsebene registrierte JavaScript-Logik kann anschließend Datenbankoperationen auf die Dokumente der jeweiligen Sammlung ausführen. Diese Operationen werden in ACID-Transaktionen der jeweiligen Umgebung gekapselt.

Das folgende Beispiel zeigt, wie Sie mithilfe von queryDocuments in der serverseitigen JavaScript-API Abfragen aus gespeicherten Prozeduren und Triggern heraus ausführen können.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a id="References"></a>Referenzen
1. [Einführung in Azure Cosmos DB][introduction]
2. [Azure Cosmos DB-SQL-Spezifikation](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-documentdb-net)
4. [Azure Cosmos DB-Konsistenzebenen][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. JavaScript-Spezifikation [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Abfrageauswertungstechniken für große Datenbanken [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.
13. G. Graefe. The Cascades framework for query optimization. IEEE Data Eng. Bull., 18(3): 1995.

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
