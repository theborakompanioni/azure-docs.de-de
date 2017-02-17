---
title: Bereitstellen eines neuen Webdiensts in Azure Machine Learning | Microsoft-Dokumentation
description: Der Workflows zur Bereitstellung eines ARM-basierten Webdiensts
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: machine-learning-publish-a-machine-learning-web-service
translationtype: Human Translation
ms.sourcegitcommit: 0813611f581a68efb8f09a1e041cfbe429bf0c5c
ms.openlocfilehash: 902be47eb59444a1214b096be10525743f406d1c


---
# <a name="deploy-a-new-web-service"></a>Bereitstellen eines neuen Webdiensts
Microsoft Azure Machine Learning bietet nun Webdienste, die auf [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) basieren, neue Abrechnungsplanoptionen zulassen und in mehreren Regionen bereitgestellt werden können.

Es folgt der allgemeine Workflow zum Bereitstellen eines Webdiensts mit Microsoft Azure Machine Learning Web Services:

* Erstellen eines Vorhersageexperiments
* Bereitstellen dieses Experiments
* Festlegen seines Namens
* Abrechnungsplan
* Testen des Experiments
* Nutzen des Experiments

Die folgende Abbildung veranschaulicht diesen Workflow.

![Workflow für die Bereitstellung eines Webdiensts][1]

## <a name="deploy-web-service-from-studio"></a>Bereitstellen des Webdiensts aus Studio
So stellen Sie ein Experiment als neuen Webdienst bereit. Melden Sie sich bei Machine Learning Studio an, und erstellen Sie einen neuen Vorhersagewebdienst. 

**Hinweis**: Wenn Sie ein Experiment bereits als klassischen Webdienst bereitgestellt haben, können Sie es nicht als neuen Webdienst bereitstellen.

Klicken Sie unten im Experimentbereich auf **Run** und anschließend auf **Deploy Web Service** und **Deploy Web Service [New]**. Die Bereitstellungsseite im Machine Learning Web Service Manager wird geöffnet.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning Web Service Manager – Seite „Deploy Experiment“
Geben Sie auf der Seite „Deploy Experiment“ einen Namen für den Webdienst ein.
Wählen Sie einen Preisplan aus. Wenn Sie bereits einen Preisplan gewählt haben, können Sie diesen auswählen. Andernfalls müssen Sie einen Preisplan für den Dienst wählen. 

1. Wählen Sie in der Dropdownliste **Price Plan** einen Preisplan aus, oder wählen Sie die Option **Select new plan** aus.
2. Geben Sie in **Plan Name**einen Namen ein, der den Plan in Ihrer Rechnung bestimmt.
3. Wählen Sie in **Monthly Plan Tiers**eine Option aus. Beachten Sie, dass die Plantarife standardmäßig auf die Pläne für Ihre Standardregion und Ihren Webdienst festgelegt sind, der in dieser Region bereitgestellt ist.

Klicken Sie auf **Deploy**. Die Seite „Quickstart“ für Ihren Webdienst wird geöffnet.

## <a name="quickstart-page"></a>Seite „Quickstart“
Die Seite „Quickstart“ des Webdiensts bietet Zugriff auf und Anleitungen für die gängigsten Aufgaben, die Sie nach dem Erstellen eines neuen Webdiensts ausführen. Hier können Sie auch auf die Seiten **Test** und **Consume** zugreifen.

## <a name="testing-your-web-service"></a>Testen des Webdiensts
Klicken Sie auf der Seite „Quickstart“ unter „Common tasks“ auf „Test web service“.   

So testen Sie den Webdienst als Request-Response Service (RRS)

* Klicken Sie auf der Menüleiste auf **Test** .
* Klicken Sie auf **Request-Response**.
* Geben Sie für die Eingabespalten des Experiments geeignete Werte ein.
* Klicken Sie auf **Test Request-Response**.

Ihre Ergebnisse werden rechts auf der Seite angezeigt.

Um einen Batch Execution Service (BES) zu testen, verwenden Sie eine CSV-Datei:

* Klicken Sie auf der Menüleiste auf **Test** .
* Klicken Sie auf **Batch**.
* Klicken Sie unter „Input“ auf „Browse“, und wechseln Sie zu Ihrer Datei mit den Beispieldaten.
* Klicken Sie auf **Test**.

Der Status des Tests wird unter **Test Batch Jobs**angezeigt.

## <a name="consuming-your-web-service"></a>Nutzen des Webdiensts
Beim Bereitstellen als Webdienst bieten Azure Machine Learning-Experimente eine REST-API, die von einer Vielzahl von Geräten und Plattformen genutzt werden kann, denn die einfache REST-API akzeptiert JSON-formatierte Nachrichten und antwortet mit diesen. Das Azure Machine Learning-Portal bietet Code zum Aufrufen des Webdiensts in R, C# und Python.

Auf der Seite „Consuming“ finden Sie Folgendes:

* API-Schlüssel und URIs für die Nutzung des Webdiensts in Apps.
* Excel- und Web-App-Vorlagen zum Beschleunigen des Nutzungsvorgangs.
* Beispielcode in C#, Python und R für die ersten Schritte.

Weitere Informationen zum Nutzen von Webdiensten finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts, der von einem Machine Learning-Experiment aus bereitgestellt wurde](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Webdiensten finden Sie in folgenden Themen:

[Nutzen eines Azure Machine Learning-Webdiensts, der von einem Machine Learning-Experiment aus bereitgestellt wurde](machine-learning-consume-web-services.md)

[Azure Machine Learning-Webdienste: Bereitstellung und Nutzung](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->



<!--HONumber=Jan17_HO4-->


