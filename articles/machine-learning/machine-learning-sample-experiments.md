---
title: "Kopieren von Machine Learning-Beispielexperimenten – Azure | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Machine Learning-Beispielexperimente verwenden, um mit dem Cortana Intelligence-Katalog und Microsoft Azure Machine Learning neue Experimente zu erstellen.
keywords: Machine learning-Beispiele, Beispielexperiment, Machine Learning-Beispiel
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: cgronlun;olgali
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: a6c8ca83e3a4020cb1ea0f1c14299f1aee3bdca5
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a>Kopieren von Beispielexperimenten zum Erstellen neuer Machine Learning-Experimente
Hier erfahren Sie, wie Sie Beispielexperimente aus dem [Cortana Intelligence-Katalog](https://gallery.cortanaintelligence.com/) verwenden, anstatt Machine Learning-Experimente von Grund auf neu zu erstellen. Auf der Grundlage der Beispiele können Sie eine eigene Machine Learning-Lösung erstellen.

Der Katalog enthält Beispielexperimente des Microsoft Azure Machine Learning-Teams und Beispiele aus der Machine Learning-Community. Sie können zudem auch Fragen stellen oder Anmerkungen zu Experimenten veröffentlichen.

Die Nutzung des Katalogs wird in dem dreiminütigen Video [Kopieren der Arbeit anderer für Ihre Data Science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) der Reihe [Data Science für Einsteiger](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) veranschaulicht.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Suchen nach einem zu kopierenden Experiment im Cortana Intelligence-Katalog
Navigieren Sie zum [Katalog](https://gallery.cortanaintelligence.com/) und klicken Sie oben auf der Seite auf **Experiments** (Experimente), um anzuzeigen, welche Experimente verfügbar sind.

### <a name="find-the-newest-or-most-popular-experiments"></a>Suchen nach den neuesten oder beliebtesten Experimenten
Auf dieser Seite können Sie Experimente der Kategorie **Recently added** anzeigen oder nach unten zu **What‘s popular** oder den neuesten **Popular Microsoft experiments** scrollen.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Suchen nach einem Experiment, das bestimmte Anforderungen erfüllt
So durchsuchen Sie alle Experimente:

1. Klicken Sie oben auf der Seite auf **Browse all** (Alle durchsuchen).
2. Wählen Sie auf der linken Seite unter **Refine by** (Einschränken nach) im Abschnitt **Categories** (Kategorien) die Option **Experiment** aus, um alle im Katalog enthaltenen Experimente anzuzeigen.
3. Es gibt verschiedene Möglichkeiten zur Ermittlung von Experimenten, die Ihre Anforderungen erfüllen:
   * **Wählen Sie auf der linken Seite Filter aus.** Wenn Sie beispielsweise Experimente durchsuchen möchten, die einen Algorithmus zur PCA-basierten Erkennung von Anomalien verwenden, vergewissern Sie sich, dass unter **Categories** (Kategorien) die Option **Experiment** ausgewählt ist, und klicken Sie anschließend auf **Show all** (Alle anzeigen) aus. Wählen Sie unter **Algorithms Used** (Verwendete Algorithmen) die Option **PCA-Based Anomaly Detection** (PCA-basierte Erkennung von Anomalien) aus. <br></br>
     ![Filter auswählen](./media/machine-learning-sample-experiments/refine-the-view.png)
   * **Verwenden Sie das Suchfeld.** Wenn Sie etwa nach Experimenten von Microsoft zur Ziffernerkennung suchen möchten, bei denen ein 2-Klassen-Support Vector Machine-Algorithmus verwendet wird, geben Sie im Suchfeld „digit recognition“ (Ziffernerkennung) ein. Wählen Sie anschließend die Filter **Experiment**, **Microsoft content only** und **Two-Class Support Vector Machine** aus:<br></br>
     ![Verwenden des Suchfelds](./media/machine-learning-sample-experiments/search-for-experiments.png)
4. Klicken Sie auf ein Experiment, um mehr darüber zu erfahren.
5. Zum Ausführen und/oder Ändern des Experiments klicken Sie auf der Seite des Experiments auf **Open in Studio** (In Studio öffnen). <br></br>

    ![Beispielexperiment](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > Wenn Sie ein Experiment in Machine Learning Studio zum ersten Mal öffnen, können Sie es kostenlos testen oder ein Azure-Abonnement kaufen. [Informationen zur kostenlosen Testversion von Machine Learning Studio im Vergleich zum kostenpflichtigen Dienst](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Erstellen eines neuen Experiments mit einem Beispiel als Vorlage
Sie können in Machine Learning Studio auch ein neues Experiment erstellen, indem Sie ein Beispiel aus dem Katalog als Vorlage verwenden.

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Microsoft-Konto bei [Studio](https://studio.azureml.net)an, und klicken Sie dann auf **Neu**, um ein Experiment zu erstellen.
2. Durchsuchen Sie die Beispiele, und wählen Sie eines aus.

In Ihrem Machine Learning Studio-Arbeitsbereich wird auf der Grundlage des Beispielexperiments ein neues Experiment erstellt.

## <a name="next-steps"></a>Nächste Schritte
* [Importieren von Daten aus verschiedenen Quellen](machine-learning-data-science-import-data.md)
* [Schnellstart-Tutorial für die Sprache R in Machine Learning](machine-learning-r-quickstart.md)
* [Bereitstellen eines Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md)

