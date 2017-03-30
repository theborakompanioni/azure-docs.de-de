---
title: Importieren von Daten aus einer Datei in Machine Learning Studio | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Trainingsdatendatei von der Festplatte in Azure Machine Learning Studio hochladen. So wird ein Datasetmodul im Arbeitsbereich erstellt.
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
ms.date: 03/20/2017
ms.author: garye;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 18010864160ceb2d76aea37196e6944bbe426457
ms.lasthandoff: 03/22/2017


---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Importieren von Trainingsdaten aus einer Datei auf der Festplatte in Machine Learning Studio
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Erfahren Sie, wie Sie eine Datendatei zur Verwendung als Trainingsdaten von der Festplatte in Azure Machine Learning Studio hochladen. Mit dem Import der Datendatei erhalten Sie ein Datasetmodul, das zur Verwendung in Ihrem Arbeitsbereich bereit ist.

## <a name="steps-to-import-data-from-a-local-file"></a>Schritte zum Importieren von Daten aus einer lokalen Datei
Sie können wie folgt Daten von einer lokalen Festplatte importieren:

1. Klicken Sie unten im Machine Learning Studio-Fenster auf **+NEW** .
2. Wählen Sie **DATASET** und **FROM LOCAL FILE** aus.
3. Navigieren Sie im Dialogfeld **Upload a new dataset** zu der Datei, die Sie hochladen möchten.
4. Geben Sie einen Namen ein, identifizieren Sie den Datentyp, und geben Sie optional eine Beschreibung ein. Eine Beschreibung wird empfohlen: Damit können Sie beliebige Merkmale der Daten erfassen, die Sie bei zukünftiger Nutzung der Daten berücksichtigen möchten.
5. Mit dem Kontrollkästchen **This is the new version of an existing dataset** können Sie ein vorhandenes Dataset mit neuen Daten aktualisieren. Aktivieren Sie einfach dieses Kontrollkästchen, und geben Sie dann den Namen eines vorhandenen Datasets ein.

![Hochladen eines neuen Datasets](media/machine-learning-import-data-from-local-file/upload-dataset.png)

Während des Uploads wird eine Meldung angezeigt, dass die Datei hochgeladen wird. Die Dauer für das Hochladen hängt von der Größe Ihrer Daten und der Geschwindigkeit der Verbindung mit dem Dienst ab. Wenn Sie wissen, dass das Hochladen der Datei eine lange Zeit dauern kann, können Sie andere Dinge in Machine Learning Studio erledigen, während Sie warten. Wenn Sie den Browser schließen, tritt jedoch beim Hochladen der Daten ein Fehler auf.

## <a name="dataset-module-is-ready-for-use"></a>Datasetmodul ist zur Verwendung bereit
Sobald Ihre Daten hochgeladen wurden, werden sie in einem Datasetmodul gespeichert und stehen für alle Experimente im Arbeitsbereich zur Verfügung.

Wenn Sie ein Experiment bearbeiten, finden Sie die Datasets, die Sie erstellt haben, in der Modulpalette unter der Liste **Gespeicherte Datasets** in der Liste **Meine Datasets**. Sie können das Dataset in den Experimentbereich ziehen und dort ablegen, wenn Sie es für weitere Analysen und maschinelles Lernen verwenden möchten.

