---
title: "Eine Vorhersagelösung für die Kreditrisikobewertung mit Machine Learning| Microsoft Docs"
description: "Eine ausführliche exemplarische Vorgehensweise zum Erstellen einer Lösung für die Vorhersageanalyse zur Kreditrisikobewertung in Azure Machine Learning Studio."
keywords: "Kreditrisiko, Predictive Analytics-Lösung, Risikobewertung"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 07cb3fe0d5688d5b63fe3312cad14c2274a58a09
ms.openlocfilehash: e98a64910f28da0a8a9b4a58c717c40d791ccf00


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Exemplarische Vorgehensweise: Entwickeln einer Lösung zur Vorhersageanalyse für die Kreditrisikobewertung in Azure Machine Learning

In dieser exemplarischen Vorgehensweise befassen wir uns eingehend mit dem Prozess der Entwicklung einer Lösung in Machine Learning Studio. Wir entwickeln ein Predictive Analytics-Modell in Machine Learning Studio und stellen es anschließend im Machine Learning-Webdienst bereit, in dem das Modell mithilfe neuer Daten Vorhersagen erstellen kann. 

> [!TIP]
> Voraussetzung in dieser exemplarischen Vorgehensweise ist, dass Sie Machine Learning Studio bereits mindestens einmal verwendet haben und über einige Kenntnisse von Machine Learning-Konzepten verfügen; es wird jedoch nicht davon ausgegangen, dass Sie Experte in diesen Bereichen sind.
> 
>Wenn Sie **Azure Machine Learning Studio** noch nie verwendet haben, sollten Sie mit dem Tutorial [Erstellen Ihres ersten Data Science-Experiments in Azure Machine Learning Studio](machine-learning-create-experiment.md) beginnen. Dieses Tutorial führt Sie durch die ersten Schritte mit Machine Learning Studio und zeigt Ihnen die Grundlagen darüber, wie Sie Module per Drag & Drop in Ihr Experiment aufnehmen, sie miteinander verbinden, das Experiment ausführen und die Ergebnisse anzeigen.
>
>Wenn dies Ihre erste Berührung mit maschinellem Lernen ist, stellt die Videoreihe [Data Science für Einsteiger](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) möglicherweise einen guten Start dar. Diese Videoreihe bietet eine gute Einführung in das maschinelle Lernen und verwendet alltägliche Sprache und Konzepte.
> 

## <a name="the-problem"></a>Die Problematik

Stellen Sie sich vor, Sie müssen das Kreditrisiko von Personen anhand der Daten auf einem Kreditantrag vorhersagen.  

Kreditrisikobewertung ist natürlich ein komplexes Problem, wir vereinfachen jedoch, die Parameter der Fragestellung ein wenig. Wir werden dies als Beispiel dafür verwenden, wie Sie Microsoft Azure Machine Learning mit Machine Learning Studio und dem Machine Learning-Webdienst zum Erstellen einer solchen Predictive Analytics-Lösung verwenden können.  

## <a name="the-solution"></a>Die Lösung

In dieser detaillierten exemplarischen Vorgehensweise beginnen wir mit öffentlich verfügbaren Kreditrisikodaten, entwickeln und trainieren ein Vorhersagemodell anhand dieser Daten und stellen das Modell als Webdienst bereit, den andere Benutzer für die Kreditrisikobewertung verwenden können.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Um eine Lösung zur Kreditrisikobewertung zu erstellen, müssen wir die folgenden Schritte ausführen:  

1. [Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)
3. [Erstellen eines neuen Experiments](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

Diese exemplarische Vorgehensweise basiert auf einer vereinfachten Version des Beispielexperiments [Binäre Klassifizierung: Kreditrisiko-Vorhersage](http://go.microsoft.com/fwlink/?LinkID=525270) im [Cortana Intelligence-Katalog](http://gallery.cortanaintelligence.com/).


> [!TIP]
> Informationen zum Herunterladen und Drucken des Diagramms, mit dem Sie sich einen Überblick über die Machine Learning Studio-Funktionen verschaffen können, finden Sie unter [Übersichtsdiagramm der Azure Machine Learning Studio-Funktionen](machine-learning-studio-overview-diagram.md).
> 
> 



<!--HONumber=Jan17_HO1-->


