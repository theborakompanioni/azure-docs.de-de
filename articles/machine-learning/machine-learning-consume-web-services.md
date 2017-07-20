---
title: Nutzen eines Azure Machine Learning-Webdiensts | Microsoft-Dokumentation
description: "Sobald ein Machine Learning-Dienst bereitgestellt wurde, kann der zur Verfügung gestellte RESTFul-Webdienst als Anforderung-/Antwort-Dienst in Echtzeit oder als Stapelausführungsdienst genutzt werden."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 804f8211-9437-4982-98e9-ca841b7edf56
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/02/2017
ms.author: garye
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: eec9f637b4b2306ab4a888dbd5ef5b9a021bcac5
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="how-to-consume-an-azure-machine-learning-web-service"></a>Nutzen eines Azure Machine Learning-Webdiensts

Sobald Sie ein Azure Machine Learning-Vorhersagemodell als Webdienst bereitstellen, können Sie eine REST-API verwenden, um Daten dorthin zu senden und Vorhersagen abzurufen. Sie können die Daten in Echtzeit oder im Batchmodus senden.

Weitere Informationen zum Erstellen und Bereitstellen eines Machine Learning-Webdiensts mit Machine Learning Studio finden Sie hier:

* Ein Tutorial zum Erstellen eines Experiments in Machine Learning Studio finden Sie unter [Erstellen Ihres ersten Experiments](machine-learning-create-experiment.md).
* Ausführliche Informationen zum Bereitstellen eines Webdiensts finden Sie unter [Bereitstellen eines Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md).
* Allgemeine Informationen zu Machine Learning finden Sie im [Machine Learning-Dokumentationscenter](https://azure.microsoft.com/documentation/services/machine-learning/).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Übersicht
Mit dem Azure Machine Learning-Webdienst kommunizieren externe Anwendungen in Echtzeit mit einem Machine Learning-Workflow-Bewertungsmodell. Ein Machine Learning-Webdienstaufruf gibt Vorhersageergebnisse an eine externe Anwendung zurück. Zur Durchführung eines Machine Learning-Webdienstaufrufs übergeben Sie einen API-Schlüssel, der beim Bereitstellen einer Vorhersage erstellt wird. Der Machine Learning-Webdienst basiert auf REST, einer verbreiteten Architektur für Webprogrammierungsprojekte.

Azure Machine Learning verfügt über zwei Arten von Diensten:

* Antwort-/Anfrage-Dienst (Request-Response Service, RRS): ein hochskalierbarer Webdienst mit niedriger Latenz, der eine Schnittstelle für zustandslose Modelle bereitstellt, die in Machine Learning Studio erstellt und bereitgestellt wurden.
* Batch Execution Service (BES) – ein asynchroner Dienst für die Bewertung eines Stapels für Datensätze.

Weitere Informationen zu Machine Learning-Webdiensten finden Sie unter [Bereitstellen eines Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Abrufen eines Autorisierungsschlüssels für Azure Machine Learning
Wenn Sie Ihr Experiment bereitstellen, werden API-Schlüssel für den Webdienst erstellt. Sie können die Schlüssel von verschiedenen Stellen abrufen.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Im Azure Machine Learning Web Services-Portal
Melden Sie sich beim [Microsoft Azure Machine Learning Web Services-Portal](https://services.azureml.net) an.

So rufen Sie den API-Schlüssel für einen neuen Machine Learning-Webdienst ab

1. Klicken Sie im Azure Machine Learning Web Services-Portal im oberen Menü auf **Web Services**.
2. Klicken Sie auf den Webdienst, für den der Schlüssel abgerufen werden soll.
3. Klicken Sie im oberen Menü auf **Consume**.
4. Kopieren und speichern Sie den **Primary Key**.

So rufen Sie den API-Schlüssel für einen klassischen Machine Learning-Webdienst ab

1. Klicken Sie im Azure Machine Learning Web Services-Portal im oberen Menü auf **Classic Web Services**.
2. Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
3. Klicken Sie auf den Endpunkt, für den der Schlüssel abgerufen werden soll.
4. Klicken Sie im oberen Menü auf **Consume**.
5. Kopieren und speichern Sie den **Primary Key**.

### <a name="classic-web-service"></a>Klassischer Webdienst
 Sie können auch einen Schlüssel für einen klassischen Webdienst aus Machine Learning Studio oder dem klassischen Azure-Portal abrufen.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. Klicken Sie in Machine Learning Studio links auf **WEB SERVICES** .
2. Klicken Sie auf einen Webdienst. Der **API Key** befindet sich auf der Registerkarte **DASHBOARD**.

#### <a name="azure-classic-portal"></a>Klassisches Azure-Portal
1. Klicken Sie links auf **MACHINE LEARNING**.
2. Klicken Sie auf den Arbeitsbereich, in dem sich der Webdienst befindet.
3. Klicken Sie auf **WEB SERVICES**.
4. Klicken Sie auf einen Webdienst.
5. Klicken Sie auf einen Endpunkt. "API KEY" befindet sich unten rechts.

## <a id="connect"></a>Herstellen der Verbindung mit einem Machine Learning-Webdienst
Sie können mit jeder Programmiersprache, die HTTP-Anforderungen und -Antworten unterstützt, eine Verbindung mit einem Machine Learning-Webdienst herstellen. Sie können Beispiele in C#, Python und R auf einer Hilfeseite für den Machine Learning-Webdienst anzeigen.

**Hilfe zur Machine Learning-API** Wenn Sie einen Webdienst bereitstellen, wird Hilfe zur Machine Learning-API erstellt. Weitere Informationen finden Sie unter [Azure Machine Learning – exemplarische Vorgehensweise – Veröffentlichen eines Webdiensts](machine-learning-walkthrough-5-publish-web-service.md).
Die Machine Learning-API-Hilfeseite enthält Details zu einem Vorhersagewebdienst.

1. Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
2. Klicken Sie auf den Endpunkt, für den die API-Hilfeseite angezeigt werden soll.
3. Klicken Sie im oberen Menü auf **Consume**.
4. Klicken Sie unter den Endpunkten „Request-Response“ oder „Batch Execution“ auf **API help page**.

**So zeigen Sie Machine Learning-API-Hilfe für einen neuen Webdienst an**

Im Azure Machine Learning Web Services-Portal:

1. Klicken Sie im oberen Menü auf **WEB SERVICES**.
2. Klicken Sie auf den Webdienst, für den der Schlüssel abgerufen werden soll.

Klicken Sie auf **Consume** , um die URIs für die Dienste „Request-Reposonse“ und „Batch Execution“ und Beispielcode in C#, R und Python abzurufen.

Klicken Sie auf **Swagger API**, um auf Swagger basierende Dokumentation zu den APIs abzurufen, die über die angegebenen URIs aufgerufen werden.

### <a name="c-sample"></a>C#-Beispiel
Verwenden Sie zum Verbinden mit einem Machine Learning-Webdienst einen **HttpClient** durch Übergeben von ScoreData. ScoreData enthält ein FeatureVector, ein n-dimensionaler Vektor von numerischen Funktionen, die die ScoreData darstellen. Sie authentifizieren sich mit einem API-Schlüssel beim Machine Learning-Dienst.

Für die Verbindung mit einem Machine Learning-Webdienst muss das NuGet-Paket **Microsoft.AspNet.WebApi.Client** installiert sein.

**Installieren von Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**

1. Veröffentlichen Sie das Dataset Download von Webdienst „UCI:  Adult 2 class dataset“.
2. Klicken Sie auf **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
3. Wählen Sie **Install-Package Microsoft.AspNet.WebApi.Client aus**.

**So führen Sie das Codebeispiel aus**

1. Veröffentlichen Sie das Experiment „Sample 1: Download dataset from UCI: Adult 2 class dataset“, Teil der Machine Learning-Beispielauflistung.
2. Weisen Sie „apiKey“ den Schlüssel von einem Webdienst zu. Siehe oben: **Abrufen eines Autorisierungsschlüssels für Azure Machine Learning** .
3. Weisen Sie "serviceUri" die Anforderungs-URI zu.

### <a name="python-sample"></a>Python-Beispiel
Verwenden Sie zum Verbinden mit einem Machine Learning-Webdienst die **urllib2**-Bibliothek durch Übergeben von „ScoreData“. „ScoreData“ enthält ein „FeatureVector“-Element, ein n-dimensionaler Vektor von numerischen Features, die „ScoreData“ darstellen. Sie authentifizieren sich mit einem API-Schlüssel beim Machine Learning-Dienst.

**So führen Sie das Codebeispiel aus**

1. Stellen Sie das Experiment „Sample 1: Download dataset from UCI: Adult 2 class dataset“ bereit, das Teil der Machine Learning-Beispielsammlung ist.
2. Weisen Sie „apiKey“ den Schlüssel von einem Webdienst zu. Im Abschnitt **Abrufen eines Autorisierungsschlüssels für Azure Machine Learning** zu Beginn dieses Artikels finden Sie weitere Informationen.
3. Weisen Sie "serviceUri" die Anforderungs-URI zu.


