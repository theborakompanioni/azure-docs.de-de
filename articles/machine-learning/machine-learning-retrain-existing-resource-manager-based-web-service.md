---
title: "Einen vorhandenen prädiktiven Webdienst erneut trainieren | Microsoft Docs"
description: Erfahren Sie, wie Sie ein Modell erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Machine Learning verwendet.
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
translationtype: Human Translation
ms.sourcegitcommit: 247d370c1f80729856e53690045991127ad54351
ms.openlocfilehash: dc4716a26ba5c2ba93dda83890dbff81f22cbd2a
ms.lasthandoff: 03/02/2017


---
# <a name="retrain-an-existing-predictive-web-service"></a>Einen vorhandenen prädiktiven Webdienst erneut trainieren
Dieses Dokument beschreibt das erneute Trainieren für das folgende Szenario:

* Sie haben ein Trainingsexperiment und ein Vorhersageexperiment, die Sie als operationalisierten Webdienst bereitgestellt haben.
* Sie haben neue Daten, die Ihr prädiktiver Webdienst zum Ausführen der Bewertung verwenden soll.

> [!NOTE] 
> Zum Bereitstellen eines neuen Webdiensts müssen Sie über ausreichende Berechtigungen in dem Abonnement verfügen, an das Sie den Webdienst bereitstellen. Weitere Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md). 

Beginnen Sie mit dem vorhandenen Webdienst und Experimenten, und führen Sie die folgenden Schritte aus:

1. Aktualisieren Sie das Modell.
   1. Ändern Sie das Trainingsexperiment, um Webdienst-Eingaben und -Ausgaben zu ermöglichen.
   2. Stellen Sie das Trainingsexperiment als Webdienst zum erneuten Trainieren bereit.
   3. Verwenden Sie den Stapelausführungsdienst (Batch Execution Service, BES) des Trainingsexperiments, um das Modell erneut zu trainieren.
2. Verwenden Sie die Azure Machine Learning-PowerShell-Cmdlets, um das prädiktive Experiment zu aktualisieren.
   1. Melden Sie sich bei Ihrem Azure Resource Manager-Konto an.
   2. Rufen Sie die Webdienstdefinition ab.
   3. Exportieren Sie die Webdienstdefinition als JSON-Code.
   4. Aktualisieren Sie im JSON-Code den Verweis auf das iLearner-Blob.
   5. Importieren Sie den JSON-Code in eine Webdienstdefinition.
   6. Aktualisieren Sie den Webdienst mit der neuen Webdienstdefinition.

## <a name="deploy-the-training-experiment"></a>Bereitstellen des Trainingsexperiments
Um das Trainingsexperiment als Webdienst zum erneuten Trainieren bereitzustellen, müssen Sie Webdienst-Eingaben und -Ausgaben dem Modell hinzufügen. Durch Herstellen einer Verbindung zwischen einem Modul *Web Service Output* und dem Modul *[Train Model][train-model]* ermöglichen Sie dem Trainingsexperiment, ein neues trainiertes Modell zu erstellen, das Sie in Ihrem Vorhersageexperiment verwenden können. Wenn Sie ein Modul *Evaluate Model* (Modell auswerten) haben, können Sie auch Webdienst-Ausgaben zum Abrufen der Ergebnisse der Auswertung als Ausgabe anfügen.

So aktualisieren Sie das Trainingsexperiment:

1. Verbinden Sie ein *Web Service Input* (Webdienst-Eingabe)-Modul mit der Dateneingabe (z.B. ein Modul *Clean Missing Data* (Fehlende Daten bereinigen)). Dadurch wird sichergestellt, dass Ihre Daten für das erneute Training genau so verarbeitet werden wie Ihre ursprünglichen Trainingsdaten.
2. Verbinden Sie ein *Web Service Output*-Modul mit der Ausgabe Ihres *Train Model*-Moduls.
3. Wenn Sie ein Modul *Evaluate Model* haben und die Ergebnisse der Auswertung ausgeben möchten, verbinden Sie ein *Web Service Output*-Modul mit der Ausgabe Ihres *Evaluate Model*-Moduls.

Führen Sie das Experiment aus.

Als nächstes müssen Sie das Trainingsexperiment als Webdienst bereitstellen, der ein trainiertes Modell und Modellauswertungsergebnisse erzeugt.  

