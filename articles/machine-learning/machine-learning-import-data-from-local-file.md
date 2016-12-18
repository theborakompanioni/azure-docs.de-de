---
title: Importieren von Daten in Machine Learning Studio aus einer lokalen Datei | Microsoft Docs
description: Importieren von Trainingsdaten in Azure Machine Learning Studio aus einer lokalen Datei.
keywords: Importieren von Daten,Datenformat,Datentypen,Datenquellen,Trainingsdaten
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: garye;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d4ab1fd19d0c865e8920ae3079df1683aaa4be7d


---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Importieren Ihrer Trainingsdaten in Azure Machine Learning Studio aus einer lokalen Datei
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Um Ihre eigenen Daten in Machine Learning Studio zu verwenden, können Sie im Voraus eine Datei von Ihrer lokalen Festplatte hochladen, um ein Datasetmodul in Ihrem Arbeitsbereich zu erstellen. 

## <a name="import-data-from-a-local-file"></a>Importieren von Daten aus einer lokalen Datei
Sie können wie folgt Daten von einer lokalen Festplatte importieren:

1. Klicken Sie unten im Machine Learning Studio-Fenster auf **+NEW** .
2. Wählen Sie **DATASET** und **FROM LOCAL FILE** aus.
3. Navigieren Sie im Dialogfeld **Upload a new dataset** zu der Datei, die Sie hochladen möchten.
4. Geben Sie einen Namen ein, identifizieren Sie den Datentyp, und geben Sie optional eine Beschreibung ein. Eine Beschreibung wird empfohlen: Damit können Sie beliebige Eigenschaften zu den Daten zu erfassen, die Sie bedenken möchten, wenn Sie die Daten in der Zukunft verwenden.
5. Mit dem Kontrollkästchen **This is the new version of an existing dataset** können Sie ein vorhandenes Dataset mit neuen Daten aktualisieren. Aktivieren Sie dieses Kontrollkästchen, und geben Sie dann den Namen eines vorhandenen Datasets ein.

Während des Uploads sehen Sie eine Meldung, dass die Datei hochgeladen wird. Die Dauer für das Hochladen hängt von der Größe Ihrer Daten und der Geschwindigkeit der Verbindung mit dem Dienst ab.
Wenn Sie wissen, dass das Hochladen der Datei eine lange Zeit dauern kann, können Sie andere Dinge in Machine Learning Studio erledigen, während Sie warten. Durch das Schließen des Browsers werden die Daten jedoch nicht hochgeladen.

Sobald Ihre Daten hochgeladen wurden, werden sie in einem Datasetmodul gespeichert und stehen für alle Experimente im Arbeitsbereich zur Verfügung.
Wenn Sie ein Experiment bearbeiten, finden Sie die Datasets, die Sie erstellt haben, in der Modulpalette unter der Liste **Gespeicherte Datasets** in der Liste **Meine Datasets**. Sie können das Dataset in den Experimentbereich ziehen und dort ablegen, wenn Sie es für weitere Analysen und Machine Learning verwenden möchten.




<!--HONumber=Nov16_HO3-->


