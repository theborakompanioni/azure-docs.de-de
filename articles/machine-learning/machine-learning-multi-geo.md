---
title: "Hilfedokumentation für mehrere geografische Räume | Microsoft Docs"
description: "Erfahren Sie, wie Sie einen Arbeitsbereich in einer anderen Azure-Region als SCUS (South Central-USA) erstellen und dort einen Webdienst veröffentlichen können."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/17/2016
ms.author: tedway; neerajkh
translationtype: Human Translation
ms.sourcegitcommit: ec9d29701915d237686625fbc3abec9827fda467
ms.openlocfilehash: 22c30bfea89e3c650d81e1ebae20c9d151e6f36c


---
# <a name="multi-geo-help-documentation"></a>Hilfedokumentation für mehrere geografische Räume
Dieser Artikel beschreibt, wie Sie in unterschiedlichen Azure-Regionen einen Arbeitsbereich erstellen und einen Webdienst veröffentlichen können.  Auf der Seite [Azure-Produkte nach Region](https://azure.microsoft.com/en-us/regions/services/) sind die Regionen aufgeführt, in denen Azure Machine Learning verfügbar ist.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs
1. Melden Sie sich beim klassischen Azure-Portal an.
2. Klicken Sie auf **+ NEU** > **DATENDIENSTE** > **MACHINE LEARNING** > **SCHNELLERFASSUNG**.  Wählen Sie unter **LOCATION** eine andere Region aus, z.B. **Southeast Asia**.
   ![Hilfe zu mehreren geografischen Räumen Bild 1][1]
3. Wählen Sie den Arbeitsbereich aus, und klicken Sie dann auf **Sign-in to ML Studio**.
   ![Hilfe zu mehreren geografischen Räumen Bild 2][2]
4. Sie verfügen jetzt über einen Arbeitsbereich in einer anderen geografischen Region, den Sie genau so wie jeden anderen Arbeitsbereich verwenden können. Wenn Sie zwischen Ihren Arbeitsbereichen wechseln möchten, betrachten Sie die Ecke rechts oben des Bildschirms. Klicken Sie auf die Dropdownliste, wählen Sie die Region aus, und wählen Sie dann den Arbeitsbereich aus. Alles ist bezogen auf die Region des Arbeitsbereichs.  Beispielsweise befinden sich alle in einem Arbeitsbereich erstellten Webdienste in der Region, in dem sich der Arbeitsbereich befindet.
   ![Hilfe zu mehreren geografischen Räumen Bild 3][3]

## <a name="open-an-experiment-from-gallery"></a>Öffnen Sie ein Experiment aus der Galerie
Wenn Sie ein Experiment aus der Galerie öffnen, können Sie ebenfalls auswählen, in welche Region Sie das Experiment kopieren möchten.

![Hilfe zu mehreren geografischen Räumen Bild 4][4a]

## <a name="web-service-management"></a>Webdienstverwaltung
Um Webdienste programmgesteuert zu verwalten, etwa zum erneuten Trainieren, verwenden Sie die regionsspezifische Adresse:

* https://asiasoutheast.management.azureml.net
* https://europewest.management.azureml.net

### <a name="things-to-note"></a>Was Sie beachten sollten
1. Experimente können auf diese Art nur zwischen Arbeitsbereichen innerhalb der gleichen Region kopiert werden. Wenn Sie Experimente Arbeitsbereiche in verschiedenen Regionen übergreifend kopieren müssen, können Sie hierzu das [PowerShell](http://aka.ms/amlps)-Cmdlet [*Copy-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) verwenden. Eine weitere Möglichkeit besteht darin, das Experiment im Katalog im nicht gelisteten Modus zu veröffentlichen und dann aus der anderen Region heraus im Arbeitsbereich zu öffnen.
2. Die Regionsauswahl zeigt immer nur jeweils die Arbeitsbereiche einer Region an.  
3. Kostenlose Arbeitsbereiche oder Gastkonten (anonym) werden in South Central-USA erstellt und gehostet.  
4. Webdienste, die aus einem Arbeitsbereich in Südostasien bereitgestellt werden, werden auch in Südostasien gehostet.  

## <a name="more-information"></a>Weitere Informationen
Stellen Sie eine Frage im [Azure Machine Learning-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png



<!--HONumber=Dec16_HO2-->


