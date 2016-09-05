<properties
	pageTitle="Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen | Microsoft Azure"
	description="Erfahren Sie, wie Sie ein Modell programmgesteuert erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Azure Machine Learning verwendet."
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
	ms.date="08/23/2016"
	ms.author="raymondl;garye;v-donglo"/>


#Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen  

Im Rahmen der Operationalisierung von Machine Learning-Modellen in Azure Machine Learning wird Ihr Modell trainiert und gespeichert. Anschließend verwenden Sie es zum Erstellen eines Vorhersagewebdiensts. Der Webdienst kann dann in Websites, Dashboards und mobilen Apps genutzt werden.

Mithilfe von Machine Learning erstellte Modelle sind in der Regel nicht statisch. Wenn neue Daten verfügbar sind oder der Endkunde der API über eigene Daten verfügt, muss das Modell erneut trainiert werden. Es kann auch vorkommen, dass Sie zum Abrufen einer Teilmenge der Daten Filter anwenden und das Modell erneut trainieren müssen.

Das erneute Training kann häufig durchgeführt werden. Mit der API-Funktion für programmgesteuertes erneutes Trainieren können Sie das Modell mithilfe der APIs zum erneuten Trainieren programmgesteuert erneut trainieren und den Webdienst mit dem neu trainierten Modell aktualisieren.

In diesem Dokument wird das erneute Trainieren beschrieben und gezeigt, wie Sie die APIs zum erneuten Trainieren verwenden.

## Warum erneut trainieren: Problemdefinition  

Im Rahmen des ML-Trainingsprozesses wird ein Modell mit einem Satz von Daten trainiert. Mithilfe von Machine Learning erstellte Modelle sind in der Regel nicht statisch. Wenn neue Daten verfügbar sind oder der Nutzer der API über eigene Daten verfügt, muss das Modell erneut trainiert werden. In anderen Szenarien kann es erforderlich sein, zum Abrufen einer Teilmenge der Daten Filter anzuwenden und das Modell erneut zu trainieren.

In diesen Szenarien bietet eine programmgesteuerte API eine komfortable Möglichkeit, mit der Sie oder die Nutzer Ihrer APIs einen Client erstellen können, der das Modell einmalig oder regelmäßig unter Verwendung eigener Daten trainieren kann. Sie können die Ergebnisse des erneuten Trainings dann auswerten und die Webdienst-API aktualisieren, sodass sie das trainierte Modell verwendet.

##Erneutes Trainieren: Prozess  

Der Prozess beinhaltet die folgenden Komponenten: ein Trainingsexperiment und ein Vorhersageexperiment, die als Webdienst veröffentlicht werden. Damit ein trainiertes Modell erneut trainiert werden kann, muss das Trainingsexperiment als Webdienst mit der Ausgabe eines trainierten Modells veröffentlicht werden. Dies ermöglicht der API, zum erneuten Trainieren auf das Modell zuzugreifen.

Das Einrichten eines erneuten Trainings umfasst die folgenden Schritte:

![Übersicht über den Prozess des erneuten Trainings][1]

Diagramm 1: Übersicht über den Prozess des erneuten Trainings

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
3.	Klicken Sie nach der Ausführung des Experiments auf **Deploy Web Service [Classic]** (Webdienst bereitstellen [klassisch]). Dadurch wird das Vorhersageexperiment als klassischer Webdienst bereitgestellt.

## Bereitstellen des Trainingsexperiments als Trainingswebdienst

Um das trainierte Modell erneut zu trainieren, müssen Sie das als „Retraining Web Service“ (Webdienst für erneutes Training) erstellte Trainingsexperiment bereitstellen. Dieser Webdienst benötigt ein Webdienst-Ausgabemodul, das mit dem Modul [Train Model][train-model] verbunden ist, um neue trainierte Modelle erzeugen zu können.

1. Um zum Trainingsexperiment zurückzukehren, klicken Sie im linken Bereich auf das Symbol für Experimente, und klicken Sie dann auf das Experiment namens „Census Model“.
2. Geben Sie „Web Service“ in das Suchfeld „Search Experiment Items“ (Experimentelemente suchen) ein.
3. Ziehen Sie ein Modul „Web Service Input“ in den Experimentbereich, und verbinden Sie seine Ausgabe mit dem Modul „Clean Missing Data“ .
4. Ziehen Sie zwei Module *Web Service Output* in den Experimentbereich. Verbinden Sie die Ausgabe des Moduls *Train Model* mit einem Modul und die Ausgabe des Moduls *Evaluate Model* mit dem anderen Modul. Die Webdienstausgabe für „Train Model“ liefert uns das neue trainierte Modell. Die mit „Evaluate Model“ verknüpfte Ausgabe gibt die Ausgabe dieses Moduls für das Auswertungsmodul zurück.
5. Klicken Sie auf **Run** (Ausführen).
6. Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service** (Webdienst einrichten), und wählen Sie **Retraining Web Service** (Webdienst für erneutes Training) aus. Dadurch wird das Trainingsexperiment als ein Webdienst bereitgestellt, der ein trainiertes Modell und Modellauswertungsergebnisse erzeugt. Das **Dashboard** des Webdiensts wird mit dem API-Schlüssel und der API-Hilfeseite für die Batchausführung angezeigt. Beachten Sie, dass nur die Batchausführungsmethode zum Erstellen von trainierten Modellen verwendet werden kann.
  
