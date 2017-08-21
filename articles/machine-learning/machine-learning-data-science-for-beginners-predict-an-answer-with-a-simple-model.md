---
title: "Vorhersagen einer Antwort mit einem einfachen Regressionsmodell – Azure Machine Learning | Microsoft-Dokumentation"
description: "Erfahren Sie im 4. Video von „Data Science für Einsteiger“ mehr über das Erstellen eines Regressionsmodells zum Vorhersagen eines Preises. Enthält eine lineare Regression mit Zieldaten."
keywords: Erstellen eines Modells,einfaches Modell,Preisvorhersage,einfaches Regressionsmodell
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.translationtype: HT
ms.sourcegitcommit: 19be73fd0aec3a8f03a7cd83c12cfcc060f6e5e7
ms.openlocfilehash: ef651e3faa2abd7c1becb5dc2888d465330d4d73
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="predict-an-answer-with-a-simple-model"></a>Vorhersagen einer Antwort mit einem einfachen Modell
## <a name="video-4-data-science-for-beginners-series"></a>4. Video der Reihe „Data Science für Einsteiger“
Erfahren Sie im 4. Video von „Data Science für Einsteiger“ mehr über das Erstellen eines einfachen Regressionsmodells zum Vorhersagen des Preises eines Diamanten. Wir zeichnen ein Regressionsmodell mit Zieldaten.

