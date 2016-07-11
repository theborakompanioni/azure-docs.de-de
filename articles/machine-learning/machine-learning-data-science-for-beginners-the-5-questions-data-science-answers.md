<properties
   pageTitle="Die 5 Data Science-Fragen: Data Science für Einsteiger | Microsoft Azure"
   description="Kurze Einführung in Data Science mithilfe der aus fünf Kurzvideos bestehenden Reihe „Data Science für Einsteiger“, die mit „Die 5 Fragen, die Data Science beantwortet“ beginnt."
   keywords="Ausführen von Data Science,Einführung in Data Science,Data Science für Einsteiger,Arten von Fragen,Data Science-Fragen,Data Science-Algorithmen"
   services="machine-learning"
   documentationCenter="na"
   authors="brohrer-ms"
   manager="paulettm"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/24/2016"
   ms.author="cgronlun;brohrer;garye"/>

# Data Science für Einsteiger, 1. Video: Die 5 Fragen, die Data Science beantwortet

In den fünf kurzen Videos der Reihe *Data Science für Einsteiger* erhalten Sie eine kurze Einführung in Data Science. Diese Videoreihe ist hilfreich, wenn Sie an Data Science interessiert sind oder mit Leuten zusammenarbeiten, die sich mit Data Science beschäftigen, und sich mit den grundlegenden Konzepten vertraut machen möchten.

