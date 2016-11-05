---
title: 'Schritt 5: Bereitstellen des Machine Learning-Webdiensts | Microsoft Docs'
description: 'Exemplarische Vorgehensweise zum Entwickeln einer Vorhersagelösung – Schritt 5: Bereitstellen eines Vorhersageexperiments als Webdienst in Machine Learning Studio.'
services: machine-learning
documentationcenter: ''
author: garyericson
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: garye

---
# <a name="walkthrough-step-5:-deploy-the-azure-machine-learning-web-service"></a>Exemplarische Vorgehensweise – Schritt 5: Bereitstellen des Azure Machine Learning-Webdiensts
Dies ist der fünfte Schritt der exemplarischen Vorgehensweise zum [Predictive Analytics-Lösung in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)
3. [Erstellen eines neuen Experiments](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. **Bereitstellen des Webdiensts**
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

- - -
Um anderen die Möglichkeit zu geben, das Vorhersagemodell zu nutzen, welches wir in dieser exemplarischen Vorgehensweise entwickelt haben, stellen wir es in Azure als Webdienst bereit.

Bis jetzt haben Sie mit dem Trainieren des Modells experimentiert. Der bereitgestellte Dienst führt jedoch kein Training mehr durch, sondern generiert Vorhersagen durch Bewerten der Benutzereingaben basierend auf unserem Modell. Wir führen einige vorbereitende Schritte durch, um dieses Experiment von einem ***Trainingsexperiment*** in ein ***Vorhersageexperiment*** umzuwandeln. 

Dies ist ein zweistufiger Prozess:  

1. Konvertieren des von uns erstellten *Trainingsexperiments* in ein *Vorhersageexperiment*
2. Bereitstellen des Vorhersageexperiments als Webdienst

Zunächst muss das Experiment optimiert werden. Zurzeit enthält das Experiment zwei verschiedene Modelle, doch nur eines davon wollen wir als Webdienst bereitstellen.  

In unserem Beispiel entscheiden wir, dass das Boosted Tree-Modell sich besser zur Verwendung eignet. Als Erstes müssen Sie nun das Modul [Two-Class Support Vector Machine][two-class-support-vector-machine] sowie die Module entfernen, mit denen dieses Modul trainiert wurde. Am besten erstellen Sie zunächst eine Kopie des Experiments, indem Sie unten in der Canvas des Experiments auf **Save As** klicken.

Folgende Module müssen gelöscht werden:  

* [Two-Class Support Vector Machine][two-class-support-vector-machine]
* Die damit verbundenen Module [Train Model][train-model] und [Score Model][score-model]
* [Normalize Data][normalize-data] (beide)
* [Evaluate Model][evaluate-model]

Wählen Sie das Modul aus, und drücken Sie die ENTF-Taste, oder klicken Sie mit der rechten Maustaste auf das Modul, und wählen Sie **Delete** aus.

Jetzt sind wir bereit, dieses Modell mit dem [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] bereitzustellen.

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Konvertieren des Trainingsexperiments in ein Vorhersageexperiment
Die Konvertierung in ein Vorhersageexperiment umfasst drei Schritte:

1. Speichern des trainierten Modells und Ersetzen der Trainingsmodule
2. Optimieren des Experiments, um Module zu entfernen, die nur zu Trainingszwecken benötigt wurden
3. Festlegen, wo der Webdienst Eingaben akzeptiert und wo er Ausgaben generiert

Glücklicherweise können alle drei Schritte ausgeführt werden, indem Sie einfach unten im Experimentbereich auf **Set Up Web Service** klicken (wählen Sie die Option **Predictive Web Service**).

Beim Klicken auf **Set Up Web Service** geschehen mehrere Dinge:

* Das trainierte Modell wird als Modul **Trained Model** in der Modulpalette gespeichert, die sich links neben dem Experimentbereich befindet. (Sie finden sie unter **Trained Models**.)
* Module, die zum Training verwendet wurden, werden entfernt. Dies gilt insbesondere in folgenden Fällen:
  * [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]
  * [Train Model][train-model]
  * [Split Data][split]
  * Das zweite [Execute R Script][execute-r-script]-Modul, das für die Testdaten verwendet wurde
* Das gespeicherte trainierte Modell wird wieder dem Experiment hinzugefügt.
* Die Module **Web service input** und **Web service output** werden hinzugefügt.

> [!NOTE]
> Das Experiment wurde in zwei Teilen unter Registerkarten gespeichert, die oben im Experimentbereich hinzugefügt wurden: das ursprüngliche Trainingsexperiment befindet sich unter der Registerkarte **Training experiment**, und das neu erstellte Vorhersageexperiment finden Sie unter **Predictive experiment**.
> 
> 

Bei diesem bestimmten Experiment ist ein zusätzlicher Schritt erforderlich.
Wir haben zwei [Execute R Script][execute-r-script]-Module hinzugefügt, um die Daten für Training und Tests mit einer Gewichtungsfunktion zu versehen. Im endgültigen Modell ist dies nicht erforderlich.

Machine Learning Studio hat beim Entfernen des [Split][split]-Moduls eines der [Execute R Script][execute-r-script]-Module entfernt. Nun können wir auch das andere entfernen und den [Metadata Editor][metadata-editor] direkt mit dem Modul [Score Model][score-model] verbinden.    

Unser Experiment sollte nun wie folgt aussehen:  

![Bewerten des trainierten Modells][4]  

> [!NOTE]
> Sie fragen sich vielleicht, warum der Datensatz „UCI German Credit Card Data“ im Vorhersageexperiment verblieben ist. Der Dienst verwendet die Daten des Benutzers, nicht das Originaldataset. Weshalb also das Originaldataset im Modell belassen?
> 
> Es stimmt zwar, dass der Dienst die Originalkreditkartendaten nicht benötigt. Er benötigt aber das Schema für diese Daten, darunter Angaben zur Anzahl der vorhandenen Spalten, und welche Spalten numerisch sind. Diese Schemainformationen sind erforderlich, um die Benutzerdaten zu interpretieren. Wir lassen diese Komponenten verbunden, damit das Bewertungsmodul über das Datasetschema verfügt, wenn der Dienst ausgeführt wird. Es werden nicht die Daten verwendet, sondern nur das Schema.  
> 
> 

Führen Sie das Experiment ein letztes Mal aus (klicken Sie auf **Run**). Wenn Sie überprüfen möchten, ob das Modell noch funktioniert, klicken Sie mit der rechten Maustaste auf die Ausgabe des Moduls [Score Model][score-model] und wählen **View Results**. Sie sehen, dass die Originaldaten zusammen mit dem Wert für das Kreditrisiko („Bewertete Beschriftungen“)' und dem Bewertungswahrscheinlichkeitswert („Bewertete Wahrscheinlichkeiten“) angezeigt werden. 

## <a name="deploy-the-web-service"></a>Bereitstellen des Webdiensts
Sie können das Experiment entweder als klassischen Webdienst oder neuen auf Azure Resource Manager basierenden Webdienst bereitstellen.

### <a name="deploy-as-a-classic-web-service"></a>Bereitstellen als klassischen Webdienst
Zum Bereitstellen eines aus dem Experiment abgeleiteten klassischen Webdiensts klicken Sie unter dem Experimentbereich auf **Deploy Web Service** und wählen dann **Deploy Web Service [Classic]**. Machine Learning Studio stellt das Experiment als Webdienst bereit und führt Sie zum Dashboard dieses Webdiensts. Von hier aus können Sie zum Experiment zurückkehren (**View snapshot** oder **View latest)** und einen einfachen Test des Webdiensts ausführen (siehe **Testen des Webdiensts** weiter unten). Dort befinden sich auch Informationen zum Erstellen von Anwendungen, die auf den Webdienst zugreifen können (mehr dazu im nächsten Schritt dieser exemplarischen Vorgehensweise).

![Dashboard des Webdiensts][6]

Sie können den Dienst konfigurieren, indem Sie auf die Registerkarte **KONFIGURATION** klicken. Hier können Sie den Dienstnamen ändern (er erhält standardmäßig den Namen des Experiments) und eine Beschreibung hinzufügen. Sie können auch benutzerfreundlichere Beschriftungen für die Eingabe- und Ausgabespalten festlegen.  

![Konfigurieren des Webdiensts][5]  

### <a name="deploy-as-a-new-web-service"></a>Bereitstellen als neuer Webdienst
Zum Bereitstellen eines aus dem Experiment abgeleiteten neuen Webdiensts klicken Sie unter dem Experimentbereich auf **Deploy Web Service** und wählen dann **Deploy Web Service [New]**. Machine Learning Studio leitet Sie zur Seite „Azure Machine Learning Web Services Deploy Experiment“ weiter.

Geben Sie einen Namen für den Webdienst ein, und wählen Sie einen Preisplan. Wenn Sie bereits einen Preisplan gewählt haben, können Sie diesen auswählen. Andernfalls müssen Sie einen Preisplan für den Dienst wählen. 

1. Wählen Sie in der Dropdownliste **Price Plan** einen Preisplan aus, oder wählen Sie die Option **Select new plan** aus.
2. Geben Sie in **Plan Name** einen Namen ein, der den Plan auf Ihrer Rechnung bestimmt.
3. Wählen Sie in **Monthly Plan Tiers** eine Option aus. Die Plantarife sind standardmäßig auf die Pläne für Ihre Standardregion und Ihren Webdienst festgelegt, der in dieser Region bereitgestellt ist.

Klicken Sie auf **Deploy**. Die Seite **Quickstart** für Ihren Webdienst wird geöffnet.

Sie können den Dienst konfigurieren, indem Sie auf die Menüoption **Configure** klicken. Hier können Sie den Dienstnamen ändern (er erhält standardmäßig den Namen des Experiments) und eine Beschreibung hinzufügen. 

Um den Webdienst zu testen, klicken Sie auf die Menüoption **Test** (siehe **Testen des Webdiensts** unten). Informationen zum Erstellen von Anwendungen, die auf den Webdienst zugreifen können, erhalten Sie durch Klicken auf die Menüoption **Consume** (mehr dazu im nächsten Schritt dieser exemplarischen Vorgehensweise).

> [!TIP]
> Sie können den Webdienst nach der Bereitstellung aktualisieren. Wenn Sie das Modell ändern möchten, bearbeiten Sie einfach die Modellparameter im Trainingsexperiment und klicken auf **Deploy Web Service**. Wählen Sie dann **Deploy Web Service [Classic]** oder **Deploy Web Service [New]**. Wenn Sie das Experiment erneut bereitstellen, wird der Webdienst ersetzt und das aktualisierte Modell verwendet.  
> 
> 

## <a name="test-the-web-service"></a>Testen des Webdiensts
### <a name="test-a-classic-web-service"></a>Testen eines klassischen Webdiensts
Sie können den Dienst in Machine Learning Studio oder im Azure Machine Learning Web Services-Portal testen. Das Testen im Azure Machine Learning Web Services-Portal bietet mehrere Vorteile. 

**Testen in Machine Learning Studio**

Klicken Sie auf der Seite **DASHBOARD** unter **Default Endpoint** auf die Schaltfläche **Test**. Ein Dialogfeld wird geöffnet, in dem Sie nach den Eingabedaten für den Dienst gefragt werden. Dies sind die gleichen Spalten, die im Originaldataset „German Credit Risk“ angezeigt wurden.  

Geben Sie eine Reihe von Daten ein, und klicken Sie auf **OK**. 

**Testen im Azure Machine Learning Web Services-Portal**

Klicken Sie auf der Seite **DASHBOARD** unter **Standardendpunkt** auf den Link **Test**. Die Testseite im Azure Machine Learning Web Services-Portal für den Webdienst-Endpunkt wird geöffnet und fordert Sie zur Angabe der Eingabedaten für den Dienst auf. Dies sind die gleichen Spalten, die im Originaldataset „German Credit Risk“ angezeigt wurden.

Klicken Sie auf **Test Request-Response**. 

Im Webdienst durchlaufen die Daten das Modul **Web service input**, dann das Modul [Metadata Editor][metadata-editor] und schließlich das Modul [Score Model][score-model], in dem sie bewertet werden. Die Ergebnisse werden danach vom Webdienst über das Modul **Web service output** ausgegeben.

**Testen eines neuen Webdiensts**

Klicken Sie im Azure Machine Learning Web Services-Portal oben auf der Seite auf **Test**. Die Seite **Test** wird geöffnet, auf der Sie Daten für den Dienst eingeben können. Die angezeigten Eingabefelder entsprechen den Spalten im Originaldataset „German Credit Risk“. 

Geben Sie eine Reihe von Daten ein, und klicken Sie auf **Test Request-Response**.

Die Ergebnisse des Tests werden rechts auf der Seite in der Ausgabespalte angezeigt. 

Beim Testen im Azure Machine Learning Web Services-Portal können Sie Beispieldaten aktivieren, mit deren Hilfe Sie den Request-Response-Dienst testen können. Wenn Sie den Webdienst in Machine Learning Studio erstellt haben, stammen die Beispieldaten aus den Daten, mit denen Sie Ihr Modell trainiert haben.

> [!TIP]
> So wie wir das Vorhersageexperiment konfiguriert haben, gibt das Modul [Score Model][score-model] sämtliche Ergebnisse zurück. Dies schließt alle Eingabedaten sowie den Kreditrisikowert und die Bewertungswahrscheinlichkeit ein. Wenn Sie etwas anderes zurückgeben möchten, z.B. nur den Kreditrisikowert, können Sie ein Modul des Typs [Project Columns][project-columns] zwischen [Score Model][score-model] und **Web service output** einfügen, um Spalten auszuschließen, die der Webdienst nicht zurückgeben soll. 
> 
> 

## <a name="manage-the-web-service"></a>Verwalten des Webdiensts
**Verwalten eines klassischen Webdiensts**

Nachdem Sie Ihren klassischen Webdienst bereitgestellt haben, können Sie ihn im [klassischen Azure-Portal](https://manage.windowsazure.com)verwalten.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie im Microsoft Azure-Dienstbereich auf **MACHINE LEARNING**.
3. Klicken Sie auf Ihren Arbeitsbereich.
4. Klicken Sie auf die Registerkarte **Web Services**.
5. Klicken Sie auf den Webdienst, den wir erstellt haben.
6. Klicken Sie auf den Endpunkt „Default“.

Hier können Sie überwachen, wie der Webdienst funktioniert, und Leistungsanpassungen vornehmen, indem Sie ändern, wie viele gleichzeitige Aufrufe der Dienst unterstützen kann.
Sie können sogar Ihren Webdienst in Azure Marketplace veröffentlichen.

Weitere Informationen finden Sie unter:

* [Erstellen von Endpunkten](machine-learning-create-endpoint.md)
* [Skalieren von Webdiensten](machine-learning-scaling-webservice.md)
* [Veröffentlichen eines Azure Machine Learning-Webdiensts in Azure Marketplace](machine-learning-publish-web-service-to-azure-marketplace.md)

**Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal**

Nachdem Sie Ihren neuen oder klassischen Webdienst bereitgestellt haben, können Sie ihn im [Azure Machine Learning Web Services-Portal](https://servics.azureml.net) verwalten.

So überwachen Sie die Leistung des Webdiensts

1. Melden Sie sich beim [Azure Machine Learning Web Services-Portal](https://servics.azureml.net) an.
2. Klicken Sie auf **Web Services**.
3. Klicken Sie auf Ihren Webdienst.
4. Klicken Sie auf das **Dashboard**.

- - -
**Nächster Schritt: [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/


<!--HONumber=Oct16_HO2-->


