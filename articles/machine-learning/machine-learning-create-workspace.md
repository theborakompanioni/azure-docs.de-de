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
ms.date: 11/21/2016
ms.author: garye;bradsev;ahgyger
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 647398c1a0994da04845672e89d9f82d5e8b6d21


---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Erstellen und Freigeben eines Azure Machine Learning-Arbeitsbereichs
Dieses Menü enthält Links zu Themen, in denen das Einrichten der verschiedenen Data Science-Umgebungen, die vom Cortana-Analyseprozess (CAP) verwendet werden, beschrieben wird.

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Um Azure Machine Learning Studio verwenden zu können, benötigen Sie einen Machine Learning-Arbeitsbereich. Dieser Arbeitsbereich enthält die Tools, die zum Erstellen, Verwalten und Veröffentlichen von Experimenten erforderlich sind.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="to-create-a-workspace"></a>Erstellen eines Arbeitsbereichs
1. Melden Sie sich beim [klassische Microsoft Azure-Portal]an.

> [!NOTE]
> Hierfür müssen Sie ein Azure-Abonnementadministrator sein. Als Besitzer eines Machine Learning-Arbeitsbereichs haben Sie keinen Zugriff auf das [klassische Microsoft Azure-Portal]. Weitere Details finden Sie unter [Berechtigungen von Azure-Abonnementadministratoren und Besitzern von Arbeitsbereichen](#subscriptionvsworkspace) .
> 
> 

1. Klicken Sie im Microsoft Azure-Dienstbereich auf **MACHINE LEARNING**.
   
    ![Machine Learning-Dienst][1]
2. Klicken Sie im unteren Seitenbereich auf **+NEU** .
3. Klicken Sie auf **DATA SERVICES** > **MACHINE LEARNING** > **SCHNELLERFASSUNG**.
   
    ![Schnellerfassung eines neuen Arbeitsbereichs][3]
4. Geben Sie einen **ARBEITSBEREICHSNAMEN** für den Arbeitsbereich ein.
5. Geben Sie den Azure-**SPEICHERORT** und dann ein vorhandenes **Azure-SPEICHERKONTO** an, oder wählen Sie **Neues Speicherkonto erstellen** aus, um ein neues zu erstellen.
6. Klicken Sie auf **ML-ARBEITSBEREICH ERSTELLEN**.

Nach dem Erstellen Ihres Machine Learning-Arbeitsbereichs wird dieser auf der Seite **Machine Learning** angezeigt.

## <a name="sharing-an-azure-machine-learning-workspace"></a>Freigeben eines Azure Machine Learning-Arbeitsbereichs
Sobald ein Machine Learning-Arbeitsbereich erstellt wurde, können Sie Benutzer zu Ihrem Arbeitsbereich einladen sowie den Zugriff auf Ihren Arbeitsbereich und alle zugehörigen Experimenten freigeben. Wir unterstützen zwei Rollen von Benutzern:

* **Benutzer** : Ein Arbeitsbereichsbenutzer kann Datasets, Experimente und Webdienste im Arbeitsbereich erstellen, öffnen, ändern und löschen.
* **Besitzer** : Ein Besitzer kann (zusätzlich zu den Möglichkeiten eines Benutzers) Benutzer mit Zugriff auf den Arbeitsbereich einladen, entfernen und auflisten. Besitzer haben auch Zugriff auf Notebooks.

### <a name="to-share-a-workspace"></a>So geben Sie einen Arbeitsbereich frei
1. Melden Sie sich bei [Machine Learning Studio]
2. Klicken Sie im Bereich „Machine Learning Studio“ auf **SETTINGS**
3. Klicken Sie im unteren Seitenbereich auf **USERS**
4. Klicken Sie im unteren Seitenbereich auf **INVITE MORE USERS**
   
    ![INVITE MORE USERS][4]
5. Geben Sie eine oder mehrere E-Mail-Adressen ein. Der Benutzer benötigt lediglich ein gültiges Microsoft-Konto (z.B. name@outlook.com)) oder ein Organisationskonto (aus Azure Active Directory).
6. Klicken Sie dann auf das Häkchen.

Jeder hinzugefügte Benutzer empfängt eine E-Mail mit Anweisungen zum Anmelden beim freigegebenen Arbeitsbereich.

Informationen zum Verwalten des Arbeitsbereichs finden Sie unter [Verwalten eines Azure Machine Learning-Arbeitsbereichs].
Sollte ein Problem beim Erstellen des Arbeitsbereichs auftreten, finden Sie weitere Informationen unter [Handbuch zur Problembehandlung: Erstellen und Verbinden eines Machine Learning-Arbeitsbereichs].

## <a name="a-namesubscriptionvsworkspaceaprivileges-of-azure-subscription-administrator-and-of-workspace-owner"></a><a name="subscriptionvsworkspace"></a>Berechtigungen von Azure-Abonnementadministratoren und Besitzern von Arbeitsbereichen
Es folgt eine Tabelle zur Verdeutlichung der Unterschiede zwischen einem Azure-Abonnementadministrator und Besitzer eines Arbeitsbereichs.

| Actions | Azure-Abonnementadministrator | Besitzer des Arbeitsbereichs |
| --- |:---:|:---:|
| Zugriff auf das [klassische Microsoft Azure-Portal] |Ja |Nein |
| Erstellen eines neuen Arbeitsbereichs |Ja |Nein |
| Löschen eines Arbeitsbereichs |Ja |Nein |
| Hinzufügen eines Endpunkts für einen Webdienst  |Ja |Nein |
| Löschen eines Endpunkts aus einen Webdienst  |Ja |Nein |
| Ändern der Parallelität für einen Webdienst |Ja |Nein |
| Zugriff auf [Machine Learning Studio] |Nein * |Ja |

> [!NOTE]
> * Ein Azure-Abonnementadministrator wird automatisch dem Arbeitsbereich hinzugefügt, den er als Arbeitsbereichsbesitzer erstellt. Azure-Abonnementadministrator zu sein allein gewährt ihm jedoch noch keinen Zugriff auf einen Arbeitsbereich unter dem Abonnement.
> 
> 

<!-- ![List of Machine Learning workspaces][2] -->

<!--Anchors-->
[Erstellen eines Arbeitsbereichs]: #createworkspace

<!--Image references-->
[1]: media/machine-learning-create-workspace/cw1.png
[2]: media/machine-learning-create-workspace/cw2.png
[3]: media/machine-learning-create-workspace/cw4.png
[4]: media/machine-learning-create-workspace/cw5.png


<!--Link references-->
[Verwalten eines Azure Machine Learning-Arbeitsbereichs]: machine-learning-manage-workspace.md
[Handbuch zur Problembehandlung: Erstellen und Verbinden eines Machine Learning-Arbeitsbereichs]: machine-learning-troubleshooting-creating-ml-workspace.md
[Machine Learning Studio]: https://studio.azureml.net/  
[klassische Microsoft Azure-Portal]: https://manage.windowsazure.com/



<!--HONumber=Nov16_HO3-->