Die Reihe bietet den größten Nutzen, wenn Sie sich die Videos in der vorgegebenen Reihenfolge ansehen. [Zur Liste der Videos wechseln](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## Aufzeichnung: Die 5 Fragen, die Data Science beantwortet

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


Es ist hilfreich, sich einen Algorithmus als Rezept und Ihre Daten als die Zutaten vorzustellen. Ein Algorithmus gibt vor, wie die Daten zu kombinieren und mischen sind, um eine Antwort zu erhalten. Computer fungieren dabei als ein Mixer. Sie übernehmen einen Großteil der harten Arbeit des Algorithmus für Sie, und zwar ziemlich schnell.

## Für die 1. Frage „Ist dies A oder B?“ werden Klassifizierungsalgorithmen verwendet.

Lassen Sie uns mit der Frage beginnen: Ist dies A oder B?

![Klassifizierungsalgorithmen: Ist dies A oder B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Diese Gruppe von Algorithmen wird als Klassifizierung mit zwei Klassen bezeichnet.

Sie empfiehlt sich für Fragen, die nur zwei mögliche Antworten haben.

Beispiel:

  *	Übersteht dieser Reifen die nächsten 1.500 km: Ja oder nein?
  *	Was bringt mehr Kunden: ein 5-€-Gutschein oder ein Rabatt von 25 %?

Diese Frage kann auch so umformuliert werden, dass es mehr als zwei Optionen gibt: Ist dies A oder B oder C oder D usw.? Dies wird als Multiklassenklassifizierung bezeichnet und ist nützlich, wenn es mehrere (oder mehrere tausend) mögliche Antworten gibt. Die Multiklassenklassifizierung wählt die wahrscheinlichste Antwort.

## Für die 2. Frage „Ist dies merkwürdig?“ werden Algorithmen zur Erkennung von Anomalien verwendet.

Die nächste Frage, die Data Science beantworten kann, lautet: Ist dies merkwürdig? Diese Frage wird mithilfe einer Gruppe von Algorithmen zur Erkennung von Anomalien beantwortet.

![Algorithmen zur Erkennung von Anomalien: Ist dies merkwürdig?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Wenn Sie eine Kreditkarte haben, haben Sie bereits von der Erkennung von Anomalien profitiert. Ihr Kreditkartenunternehmen analysiert Ihre Einkaufsmuster, damit es Sie bei einem möglichen Betrug warnen kann. „Merkwürdige“ Abrechnungspositionen sind z.B. ein Kauf in einem Geschäft, in dem Sie normalerweise nicht einkaufen, oder der Kauf eines ungewöhnlich teuren Artikels.

Diese Frage kann auf viele Weisen nützlich sein. Beispiel:

  *	Wenn Sie ein Auto mit Druckprüfern haben, möchten Sie ggf. wissen: Ist dieser Druckprüfermesswert normal?
  *	Wenn Sie das Internet überwachen, möchten Sie vielleicht wissen: Ist diese Nachricht aus dem Internet typisch?

Die Erkennung von Anomalien dient zum Kennzeichnen unerwarteter oder ungewöhnlicher Ereignisse oder Verhaltensweisen. Sie gibt Hinweise, wo nach Problemen zu suchen ist.



## Für die 3. Frage „Wie viel? oder Wie viele?“ wird ein Regressionsalgorithmus verwendet.

Machine Learning kann auch die Antwort auf die Fragen „Wie viel?“ oder „Wie viele?“ vorhersagen. Die Gruppe von Algorithmen, die diese Frage beantwortet, wird als Regression bezeichnet.

![Regressionsalgorithmus: Wie viel? oder Wie viele?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Regressionsalgorithmen treffen numerische Vorhersagen, wie z.B.:

  *	Wie wird die Temperatur nächsten Dienstag sein?
  *	Wie hoch wird mein Umsatz im vierten Quartal sein?

Diese Algorithmen helfen beim Beantworten von Fragen nach einer Zahl.

## Für die 4. Frage „Wie wird dies organisiert?“ werden Clusteringalgorithmen verwendet.

Die letzten beiden Fragen sind allerdings etwas komplexer.

Mitunter möchten Sie die Struktur eines Datasets verstehen, d.h. wie es organisiert ist. Bei dieser Frage gibt es keine Beispiele, für die Sie bereits Ergebnisse kennen.

Es gibt viele Möglichkeiten, die Struktur von Daten zu ermitteln. Ein Ansatz ist das Clustering. Es dient zum Trennen von Daten in natürliche „Haufen“ zur Vereinfachung der Interpretation. Beim Clustering gibt es nicht die eine richtige Antwort.

![Clusteringalgorithmen: Wie ist dies organisiert?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Gängige Beispiele für Clusteringfragen sind u.a.:

  *	Welche Besucher mögen dieselben Arten von Filmen?
  *	Welche Druckermodelle fallen auf dieselbe Weise aus?

Indem Sie verstehen, wie Daten organisiert sind, können Sie Verhalten und Ereignisse besser verstehen und vorhersagen.

## Für die 5. Frage „Was soll ich jetzt tun?“ werden Algorithmen für vertiefendes Lernen verwendet.

Für die letzte Frage „Was soll ich jetzt tun?“ wird eine Gruppe von Algorithmen mit der Bezeichnung „Vertiefendes Lernen“ verwendet.

Vertiefendes Lernen wurde dadurch inspiriert, wie die Gehirne von Ratten und Menschen auf Bestrafung und Belohnungen reagieren. Diese Algorithmen lernen von Ergebnissen und entscheiden sich für die nächste Aktion.

Vertiefendes Lernen ist in der Regel gut für automatisierte Systeme geeignet, die viele kleine Entscheidungen ohne menschliche Anleitung treffen müssen.

![Algorithmen für vertiefendes Lernen: Was soll ich als Nächstes tun?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Fragen, die beantwortet werden, drehen sich stets darum, welche Aktion ausgeführt werden sollte, meist von einer Maschine oder einem Roboter. Beispiele:

  *	Wenn ich ein Temperaturregelsystem für ein Haus bin: Die Temperatur anpassen oder unverändert lassen?
  *	Wenn ich ein selbstfahrendes Auto bin: Bei Gelb bremsen oder beschleunigen?
  *	Für einen Staubsauger-Roboter: Weiter saugen oder zur Ladestation zurückkehren?

Algorithmen für vertiefendes Lernen sammeln Daten im laufenden Betrieb und lernen durch systematisches Ausprobieren.

Das sind sie also: Die 5 Fragen, die Data Science beantworten kann.



## Andere Videos in dieser Reihe

In fünf Kurzvideos der Reihe *Data Science für Einsteiger* erhalten Sie eine Schnelleinführung in Data Science. Sehen Sie sich die anderen vier Videos an:

  * 2\. Video: [Sind Ihre Daten für Data Science bereit?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) **Jetzt verfügbar.**
  * 3\. Video: [Stellen einer Frage, die Sie mit Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md). **Jetzt verfügbar.**
  * 4\. Video: [Vorhersagen einer Antwort mit einem einfachen Modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md). **Jetzt verfügbar.**
  * 5\. Video: [Kopieren der Arbeit anderer für Ihre Data Science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md). Am 30. Juni verfügbar.

## Nächste Schritte

  * [Durchführen Ihres ersten Data Science-Experiments mit Azure Machine Learning](machine-learning-create-experiment.md)
  * [Einführung in Machine Learning in Microsoft Azure](machine-learning-what-is-machine-learning.md)

<!---HONumber=AcomDC_0629_2016-->