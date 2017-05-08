---
title: "Die fünf Data Science-Fragen – Data Science für Einsteiger – Azure | Microsoft-Dokumentation"
description: "Kurze Einführung in Data Science mithilfe der aus fünf Kurzvideos bestehenden Reihe „Data Science für Einsteiger“, die mit „Die 5 Fragen, die Data Science beantwortet“ beginnt."
keywords: "Ausführen von Data Science,Data Science-Einsteiger,Data Science für Einsteiger,Data Science-Grundlagen,Data Science-Fragen,Data Science-Video,Data Science-Einführung"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a01f93ee-01eb-4afe-abbd-cfa035c119b0
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2017
ms.author: cgronlun;garye
translationtype: Human Translation
ms.sourcegitcommit: 50d1f8dfc7ce2d8bf4b04cff68698eafd56ce9c2
ms.openlocfilehash: 518a7d2b5e43d8133d54ab583e4545a3ee9b3cba
ms.lasthandoff: 05/05/2017


---
# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Data Science für Einsteiger, 1. Video: Die 5 Fragen, die Data Science beantwortet
In den fünf kurzen Videos der Reihe *Data Science für Einsteiger* von einem Data Science-Experten erhalten Sie eine kurze Einführung in Data Science. In diesen Videos werden zwar nur die Grundlagen behandelt, sie sind aber hilfreich, wenn Sie sich selbst mit Data Science beschäftigen möchten oder mit Data Scientists arbeiten.

