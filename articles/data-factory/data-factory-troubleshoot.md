<properties 
	pageTitle="Problembehandlung bei Azure Data Factory" 
	description="Erfahren Sie, wie Sie Probleme mithilfe von Azure Data Factory beheben." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/18/2016" 
	ms.author="spelluru"/>

# Problembehandlung bei Data Factory
Dieser Artikel enthält Tipps zur Behandlung von Problemen bei der Verwendung von Azure Data Factory. Dies umfasst nicht alle möglichen Probleme bei Verwendung des Diensts, aber es werden einige Probleme und die allgemeine Problembehandlung behandelt.

## Tipps zur Problembehandlung

### Fehler: Das Abonnement ist nicht für die Verwendung des Namespace „Microsoft.DataFactory“ registriert.
Wenn Sie diesen Fehler erhalten, wurde der Azure Data Factory-Ressourcenanbieter nicht auf Ihrem Computer registriert. Gehen Sie wie folgt vor:

1. Starten Sie Azure PowerShell. 
2. Melden Sie sich mithilfe des folgenden Befehls beim Azure-Konto an. Login-AzureRmAccount 
3. Führen Sie den folgenden Befehl aus, um den Azure Data Factory-Anbieter zu registrieren. Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory

### Problem: Autorisierungsfehler beim Ausführen eines Data Factory-Cmdlets
Sie verwenden wahrscheinlich nicht das richtige Azure-Konto oder -Abonnement für Azure-PowerShell. Wählen Sie mithilfe der folgenden Cmdlets das richtige Azure-Konto und -Abonnement für die Verwendung mit Azure-PowerShell.

1. Login-AzureRmAccount: Verwenden Sie die richtige Benutzer-ID und das richtige Kennwort.
2. Get-AzureRmSubscription: Zeigen Sie alle Abonnements für das Konto an. 
3. Select-AzureRmSubscription <subscription name>: Wählen Sie das richtige Abonnement aus. Verwenden Sie dasselbe Abonnement wie zum Erstellen einer Data Factory im Azure-Portal.

### Problem: Das Express-Setup für das Datenverwaltungsgateway kann über das Azure-Portal nicht gestartet werden.
Für das Express-Setup des Datenverwaltungsgateways ist Internet Explorer oder ein mit Microsoft ClickOnce kompatibler Webbrowser erforderlich. Wenn das Express-Setup nicht gestartet wird, führen Sie einen der folgenden Schritte aus:

- Hierfür ist Internet Explorer oder ein mit Microsoft ClickOnce kompatibler Webbrowser erforderlich.

	Navigieren Sie bei Verwendung von Chrome zum [Chrome Web Store](https://chrome.google.com/webstore/), führen Sie eine Suche mit dem Begriff „ClickOnce“ durch, wählen Sie eine der ClickOnce-Erweiterungen aus, und installieren Sie sie.
	
	Gehen Sie bei Firefox genauso vor (Installation des Add-Ins). Klicken Sie auf der Symbolleiste auf die Schaltfläche „Menü öffnen“ (drei waagerechte Striche oben rechts), klicken Sie auf „Add-Ons“, suchen Sie nach dem Stichwort „ClickOnce“, wählen Sie eine der ClickOnce-Erweiterungen aus, und installieren Sie sie.

- Verwenden Sie den Link **Manuelles Setup**, der auf dem gleichen Blatt im Portal angezeigt wird, um die Installationsdatei herunterzuladen, und führen Sie sie manuell aus. Wenn die Installation abgeschlossen ist, wird das Dialogfeld für die Datenverwaltungsgateway-Konfiguration angezeigt. Kopieren Sie den **Schlüssel** vom Portalbildschirm, und verwenden Sie ihn im Konfigurations-Manager, um das Gateway manuell für den Dienst zu registrieren.

### Problem: Fehler beim Herstellen einer Verbindung mit der lokalen SQL Server-Datenbank 
Starten Sie den **Datenverwaltungsgateway-Konfigurations-Manager** auf dem Gatewaycomputer, und verwenden Sie die Registerkarte **Problembehandlung**, um die Verbindung mit SQL Server über den Gatewaycomputer zu testen. Details finden Sie unter [Problembehandlung bei Gateways](data-factory-move-data-between-onprem-and-cloud.md#gateway-troubleshooting).
 

### Problem: Eingabeslices haben dauerhaft den Status „Waiting“

Slices können den Status **Waiting** aus unterschiedlichen Gründen aufweisen. Zu den häufigsten Gründen zählt, dass die Eigenschaft **external** nicht auf **true** festgelegt ist. Ein Dataset, das außerhalb des Gültigkeitsbereichs von Azure Data Factory erstellt wird, sollte mit der Eigenschaft **external** gekennzeichnet sein. Dies bedeutet, dass es sich um externe Daten handelt, die nicht von Pipelines innerhalb der Data Factory unterstützt werden. Die Datenslices werden als **Ready** gekennzeichnet, sobald die Daten im entsprechenden Speicher verfügbar sind.

Das folgende Beispiel zeigt die Verwendung der Eigenschaft **external**. Sie können optional **externalData*** angeben, wenn Sie "external" auf "true" festlegen.

Weitere Informationen zu dieser Eigenschaft finden Sie im Artikel [Datasets](data-factory-create-datasets.md).
	
	{
	  "name": "CustomerTable",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "MyLinkedService",
	    "typeProperties": {
	      "folderPath": "MyContainer/MySubFolder/",
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "rowDelimiter": ";"
	      }
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      }
	    }
	  }
	}

