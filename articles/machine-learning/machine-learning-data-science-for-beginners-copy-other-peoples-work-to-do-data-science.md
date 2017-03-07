---
title: "Kopieren der Data Science-Beispiele von anderen – Machine Learning | Microsoft-Dokumentation"
description: 'Branchengeheimnis von Data Science: Lassen Sie andere Ihre Arbeit erledigen. Abrufen von Machine Learning-Beispielen aus dem Cortana Analytics-Katalog.'
keywords: Data Science-Beispiele,Machine Learning-Beispiel,Clusteringalgorithmus,Beispiel eines Clusteringalgorithmus
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: ec2be823-c325-4ad8-b8b2-3e664f1a44b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2017
ms.author: cgronlun;garye
translationtype: Human Translation
ms.sourcegitcommit: 1e941778c194462b311e6347c56670d3e0193e90
ms.openlocfilehash: ebc44a4190bac697697e0b651039a0e8ee8e4a95
ms.lasthandoff: 02/10/2017


---
# <a name="copy-other-peoples-work-to-do-data-science"></a>Kopieren der Arbeit anderer für Ihre Data Science
## <a name="video-5-data-science-for-beginners-series"></a>5. Video der Reihe „Data Science für Einsteiger“
Eines der Branchengeheimnisse von Data Science besteht darin, Ihre Arbeit von anderen erledigen zu lassen. Suchen Sie im Cortana Analytics-Katalog ein Beispiel für einen Clusteringalgorithmus, das Sie für Ihr eigenes Machine Learning-Experiment verwenden können.

