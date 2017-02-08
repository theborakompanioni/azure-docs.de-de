---
title: ALM in Azure ML | Microsoft Docs
description: Anwenden von Best Practices in Application Lifecycle Management in Azure Machine Learning Studio
keywords: ALM, AML, Azure ML, Application Life Cycle Management, Versionskontrolle
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1be6577d-f2c7-425b-b6b9-d5038e52b395
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: haining
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 614aa45b365abe90fcc2e75fcde50f1c62a95ed8


---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Application Lifecycle Management in Azure Machine Learning Studio
Azure Machine Learning Studio ist ein Tool, mit dem Machine Learning-Experimente entwickelt und in der Azure-Cloud eingesetzt werden. Es entspricht der Zusammenführung von Visual Studio-IDE und skalierbaren Webdienst-Hostingdiensten zu einer einzelnen Plattform. Daher ist es nur logisch/natürlich, standardmäßige ALM-Methoden (Application Lifecycle Management) von der Versionsverwaltung verschiedener Ressourcen bis zur automatischen Ausführung und Bereitstellung  in Azure Machine Learning Studio zu integrieren. In diesem Artikel werden einige der Optionen und Ansätze abdecken behandelt. 

## <a name="versioning-experiment"></a>Versionsverwaltungsexperiment
Es gibt zwei empfohlene Methoden zur Verwaltung der Versionen Ihrer Experimente. Sie können sich entweder auf den integrierten Ausführungsverlauf verlassen oder das Experiment in das JSON-Format exportieren und extern verwalten. Jeder Ansatz hat Vor- und Nachteile.

### <a name="experiment-snapshots-using-run-history"></a>Momentaufnahmen des Experiments mithilfe des Ausführungsverlaufs
Das Ausführungsmodell des Azure Machine Learning-Experiments sieht vor, das immer dann, wenn Sie die Run-Schaltfläche im Experiment-Editor betätigen, eine unveränderliche Momentaufnahme des Experiments an den Auftragsplaner gesendet wird. Sie können diese Liste der Momentaufnahmen durch Klicken auf die Schaltfläche „RUN HISTORY“ in der Befehlsleiste in der Experiment-Editor-Ansicht anzeigen.

![Schaltfläche „RUN HISTORY“](media/machine-learning-version-control/runhistory.png)

Sie können dann eine Momentaufnahme im gesperrten Modus öffnen, indem Sie auf den Namen des Experiments mit dem Zeitpunkt klicken, zu dem das Experiment zur Ausführung gesendet und die Momentaufnahme aufgezeichnet wurde. Beachten Sie, dass nur das erste Element in der Liste, das das aktuelle Experiment darstellt, bearbeitbar ist. Beachten Sie außerdem, dass jede Momentaufnahme auch in verschiedenen Status vorhanden sein kann, einschließlich „Finished (Partial run)“, „Failed“, „Failed (Partial run)“ oder „Draft“.

![RUN HISTORY-Liste](media/machine-learning-version-control/runhistorylist.png)

Nach dem Öffnen können Sie das Momentaufnahmenexperiment als neues Experiment speichern und dann bearbeiten. Ein Nachteil ist: Wenn Ihre Experimentmomentaufnahme Ressourcen wie z.B. trainierte Modelle, Transformationen, Datasets usw. enthält, deren Versionen mittlerweile aktualisiert wurden, behält die Momentaufnahme die Verweise auf die ursprüngliche Version bei, die zum Zeitpunkt der Momentaufnahme vorlag. Aber wenn Sie die gesperrte Momentaufnahme als neues Experiment speichern, erkennt ML Studio das Vorhandensein neuerer Versionen dieser Ressourcen und aktualisiert sie automatisch auf die neueste Version. 

Beachten Sie außerdem, dass beim Löschen des Experiments alle Momentaufnahmen dieses Experiments ebenfalls gelöscht werden.