In diesem ersten Video geht es um die Arten von Fragen, die Data Science beantworten kann. Die Reihe bietet den größten Nutzen, wenn Sie sich alle Videos ansehen. [Zur Liste der Videos wechseln](#other-videos-in-this-series)

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere Videos in dieser Reihe
In *Data Science für Einsteiger* erhalten Sie eine ca. 25 Minuten dauernde Schnelleinführung in Data Science. Sehen Sie sich die anderen vier Videos an:

* 1. Video: Die 5 Fragen, die Data Science beantwortet
* 2. Video: [Sind Ihre Daten für Data Science bereit?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4:56 Min.)*
* Video 3: [Stellen einer Frage, die Sie mit Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4:17 Min.)*
* Video 4: [Vorhersagen einer Antwort mit einem einfachen Modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7:42 Min.)*
* 5. Video: [Kopieren der Arbeit anderer für Ihre Data Science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3:18 Min.)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Aufzeichnung: Die 5 Fragen, die Data Science beantwortet
Hallo! Willkommen bei der Videoreihe *Data Science für Einsteiger*.

Data Science kann einschüchternd sein, weshalb ich hier die Grundlagen ohne jegliche Gleichungen oder Fachausdrücke von Programmierern vorstelle.

In diesem ersten Video befassen wir uns mit den „5 Fragen, die Data Science beantwortet“.

Data Science verwendet Zahlen und Namen (auch „Kategorien“ oder „Bezeichnungen“ genannt), um Antworten auf Fragen vorherzusagen.

Es mag Sie vielleicht überraschen, doch *es gibt nur fünf Fragen, die Data Science beantwortet*:

* Ist dies A oder B?
* Ist dies merkwürdig?
* Wie viel – oder – wie viele?
* Wie ist dies organisiert?
* Was soll ich als Nächstes tun?

  Jede dieser Fragen wird mithilfe einer eigenen Gruppe von Machine Learning-Methoden beantwortet, die Algorithmen genannt werden.

Es ist hilfreich, sich einen Algorithmus als Rezept und Ihre Daten als die Zutaten vorzustellen. Ein Algorithmus gibt vor, wie die Daten zu kombinieren und zu mischen sind, um eine Antwort zu erhalten. Computer fungieren dabei als ein Mixer. Sie übernehmen einen Großteil der harten Arbeit des Algorithmus für Sie, und zwar ziemlich schnell.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Für die 1. Frage „Ist dies A oder B?“ werden Klassifizierungsalgorithmen verwendet.
Lassen Sie uns mit der Frage beginnen: Ist dies A oder B?

![Klassifizierungsalgorithmen: Ist dies A oder B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Diese Gruppe von Algorithmen wird als Klassifizierung mit zwei Klassen bezeichnet.

Sie empfiehlt sich für Fragen, die nur zwei mögliche Antworten haben.

Beispiel:

* Übersteht dieser Reifen die nächsten 1.500 km: ja oder nein?
* Was bringt mehr Kunden: ein 5-€-Gutschein oder ein Rabatt von 25 %?

Diese Frage kann auch so umformuliert werden, dass es mehr als zwei Optionen gibt: Ist dies A oder B oder C oder D usw.?  Dies wird als Multiklassenklassifizierung bezeichnet und ist nützlich, wenn es mehrere (oder mehrere tausend) mögliche Antworten gibt. Die Multiklassenklassifizierung wählt die wahrscheinlichste Antwort.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Für die 2. Frage „Ist dies merkwürdig?“ werden Algorithmen zur Erkennung von Anomalien verwendet.
Die nächste Frage, die Data Science beantworten kann, lautet: Ist dies merkwürdig? Diese Frage wird mithilfe einer Gruppe von Algorithmen zur Erkennung von Anomalien beantwortet.

![Algorithmen zur Erkennung von Anomalien: Ist dies merkwürdig?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)

Wenn Sie eine Kreditkarte haben, haben Sie bereits von der Erkennung von Anomalien profitiert. Ihr Kreditkartenunternehmen analysiert Ihre Einkaufsmuster, damit es Sie bei einem möglichen Betrug warnen kann. „Merkwürdige“ Abrechnungspositionen sind z.B. ein Kauf in einem Geschäft, in dem Sie normalerweise nicht einkaufen, oder der Kauf eines ungewöhnlich teuren Artikels.

Diese Frage kann auf viele Weisen nützlich sein. Beispiel:

* Wenn Sie ein Auto mit Druckprüfern haben, möchten Sie ggf. wissen: Ist dieser Druckprüfermesswert normal?
* Wenn Sie das Internet überwachen, möchten Sie vielleicht wissen: Ist diese Nachricht aus dem Internet typisch?

Die Erkennung von Anomalien dient zum Kennzeichnen unerwarteter oder ungewöhnlicher Ereignisse oder Verhaltensweisen. Sie gibt Hinweise, wo nach Problemen zu suchen ist.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Für die 3. Frage „Wie viel? oder Wie viele?“ wird ein Regressionsalgorithmus verwendet.
Machine Learning kann auch die Antwort auf die Fragen „Wie viel?“ oder „Wie viele?“ vorhersagen. Die Gruppe von Algorithmen, die diese Frage beantwortet, wird als Regression bezeichnet.

![Regressionsalgorithmus: Wie viel? oder Wie viele?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)

Regressionsalgorithmen treffen numerische Vorhersagen, wie z.B.:

* Wie wird die Temperatur nächsten Dienstag sein?  
* Wie hoch wird mein Umsatz im vierten Quartal sein?

Diese Algorithmen helfen beim Beantworten von Fragen nach einer Zahl.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Für die 4. Frage „Wie wird dies organisiert?“ werden Clusteringalgorithmen verwendet.
Die letzten beiden Fragen sind allerdings etwas komplexer.

Mitunter möchten Sie die Struktur eines Datasets verstehen, d.h. wie es organisiert ist. Bei dieser Frage gibt es keine Beispiele, für die Sie bereits Ergebnisse kennen.

Es gibt viele Möglichkeiten, die Struktur von Daten zu ermitteln. Ein Ansatz ist das Clustering. Es dient zum Trennen von Daten in natürliche „Haufen“ zur Vereinfachung der Interpretation. Beim Clustering gibt es nicht die eine richtige Antwort.

![Clusteringalgorithmen: Wie ist dies organisiert?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Gängige Beispiele für Clusteringfragen sind u.a.:

* Welche Besucher mögen dieselben Arten von Filmen?
* Welche Druckermodelle fallen auf dieselbe Weise aus?

Indem Sie verstehen, wie Daten organisiert sind, können Sie Verhalten und Ereignisse besser verstehen und vorhersagen.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Für die 5. Frage „Was soll ich jetzt tun?“ werden Algorithmen für vertiefendes Lernen verwendet.
Für die letzte Frage „Was soll ich jetzt tun?“ wird eine Gruppe von Algorithmen mit der Bezeichnung „Vertiefendes Lernen“ verwendet.

Vertiefendes Lernen wurde dadurch inspiriert, wie die Gehirne von Ratten und Menschen auf Bestrafung und Belohnungen reagieren. Diese Algorithmen lernen von Ergebnissen und entscheiden sich für die nächste Aktion.

Vertiefendes Lernen ist in der Regel gut für automatisierte Systeme geeignet, die viele kleine Entscheidungen ohne menschliche Anleitung treffen müssen.

![Algorithmen für vertiefendes Lernen: Was soll ich als Nächstes tun?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Fragen, die beantwortet werden, drehen sich stets darum, welche Aktion ausgeführt werden sollte, meist von einer Maschine oder einem Roboter. Beispiele:

* Wenn ich ein Temperaturregelsystem für ein Haus bin: Die Temperatur anpassen oder unverändert lassen?  
* Wenn ich ein selbstfahrendes Auto bin: Bei Gelb bremsen oder beschleunigen?  
* Für einen Staubsauger-Roboter: Weiter saugen oder zur Ladestation zurückkehren?

Algorithmen für vertiefendes Lernen sammeln Daten im laufenden Betrieb und lernen durch systematisches Ausprobieren.

Das sind sie also: Die 5 Fragen, die Data Science beantworten kann.

## <a name="next-steps"></a>Nächste Schritte
* [Durchführen eines ersten Data Science-Experiments mit Machine Learning Studio](machine-learning-create-experiment.md)
* [Einführung in Machine Learning in Microsoft Azure](machine-learning-what-is-machine-learning.md)

