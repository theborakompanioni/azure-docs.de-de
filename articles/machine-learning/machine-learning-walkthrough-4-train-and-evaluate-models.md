---
title: 'Schritt 4: Trainieren und Auswerten des Predictive Analytics-Modells | Microsoft Docs'
description: "Exemplarische Vorgehensweise zum Entwickeln einer Vorhersagelösung – Schritt 4: Trainieren, Bewerten und Auswerten mehrerer Modelle in Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: a9ebbbdc431a34553de04e920efbbc8c2496ce5f
ms.openlocfilehash: 1ef11386d9040c3929546ea05cd15237defd8a28
ms.lasthandoff: 02/11/2017


---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Exemplarische Vorgehensweise, Schritt 4: Trainieren und Auswerten des Predictive Analytics-Modells
Dieses Thema enthält den vierten Schritt der exemplarischen Vorgehensweise zum [Entwickeln einer Predictive Analytics-Lösung in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)
3. [Erstellen eines neuen Experiments](machine-learning-walkthrough-3-create-new-experiment.md)
4. **Trainieren und Bewerten der Modelle**
5. [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

- - -
Einer der Vorteile von Azure Machine Learning Studio zum Erstellen von Machine Learning-Modellen ist die Möglichkeit, mehr als einen Typ von Modell gleichzeitig in einem einzelnen Experiment zu testen und die Ergebnisse zu vergleichen. Dieser Typ von Experiment hilft Ihnen, die beste Lösung für Ihr Problem zu finden.

In dem Experiment, das wir in dieser exemplarischen Vorgehensweise entwickeln, werden zwei verschiedene Modelltypen erstellt und dann deren Bewertungsergebnisse verglichen, um zu entscheiden, welcher Algorithmus im endgültigen Experiment verwendet werden soll.  

Es stehen verschiedene Modelle zur Auswahl. Um die verfügbaren Modelle anzuzeigen, erweitern Sie den Knoten **Machine Learning** in der Modulpalette, und erweitern Sie dann **Initialize Model** und die darunter liegenden Knoten. Für dieses Experiment wählen wir die Module [Two-Class Support Vector Machine][two-class-support-vector-machine] (SVM) und [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].    

> [!TIP]
> Hilfe für die Entscheidung, welcher Machine Learning-Algorithmus für das Lösen des jeweiligen Problems am besten geeignet ist, finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
> 
> 

## <a name="train-the-models"></a>Trainieren der Modelle

Wir fügen diesem Experiment sowohl das Modul [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] als auch das Modul [Two-Class Support Vector Machine][two-class-support-vector-machine] hinzu.

### <a name="two-class-boosted-decision-tree"></a>Two-Class Boosted Decision Tree

Zuerst wird das Boosted Decision Tree-Modell eingerichtet.

1. Suchen Sie das Modul [Two-Class Boosted Decision Trees][two-class-boosted-decision-tree] in der Modulpalette, und ziehen Sie es in den Experimentbereich.

2. Suchen Sie das Modul [Train Model][train-model], ziehen Sie es in den Bereich, und verbinden Sie die Ausgabe des Moduls [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] mit dem linken Eingabeport des Moduls [Train Model][train-model].
   
   Das Modul [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] initialisiert das allgemeine Modell. Das Modul [Train Model][train-model] nutzt Trainingsdaten zum Trainieren des Modells. 

3. Verbinden Sie die linke Ausgabe des linken Moduls [Execute R Script][execute-r-script] mit dem rechten Eingabeport des Moduls [Train Model][train-model] (wir haben uns entschlossen, in [Schritt 3](machine-learning-walkthrough-3-create-new-experiment.md) dieser exemplarischen Vorgehensweise die Daten von der linken Seite des Moduls „Split Data“ für das Training zu verwenden).
   
   > [!TIP]
   > Zwei der Eingaben und eine der Ausgaben des Moduls [Execute R Script][execute-r-script] werden für dieses Experiment nicht benötigt und daher nicht angefügt. 
   > 
   > 

Dieser Teil des Experiments sieht jetzt in etwa wie folgt aus:  

![Trainieren eines Modells][1]

Nun müssen wir das Modul [Train Model][train-model] informieren, dass das Modell den Wert des Kreditrisikos vorhersagen soll.

1. Wählen Sie das Modul [Train Model][train-model] aus. Klicken Sie im Bereich **Properties** auf **Launch column selector**.

2. Geben Sie im Dialogfeld **Select a single column** in das Suchfeld unter **Available Columns** „Credit risk“ ein. Wählen Sie unten „Credit risk“ aus, und klicken Sie auf den nach rechts zeigenden Pfeil (**>**), um „Credit risk“ in **Selected Columns** zu verschieben. 

    ![Wählen Sie die Spalte „Credit Risk“ des Moduls „Train Model“ aus][0]

3. Klicken Sie auf das Häkchen **OK**.

### <a name="two-class-support-vector-machine"></a>Two-Class Support Vector Machine

Als Nächstes wird das SVM-Modell eingerichtet.  

Zunächst soll SVM ein wenig erläutert werden. Boosted Decision Trees funktionieren hervorragend mit allen Arten von Merkmalen. Da das SVM-Modul jedoch einen linearen Klassifikator generiert, hat das entsprechende generierte Modell den besten Testfehler, wenn alle numerischen Merkmale dieselbe Skala verwenden. Um alle numerischen Features zur gleichen Skala zu konvertieren, verwenden wir das Modul [Normalize Data][normalize-data] mit einer Tanh-Transformation. Diese transformiert unsere Zahlen in den Bereich [0,1]. Zeichenfolgenfeatures werden vom SVM-Modul in kategorische Features und anschließend in binäre 0/1-Features konvertiert. Daher müssen wir Zeichenfolgenfeatures nicht manuell transformieren. Zudem darf die Spalte "Credit Risk" (Spalte 21) nicht transformiert werden. Diese ist zwar numerisch, da es sich dabei aber um den Wert handelt, für dessen Vorhersage das Modell trainiert wird, darf er nicht verändert werden.  

Um das SVM-Modell einzurichten, führen Sie folgende Schritte aus:

1. Suchen Sie das Modul [Two-Class Support Vector Machine][two-class-support-vector-machine] in der Modulpalette, und ziehen Sie es in den Experimentbereich.

2. Klicken Sie mit der rechten Maustaste auf das Modul [Train Model][train-model], und wählen Sie **Copy** aus. Klicken Sie anschließend mit der rechten Maustaste auf die Canvas, und wählen Sie **Paste** aus. Beachten Sie, dass die Kopie des Moduls [Train Model][train-model] die gleiche Spaltenauswahl wie das Original hat.

3. Verbinden Sie die Ausgabe des Moduls [Two-Class Support Vector Machine][two-class-support-vector-machine] mit dem linken Eingabeport des zweiten Moduls [Train Model][train-model].

4. Suchen Sie das Modul [Normalize Data][normalize-data], und ziehen Sie es in den Experimentbereich.

5. Verbinden Sie die Eingabe dieses Moduls mit der linken Ausgabe des linken Moduls [Execute R Script][execute-r-script] (beachten Sie, dass der Ausgabeport eines Moduls mit mehr als einem anderen Modul verbunden sein kann).

6. Verbinden Sie den linken Ausgabeport des Moduls [Normalize Data][normalize-data] mit dem rechten Eingabeport des Moduls [Train Model][train-model].

Dieser Teil des Experiments sieht jetzt in etwa wie folgt aus:  

![Trainieren des zweiten Modells][2]  

Konfigurieren Sie jetzt das Modul [Normalize Data][normalize-data]:

1. Klicken Sie, um das Modul [Normalize Data][normalize-data] auszuwählen. Wählen Sie im Bereich **Properties** den Wert **Tanh** für den Parameter **Transformation method** aus.

2. Klicken Sie auf **Launch column selector**, wählen Sie für **Begin With** „No columns“, und wählen Sie in der ersten Dropdownliste **Include**, in der zweiten Dropdownliste **column type** und in der dritten Dropdownliste **Numeric** aus. Damit wird festgelegt, dass alle numerischen Spalten (und nur die numerischen Spalten) transformiert werden.

3. Klicken Sie auf das Pluszeichen (+) rechts neben dieser Zeile. Dadurch wird eine neue Zeile mit Dropdownlisten erstellt. Wählen Sie in der ersten Dropdownliste **Exclude**aus. Wählen Sie in der zweiten Dropdownliste **column names** aus, und geben Sie „Credit risk“ in das Textfeld ein. Dies gibt an, dass die Spalte „Credit Risk“ ignoriert werden soll (wir müssen dies tun, da diese Spalte numerisch ist und transformiert würde, wenn wir sie nicht ausschließen würden).

4. Klicken Sie auf das Häkchen **OK**.  

    ![Wählen von Spalten für das Modul „Normalize Data“][5]

Das Modul [Normalize Data][normalize-data] ist jetzt für eine Tanh-Transformation aller numerischen Spalten mit Ausnahme der Spalte „Credit Risk“ eingerichtet.  

## <a name="score-and-evaluate-the-models"></a>Bewerten und Auswerten der Modelle

Wir verwenden die Testdaten, die durch das Modul [Split Data][split] getrennt wurden, um die trainierten Modelle zu bewerten. Danach können die Ergebnisse der beiden Modelle verglichen werden, um festzustellen, welches bessere Ergebnisse erbrachte.  

### <a name="add-the-score-model-modules"></a>Hinzufügen der „Score Model“-Module

1. Suchen Sie das Modul [Score Model][score-model], und ziehen Sie es in den Bereich.

2. Verbinden Sie das Modul [Train Model][train-model], das mit dem Modul [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] verbunden ist, mit dem linken Eingabeport des Moduls [Score Model][score-model].

3. Verbinden Sie das rechte Modul [Execute R Script][execute-r-script] (unsere Testdaten) mit dem rechten Eingabeport des Moduls [Score Model][score-model].

    ![Hergestellte Verbindung mit dem Modul „Score Model“][6]
   
   Das Modul [Score Model][score-model] kann nun die Kreditinformationen aus den Testdaten entnehmen, sie durch das Modell laufen lassen und vom Modell generierte Vorhersagen mit der Spalte mit dem tatsächlichen Kreditrisiko in den Testdaten vergleichen.

4. Kopieren Sie das Modul [Score Model][score-model], und fügen Sie es ein, um eine zweite Kopie zu erstellen.

5. Verbinden Sie die Ausgabe des SVM-Modells (d.h. den Ausgabeport des Moduls [Train Model][train-model], der mit dem Modul [Two-Class Support Vector Machine][two-class-support-vector-machine] verbunden ist) mit dem Eingabeport des zweiten Moduls [Score Model][score-model].

6. Für das SVM-Modell muss die gleiche Transformation für die Testdaten durchgeführt werden wie für die Trainingsdaten. Kopieren Sie also das Modul [Normalize Data][normalize-data], und fügen Sie es ein, um eine zweite Kopie zu erstellen, und verbinden Sie es mit dem rechten Modul [Execute R Script][execute-r-script].

7. Verbinden Sie die linke Ausgabe des zweiten Moduls [Normalize Data][normalize-data] mit dem rechten Eingabeport des zweiten Moduls [Score Model][score-model].

    ![Hergestellte Verbindung mit beiden „Score Model“-Modulen][7]

### <a name="add-the-evaluate-model-module"></a>Hinzufügen des Moduls „Evaluate Model“

Zum Evaluieren und Vergleichen der beiden Bewertungsergebnisse wird das Modul [Evaluate Model][evaluate-model] verwendet.  

1. Suchen Sie das Modul [Evaluate Model][evaluate-model], und ziehen Sie es in den Bereich.

2. Verbinden Sie den Ausgangsport des Moduls [Score Model][score-model], der dem „Boosted Decision Tree“-Modell zugeordnet ist, mit dem linken Eingabeport des Moduls [Evaluate Model][evaluate-model].

3. Verbinden Sie das andere Modul [Score Model][score-model] mit dem rechten Eingabeport.  

    ![Hergestellte Verbindung mit dem Modul „Evaluate Model“][8]

### <a name="run-the-experiment-and-check-the-results"></a>Ausführen des Experiments und Überprüfen der Ergebnisse

Klicken Sie auf die Schaltfläche **RUN** unter dem Bereich, um das Experiment auszuführen. Dies kann einige Minuten dauern. Für jedes Modul wird ein Drehzeiger angezeigt, um anzugeben, dass es ausgeführt wird. Wenn das Modul abgeschlossen ist, wird ein grünes Häkchen angezeigt. Wenn alle Module ein Häkchen aufweisen, ist die Ausführung des Experiments beendet.

Das Experiment sollte in etwa wie folgt aussehen:  

![Evaluieren beider Modelle][3]

Um die Ergebnisse zu prüfen, klicken Sie auf den Ausgabeport des Moduls [Evaluate Model][evaluate-model], und wählen Sie **Visualize** aus.  

Das Modul [Evaluate Model][evaluate-model] erzeugt ein Paar aus Kurven und Metriken, mit denen die Ergebnisse der beiden bewerteten Modelle verglichen werden können. Sie können die Ergebnisse als ROC-Kurven (Receiver Operator Characteristic), Genauigkeits-/Trefferkurven oder Prognosegütekurven anzeigen. Zu den zusätzlich angezeigten Daten zählen eine Wahrheitsmatrix, kumulative Werte für den Bereich unter der Kurve (AUC) sowie weitere Metriken. Sie können den Schwellwert ändern, indem Sie den Schieberegler nach links oder rechts bewegen und beobachten, wie sich dies auf den Metriksatz auswirkt.  

Klicken Sie rechts neben dem Graph auf **Scored dataset** oder auf **Scored dataset to compare**, um die zugeordnete Kurve zu markieren und die zugeordneten Metriken darunter anzuzeigen. In der Legende für die Kurven entspricht „Bewertetes Dataset“ dem linken Eingabeport des Moduls [Evaluate Model][evaluate-model] – in unserem Fall ist dies das Boosted Decision Tree-Modell. "Scored dataset to compare" entspricht dem rechten Eingabeport – in unserem Fall dem SVM-Modell. Wenn Sie auf eine dieser Beschriftungen klicken, werden die Kurve für das betreffende Modell markiert und die entsprechenden Metriken darunter angezeigt (siehe die folgende Abbildung).  

![ROC-Kurven für Modelle][4]

Wenn Sie diese Werte prüfen, können Sie entscheiden, welches Modell am ehesten den gewünschten Ergebnissen entspricht. Sie können zurückgehen und das Experiment erneut ausführen, indem Sie Parameterwerte in den verschiedenen Modellen ändern. 

Die wissenschaftliche Vorgehensweise und Kunst der Interpretation dieser Ergebnisse und das Optimieren der Modellleistung werden in dieser exemplarischen Vorgehensweise nicht behandelt. Weitere Hilfe erhalten Sie in den folgenden Artikeln:
- [Auswerten der Modellleistung in Azure Machine Learning](machine-learning-evaluate-model-performance.md)
- [Auswählen von Parametern zur Optimierung von Algorithmen in Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md)
- [Interpretieren von Modellergebnissen in Azure Machine Learning](machine-learning-interpret-model-results.md)

> [!TIP]
> Jedes Mal, wenn Sie das Experiment ausführen, wird im Ausführungsverlauf ein Dataset für diese Iteration aufbewahrt. Sie können die Iterationen anzeigen und zu jeder von ihnen zurückkehren, indem Sie unter dem Bereich auf **VIEW RUN HISTORY** klicken. Sie können auch im Fenster **Properties** auf **Prior Run** klicken, um zur Iteration unmittelbar vor der geöffneten Iteration zurückzukehren.
> 
> Sie können eine Kopie jeder Iteration des Experiments anfertigen, indem Sie unter dem Bereich auf **SAVE AS** klicken. 
> Verwenden Sie die Eigenschaften **Summary** und **Description** des Experiments, um nachzuhalten, was Sie in den Iterationen Ihres Experiments versucht haben.
> 
> Weitere Informationen finden Sie unter [Verwalten von Experimentiterationen in Azure Machine Learning-Studio](machine-learning-manage-experiment-iterations.md).  
> 
> 

- - -
**Nächster Schritt: [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)**

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

