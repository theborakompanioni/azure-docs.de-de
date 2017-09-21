---
title: "Lucene-Abfragebeispiele für Azure Search | Microsoft Docs"
description: "Die Lucene-Abfragesyntax für Fuzzy- und NEAR-Suchen, Term Boosts, reguläre Ausdrücke und die Platzhaltersuche."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: Lucene query analyzer syntax
ms.assetid: 147f360d-a5ce-4d7b-a909-c8b65bfb748c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: liamca
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 1faed621039ecd04064cb074e6b9011418e6ec47
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Beispiele für die Lucene-Abfragesyntax zum Erstellen von Abfragen in Azure Search
Beim Erstellen von Abfragen für Azure Search können Sie entweder die standardmäßige [einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) oder alternativ dazu den [Lucene-Abfrageparser in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) verwenden. Der Lucene-Abfrageparser unterstützt komplexere Abfragekonstrukte, beispielsweise feldbezogene Abfragen, Fuzzysuche, NEAR-Suche, Begriffsverstärkung (Term Boosting) und die Suche nach regulären Ausdrücken.

In diesem Artikel können Sie Beispiele mit Abfragevorgängen durchlaufen, die bei Verwendung der vollständigen Syntax verfügbar sind.

## <a name="viewing-the-examples-in-jsfiddle"></a>Anzeigen der Beispiele in JSFiddle