Klicken Sie am unteren Rand des Experimentbereichs auf **Webdienst einrichten**, und wählen Sie **Webdienst bereitstellen [Neu]** aus. Das Azure Machine Learning-Webdienstportal wird mit der Seite **Webdienst bereitstellen** geöffnet. Geben Sie einen Namen für den Webdienst ein, wählen Sie einen Zahlungsplan aus, und klicken Sie anschließend auf **Bereitstellen**. Beachten Sie, dass nur die Batchausführungsmethode zum Erstellen von trainierten Modellen verwendet werden kann.

## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Erneutes Trainieren des Modells mit neuen Daten mithilfe von BES
In diesem Beispiel dient C# zum Erstellen der Anwendung für erneutes Trainieren. Für diese Aufgabe können Sie auch Python- oder R-Beispielcode verwenden.

So rufen Sie die APIs zum erneuten Trainieren auf:

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio (**Neu** > **Projekt** > **Windows-Desktop** > **Konsolenanwendung**).
2. Melden Sie sich beim Machine Learning-Webdienstportal an.
3. Klicken Sie auf den verwendeten Webdienst.
4. Klicken Sie auf **Consume**.
5. Klicken Sie am unteren Rand der Seite **Consume** im Abschnitt **Sample Code** auf **Batch**.
6. Kopieren Sie den C#-Beispielcode für die Batchausführung, und fügen Sie ihn in die Datei „Program.cs“ ein. Achten Sie dabei darauf, dass der Namespace intakt bleibt.

Fügen Sie das NuGet-Paket „Microsoft.AspNet.WebApi.Client“ hinzu, wie in den Kommentaren angegeben. Vor dem Hinzufügen des Verweises auf „Microsoft.WindowsAzure.Storage.dll“ müssen Sie unter Umständen zuerst die [Clientbibliothek für Azure Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage) installieren.

Der folgende Screenshot zeigt die Seite **Consume** im Azure Machine Learning-Webdienste-Portal.

![Seite Consume][1]

### <a name="update-the-apikey-declaration"></a>Aktualisieren der apiKey-Deklaration
Suchen Sie die **apiKey**-Deklaration:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Kopieren Sie auf der Seite **Consume** im Abschnitt **Basic consumption info** den Primärschlüssel, und fügen Sie ihn in die **apiKey**-Deklaration ein.

### <a name="update-the-azure-storage-information"></a>Aktualisieren der Azure Storage-Informationen
Der BES-Beispielcode lädt eine Datei von einem lokalen Laufwerk (z.B. „C:\temp\CensusIpnput.csv“) in Azure Storage hoch, verarbeitet sie und schreibt die Ergebnisse zurück in Azure Storage.  

Zum Aktualisieren von Azure Storage Informationen müssen Sie den Namen des Speicherkontos, Schlüssel und Container-Informationen für das Speicherkonto aus dem Azure-Portal abrufen, und dann die entsprechenden Nach Ausführen des Experiments sollte der resultierende Workflow etwa wie folgt aussehen:

![Resultierender Workflow nach der Ausführung][4]Werte im Code aktualisieren.

1. Melden Sie sich am klassischen Azure-Portal an.
2. Klicken Sie im linken Navigationsbereich auf **Storage**.
3. Wählen Sie in der Speicherkontenliste ein Speicherkonto zum Speichern des neu trainierten Modells aus.
4. Klicken Sie im unteren Bereich der Seite auf **Zugriffsschlüssel verwalten**.
5. Kopieren und speichern Sie den primären Zugriffsschlüssel **** , und schließen Sie anschließend das Dialogfeld.
6. Klicken Sie im oberen Bereich der Seite auf **Container**.
7. Wählen Sie einen vorhandenen Container aus, oder erstellen Sie einen neuen, und speichern Sie den Namen.

Suchen Sie die Deklarationen *StorageAccountName*, *StorageAccountKey* und *StorageContainerName*, und aktualisieren Sie die Werte mit den gespeicherten Werten aus dem klassischen Azure-Portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Außerdem müssen Sie sicherstellen, dass die Eingabedatei an dem im Code angegebenen Speicherort verfügbar ist.

