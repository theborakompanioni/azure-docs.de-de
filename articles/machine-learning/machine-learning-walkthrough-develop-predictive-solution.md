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
ms.date: 02/15/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: b652c0f817e5e56d4ff50701345b03db634f616c
ms.openlocfilehash: 043c54094f53ae539c833eb8f167201781c677a6
ms.lasthandoff: 02/17/2017


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Exemplarische Vorgehensweise: Entwickeln einer Lösung zur Vorhersageanalyse für die Kreditrisikobewertung in Azure Machine Learning

In dieser exemplarischen Vorgehensweise befassen wir uns eingehend mit dem Prozess der Entwicklung einer Predictive Analytics-Lösung in Machine Learning Studio. Wir entwickeln ein einfaches Modell in Machine Learning Studio und stellen es anschließend im Machine Learning-Webdienst bereit, in dem das Modell mithilfe neuer Daten Vorhersagen erstellen kann. 

Voraussetzung in dieser exemplarischen Vorgehensweise ist, dass Sie Machine Learning Studio bereits mindestens einmal verwendet haben und über einige Kenntnisse von Machine Learning-Konzepten verfügen. Es wird aber nicht davon ausgegangen, dass Sie Experte in diesen Bereichen sind.

Wenn Sie **Azure Machine Learning Studio** noch nie verwendet haben, sollten Sie mit dem Tutorial [Erstellen Ihres ersten Data Science-Experiments in Azure Machine Learning Studio](machine-learning-create-experiment.md) beginnen. In diesem Tutorial wird Machine Learning Studio zum ersten Mal Schritt für Schritt für Sie beschrieben. Es vermittelt die Grundlagen, wie Sie Module per Drag & Drop in Ihr Experiment aufnehmen, sie miteinander verbinden, das Experiment ausführen und die Ergebnisse anzeigen. Ein anderes Tool, das für die ersten Schritte nützlich sein kann, ist ein Diagramm mit einer Übersicht über die Funktionen von Machine Learning Studio. Sie können es hier herunterladen und drucken: [Übersichtsdiagramm der Azure Machine Learning Studio-Funktionen](machine-learning-studio-overview-diagram.md).
 
Falls Sie mit dem Thema Machine Learning noch nicht vertraut sind, können Sie sich eine Videoreihe dazu ansehen. Sie hat den Namen [Data Science für Einsteiger](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) und kann als gute Einführung in den Machine Learning-Bereich mit einfacher Sprache und einfachen Begriffen dienen.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>Die Problematik

Stellen Sie sich vor, Sie müssen das Kreditrisiko von Personen anhand der Daten auf einem Kreditantrag vorhersagen.  

Die Bewertung des Kreditrisikos ist ein komplexes Problem, das wir für diese exemplarische Vorgehensweise daher vereinfacht haben. Diese Aufgabenstellung dient als Beispiel dafür, wie Sie eine Predictive Analytics-Lösung mit Microsoft Azure Machine Learning erstellen können. Wir nutzen hierfür Azure Machine Learning Studio und einen Machine Learning-Webdienst.  

## <a name="the-solution"></a>Die Lösung

In dieser exemplarischen Vorgehensweise beginnen wir mit öffentlich verfügbaren Daten zum Kreditrisiko und entwickeln und trainieren basierend auf diesen Daten ein Vorhersagemodell. Anschließend stellen wir das Modell als Webdienst bereit, damit es von anderen Personen für die Bewertung des Kreditrisikos verwendet werden kann.

Wir führen diese Schritte aus, um eine Lösung zur Bewertung des Kreditrisikos zu erstellen:  

1. [Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)
3. [Erstellen eines Experiments](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Eine Arbeitskopie des Experiments, das wir in dieser exemplarischen Vorgehensweise entwickeln, finden Sie in der [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Navigieren Sie zu **[Walkthrough – Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**, und klicken Sie auf **Open in Studio**, um eine Kopie des Experiments in Ihren Machine Learning Studio-Arbeitsbereich herunterzuladen.
> 
> Diese exemplarische Vorgehensweise basiert auf einer vereinfachten Version des Beispielexperiments [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), das auch in der [Gallery](http://gallery.cortanaintelligence.com/) verfügbar ist.

