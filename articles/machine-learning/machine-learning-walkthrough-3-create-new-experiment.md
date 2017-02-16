---
title: 'Schritt 3: Erstellen eines neuen Machine Learning-Experiments | Microsoft Docs'
description: "Exemplarische Vorgehensweise zum Entwickeln einer Vorhersagelösung – Schritt 3: Erstellen eines neuen Trainingsexperiments in Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: bd4a38e74ecab47071631f7e67e99c7806abd935
ms.openlocfilehash: e8f1d55dd374608b49d4189fb47603a6ddaee3dd


---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Exemplarische Vorgehensweise, Schritt 3: Erstellen eines neuen Azure Machine Learning-Experiments
Dies ist der dritte Schritt der exemplarischen Vorgehensweise zum [Entwickeln einer Predictive Analytics-Lösung in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)
3. **Erstellen eines neuen Experiments**
4. [Trainieren und Bewerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

- - -
Der nächste Schritt in dieser exemplarischen Vorgehensweise ist die Erstellung eines Experiments in Machine Learning Studio, welches das von uns hochgeladene Dataset verwendet.  

1. Klicken Sie in Studio unten auf der Seite auf **+NEW** .
2. Wählen Sie **EXPERIMENT**und anschließend "Blank Experiment" aus. 

    ![Erstellen eines neuen Experiments][0]

2. Wählen Sie oben in der Canvas den Namen des Standardexperiments aus, und geben Sie einen aussagekräftigen Namen ein.

    ![Umbenennen von Experimenten][5]
   
   > [!TIP]
   > Es ist eine bewährte Methode, die Felder **Summary** und **Description** für das Experiment im Bereich **Properties** auszufüllen. Diese Eigenschaften geben Ihnen die Möglichkeit, das Experiment zu dokumentieren. Wenn Benutzer später darauf zugreifen, können sie Ihre Ziele und die Methodik verstehen.
   > 
   > ![Eigenschaften von Experimenten][6]
   > 
3. Erweitern Sie in der Modulpalette links vom Experimentbereich **Gespeicherte Datasets**.
4. Suchen Sie das Dataset, das Sie unter **Meine Datasets** erstellt haben, und ziehen Sie es in den Bereich. Sie können auch nach dem Dataset suchen, indem Sie den Namen in das Feld **Suchen** oberhalb der Palette eingeben.  

    ![Hinzufügen des Datasets zum Experiment][7]

## <a name="prepare-the-data"></a>Vorbereiten der Daten
Sie können die ersten 100 Datenzeilen sowie einige statistische Informationen für das ganze Dataset anzeigen: Klicken Sie hierzu auf den Ausgabeport des Datasets (den kleinen Kreis unten), und wählen Sie die Option **Visualize**.  

Da die Datendatei keine Spaltenüberschriften aufweist, hat Studio allgemeine Überschriften (Col1, Col2 *usw.*) bereitgestellt. Aussagekräftige Überschriften haben keine Bedeutung für die Erstellung eines Modells, erleichtern aber die Arbeit mit den Daten im Experiment. Wenn später das Modell in einem Webdienst veröffentlicht wird, kann der Benutzer des Diensts die Spalten anhand der Überschriften auch leichter identifizieren.  

Verwenden Sie das Modul [Edit Metadata][edit-metadata], um Spaltenüberschriften hinzuzufügen.
Sie verwenden das Modul [Edit Metadata][edit-metadata] zum Ändern der Metadaten, die einem Dataset zugeordnet sind. In diesem Fall nutzen wir es, um aussagekräftigere Namen für die Spaltenüberschriften anzugeben. 

Zum Verwenden von [Edit Metadata][edit-metadata] müssen Sie zuerst die zu ändernden Spalten angeben (in diesem Fall alle). Als Nächstes geben Sie die Aktion an, die auf diese Spalten angewendet werden soll (in diesem Fall das Ändern der Spaltenüberschriften).

1. Geben Sie in der Modulpalette „Metadaten“ in das Feld **Suchen** ein. [Edit Metadata][edit-metadata] wird in der Modulliste angezeigt.

2. Klicken und ziehen Sie das Modul [Edit Metadata][edit-metadata] in den Experimentbereich, und legen Sie es unter dem zuvor hinzugefügten Dataset ab.

3. Verbinden Sie das Dataset mit [Edit Metadata][edit-metadata]: Klicken Sie auf den Ausgabeport des Datasets (den kleinen Kreis unten im Dataset), ziehen Sie ihn zum Eingabeport von [Edit Metadata][edit-metadata] (den kleinen Kreis oben im Modul), und lassen Sie die Maustaste los. Das Dataset und das Modul bleiben verbunden, auch wenn Sie diese im Bereich verschieben.
   
   Das Experiment sollte in etwa wie folgt aussehen:  
   
   ![Hinzufügen von „Edit Metadata“][1]
   
   Das rote Ausrufezeichen gibt an, dass wir noch nicht die Eigenschaften für dieses Modul festgelegt haben. Das machen wir als Nächstes.
   
   > [!TIP]
   > Sie können einen Kommentar zu einem Modul eingeben, indem Sie auf das Modul doppelklicken und Text eingeben. Auf diese Weise können Sie mit einem Blick sehen, welche Funktion das Modul in Ihrem Experiment erfüllt. Doppelklicken Sie hierfür auf das Modul [Edit Metadata][edit-metadata], und geben Sie den Kommentar „Spaltenüberschriften hinzufügen“ ein. Klicken Sie auf eine Stelle des Experimentbereichs, um das Textfeld zu schließen. Klicken Sie auf den nach unten zeigenden Pfeil für das Modul, um den Kommentar anzuzeigen.
   > 
   > ![Modul „Edit Metadata“ mit hinzugefügtem Kommentar][8]
   > 
4. Klicken Sie bei ausgewähltem Modul [Edit Metadata][edit-metadata] im Bereich **Eigenschaften** rechts neben dem Experimentbereich auf **Launch column selector**.

5. Wählen Sie im Dialogfeld **Select columns** in **Available Columns** alle Zeilen aus, und klicken Sie auf „>“, um sie in **Selected Columns** zu verschieben.
   Das Dialogfeld sollte wie folgt aussehen:

   ![Spaltenauswahl, alle Spalten ausgewählt][2]

6. Klicken Sie auf das Häkchen **OK**.

7. Suchen Sie im Bereich **Properties** nach dem Parameter **New column name**. Geben Sie in diesem Feld eine Liste der Namen für die 21 Spalten im Dataset ein, durch Kommas getrennt und in der Reihenfolge der Spalten. Die Spaltennamen können Sie der Datasetdokumentation auf der UCI-Website entnehmen, oder Sie können einfach die folgende Liste kopieren und einfügen:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Der Bereich „Eigenschaften“ sieht wie folgt aus:
   
   ![Eigenschaften für „Edit Metadata“][3]

> [!TIP]
> Wenn Sie die Spaltenüberschriften überprüfen möchten, führen Sie das Experiment aus (klicken Sie unter dem Experimentbereich auf **RUN** ). Wenn die Ausführung abgeschlossen ist (in [Edit Metadata][edit-metadata] wird ein grünes Häkchen angezeigt), klicken Sie auf den Ausgabeport des Moduls [Edit Metadata][edit-metadata], und wählen Sie **Visualize**. Auf die gleiche Weise können Sie die Ausgabe jedes anderen Moduls anzeigen, um den Datenfortschritt im Experiment zu sehen.
> 
> 

## <a name="create-training-and-test-datasets"></a>Erstellen von Trainings- und Testdatasets
Im nächsten Schritt des Experiments wird das Dataset in zwei separate Datasets aufgeteilt. Wir verwenden ein Dataset zum Trainieren des Modells und das andere zum Testen.

Hierfür wird das Modul [Split Data][split] verwendet.  

1. Wechseln Sie zum Modul [Split Data][split], ziehen Sie es in den Experimentbereich, und verbinden Sie es mit dem Modul [Edit Metadata][edit-metadata].

2. Standardmäßig beträgt das Aufteilungsverhältnis 0,5, und der Parameter **Zufällige Aufteilung** ist festgelegt. Dies bedeutet, dass eine zufällig ausgewählte Hälfte der Daten über einen Port des Moduls [Split Data][split] und die andere Hälfte über den anderen Port ausgegeben wird. Sie können diese Parameter und den Parameter **Random seed** anpassen, um die Aufteilung zwischen Trainings- und Bewertungsdaten zu ändern. Für dieses Beispiel bleiben die Werte unverändert.
   
   > [!TIP]
   > Die Eigenschaft **Fraction of rows in the first output dataset** bestimmt, welcher Anteil der Daten über den linken Ausgabeport ausgegeben werden. Wenn Sie z. B. ein Aufteilungsverhältnis von 0,7 festlegen, werden 70 % der Daten über den linken Port und 30 % der Daten über den rechten Port ausgegeben.  
   > 
   > 

3. Doppelklicken Sie auf das Modul [Split Data][split], und geben Sie den Kommentar „Aufteilung zwischen Trainings- und Testdaten 50 %“ ein. 

Die Ausgaben des Moduls [Split Data][split] können beliebig verwendet werden. In diesem Fall wählen wir die linke Ausgabe als Trainingsdaten und die rechte Ausgabe als Testdaten.  

Wie bereits erwähnt, sind die Kosten einer Fehlklassifizierung eines hohen Risikos als niedriges Risiko fünf Mal höher als die Kosten einer Fehlklassifizierung eines niedrigen Risikos als hohes Risiko. Um dies zu berücksichtigen, generieren Sie ein neues Dataset, das diese Kostenfunktion darstellt. Im neuen Dataset wird jedes Hochrisikobeispiel fünfmal repliziert, während keines der Niedrigrisikobeispiele repliziert wird.   

Für diese Replikation kann der R-Code verwendet werden:  

1. Suchen Sie nach dem Modul [Execute R Script][execute-r-script], und ziehen Sie es in den Experimentbereich. 

2. Verbinden Sie den linken Ausgabeport des Moduls [Split Data][split] mit dem ersten Eingabeport („Dataset1“) des Moduls [Execute R Script][execute-r-script].

3. Doppelklicken Sie auf das Modul [Execute R Script][execute-r-script], und geben Sie den Kommentar „Kostenanpassung festlegen“ ein.

4. Löschen Sie im Bereich mit den **Eigenschaften** den Standardtext im Parameter **R Script**, und geben Sie dieses Skript ein:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-Skript im Modul „Execute R Script“][9]

