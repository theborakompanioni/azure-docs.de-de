<properties
	pageTitle="Schnellstartanleitung: Machine Learning-Empfehlungs-API | Microsoft Azure"
	description="Azure Machine Learning-Empfehlungen – Kurzanleitung"
	services="cognitive-services"
	documentationCenter=""
	authors="luiscabrer"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="cognitive-services"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="luisca"/>

#  Buildtypen und Modellqualität #

<a name="TypeofBuilds"></a>
## Unterstützte Buildtypen ##

Die Empfehlungs-API unterstützt derzeit zwei Buildtypen: *Empfehlung* und *FBT*. Beide werden mit unterschiedlichen Algorithmen erstellt und bieten unterschiedliche Vorteile. In diesem Dokument werden alle Builds sowie Techniken zum Vergleichen der Qualität der generierten Modelle beschrieben.

Sofern Sie dies noch nicht getan haben, empfehlen wir Ihnen, die [Kurzanleitung](cognitive-services-recommendations-quick-start.md) durchzugehen.

<a name="RecommendationBuild"></a>
### Buildtyp „Empfehlung“ ###

Der Buildtyp „Empfehlung“ verwendet die Matrixfaktorisierung, um Empfehlungen bereitzustellen. Er generiert basierend auf Ihren Transaktionen [Eigenvektoren von Features](https://en.wikipedia.org/wiki/Latent_variable), um jeden Artikel zu beschreiben, und verwendet diese Eigenvektoren anschließend zum Vergleichen ähnlicher Artikel.

Wenn Sie das Modell basierend auf Käufen in einem Elektromarkt trainieren und ein Smartphone des Typs Lumia 650 als Eingabe für das Modell verwenden, gibt das Modell einen Satz von Artikeln zurück, die zumeist von Kunden gekauft werden, die wahrscheinlich ein Lumia 650 kaufen. Die Artikel ergänzen sich möglicherweise nicht. In diesem Beispiel ist es möglich, dass das Modell andere Telefone zurückgibt, da Kunden, denen das Lumia 650 gefällt, vielleicht andere Telefone mögen.

Der Empfehlungsbuild hat zwei Merkmale, die ihn reizvoll machen:

**Der Empfehlungsbuild unterstützt die Platzierung *kalter Artikel*.**

Artikel, die nur selten genutzt werden, werden als kalte Artikel bezeichnet. Wenn Sie beispielsweise eine Lieferung eines Telefons erhalten, das Sie noch nie verkauft haben, kann das System keine Empfehlungen für dieses Produkt ableiten, die ausschließlich auf Transaktionen basieren. Dies bedeutet, dass sich das System anhand von Informationen zum Produkt selbst informieren muss.

Wenn Sie die Platzierung kalter Artikel verwenden möchten, müssen Sie für jeden Artikel im Katalog Informationen zu Produktfeatures bereitstellen. Das folgende Beispiel zeigt, wie die ersten Zeilen Ihres Katalogs aussehen könnten (beachten Sie das „Schlüssel=Wert“-Format für die Features).

>6CX-00001,Surface Pro2, Surface,, Type=Hardware, Storage=128 GB, Memory=4G, Manufacturer=Microsoft

>73H-00013,Wake Xbox 360,Gaming,, Type=Software, Language=English, Rating=Mature

>WAH-0F05,Minecraft Xbox 360,Gaming,, * Type=Software, Language=Spanish, Rating=Youth

Sie müssen auch die folgenden Buildparameter festlegen:

| Buildparameter | Hinweise
|------------------     |-----------
|*useFeaturesInModel* | Legen Sie diesen Parameter auf **True** fest. Gibt an, ob Features verwendet werden können, um das Empfehlungsmodell zu erweitern.
|*allowColdItemPlacement* | Legen Sie diesen Parameter auf **True** fest. Hier wird angegeben, ob durch die Empfehlung ein „Push“ kalter Artikel anhand der Ähnlichkeit der Features erfolgen soll.
| *modelingFeatureList* | Kommagetrennte Liste von Featurenamen, die im Empfehlungsbuild verwendet werden sollen, um die Empfehlung zu verbessern (z.B. „Language,Storage“ im obigen Beispiel).

**Der Empfehlungsbuild unterstützt Benutzerempfehlungen.**

Ein Empfehlungsbuild bietet Unterstützung für [Benutzerempfehlungen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Dies bedeutet, dass er basierend auf den Transaktionsverläufen personalisierte Empfehlungen für Benutzer bereitstellen kann. Für Benutzerempfehlungen können Sie die Benutzer-ID oder den aktuellen Transaktionsverlauf des Benutzers angeben.

Ein klassisches Beispiel für die Verwendung von Benutzerempfehlungen ist die Anmeldung auf der Willkommensseite. Dort können Sie Inhalt bewerben, der zu dem jeweiligen Benutzer passt.

Sie können auch einen Empfehlungsbuildtyp anwenden, wenn der Benutzer zur Kasse geht. An dieser Stelle wird die Liste der Artikel gezeigt, die der Benutzer kaufen möchte. Dies ist eine gute Möglichkeit, um auf dem aktuellen Warenkorb basierende Empfehlungen anzuzeigen.

#### Parameter für Empfehlungsbuilds

| Name | 	Beschreibung |	 Typ, <br> gültige Werte, <br> (Standardwert)
|-------|-------------------|------------------
| *NumberOfModelIterations* |	Die Anzahl der Iterationen, die vom Modell ausgeführt werden, beeinflusst die Gesamtrechenzeit sowie die Modellgenauigkeit. Je höher die Anzahl ist, desto genauer ist das Modell (die Berechnung dauert bei einer höheren Anzahl allerdings länger). |	 Integer, <br> 10 bis 50 <br>(40)
| *NumberOfModelDimensions* |	Die Anzahl von Dimensionen bezieht sich auf die Anzahl von Features, die das Modell in Ihren Daten zu finden versucht. Wenn Sie die Anzahl der Dimensionen erhöhen, können Sie die Ergebnisse in kleineren Clustern feiner abstimmen. Durch zu viele Dimensionen wird jedoch verhindert, dass Korrelationen zwischen den Elementen gefunden werden. |	Integer, <br> 10 bis 40 <br>(20) |
| *ItemCutOffLowerBound* |	Definiert die Mindestanzahl von Nutzungspunkten, in denen sich ein Artikel befinden sollte, damit er im Modell berücksichtigt wird. |		Integer, <br> 2 oder mehr <br> (2) |
| *ItemCutOffUpperBound* | 	Definiert die Höchstanzahl von Nutzungspunkten, in denen sich ein Artikel befinden sollte, damit er im Modell berücksichtigt wird. | Integer, <br>2 oder mehr<br> (2147483647) |
|*UserCutOffLowerBound* |	Definiert die Mindestanzahl von Transaktionen, die ein Benutzer ausgeführt haben muss, um im Modell berücksichtigt zu werden. |	Integer, <br> 2 oder mehr <br> (2)
| *UserCutOffUpperBound* |	Definiert die Höchstanzahl von Transaktionen, die ein Benutzer ausgeführt haben muss, um im Modell berücksichtigt zu werden. |	Integer, <br>2 oder mehr <br> (2147483647)|
| *UseFeaturesInModel* |	Gibt an, ob Features verwendet werden können, um das Empfehlungsmodell zu erweitern. | 	 Boolean<br> Standard: True
|*ModelingFeatureList* |	Kommagetrennte Liste von Featurenamen, die im Empfehlungsbuild verwendet werden sollen, um die Empfehlung zu verbessern. Diese Liste hängt von den wichtigen Features ab. |	String, bis zu 512 Zeichen
| *AllowColdItemPlacement* |	Hier wird angegeben, ob durch die Empfehlung ein „Push“ kalter Artikel anhand der Ähnlichkeit der Features erfolgen soll. | Boolean<br> Standard: False
| *EnableFeatureCorrelation* | Hier wird angegeben, ob Features bei der Argumentation verwendet werden können. |	Boolean<br> Standard: False
| *ReasoningFeatureList* |	Kommagetrennte Liste von Featurenamen, die für Argumentationssätze (z.B. Erklärungen von Empfehlungen) verwendet werden sollen. Diese Liste hängt von den für Kunden wichtigen Features ab. | String, bis zu 512 Zeichen
| *EnableU2I* |	Aktiviert personalisierte Empfehlungen, so genannte „User to Item“-Empfehlungen (U2I). | Boolean<br> Standard: True
|*EnableModelingInsights* |	Definiert, ob eine Offlineauswertung erfolgen soll, um Modellierungserkenntnisse (d.h. Metriken für Genauigkeit und Vielfalt) zu sammeln. Ist dieser Parameter auf „True“ festgelegt, wird eine Teilmenge der Daten nicht für das Training verwendet, da sie zum Testen des Modells reserviert werden müssen. Weitere Informationen zu [Offlineauswertungen](#OfflineEvaluation). | Boolean<br> Standard: False
| *SplitterStrategy* | Wenn „EnableModelingInsights“ auf *True* festgelegt ist, bestimmt dieser Parameter, wie Daten für Auswertungszwecke aufgeteilt werden sollen. | String, *RandomSplitter* oder *LastEventSplitter* <br>Standard: RandomSplitter


<a name="FBTBuild"></a>
### FBT-Buildtyp ###

Der FBT-Build (Frequently Bought Together = häufig zusammen gekauft) führt eine Analyse durch, die die Anzahl von Kookkurrenzen von zwei oder drei verschiedenen Produkte zählt, d.h. wie oft sie gemeinsam vorkommen. Anschließend werden die Sätze auf Grundlage einer Ähnlichkeitsfunktion sortiert (**Kookkurrenzen**, **Jaccard**, **Lift**).

**Jaccard** und **Lift** können Sie sich als Möglichkeiten zum Normalisieren von Kookkurrenzen vorstellen. Dies bedeutet, dass die Artikel nur zurückgegeben werden, wenn sie zusammen mit dem Ausgangsartikel gekauft wurden.

Bei unserem Beispiel mit dem Lumia 650 wird Telefon X nur dann zurückgegeben, wenn Telefon X in derselben Sitzung wie das Lumia 650 gekauft wurde. Da dies eher unwahrscheinlich ist, erwarten wir, dass ergänzende Artikel für das Lumia 650 zurückgegeben werden, z.B. eine Displayschutzfolie oder ein Netzteil für das Lumia 650.

Derzeit gelten zwei Artikel als in derselben Sitzung gekauft, wenn sie in einer Transaktion mit derselben Benutzer-ID und demselben Zeitstempel vorkommen.

FBT-Builds unterstützen keine kalten Artikel, da sie per definitionem erwarten, dass zwei Artikel bei derselben Transaktion gekauft werden. FBT-Builds können zwar Sätze von Artikeln (Dreiergruppen) zurückgeben, unterstützen aber keine persönlichen Empfehlungen, da sie einen einzelnen Ausgangsartikel als Eingabe akzeptieren.


#### Parameter für FBT-Builds

| Name | 	Beschreibung |		Typ, <br> gültige Werte, <br> (Standardwert)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Dies ist ein Maß dafür, wie konservativ das Modell ist. Dies ein Maß für die Grauwerte von Elementen, die bei der Modellierung berücksichtigt werden. | Integer, <br> 3 bis 50 <br> (6)
| *FbtMaxItemSetSize* | Begrenzt die Anzahl der Elemente in einem häufigen Satz.| Integer <br> 2 bis 3 <br> (2)
| *FbtMinimalScore* | Die Mindestbewertung, die ein häufiger Satz haben muss, um in die zurückgegebenen Ergebnisse eingeschlossen zu werden. Höhere Bewertungen sind besser als niedrigere. | Double <br> 0 und höher <br> (0)
| *FbtSimilarityFunction* | Definiert die Ähnlichkeitsfunktion, die vom Build verwendet werden soll. **Lift** fördert die Zufälligkeit, **Kookkurrenz** fördert die Vorhersagbarkeit und **Jaccard** stellt einen Kompromiss zwischen diesen beiden Optionen dar. | String, <br> <i>cooccurrence, lift, jaccard</i><br> Standard: <i>jaccard</i>

<a name="SelectBuild"></a>
## Buildauswertung und -auswahl ##

Diese Richtlinien können Ihnen bei der Entscheidung zwischen Empfehlungs- oder FBT-Build helfen. In Fällen, in denen Sie beide Buildtypen verwenden könnten, liefern sie jedoch keine definitive Antwort. Doch auch wenn Sie wissen, dass Sie einen FBT-Buildtyp verwenden möchten, können Sie noch immer entscheiden, ob Sie **Jaccard** oder **Lift** als Ähnlichkeitsfunktion einsetzen möchten.

Die beste Methode zur Auswahl des geeigneten Buildtyps besteht darin, diese in der Praxis zu testen (Onlineauswertung) und die Konversionsrate für die verschiedenen Builds nachzuverfolgen. Die Konversionsrate kann basierend auf Empfehlungsklicks, der Anzahl tatsächlicher Käufe aufgrund angezeigter Empfehlungen oder sogar dem tatsächlichen Verkaufserlös bei der Anzeige verschiedener Empfehlungen gemessen werden. Sie können Ihre Metrik für die Konversionsrate basierend auf Ihrem Geschäftsziel auswählen.

In einigen Fällen möchten Sie das Modell möglicherweise offline auswerten, bevor Sie es in der Produktion einsetzen. Obwohl die Offlineauswertung kein Ersatz für die Onlineauswertung ist, kann sie uns als Metrik dienen.

<a name="OfflineEvaluation"></a>
## Offlineauswertung  ##

Ziel der Offlineauswertung ist das Vorhersagen der Genauigkeit (Anzahl von Benutzern, die einen der empfohlenen Artikel kaufen) und der Vielfältigkeit von Empfehlungen (Anzahl von Artikeln, die empfohlen werden). Im Rahmen der Auswertung der Genauigkeits- und Vielfältigkeitsmetriken bestimmt das System eine Stichprobe von Benutzern und teilt die Transaktionen dieser Benutzer in zwei Gruppen auf: das Trainingsdataset und das Testdataset.

> [AZURE.NOTE] Zur Verwendung von Offlinemetriken müssen Ihre Nutzungsdaten Zeitstempel aufweisen. Die Zeitdaten sind erforderlich, um die Nutzung korrekt auf das Trainingsdataset und das Testdataset aufzuteilen.

> Zudem führt die Offlineauswertung bei kleinen Nutzungsdateien möglicherweise nicht zu Ergebnissen. Für eine umfassende Auswertung sollte das Testdataset mindestens 1.000 Nutzungspunkte enthalten.

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
In der obigen Tabelle stellt *K* die Anzahl von Empfehlungen dar, die für den Kunden angezeigt werden. Die Tabelle liest sich wie folgt: „Wenn während des Testzeitraums für Kunden nur eine Empfehlung angezeigt worden wäre, wären nur 13,75 % der Benutzer dieser Kaufempfehlung gefolgt.“ Diese Aussage basiert auf der Annahme, dass das Modell mit Kaufdaten trainiert wurde. Anders ausgedrückt: 13,75 ist die „Genauigkeit bei 1“.

Beachten Sie, dass je mehr Artikel dem Kunden gezeigt werden, die Wahrscheinlichkeit steigt, dass der Kunde einen empfohlenen Artikel kauft. Beim obigen Experiment verdoppelt sich die Wahrscheinlichkeit auf nahezu 26,61 %, wenn fünf Artikel empfohlen werden.

#### Prozentsatz
Der Prozentsatz der Benutzer, die mit mindestens einer der gezeigten *K* Empfehlungen interagiert haben. Zum Berechnen des Prozentsatzes wird die Anzahl von Benutzern, die mit mindestens einer Empfehlung interagiert haben, durch die Gesamtanzahl berücksichtigter Benutzer dividiert. Weitere Informationen finden Sie unter „Berücksichtigte Benutzer“.

#### Benutzer im Test
Die Daten in dieser Zeile stellen die Gesamtanzahl von Benutzern im Testdataset dar.

#### Berücksichtigte Benutzer
Ein Benutzer wird nur berücksichtigt, wenn das System mindestens *K* Artikel basierend auf dem mit dem Trainingsdataset generierten Modell empfohlen hat.

#### Nicht berücksichtigte Benutzer
Die Daten in dieser Zeile stellen alle Benutzer dar, die nicht berücksichtigt wurden, d.h. die Benutzer, denen nicht mindestens *K* Artikel empfohlen wurden.

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
Jeder Quantilbucket wird durch eine Spanne dargestellt (Mindest- und Höchstwerte im Bereich von 0 bis 100). Die Artikel nahe bei 100 sind die beliebtesten Artikel, die Artikel nahe bei 0 die unbeliebtesten. Wenn beispielsweise der Prozentwert für den Quantilbucket 99-100 gleich 10,6 ist, bedeutet dies, dass 10,6 % der Empfehlungen nur das eine oberste Prozent der beliebtesten Artikel zurückgegeben haben. Der Mindestwert des Quantilbuckets ist inklusiv, wohingegen der Höchstwert außer für 100 exklusiv ist.
#### Empfohlene eindeutige Artikel
Die Metrik für die empfohlenen eindeutigen Artikel zeigt die Anzahl unterschiedlicher Artikel, die zur Auswertung zurückgegeben wurden.
#### Gesamtanzahl empfohlener Artikel
Die Metrik für die Gesamtzahl empfohlener Artikel zeigt die Anzahl von Artikeln, die empfohlen wurden. Bei einigen dieser Artikel kann es sich um doppelte Empfehlungen handeln.

<a name="ImplementingEvaluation"></a>
### Offlineauswertungsmetriken ###
Die Offlinemetriken für Genauigkeit und Vielfältigkeit können bei der Wahl des zu verwendenden Buildtyps hilfreich sein. Nehmen Sie zur Buildzeit als Teil der entsprechenden Parameter für FBT- oder Empfehlungsbuilds folgende Einstellungen vor:

-	Legen Sie den Buildparameter *enableModelingInsights* auf **True** fest.
-	Wählen Sie optional die *splitterStrategy* aus (entweder *RandomSplitter* oder *LastEventSplitter*). *RandomSplitter* teilt die Nutzungsdaten basierend auf dem angegebenen Testprozentsatz *randomSplitterParameters* und zufälligen Ausgangswerten in Trainings- und Testdatasets auf. *LastEventSplitter* teilt die Nutzungsdaten basierend auf der letzten Transaktion jedes Benutzers in Trainings- und Testdatasets auf.

Dies löst einen Build aus, der nur eine Teilmenge der Daten für das Training verwendet und die restlichen Daten zur Berechnung von Auswertungsmetriken verwendet. Nachdem der Build abgeschlossen wurde, müssen Sie zum Abrufen der Ausgabe der Auswertung die [API zum Abrufen von Buildmetriken](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f) aufrufen und die entsprechende *modelId* und *buildId* übergeben.

 Im Folgenden sehen Sie die JSON-Ausgabe der Beispielauswertung.


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

<!---HONumber=AcomDC_0921_2016-->