Die Reihe bietet den größten Nutzen, wenn Sie sich alle Videos ansehen. [Zur Liste der Videos wechseln](#other-videos-in-this-series)

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-copy-other-peoples-work-to-do-data-science/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere Videos in dieser Reihe
*Data Science für Einsteiger* erhalten Sie eine Schnelleinführung in Data Science.

* Video 1: [Die 5 Fragen, die Data Science beantwortet](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5:14 Min.)*
* Video 2: [Sind Ihre Daten für Data Science bereit?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4:56 Min.)*
* Video 3: [Stellen einer Frage, die Sie mit Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4:17 Min.)*
* Video 4: [Vorhersagen einer Antwort mit einem einfachen Modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7:42 Min.)*
* Video 5: Kopieren der Arbeit anderer für Ihre Data Science

## <a name="transcript-copy-other-peoples-work-to-do-data-science"></a>Aufzeichnung: Kopieren der Arbeit anderer für Ihre Data Science
Willkommen beim fünften Video der Reihe „Data Science für Einsteiger“.

In diesem Artikel lernen Sie einen Ort mit Beispielen kennen, die Sie als Ausgangspunkt für Ihre eigene Arbeit nutzen können. Sie lernen bei diesem Video am meisten, wenn Sie sich zuerst die vorherigen Videos der Reihe ansehen.

Eines der Branchengeheimnisse von Data Science besteht darin, Ihre Arbeit von anderen erledigen zu lassen.

## <a name="find-examples-in-the-cortana-intelligence-gallery"></a>Suchen nach Beispielen im Cortana Intelligence-Katalog
Microsoft bietet einen cloudbasierten Dienst namens [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), den Sie kostenlos testen können. Dabei wird ein Arbeitsbereich bereitgestellt, in dem Sie mit unterschiedlichen Machine Learning-Algorithmen experimentieren können. Nachdem Sie Ihre Lösung erstellt haben, können Sie sie als Webdienst starten.

Eine Komponente dieses Diensts ist der **[Cortana Intelligence-Katalog](http://aka.ms/CortanaIntelligenceGallery)**. Er enthält viele verschiedene Ressourcen, z.B. eine Sammlung mit Azure Machine Learning-Experimenten oder Modelle, die von Benutzern erstellt und für andere Benutzer zur Verfügung gestellt wurden. Diese Experimente stellen eine hervorragende Möglichkeit dar, die investierte Arbeit anderer Benutzer als Starthilfe für Ihre eigenen Lösungen zu nutzen.

Sie finden diesen Katalog unter [aka.ms/CortanaIntelligenceGallery](http://aka.ms/CortanaIntelligenceGallery). Er ist für alle interessierten Benutzer zugänglich.

![Cortana Intelligence Gallery](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/cortana-intelligence-gallery.png)

Wenn Sie oben auf **Experimente** klicken, werden die neuesten und beliebtesten Experimente im Katalog angezeigt. Sie können die restlichen Experimente durchsuchen, indem Sie oben im Fenster auf **Alle durchsuchen** klicken. Sie können Suchbegriffe eingeben und Suchfilter auswählen.

## <a name="find-and-use-a-clustering-algorithm-example"></a>Suchen und Verwenden eines Beispiels eines Clusteringalgorithmus
Angenommen, Sie möchten ein Beispiel für die Funktionsweise des Clusterings anzeigen. Sie suchen dann nach Experimenten mit dem Begriff **Clustering**.

![Nach Clusteringexperimenten suchen](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/search-for-clustering-experiments.png)

Hier ist ein interessantes Experiment dargestellt, das dem Katalog als Beitrag hinzugefügt wurde.

![Clusteringexperiment](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment.png)

Wenn Sie auf dieses Experiment klicken, wird eine Webseite angezeigt, auf der die Arbeit des Erstellers und die Ergebnisse beschrieben werden.

![Seite mit Beschreibung von Clusteringexperimenten](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment-description-page.png)

Beachten Sie den Link **Open in Studio**.

![Schaltfläche „In Studio öffnen“](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/open-in-studio.png)

Ich kann auf den Link klicken, um direkt zu **Azure Machine Learning Studio**zu gelangen. Es wird eine Kopie des Experiments erstellt und in meinen Arbeitsbereich eingefügt. Dies umfasst das Dataset des Erstellers, alle Verarbeitungsschritte, alle verwendeten Algorithmen und die Speicherung der Ergebnisse.

![Öffnen Sie ein Katalogexperiment in Machine Learning Studio – Beispiel eines Clusteringalgorithmus](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/cluster-experiment-open-in-studio.png)

Außerdem verfüge ich jetzt über einen Ausgangspunkt. Ich kann die Daten des Erstellers gegen meine Daten austauschen und das Modell für meine Zwecke optimieren. So verschaffe ich mir einen Vorsprung, und ich kann auf der Arbeit von Benutzern aufbauen, die wissen, was sie tun.

## <a name="find-experiments-that-demonstrate-machine-learning-techniques"></a>Suchen nach Experimenten zur Veranschaulichung von Machine Learning-Verfahren
Die [Cortana Intelligence Gallery](http://aka.ms/CortanaIntelligenceGallery) enthält noch weitere Experimente, die speziell als Gewusst-wie-Beispiele für Personen bereitgestellt wurden, für die Data Science neu ist. Im Katalog ist beispielsweise ein Experiment enthalten, das veranschaulicht, wie Sie mit fehlenden Werten umgehen ([Methods for handling missing values](https://gallery.cortanaintelligence.com/Experiment/Methods-for-handling-missing-values-1)Methoden zum Behandeln fehlender Werte). Es werden 15 unterschiedliche Wege zum Ersetzen leerer Werte beschrieben, und es werden jeweils die Vorteile der einzelnen Methoden und die Anwendungsmöglichkeiten erläutert.

![In Machine Learning Studio geöffnete Katalogexperimente – Methoden für fehlende Werte](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/experiment-methods-for-handling-missing-values.png)

[Cortana Intelligence Gallery](http://aka.ms/CortanaIntelligenceGallery) ist der Ort, an dem Sie funktionierende Experimente finden, die Sie als Ausgangspunkt für Ihre eigenen Lösungen verwenden können.

Sehen Sie sich unbedingt auch die anderen Videos in der Reihe „Data Science für Einsteiger“ von Microsoft Azure Machine Learning an.

## <a name="next-steps"></a>Nächste Schritte
* [Durchführen Ihres ersten Data Science-Experiments mit Azure Machine Learning](machine-learning-create-experiment.md)
* [Einführung in Machine Learning in Microsoft Azure](machine-learning-what-is-machine-learning.md)

