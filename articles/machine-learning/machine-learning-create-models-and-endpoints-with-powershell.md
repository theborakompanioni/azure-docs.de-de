<properties
pageTitle="Erstellen mehrerer Modelle mit einem Experiment | Microsoft Azure"
description="Verwenden Sie PowerShell, um mehrere Machine Learning-Modelle und Webdienst-Endpunkte mit demselben Algorithmus, aber verschiedenen Trainingsdatasets, zu erstellen."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="paulettm"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="05/12/2016"
ms.author="garye;haining"/>

# Erstellen vieler Machine Learning-Modelle und Webdienst-Endpunkte in nur einem Experiment mit PowerShell

Dies ist ein Beispiel für ein häufiges Machine Learning-Problem: Sie möchten viele Modelle erstellen, die über den gleichen Trainingsworkflow verfügen und denselben Algorithmus nutzen, als Eingabe aber unterschiedliche Trainingsdatasets verwenden. In diesem Artikel erfahren Sie, wie Sie dies in größerem Umfang in Azure Machine Learning Studio mit nur einem Experiment durchführen.

Angenommen, Sie besitzen eine Fahrradvermietung als globales Franchise-Unternehmen. Sie möchten ein Regressionsmodell erstellen, um die Mietnachfrage anhand von bisherigen Daten vorherzusagen. Sie betreiben weltweit 1.000 Mietstandorte und haben speziell für jeden Standort ein Dataset mit wichtigen Daten wie Datum, Uhrzeit, Wetter und Verkehrssituation erfasst.

Sie können das Modell einmalig trainieren, indem Sie eine zusammengefasste Version mit allen Datasets aller Standorte verwenden. Da jeder Standort aber über eine einzigartige Umgebung verfügt, besteht ein besserer Ansatz darin, das Regressionsmodell separat zu trainieren, indem Sie jeweils das Dataset für den Standort nutzen. Auf diese Weise können bei jedem trainierten Modell die unterschiedlichen Ladengrößen, das Volumen, die Geografie, Einwohnerzahl, Fahrradfreundlichkeit der Verkehrsinfrastruktur usw. berücksichtigt werden.

Dies wäre vermutlich der beste Ansatz. Sie möchten in Azure Machine Learning aber nicht 1.000 Trainingsexperimente erstellen müssen, also ein Experiment pro Standort. Dies ist nicht nur viel zu aufwändig, sondern auch eine sehr ineffiziente Vorgehensweise, da alle Experimente mit Ausnahme des Trainingsdatasets die gleichen Komponenten aufweisen würden.