### <a name="specify-the-output-location"></a>Angeben des Ausgabespeicherorts
Wenn Sie den Ausgabespeicherort in der Anforderungsnutzlast angeben, muss die Erweiterung der in *RelativeLocation* angegebenen Datei als `ilearner` angegeben werden. Siehe folgendes Beispiel:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Nachfolgend eine Beispielausgabe zum erneuten Trainieren: ![Ausgabe des erneuten Trainings][6]

## <a name="evaluate-the-retraining-results"></a>Auswerten der Ergebnisse des erneuten Trainings
Wenn Sie die Anwendung ausführen, enthält die Ausgabe die URL und das erforderliche SAS-Token für den Zugriff auf die Auswertungsergebnisse.

Sie können die Leistungsergebnisse des erneut trainierten Modells anzeigen, indem Sie *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen für *output2* kombinieren (wie in der obigen Abbildung zur Ausgabe des erneuten Trainings gezeigt), und die vollständige URL in die Adressleiste des Browsers einfügen.  

Überprüfen Sie die Ergebnisse, um festzustellen, ob das neu trainierte Modell gut genug ist, um das vorhandene Modell zu ersetzen.

Kopieren Sie die Werte für *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen.

## <a name="retrain-the-web-service"></a>Erneutes Trainieren des Webdiensts
Beim erneuten Trainieren eines neuen Webdiensts wird die Definition des Vorhersagewebdiensts so aktualisiert, dass sie auf das neue trainierte Modell verweist. Die Webdienstdefinition ist eine interne Darstellung des trainierten Modells für den Webdienst und kann nicht direkt geändert werden. Achten Sie darauf, die Webdienstdefinition für das Vorhersageexperiment (und nicht für das Trainingsexperiment) abzurufen.

## <a name="sign-in-to-azure-resource-manager"></a>Anmelden bei Azure Resource Manager
Melden Sie sich innerhalb Ihrer PowerShell-Umgebung zunächst mithilfe des Cmdlets [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) bei Ihrem Azure-Konto an.

## <a name="get-the-web-service-definition-object"></a>Abrufen des Webdienstdefinition-Objekts
Rufen Sie als nächstes mithilfe des Cmdlets [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) das Webdienstdefinition-Objekt ab.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Führen Sie zum Ermitteln des Ressourcengruppennamens eines vorhandenen Webdiensts das Cmdlet „Get-AzureRmMlWebService“ ohne Parameter aus, um die Webdienste in Ihrem Abonnement anzuzeigen. Suchen Sie den Webdienst, und sehen Sie sich die Webdienst-ID an. Der Ressourcengruppenname ist das vierte Element in der ID (direkt nach dem *resourceGroups* -Element). Im folgenden Beispiel lautet der Name der Ressourcengruppe „Default-MachineLearning-SouthCentralUS“.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativ können Sie sich zur Ermittlung des Ressourcengruppennamens eines vorhandenen Webdiensts beim Azure Machine Learning-Webdiensteportal anmelden. Wählen Sie den Webdienst aus. Der Ressourcengruppenname ist das fünfte Element in der Webdienst-URL (direkt nach dem *resourceGroups* -Element). Im folgenden Beispiel lautet der Name der Ressourcengruppe „Default-MachineLearning-SouthCentralUS“.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Exportieren des Webdienstdefinition-Objekts als JSON-Code
Um die Definition so zu ändern, dass das neu trainierte Modell verwendet wird, müssen Sie sie zunächst mithilfe des Cmdlets [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) in eine Datei im JSON-Format exportieren.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Aktualisieren des Verweises auf das iLearner-Blob
Suchen Sie in den Assets nach „[trained model]“, und aktualisieren Sie den Wert *uri* im Knoten *locationInfo* mit dem URI des iLearner-Blobs. Der URI wird auf der Grundlage von *BaseLocation* und *RelativeLocation* aus der Ausgabe des BES-Aufrufs für das erneute Training generiert.

     "asset3": {
        "name": "Retrain Sample [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition-object"></a>Importieren des JSON-Codes in ein Webdienstdefinition-Objekt
Konvertieren Sie die geänderte JSON-Datei mithilfe des Cmdlets [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) wieder in ein Webdienstdefinition-Objekt, mit dem Sie das Vorhersageexperiment aktualisieren können.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Aktualisieren des Webdiensts
Verwenden Sie abschließend das Cmdlet [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx), um das Vorhersageexperiment zu aktualisieren.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

