<properties
    pageTitle="Verwenden von Beispielexperimenten zum Erstellen neuer Experimente | Microsoft Azure"
    description="Erstellen neuer Azure Machine Learning-Experimente anhand von Beispielexperimenten und Experimenten, die von der Community freigegeben wurden."
    services="machine-learning"
    documentationCenter=""
    authors="cjgronlund"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/28/2016"
    ms.author="chhavib;olgali"/>

# Verwenden von Beispielexperimenten zum Erstellen neuer Experimente
Wenn Sie ein Experiment erstellen möchten, finden Sie unter [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) (Cortana Intelligence-Katalog) Beispiele. Dort finden Sie Beispielexperimente des Machine Learning-Teams sowie von der Machine Learning-Community. Sie können zudem auch Fragen stellen oder Anmerkungen zu Experimenten veröffentlichen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Durchsuchen des Cortana Intelligence-Katalogs
Klicken Sie im [Katalog](http://gallery.cortanaintelligence.com/) oben auf der Seite auf **Experiments** (Experimente), um anzuzeigen, welche Experimente im Katalog vorhanden sind. Auf dieser Seite können Sie auch Experimente der Kategorien **Recently added** (vor Kurzem hinzugefügt), **What‘s popular** (Beliebte Experimente) oder die neuesten **Popular Microsoft experiments** (beliebte Microsoft-Experimente) anzeigen.

So durchsuchen Sie alle Experimente:

1. Klicken Sie oben auf der Seite auf **Browse all** (Alle durchsuchen).
2. Wählen Sie unter **Refine by** (Einschränken nach) die Option **Experiment** (Experiment) aus, um alle im Katalog enthaltenen Experimente anzuzeigen.
3. Sie können die Liste weiter einschränken, indem Sie auf der linken Seite Filter auswählen. Wenn Sie beispielsweise Experimente durchsuchen möchten, die einen Algorithmus zur PCA-basierten Erkennung von Anomalien verwenden, wählen Sie unter **Categories** (Kategorien) die Option **Experiment** (Experiment) und unter **Algorithms Used** (Verwendete Algorithmen) die Option **PCA-Based Anomaly Detection** (PCA-basierte Erkennung von Anomalien) aus. (Wenn Ihnen dieser Algorithmus nicht angezeigt wird, klicken Sie unten in der Liste auf **Show all** (Alle anzeigen).)<br></br> ![](./media/machine-learning-sample-experiments/refine-the-view.png) 
4. Mithilfe des Suchfelds können Sie die Auswahl noch weiter eingrenzen. Wenn Sie etwa nach Experimenten von Microsoft zur Ziffernerkennung suchen möchten, bei denen ein zweiklassiger Support Vector Machine-Algorithmus verwendet wird, geben Sie im Suchfeld „digit recognition“ (Ziffernerkennung) ein, und wählen Sie **Experiment** (Experiment), **Microsoft content only** (Nur Inhalte von Microsoft) und **Two-Class Support Vector Machine** (Zweiklassige Support Vector Machine) aus: ![](./media/machine-learning-sample-experiments/search-for-experiments.png) 
5. Klicken Sie auf ein Experiment, um mehr darüber zu erfahren. Zum Ausführen und/oder Ändern des Experiments klicken Sie auf der Seite des Experiments auf **Open in Studio** (In Studio öffnen).

> [AZURE.NOTE] Wenn Sie ein Experiment in Machine Learning Studio öffnen möchten, müssen Sie sich mit Ihren Microsoft-Anmeldeinformationen anmelden. Wenn Sie noch keinen Machine Learning-Arbeitsbereich besitzen, wird ein Arbeitsbereich in der kostenlosen Testversion erstellt. [Informieren Sie sich über den Funktionsumfang der kostenlosen Testversion von Machine Learning](https://azure.microsoft.com/pricing/details/machine-learning/)

![](./media/machine-learning-sample-experiments/example-experiment.png)


## Verwenden einer Vorlage in Machine Learning Studio

Sie können in Machine Learning Studio auch ein neues Experiment erstellen, indem Sie ein Beispiel aus dem Katalog als Vorlage verwenden.

1. Melden Sie sich mit den Anmeldeinformationen für ihr Microsoft-Konto bei [Studio](https://studio.azureml.net) an, und klicken Sie dann auf **New*** (Neu), um ein neues Experiment zu erstellen.
2. Durchsuchen Sie die Beispiele, und wählen Sie eines aus.

In Ihrem Arbeitsbereich wird ein neues Experiment erstellt, bei dem das Beispielexperiment als Vorlage genutzt wird.

## Nächste Schritte
- [Vorbereiten der Daten](machine-learning-data-science-import-data.md)
- [Verwenden von R im Experiment](machine-learning-r-quickstart.md)
- [Überprüfen der R-Beispielexperimente](machine-learning-r-csharp-web-service-examples.md)
- [Erstellen einer Webdienst-API](machine-learning-publish-a-machine-learning-web-service.md)
- [Durchsuchen einsatzbereiter Anwendungen](https://datamarket.azure.com/browse?query=machine+learning)

<!---HONumber=AcomDC_0504_2016-->