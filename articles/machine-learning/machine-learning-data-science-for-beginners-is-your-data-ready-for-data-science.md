---
title: "Sind Ihre Daten für Data Science bereit? Datenbewertung – Azure | Microsoft-Dokumentation"
description: "Lernen Sie die vier Kriterien zum Prüfen der Bereitschaft Ihrer Daten für Data Science kennen. Das 2. Video von „Data Science für Einsteiger“ bietet konkrete Beispiele für eine grundlegende Bewertung von Daten."
keywords: relevante Daten,Bewerten von Daten,Vorbereiten von Daten,Kriterien von Daten,vorbereitete Daten
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2017
ms.author: cgronlun;garye
translationtype: Human Translation
ms.sourcegitcommit: b167b88d227bf239fd5d3676e48e04436f140c34
ms.openlocfilehash: 3d2b259c29a52394ac42afba0c5572364cfeea39
ms.lasthandoff: 02/10/2017


---
# <a name="is-your-data-ready-for-data-science"></a>Sind Ihre Daten für Data Science bereit?
## <a name="video-2-data-science-for-beginners-series"></a>2. Video der Reihe „Data Science für Einsteiger“
Erfahren Sie, wie Sie Ihre Daten bewerten können, um sicherzustellen, dass sie die Basiskriterien erfüllen und für Data Science bereit sind.

