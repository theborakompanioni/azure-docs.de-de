<properties
	pageTitle="Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie ein Modell programmgesteuert erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Machine Learning verwendet."
	services="machine-learning"
	documentationCenter=""
	authors="raymondlaghaeian"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/28/2016"
	ms.author="raymondl;garye;v-donglo"/>


#Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen  

Im Rahmen der Operationalisierung von Machine Learning-Modellen in Azure Machine Learning wird Ihr Modell trainiert und gespeichert. Anschließend kann es dann zum Erstellen eines Vorhersagewebdiensts verwendet werden. Dieser Webdienst kann von Websites, Dashboards und mobilen Apps genutzt werden.

Mithilfe von Machine Learning erstellte Modelle sind in der Regel nicht statisch. Wenn neue Daten verfügbar sind oder der Endkunde der API über eigene Daten verfügt, muss das Modell erneut trainiert werden. Es kann auch vorkommen, dass Sie zum Abrufen einer Teilmenge der Daten Filter anwenden und das Modell erneut trainieren müssen.

Das erneute Training kann häufig durchgeführt werden. Mit dem API-Feature für programmgesteuertes erneutes Trainieren können Sie das Modell mithilfe der APIs zum erneuten Trainieren programmgesteuert erneut trainieren und den Webdienst mit dem neu trainierten Modell aktualisieren.

In diesem Dokument wird das erneute Trainieren beschrieben und gezeigt, wie Sie die APIs zum erneuten Trainieren verwenden.

## Warum erneut trainieren: Problemdefinition  

Im Rahmen des Machine Learning-Trainingsprozesses wird ein Modell mit einem Satz von Daten trainiert. Mithilfe von Machine Learning erstellte Modelle sind in der Regel nicht statisch. Wenn neue Daten verfügbar sind oder der Nutzer der API über eigene Daten verfügt, muss das Modell erneut trainiert werden. In anderen Szenarien kann es erforderlich sein, zum Abrufen einer Teilmenge der Daten Filter anzuwenden und das Modell erneut zu trainieren.

In diesen Szenarien bietet eine programmgesteuerte API eine komfortable Möglichkeit, mit der Sie oder die Nutzer Ihrer APIs einen Client erstellen können, der das Modell einmalig oder regelmäßig unter Verwendung eigener Daten trainieren kann. Anschließend können Sie die Ergebnisse des erneuten Trainierens auswerten und die Webdienst-API aktualisieren, damit sie das neu trainierte Modell verwendet.

##Erneutes Trainieren: Prozess  

Der Prozess umfasst ein Trainingsexperiment und ein Vorhersageexperiment (veröffentlicht als Webdienst). Damit ein trainiertes Modell erneut trainiert werden kann, muss das Trainingsexperiment als Webdienst mit der Ausgabe eines trainierten Modells veröffentlicht werden. Dies ermöglicht der API, zum erneuten Trainieren auf das Modell zuzugreifen.

Das Einrichten eines erneuten Trainings für einen klassischen Webdienst umfasst die folgenden Schritte:

![Übersicht über den Prozess des erneuten Trainings][1]

Diagramm 1: Übersicht über den Prozess zum erneuten Trainieren eines klassischen Webdiensts

Das Einrichten eines erneuten Trainings für einen neuen Webdienst umfasst die folgenden Schritte:

![Übersicht über den Prozess des erneuten Trainings][7]

Diagramm 2: Übersicht über den Prozess zum erneuten Trainieren eines neuen Webdiensts

## Erstellen eines Trainingsexperiments
 
In diesem Beispiel verwenden Sie „Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset“ aus den Microsoft Azure Machine Learning-Beispielen.
	
So erstellen Sie das Experiment

1.	Melden Sie sich bei Microsoft Azure Machine Learning Studio an.
2.	Klicken Sie in der unteren rechten Ecke des Dashboards auf **Neu**.
3.	Wählen Sie in den Microsoft-Beispielen das Beispiel 5 aus.
4.	Wählen Sie zum Umbenennen des Experiments am oberen Rand des Experimentbereichs den Experimentnamen „Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset“ aus.
5.	Geben Sie „Census Model“ ein.
6.	Klicken Sie am unteren Rand des Experimentbereichs auf **Run**.
7.	Klicken Sie auf **Set Up Web Service** (Webdienst einrichten), und wählen Sie **Retraining Web Service** (Webdienst für erneutes Training) aus.

 	![Anfängliches Experiment][2]

Diagramm 2: Anfängliches Experiment

## Erstellen eines Bewertungsexperiments und Veröffentlichen des Bewertungsexperiments als Webdienst  

Erstellen Sie als Nächstes ein Vorhersageexperiment.