Nachdem die Ausführung des Experiments abgeschlossen wurde, sollte der resultierende Workflow wie folgt aussehen:

![Resultierender Workflow nach der Ausführung][4]

Abbildung 3: Resultierender Workflow nach der Ausführung

## Hinzufügen eines neuen Endpunkts
 
Der von Ihnen bereitgestellte Vorhersagewebdienst ist der Standardbewertungsendpunkt. Standardendpunkte werden mit den ursprünglichen Trainings- und Bewertungsexperimenten synchronisiert. Daher kann das trainierte Modell für den Standardendpunkt nicht ersetzt werden.

Gehen Sie wie im Folgenden beschrieben vor, um einen neuen Bewertungsendpunkt für den Vorhersagewebdienst zu erstellen, der mit dem trainierten Modell aktualisiert werden kann:

>[AZURE.NOTE] Achten Sie darauf, dass Sie den Endpunkt dem Vorhersagewebdienst hinzufügen und nicht dem Trainingswebdienst. Wenn Sie sowohl einen Trainings- als auch einen Vorhersagewebdienst korrekt bereitgestellt haben, werden zwei separate Webdienste aufgeführt. Der Vorhersagewebdienst sollte mit „[predictive exp.]“ enden.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.
2. Klicken Sie im linken Menü auf **Machine Learning**.
3. Klicken Sie unter „Name“ auf Ihren Arbeitsbereich und dann auf **Web Services**.
4. Klicken Sie unter „Name“ auf **Census Model [predictive exp.]**.
5. Klicken Sie unten auf der Seite auf **Endpunkt hinzufügen**. Weitere Informationen zum Hinzufügen von Endpunkten finden Sie unter [Erstellen von Endpunkten](machine-learning-create-endpoint.md).

Sie können Bewertungsendpunkte auch mithilfe des Beispielcodes in diesem [GitHub-Repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) hinzufügen.

## Erneutes Trainieren des Modells mit neuen Daten mithilfe von BES

So rufen Sie die APIs zum erneuten Trainieren auf

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio (Neu > Projekt > Windows-Desktop > Konsolenanwendung).
2. Kopieren Sie den C#-Beispielcode von der Hilfeseite der Trainingswebdienst-API für die Batchausführung, und fügen Sie ihn in die Datei „Program.cs“ ein. Achten Sie dabei darauf, dass der Namespace intakt bleibt.
3. Der Beispielcode enthält Kommentare, die auf die Teile des Codes hinweisen, die Sie aktualisieren müssen.
4. Wenn Sie den Ausgabespeicherort in der Anforderungsnutzlast angeben, muss die Erweiterung der in *RelativeLocation* angegebenen Datei von „csv“ in „ilearner“ geändert werden. Siehe folgendes Beispiel.

		Outputs = new Dictionary<string, AzureBlobDataReference>()
		{
			{
				"output1",
				new AzureBlobDataReference()
				{
					ConnectionString = "DefaultEndpointsProtocol=https;AccountName=mystorageacct;AccountKey=Dx9WbMIThAvXRQWap/aLnxT9LV5txxw==",
					RelativeLocation = "mycontainer/output1results.ilearner"
				}
			},
		},

>[AZURE.NOTE] Die Namen Ihrer Ausgabespeicherorte können sich abhängig von der Reihenfolge, in der Sie die Ausgabemodule für den Webdienst hinzugefügt haben, von denen in dieser exemplarischen Vorgehensweise unterscheiden. Da Sie dieses Trainingsexperiment mit zwei Ausgaben einrichten, enthalten die Ergebnisse Speicherortinformationen für beide Ausgaben.

### Aktualisieren der Azure Storage-Informationen

Der BES-Beispielcode lädt eine Datei von einem lokalen Laufwerk (z.B. „C:\\temp\\CensusIpnput.csv“) in Azure Storage hoch, verarbeitet sie und schreibt die Ergebnisse zurück in Azure Storage.

Zum Durchführen dieser Aufgabe müssen Sie Informationen zum Namen, Schlüssel und Container des Speicherkontos aus dem klassischen Azure-Portal für Ihr Speicherkonto abrufen und die entsprechenden Werte im Code aktualisieren.

Außerdem müssen Sie sicherstellen, dass die Eingabedatei an dem im Code angegebenen Speicherort verfügbar ist.

![Ausgabe des erneuten Trainings][6]