Wir müssen den gleichen Replikationsvorgang für jede Ausgabe des Moduls [Split Data][split] durchführen, damit die Trainings- und die Testdaten die gleiche Kostenanpassung haben. Hierfür duplizieren wir das eben erstellte Modul [Execute R Script][execute-r-script] und verbinden es mit dem anderen Ausgabeport des Moduls [Split Data][split].

1. Klicken Sie mit der rechten Maustaste auf das Modul [Execute R Script][execute-r-script], und wählen Sie die Option **Kopieren**.

2. Klicken Sie mit der rechten Maustaste in den Experimentbereich, und wählen Sie **Einfügen**aus.

3. Ziehen Sie das neue Modul an die richtige Position, und verbinden Sie den rechten Ausgabeport des Moduls [Split Data][split] dann mit dem ersten Eingabeport des neuen Moduls [Execute R Script][execute-r-script]. 

4. Klicken Sie am unteren Rand des Experimentbereichs auf **Run**. 

> [!TIP]
> Die Kopie des Moduls "Execute R Script" enthält das gleiche Skript wie das Originalmodul. Wenn Sie ein Modul kopieren und im Bereich einfügen, behält die Kopie alle Eigenschaften des Originals bei.  
> 
> 

Unser Experiment sieht nun in etwa wie folgt aus:

![Hinzufügen des Aufteilungsmoduls und der R-Skripte][4]

Weitere Informationen zum Verwenden von R-Skripts in Ihren Experimenten finden Sie unter [Erweitern Sie Ihr Experiment mit R](machine-learning-extend-your-experiment-with-r.md).

**Nächster Schritt: [Trainieren und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/



<!--HONumber=Dec16_HO3-->


