<properties
	pageTitle="Schnellstartanleitung: Machine Learning-Empfehlungs-API | Microsoft Azure"
	description="Azure Machine Learning-Empfehlungen – Erste Schritte"
	services="cognitive-services"
	documentationCenter=""
	authors="luiscabrer"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="cognitive-services"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/24/2016"
	ms.author="luisca"/>

#  Buildtypen und Modellqualität #

<a name="TypeofBuilds"></a>
## Welche Art von Empfehlungsbuild sollte ich verwenden? ##

Derzeit unterstützen wir zwei Buildtypen: *Empfehlung* und *FBT*. Beide werden mit unterschiedlichen Algorithmen erstellt und bieten unterschiedliche Vorteile.

<a name="RecommendationBuild"></a>
### Buildtyp „Empfehlung“ ###

Die Buildtyp *Empfehlung* verwendet die Matrixfaktorisierung, um Empfehlungen bereitzustellen. Kurz gesagt, werden dabei die Transaktionen des Benutzers zum Generieren von [Eigenvektoren von Features](https://en.wikipedia.org/wiki/Latent_variable) verwendet, um jeden Artikel zu beschreiben. Anschließend werden diese Eigenvektoren zum Vergleichen mit ähnlichen Artikeln verwendet.

Angenommen, Sie trainieren das Modell basierend auf Käufen in einem Elektromarkt. Wenn Sie zum Bewertungszeitpunkt ein Smartphone des Typs Lumia 650 als Eingabe in das Modell verwenden, wird eine Menge von Artikeln zurückgegeben, die zumeist von Kunden gekauft werden, die ein Lumia 650 wahrscheinlich kaufen. Beachten Sie, dass sich die Artikel möglicherweise nicht ergänzen. Bei diesem Beispiel ist es möglich, dass andere Telefone zurückgegeben werden, da Kunden, denen das Lumia 650 gefällt, ggf. andere Telefone mögen.

Der Empfehlungsbuild hat zwei Merkmale, die ihn reizvoll machen:

-	Er unterstützt das Platzieren kalter Artikel.

 *Cold item* (kalter Artikel) ist der Begriff für Artikel, die nicht besonders verbreitet ist. Angenommen, Sie haben soeben eine Lieferung des neuesten Modells des Lumia-Telefons erhalten. Da Sie es bislang noch nicht verkauft haben, kann das System keine Empfehlungen für dieses Produkt herleiten, die allein auf Transaktionen basieren. Das bedeutet, dass sich das System anhand von Informationen zum Produkt selbst informieren muss.

 Wenn Sie kalte Artikel platzieren möchten, müssen Sie für jeden Artikel im Katalog Informationen zu den Features hinzufügen. So können die ersten Zeilen Ihres Katalogs beispielsweise aussehen (beachten Sie das Schlüssel=Wert-Format für die Features):

> 6CX-00001,Surface Pro2, Surface, Type=Hardware, Storage=128GB, Memory=4G, Manufacturer=Microsoft

> 73H-00013,Wake Xbox 360,Gaming, Type=Software, Language=English, Rating=Mature

> WAH-0F05,Minecraft Xbox 360,Gaming, * Type=Software, Language=Spanish, Rating=Youth

> ...

 Als Teil der Buildparameter müssen Sie außerdem die folgenden festlegen:

| Buildparameter | Hinweise
|------------------     |-----------
|useFeaturesInModel | Auf „True“ festlegen. Hier wird angegeben, ob Features verwendet werden können, um das Empfehlungsmodell zu verbessern. 
|allowColdItemPlacement | Auf „True“ festlegen. Hier wird angegeben, ob durch die Empfehlung ein „Push“ kalter Artikel anhand der Ähnlichkeit der Features erfolgen soll.
| modelingFeatureList | Dies ist die kommagetrennte Liste von Komponentennamen, die im Empfehlungsbuild verwendet werden soll, um die Empfehlung zu verbessern. Beispielsweise „Language,Storage“ im obigen Beispiel.

-	Er unterstützt Benutzerempfehlungen.

 Ein Empfehlungsbuild unterstützt [Benutzerempfehlungen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Dies bedeutet, dass ein Verlauf der Transaktionen eines Benutzers verwendet werden kann, um persönliche Empfehlungen für diesen Benutzer anzubieten. Für Benutzerempfehlungen können Sie die Benutzer-ID und/oder den jüngsten Verlauf der Transaktionen für diesen Benutzer angeben.

 Ein klassisches Beispiel, bei dem Sie möglicherweise Benutzerempfehlungen anzeigen möchten, ist bei der ersten Anmeldung des Benutzers bei Ihrem Onlineshop bzw. Ihrer Website auf der Begrüßungsseite. Dort können Sie Inhalt bewerben, der zu dem jeweiligen Benutzer passt.
 
 Sie können auch einen Empfehlungsbuildtyp anwenden, wenn der Benutzer vor dem Zahlvorgang steht. An dieser Stelle wird die Liste der Artikel gezeigt, die der Kunde kaufen möchten. Dies ist Ihre Chance, Empfehlungen basierend auf dem aktuellen Warenkorb einzublenden.

<a name="FBTBuild"></a>
### FBT-Buildtyp ###

*FBT* steht für „Frequently-Bought-Together“ (Häufig zusammen gekauft). Der FBT-Build führt eine Analyse durch, die zählt, wie oft zwei oder drei verschiedene Produkte zusammen vorkommen, und sortiert dann die Produktgruppen anhand einer Ähnlichkeitsfunktion (Kookkurenzen, Jaccard, Lift).

Stellen Sie sich Jaccard und Lift als Möglichkeiten zum Normalisieren von Kookkurenzen vor. Das heißt, dass die Artikel nur zurückgegeben werden, wenn Sie gemeinsam mit dem Ausgangsartikel gekauft wurden.

Bei unserem Beispiel mit dem Lumia 650 wird Telefon X nur dann zurückgegeben, wenn Telefon X in der gleichen Sitzung wie das Lumia 650 erworben wurde. Da dies eher unwahrscheinlich ist, erwarten wir die Rückgabe ergänzender Artikel für das Lumia 650, z. B. eine Displayschutzfolie oder ein Netzteil für das Lumia 650.

Derzeit gelten zwei Artikel als in derselben Sitzung erworben, wenn sie in einer Transaktion mit derselben Benutzer-ID und demselben Zeitstempel vorkommen.

FBT-Builds unterstützen derzeit keine kalten Artikel, das sie laut Definition erwarten, dass zwei Artikel tatsächlich in derselben Sitzung gekauft werden. Wenngleich FBT-Builds Gruppen von Artikeln (Dreiergruppen) zurückgeben können, unterstützen sie keine persönlichen Empfehlungen, da sie einen einzelnen Ausgangsartikel als Eingabe akzeptieren.

<a name="SelectBuild"></a>
## Wie wähle ich den zu verwendenden Build? ##

Die bisherigen Angaben sind vielleicht schon ausreichend, um Ihnen zu helfen, ob Sie einen Empfehlungs- oder FBT-Build wählen sollten. Sie liefern aber keine definitive Antwort in den Fällen, in denen Sie beide einsetzen könnten. Doch auch wenn Sie wissen, dass Sie einen FBT-Buildtyp verwenden möchten, müssen Sie sich immer noch entscheiden, ob Sie Jaccard oder Lift als Ähnlichkeitsfunktion nutzen wollen.

Die beste Möglichkeit zum Wählen des geeigneten Builds besteht darin, diese in der Praxis zu testen (Onlineauswertung) und die Konversionsrate für die verschiedenen Builds zu messen. Die Konversionsrate kann gemessen werden basierend auf Empfehlungsklicks, der gezeigten Anzahl aufgrund von Empfehlungen erfolgter Käufe oder dem tatsächlich Verkaufserlös, als die verschiedenen Empfehlungen gezeigt wurden. Sie können Ihre Metrik für die Konversionsrate basierend auf Ihrem Geschäftsziel auswählen.

In einigen Fällen möchten Sie das Modell möglicherweise offline auswerten, bevor Sie es in der Produktion einsetzen. Wenngleich die Offlineauswertung kein Ersatz für die Onlineauswertung ist, kann sie uns als Metrik dienen.

<a name="OfflineEvaluation"></a>
## Offlineauswertung  ##

Ziel der Offlineauswertung ist das Vorhersagen der Genauigkeit (Anzahl der Benutzer, die tatsächlich einen der empfohlenen Artikel kaufen) und der Vielfältigkeit von Empfehlungen (tatsächlich Anzahl von Artikeln, die empfohlen werden). Im Rahmen der Auswertung der Metriken für Genauigkeit und Vielfältigkeit bestimmt das System eine Stichprobe von Benutzern. Anschließend werden die Transaktionen dieser Benutzer in zwei Gruppen aufgeteilt: das Trainingsdataset und das Testdataset.

> Hinweis:
>
> Um Offlinemetriken verwenden zu können, müssen Ihre Nutzungsdaten Zeitstempel aufweisen. (Diese Zeitdaten sind erforderlich, um die Nutzung ordnungsgemäß auf das Trainingsdataset und das Testdataset aufzuteilen.)

> Darüber hinaus liefert die Offlineauswertung ggf. keine Ergebnisse für kleine Nutzungsdateien, da für eine sorgfältige Auswertung mindestens 1000 Nutzungspunkte im Testdataset erforderlich sind.

<a name="Precision"></a>
### Genauigkeit bei K ###
Die folgende Tabelle zeigt die Ausgabe der Offlineauswertung „Genauigkeit bei K“.

| K | 1 | 2 | 3 | 	4 | 	5
|---|---|---|---|---|---|
|Prozentsatz |	13,75 |	18,04 | 21 |	24,31 |	26,61
|Benutzer im Test |	10\.000 |	10\.000 |	10\.000 |	10\.000 |	10\.000
|Berücksichtigte Benutzer |	10\.000 |	10\.000 |	10\.000 |	10\.000 |	10\.000
|Nicht berücksichtigte Benutzer |	0 |	0 |	0 |	0 |	0

#### K
In dieser Tabelle stellt *K* die Anzahl von Empfehlungen dar, die dem Kunden gezeigt werden. Die Tabelle liest sich wie folgt: „Wenn während des Testzeitraums den Kunden nur eine Empfehlung gezeigt worden wäre, wären nur 13,75 % der Kunden tatsächlich dieser Kaufempfehlung gefolgt.“ (Vorausgesetzt wird, dass das Modell mit Kaufdaten trainiert wurde.) Eine andere Ausdrucksmöglichkeit lautet, dass 13,75 die „Genauigkeit bei 1“ ist.

Beachten Sie, dass je mehr Artikel dem Kunden gezeigt werden, die Wahrscheinlichkeit steigt, dass der Kunde einen empfohlenen Artikel kauft. Beim obigen Experiment verdoppelt sich die Wahrscheinlichkeit auf nahezu 26,61 %, wenn 5 Artikel empfohlen werden.

#### Prozentsatz
Der Prozentsatz der Benutzer, die mit mindestens einer der gezeigten K Empfehlungen interagiert hat. Der Prozentsatz wird berechnet, indem die Anzahl der Benutzer, die mit mindestens einer Empfehlung interagiert hat, durch die Gesamtanzahl der berücksichtigten Benutzer dividiert wird. (Siehe die Definition von „Berücksichtigte Benutzer“).

#### Benutzer im Test
Die Gesamtanzahl der Benutzer im Testdataset.

#### Berücksichtigte Benutzer
Ein Benutzer wird nur berücksichtigt, wenn das System mindestens K Artikel basierend auf dem mithilfe des Trainingsdatasets generierten Modells empfohlen hat.

#### Nicht berücksichtigte Benutzer
Alle Benutzer, die nicht berücksichtigt werden, d.h. die Benutzer, denen nicht mindestens K Artikel empfohlen wurden.

Nicht berücksichtigte Benutzer = Benutzer im Test – Berücksichtigte Benutzer

<a name="Diversity"></a>
### Vielfältigkeit ###
Metriken für die Vielfältigkeit dienen zum Messen des Typs der empfohlenen Artikel. Die folgende Tabelle zeigt die Ausgabe der Offlineauswertung der Vielfältigkeit.

|Quantilbucket |	0-90| 90-99| 99-100
|------------------|--------|-------|---------
|Prozentsatz | 34,258 | 55,127| 10,615


Gesamtanzahl der empfohlenen Artikel: 100.000

Empfohlene eindeutige Artikel: 954

#### Quantilbuckets
Jeder Quantilbucket stellt eine Spanne dar (d.h. Mindest- und Höchstwerte im Bereich von 0 bis 100). Die Artikel nahe bei 100 sind die beliebtesten Artikel, die Artikel nahe bei 0 die unbeliebtesten. Wenn beispielsweise der Prozentwert für den Quantilbucket 99-100 gleich 10,6 ist, heißt dies, dass 10,6 % der Empfehlungen nur die obersten 1 % der beliebtesten Artikel zurückgegeben haben. Der Mindestwert des Quantilbuckets ist inklusiv, wohingegen der Höchstwert außer für 100 exklusiv ist.
#### Empfohlene eindeutige Artikel
Anzahl unterschiedlicher Artikel, die für die Auswertung zurückgegeben wurden.
#### Gesamtanzahl der empfohlenen Artikel
Die Gesamtanzahl empfohlener Artikel (von denen einige Duplikate sein können).


<a name="ImplementingEvaluation"></a>
### Wie erfolgen Offlineauswertungen? ###
Die Offlinemetriken für Genauigkeit und Vielfältigkeit sind ggf. nützlich bei der Wahl des zu verwendenden Builds. Um Offlinemetriken zu erhalten, müssen Sie die folgenden Schritte ausführen:

Zum Buildzeitpunkt als Teil der entsprechenden Parameter für FTB- oder Empfehlungsbuilds:
1.	Legen Sie den Buildparameter „enableModelingInsights“ auf „True“ fest.

2.	Optional können Sie *splitterStrategy* (entweder *RandomSplitter* oder *LastEventSplitter*) auswählen. *RandomSplitter* teilt die Nutzungsdaten basierend auf dem angegebenen Testprozentsatz *randomSplitterParameters* und zufälligen Ausgangswerten in Trainings- und Testdatasets auf. *LastEventSplitter* teilt die Nutzungsdaten basierend auf der letzten Transaktion jedes Benutzers in Trainings- und Testdatasets auf.

Dies löst einen Build aus, der nur eine Teilmenge der Daten für das Training verwendet. Der Rest der Daten wird verwendet, um Auswertungsmetriken zu berechnen. Nachdem der Build abgeschlossen wurde, müssen Sie zum Abrufen der Ausgabe der Auswertung nur die[API zum Abrufen von Buildmetriken](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/573e43bb3e9d4627a8c4bd3e/console) aufrufen und die entsprechende *modelId* und *buildId* übergeben.

 Nachfolgend finden Sie die JSON-Ausgabe unserer Beispielauswertung:


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }

<!---HONumber=AcomDC_0601_2016-->