1.	Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service** (Webdienst einrichten), und wählen Sie **Predictive Web Service** (Vorhersagewebdienst) aus. Dadurch wird das Modell als trainiertes Modell gespeichert, und es werden Ein- und Ausgabemodule für den Webdienst hinzugefügt.
2.	Klicken Sie auf **Run** (Ausführen).
3.	Klicken Sie nach der Ausführung des Experiments auf **Deploy Web Service [Classic]** (Webdienst bereitstellen [klassisch]) oder auf **Deploy Web Service [New]** (Webdienst bereitstellen [neu]).

## Bereitstellen des Trainingsexperiments als Trainingswebdienst

Um das trainierte Modell erneut zu trainieren, müssen Sie das erstellte Trainingsexperiment als Webdienst für erneutes Training bereitstellen. Dieser Webdienst benötigt ein mit dem Modul *[Modell trainieren][train-model]* verbundenes Webdienst-Ausgabemodul, um neue trainierte Modelle erzeugen zu können.

1. Um zum Trainingsexperiment zurückzukehren, klicken Sie im linken Bereich auf das Symbol für Experimente, und klicken Sie dann auf das Experiment namens „Census Model“.
2. Geben Sie im Suchfeld „Search Experiment Items“ (Experimentelemente suchen) den Suchbegriff „Webdienst“ ein.
3. Ziehen Sie ein Webdiensteingabe-Modul in den Experimentbereich, und stellen Sie eine Verbindung zwischen der Ausgabe und dem Modul zum Bereinigen fehlender Daten her.
4. Ziehen Sie zwei Module Webdienstausgabe-Module in den Experimentbereich. Verbinden Sie die Ausgabe des Moduls *Train Model* mit einem Modul und die Ausgabe des Moduls *Evaluate Model* mit dem anderen Modul. Die Webdienstausgabe für **Modell trainieren** liefert das neue trainierte Modell. Die mit **Evaluate Model** (Modell auswerten) verknüpfte Ausgabe gibt die Ausgabe dieses Moduls zurück.
5. Klicken Sie auf **Run** (Ausführen).

Als Nächstes müssen Sie das Trainingsexperiment als Webdienst bereitstellen, der ein trainiertes Modell und Modellauswertungsergebnisse erzeugt. Ihre nächsten Schritte hängen davon ab, ob Sie einen klassischen oder einen neuen Webdienst verwenden.
  
**Klassischer Webdienst**

Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service** (Webdienst einrichten), und wählen Sie **Deploy Web Service [Classic]** (Webdienst bereitstellen [klassisch]) aus. Das **Dashboard** des Webdiensts wird mit dem API-Schlüssel und der API-Hilfeseite für die Batchausführung angezeigt. Beachten Sie, dass nur die Batchausführungsmethode zum Erstellen von trainierten Modellen verwendet werden kann.

**Neuer Webdienst**

Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service** (Webdienst einrichten), und wählen Sie **Deploy Web Service [New]** (Webdienst bereitstellen [neu]) aus. Das Azure Machine Learning-Webdienstportal wird mit der Seite für die Webdienstbereitstellung geöffnet. Geben Sie einen Namen für Ihren Webdienst ein, wählen Sie einen Zahlungsplan aus, und klicken Sie anschließend auf **Bereitstellen**. Zum Erstellen trainierter Modelle kann nur die Batchausführungsmethode verwendet werden.

Nach Abschluss der Experimentausführung sieht der resultierende Workflow wie folgt aus:

![Resultierender Workflow nach der Ausführung][4]

Abbildung 3: Resultierender Workflow nach der Ausführung

## Erneutes Trainieren des Modells mit neuen Daten mithilfe von BES

So rufen Sie die APIs zum erneuten Trainieren auf

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio (Neu > Projekt > Windows-Desktop > Konsolenanwendung).
2.	Melden Sie sich beim Machine Learning-Webdienstportal an.
3.	Klicken Sie bei Verwendung eines klassischen Webdiensts auf **Classic Web Services** (Klassische Webdienste).
	1.	Klicken Sie auf den verwendeten Webdienst.
	2.	Klicken Sie auf den Standardendpunkt.
	3.	Klicken Sie auf **Consume**.
	4.	Klicken Sie am unteren Rand der Seite **Consume** (Nutzung) im Abschnitt **Beispielcode** auf **Batch**.
	5.	Fahren Sie mit Schritt 5 dieses Verfahrens fort.
4.	Klicken Sie bei Verwendung eines neuen Webdiensts auf **Webdienste**.
	1.	Klicken Sie auf den verwendeten Webdienst.
	2.	Klicken Sie auf **Consume**.
	3.	Klicken Sie am unteren Rand der Verarbeitungsseite im Abschnitt **Beispielcode** auf **Batch**.
