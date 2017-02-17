---
title: Debuggen Ihres Modells in Azure Machine Learning | Microsoft Docs
description: Erfahren Sie, wie Sie Ihr Modell in Azure Machine Learning debuggen.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bradsev;garye
translationtype: Human Translation
ms.sourcegitcommit: 8c2fb72de830a699c96f3593e9f07a871614267b
ms.openlocfilehash: 58e6cc28a624b0d873e3eabeba971cc8e909267a


---
# <a name="debug-your-model-in-azure-machine-learning"></a>Debuggen Ihres Modells in Azure Machine Learning
In diesem Artikel wird erläutert, wie Sie Ihre Modelle in Microsoft Azure Machine Learning debuggen. Er behandelt insbesondere die möglichen Gründe, warum bei der Ausführung eines Modells die folgenden zwei Fehler auftreten können:

* Für das Modul [Train Model][train-model] tritt ein Fehler auf. 
* Das Modul [Score Model][score-model] liefert falsche Ergebnisse. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Für das Modul „Train Model“ tritt ein Fehler auf
![Bild1](./media/machine-learning-debug-models/train_model-1.png)

Für das Modul [Train Model][train-model] werden die folgenden zwei Eingaben erwartet:

1. Der Typ des Klassifizierungs- oder Regressionsmodells aus der in Azure Machine Learning bereitgestellten Modellsammlung
2. Die Trainingsdaten mit einer angegebenen Spalte "Label" Die Spalte "Label" gibt die vorherzusagende Variable an. Bei den restlichen Spalten wird davon ausgegangen, dass es sich um Funktionen handelt.

Für dieses Modul tritt in den folgenden Fällen ein Fehler auf:

1. Die Spalte "Label" wurde falsch angegeben, da entweder als Bezeichner (Label) mehrere Spalten ausgewählt sind oder ein falscher Spaltenindex ausgewählt ist. Der zweite Fall gilt beispielsweise, wenn der Spaltenindex "30" für ein Eingabedataset verwendet wurde, das nur 25 Spalten umfasst.
2. Das Dataset enthält keine Spalten mit Funktionen. Wenn das Eingabedataset beispielsweise nur eine Spalte umfasst, die als Spalte „Label“ gekennzeichnet ist, sind keine Funktionen vorhanden, mit denen das Modell erstellt werden kann. In diesem Fall löst das Modul [Train Model][train-model] einen Fehler aus.
3. Das Eingabedataset (Funktionen oder Bezeichner) enthält „Infinity“ als Wert.

## <a name="score-model-module-does-not-produce-correct-results"></a>"Score Model"-Modul liefert falsche Ergebnisse
![image2](./media/machine-learning-debug-models/train_test-2.png)

In einem typischen Trainings- und Testdiagramm für das beaufsichtigte Lernen teilt das Modul [Split Data][split] das ursprüngliche Dataset in zwei Teile auf: in einen Teil, der zum Trainieren des Modells verwendet wird, und in einen zweiten Teil, mit dem die Leistung des trainierten Modells für Daten bewertet wird, für die es nicht trainiert wurde. Mit dem trainierten Modell werden dann die Testdaten bewertet und anschließend die Ergebnisse evaluiert, um die Genauigkeit des Modells zu bestimmen.

Für das Modul [Score Model][score-model] sind zwei Eingaben erforderlich:

1. Die Ausgabe eines trainierten Modells aus dem [Train Model][train-model]-Modul
2. Ein Dataset für die Bewertung, das sich von dem Dataset unterscheidet, mit dem das Modell trainiert wurde

Es kann vorkommen, dass das [Score Model][score-model]-Modul auch dann falsche Ergebnisse liefert, wenn das Experiment erfolgreich ausgeführt wird. Dies kann durch verschiedene Szenarios verursacht werden:

1. Wenn der angegebene Bezeichner kategorisch ist und ein Regressionsmodell für die Daten trainiert wird, gibt das [Score Model][score-model]-Modul eine falsche Ausgabe aus. Dies ist darauf zurückzuführen, dass für die Regression eine stetige Antwortvariable erforderlich ist. In diesem Fall empfiehlt sich die Verwendung eines Klassifizierungsmodells. 
2. In ähnlicher Weise liefert ein Klassifizierungsmodell möglicherweise unerwünschte Ergebnisse, wenn es für ein Dataset mit Gleitkommazahlen in der Spalte "Label" trainiert wird. Dies liegt daran, dass für die Klassifizierung eine diskrete Antwortvariable erforderlich ist, die nur Werte zulässt, die innerhalb einer begrenzten und normalerweise eher kleinen Gruppe von Klassen liegen.
3. Wenn das Dataset für die Bewertung nicht alle Funktionen enthält, mit denen das Modell trainiert wird, gibt das [Score Model][score-model]-Modul einen Fehler aus.
4. Wenn eine Zeile im Dataset für die Bewertung einen fehlenden Wert oder einen unendlichen Wert für die zugehörigen Funktionen aufweist, gibt das [Score Model][score-model]-Modul keine Ergebnisse für diese Zeile aus.
5. Das [Score Model][score-model]-Modul generiert unter Umständen identische Ausgaben für alle Zeilen im Dataset für die Bewertung. Dies ist beispielsweise möglich, wenn bei der Klassifizierung mithilfe von Decision Forests die ausgewählte minimale Anzahl der Stichproben pro Leaf-Knoten höher als die Anzahl der verfügbaren Trainingsbeispiele ist.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/




<!--HONumber=Feb17_HO3-->


