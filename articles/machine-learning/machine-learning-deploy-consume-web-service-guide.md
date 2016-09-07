<properties
	pageTitle="Azure Machine Learning-Webdienste: Bereitstellung und Nutzung | Microsoft Azure"
	description="Ressourcen zum Bereitstellen und Nutzen von Webdiensten."
	services="machine-learning"
	documentationCenter=""
	authors="vDonGlover"
	manager="raymondl"
	editor=""/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/19/2016"
	ms.author="v-donglo"/>

# Azure Machine Learning-Webdienste: Bereitstellung und Nutzung

Mit Azure Machine Learning (Azure ML) können Sie Machine Learning-Workflows und -Modelle als Webdienste bereitstellen. Diese Webdienste können dann verwendet werden, um die ML-Modelle in Anwendungen über das Internet aufzurufen und Vorhersagen im Echtzeit- oder Batchmodus zu nutzen. Die Webdienste sind RESTful und können daher über verschiedene Programmiersprachen und Plattformen (etwa .NET und Java) sowie über Anwendungen (etwa Excel) aufgerufen werden.

In den nächsten Abschnitten finden Sie Informationen zu den Schritten für den Einstieg sowie Links zu Code und zur Dokumentation.

## Bereitstellen

### Mithilfe von Azure ML Studio

Mit Machine Learning Studio und dem Microsoft Azure Machine Learning-Webdiensteportal können Sie einen Webdienst ganz ohne Programmieraufwand bereitstellen und verwalten.

Unter den folgenden Links finden Sie allgemeine Informationen zur Bereitstellung eines neuen Webdiensts:

* Eine Übersicht über das Bereitstellen eines neuen Azure Resource Manager-basierten Webdiensts finden Sie unter [Bereitstellen eines neuen Webdiensts](machine-learning-webservice-deploy-a-web-service.md).
* Eine exemplarische Vorgehensweise zum Bereitstellen eines Webdiensts finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md).

Eine umfassende exemplarische Vorgehensweise zum Erstellen und Bereitstellen eines Webdiensts finden Sie unter [Exemplarische Vorgehensweise, Schritt 1: Erstellen eines Machine Learning-Arbeitsbereichs](machine-learning-walkthrough-1-create-ml-workspace.md).

Spezifische Beispiele für das Bereitstellen eines Webdiensts finden Sie hier:

* [Anleitung Schritt 5: Bereitstellen des Azure Machine Learning-Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
* [Gewusst wie: Bereitstellen eines Webdiensts in mehreren Regionen](machine-learning-how-to-deploy-to-multiple-regions.md)

### Mithilfe von Web Services-Ressourcenanbieter-APIs (Azure Resource Manager-APIs)

Der Azure ML-Ressourcenanbieter für Webdienste ermöglicht die Bereitstellung und Verwaltung von Webdiensten mithilfe von REST-API-Aufrufen. Ausführlichere Informationen finden Sie in den folgenden Artikeln sowie im Beispielcode.

* [Machine Learning-Webdienst (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx)-Referenz auf MSDN.


### Mithilfe von PowerShell-Cmdlets

Der Azure ML-Ressourcenanbieter für Webdienste ermöglicht die Bereitstellung und Verwaltung von Webdiensten über PowerShell-Cmdlets.

Um die Cmdlets verwenden zu können, müssen Sie sich innerhalb Ihrer PowerShell-Umgebung zunächst mithilfe des Cmdlets [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) bei Ihrem Azure-Konto anmelden. Informationen zum Aufrufen Resource Manager-basierter PowerShell-Befehle finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Verwenden Sie [diesen Beispielcode](https://github.com/ritwik20/AzureML-WebServices), um Ihr Vorhersageexperiment zu exportieren. Nachdem Sie die EXE-Datei auf der Grundlage des Codes erstellt haben, können Sie Folgendes eingeben:

	C:<folder>\GetWSD <experiment-url> <workspace-auth-token>

Durch Ausführen der Anwendung wird eine JSON-Webdienstvorlage erstellt. Fügen Sie folgende Informationen hinzu, um mithilfe dieser Vorlage einen Webdienst bereitzustellen:

* Speicherkontoname und -schlüssel
	* Den Namen des Speicherkontos und den Schlüssel können Sie sowohl über das neue als auch über das klassische Azure-Portal ermitteln.
* Vertragsplan-ID
	* Die Plan-ID können Sie über das [Azure Machine Learning-Webdiensteportal](https://services.azureml.net) ermitteln, indem Sie sich anmelden und auf einen Plannamen klicken.

Fügen Sie der JSON-Vorlage die Werte als untergeordnete Elemente des Knotens *Eigenschaften* auf der Ebene des Knotens *MachineLearningWorkspace* hinzu.

Beispiel:

	"StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
	},
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Ausführlichere Informationen finden Sie in den folgenden Artikeln sowie im Beispielcode.

* Referenz zu [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) auf MSDN
* [Exemplarische Vorgehensweise](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) auf GitHub

## Nutzen der Webdienste

### Über die Benutzeroberfläche der Azure ML-Webdienste (Test)

Sie können Ihren Webdienst über das Azure ML-Webdiensteportal testen. Dies beinhaltet das Testen der RRS- und der BES-Schnittstelle.

* [Bereitstellen eines neuen Webdiensts](machine-learning-webservice-deploy-a-web-service.md)
* [Bereitstellen eines Azure Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md)
* [Anleitung Schritt 5: Bereitstellen des Azure Machine Learning-Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)

### Über Excel

Sie können eine Excel-Vorlage zur einfachen Nutzung des Webdiensts herunterladen:

* [Verwenden eines Azure Machine Learning-Webdiensts aus Excel](machine-learning-consuming-from-excel.md)
* [Excel-Add-In für Azure Machine Learning-Webdienste](machine-learning-excel-add-in-for-web-services.md)


### Über einen REST-basierten Client

Bei den Azure ML-Webdiensten handelt es sich um RESTful-APIs. Diese APIs können über verschiedene Plattformen (.NET, Python, R, Java usw.) genutzt werden. Im [Microsoft Azure Machine Learning-Webdiensteportal](https://services.azureml.net) steht auf der Nutzungsseite Ihres Webdiensts Beispielcode zur Verfügung, der Ihnen den Einstieg erleichtert.

* [Nutzen eines Azure Machine Learning-Webdiensts, der von einem Machine Learning-Experiment aus bereitgestellt wurde](machine-learning-consume-web-services.md)

<!---HONumber=AcomDC_0824_2016-->