---
title: Erstellen eines Machine Learning-Arbeitsbereichs | Microsoft Docs
description: "Erstellen eines Arbeitsbereichs für Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
translationtype: Human Translation
ms.sourcegitcommit: 613cf7e34d69afa21b1808ffe57af9a8b64944f5
ms.openlocfilehash: 182a34822e71d63f4d7229548ae3f59d9f195337
ms.lasthandoff: 03/01/2017


---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Erstellen und Freigeben eines Azure Machine Learning-Arbeitsbereichs
Dieses Menü enthält Links zu Themen, in denen das Einrichten der verschiedenen Data Science-Umgebungen, die vom Cortana-Analyseprozess (CAP) verwendet werden, beschrieben wird.

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Um Azure Machine Learning Studio verwenden zu können, benötigen Sie einen Machine Learning-Arbeitsbereich. Dieser Arbeitsbereich enthält die Tools, die zum Erstellen, Verwalten und Veröffentlichen von Experimenten erforderlich sind.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a>Erstellen eines Arbeitsbereichs
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

    > [!NOTE]
    > Um sich anzumelden und einen Arbeitsbereich zu erstellen, müssen Sie ein Azure-Abonnementadministrator sein. 
    >
    > 

2. Klicken Sie auf **+Neu**.

3. Wählen Sie **Intelligence + Analyse**, klicken Sie auf **Machine Learning-Arbeitsbereich**, und klicken Sie dann auf **Erstellen**.

4. Geben Sie die Daten für Ihren Arbeitsbereich ein.

    - Der *Arbeitsbereichsname* darf bis zu 260 Zeichen enthalten und darf nicht auf ein Leerzeichen enden. Der Name darf nicht die folgenden Zeichen enthalten: `< > * % & : \ ? + /`
    - Der von Ihnen gewählte (oder erstellte) *Webdiensttarif* wird zusammen mit dem zugehörigen von Ihnen ausgewählten *Tarif* verwendet, wenn Sie Webdienste aus diesem Arbeitsbereich bereitstellen.

    ![Erstellen eines neuen Arbeitsbereichs](media/machine-learning-create-workspace/create-new-workspace.png)

5. Klicken Sie auf **Erstellen**

Nachdem der Arbeitsbereich bereitgestellt wurde, können Sie ihn in Machine Learning Studio öffnen.

1. Wechseln Sie zu Machine Learning Studio unter [https://studio.azureml.net/](https://studio.azureml.net/).

2. Wählen Sie Ihren Arbeitsbereich in der oberen rechten Ecke aus.

    ![Arbeitsbereich auswählen](media/machine-learning-create-workspace/open-workspace.png)

3. Klicken Sie auf **Meine Experimente**.

    ![Experimente öffnen](media/machine-learning-create-workspace/my-experiments.png)

Informationen zum Verwalten des Arbeitsbereichs finden Sie unter [Verwalten eines Azure Machine Learning-Arbeitsbereichs](machine-learning-manage-workspace.md).
Sollte beim Erstellen des Arbeitsbereichs ein Problem auftreten, finden Sie weitere Informationen unter [Handbuch zur Problembehandlung: Erstellen und Verbinden eines Machine Learning-Arbeitsbereichs](machine-learning-troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Freigeben eines Azure Machine Learning-Arbeitsbereichs
Sobald ein Machine Learning-Arbeitsbereich erstellt wurde, können Sie Benutzer zu Ihrem Arbeitsbereich einladen, um den Zugriff auf Ihren Arbeitsbereich und alle zugehörigen Experimente, Datasets, Notizbücher etc. freizugeben. Sie können Benutzer in einer der beiden Rollen hinzufügen:

* **Benutzer**: Ein Arbeitsbereichsbenutzer kann Experimente, Datasets etc. im Arbeitsbereich erstellen, öffnen, ändern und löschen.
* **Besitzer**: Zusätzlich zu den Möglichkeiten eines Benutzers kann ein Besitzer Benutzer im Arbeitsbereich einladen und entfernen.

> [!NOTE]
> Das Administratorkonto, das den Arbeitsbereich erstellt, wird dem Arbeitsbereich automatisch als Arbeitsbereichsbesitzer hinzugefügt. Allerdings erhalten andere Administratoren oder Benutzer in diesem Abonnement nicht automatisch Zugriff auf den Arbeitsbereich – Sie müssen sie explizit einladen.
> 
> 

### <a name="to-share-a-workspace"></a>So geben Sie einen Arbeitsbereich frei

1. Melden Sie sich unter [https://studio.azureml.net/Home](https://studio.azureml.net/Home) bei Machine Learning Studio an.

2. Klicken Sie im linken Bereich auf **EINSTELLUNGEN**.

3. Klicken Sie auf die Registerkarte **BENUTZER**.

4. Klicken Sie im unteren Seitenbereich auf **WEITERE BENUTZER EINLADEN**.

    ![Studio-Einstellungen](media/machine-learning-create-workspace/settings.png)

5. Geben Sie mindestens eine E-Mail-Adresse ein. Die Benutzer benötigen ein gültiges Microsoft-Konto oder ein Organisationskonto (aus Azure Active Directory).

6. Wählen Sie, ob Benutzer als Besitzer oder Benutzer hinzugefügt werden sollen.

7. Klicken Sie auf die Häkchenschaltfläche **OK** .

Jeder hinzugefügte Benutzer erhält eine E-Mail mit Anweisungen zum Anmelden beim freigegebenen Arbeitsbereich.

> [!NOTE]
> Damit Benutzer Webdienste in diesem Arbeitsbereich bereitstellen oder verwalten können, müssen sie Mitwirkender oder Administrator im Azure-Abonnement sein. 




