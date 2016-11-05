---
title: Erneutes Trainieren eines neuen Webdiensts mithilfe der PowerShell-Cmdlets für die Verwaltung von Machine Learning | Microsoft Docs
description: Hier erfahren Sie, wie Sie mithilfe der PowerShell-Cmdlets für die Verwaltung von Machine Learning ein Modell programmgesteuert erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Machine Learning verwendet.
services: machine-learning
documentationcenter: ''
author: vDonGlover
manager: raymondlaghaeian
editor: ''

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: v-donglo

---
# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Erneutes Trainieren eines neuen Webdiensts mithilfe der PowerShell-Cmdlets für die Verwaltung von Machine Learning
Beim erneuten Trainieren eines neuen Webdiensts wird die Definition des Vorhersagewebdiensts so aktualisiert, dass sie auf das neue trainierte Modell verweist.  

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein Trainingsexperiment und ein Vorhersageexperiment (wie unter „Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen“ beschrieben). Informationen zum Erstellen des Trainings- und des Vorhersageexperiments finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](machine-learning-retrain-models-programmatically.md).

Für diesen Prozess müssen die Azure Machine Learning-Cmdlets installiert sein. Informationen zum Installieren der Machine Learning-Cmdlets finden Sie in der Referenz zu [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) auf MSDN.

Kopieren Sie folgende Informationen aus der Ausgabe des erneuten Trainings:

* BaseLocation
* RelativeLocation

Auszuführende Schritte:

1. Melden Sie sich bei Ihrem Azure Resource Manager-Konto an.
2. Rufen Sie die Webdienstdefinition ab.
3. Exportieren Sie die Webdienstdefinition als JSON-Code.
4. Aktualisieren Sie im JSON-Code den Verweis auf das iLearner-Blob.
5. Importieren Sie den JSON-Code in eine Webdienstdefinition.
6. Aktualisieren Sie den Webdienst mit der neuen Webdienstdefinition.

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Anmelden bei Ihrem Azure Resource Manager-Konto
Melden Sie sich innerhalb Ihrer PowerShell-Umgebung zunächst mithilfe des Cmdlets [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) bei Ihrem Azure-Konto an.

## <a name="get-the-web-service-definition"></a>Rufen Sie die Webdienstdefinition ab.
Rufen Sie als Nächstes mithilfe des Cmdlets [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) den Webdienst ab. Die Webdienstdefinition ist eine interne Darstellung des trainierten Modells für den Webdienst und kann nicht direkt geändert werden. Achten Sie darauf, die Webdienstdefinition für Ihr Vorhersageexperiment (und nicht für Ihr Trainingsexperiment) abzurufen.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Führen Sie zum Ermitteln des Ressourcengruppennamens eines vorhandenen Webdiensts das Cmdlet „Get-AzureRmMlWebService“ ohne Parameter aus, um die Webdienste in Ihrem Abonnement anzuzeigen. Suchen Sie den Webdienst, und sehen Sie sich die Webdienst-ID an. Der Ressourcengruppenname ist das vierte Element in der ID (direkt nach dem *resourceGroups* -Element). Im folgenden Beispiel lautet der Name der Ressourcengruppe „Default-MachineLearning-SouthCentralUS“.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativ können Sie sich zur Ermittlung des Ressourcengruppennamens eines vorhandenen Webdiensts beim Microsoft Azure Machine Learning-Webdiensteportal anmelden. Wählen Sie den Webdienst aus. Der Ressourcengruppenname ist das fünfte Element in der Webdienst-URL (direkt nach dem *resourceGroups* -Element). Im folgenden Beispiel lautet der Name der Ressourcengruppe „Default-MachineLearning-SouthCentralUS“.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exportieren Sie die Webdienstdefinition als JSON-Code.
Um die Definition so zu ändern, dass das neu trainierte Modell verwendet wird, müssen Sie sie zunächst mithilfe des Cmdlets [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) in eine Datei im JSON-Format exportieren.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json."></a>Aktualisieren Sie im JSON-Code den Verweis auf das iLearner-Blob.
Suchen Sie in den Assets nach „[trained model]“, und aktualisieren Sie den Wert *uri* im Knoten *locationInfo* mit dem URI des iLearner-Blobs. Der URI wird auf der Grundlage von *BaseLocation* und *RelativeLocation* aus der Ausgabe des BES-Aufrufs für das erneute Training generiert.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Importieren Sie den JSON-Code in eine Webdienstdefinition.
Konvertieren Sie die geänderte JSON-Datei mithilfe des Cmdlets [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) wieder in eine Webdienstdefinition, mit der Sie das Vorhersageexperiment aktualisieren können.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Aktualisieren Sie den Webdienst mit der neuen Webdienstdefinition.
Verwenden Sie abschließend das Cmdlet [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) , um das Vorhersageexperiment zu aktualisieren.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Zusammenfassung
Mithilfe der PowerShell-Cmdlets für die Verwaltung von Machine Learning können Sie das trainierte Modell eines Vorhersagewebdiensts aktualisieren. Dies ermöglicht beispielsweise folgende Szenarien:

* Regelmäßiges erneutes Trainieren des Modells mit neuen Daten
* Verteilung eines Modells an Kunden, sodass sie das Modell mit ihren eigenen Daten erneut trainieren können

<!--HONumber=Oct16_HO2-->


