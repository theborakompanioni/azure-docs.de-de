---
title: Konvertieren in ein Vorhersageexperiment in Azure Machine Learning| Microsoft-Dokumentation
description: "Konvertieren eines Machine Learning-Trainingsexperiments zum Trainieren Ihres Modells für Vorhersageanalysen in ein Vorhersageexperiment, das als Webdienst bereitgestellt werden kann."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: db91a464843a7c2dc5460f12f7f306972d3a7da8
ms.lasthandoff: 03/22/2017


---
# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Konvertieren eines Machine Learning-Trainingsexperiments in ein Vorhersageexperiment
Azure Machine Learning ermöglicht es Ihnen, Vorhersageanalyselösungen zu erstellen, zu testen und bereitzustellen .

Sobald Sie ein *Trainingsexperiment* zum Trainieren Ihres Vorhersageanalysemodells erstellt und durchlaufen haben und bereit sind, dieses zum Bewerten neuer Daten zu verwenden, müssen Sie das Experiment vorbereiten und für die Bewertung optimieren. Sie können dieses *Vorhersageexperiment* dann als Azure-Webdienst operationalisieren, damit Benutzer Daten an Ihr Modell senden und dessen Vorhersagen abrufen können.

Durch die Konvertierung in ein Vorhersageexperiment wird das trainierte Modell darauf vorbereitet, als Webdienst bereitgestellt zu werden. Benutzer des Webdiensts senden Eingabedaten an das Modell, und das Modell sendet Vorhersageergebnisse zurück. Während der Konvertierung in ein Vorhersageexperiment sollten Sie daher berücksichtigen, wie das Modell aus Ihrer Sicht erwartungsgemäß von anderen Benutzern verwendet wird.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Der Prozess der Konvertierung eines Trainingsexperiments in ein Vorhersageexperiment umfasst drei Schritte:

1. Speichern Sie das Machine Learning-Modell, das Sie trainiert haben, und ersetzen Sie dann den Machine Learning-Algorithmus und die [Train Model][train-model]-Module durch Ihr gespeichertes trainiertes Modell.
2. Reduzieren des Experiments auf die Module, die für die Bewertung erforderlich sind. Ein Trainingsexperiment enthält mehrere Module, die für das Training erforderlich sind, jedoch nicht mehr benötigt werden, nachdem das Modell entsprechend trainiert wurde und bereit für die Bewertung ist.
3. Definieren Sie, wo Sie im Experiment Daten des Webdienst-Benutzers akzeptieren und welche Daten zurückgegeben werden.

## <a name="set-up-web-service-button"></a>Schaltfläche zur Webdiensteinrichtung
Nachdem Sie das Experiment ausgeführt haben (mit der Schaltfläche **RUN** am unteren Rand der Experimentbereichs), führen Sie mit der Schaltfläche **Set Up Web Service** (wählen Sie die Option **Predictive Web Service**) die drei Schritte zum Konvertieren Ihres Trainingsexperiment in ein Vorhersageexperiment durch:

1. Dabei wird das trainierte Modell als Modul im Abschnitt **Trained Models** der Modulpalette (links neben dem Experimentbereich) gespeichert, und anschließend werden der Machine Learning-Algorithmus und die [Train Model][train-model]-Module durch das gespeicherte Modell ersetzt.
2. Module, die offensichtlich nicht erforderlich sind, werden entfernt. In unserem Beispiel gehören dazu die Module [Split Data][split], zweites<sup></sup> [Score Model][score-model] und [Evaluate Model][evaluate-model].
3. Webdiensteingabe- und -ausgabemodule werden erstellt und in den Standardpositionen Ihres Experiments eingefügt.

Das folgende Experiment trainiert z. B. ein Boosted Decision Tree-Zwei-Klassen-Modell mit Beispiel-Erhebungsdaten:

![Trainingsexperiment][figure1]

Die Module in diesem Experiment führen im Wesentlichen vier verschiedene Funktionen aus:

![Modulfunktionen][figure2]

