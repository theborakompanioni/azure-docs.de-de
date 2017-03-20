---
title: "PowerShell-Modul für Machine Learning | Microsoft Docs"
description: "Das PowerShell-Modul für Azure Machine Learning ist im öffentlichen Vorschaumodus verfügbar. Sie können PowerShell verwenden, um Arbeitsbereiche, Experimente, Webdienste und vieles mehr zu erstellen und zu verwalten."
keywords: "Experiment,Lineare Regression,Machine Learning-Algorithmen,Machine Learning-Tutorial,Verfahren für Vorhersagemodellierung,Data Science-Experiment"
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2016
ms.author: garye;haining
translationtype: Human Translation
ms.sourcegitcommit: 247d370c1f80729856e53690045991127ad54351
ms.openlocfilehash: ee3255493760917b2a96facfabe17120764cb638
ms.lasthandoff: 03/02/2017


---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-Modul für Microsoft Azure Machine Learning
Das PowerShell-Modul für Azure Machine Learning ist ein leistungsfähiges Tool, mit dem Sie Windows PowerShell zum Verwalten von Arbeitsbereichen, Experimenten, Datasets, klassischen Webdiensten und vielem mehr verwenden können.

Sie können die Dokumentation anzeigen und das Modul zusammen mit dem vollständigen Quellcode unter [https://aka.ms/amlps](https://aka.ms/amlps)herunterladen. 

> [!NOTE]
> Das Azure Machine Learning-PowerShell-Modul befindet sich derzeit im Vorschaumodus. Das Modul wird während dieses Vorschauzeitraums weiter verbessert und erweitert. Neuigkeiten und Informationen finden Sie im [Blog zu Cortana Intelligence und Machine Learning](https://blogs.technet.microsoft.com/machinelearning/) .

## <a name="what-is-the-machine-learning-powershell-module"></a>Was ist das Machine Learning PowerShell-Modul?
Das Machine Learning PowerShell-Modul ist ein .NET-basiertes DLL-Modul, mit dem Sie Azure Machine Learning-Arbeitsbereiche, -Experimente, -Datasets, klassische Webdienste und klassische Webdienst-Endpunkte über Windows PowerShell vollständig verwalten können. 

Zusammen mit dem Modul können Sie den gesamten Quellcode herunterladen, einschließlich einer sauber abgetrennten [C#-API-Ebene](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Sie können auf diese DLL aus Ihrem eigenen .NET-Projekt verweisen und Azure Machine Learning per .NET-Code verwalten. Außerdem hängt die DLL von zugrunde liegenden REST-APIs ab, die Sie direkt über Ihren bevorzugten Client nutzen können.

## <a name="what-can-i-do-with-the-powershell-module"></a>Was kann mit dem PowerShell-Modul durchgeführt werden?
Hier sind einige Aufgaben angegeben, die Sie mit diesem PowerShell-Modul durchführen können. In der [gesamten Dokumentation](https://aka.ms/amlps) finden Sie Informationen zu diesen und vielen anderen Funktionen.

* Bereitstellen eines neuen Arbeitsbereichs mit einem Verwaltungszertifikat ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Exportieren und Importieren einer JSON-Datei, die einen Experimentgraphen darstellt ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) und [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Ausführen eines Experiments ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Erstellen eines Webdiensts aus einem Vorhersageexperiment ([neu AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Erstellen eines Endpunkts in einem veröffentlichten Webdiensts ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Aufrufen eines RRS- bzw. BES-Webdienst-Endpunkts ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) und [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier ist ein kurzes Beispiel für die Verwendung von PowerShell zum Ausführen eines vorhandenen Experiments angegeben:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Einen ausführlicheren Anwendungsfall finden Sie in diesem Artikel zur Verwendung des PowerShell-Moduls zum Automatisieren einer häufig angeforderten Aufgabe: [Erstellen vieler Machine Learning-Modelle und Webdienst-Endpunkte in nur einem Experiment mit PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Wie fange ich an?
Laden Sie für Machine Learning PowerShell zunächst das [Versionspaket](https://github.com/hning86/azuremlps/releases) von GitHub herunter, und führen Sie die Schritte der [Installationsanleitung](https://github.com/hning86/azuremlps/blob/master/README.md) aus. In den Anweisungen wird erklärt, wie Sie die Blockierung der heruntergeladenen/entzippten DLL aufheben und sie dann in Ihre PowerShell-Umgebung importieren. Für die meisten Cmdlets ist es erforderlich, die Arbeitsbereich-ID, das Autorisierungstoken für den Arbeitsbereich und die Azure-Region anzugeben, in der sich der Arbeitsbereich befindet. Die einfachste Methode diese Werte bereitzustellen ist eine config.json-Standarddatei. In den Anweisungen wird auch erläutert, wie diese Datei zu konfigurieren ist. 

Sie haben auch die Möglichkeit, die Git-Struktur zu klonen, den Code zu ändern und mithilfe von Visual Studio lokal zu kompilieren.

## <a name="next-steps"></a>Nächste Schritte
Die vollständige Dokumentation für das PowerShell-Modul finden Sie unter [https://aka.ms/amlps](https://aka.ms/amlps). 

Ein erweitertes Beispiel für die Verwendung des Moduls in einem realen Szenario finden Sie im ausführlichen Anwendungsfall [Erstellen vieler Machine Learning-Modelle und Webdienst-Endpunkte in nur einem Experiment mit PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