Die Reihe bietet den größten Nutzen, wenn Sie sich alle Videos ansehen. [Zur Liste der Videos wechseln](#other-videos-in-this-series)

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere Videos in dieser Reihe
*Data Science für Einsteiger* erhalten Sie eine Schnelleinführung in Data Science.

* Video 1: [Die 5 Fragen, die Data Science beantwortet](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5:14 Min.)*
* Video 2: Sind Ihre Daten für Data Science bereit?
* Video 3: [Stellen einer Frage, die Sie mit Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4:17 Min.)*
* Video 4: [Vorhersagen einer Antwort mit einem einfachen Modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7:42 Min.)*
* Video 5: [Kopieren der Arbeit anderer für Ihre Data Science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3:18 Min.)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Aufzeichnung: Sind Ihre Daten für Data Science bereit?
Willkommen bei „Sind Ihre Daten für Data Science bereit?“, dem zweiten Video der Reihe *Data Science für Einsteiger*.  

Bevor Sie sich mit Data Science die gewünschten Antworten verschaffen können, müssen Sie Rohdatenmaterial hoher Qualität zum Verarbeiten zur Verfügung stellen. Es ist wie beim Backen einer Pizza: je besser die Ausgangszutaten, desto besser das Endprodukt.

## <a name="criteria-for-data"></a>Kriterien für Daten
Im Fall von Data Science gibt es also verschiedene Zutaten, die wir zusammenbringen müssen.

Wir benötigen Daten mit folgenden Kriterien:

* Relevanz
* Verbundenheit
* Genauigkeit
* Ausreichende Menge für die Verarbeitung

## <a name="is-your-data-relevant"></a>Sind Ihre Daten relevant?
Die erste Zutat sind also Daten, die relevant sind.

![Relevante Daten im Vergleich mit nicht relevanten Daten: Bewerten von Daten](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Sehen Sie sich die Tabelle auf der linken Seite an. Hier sehen wir den Blutalkoholspiegel, den wir bei sieben Personen vor der Tür von Bars in Boston gemessen haben, die Durchschnittsleistung des Schlägers der Baseballmannschaft Boston Red Sox im letzten Spiel und den Milchpreis im Mini-Markt um die Ecke.

Dies sind alles ganz seriöse Daten. Das einzige Problem ist, dass sie nicht relevant sind. Denn es gibt keine offensichtliche Beziehung zwischen diesen Werten. Wenn ich Ihnen den aktuellen Milchpreis und die Durchschnittsleistung des Schlägers der Red Sox nenne, gibt es keine Möglichkeit, wie Sie meinen Blutalkoholgehalt erraten können.

Sehen wir uns nun die Tabelle auf der rechten Seite an. Dieses Mal haben wir die Körpermasse jeder Person gemessen und die Anzahl ihrer konsumierten Drinks gezählt.  Die Werte in jeder Zeile sind nun relevant zueinander. Wenn ich Ihnen meine Körpermasse und die Anzahl der Margaritas nenne, die ich intus habe, können Sie meinen Alkoholpegel erraten.

## <a name="do-you-have-connected-data"></a>Sind Ihre Daten verbunden?
Die nächste Zutat sind verbundene Daten.

![Verbundene Daten im Vergleich mit nicht verbundenen Daten: Kriterien von Daten, Daten bereit](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Hier einige relevante Daten zur Qualität von Hamburgern: Grilltemperatur, Fleischgewicht und Bewertung im lokalen Gastroführer. Beachten Sie jedoch die Lücken in der Tabelle auf der linken Seite.

Bei den meisten Datasets fehlen einige Werte. Solche Lücken sind durchaus üblich, und es gibt Möglichkeiten, sie zu umgehen. Doch wenn zu viel fehlt, fangen Ihre Daten an, wie ein Schweizer Käse auszusehen.

In der Tabelle links sehen Sie, dass sehr viele Daten fehlen, weshalb es schwierig ist, irgendeine Beziehung zwischen Grilltemperatur und Fleischgewicht herzustellen. Dies ist ein Beispiel für nicht verbundene Daten.

Die Tabelle auf der rechten Seite ist dagegen vollständig und deshalb ein Beispiel für verbundene Daten.

## <a name="is-your-data-accurate"></a>Sind Ihre Daten genau?
Die nächste benötigte Zutat ist Genauigkeit. Hier sind vier Ziele, die wir mit Pfeilen treffen möchten.

![Genaue Daten im Vergleich mit ungenauen Daten: Kriterien von Daten](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Sehen Sie sich das Ziel rechts oben an. Wir sehen eine enge Gruppierung ganz in der Nähe der Zielscheibenmitte. Das ist natürlich genau. Seltsamerweise wird in der Data Science-Sprache unsere Treffleistung beim Ziel direkt darunter auch als genau bezeichnet.

Wenn Sie die Mitte dieser Pfeile abbilden müssten, sähen Sie, dass sich diese sehr nahe bei der Zielscheibenmitte befindet. Die Pfeile sind rund um das Ziel verteilt, weshalb sie als ungenau betrachtet werden. Sie sind jedoch gleichmäßig um die Zielscheibenmitte verteilt, weshalb sie als genau gelten.

Sehen Sie sich nun das Ziel links oben an. Hier sind unsere Pfeil nah nebeneinander eingeschlagen, eine enge Gruppierung. Sie sind zwar präzise, aber ungenau, da das Zentrum weit von der Zielscheibenmitte entfernt ist. Und selbstredend sind die Pfeile im Ziel links unten sowohl ungenau als auch unpräzise. Dieser Bogenschütze braucht mehr Übung.

## <a name="do-you-have-enough-data-to-work-with"></a>Haben Sie genügend Daten, mit denen Sie arbeiten können?
Die vierte Zutat ist schließlich eine ausreichend große Datenmenge.

![Haben Sie genügend Daten für die Analyse? Bewertung von Daten](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Stellen Sie sich jeden Datenpunkt in Ihrer Tabelle als einen Pinselstrich auf einem Gemälde vor. Wenn es nur wenige davon gibt, kann das Gemälde ziemlich unscharf sein, sodass man nicht erkennt, was es ist.

Wenn Sie weitere Striche hinzufügen, beginnt Ihr Bild, etwas schärfer zu werden.

Wenn gerade genügend Striche vorhanden sind, können Sie in etwa ausreichend viel erkennen, um einige allgemeine Entscheidungen treffen zu können. Handelt es sich um etwas, dass ich möglicherweise besuchen möchte? Es sieht hell aus, wie klares Wasser, ja, dahin möchte in Urlaub fahren.

Je mehr Daten Sie hinzufügen, umso klarer wird Ihr Bild, sodass Sie detaillierte Entscheidungen treffen können. Jetzt kann ich die drei Hotels am linken Ufer erkennen. Mir gefallen besonders die architektonischen Merkmale des Hotels im Vordergrund. Da möchte ich hin, in den dritten Stock.

Mit Daten, die relevant, verbunden, genau und in ausreichender Menge vorhanden sind, verfügen wir über alle Zutaten für Data Science in hoher Qualität.

Sehen Sie sich unbedingt auch die anderen vier Videos in der Reihe *Data Science für Einsteiger* von Microsoft Azure Machine Learning an.

## <a name="next-steps"></a>Nächste Schritte
* [Durchführen eines ersten Data Science-Experiments mit Machine Learning Studio](machine-learning-create-experiment.md)
* [Einführung in Machine Learning in Microsoft Azure](machine-learning-what-is-machine-learning.md)

