---
title: Erneutes Trainieren eines Machine Learning-Modells | Microsoft Docs
description: Erfahren Sie, wie Sie ein Modell erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Machine Learning verwendet.
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
translationtype: Human Translation
ms.sourcegitcommit: 201b07536bcee58e2b7102379dff1c1c93c4b675
ms.openlocfilehash: 882157d2cb544e5bb59caf7d64de579e23b12480


---
# <a name="retrain-a-machine-learning-model"></a>Erneutes Trainieren eines Machine Learning-Modells
Im Rahmen der Operationalisierung von Machine Learning-Modellen in Azure Machine Learning wird Ihr Modell trainiert und gespeichert. Anschließend kann es dann zum Erstellen eines Vorhersagewebdiensts verwendet werden. Dieser Webdienst kann von Websites, Dashboards und mobilen Apps genutzt werden. 

Mithilfe von Machine Learning erstellte Modelle sind in der Regel nicht statisch. Wenn neue Daten verfügbar sind oder der Endkunde der API über eigene Daten verfügt, muss das Modell erneut trainiert werden. 

Das erneute Training kann häufig durchgeführt werden. Mit dem API-Feature für programmgesteuertes erneutes Trainieren können Sie das Modell mithilfe der APIs zum erneuten Trainieren programmgesteuert erneut trainieren und den Webdienst mit dem neu trainierten Modell aktualisieren. 

In diesem Dokument wird das erneute Trainieren beschrieben und gezeigt, wie Sie die APIs zum erneuten Trainieren verwenden.

## <a name="why-retrain-defining-the-problem"></a>Warum erneut trainieren: Problemdefinition
Im Rahmen des Machine Learning-Trainingsprozesses wird ein Modell mit einem Satz von Daten trainiert. Mithilfe von Machine Learning erstellte Modelle sind in der Regel nicht statisch. Wenn neue Daten verfügbar sind oder der Endkunde der API über eigene Daten verfügt, muss das Modell erneut trainiert werden.

In diesen Szenarien bietet eine programmgesteuerte API eine komfortable Möglichkeit, mit der Sie oder die Nutzer Ihrer APIs einen Client erstellen können, der das Modell einmalig oder regelmäßig unter Verwendung eigener Daten trainieren kann. Anschließend können Sie die Ergebnisse des erneuten Trainierens auswerten und die Webdienst-API aktualisieren, damit sie das neu trainierte Modell verwendet.

> [!NOTE]
> Wenn Sie ein vorhandenes Trainingsexperiment und einen neuen Webdienst haben, können Sie einen vorhandenen Vorhersagewebdienst neu trainieren, anstatt die im folgenden Abschnitt erwähnte exemplarische Vorgehensweise durchzuführen.
> 
> 

## <a name="end-to-end-workflow"></a>Kompletter Workflow
Der Prozess umfasst ein Trainingsexperiment und ein Vorhersageexperiment (veröffentlicht als Webdienst). Damit ein trainiertes Modell erneut trainiert werden kann, muss das Trainingsexperiment als Webdienst mit der Ausgabe eines trainierten Modells veröffentlicht werden. Dies ermöglicht der API, zum erneuten Trainieren auf das Modell zuzugreifen. 

Die folgenden Schritte gelten für neue und klassische Webdienste:

Erstellen des anfänglichen Vorhersagewebdiensts:

* Erstellen eines Trainingsexperiments
* Erstellen eines Vorhersageexperiments
* Bereitstellen eines Vorhersagewebdiensts

Erneutes Trainieren des Webdiensts:

* Aktualisieren des Trainingsexperiments zum erneuten Trainieren
* Bereitstellen des Webdiensts zum erneuten Trainieren
* Verwenden des BES-Codes zum erneuten Trainieren des Modells

Eine exemplarische Vorgehensweise für die vorangehenden Schritte finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](machine-learning-retrain-models-programmatically.md).

Wenn Sie einen klassischen Webdienst bereitgestellt haben:

* Erstellen Sie einen neuen Endpunkt für den Vorhersagewebdienst
* Rufen Sie die PATCH-URL und den Code ab.
* Verwenden Sie die PATCH-URL, um den neuen Endpunkt auf das neu trainierte Modell zu zeigen. 

Eine exemplarische Vorgehensweise für die vorangehenden Schritte finden Sie unter [Erneutes Trainieren eines klassischen Webdiensts](machine-learning-retrain-a-classic-web-service.md).

Wenn beim erneuten Trainieren eines klassischen Webdiensts Probleme auftreten, finden Sie Hilfe unter [Problembehandlung beim erneuten Trainieren eines klassischen Azure Machine Learning Webdiensts](machine-learning-troubleshooting-retraining-models.md).

Wenn Sie einen neuen Webdienst bereitgestellt haben:

* Anmelden bei Ihrem Azure Resource Manager-Konto
* Rufen Sie die Webdienstdefinition ab.
* Exportieren Sie die Webdienstdefinition als JSON-Code.
* Aktualisieren Sie im JSON-Code den Verweis auf das `ilearner`-Blob
* Importieren Sie den JSON-Code in eine Webdienstdefinition.
* Aktualisieren Sie den Webdienst mit der neuen Webdienstdefinition.

Eine exemplarische Vorgehensweise für die vorangehenden Schritte finden Sie unter [Erneutes Trainieren eines neuen Webdiensts mithilfe der PowerShell-Cmdlets für die Verwaltung von Machine Learning](machine-learning-retrain-new-web-service-using-powershell.md).

Das Einrichten eines erneuten Trainings für einen klassischen Webdienst umfasst die folgenden Schritte:

![Übersicht über den Prozess des erneuten Trainings][1]

Das Einrichten eines erneuten Trainings für einen neuen Webdienst umfasst die folgenden Schritte:

![Übersicht über den Prozess des erneuten Trainings][7]

## <a name="other-resources"></a>Weitere Ressourcen
* [Erneutes Trainieren und Aktualisieren von Azure Machine Learning-Modellen mit Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Erstellen vieler Machine Learning-Modelle und Webdienst-Endpunkte in nur einem Experiment mit PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
* Im Video [AML-Modelle zum erneuten Trainieren mithilfe von APIs](https://www.youtube.com/watch?v=wwjglA8xllg) erfahren Sie, wie Sie APIs zum erneuten Trainieren und PowerShell zum erneuten Trainieren von in Azure Machine Learning erstellten Machine Learning-Modellen verwenden.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png




<!--HONumber=Feb17_HO2-->


