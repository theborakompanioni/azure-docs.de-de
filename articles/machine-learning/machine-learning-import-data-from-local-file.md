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
ms.date: 12/14/2016
ms.author: garye;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 844dafe79c71594212d966286c74aa7f916ba60e
ms.openlocfilehash: e17a3dd8b70f22ab0c68eecde50a7506167188b9


---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Importieren Ihrer Trainingsdaten in Azure Machine Learning Studio aus einer lokalen Datei
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Um Ihre eigenen Daten in Machine Learning Studio zu verwenden, können Sie im Voraus eine Datei von Ihrer lokalen Festplatte hochladen, um ein Datasetmodul in Ihrem Arbeitsbereich zu erstellen. 

## <a name="import-data-from-a-local-file"></a>Importieren von Daten aus einer lokalen Datei
Sie können wie folgt Daten von einer lokalen Festplatte importieren:

1. Klicken Sie unten im Machine Learning Studio-Fenster auf **+NEW** .
2. Wählen Sie **DATASET** und **FROM LOCAL FILE** aus.
3. Navigieren Sie im Dialogfeld **Upload a new dataset** zu der Datei, die Sie hochladen möchten.
4. Geben Sie einen Namen ein, identifizieren Sie den Datentyp, und geben Sie optional eine Beschreibung ein. Eine Beschreibung wird empfohlen: Damit können Sie beliebige Merkmale der Daten erfassen, die Sie bei zukünftiger Nutzung der Daten berücksichtigen möchten.
5. Mit dem Kontrollkästchen **This is the new version of an existing dataset** können Sie ein vorhandenes Dataset mit neuen Daten aktualisieren. Aktivieren Sie einfach dieses Kontrollkästchen, und geben Sie dann den Namen eines vorhandenen Datasets ein.

Während des Uploads wird eine Meldung angezeigt, dass die Datei hochgeladen wird. Die Dauer für das Hochladen hängt von der Größe Ihrer Daten und der Geschwindigkeit der Verbindung mit dem Dienst ab.
Wenn Sie wissen, dass das Hochladen der Datei eine lange Zeit dauern kann, können Sie andere Dinge in Machine Learning Studio erledigen, während Sie warten. Wenn Sie den Browser schließen, tritt jedoch beim Hochladen der Daten ein Fehler auf.

Sobald Ihre Daten hochgeladen wurden, werden sie in einem Datasetmodul gespeichert und stehen für alle Experimente im Arbeitsbereich zur Verfügung.
Wenn Sie ein Experiment bearbeiten, finden Sie die Datasets, die Sie erstellt haben, in der Modulpalette unter der Liste **Gespeicherte Datasets** in der Liste **Meine Datasets**. Sie können das Dataset in den Experimentbereich ziehen und dort ablegen, wenn Sie es für weitere Analysen und maschinelles Lernen verwenden möchten.




<!--HONumber=Dec16_HO3-->


