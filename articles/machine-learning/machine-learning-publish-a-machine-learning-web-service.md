---
title: Bereitstellen eines Machine Learning-Webdiensts | Microsoft Docs
description: So konvertieren Sie ein Trainingsexperiment in ein Vorhersageexperiment, bereiten es auf die Bereitstellung vor und stellen es dann aus Azure Machine Learning-Webdienst bereit
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 5bcdb2cd11ffa633bce4e0faee4f49d9053a662b
ms.lasthandoff: 03/31/2017


---
# <a name="deploy-an-azure-machine-learning-web-service"></a>Bereitstellen eines Azure Machine Learning-Webdiensts
Azure Machine Learning ermöglicht es Ihnen, Vorhersageanalyselösungen zu erstellen, zu testen und bereitzustellen.

Allgemein betrachtet, geschieht dies in drei Schritten:

* **[Erstellen eines Trainingsexperiments]** : Azure Machine Learning Studio ist eine gemeinsame visuelle Entwicklungsumgebung, mit der Sie ein Vorhersageanalysemodell mithilfe von Trainingsdaten, die Sie bereitstellen, trainieren und testen können.
* **[Konvertieren in ein Vorhersageexperiment]**: Sobald Ihr Modell mit vorhandenen Daten trainiert wurde, können Sie es verwenden, um neue Daten zu bewerten. Sie bereiten das Experiment auf Vorhersagen vor und optimieren es.
* **[Bereitstellen als Webdienst:]** Sie können Ihr Vorhersageexperiment als [neuen] oder [klassischen] Azure-Webdienst bereitstellen. Die Benutzer Daten an Ihr Modell senden und die Vorhersagen Ihres Modells empfangen.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Erstellen eines Trainingsexperiments
Zum Trainieren eines Vorhersageanalysemodells erstellen Sie in Azure Machine Learning Studio ein Trainingsexperiment, in das Sie verschiedene Module zum Laden der Trainingsdaten, zum ggf. erforderlichen Vorbereiten der Daten, Anwenden von Lernalgorithmen und Auswerten der Ergebnisse einbinden. Sie können ein Experiment iterieren und unterschiedliche Lernalgorithmen zum Vergleichen und Bewerten der Ergebnisse ausprobieren.

Der Prozess der Erstellung und Verwaltung von Trainingsexperimenten wird an anderer Stelle ausführlicher behandelt. Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

