---
title: Exemplarische Vorgehensweisen mit HDInsight Spark unter Verwendung von PySpark und Scala in Azure | Microsoft-Dokumentation
description: "Beispiele des Team Data Science-Prozesses mit exemplarischer Vorgehensweise der Nutzung von PySpark und Scala in Azure HDInsight Spark für Predictive Analytics."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 27065c3437c4617ed035623b48aa1a1a31a7094f
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---


# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>Exemplarische Data Science-Vorgehensweisen mit HDInsight Spark unter Verwendung von PySpark und Scala in Azure

In diesen exemplarischen Vorgehensweisen werden PySpark und Scala in einem Azure Spark-Cluster für Predictive Analytics verwendet. Sie folgen den im Team Data Science-Prozess aufgeführten Schritten. Eine Übersicht über den Team Data Science-Prozess finden Sie unter [Data Science-Prozess](data-science-process-overview.md). Einen Überblick über Spark in HDInsight finden Sie unter [Einführung in Spark in HDInsight](../hdinsight/hdinsight-apache-spark-overview.md).

Weitere exemplarische Vorgehensweisen zu Data Science, in denen der Team Data Science-Prozess ausgeführt wird, sind nach der jeweils verwendeten **Plattform** gruppiert: 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Vorhersagen von Taxitrinkgeldern mithilfe von PySpark in Azure Spark

Die exemplarische Vorgehensweise [Verwenden von Spark in Azure HDInsight](machine-learning-data-science-spark-overview.md) arbeitet mit Daten von New Yorker Taxis, um vorherzusagen, ob ein Trinkgeld (und in welcher Höhe) gezahlt wird. Es wird der Team Data Science-Prozess in einem Szenario mit einem [Azure HDInsight Spark-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet, um Daten aus dem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York zu speichern, zu untersuchen und einem Feature Engineering zu unterziehen. In diesem Übersichtsthema richten Sie einen HDInsight Spark-Cluster und die Jupyter PySpark Notebooks ein, die im weiteren Verlauf der exemplarischen Vorgehensweise verwendet werden. Diese Notebooks zeigen Ihnen, wie Sie Ihre Daten durchsuchen sowie Modelle erstellen und nutzen. Das Notebook zum erweiterten Durchsuchen von Daten und Modellieren zeigt, wie Sie die Kreuzvalidierung, das Hyper-Parameter-Sweeping und die Auswertung von Modellen einbeziehen können.

### <a name="data-exploration-and-modeling-with-spark"></a>Datenuntersuchung und -modellierung mit Spark 
Untersuchen Sie das Dataset, und erstellen und bewerten Sie die Machine Learning-Modelle. Arbeiten Sie dazu das Thema [Erstellen von binären Klassifizierungs- und Regressionsmodellen für Daten mit dem Spark MLib-Toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) durch.

### <a name="model-consumption"></a>Nutzung von Modellen
Informationen zum Bewerten der in diesem Thema erstellten Klassifizierungs- und Regressionsmodelle finden Sie unter [Bewerten von Machine Learning-Modellen, die mit Spark erstellt wurden](machine-learning-data-science-spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Kreuzvalidierung und Hyperparameter-Sweeping
Unter [Erweiterte Datendurchsuchung und Modellierung mit Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) erfahren Sie, wie Modelle mit Kreuzvalidierung und Hyperparameter-Sweeping trainiert werden können.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Vorhersagen von Taxitrinkgeldern mithilfe von Scala in Azure Spark

Die exemplarische Vorgehensweise [Verwenden von Scala in Azure](machine-learning-data-science-process-scala-walkthrough.md) arbeitet mit Daten zu New Yorker Taxis, um vorherzusagen, ob ein Trinkgeld (und in welcher Höhe) gezahlt wird. Gezeigt wird die Verwendung von Scala für überwachte Machine Learning-Aufgaben mit der Machine Learning-Bibliothek (MLlib) von Spark und SparkML-Paketen in einem Azure HDInsight Spark-Cluster. Sie führt Sie durch die Aufgaben, aus denen der [Data Science-Prozess](http://aka.ms/datascienceprocess)besteht: Erfassen und Durchsuchen von Daten, Visualisierung, Featureentwicklung, Modellierung und Modellnutzung. Die erstellten Modelle umfassen logistische und lineare Regression, Random Forests und Gradient-Boosted-Strukturen.


## <a name="next-steps"></a>Nächste Schritte

Eine Erläuterung der Hauptkomponenten, die den Team Data Science-Prozess bilden, finden Sie unter [Übersicht über den Team Data Science-Prozess](data-science-process-overview.md).

Eine Erläuterung des Team Data Science-Prozesslebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können, finden Sie unter [Team Data Science-Prozesslebenszyklus](data-science-process-lifecycle.md). Im Lebenszyklus sind die gesamten Schritte aufgeführt, die bei Projekten bei der Ausführung normalerweise durchlaufen werden. 


