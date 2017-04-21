---
title: "Problembehandlung für das erneute Trainieren eines klassischen Azure Machine Learning-Webdiensts | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie häufige Probleme erkennen und beheben, die beim erneuten Trainieren des Modells für einen Azure Machine Learning-Webdienst auftreten können."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
translationtype: Human Translation
ms.sourcegitcommit: 66fb3dc316ce25aea4dff4add5c25b7f0f56ad7a
ms.openlocfilehash: 5bbd1bd74176b67beaecfc9995d16e923e5a40f1
ms.lasthandoff: 01/31/2017


---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Problembehandlung für das erneute Trainieren eines klassischen Azure Machine Learning-Webdiensts
## <a name="retraining-overview"></a>Erneutes Trainieren – Übersicht
Wenn Sie ein Vorhersageexperiment als Bewertungswebdienst bereitstellen, handelt es sich um ein statisches Modell. Wenn neue Daten verfügbar sind oder der Endkunde der API über eigene Daten verfügt, muss für das Modell das erneute Trainieren durchgeführt werden. 

Die vollständige exemplarische Vorgehensweise des erneuten Trainierens eines klassischen Webdiensts finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Prozess für das erneute Trainieren
Wenn Sie den Webdienst erneut trainieren möchten, müssen Sie einige zusätzliche Teile hinzufügen:

* Einen Webdienst, der über das Trainingsexperiment bereitgestellt wird. Für das Experiment muss an das **Web Service Output**-Modul das **Train Model**-Modul angefügt werden.  
  
    ![Webdienstausgabe an Trainingsmodell anfügen][image1]
* Einen neuen Endpunkt, der dem Bewertungswebdienst hinzugefügt wird.  Sie können den Endpunkt programmgesteuert mit dem Beispielcode hinzufügen, der im Thema „Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen“ angegeben ist, oder Sie können das klassische Azure-Portal verwenden.

Sie können anschließend den C#-Beispielcode von der API-Hilfeseite des Trainingswebdiensts verwenden, um das Modell erneut zu trainieren. Nachdem Sie die Ergebnisse ausgewertet haben und damit zufrieden sind, aktualisieren Sie den Bewertungswebdienst des trainierten Modells, indem Sie den neu hinzugefügten Endpunkt verwenden.

Wenn alle Elemente vorhanden sind, können Sie diese Hauptschritte zu erneuten Trainieren des Modells ausführen:

1. Aufrufen des Trainingswebdiensts: Hierbei wird der Stapelausführungsdienst (Batch Execution Service, BES) aufgerufen und nicht der Anforderung/Antwort-Dienst (Request Response Service, RRS). Sie können den C#-Beispielcode auf der API-Hilfeseite verwenden, um den Aufruf durchzuführen. 
2. Suchen nach den Werten für *BaseLocation*, *RelativeLocation* und *SasBlobToken*: Diese Werte werden in der Ausgabe des Aufrufs des Trainingswebdiensts zurückgegeben. 
   ![Ausgabe des Beispiels für das erneute Trainieren und Werte „BaseLocation“, „RelativeLocation“ und „SasBlobToken“][image6]
3. Aktualisieren des hinzugefügten Endpunkts über den Bewertungswebdienst mit dem neuen trainierten Modell: Aktualisieren Sie mit dem Beispielcode aus „Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen“ den neuen Endpunkt, den Sie dem Bewertungsmodell mit dem neu trainierten Modell aus dem Trainingswebdienst hinzugefügt haben.

## <a name="common-obstacles"></a>Häufige Hindernisse
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Überprüfen des Vorhandenseins der richtigen PATCH-URL
Die von Ihnen verwendete PATCH-URL muss die dem neuen Bewertungsendpunkt zugeordnete URL sein, den Sie dem Bewertungswebdienst hinzugefügt haben. Es gibt verschiedene Möglichkeiten, die PATCH-URL zu erhalten:

**Option 1: Programmgesteuert**

Gehen Sie wie folgt vor, um die richtige PATCH-URL abzurufen:

