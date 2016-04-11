<properties
    pageTitle="Lucene-Abfragebeispiele für Azure Search | Microsoft Azure Search"
    description="Die Lucene-Abfragesyntax für Fuzzy- und NEAR-Suchen, Term Boosts, reguläre Ausdrücke und die Platzhaltersuche."
    services="search"
    documentationCenter=""
	authors="LiamCa"
	manager="pablocas"
	editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="03/25/2016"
    ms.author="liamca"
/>

# Beispiele für die Lucene-Abfragesyntax zum Erstellen von Abfragen in Azure Search

Eine Alternative zur [Standardsyntax für einfache Abfragen](https://msdn.microsoft.com/library/azure/dn798920.aspx) besteht darin, den [Lucene-Abfrageparser in Azure Search](https://msdn.microsoft.com/library/azure/mt589323.aspx) zu verwenden. Der Lucene-Abfrageparser unterstützt komplexere Abfragekonstrukte, beispielsweise feldbezogene Abfragen, Fuzzysuche, NEAR-Suche, Begriffsverstärkung (Term Boosting) und die Suche nach regulären Ausdrücken.

In diesem Artikel können Sie Beispiele durchlaufen, in denen die Abfragesyntax und die Ergebnisse nebeneinander dargestellt werden. Die Beispiele werden für einen vorab geladenen Suchindex in [JSFiddle](https://jsfiddle.net/) ausgeführt, einem Online-Code-Editor zum Testen von Skripts und HTML.

Klicken Sie mit der rechten Maustaste auf die URLs im Abfragebeispiel, um JSFiddle in einem separaten Browserfenster zu öffnen.

> [AZURE.NOTE] Der in den folgenden Beispielen verwendete Suchindex besteht aus Stellenangeboten basierend auf einem Dataset, das von der Initiative [City of New York OpenData](https://nycopendata.socrata.com/) bereitgestellt wird. Diese Daten sollten weder als aktuell noch als vollständig betrachtet werden. Der Index gilt für einen von Microsoft bereitgestellten Sandkastendienst. Sie benötigen weder ein Azure-Abonnement noch Azure Search, um diese Abfragen auszuprobieren.

## Festlegen des Lucene-Abfrageparsers für die Suchanforderung

Geben Sie mithilfe des Suchparameters **queryType** den Lucene-Abfrageparser an. Der **queryType** wird bei jeder Anforderung angegeben. Gültige Werte sind **simple**|**full**, der Standardwert lautet **simple**. Ausführliche Informationen zur Angabe von Abfrageparametern finden Sie unter [Dokumente durchsuchen (REST-API in Azure Search-Dienst)](https://msdn.microsoft.com/library/azure/dn798927.aspx).

**Beispiel 1**: Klicken Sie mit der rechten Maustaste auf den folgenden Abfrageausschnitt, um diesen auf einer neuen Browserseite zu öffnen, auf der JSFiddle geladen und die Abfrage ausgeführt wird. Diese Abfrage gibt Dokumente aus unserem Stellenangebotsindex zurück (der in einem Sandkastendienst geladen ist):
- [&queryType=full&search=*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

## Feldbezogener Abfragevorgang

In den folgenden Beispielen geben Sie ein Konstrukt aus **Feldname:Suchbegriff** an, um einen feldbezogenen Abfragevorgang zu definieren. Dabei besteht das Feld aus einem einzelnen Wort, und der Suchbegriff ist ebenfalls ein einzelnes Wort oder ein Ausdruck, optional mit booleschen Operatoren. Beispiele hierfür sind:

- business\_title:senior NOT junior
- state:"New York" AND "New Jersey"

Achten Sie darauf, dass Sie mehrere Zeichenfolgen in Anführungszeichen setzen, wenn beide Zeichenfolgen als einzelne Entität ausgewertet werden sollen, da in diesem Fall im Ortsfeld nach zwei verschiedenen Städte gesucht wird. Stellen Sie außerdem sicher, dass der Operator großgeschrieben wird, wie im Fall von NOT und AND.

Das in **Feldname:Suchbegriff** angegebene Feld muss durchsuchbar sein. Einzelheiten zur Verwendung von Indexattributen in Felddefinitionen finden Sie unter [Index erstellen (REST-API in Azure Search-Dienst)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## Fuzzysuche

Bei einer Fuzzysuche werden Übereinstimmungen in Ausdrücken gefunden, die ähnlich aufgebaut sind. Laut [Lucene-Dokumentation](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) basieren Fuzzysuchen auf der [Damerau-Levenshtein-Distanz](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Verwenden Sie für eine Fuzzysuche das Tildesymbol „~“ am Ende eines einzelnen Worts mit einem optionalen Parameter, einem Wert zwischen 0 und 2, der die Editierdistanz angibt. Beispielsweise würden bei „blue~“ oder „blue~1“ die Werte „blue“, „blues“ und „glue“ zurückgegeben.

**Beispiel 2**: Klicken Sie mit der rechten Maustaste auf den folgenden Abfrageausschnitt, um die Abfrage auszuprobieren. Diese Abfrage sucht nach Geschäftstiteln, in denen der Begriff „Senior“, jedoch nicht der Begriff „Junior“ enthalten ist:

- [&queryType=full&search= business\_title:senior NOT junior](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## NEAR-Suche

NEAR-Suchen werden verwendet, um Begriffe zu suchen, die in einem Dokument nahe beieinander liegen. Fügen Sie ein Tildesymbol „~“ Symbol am Ende eines Ausdrucks ein, gefolgt von der Anzahl der Wörter, die den NEAR-Bereich bilden. Beispielsweise finden Sie mit der Abfrage „"Hotel Flughafen"~5“ die Begriffe „Hotel“ und „Flughafen“, wenn sie innerhalb von 5 Wörtern in einem Dokument vorkommen.

**Beispiel 3**: Klicken Sie mit der rechten Maustaste auf den folgenden Abfrageausschnitt. Diese Abfrage sucht nach Stellenangeboten mit dem Begriff „Associate“ (der falsch geschrieben ist):

- [&queryType=full&search= business\_title:asosiate~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Beispiel 4**: Klicken Sie mit der rechten Maustaste auf die Abfrage. Suchen Sie nach Stellen mit dem Begriff „Senior Analyst“, wobei die beiden Wörter durch höchstens ein Wort getrennt sein sollen:

- [&queryType=full&search=business\_title:"senior analyst"~1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Beispiel 5**: Versuchen Sie es noch einmal, und entfernen Sie die Wörter zwischen „Senior“ und „Analyst“.

- [&queryType=full&search=business\_title:"senior analyst"~0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## Term Boosting

Die Begriffsverstärkung (Term Boosting) bezieht sich auf das Höherbewerten eines Dokuments, wenn es den verstärkten Begriff enthält, im Verhältnis zu Dokumenten, die den Begriff nicht enthalten. Dies unterscheidet sich insofern von Bewertungsprofilen, als dass bei Bewertungsprofilen bestimmte Felder statt bestimmter Begriffe verstärkt werden. Im folgenden Beispiel werden die Unterschiede veranschaulicht.

Betrachten Sie ein Bewertungsprofil, das Übereinstimmungen in einem bestimmten Feld verstärkt, beispielsweise **genre** im musicstoreindex-Beispiel. Mit der Begriffsverstärkung könnten Sie bestimmte Suchbegriffe noch höher bewerten als andere. Zum Beispiel werden mit „rock^2 elektronic“ Dokumente, die die Suchbegriffe im Feld **genre** enthalten, höher eingestuft als andere durchsuchbare Felder im Index. Darüber hinaus werden Dokumente, die den Suchbegriff „Rock“ enthalten, höher eingestuft als der andere Suchbegriff „electronic“ – aufgrund des Werts (2) für die Begriffsverstärkung.

Verwenden Sie zum Verstärken eines Begriffs das Caretzeichen „^“ mit einem Verstärkungsfaktor (einer Zahl) am Ende des Begriffs, nach dem Sie suchen. Je höher der Verstärkungsfaktor, desto relevanter wird der Begriff im Verhältnis zu anderen Suchbegriffen. Der Standardverstärkungsfaktor ist 1. Der Verstärkungsfaktor muss zwar positiv, kann jedoch kleiner als 1 sein (z. B. „0.2“).

**Beispiel 6**: Klicken Sie mit der rechten Maustaste auf die Abfrage. Wir suchen nach Stellen mit dem Begriff „Computer Analyst“. Es gibt keine Ergebnisse mit beiden Wörtern, „Computer“ und „Analyst“. Stellen für Analysten werden in den Ergebnissen weit oben angezeigt.

- [&queryType=full&search=business\_title:computer analyst](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Beispiel 7**: Versuchen Sie es noch mal, indem Sie dieses Mal in den Ergebnissen den Begriff „Computer“ gegenüber dem Begriff „Analyst“ verstärken, wenn nicht beide Wörter zusammen vorhanden sind.

- [&queryType=full&search=business\_title:computer^2 analyst](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## Regulärer Ausdruck

Bei einer Suche mit regulärem Ausdruck werden Übereinstimmungen basierend auf dem Inhalt zwischen Schrägstrichen „/“ gefunden, wie in der [RegExp-Klasse](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html) dokumentiert.

**Beispiel 8**: Klicken Sie mit der rechten Maustaste auf die Abfrage. Suchen Sie Stellen mit den Begriff „Senior“ oder „Junior“.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Die URL für dieses Beispiel wird auf der Seite nicht ordnungsgemäß gerendert. Um dieses Problem zu umgehen, kopieren Sie die folgende URL, und fügen Sie sie in das URL-Adressfeld des Browsers ein: `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## Platzhaltersuche

Sie können die allgemein bekannte Syntax für die Platzhaltersuche nach mehreren (*) oder einzelnen (?) Zeichen verwenden. Beachten Sie, dass der Lucene-Abfrageparser die Verwendung dieser Symbole bei einem einzelnen Begriff, nicht bei einem Ausdruck, unterstützt.

**Beispiel 9**: Klicken Sie mit der rechten Maustaste auf die Abfrage. Suchen Sie nach Stellen, die das Präfix „Prog“ enthalten. Dazu gehören beispielsweise Berufsbezeichnungen mit den Begriffen „Programmierer“ und „Programmierung“.

- [&queryType=full&$select=business\_title&search=business\_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Sie können „*“ und „?“ nicht als erstes Zeichen in einer Suche verwenden.


## Nächste Schritte

Versuchen Sie, den Lucene-Abfrageparser in Ihrem Code anzugeben. Unter den folgenden Links wird erläutert, wie Sie Suchabfragen sowohl für .NET als auch für die REST-API einrichten. Bei diesen Links wird die einfache Standardsyntax „simple“ verwendet, daher müssen Sie das in diesem Artikel Gelernte anwenden, um den **queryType** anzugeben.

- [Abfragen des Azure Search-Indexes mit dem .NET SDK](search-query-dotnet.md)
- [Abfragen des Azure Search-Indexes mit der REST-API](search-query-rest-api.md)


 

<!---HONumber=AcomDC_0330_2016-->