Glücklicherweise können wir hierfür die [Azure Machine Learning-API für das erneute Trainieren](machine-learning-retrain-models-programmatically.md) verwenden und die Aufgabe mit der [Azure Machine Learning PowerShell](https://blogs.technet.microsoft.com/machinelearning/2016/05/04/announcing-the-powershell-module-for-azure-ml/) automatisieren.

> [AZURE.NOTE] Damit das Beispiel schneller ausgeführt wird, reduzieren wir die Standortanzahl von 1.000 auf 10. Es gelten aber dieselben Prinzipien und Verfahren wie für 1.000 Standorte. Der einzige Unterschied ist: Wenn Sie das Trainieren mit 1.000 Datasets durchführen möchten, ist es ratsam, die folgenden PowerShell-Skripts parallel auszuführen. Die Erklärung hierzu würde den Rahmen dieses Artikels sprengen, aber im Internet finden Sie Beispiele für das PowerShell-Multithreading.

## Einrichten des Trainingsexperiments

Wir verwenden ein Beispiel-[Trainingsexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1), das wir in der [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) bereits erstellt haben. Öffnen Sie dieses Experiment im [Azure Machine Learning Studio](https://studio.azureml.net)-Arbeitsbereich.

>[AZURE.NOTE] Damit Sie dieses Beispiel nachvollziehen können, empfiehlt es sich, anstelle eines kostenlosen Arbeitsbereichs einen Standardarbeitsbereich zu verwenden. Wir erstellen einen Endpunkt pro Kunde, also insgesamt zehn Endpunkte. Ein Standardarbeitsbereich ist erforderlich, da ein kostenloser Arbeitsbereich auf drei Endpunkte beschränkt ist. Falls Ihnen nur ein kostenloser Arbeitsbereich zur Verfügung steht, können Sie unten einfach die Skripts ändern, um nur drei Standorte zu verwenden.

Im Experiment wird ein **Import Data**-Modul zum Importieren des Trainingsdatasets *customer001.csv* aus einem Azure-Speicherkonto verwendet. Angenommen, wir haben Trainingsdatasets für alle Standorte der Fahrradvermietung erfasst und in demselben Blobspeicher in den Dateien *rentalloc001.csv* bis *rentalloc10.csv* gespeichert.

![image](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Beachten Sie, dass dem Modul **Train Model** das Modul **Web Service Output** hinzugefügt wurde. Wenn dieses Experiment als Webdienst bereitgestellt wird, gibt der Endpunkt, der dieser Ausgabe zugeordnet ist, das trainierte Modell als ILEARNER-Datei zurück.

Beachten Sie auch, dass wir einen Webdienstparameter für die vom **Import Data**-Modul verwendete URL eingerichtet haben. Dies ermöglicht uns die Verwendung des Parameters zum Angeben individueller Trainingsdatasets, um das Modell für jeden Standort zu trainieren. Es gibt auch andere Möglichkeiten, z.B. die Nutzung einer SQL-Abfrage mit einem Webdienstparameter zum Abrufen von Daten aus einer SQL Azure-Datenbank oder einfach die Nutzung des Moduls **Web Service Input** zum Übergeben eines Datasets an den Webdienst.

![image](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Wir führen dieses Trainingsexperiment jetzt aus, indem wir den Standardwert *rental001.csv* als Trainingsdataset verwenden. Wenn Sie die Ausgabe des Auswertungsmoduls (**Evaluate**) anzeigen, indem Sie auf die Ausgabe klicken und **Visualisieren** wählen, sehen Sie, dass eine zufriedenstellende Leistung von „*AUC* = 0.91“ angezeigt wird. Wir sind jetzt so weit, dass wir über dieses Trainingsexperiment einen Webdienst bereitstellen können.

## Bereitstellen der Webdienste für das Training und die Bewertung

Klicken Sie zum Bereitstellen des Webdiensts für das Training unter dem Experimentbereich auf die Schaltfläche **Set Up Web Service** (Webdienst einrichten), und wählen Sie die Option **Deploy Web Service** (Webdienst bereitstellen). Geben Sie dem Webdienst den Namen „Bike Rental Training“.

Als Nächstes müssen wir den Webdienst für die Bewertung bereitstellen. Hierzu können wir unter dem Experimentbereich auf **Set Up Web Service** (Webdienst einrichten) klicken und die Option **Predictive Web Service** (Vorhersagewebdienst) wählen. Ein Bewertungsexperiment wird erstellt. Wir müssen einige kleinere Anpassungen vornehmen, damit der Webdienst funktioniert. Beispielsweise müssen wir die Bezeichnungsspalte „cnt“ aus den Eingabedaten entfernen und die Ausgabe auf die Instanz-ID und den entsprechenden vorhergesagten Wert beschränken.

Der Einfachheit halber können Sie das vorbereitete [Vorhersageexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) einfach im Katalog (Gallery) öffnen.

Führen Sie zum Bereitstellen des Webdiensts das Vorhersageexperiment aus, und klicken Sie dann unter dem Experimentbereich auf die Schaltfläche **Deploy Web Service** (Webdienst bereitstellen). Geben Sie dem Webdienst für die Bewertung den Namen „Bike Rental Scoring“.

## Erstellen von zehn identischen Webdienst-Endpunkten mit PowerShell

Dieser Webdienst verfügt über einen Standardendpunkt. Der Standardendpunkt ist für uns aber nicht von so großem Interesse, da er nicht aktualisiert werden kann. Wir müssen zehn zusätzliche Endpunkte erstellen, also einen für jeden Standort. Hierfür verwenden wir PowerShell.

Zuerst richten wir unsere PowerShell-Umgebung ein:

	Import-Module .\AzureMLPS.dll
	# Assume the default configuration file exists and is properly set to point to the valid Workspace.
	$scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
	$trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'
	
Führen Sie anschließend den folgenden PowerShell-Befehl aus:

	# Create 10 endpoints on the scoring web service.
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    Write-Host ('adding endpoint ' + $endpontName + '...')
	    Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
	}

Wir haben jetzt zehn Endpunkte erstellt, die alle das gleiche trainierte Modell basierend auf *customer001.csv* enthalten. Diese können Sie im Azure-Verwaltungsportal anzeigen.

![image](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## Aktualisieren der Endpunkte zur Verwendung separater Trainingsdatasets mit PowerShell

Der nächste Schritt besteht darin, die Endpunkte mit Modellen zu aktualisieren, die basierend auf den individuellen Daten der einzelnen Kunden eindeutig trainiert wurden. Zuerst müssen wir diese Modelle aber über den Webdienst **Bike Rental Training** erzeugen. Wir kehren zum Webdienst **Bike Rental Training** zurück. Wir müssen seinen BES-Endpunkt zehnmal mit zehn verschiedenen Trainingsdatasets aufrufen, um zehn verschiedene Modelle zu erzeugen. Hierfür verwenden wir das PowerShell-Cmdlet **InovkeAmlWebServiceBESEndpoint**.

	# Invoke the retraining API 10 times
	# This is the default (and the only) endpoint on the training web service 
	$trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
	$submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
	$apiKey = $trainingSvcEp.PrimaryKey;
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
	    $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
	    Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
	    Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
	}

>[AZURE.NOTE] Der BES-Endpunkt ist der einzige unterstützte Modus für diesen Vorgang. RRS kann für die Erzeugung von trainierten Modellen nicht verwendet werden.

Oben ist Folgendes zu sehen: Anstatt zehn unterschiedliche JSON-Dateien für die BES-Auftragskonfiguration zu erstellen, erstellen wir stattdessen auf dynamische Weise die Konfigurationszeichenfolge und fügen sie in den Parameter *jobConfigString* des **InvokeAmlWebServceBESEndpoint**-Cmdlets ein. Es ist nämlich nicht erforderlich, eine Kopie auf Datenträger zu speichern.

Wenn alles gut geht, sollten nach einer Weile in Ihrem Azure-Speicherkonto zehn ILEARNER-Dateien mit der Bezeichnung *model001.ilearner* bis *model010.ilearner* angezeigt werden. Jetzt können wir unsere zehn Endpunkte des Webdiensts für die Bewertung mit diesen Modellen aktualisieren, indem wir das PowerShell-Cmdlet **Patch-AmlWebServiceEndpoint** verwenden. Bedenken Sie, dass wir nur die nicht standardmäßigen Endpunkte patchen können, die wir zuvor programmgesteuert erstellt haben.

	# Patch the 10 endpoints with respective .ilearner models
	$baseLoc = 'http://bostonmtc.blob.core.windows.net/'
	$sasToken = '<my_blob_sas_token>'
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
	    Write-Host ('Patching endpoint ' + $endpointName + '...');
	    Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
	}

Dieser Vorgang sollte relativ schnell ausgeführt werden. Nachdem die Ausführung abgeschlossen ist, verfügen wir über zehn erstellte Vorhersagewebdienst-Endpunkte. Sie enthalten jeweils ein trainiertes Modell, das mit dem Dataset eines Vermietungsstandorts eindeutig trainiert wurde. Und all dies mit nur einem Trainingsexperiment. Zur Überprüfung können Sie versuchen, diese Endpunkte mit dem **InvokeAmlWebServiceRRSEndpoint**-Cmdlet aufzurufen, indem Sie die gleichen Eingabedaten verwenden. Es ergeben sich vermutlich andere Vorhersageergebnisse, da die Modelle mit anderen Trainingsdatasets trainiert wurden.

## Vollständiges PowerShell-Skript

Hier ist der vollständige Quellcode angegeben:
	
	Import-Module .\AzureMLPS.dll
	# Assume the default configuration file exists and properly set to point to the valid workspace.
	$scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
	$trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'
	
	# Create 10 endpoints on the scoring web service
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    Write-Host ('adding endpoint ' + $endpontName + '...')
	    Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
	}
	
	# Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
	$trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
	$submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
	$apiKey = $trainingSvcEp.PrimaryKey;
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
	    $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
	    Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
	    Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
	}
	
	# Patch the 10 endpoints with respective .ilearner models
	$baseLoc = 'http://bostonmtc.blob.core.windows.net/'
	$sasToken = '?test'
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
	    Write-Host ('Patching endpoint ' + $endpointName + '...');
	    Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
	}

<!---HONumber=AcomDC_0608_2016-->