5.	Kopieren Sie den C#-Beispielcode für die Batchausführung, und fügen Sie ihn in die Datei „Program.cs“ ein. Achten Sie dabei darauf, dass der Namespace intakt bleibt.

Fügen Sie das NuGet-Paket „Microsoft.AspNet.WebApi.Client“ hinzu, wie in den Kommentaren angegeben. Vor dem Hinzufügen des Verweises auf „Microsoft.WindowsAzure.Storage.dll“ müssen Sie unter Umständen zuerst die Clientbibliothek für Microsoft Azure Storage Services installieren. Weitere Informationen finden Sie unter [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage).

### Aktualisieren der apiKey-Deklaration

Suchen Sie die **apiKey**-Deklaration.

	const string apiKey = "abc123"; // Replace this with the API key for the web service

Kopieren Sie auf der Seite **Consume** (Nutzung) im Abschnitt mit den grundlegenden Nutzungsinformationen den Primärschlüssel, und fügen Sie ihn in die **apiKey**-Deklaration ein.

### Aktualisieren der Azure Storage-Informationen

Der BES-Beispielcode lädt eine Datei von einem lokalen Laufwerk (z.B. „C:\\temp\\CensusIpnput.csv“) in Azure Storage hoch, verarbeitet sie und schreibt die Ergebnisse zurück in Azure Storage.

Für diese Aufgabe müssen Sie über das klassische Azure-Portal den Namen, Schlüssel und Container Ihres Speicherkontos ermitteln und anschließend die entsprechenden Werte im Code aktualisieren.

1. Melden Sie sich beim klassischen Azure-Portal an.
1. Klicken Sie im linken Navigationsbereich auf **Storage**.
1. Wählen Sie in der Speicherkontenliste ein Speicherkonto zum Speichern des neu trainierten Modells aus.
1. Klicken Sie im unteren Bereich der Seite auf **Zugriffsschlüssel verwalten**.
1. Kopieren und speichern Sie den primären Zugriffsschlüssel, und schließen Sie anschließend das Dialogfeld.
1. Klicken Sie im oberen Bereich der Seite auf **Container**.
1. Wählen Sie einen vorhandenen Container aus, oder erstellen Sie einen neuen, und speichern Sie den Namen.

Suchen Sie die Deklarationen *StorageAccountName*, *StorageAccountKey* und *StorageContainerName*, und aktualisieren Sie die Werte mit den gespeicherten Werten aus dem Azure-Portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Außerdem müssen Sie sicherstellen, dass die Eingabedatei an dem im Code angegebenen Speicherort verfügbar ist.

### Angeben des Ausgabespeicherorts

Wenn Sie den Ausgabespeicherort in der Anforderungsnutzlast angeben, muss die Erweiterung der in *RelativeLocation* angegebenen Datei als „ilearner“ angegeben werden. Siehe folgendes Beispiel.

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Die Namen Ihrer Ausgabespeicherorte können sich abhängig von der Reihenfolge, in der Sie die Ausgabemodule für den Webdienst hinzugefügt haben, von denen in dieser exemplarischen Vorgehensweise unterscheiden. Da Sie dieses Trainingsexperiment mit zwei Ausgaben einrichten, enthalten die Ergebnisse Speicherortinformationen für beide Ausgaben.

![Ausgabe des erneuten Trainings][6]

Diagramm 4: Ausgabe des erneuten Trainings

## Auswerten der Ergebnisse des erneuten Trainings
 
Wenn Sie die Anwendung ausführen, enthält die Ausgabe die URL und das erforderliche SAS-Token für den Zugriff auf die Auswertungsergebnisse.

Sie können die Leistungsergebnisse des erneut trainierten Modells anzeigen, indem Sie *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen für *output2* kombinieren (wie in der obigen Abbildung zur Ausgabe des erneuten Trainings gezeigt) und die vollständige URL in die Adressleiste des Browsers einfügen.

Überprüfen Sie die Ergebnisse, um festzustellen, ob das neu trainierte Modell gut genug ist, um das vorhandene Modell zu ersetzen.

Kopieren Sie die Werte für *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen. Diese werden für den Prozess zum erneuten Trainieren benötigt.

## Nächste Schritte

[Retrain a Classic Web service](machine-learning-retrain-a-classic-web-service.md) (Erneutes Trainieren eines klassischen Webdiensts)

[Retrain a New Web service using the Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md) (Erneutes Trainieren eines neuen Webdiensts mithilfe der Verwaltungs-Cmdlets von Machine Learning)

<!-- Retrain a New Web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

<!---HONumber=AcomDC_0928_2016-->