Wenn Sie dieses Trainingsexperiment in ein Vorhersageexperiment konvertieren, werden einige dieser Module nicht mehr benötigt oder erhalten einen anderen Zweck:

* **Data**: Die Daten in diesem Beispieldataset werden nicht während der Bewertung verwendet. Der Benutzer des Webdiensts stellt die Daten für die Bewertung bereit. Die Metadaten aus dem DataSet, z. B. Datentypen, werden jedoch vom trainierten Modell verwendet. Daher müssen Sie das DataSet im Vorhersageexperiment beibehalten, damit diese Metadaten bereitgestellt werden können.
* **Prep** – Abhängig von den zum Bewerten übermittelten Daten können diese Module zum Verarbeiten der eingehenden Daten erforderlich sein oder auch nicht.
  
    In diesem Beispiel können etwa im Beispiel-DataSet Werte fehlen und Spalten enthalten sein, die zum Trainieren des Modells nicht benötigt werden. Das Modul [Clean Missing Data][clean-missing-data] wurde zur Behandlung fehlender Werte hinzugefügt, und das Modul [Select Columns in Dataset][select-columns] wurde eingeschlossen, um diese zusätzlichen Spalten vom Datenfluss auszuschließen. Wenn Sie wissen, dass in den zum Bewerten durch den Webdienst übermittelten Daten keine Werte fehlen, können Sie das Modul [Clean Missing Data][clean-missing-data] entfernen. Da das Modul [Select Columns in Dataset][select-columns] beim Festlegen der bewerteten Features hilft, muss dieses Modul beibehalten werden.
* **Train** – Diese Module werden zum Trainieren des Modells verwendet. Wenn Sie auf **Webdienst einrichten** klicken, werden diese Module durch ein einzelnes „Trained Model“-Modul ersetzt. Dieses neue Modul wird im Abschnitt **Trained Models** der Modulpalette gespeichert.
* **Score** – In diesem Beispiel wird das Modul [Split Data][split] zum Unterteilen des Datenstroms in einen Satz von Testdaten und Trainingsdaten verwendet. Da dies im Vorhersageexperiment nicht erforderlich ist, kann das Modul entfernt werden. Ebenso werden das zweite Modul<sup></sup> [Score Model][score-model] und das Modul [Evaluate Model][evaluate-model] verwendet, um Ergebnisse aus den Testdaten zu vergleichen, und werden daher im Vorhersageexperiment ebenfalls nicht benötigt. Das verbleibende Modul [Score Model][score-model] ist jedoch erforderlich, um über den Webdienst ein Bewertungsergebnis zurückzugeben.

So sieht das Beispiel nach dem Klicken auf **Webdienst einrichten**aus:

![Konvertiertes Vorhersageexperiment][figure3]

Das kann ausreichend sein, um das Experiment zur Bereitstellung als Webdienst vorzubereiten. Allerdings sollten Sie einige zusätzliche Aufgaben für das Experiment durchführen.

### <a name="adjust-input-and-output-modules"></a>Anpassen von Eingabe- und Ausgabemodulen
Im Trainingsexperiment haben Sie einen Satz von Trainingsdaten verwendet und dann durch Verarbeitung die Daten in einem Formular abgerufen, die für den Algorithmus für maschinelles Lernen erforderlich sind. Wenn diese Verarbeitung für die Daten, die voraussichtlich über den Webdienst empfangen werden, nicht erforderlich ist, können Sie das Modul **Web Service Input** auf einen anderen Knoten im Experiment verschieben (klicken Sie auf die Ausgabe des Moduls **Web Service Input**, und ziehen Sie sie zum Eingangsport des entsprechenden Moduls).

Beispielsweise setzt **Set Up Web Service** standardmäßig das **Web service input**-Modul an den Beginn des Datenflusses, wie in der Abbildung oben dargestellt. Wenn diese Verarbeitung für die Eingabedaten nicht erforderlich ist, können Sie **Web Service Input** manuell hinter den Datenverarbeitungsmodulen platzieren:

![Verschieben des Web Service Input][figure4]