Alle Beispiele in diesem Artikel sind ausführbare Abfragen, die für einen vorab geladenen Suchindex in [JSFiddle](https://jsfiddle.net)ausgeführt werden, einem Online-Code-Editor zum Testen von Skripts und HTML. 

Klicken Sie für die Ausführung mit der rechten Maustaste auf die URLs im Abfragebeispiel, um JSFiddle in einem separaten Browserfenster zu öffnen.

> [!NOTE]
> Der in den folgenden Beispielen verwendete Suchindex besteht aus Stellenangeboten basierend auf einem Dataset, das von der Initiative [City of New York OpenData](https://nycopendata.socrata.com/) bereitgestellt wird. Diese Daten sollten weder als aktuell noch als vollständig betrachtet werden. Der Index gilt für einen von Microsoft bereitgestellten Sandkastendienst. Sie benötigen weder ein Azure-Abonnement noch Azure Search, um diese Abfragen auszuprobieren.
>


## <a name="how-to-invoke-full-lucene-parsing"></a>Aufrufen der vollständigen Lucene-Analyse

In allen Beispielen in diesem Artikel wird der Suchparameter **queryType=full** verwendet. Damit wird angegeben, dass die vollständige Syntax vom Lucene-Abfrageparser verarbeitet wird. 

**Beispiel 1** : Klicken Sie mit der rechten Maustaste auf den folgenden Abfrageausschnitt, um diesen auf einer neuen Browserseite zu öffnen, auf der JSFiddle geladen und die Abfrage ausgeführt wird:

* [&queryType=full&search=*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Im neuen Browserfenster werden die JavaScript-Quelle und die HTML-Ausgabe nebeneinander dargestellt. Das Skript verweist auf eine vollständige Abfrage (nicht nur den unter dem Link dargestellten Ausschnitt). Die vollständige Abfrage wird für jedes Beispiel in den URLs angezeigt. 

Diese Abfrage gibt Dokumente aus unserem Stellenangebotsindex für New York City zurück (nycjobs, geladen in einem Sandboxdienst). Aus Platzgründen gibt die Abfrage nur die Rückgabe von Berufsbezeichnungen an. Die vollständige zugrunde liegende Abfrage lautet wie folgt:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Der Parameter **searchFields** begrenzt die Suche auf das Feld mit den Berufsbezeichnungen. Der **queryType** ist auf **full** festgelegt, wodurch Azure Search angewiesen wird, für diese Abfrage den Lucene-Abfrageparser zu verwenden.

> [!NOTE]
> Hintergrundinformationen zur Abfrageverarbeitung finden Sie unter [Funktionsweise der Volltextsuche in Azure Search](search-lucene-query-architecture.md). Weitere Informationen zu den Suchparametern finden Sie unter [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Durchsuchen von Dokumenten (REST-API des Azure Search-Diensts)).
>

### <a name="fielded-query-operation"></a>Feldbezogener Abfragevorgang
Sie können die Beispiele in diesem Artikel ändern, indem Sie ein Konstrukt aus **Feldname:Suchbegriff** angeben, um einen feldbezogenen Abfragevorgang zu definieren. Dabei besteht das Feld aus einem einzelnen Wort, und der Suchbegriff ist ebenfalls ein einzelnes Wort oder ein Ausdruck, optional mit booleschen Operatoren. Beispiele hierfür sind:

* business_title:(senior NOT junior)
* state:("New York" AND "New Jersey")

Achten Sie darauf, dass Sie mehrere Zeichenfolgen in Anführungszeichen setzen, wenn beide Zeichenfolgen als einzelne Entität ausgewertet werden sollen, da in diesem Fall im Ortsfeld nach zwei verschiedenen Städte gesucht wird. Stellen Sie außerdem sicher, dass der Operator großgeschrieben wird, wie im Fall von NOT und AND.

Das in **Feldname:Suchbegriff** angegebene Feld muss durchsuchbar sein. Einzelheiten zur Verwendung von Indexattributen in Felddefinitionen finden Sie unter [Index erstellen (REST-API in Azure Search-Dienst)](https://docs.microsoft.com/rest/api/searchservice/create-index) .

**Beispiel 2:** Klicken Sie mit der rechten Maustaste auf den folgenden Codeausschnitt. Diese Abfrage sucht nach Berufsbezeichnungen, in denen der Begriff „Senior“, jedoch nicht der Begriff „Junior“ enthalten ist:

* [&queryType=full&search= business_title:senior NOT junior](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Beispiel für die Fuzzysuche
Bei einer Fuzzysuche werden Übereinstimmungen in Ausdrücken gefunden, die ähnlich aufgebaut sind. Laut [Lucene-Dokumentation](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) basiert die Fuzzysuche auf der [Damerau-Levenshtein-Distanz](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Hängen Sie für eine Fuzzysuche das Tildesymbol „~“ ans Ende eines einzelnen Worts mit einem optionalen Parameter an, einem Wert zwischen 0 und 2, der die Editierdistanz angibt. Beispielsweise würden bei „blue~“ oder „blue~1“ die Werte „blue“, „blues“ und „glue“ zurückgegeben.

**Beispiel 3** : Klicken Sie mit der rechten Maustaste auf den folgenden Abfrageausschnitt. Diese Abfrage sucht nach Stellenangeboten mit dem Begriff „Associate“ (der falsch geschrieben ist):

* [&queryType=full&search= business_title:asosiate~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Fuzzyabfragen werden nicht [analysiert](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). Dies mag überraschend sein, wenn Sie Wortstammerkennung oder Lemmatisierung erwarten. Die lexikalische Analyse erfolgt nur für vollständige Begriffe (Begriffsabfrage oder Ausdrucksabfrage). Abfragetypen mit unvollständigen Begriffen (Präfixabfrage, Platzerhalterabfrage, Abfrage regulärer Ausdrücke, Fuzzyabfrage) werden direkt unter Umgehung der Analysephase an die Abfragestruktur angehängt. Die einzige Transformation, die für unvollständige Abfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
>

## <a name="proximity-search-example"></a>Beispiel für die NEAR-Suche
NEAR-Suchen werden verwendet, um Begriffe zu suchen, die in einem Dokument nahe beieinander liegen. Fügen Sie ein Tildesymbol „~“ Symbol am Ende eines Ausdrucks ein, gefolgt von der Anzahl der Wörter, die den NEAR-Bereich bilden. Beispielsweise finden Sie mit der Abfrage „"Hotel Flughafen"~5“ die Begriffe „Hotel“ und „Flughafen“, wenn sie innerhalb von 5 Wörtern in einem Dokument vorkommen.

**Beispiel 4** : Klicken Sie mit der rechten Maustaste auf die Abfrage. Suchen Sie nach Stellen mit dem Begriff „Senior Analyst“, wobei die beiden Wörter durch höchstens ein Wort getrennt sein sollen:

* [&queryType=full&search=business_title:"senior analyst"~1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Beispiel 5** : Versuchen Sie es noch einmal, und entfernen Sie die Wörter zwischen „Senior“ und „Analyst“.

* [&queryType=full&search=business_title:"senior analyst"~0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Beispiele für Begriffsverstärkung (Term Boosting)
Die Begriffsverstärkung (Term Boosting) bezieht sich auf das Höherbewerten eines Dokuments, wenn es den verstärkten Begriff enthält, im Verhältnis zu Dokumenten, die den Begriff nicht enthalten. Dies unterscheidet sich insofern von Bewertungsprofilen, als dass bei Bewertungsprofilen bestimmte Felder statt bestimmter Begriffe verstärkt werden. Im folgenden Beispiel werden die Unterschiede veranschaulicht.

Betrachten Sie ein Bewertungsprofil, das Übereinstimmungen in einem bestimmten Feld verstärkt, beispielsweise **genre** im musicstoreindex-Beispiel. Mit der Begriffsverstärkung könnten Sie bestimmte Suchbegriffe noch höher bewerten als andere. Zum Beispiel werden mit „rock^2 electronic“ Dokumente, die die Suchbegriffe im Feld **genre** enthalten, höher eingestuft als andere durchsuchbare Felder im Index. Darüber hinaus werden Dokumente, die den Suchbegriff „Rock“ enthalten, höher eingestuft als der andere Suchbegriff „electronic“ – aufgrund des Werts (2) für die Begriffsverstärkung.

Verwenden Sie zum Verstärken eines Begriffs das Caretzeichen „^“ mit einem Verstärkungsfaktor (einer Zahl) am Ende des Begriffs, nach dem Sie suchen. Je höher der Verstärkungsfaktor, desto relevanter wird der Begriff im Verhältnis zu anderen Suchbegriffen. Der Standardverstärkungsfaktor ist 1. Der Verstärkungsfaktor muss zwar positiv, kann jedoch kleiner als 1 sein (z. B. „0.2“).

**Beispiel 6**: Klicken Sie mit der rechten Maustaste auf die Abfrage. Wir suchen nach Stellen mit dem Begriff „Computer Analyst“. Es gibt keine Ergebnisse mit beiden Wörtern, „Computer“ und „Analyst“. Stellen für Analysten werden in den Ergebnissen weit oben angezeigt.

* [&queryType=full&search=business_title:computer analyst](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Beispiel 7**: Versuchen Sie es noch einmal, indem Sie dieses Mal in den Ergebnissen den Begriff „Computer“ gegenüber dem Begriff „Analyst“ verstärken, wenn nicht beide Wörter zusammen vorhanden sind.

* [&queryType=full&search=business_title:computer^2 analyst](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Beispiel für einen regulären Ausdruck
Bei einer Suche mit regulärem Ausdruck werden Übereinstimmungen basierend auf dem Inhalt zwischen Schrägstrichen „/“ gefunden, wie in der [RegExp-Klasse](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html)dokumentiert.

**Beispiel 8** : Klicken Sie mit der rechten Maustaste auf die Abfrage. Suchen Sie Stellen mit den Begriff „Senior“ oder „Junior“.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Die URL für dieses Beispiel wird auf der Seite nicht ordnungsgemäß gerendert. Um dieses Problem zu umgehen, kopieren Sie die folgende URL, und fügen Sie sie in das URL-Adressfeld des Browsers ein: `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Beispiel für die Platzhaltersuche
Sie können die allgemein bekannte Syntax für die Platzhaltersuche nach mehreren (\*) oder einzelnen (?) Zeichen verwenden. Beachten Sie, dass der Lucene-Abfrageparser die Verwendung dieser Symbole bei einem einzelnen Begriff, nicht bei einem Ausdruck, unterstützt.

**Beispiel 9**: Klicken Sie mit der rechten Maustaste auf die Abfrage. Suchen Sie nach Stellen, die das Präfix „Prog“ enthalten. Dazu gehören beispielsweise Berufsbezeichnungen mit den Begriffen „Programmierer“ und „Programmierung“.

* [&queryType=full&$select=business_title&search=business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:prog*)

Sie können die Symbole * oder ? nicht als erstes Zeichen in einer Suche verwenden.

## <a name="next-steps"></a>Nächste Schritte
Versuchen Sie, den Lucene-Abfrageparser in Ihrem Code anzugeben. Unter den folgenden Links wird erläutert, wie Sie Suchabfragen sowohl für .NET als auch für die REST-API einrichten. Bei diesen Links wird die einfache Standardsyntax „simple“ verwendet, daher müssen Sie das in diesem Artikel Gelernte anwenden, um den **queryType**anzugeben.

* [Abfragen des Azure Search-Indexes mit dem .NET SDK](search-query-dotnet.md)
* [Abfragen des Azure Search-Indexes mit der REST-API](search-query-rest-api.md)

## <a name="see-also"></a>Weitere Informationen

 [Funktionsweise der Volltextsuche in Azure Search](search-lucene-query-architecture.md)