* [Erstellen eines einfachen Experiments im Azure Machine Learning-Studio](machine-learning-create-experiment.md)
* [Entwickeln einer Vorhersagelösung mit Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)
* [Importieren von Trainingsdaten in Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
* [Verwalten von Experimentiterationen in Azure Machine Learning-Studio](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Konvertieren des Trainingsexperiments in ein Vorhersageexperiment
Nachdem Sie das Modell trainiert haben, können Sie Ihr Trainingsexperiment in ein Vorhersageexperiment konvertieren, um neue Daten zu bewerten.

Durch die Konvertierung in ein Vorhersageexperiment wird das trainierte Modell darauf vorbereitet, als Bewertungswebdienst bereitgestellt zu werden. Benutzer des Webdiensts können Eingabedaten an das Modell senden, und das Modell sendet Vorhersageergebnisse zurück. Während der Konvertierung in ein Vorhersageexperiment sollten Sie berücksichtigen, wie das Modell aus Ihrer Sicht erwartungsgemäß von anderen Benutzern verwendet wird.

Zum Konvertieren Ihres Trainingsexperiments in ein Vorhersageexperiment klicken Sie im Experimentbereich unten auf **Ausführen** und dann auf **Set Up Web Service**. Wählen Sie dann **Predictive Web Service** aus.

![Konvertieren in ein Bewertungsexperiment](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Weitere Informationen zum Durchführen dieser Konvertierung finden Sie unter [Vorbereiten des Modells für die Bereitstellung in Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Die folgenden Schritte zeigen, wie Sie ein Vorhersageexperiment als neuen Webdienst bereitstellen. Sie können das Experiment auch als klassischen Webdienst bereitstellen.

## <a name="deploy-it-as-a-web-service"></a>Bereitstellen des Experiments als Webdienst

Sie können das Vorhersageexperiment als neuen oder klassischen Webdienst bereitstellen.

### <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Bereitstellen des Vorhersageexperiments als neuen Webdienst
Nachdem das Vorhersageexperiment vorbereitet wurde, können Sie es neuen als Azure-Webdienst bereitstellen. Mithilfe des Webdiensts können Benutzer Daten an das Modell senden, und das Modell gibt seine Vorhersagen zurück.

Um das Vorhersageexperiment bereitzustellen, klicken Sie unten im Experimentbereich auf **Run** . Sobald das Experiment ausgeführt wurde, klicken Sie auf **Deploy Web Service**, und wählen Sie **Deploy Web Service [new]** aus.  Die Bereitstellungsseite des Machine Learning Web Service-Portals wird geöffnet.

> [!NOTE] 
> Zum Bereitstellen eines neuen Webdiensts müssen Sie über ausreichende Berechtigungen in dem Abonnement verfügen, an das Sie den Webdienst bereitstellen. Weitere Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning Web Services-Portal: Seite „Deploy Experiment“
Geben Sie auf der Seite „Deploy Experiment“ einen Namen für den Webdienst ein.
Wählen Sie einen Preisplan aus. Wenn Sie bereits einen Preisplan gewählt haben, können Sie diesen auswählen. Andernfalls müssen Sie einen Preisplan für den Dienst wählen.

1. Wählen Sie in der Dropdownliste **Price Plan** einen Preisplan aus, oder wählen Sie die Option **Select new plan** aus.
2. Geben Sie in **Plan Name**einen Namen ein, der den Plan in Ihrer Rechnung bestimmt.
3. Wählen Sie in **Monthly Plan Tiers**eine Option aus. Die Plantarife sind standardmäßig auf die Pläne für Ihre Standardregion festgelegt, und Ihr Webdienst wird in dieser Region bereitgestellt.

Klicken Sie auf **Deploy**. Die Seite **Quickstart** für Ihren Webdienst wird geöffnet.

Die Seite „Quickstart“ des Webdiensts bietet Zugriff auf und Anleitungen für die gängigsten Aufgaben, die Sie nach dem Erstellen eines neuen Webdiensts ausführen. Hier können Sie auch auf die Seiten „Test“ und „Consume“ zugreifen.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Testen des neuen Webdiensts
Klicken Sie zum Testen Ihres neuen Webdiensts unter „Common Tasks“ auf **Test web service** . Auf der Seite „Test“ können Sie Ihren Webdienst als Anforderung-/Antwort-Dienst (Request-Response Service, RRS) oder Stapelausführungsdienst (Batch Execution Service, BES) testen.

Die RRS-Testseite zeigt die Eingaben, Ausgaben und alle globalen Parameter, die Sie für das Experiment definiert haben. Um den Webdienst zu testen, können Sie als Eingaben geeignete Werte manuell eingeben oder eine CSV-Datei mit den Testwerten angeben.

Zum Testen mit dem RRS geben Sie im Listenansichtsmodus für die Eingaben geeignete Werte ein und klicken dann auf **Test Request-Response**. Ihre Vorhersageergebnisse werden in der Ausgabespalte links angezeigt.

![Bereitstellen des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Klicken Sie zum Testen Ihres BES auf **Batch**. Klicken Sie auf der Testseite „Batch“ unter Ihrer Eingabe auf „Browse“, und wählen Sie eine CSV-Datei mit den entsprechenden Beispielwerten aus. Wenn Sie keine CSV-Datei haben und Ihr Vorhersageexperiment mithilfe von Machine Learning Studio erstellt haben, können Sie das Dataset für Ihr Vorhersageexperiment herunterladen und nutzen.

Um das Dataset herunterzuladen, öffnen Sie Machine Learning Studio. Öffnen Sie das Vorhersageexperiment, und klicken Sie mit der rechten Maustaste auf die Eingabe für das Experiment. Wählen Sie im Kontextmenü **Dataset** und dann **Download** aus.

![Bereitstellen des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klicken Sie auf **Test**. Der Status Ihres Batchausführungsauftrags wird rechts unter **Test Batch Jobs**angezeigt.

![Bereitstellen des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Auf der Seite **CONFIGURATION** können Sie die Beschreibung und den Titel ändern, den Speicherkontoschlüssel aktualisieren und Beispieldaten für Ihren Webdienst aktivieren.

![Konfigurieren des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Nachdem Sie den Webdienst bereitgestellt haben, können Sie Folgendes tun:

* **zugreifen** 
* **verwalten** 
* **aktualisieren** , wenn sich Ihr Modell ändert

#### <a name="access-your-new-web-service"></a>Zugreifen auf den neuen Webdienst
Nachdem Sie den Webdienst in Machine Learning Studio bereitgestellt haben, können Sie Daten an den Dienst senden und programmgesteuert Antworten erhalten.

Die Seite **Consume** bietet alle Informationen, die Sie benötigen, um auf den Webdienst zuzugreifen. Beispielsweise wird der API-Schlüssel bereitgestellt, um den autorisierten Zugriff auf den Dienst zuzulassen.

Weitere Informationen zum Zugreifen auf einen Machine Learning-Webdienst finden Sie unter [Nutzen eines bereitgestellten Azure Machine Learning-Webdiensts](machine-learning-consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Verwalten des neuen Webdiensts
Sie können Ihre neuen Webdienste im Machine Learning Web Services-Portal verwalten. Klicken Sie auf der [Hauptseite des Portals](https://services.azureml-test.net/) auf **Web Services**. Auf der Seite „Web Services“ können Sie einen Dienst löschen oder kopieren. Um einen bestimmten Dienst zu überwachen, klicken Sie auf den Dienst und dann auf **Dashboard**. Klicken Sie zum Überwachen von Batchaufträgen im Zusammenhang mit dem Webdienst auf **Batch Request Log**.

### <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Bereitstellen des Vorhersageexperiments als klassischen Webdienst

Nachdem das Vorhersageexperiment ausreichend vorbereitet wurde, können Sie es als klassischen Azure-Webdienst bereitstellen. Mithilfe des Webdiensts können Benutzer Daten an das Modell senden, und das Modell gibt seine Vorhersagen zurück.

Um das Vorhersageexperiment bereitzustellen, klicken Sie unten im Experimentbereich auf **Run** und anschließend auf **Deploy Web Service**. Der Webdienst wird eingerichtet, und Sie werden zum Webdienst-Dashboard weitergeleitet.

![Bereitstellen des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Testen des klassischen Webdiensts

Sie können den Webdienst im Machine Learning-Webdienstportal oder in Machine Learning Studio testen.

Klicken Sie zum Testen des Anforderung-/Antwort-Webdiensts im Dashboard des Webdiensts auf die Schaltfläche **Test**. Ein Dialogfeld wird geöffnet, in dem nach den Eingabedaten für den Dienst gefragt werden. Hierbei handelt es sich um die Spalten, die vom Bewertungsexperiment erwartet werden. Geben Sie eine Reihe von Daten ein, und klicken Sie auf **OK**. Die vom Webdienst generierten Ergebnisse werden jetzt unten im Dashboard angezeigt.

Sie können auf den Vorschaulink **Test** klicken, um Ihren Dienst im Azure Machine Learning-Webdienstportal zu testen (wie im vorherigen Abschnitt „Neuer Webdienst“ gezeigt).

Klicken Sie zum Testen des Stapelausführungsdiensts auf den Vorschaulink **Test**. Klicken Sie auf der Testseite „Batch“ unter Ihrer Eingabe auf „Browse“, und wählen Sie eine CSV-Datei mit den entsprechenden Beispielwerten aus. Wenn Sie keine CSV-Datei haben und Ihr Vorhersageexperiment mithilfe von Machine Learning Studio erstellt haben, können Sie das Dataset für Ihr Vorhersageexperiment herunterladen und nutzen.

![Testen des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Auf der Registerkarte **CONFIGURATION** können Sie den Anzeigenamen des Diensts ändern und eine Beschreibung eingeben. Der Name und die Beschreibung werden im [klassischen Azure-Portal](http://manage.windowsazure.com/) angezeigt, in dem Sie Ihre Webdienste verwalten.

Sie können eine Beschreibung für die Eingabedaten, Ausgabedaten und Webdienstparameter bereitstellen, indem Sie Zeichenfolgen für jede Spalte unter **INPUT SCHEMA**, **OUTPUT SCHEMA** und **WEB SERVICE PARAMETER** eingeben. Diese Beschreibungen werden in der Beispielcodedokumentation für den Webdienst verwendet.

Sie können die Protokollierung aktivieren, um Fehler zu diagnostizieren, die angezeigt werden, wenn auf den Webdienst zugegriffen wird. Weitere Informationen hierzu finden Sie unter [Aktivieren der Protokollierung für Machine Learning-Webdienste](machine-learning-web-services-logging.md).

![Konfigurieren des Webdiensts](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Sie können auch die Endpunkte für den Webdienst im Azure Machine Learning-Webdienstportal konfigurieren (wie im zuvor im Abschnitt „Neuer Webdienst“ gezeigten Verfahren). Die Optionen sind unterschiedlich. Sie können die Dienstbeschreibung hinzufügen oder ändern, die Protokollierung aktivieren und Beispieldaten für Tests aktivieren.

#### <a name="access-your-classic-web-service"></a>Zugreifen auf den klassischen Webdienst
Nachdem Sie den Webdienst in Machine Learning Studio bereitgestellt haben, können Sie Daten an den Dienst senden und programmgesteuert Antworten erhalten.

Das Dashboard bietet alle Informationen, die Sie benötigen, um auf den Webdienst zuzugreifen. Zum Beispiel wird der API-Schlüssel bereitgestellt, um den autorisierten Zugriff auf den Dienst zu ermöglichen, und API-Hilfeseiten werden bereitgestellt, um das Schreiben von Code zu erleichtern.

Weitere Informationen zum Zugreifen auf einen Machine Learning-Webdienst finden Sie unter [Nutzen eines bereitgestellten Azure Machine Learning-Webdiensts](machine-learning-consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Verwalten des klassischen Webdiensts
Es gibt verschiedene Aktionen, die Sie durchführen können, um einen Webdienst zu überwachen. Sie können ihn aktualisieren und löschen. Sie können einem klassischen Webdienst auch zusätzlich zum Standardendpunkt, der beim Bereitstellen erstellt wird, weitere Endpunkte hinzufügen.

Weitere Informationen finden Sie unter [Verwalten eines Azure Machine Learning-Arbeitsbereichs](machine-learning-manage-workspace.md) und [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Aktualisieren des Webdiensts
Sie können Änderungen am Webdienst vornehmen, z. B. das Modell mit zusätzlichen Trainingsdaten aktualisieren, und ihn dann erneut bereitstellen, wobei der ursprüngliche Webdienst überschrieben wird.

Um den Webdienst zu aktualisieren, öffnen Sie das ursprüngliche Vorhersageexperiment, das Sie zum Bereitstellen des Webdiensts verwendet haben, und klicken Sie auf **SPEICHERN UNTER**, um eine bearbeitbare Kopie zu erstellen. Nehmen Sie die gewünschten Änderungen vor, und klicken Sie dann auf **Webdienst bereitstellen**.

Da Sie dieses Experiment zuvor bereitgestellt haben, werden Sie gefragt, ob Sie den vorhandenen Dienst überschreiben (klassischer Webdienst) oder aktualisieren (neuer Webdienst) möchten. Wenn Sie auf **YES** oder **Update** klicken, wird der vorhandene Webdienst beendet und das neue Vorhersageexperiment an seiner Stelle bereitgestellt.

> [!NOTE]
> Wenn Sie Konfigurationsänderungen im ursprünglichen Webdienst vorgenommen haben, z. B. einen anderen Anzeigenamen oder eine andere Beschreibung eingegeben haben, dann müssen Sie diese Werte erneut eingeben.
> 
> 

Eine Option zum Aktualisieren des Webdiensts ist das programmgesteuerte erneute Trainieren des Modells. Weitere Informationen finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](machine-learning-retrain-models-programmatically.md).

<!-- internal links -->
[Erstellen eines Trainingsexperiments]: #create-a-training-experiment
[Konvertieren in ein Vorhersageexperiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Bereitstellen als Webdienst:]: #deploy-it-as-a-web-service
[neuen]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klassischen]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service

