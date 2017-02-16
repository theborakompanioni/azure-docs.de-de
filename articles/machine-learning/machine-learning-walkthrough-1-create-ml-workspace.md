---
title: 'Schritt 1: Erstellen eines Machine Learning-Arbeitsbereichs | Microsoft Docs'
description: "Exemplarische Vorgehensweise zum Entwickeln einer Vorhersagelösung – Schritt 1: Erfahren Sie, wie Sie einen neuen Azure Machine Learning Studio-Arbeitsbereich einrichten."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: a9ebbbdc431a34553de04e920efbbc8c2496ce5f
ms.openlocfilehash: 116dbf0ee30497d82c984b10e61ae02301e820a4


---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Exemplarische Vorgehensweise, Schritt 1: Erstellen eines Machine Learning-Arbeitsbereichs
Dies ist der erste Schritt der exemplarischen Vorgehensweise zum [Entwickeln einer Predictive Analytics-Lösung mit Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

1. **Erstellen eines Machine Learning-Arbeitsbereichs**
2. [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)
3. [Erstellen eines neuen Experiments](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

Um Machine Learning Studio verwenden zu können, benötigen Sie einen Microsoft Azure Machine Learning-Arbeitsbereich. Dieser Arbeitsbereich enthält die Tools, die zum Erstellen, Verwalten und Veröffentlichen von Experimenten erforderlich sind.  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Der Administrator für Ihr Azure-Abonnement muss den Arbeitsbereich erstellen und Sie dann als Besitzer oder Mitwirkenden hinzufügen. Details finden Sie unter [Erstellen und Freigeben eines Azure Machine Learning-Arbeitsbereichs](machine-learning-create-workspace.md).

Nachdem Ihr Arbeitsbereich erstellt wurde, öffnen Sie Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)). Wenn dies Ihr einziger Arbeitsbereich ist, wird er in Studio automatisch geöffnet. Andernfalls können Sie den Arbeitsbereich im Fenster oben rechts auf der Symbolleiste auswählen.

![Auswählen eines Arbeitsbereichs in Studio][2]

> [!TIP]
> Wenn Sie als Besitzer des Arbeitsbereichs hinzugefügt wurden, können Sie die Experimente, an denen Sie arbeiten, mit anderen Personen teilen, indem Sie sie zum Arbeitsbereich einladen. Dies können Sie in Machine Learning Studio auf der Seite **EINSTELLUNGEN** vornehmen. Sie benötigen nur das Microsoft-Konto oder Organisationskonto des betreffenden Benutzers.
> 
> Klicken Sie auf der Seite **EINSTELLUNGEN** auf **BENUTZER** und dann unten in Fenster auf **INVITE MORE USERS** (WEITERE BENUTZER EINLADEN).
> 
> 

- - -
**Nächster Schritt: [Hochladen vorhandener Daten](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png



<!--HONumber=Dec16_HO3-->


