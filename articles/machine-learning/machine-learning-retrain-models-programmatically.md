---
title: Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen | Microsoft Docs
description: Erfahren Sie, wie Sie ein Modell programmgesteuert erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Azure Machine Learning verwendet.
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
translationtype: Human Translation
ms.sourcegitcommit: 0dc0f84ed65e76a95dcac523e42268039212773c
ms.openlocfilehash: 5032e66556622e44e5f5bba84c4ddf5318b1e194
ms.lasthandoff: 03/02/2017


---
# <a name="retrain-machine-learning-models-programmatically"></a>Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen
In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie einen Azure Machine Learning-Webdienst mithilfe von C# und dem Machine Learning Batch Execution-Dienst neu trainieren.

Nachdem Sie das Modell neu trainiert haben, veranschaulichen die folgenden exemplarischen Vorgehensweisen das Aktualisieren des Modells in Ihrem Vorhersagewebdienst:

* Wenn Sie im Machine Learning-Webdienstportal einen klassischen Webdienst bereitgestellt haben, lesen Sie [Erneutes Trainieren eines klassischen Webdiensts](machine-learning-retrain-a-classic-web-service.md). 
* Wenn Sie einen neuen Webdienst bereitgestellt haben, lesen Sie [Erneutes Trainieren eines neuen Webdiensts mithilfe der Cmdlets für die Verwaltung von Machine Learning](machine-learning-retrain-new-web-service-using-powershell.md).

Eine Übersicht über den Prozess zum erneuten Trainieren finden Sie unter [Erneutes Trainieren eines Machine Learning-Modells](machine-learning-retrain-machine-learning-model.md).