Um den Fehler zu beheben, fügen Sie die Eigenschaft **external** und den optionalen Abschnitt **externalData** der JSON-Definition für die Eingabetabelle hinzu und erstellen die Tabelle erneut.

### Problem: Fehler beim Hybridkopiervorgang
Die Schritte zum Behandeln von Problemen beim Kopieren in und aus lokalen Datenspeichern mit dem Datenverwaltungsgateway finden Sie unter [Problembehandlung bei Gateways](data-factory-move-data-between-onprem-and-cloud.md#gateway-troubleshooting).

### Problem: Fehler bei der bedarfsgesteuerten HDInsight-Bereitstellung
Wenn Sie einen verknüpften Dienst vom Typ „HDInsightOnDemand“ verwenden, müssen Sie einen „linkedServiceName“ angeben, der auf einen Azure-Blobspeicher verweist. Der Data Factory-Dienst verwendet diesen Speicher, um Protokolle und unterstützende Dateien für Ihren bedarfsgesteuerten HDInsight-Cluster zu speichern. Manchmal schlägt die Bereitstellung eines bedarfsgesteuerten HDInsight-Clusters mit dem folgenden Fehler fehl:

		Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Dieser Fehler gibt normalerweise an, dass der Speicherort des Speicherkontos, das in „linkedServiceName“ angegeben ist, nicht dem Speicherort im Rechenzentrum entspricht, in dem die HDInsight-Bereitstellung erfolgt. Beispiel: Wenn der Speicherort von Azure Data Factory "USA (Westen)" ist und die bedarfsgesteuerte HDInsight-Bereitstellung in "USA (Westen)" erfolgt, aber der Speicherort des Azure-Blobspeicherkontos auf "USA (Osten)" festgelegt ist, schlägt die bedarfsgesteuerte Bereitstellung fehl.

Darüber hinaus können in der weiteren JSON-Eigenschaft "additionalLinkedServiceNames" zusätzliche Speicherkonten in bedarfsgesteuertem HDInsight angegeben werden. Diese zusätzlichen verknüpften Speicherkonten müssen sich am gleichen Speicherort wie der HDInsight-Cluster befinden. Andernfalls tritt derselbe Fehler auf.

### Problem: Fehler bei benutzerdefinierter .NET-Aktivität
Die ausführlichen Schritte finden Sie unter [Debuggen einer Pipeline mit benutzerdefinierten Aktivitäten](data-factory-use-custom-activities.md#debug-the-pipeline).

## Verwenden des Azure-Portals zur Problembehandlung 

### Verwenden von Portalblättern
Die Schritte finden Sie unter [Überwachen der Pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline).

### Verwenden der App „Überwachung und Verwaltung“
Details finden Sie unter [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der neuen App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md).

## Verwenden von Azure PowerShell zur Problembehandlung

### Verwenden von Azure PowerShell zur Behandlung eines Fehlers  
Details finden Sie unter [Überwachen von Data Factory-Pipelines mithilfe von Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline).


[adfgetstarted]: data-factory-get-started.md
[adf-tutorial]: data-factory-tutorial.md
[use-custom-activities]: data-factory-use-custom-activities.md
[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 

<!---HONumber=AcomDC_0420_2016-->