### <a name="exportimport-experiment-in-json-format"></a>Exportieren/Importieren des Experiments im JSON-Format
Obwohl die Ausführungsverlaufs-Momentaufnahmen immer dann, wenn das Experiment zur Ausführung gesendet wird, eine unveränderliche Version des Experiments in ML Studio beibehalten, möchten Sie sicher in manchen Fällen eine lokale Kopie des Experiments speichern und sie vielleicht in Ihr bevorzugtes Quellcodeverwaltungssystem einchecken – z.B. Team Foundation Server – und später aus dieser lokalen Datei ein Experiment neu erstellen. Sie können hierzu die [Azure ML PowerShell](http://aka.ms/amlps)-Cmdlets [*Export-AmlExperimentGraph*](https://github.com/hning86/azuremlps#export-amlexperimentgraph) und [*Import-AmlExperimentGraph*](https://github.com/hning86/azuremlps#import-amlexperimentgraph) verwenden.

Bitte beachten Sie: Die JSON-Datei ist zwar eine Textdarstellung des Experimentdiagramms, die möglicherweise Verweise auf Ressourcen im Arbeitsbereich enthält, z.B. Datasets oder trainierte Modelle. Aber sie enthält KEINE serialisierte Version solcher Ressourcen. Wenn Sie also versuchen, das JSON-Dokument wieder in den Arbeitsbereich zu importieren, müssen diese referenzierten Ressourcen bereits mit den gleichen Ressourcen-IDs vorhanden sein, auf die im Experiment verwiesen wird; andernfalls können Sie nicht auf das importierte Experiment zugreifen.

## <a name="versioning-trained-model"></a>Versionsverwaltung trainierter Modelle
Ein trainiertes Modell in Azure ML wird in ein .iLearner-Datei genanntes Format serialisiert und in dem Azure-Blobspeicherkonto gespeichert, das mit dem Arbeitsbereich verknüpft ist. Eine Möglichkeit zum Abrufen einer Kopie der iLearner-Datei bietet die API zum Umtrainieren. In diesem [Artikel](machine-learning-retrain-models-programmatically.md) wird viel ausführlicher erläutert, wie die API zum Umtrainieren funktioniert. Die allgemeinen Schritte sind allerdings folgende:

1. Richten Sie Ihr Trainingsexperiment ein.
2. Fügen Sie den Webdienst-Ausgabeport dem Train Model-Modul hinzu – oder dem Modul, das das trainierte Modell erzeugt, z.B. dem Tune Model Hyperparameter- oder Create R Model-Modul.
3. Führen Sie Ihr Trainingsexperiment aus, und stellen Sie es anschließend als Modelltraining-Webdienst bereit. 
4. Rufen Sie den BES-Endpunkt des Trainingswebdiensts auf, und geben Sie den Namen der gewünschten .iLearner-Datei und den Speicherplatz des Azure-Blobspeicherkontos an, wo sie gespeichert werden soll.
5. Nach Abschluss des BES-Aufrufs erhalten Sie die erstellte .iLearner-Datei.

Sie können die .iLearner-Datei auch über das PowerShell-Cmdlet [*Download-AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput) abrufen. Dies ist möglicherweise einfacher, wenn Sie nur eine Kopie der iLearner-Datei abrufen möchten ohne die Notwendigkeit, das Modell programmgesteuert neu zu trainieren.

Sobald Sie über die .iLearner-Datei mit dem trainierten Modell verfügen, können Sie Ihre eigene Versionsverwaltungsstrategie anwenden – damit angefangen, dass Sie einfach ein Prä- oder Postfix als Benennungskonvention anfügen und die .iLearner-Datei im Azure-Blobspeicher lassen, bis zum Kopieren/Importieren der Datei in Ihr Versionskontrollsystem.

Die gespeicherte .iLearner-Datei kann dann für die Bewertung über bereitgestellte Webdienste verwendet werden.

## <a name="versioning-web-service"></a>Versionsverwaltung-Webdienst
Sie können zwei Arten von Webdiensten von einem Azure ML-Experiment aus bereitstellen. Der klassische Webdienst ist sowohl mit dem Experiment als auch dem Arbeitsbereich eng verbunden. Der neue Webdienst nutzt das Framework der Ressourcenverwaltung in Azure und ist nicht mehr mit dem ursprünglichen Experiment noch dem Arbeitsbereich verbunden. 

### <a name="classic-web-service"></a>Klassischer Webdienst
Zur Versionsverwaltung bei einem klassischen Webdienst können Sie das Webdienst-Endpunkt-Konstrukt nutzen. Hier sehen Sie einen typischen Ablauf:

1. Von Ihrem Vorhersageexperiment aus stellen Sie einen neuen klassischen Webdienst bereit, der einen standardmäßigen Endpunkt enthält.
2. Anschließend erstellen Sie einen neuen Endpunkt mit dem Namen „ep2“, der die aktuelle Version des Experiments/trainierten Modells verfügbar macht.
3. Dann gehen Sie zurück und aktualisieren Ihr Vorhersageexperiment und Ihr trainiertes Modell.
4. Sie können dann das Vorhersageexperiment erneut bereitstellen, das dann den Standardendpunkt aktualisiert. Dies ändert „ep2“ jedoch nicht.
5. Sie können jetzt einen zusätzlichen Endpunkt „ep3“ erstellen, der die neue Version der Experiments und des trainierten Modells verfügbar macht. 
6. Gehen Sie ggf. zurück zu Schritt 3.

Im Laufe der Zeit haben Sie möglicherweise viele Endpunkte im gleichen Webdienst erstellt, wobei jeder von ihnen eine Kopie des Experiments zu einem bestimmten Zeitpunkt mit der jeweils gültigen Version des trainierten Modells darstellt. Dann können Sie mittels externer Logik bestimmen, welcher Endpunkt aufgerufen werden soll, was der Auswahl einer Version des trainierten Modells für die Bewertungsausführung entspricht.

Sie können auch viele identische Webdienst-Endpunkte erstellen und dann verschiedene Versionen der .iLearner-Datei mit dem Endpunkt patchen, um einen ähnlichen Effekt zu erzielen. In diesem [Artikel](machine-learning-create-models-and-endpoints-with-powershell.md) wird viel ausführlicher erläutert, wie Sie dies durchführen können.

### <a name="new-web-service"></a>Neuer Webdienst
Wenn Sie einen neuen Webdienst auf Basis des Azure Resource Manager erstellen, ist das Endpunktkonstrukt nicht mehr verfügbar. Sie können stattdessen von Ihrem Vorhersageexperiment aus mit dem [Export-AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment)-PowerShell-Cmdlet oder von einem bereits bereitgestellten Webdienst auf Basis des Resource Manager mit dem [*Export-AzureRmMlWebservice*](https://msdn.microsoft.com/library/azure/mt767935.aspx)-PowerShell-Cmdlet WSD-Dateien (Web Service Definition, Webdienstdefinition) im JSON-Format generieren. 

Sobald Sie die WSD-Datei exportiert haben und ihre Version verwalten,  können Sie auch die WSD in einem anderen Webdienstplan in einer anderen Azure-Region als neuen Webdienst bereitstellen. Achten Sie nur darauf, dass Sie sowohl die richtige Speicherkontokonfiguration als auch die neue Webdienstplan-ID angeben. Um in verschiedenen .iLearner-Dateien zu patchen, können Sie die WSD-Datei ändern und den Speicherortverweis des trainierten Modells aktualisieren und als neuen Webdienst bereitstellen.

## <a name="automate-experiment-execution-and-deployment"></a>Automatisieren von Ausführung und Bereitstellung des Experiments
Ein wichtiger Aspekt von ALM ist die Möglichkeit, den Ausführungs- und Bereitstellungsprozess der Anwendung zu automatisieren. In Azure ML erreichen Sie dies mithilfe des [PowerShell-Moduls](http://aka.ms/amlps). Hier ist ein Beispiel der gesamten Schritte, die für einen standardmäßigen automatisierten ALM-Ausführungs-/Bereitstellungsprozess mit dem [Azure ML Studio PowerShell-Modul](http://aka.ms/amlps) relevant sind. Jeder Schritt ist mit mindestens einem PowerShell-Cmdlet verknüpft, das/die Sie nutzen können, um diesen Schritt auszuführen.

1. [Laden Sie ein Dataset hoch.](https://github.com/hning86/azuremlps#upload-amldataset) 
2. Kopieren Sie ein Trainingsexperiment aus einem [Arbeitsbereich](https://github.com/hning86/azuremlps#copy-amlexperiment) oder [Katalog](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery) in den Arbeitsbereich, oder [importieren](https://github.com/hning86/azuremlps#import-amlexperimentgraph) Sie ein [exportiertes](https://github.com/hning86/azuremlps#export-amlexperimentgraph) Experiment vom lokalen Datenträger.
3. [Aktualisieren Sie das Dataset](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) im Trainingsexperiment.
4. [Führen Sie das Trainingsexperiment aus.](https://github.com/hning86/azuremlps#start-amlexperiment)
5. [Stufen Sie das trainierte Modell höher.](https://github.com/hning86/azuremlps#promote-amltrainedmodel)
6. [Kopieren Sie ein Vorhersageexperiment](https://github.com/hning86/azuremlps#copy-amlexperiment) in den Arbeitsbereich.
7. [Aktualisieren Sie das trainierte Modell](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) im Vorhersageexperiment.
8. [Führen Sie das Vorhersageexperiment aus.](https://github.com/hning86/azuremlps#start-amlexperiment)
9. Stellen Sie vom Vorhersageexperiment aus einen [Webdienst bereit](https://github.com/hning86/azuremlps#new-amlwebservice).
10. Testen Sie den [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint)- oder [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint)-Endpunkt des Webdiensts. 

## <a name="next-steps"></a>Nächste Schritte
* Laden Sie das [Azure ML Studio PowerShell](http://aka.ms/amlps)-Modul herunter, und starten Sie die Automatisierung Ihrer ALM-Aufgaben.
* Erfahren Sie, wie Sie über PowerShell und die API zum Umtrainieren [eine große Anzahl von ML-Modellen mit nur einem einzigen Experiment erstellen und verwalten](machine-learning-create-models-and-endpoints-with-powershell.md).
* Erfahren Sie mehr über [das Bereitstellen von Azure ML-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md).




<!--HONumber=Nov16_HO3-->