Wenn Sie mit Ihrem vorhandenen neuen auf Azure Resource Manager basierenden Webdienst beginnen möchten, lesen Sie [Erneutes Trainieren eines vorhandenen Vorhersagewebdiensts](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Erstellen eines Trainingsexperiments
In diesem Beispiel verwenden Sie „Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset“ aus den Microsoft Azure Machine Learning-Beispielen. 

So erstellen Sie das Experiment

1. Melden Sie sich bei Microsoft Azure Machine Learning Studio an. 
2. Klicken Sie in der unteren rechten Ecke des Dashboards auf **Neu**.
3. Wählen Sie in den Microsoft-Beispielen das Beispiel 5 aus.
4. Wählen Sie zum Umbenennen des Experiments am oberen Rand des Experimentbereichs den Experimentnamen „Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset“ aus.
5. Geben Sie „Census Model“ ein.
6. Klicken Sie am unteren Rand des Experimentbereichs auf **Run**.
7. Klicken Sie auf **Set Up Web Service**, und wählen Sie **Retraining Web Service** aus. 

Das folgende Beispiel zeigt das anfängliche Experiment.
   
   ![Anfängliches Experiment][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Erstellen eines Vorhersageexperiments und Veröffentlichen als Webdienst
Erstellen Sie als Nächstes ein Vorhersageexperiment.

1. Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service**, und wählen Sie **Predictive Web Service** aus. Dadurch wird das Modell als trainiertes Modell gespeichert, und es werden Ein- und Ausgabemodule für den Webdienst hinzugefügt. 
2. Klicken Sie auf **Run**(Ausführen). 
3. Klicken Sie nach der Ausführung des Experiments auf **Deploy Web Service [Classic]** oder auf **Deploy Web Service [New]**.

> [!NOTE] 
> Zum Bereitstellen eines neuen Webdiensts müssen Sie über ausreichende Berechtigungen in dem Abonnement verfügen, an das Sie den Webdienst bereitstellen. Weitere Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md). 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Bereitstellen des Trainingsexperiments als Trainingswebdienst
Um das trainierte Modell erneut zu trainieren, müssen Sie das erstellte Trainingsexperiment als Webdienst für erneutes Training bereitstellen. Dieser Webdienst benötigt ein mit dem Modul *[Train Model][train-model]* verbundenes *Webdienst-Ausgabemodul*, um neue trainierte Modelle erzeugen zu können.

1. Um zum Trainingsexperiment zurückzukehren, klicken Sie im linken Bereich auf das Symbol für Experimente, und klicken Sie dann auf das Experiment namens „Census Model“.  
2. Geben Sie im Suchfeld „Search Experiment Items“ den Suchbegriff „Webdienst“ ein. 
3. Ziehen Sie ein *Web Service Input*-Modul in den Experimentbereich, und stellen Sie eine Verbindung zwischen der Ausgabe und dem *Clean Missing Data*-Modul her.  Dadurch wird sichergestellt, dass Ihre Daten für das erneute Training genau so verarbeitet werden wie Ihre ursprünglichen Trainingsdaten.
4. Ziehen Sie zwei *Web Service Output*-Module in den Experimentbereich. Verbinden Sie die Ausgabe des *Train Model*-Moduls mit dem einem Modul und die Ausgabe des *Evaluate Model*-Moduls mit dem anderen Modul. Die Webdienstausgabe für **Train Model** liefert das neue trainierte Modell. Die mit **Evaluate Model** verknüpfte Ausgabe gibt die Ausgabe dieses Moduls zurück (die Leistungsergebnisse).
5. Klicken Sie auf **Run**(Ausführen). 

Als Nächstes müssen Sie das Trainingsexperiment als Webdienst bereitstellen, der ein trainiertes Modell und Modellauswertungsergebnisse erzeugt. Ihre nächsten Schritte hängen davon ab, ob Sie einen klassischen oder einen neuen Webdienst verwenden.  

**Klassischer Webdienst**

Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service**, und wählen Sie **Deploy Web Service [Classic]** aus. Das **Dashboard** des Webdiensts wird mit dem API-Schlüssel und der API-Hilfeseite für die Batchausführung angezeigt. Beachten Sie, dass nur die Batchausführungsmethode zum Erstellen von trainierten Modellen verwendet werden kann.

**Neuer Webdienst**

Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service**, und wählen Sie **Deploy Web Service [New]** aus. Das Azure Machine Learning-Webdienstportal wird mit der Seite für die Webdienstbereitstellung geöffnet. Geben Sie einen Namen für Ihren Webdienst ein, wählen Sie einen Zahlungsplan aus, und klicken Sie anschließend auf **Bereitstellen**. Zum Erstellen trainierter Modelle kann nur die Batchausführungsmethode verwendet werden.

Nach Abschluss der Experimentausführung sieht der resultierende Workflow wie folgt aus:

![Resultierender Workflow nach der Ausführung][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a>Erneutes Trainieren des Modells mit neuen Daten mithilfe von BES
In diesem Beispiel verwenden Sie C# zum Erstellen der Anwendung für erneutes Trainieren. Für diese Aufgabe können Sie auch Python- oder R-Beispielcode verwenden.

So rufen Sie die APIs zum erneuten Trainieren auf

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio (Neu > Projekt > Windows-Desktop > Konsolenanwendung).
2. Melden Sie sich beim Machine Learning-Webdienstportal an.
3. Klicken Sie bei Verwendung eines klassischen Webdiensts auf **Classic Web Services**.
   1. Klicken Sie auf den verwendeten Webdienst.
   2. Klicken Sie auf den Standardendpunkt.
   3. Klicken Sie auf **Consume**.
   4. Klicken Sie am unteren Rand der Seite **Consume** im Abschnitt **Sample Code** auf **Batch**.
   5. Fahren Sie mit Schritt 5 dieses Verfahrens fort.
4. Klicken Sie bei Verwendung eines neuen Webdiensts auf **Web Services**.
   1. Klicken Sie auf den verwendeten Webdienst.
   2. Klicken Sie auf **Consume**.
   3. Klicken Sie am unteren Rand der Seite „Consume“ im Abschnitt **Sample Code** auf **Batch**.
5. Kopieren Sie den C#-Beispielcode für die Batchausführung, und fügen Sie ihn in die Datei „Program.cs“ ein. Achten Sie dabei darauf, dass der Namespace intakt bleibt.

Fügen Sie das NuGet-Paket „Microsoft.AspNet.WebApi.Client“ hinzu, wie in den Kommentaren angegeben. Vor dem Hinzufügen des Verweises auf „Microsoft.WindowsAzure.Storage.dll“ müssen Sie unter Umständen zuerst die Clientbibliothek für Microsoft Azure Storage Services installieren. Weitere Informationen finden Sie unter [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Aktualisieren der apiKey-Deklaration
Suchen Sie die **apiKey** -Deklaration.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Kopieren Sie auf der Seite **Consume** im Abschnitt **Basic consumption info** den Primärschlüssel, und fügen Sie ihn in die **apiKey**-Deklaration ein.

### <a name="update-the-azure-storage-information"></a>Aktualisieren der Azure Storage-Informationen
Der BES-Beispielcode lädt eine Datei von einem lokalen Laufwerk (z.B. „C:\temp\CensusIpnput.csv“) in Azure Storage hoch, verarbeitet sie und schreibt die Ergebnisse zurück in Azure Storage.  

Für diese Aufgabe müssen Sie über das klassische Azure-Portal den Namen, Schlüssel und Container Ihres Speicherkontos ermitteln und anschließend die entsprechenden Werte im Code aktualisieren. 

1. Melden Sie sich beim klassischen Azure-Portal an.
2. Klicken Sie im linken Navigationsbereich auf **Storage**.
3. Wählen Sie in der Speicherkontenliste ein Speicherkonto zum Speichern des neu trainierten Modells aus.
4. Klicken Sie im unteren Bereich der Seite auf **Zugriffsschlüssel verwalten**.
5. Kopieren und speichern Sie den primären Zugriffsschlüssel **** , und schließen Sie anschließend das Dialogfeld. 
6. Klicken Sie im oberen Bereich der Seite auf **Container**.
7. Wählen Sie einen vorhandenen Container aus, oder erstellen Sie einen neuen, und speichern Sie den Namen.

Suchen Sie die Deklarationen *StorageAccountName*, *StorageAccountKey* und *StorageContainerName*, und aktualisieren Sie die Werte mit den gespeicherten Werten aus dem Azure-Portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Außerdem müssen Sie sicherstellen, dass die Eingabedatei an dem im Code angegebenen Speicherort verfügbar ist. 

### <a name="specify-the-output-location"></a>Angeben des Ausgabespeicherorts
Wenn Sie den Ausgabespeicherort in der Anforderungsnutzlast angeben, muss die Erweiterung der in *RelativeLocation* angegebenen Datei als „ilearner“ angegeben werden. 

Siehe folgendes Beispiel:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> Die Namen Ihrer Ausgabespeicherorte können sich abhängig von der Reihenfolge, in der Sie die Ausgabemodule für den Webdienst hinzugefügt haben, von denen in dieser exemplarischen Vorgehensweise unterscheiden. Da Sie dieses Trainingsexperiment mit zwei Ausgaben einrichten, enthalten die Ergebnisse Speicherortinformationen für beide Ausgaben.  
> 
> 

![Ausgabe des erneuten Trainings][6]

Diagramm 4: Ausgabe des erneuten Trainings

## <a name="evaluate-the-retraining-results"></a>Auswerten der Ergebnisse des erneuten Trainings
Wenn Sie die Anwendung ausführen, enthält die Ausgabe die URL und das erforderliche SAS-Token für den Zugriff auf die Auswertungsergebnisse.

Sie können die Leistungsergebnisse des erneut trainierten Modells anzeigen, indem Sie *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen für *output2* kombinieren (wie in der obigen Abbildung zur Ausgabe des erneuten Trainings gezeigt) und die vollständige URL in die Adressleiste des Browsers einfügen.  

Überprüfen Sie die Ergebnisse, um festzustellen, ob das neu trainierte Modell gut genug ist, um das vorhandene Modell zu ersetzen.

Kopieren Sie die Werte für *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen. Diese werden für den Prozess zum erneuten Trainieren benötigt.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie den Vorhersagewebdienst durch Klicken auf **Deploy Web Service [Classic]** bereitgestellt haben, finden Sie weitere Informationen unter [Erneutes Trainieren eines klassischen Webdiensts](machine-learning-retrain-a-classic-web-service.md).

Wenn Sie den Vorhersagewebdienst durch Klicken auf **Deploy Web Service [New]** bereitgestellt haben, finden Sie weitere Informationen unter [Erneutes Trainieren eines neuen Webdiensts mithilfe der Cmdlets für die Verwaltung von Machine Learning](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