1. Führen Sie den [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs)-Beispielcode aus.
2. Suchen Sie in der Ausgabe von AddEndpoint nach dem Wert *HelpLocation*, und kopieren Sie die URL.
   
   ![HelpLocation in der Ausgabe des addEndpoint-Beispiels][image2]
3. Fügen Sie die URL in einen Browser ein, um auf eine Seite zu navigieren, auf der Hilfelinks für den Webdienst angegeben sind.
4. Klicken Sie auf den Link **Ressource aktualisieren** , um die Hilfeseite für das Patching zu öffnen.

**Option 2: Verwenden des klassischen Azure-Portals**

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.
2. Öffnen Sie die Registerkarte „Machine Learning“. 
   ![Registerkarte „Machine Learning“][image4]
3. Klicken Sie auf den Namen Ihres Arbeitsbereichs und dann auf **Webdienste**.
4. Klicken Sie auf den Bewertungswebdienst, den Sie verwenden. (Wenn Sie den Standardnamen des Webdiensts nicht geändert haben, endet er mit „[Scoring Exp.]“.)
5. Klicken Sie auf **Endpunkt hinzufügen**.
6. Klicken Sie nach dem Hinzufügen des Endpunkts auf dessen Namen. Klicken Sie anschließend auf **Ressource aktualisieren** , um die Hilfeseite für das Patching zu öffnen.

> [!NOTE]
> Wenn Sie den Endpunkt nicht dem Vorhersagewebdienst, sondern dem Trainingswebdienst hinzugefügt haben, tritt beim Klicken auf den Link **Ressource aktualisieren** der folgende Fehler auf: „Sorry, but this feature is not supported or available in this context. This Web Service has no updatable resources. We apologize for the inconvenience and are working on improving this workflow.“ (Dieses Feature wird in diesem Kontext leider nicht unterstützt oder ist nicht verfügbar. Der Webdienst besitzt keine aktualisierbaren Ressourcen. Wir arbeiten bereits an einer Optimierung dieses Workflows.)
> 
> 

![Dashboard für neuen Endpunkt][image3]

Die PATCH-Hilfeseite enthält die PATCH-URL, die Sie verwenden müssen, und außerdem Beispielcode, den Sie zum Aufrufen verwenden können.

![Patch-URL][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Sicherstellen, dass der richtige Bewertungsendpunkt aktualisiert wird
* Führen Sie das Patching nicht für den Trainingswebdienst durch: Der Patchvorgang muss für den Bewertungswebdienst erfolgen.
* Führen Sie das Patching nicht für den Standardendpunkt des Webdiensts durch: Der Patchvorgang muss für den neuen Bewertungswebdienst-Endpunkt erfolgen, den Sie hinzugefügt haben.

Sie können überprüfen, unter welchem Webdienst sich der Endpunkt befindet, indem Sie das klassische Azure-Portal verwenden. 

> [!NOTE]
> Achten Sie darauf, dass Sie den Endpunkt dem Vorhersagewebdienst hinzufügen und nicht dem Trainingswebdienst. Wenn Sie sowohl einen Trainings- als auch einen Vorhersagewebdienst korrekt bereitgestellt haben, werden zwei separate Webdienste aufgeführt. Der Vorhersagewebdienst sollte mit „[predictive exp.]“ enden.
> 
> 

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Öffnen Sie die Registerkarte „Machine Learning“. 
   ![ Benutzeroberfläche des Machine Learning-Arbeitsbereichs][image4]
3. Wählen Sie Ihren Arbeitsbereich aus.
4. Klicken Sie auf **Webdienste**.
5. Wählen Sie Ihren Vorhersagewebdienst aus.
6. Stellen Sie sicher, dass der neue Endpunkt dem Webdienst hinzugefügt wurde.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Sicherstellen, dass sich der Webdienst-Arbeitsbereich in der richtigen Region befindet
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im Menü die Option „Machine Learning“.
   ![Benutzeroberfläche der Machine Learning-Region][image4]
3. Überprüfen Sie den Speicherort Ihres Arbeitsbereichs.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png