Die Reihe bietet den größten Nutzen, wenn Sie sich alle Videos ansehen. [Zur Liste der Videos wechseln](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere Videos in dieser Reihe
*Data Science für Einsteiger* erhalten Sie eine Schnelleinführung in Data Science.

* Video 1: [Die 5 Fragen, die Data Science beantwortet](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5:14 Min.)*
* Video 2: [Sind Ihre Daten für Data Science bereit?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4:56 Min.)*
* Video 3: [Stellen einer Frage, die Sie mit Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4:17 Min.)*
* Video 4: Vorhersagen einer Antwort mit einem einfachen Modell
* Video 5: [Kopieren der Arbeit anderer für Ihre Data Science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3:18 Min.)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Aufzeichnung: Vorhersagen einer Antwort mit einem einfachen Modell
Willkommen beim vierten Video der Reihe „Data Science für Einsteiger“. In diesem Video erstellen wir ein einfaches Modell, um eine Vorhersage zu treffen.

Ein *Modell* ist eine vereinfachte, aber aussagekräftige Darstellung unserer Daten. Ich zeige Ihnen, was ich meine.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Sammeln relevanten, genauer und verbundener Daten in ausreichender Menge
Angenommen, ich möchte einen Diamanten kaufen. Ich habe einen Ring, der meiner Großmutter gehört hat, mit einer Fassung für einen Diamanten mit 1,35 Karat, und möchte eine Vorstellung erhalten, wie viel er kostet. Als Nächstes zeichnen wir eine vertikale Achse zum Aufzeichnen des Preises und verbinden Sie mit der horizontalen Gewichtsachse. Der erste Diamant hat 1,01 Karat und kostet 7.366 US-Dollar.

Das wiederhole ich jetzt für alle anderen Diamanten in dem Geschäft.

![Spalten mit Daten zu Diamanten](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Wie Sie sehen, hat unsere Liste zwei Spalten. Jede Spalte weist ein anderes Attribut auf (Gewicht in Karat und Preis). Jede Zeile ist ein einzelner Datenpunkt, der einen einzelnen Diamanten darstellt.

Wir haben soeben ein kleines Dataset erstellt, eine Tabelle. Beachten Sie, dass es unsere Qualitätskriterien erfüllt:

* Die Daten sind **relevant** , denn das Gewicht steht definitiv in Bezug zum Preis.
* Sie sind **genau** , denn wir haben die Preise gewissenhaft überprüft und notiert.
* Sie sind **verbunden** , denn es gibt in diesen Spalten keine leeren Stellen.
* Und wie wir sehen werden, verfügen wir über **genügend** Daten, um unsere Frage zu beantworten.

## <a name="ask-a-sharp-question"></a>Stellen einer genauen Frage
Nun stellen wir unsere Frage so genau wie möglich: „Wie viel kostet ein Diamant mit 1,35 Karat?“

Unsere Liste enthält keinen Diamanten mit 1,35 Karat, weshalb wir unsere restlichen Daten nutzen, um die Frage zu beantworten.

## <a name="plot-the-existing-data"></a>Zeichnen der vorhandenen Daten
Als Erstes zeichnen wir einen horizontalen Zahlenstrahl, der Achse genannt wird, um die Gewichte abzubilden. Der Gewichtsbereich ist 0 bis 2, weshalb wir eine Gerade zeichnen, die diesen Bereich abdeckt und die wir mit Teilstrichen für jedes halbe Karat versehen.

Als Nächstes zeichnen wir eine vertikale Achse zum Aufzeichnen des Preises und verbinden sie mit der horizontalen Gewichtsachse. Diese Achse wird mit Einheiten in Dollar versehen. Jetzt haben wir Koordinatenachsen.

![Achsen für Preis und Gewicht](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Anhand dieser Daten erstellen wir nun ein *Punktdiagramm*. Dies ist eine hervorragende Möglichkeit zum Visualisieren numerischer Datasets.

Für den ersten Datenpunkt zeichnen wir Pi mal Daumen eine vertikale Gerade bei 1,01 Karat. Dann zeichnen wir ebenso eine horizontale Gerade bei 7.366 US-Dollar. An ihrem Schnittpunkt zeichnen wir einen Punkt. Dieser stellt unseren ersten Diamanten dar.

Dasselbe machen wir nun für jeden Diamanten in dieser Liste. Wenn wir damit fertig sind, erhalten wir das hier: eine Menge von Punkten, einer für jeden Diamanten.

![Punktdiagramm](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Zeichnen des Modells durch die Datenpunkte
Wenn wir uns jetzt die Punkte etwas genauer ansehen, sieht die Sammlung wie eine dicke, unscharfe Linie aus. Wir nehmen unseren Textmarker und zeichnen eine Gerade mittendurch.

Durch das Zeichnen einer Geraden haben wir ein *Modell*erstellt. Stellen Sie sich vor, dies wäre die reale Welt, von der Sie eine vereinfachte Cartoonversion erstellen. Doch der Cartoon ist falsch, denn die Gerade verläuft nicht durch alle Datenpunkte. Aber es ist eine nützliche Vereinfachung.

![Lineare Regressionsgerade](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Die Tatsache, dass die Gerade nicht exakt durch alle Punkte verläuft, ist okay. Data Scientists erklären dies, indem sie sagen, dass das Modell vorhanden ist (hier die Gerade) und dass jedem Punkt eine *Verzerrung* bzw. *Varianz* zugeordnet ist. Da gibt es zum einen die zugrunde liegende perfekte Beziehung und zum anderen die raue, reale Welt, die Verzerrung und Unsicherheit hinzufügt.

Da wir die Frage *Wie viel?* beantworten möchten, wird dies als *Regression* bezeichnet. Und da wir eine Gerade verwenden, handelt es sich um eine *lineare Regression*.

## <a name="use-the-model-to-find-the-answer"></a>Verwenden des Modells zum Finden der Antwort
Jetzt haben wir ein Modell, dem wir unsere Frage stellen: Was kostet ein Diamant mit 1,35 Karat?

Um unsere Frage zu beantworten, machen wir 1,35 Karat aus und zeichnen eine vertikale Gerade. An der Stelle, an der sie die Modellgerade schneidet, zeichnen wir eine horizontale Gerade zur Dollar-Achse. Das Ergebnis liegt genau bei 10.000. Wow! Das ist die Antwort: ein Diamant mit 1,35 Karat kostet 10.000 US-Dollar.

![Finden der Antwort im Modell](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Erstellen eines Konfidenzintervalls
Es stellt sich natürlich die Frage, wie genau diese Vorhersage ist. Es ist hilfreich zu wissen, ob ein Diamant mit 1,35 Karat sehr nah bei 10.000 US-Dollar liegt oder sehr viel darüber oder darunter. Um das herauszufinden, wollen wir eine Umhüllungskurve um die Regressionsgerade zeichnen, die die meisten Punkte einschließt. Diese Umhüllungskurve bezeichnen wir als unser *Konfidenzintervall*: Wir sind zuversichtlich, dass sich Preise in dieser Umhüllungskurve befinden, da dies in der Vergangenheit meist der Fall war. Wir können zwei weitere horizontale Geraden zeichnen, und zwar an den Stellen, an denen die Gerade von 1,35 Karat den oberen und den unteren Rand der Umhüllungskurve schneidet.

![Konfidenzintervall](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nun können wir etwas zu unserem Konfidenzintervall sagen, nämlich mit Bestimmtheit, dass der Preis eines Diamanten mit 1,35 Karat ca. 10.000 US-Dollar beträgt, jedoch auch nur 8.000 oder sogar 12.000 US-Dollar betragen kann.

## <a name="were-done-with-no-math-or-computers"></a>Wir sind fertig – ohne Berechnungen oder Computer.
Wir haben das getan, wofür Data Scientist bezahlt werden, und mussten dazu nichts weiter tun als zeichnen:

* Wir haben eine Frage gestellt, die wir mit Daten beantworten konnten.
* Wir haben ein *Modell* mithilfe der *linearen Regression* erstellt.
* Wir haben eine *Vorhersage* samt *Konfidenzintervall* getroffen.

Und wir haben dafür nicht auf Berechnungen oder Computer zurückgegriffen.

Nun, wenn wir weitere Informationen gehabt hätten, wie beispielsweise...

* den Schliff des Diamanten
* Farbvariationen (wie nahe der Farbton des Diamanten an Weiß ist)
* die Anzahl der Einschlüsse im Diamanten

... hätten wir mehr Spalten. In diesem Fall sind Berechnungen hilfreich. Bei mehr als zwei Spalten ist es schwierig, Punkte auf Papier zu zeichnen. Mithilfe von Berechnungen können Sie die jeweilige Gerade oder Ebene mühelos an Ihre Daten anpassen.

Auch wenn wir anstelle einer Handvoll Diamanten zweitausend oder zwei Millionen hätten, kann diese Aufgabe mit einem Computer wesentlich schneller erledigt werden.

Heute haben wir uns über lineare Regression unterhalten und eine Vorhersage mithilfe von Daten gemacht.

Sehen Sie sich unbedingt auch die anderen Videos in der Reihe „Data Science für Einsteiger“ von Microsoft Azure Machine Learning an.

## <a name="next-steps"></a>Nächste Schritte
* [Durchführen eines ersten Data Science-Experiments mit Machine Learning Studio](machine-learning-create-experiment.md)
* [Einführung in Machine Learning in Microsoft Azure](machine-learning-what-is-machine-learning.md)