Diagramm 4: Ausgabe des erneuten Trainings

## Auswerten der Ergebnisse des erneuten Trainings
 
Wenn Sie die Anwendung ausführen, enthält die Ausgabe die URL und das erforderliche SAS-Token für den Zugriff auf die Auswertungsergebnisse.

Sie können die Leistungsergebnisse des erneut trainierten Modells anzeigen, indem Sie *BaseLocation*, *RelativeLocation* und *SasBlobToken* aus den Ausgabeergebnissen für *output2* kombinieren (wie in der obigen Abbildung zur Ausgabe des erneuten Trainings gezeigt) und die vollständige URL in die Adressleiste des Browsers einfügen.

Überprüfen Sie die Ergebnisse, um festzustellen, ob das neu trainierte Modell gut genug ist, um das vorhandene Modell zu ersetzen.

## Aktualisieren des trainierten Modells des hinzugefügten Endpunkts

Um das erneute Training abzuschließen, müssen Sie das trainierte Modell des von Ihnen hinzugefügten neuen Endpunkts aktualisieren.

* Wenn Sie den neuen Endpunkt im Azure-Portal hinzugefügt haben, können Sie im Azure-Portal auf seinen Namen klicken. Klicken Sie anschließend auf den **UpdateResource**-Link, um die URL abzurufen, die Sie benötigen, um das Modell des Endpunkts zu aktualisieren.
* Wenn Sie den Endpunkt mithilfe des Beispielcodes hinzugefügt haben, ist der Speicherort der Hilfe-URL im Wert *HelpLocationURL* in der Ausgabe angegeben.

So rufen Sie die Pfad-URL ab

1. Kopieren Sie die URL, und fügen Sie sie in Ihren Browser ein.
2. Klicken Sie auf den Link „Ressource aktualisieren“.
3. Kopieren Sie die POST-URL der PATCH-Anforderung. Beispiel:

		PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2

Sie können das trainierte Modell jetzt verwenden, um den zuvor erstellten Bewertungsendpunkt zu aktualisieren.

Der folgende Beispielcode zeigt, wie Sie *BaseLocation*, *RelativeLocation*, *SasBlobToken* und die PATCH-URL zum Aktualisieren des Endpunkts verwenden.

	private async Task OverwriteModel()
	{
		var resourceLocations = new
		{
			Resources = new[]
			{
				new
				{
					Name = "Census Model [trained model]",
					Location = new AzureBlobDataReference()
					{
						BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
						RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
						SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
					}
				}
			}
		};

		using (var client = new HttpClient())
		{
			client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

			using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
			{
				request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
				HttpResponseMessage response = await client.SendAsync(request);

				if (!response.IsSuccessStatusCode)
				{
					await WriteFailedResponse(response);
				}

				// Do what you want with a successful response here.
			}
		}
	}

Der *apiKey* und die *endpointUrl* für den Aufruf werden im Dashboard des Endpunkts angezeigt.

Der Wert des Parameters *Name* in *Resources* sollte mit dem Namen des gespeicherten trainierten Modells im Vorhersageexperiment übereinstimmen. Gehen Sie wie folgt vor, um den Ressourcennamen abzurufen:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.
2. Klicken Sie im linken Menü auf **Machine Learning**.
3. Klicken Sie unter „Name“ auf Ihren Arbeitsbereich und dann auf **Web Services**.
4. Klicken Sie unter „Name“ auf **Census Model [predictive exp.]**.
5. Klicken Sie auf den neuen Endpunkt, den Sie hinzugefügt haben.
6. Klicken Sie im Dashboard des Endpunkts auf *Ressource aktualisieren*.
7. Auf der Dokumentationsseite der API zur Ressourcenaktualisierung für den Webdienst wird der **Ressourcenname** unter **Updatable Resources** (Aktualisierbare Ressourcen) angezeigt.

Wenn Ihr SAS-Token abläuft, bevor Sie die Aktualisierung des Endpunkts abgeschlossen haben, müssen Sie einen GET-Befehl mit der Einzelvorgangs-ID ausführen, um ein neues Token abzurufen.

Nach der erfolgreichen Ausführung des Codes sollte der neue Endpunkt nach ungefähr 30 Sekunden mit der Verwendung des neuen trainierten Modells beginnen.

##Zusammenfassung  
Mithilfe der APIs zum erneuten Trainieren können Sie das trainierte Modell eines Vorhersagewebdiensts aktualisieren und so z.B. folgende Szenarien ermöglichen:

* Regelmäßiges erneutes Trainieren des Modells mit neuen Daten
* Verteilung eines Modells an Kunden, sodass sie das Modell mit ihren eigenen Daten erneut trainieren können

## Nächste Schritte
[Problembehandlung für das erneute Trainieren eines klassischen Azure Machine Learning-Webdiensts](machine-learning-troubleshooting-retraining-models.md)

[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

<!---HONumber=AcomDC_0824_2016-->