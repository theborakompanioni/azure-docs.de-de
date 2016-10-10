<properties
	pageTitle="Erneutes Trainieren eines klassischen Webdiensts | Microsoft Azure"
	description="Erfahren Sie, wie Sie ein Modell programmgesteuert erneut trainieren und den Webdienst aktualisieren, sodass er das neu trainierte Modell in Azure Machine Learning verwendet."
	services="machine-learning"
	documentationCenter=""
	authors="vDonGlover"
	manager="raymondlaghaeian"
	editor=""/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="v-donglo"/>

# Erneutes Trainieren eines klassischen Webdiensts

Der von Ihnen bereitgestellte Vorhersagewebdienst ist der Standardbewertungsendpunkt. Standardendpunkte werden mit den ursprünglichen Trainings- und Bewertungsexperimenten synchronisiert. Daher kann das trainierte Modell für den Standardendpunkt nicht ersetzt werden. Zum erneuten Trainieren des Webdiensts müssen Sie dem Webdienst einen neuen Endpunkt hinzufügen.

## Hinzufügen eines neuen Endpunkts
 
Der von Ihnen bereitgestellte Vorhersagewebdienst enthält einen standardmäßigen Bewertungsendpunkt, der mit den ursprünglichen Trainings- und Bewertungsexperimenten des trainierten Modells synchronisiert wird. Erstellen Sie zum Aktualisieren des Webdiensts mit einem neuen trainierten Modell einen neuen Bewertungsendpunkt.

Gehen Sie wie im Folgenden beschrieben vor, um einen neuen Bewertungsendpunkt für den Vorhersagewebdienst zu erstellen, der mit dem trainierten Modell aktualisiert werden kann:

>[AZURE.NOTE] Achten Sie darauf, dass Sie den Endpunkt dem Vorhersagewebdienst hinzufügen und nicht dem Trainingswebdienst. Wenn Sie sowohl einen Trainings- als auch einen Vorhersagewebdienst korrekt bereitgestellt haben, werden zwei separate Webdienste aufgeführt. Der Vorhersagewebdienst sollte mit „[predictive exp.]“ enden.

Es gibt drei Möglichkeiten zum Hinzufügen eines neuen Endpunkts zu einem Webdienst:

3. Programmgesteuert
1. Verwenden des Portals für Microsoft Azure-Webdienste
2. Verwenden des klassischen Azure-Portals

### Programmgesteuertes Hinzufügen eines Endpunkts

Sie können Bewertungsendpunkte mithilfe des Beispielcodes in diesem [GitHub-Repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) hinzufügen.

### Verwenden des Portals für Microsoft Azure-Webdienste zum Hinzufügen eines Endpunkts

1. Klicken Sie in Machine Learning Studio links auf „Webdienste“.
2. Klicken Sie am unteren Rand des Webdienst-Dashboards auf **Manage endpoints preview** (Endpunktvorschau verwalten).
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie einen Namen und eine Beschreibung für den neuen Endpunkt ein. Wählen Sie die Protokollierungsstufe aus, und legen Sie fest, ob Beispieldaten aktiviert sind. Weitere Informationen zur Protokollierung finden Sie unter [Aktivieren der Protokollierung für Machine Learning-Webdienste](machine-learning-web-services-logging.md).

### Verwenden des klassischen Azure-Portals zum Hinzufügen eines Endpunkts

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.
2. Klicken Sie im linken Menü auf **Machine Learning**.
3. Klicken Sie unter „Name“ auf Ihren Arbeitsbereich und dann auf **Web Services**.
4. Klicken Sie unter „Name“ auf **Census Model [predictive exp.]**.
5. Klicken Sie unten auf der Seite auf **Endpunkt hinzufügen**. Weitere Informationen zum Hinzufügen von Endpunkten finden Sie unter [Erstellen von Endpunkten](machine-learning-create-endpoint.md).

## Aktualisieren des trainierten Modells des hinzugefügten Endpunkts

Um das erneute Training abzuschließen, müssen Sie das trainierte Modell des von Ihnen hinzugefügten neuen Endpunkts aktualisieren.

* Wenn Sie den neuen Endpunkt im Azure-Portal hinzugefügt haben, können Sie im Azure-Portal auf seinen Namen klicken. Klicken Sie anschließend auf den **UpdateResource**-Link, um die URL abzurufen, die Sie benötigen, um das Modell des Endpunkts zu aktualisieren.
* Wenn Sie den Endpunkt mithilfe des Beispielcodes hinzugefügt haben, ist der Speicherort der Hilfe-URL im Wert *HelpLocationURL* in der Ausgabe angegeben.
 
So rufen Sie die Pfad-URL ab

1.	Kopieren Sie die URL, und fügen Sie sie in Ihren Browser ein.
2.	Klicken Sie auf den Link „Ressource aktualisieren“.
3.	Kopieren Sie die POST-URL der PATCH-Anforderung. Beispiel:

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

1.	Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.
2.	Klicken Sie im linken Menü auf **Machine Learning**.
3.	Klicken Sie unter „Name“ auf Ihren Arbeitsbereich und dann auf **Web Services**.
4.	Klicken Sie unter „Name“ auf „Census Model [predictive exp.]“.
5.	Klicken Sie auf den neuen Endpunkt, den Sie hinzugefügt haben.
6.	Klicken Sie im Dashboard des Endpunkts auf **Ressource aktualisieren**.
7.	Auf der Dokumentationsseite der API zur Ressourcenaktualisierung für den Webdienst wird der **Ressourcenname** unter **Updatable Resources** (Aktualisierbare Ressourcen) angezeigt.

Wenn Ihr SAS-Token abläuft, bevor Sie die Aktualisierung des Endpunkts abgeschlossen haben, müssen Sie einen GET-Befehl mit der Einzelvorgangs-ID ausführen, um ein neues Token abzurufen.

Nach der erfolgreichen Ausführung des Codes sollte der neue Endpunkt nach ungefähr 30 Sekunden mit der Verwendung des neuen trainierten Modells beginnen.

##Zusammenfassung

Mithilfe der APIs zum erneuten Trainieren können Sie das trainierte Modell eines Vorhersagewebdiensts aktualisieren und so z.B. folgende Szenarien ermöglichen:

* Regelmäßiges erneutes Trainieren des Modells mit neuen Daten
* Verteilung eines Modells an Kunden, sodass sie das Modell mit ihren eigenen Daten erneut trainieren können

Nächste Schritte

[Problembehandlung für das erneute Trainieren eines klassischen Azure Machine Learning-Webdiensts](machine-learning-troubleshooting-retraining-models.md)

<!---HONumber=AcomDC_0928_2016-->