Die über den Webdienst bereitgestellten Eingabedaten werden jetzt ohne Vorverarbeitung direkt an das "Score Model"-Modul übergeben.

Auf ähnliche Weise setzt **Webdienst einrichten** standardmäßig das Webdienstausgabe-Modul an das Ende des Datenflusses. In diesem Beispiel gibt der Webdienst die Ausgabe des Moduls [Score Model][score-model], die den vollständigen Eingabedatenvektor sowie die Bewertungsergebnisse enthält, an den Benutzer zurück.

Falls Sie jedoch andere Daten zurückgeben möchten, können Sie zusätzliche Module vor dem Modul **Web Service Output** hinzufügen. Wenn Sie beispielsweise nur die Bewertungsergebnisse und nicht den gesamten Eingabedatenvektor zurückgeben möchten, können Sie ein Modul [Select Columns in Dataset][select-columns] hinzufügen, um alle Spalten mit Ausnahme der Bewertungsergebnisse auszuschließen. Anschließend verschieben Sie das Modul **Web Service Output** zur Ausgabe des Moduls [Select Columns in Dataset][select-columns]. Das Experiment würde dann wie folgt aussehen:

![Verschieben des Web Service Output][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Hinzufügen oder Entfernen zusätzlicher Datenverarbeitungsmodule
Wenn in Ihrem Experiment mehrere Module vorhanden sind, von denen Sie wissen, dass sie für die Bewertung nicht benötigt werden, können diese entfernt werden. Da wir das Modul **Web service input** an einen Punkt hinter den Datenverarbeitungsmodulen verschoben haben, können wir beispielsweise das Modul [Clean Missing Data][clean-missing-data] aus dem Vorhersageexperiment entfernen.

Unser Vorhersageexperiment sieht nun folgendermaßen aus:

![Entfernen zusätzlicher Module][figure6]

> [!TIP]
> Beachten Sie, dass wir das Dataset oder das Modul [Select Columns in Dataset][select-columns] nicht entfernt haben. Das Modell im Webdienst verwendet die Daten im ursprünglichen Dataset nicht, jedoch die im Dataset definierten Metadaten, beispielsweise den Datentyp jeder Spalte. Ebenso identifiziert das Modul [Select Columns in Dataset][select-columns] die Datenspalten, die vom Modell verwendet werden. Beide Module müssen daher im Vorhersageexperiment erhalten bleiben.

### <a name="add-optional-web-service-parameters"></a>Hinzufügen von optionalen Webdienst-Parametern
In einigen Fällen empfiehlt es sich, dem Benutzer des Webdiensts das Ändern des Modulverhaltens zu erlauben, wenn auf den Dienst zugegriffen wird. *Webdienstparameter* erlauben Ihnen, dies zu tun.

Ein typisches Beispiel ist das Einrichten eines Moduls [Import Data][import-data], damit die Benutzer des bereitgestellten Webdiensts beim Zugreifen auf den Webdienst eine andere Datenquelle angeben können. Ein weiteres Beispiel ist die Konfiguration eines Moduls [Export Data][export-data], damit ein anderes Ziel angegeben werden kann.

Sie können Webdienstparameter definieren und einem oder mehreren Modulparametern zuordnen, und Sie können angeben, ob diese Parameter erforderlich oder optional sind. Der Benutzer des Webdiensts kann dann beim Zugreifen auf den Dienst Werte für diese Parameter angeben, und das Verhalten des Moduls ändert sich entsprechend.

Weitere Informationen zu Webdienstparametern finden Sie unter [Verwenden von Azure Machine Learning-Webdienstparametern][webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Bereitstellung des Vorhersageexperiments als Webdienst
Nachdem das Vorhersageexperiment ausreichend vorbereitet wurde, können Sie es als Azure-Webdienst bereitstellen. Mithilfe des Webdiensts können Benutzer Daten an das Modell senden, und das Modell gibt seine Vorhersagen zurück.

Weitere Informationen zum vollständigen Bereitstellungsprozess finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts][deploy].

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

