<properties
	pageTitle="Erstellen der ersten Data Factory (Azure-Portal) | Microsoft Azure"
	description="In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mit dem Data Factory-Editor im Azure-Portal."
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
	ms.topic="hero-article" 
	ms.date="09/14/2016"
	ms.author="spelluru"/>

# Tutorial: Erstellen der ersten Azure Data Factory mit dem Azure-Portal
> [AZURE.SELECTOR]
- [Übersicht und Voraussetzungen](data-factory-build-your-first-pipeline.md)
- [Azure-Portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel erfahren Sie, wie Sie mithilfe des [Azure-Portals](https://portal.azure.com/) Ihre erste Azure Data Factory erstellen.

## Voraussetzungen		
1. Lesen Sie sich den Artikel mit der [Übersicht über das Tutorial](data-factory-build-your-first-pipeline.md) durch, und führen Sie die erforderlichen Schritte aus, damit die **Voraussetzungen** erfüllt sind.
2. Dieser Artikel bietet keine grundlegende Übersicht über den Azure Data Factory-Dienst. Sie sollten den Artikel [Einführung in Azure Data Factory](data-factory-introduction.md) lesen, um eine ausführliche Übersicht über den Dienst zu erhalten.

## Erstellen einer Data Factory
Eine Data Factory kann eine oder mehrere Aktivitäten aufweisen. Eine Pipeline kann eine oder mehrere Aktivitäten aufweisen. Beispielsweise eine Kopieraktivität zum Kopieren von Daten aus einer Quelle in einen Zieldatenspeicher und eine HDInsight-Hive-Aktivität zum Ausführen eines Hive-Skripts zum Transformieren von Eingabedaten in Produktausgabedaten. In diesem Schritt erstellen wir zunächst die Data Factory.

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.	Klicken Sie im Menü auf der linken Seite auf **NEU**, und klicken Sie auf **Data + Analytics** und dann auf **Data Factory**.
		
	![Blatt "Erstellen"](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.	Geben Sie auf dem Blatt **Neue Data Factory** unter „Name“ die Zeichenfolge **GetStartedDF** ein.

	![Blatt "Neue Data Factory"](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

	> [AZURE.IMPORTANT] 
	Der Name der Azure Data Factory muss **global eindeutig** sein. Gehen Sie wie folgt vor, wenn Sie die Fehlermeldung **Die Data Factory mit dem Namen „GetStartedDF“ ist nicht verfügbar** erhalten: Ändern Sie den Namen der Data Factory (z.B. in „ihrnameGetStartedDF“), und versuchen Sie, die Erstellung erneut durchzuführen. Im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md) finden Sie Benennungsregeln für Data Factory-Artefakte.
	> 
	> Der Name der Data Factory kann in Zukunft als **DNS**-Name registriert und so öffentlich sichtbar werden.

3.	Wählen Sie das **Azure-Abonnement**, in dem die Data Factory erstellt werden soll.
4.	Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine Ressourcengruppe. Erstellen Sie für das Tutorial eine Ressourcengruppe mit dem Namen **ADFGetStartedRG**.
5.	Klicken Sie auf dem Blatt **Neue Data Factory** auf **Erstellen**.

	> [AZURE.IMPORTANT] Zum Erstellen von Data Factory-Instanzen müssen Sie Mitglied der Rolle [Data Factory-Mitwirkender](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) auf Abonnement- bzw. Ressourcengruppenebene sein.
6.	Sie sehen, dass die Data Factory im **Startmenü** des Azure-Portals wie folgt erstellt wird:

	![Erstellen des Data Factory-Status](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Glückwunsch! Sie haben erfolgreich Ihre erste Data Factory erstellt. Nachdem die Data Factory erfolgreich erstellt wurde, sehen Sie die Data Factory-Seite mit dem Inhalt der Data Factory.

	![Blatt "Data Factory"](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Vor dem Erstellen einer Pipeline in der Data Factory müssen Sie zunächst einige Data Factory-Entitäten erstellen. Zuerst erstellen Sie verknüpfte Dienste zum Verknüpfen von Datenspeichern/Berechnungen mit Ihrem Datenspeicher, definieren die Datasets für Ein- und Ausgabe, um Eingabe-/Ausgabedaten in verknüpften Datenspeichern darzustellen, und erstellen dann die Pipeline mit einer Aktivität, die diese Datasets verwendet.

## Erstellen von verknüpften Diensten
In diesem Schritt verknüpfen Sie Ihr Azure Storage-Konto und einen bedarfsgesteuerten Azure HDInsight-Cluster mit Ihrer Data Factory. Das Azure Storage-Konto enthält in diesem Beispiel die Ein- und Ausgabedaten für die Pipeline. Der verknüpfte HDInsight-Dienst wird verwendet, um das in der Aktivität der Pipeline in diesem Beispiel angegebene Hive-Skript auszuführen. Identifizieren Sie, welche [Datenspeicher-](data-factory-data-movement-activities.md)/[Computedienste](data-factory-compute-linked-services.md) in Ihrem Szenario verwendet werden, und verknüpfen Sie diese Dienste mit der Data Factory, indem Sie verknüpfte Dienste erstellen.

### Erstellen des mit Azure Storage verknüpften Diensts
In diesem Schritt verknüpfen Sie Ihr Azure Storage-Konto mit Ihrer Data Factory. In diesem Tutorial verwenden Sie das gleiche Azure Storage-Konto, um Ein-/Ausgabedaten und die HQL-Skriptdatei zu speichern.

1.	Klicken Sie auf dem Blatt **DATA FACTORY** für **GetStartedDF** auf **Erstellen und bereitstellen**. Der Data Factory-Editor sollte angezeigt werden.
	 
	![Kachel "Erstellen und bereitstellen"](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.	Klicken Sie auf **Neuer Datenspeicher**, und wählen Sie **Azure Storage**.

	![Neuer Datenspeicher – Azure Storage – Menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.	Das JSON-Skript zum Erstellen eines mit einem Azure-Speicher verknüpften Diensts sollte im Editor angezeigt werden.
	
	![Mit Azure Storage verknüpfter Dienst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
	 
4. Ersetzen Sie **Kontoname** durch den Namen Ihres Azure-Speicherkontos und **Kontoschlüssel** durch den Zugriffsschlüssel des Azure-Speicherkontos. Informationen zum Abrufen Ihres Speicherzugriffsschlüssels finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
5. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um den verknüpften Dienst bereitzustellen.

	![Schaltfläche "Bereitstellen"](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Nach erfolgreicher Bereitstellung des verknüpften Diensts sollte das Fenster **Draft-1** nicht mehr angezeigt werden, und Sie sehen **StorageLinkedService** in der Strukturansicht links.
        ![Mit Storage verknüpfter Dienst im Menü](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)

 
### Erstellen des mit Azure-HDInsight verknüpften Diensts
In diesem Schritt verknüpfen Sie einen bedarfsgesteuerten HDInsight-Cluster mit Ihrer Data Factory. Der HDInsight-Cluster wird automatisch zur Laufzeit erstellt und gelöscht, nachdem die Verarbeitung abgeschlossen und die angegebene Leerlaufzeit verstrichen ist.

1. Klicken Sie im **Data Factory-Editor** auf **... Weitere** und dann auf **Neu berechnen**, und wählen Sie **On-demand HDInsight cluster** (Bedarfsgesteuerter HDInsight-Cluster).

	![Neu berechnen](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in das Fenster **Draft-1** ein. Der JSON-Codeausschnitt beschreibt die Eigenschaften, die zum Erstellen des bedarfsgesteuerten HDInsight-Clusters verwendet werden.

		{
		  "name": "HDInsightOnDemandLinkedService",
		  "properties": {
		    "type": "HDInsightOnDemand",
		    "typeProperties": {
		      "version": "3.2",
		      "clusterSize": 1,
		      "timeToLive": "00:30:00",
		      "linkedServiceName": "AzureStorageLinkedService"
		    }
		  }
		}
	
	Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften, die im Codeausschnitt verwendet werden:
	
	| Eigenschaft | Beschreibung |
	| :------- | :---------- |
	| Version | Gibt die Version des zu erstellenden HDInsight-Clusters als 3.2 an. | 
	| ClusterSize | Gibt die Größe des HDInsight-Clusters an. | 
	| TimeToLive | Gibt die Leerlaufzeit des HDInsight-Clusters an, bevor er gelöscht wird. |
	| linkedServiceName | Gibt das Speicherkonto an, das verwendet wird, um die von HDInsight generierten Protokolle zu speichern. |

	Beachten Sie folgende Punkte:
	
	- Die Data Factory erstellt mit dem JSON-Code einen **Windows-basierten** HDInsight-Cluster für Sie. Sie können sich auch für die Erstellung eines **Linux-basierten** HDInsight-Clusters entscheiden. Ausführliche Informationen finden Sie unter [Bedarfsgesteuerter verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
	- Anstelle eines bedarfsgesteuerten HDInsight-Clusters könnten Sie **Ihren eigenen HDInsight-Cluster** verwenden. Ausführliche Informationen finden Sie unter [Verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
	- Der HDInsight-Cluster erstellt einen **Standardcontainer** im Blobspeicher, den Sie im JSON-Code angegeben haben (**linkedServiceName**). HDInsight löscht diesen Container nicht, wenn der Cluster gelöscht wird. Dieses Verhalten ist beabsichtigt. Beim bedarfsgesteuerten verknüpften HDInsight-Dienst wird jedes Mal ein HDInsight-Cluster erstellt, wenn ein Slice verarbeitet wird. Dies gilt nur dann nicht, wenn ein aktiver Cluster (**timeToLive**) vorhanden ist. Der Cluster wird automatisch gelöscht, nachdem die Verarbeitung abgeschlossen ist.
	
		Wenn mehr Segmente verarbeitet werden, werden in Ihrem Azure-Blobspeicher viele Container angezeigt. Falls Sie diese für die Problembehandlung der Aufträge nicht benötigen, sollten Sie sie ggf. löschen, um die Speicherkosten zu verringern. Die Namen dieser Container basieren auf dem folgenden Muster: „adf**IhrDataFactoryName**-**NameVerknüpfterDienst**-Datums-/Uhrzeitstempel“. Verwenden Sie Tools wie [Microsoft Storage-Explorer](http://storageexplorer.com/), um Container in Ihrem Azure-Blobspeicher zu löschen.

	Ausführliche Informationen finden Sie unter [Bedarfsgesteuerter verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
3. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um den verknüpften Dienst bereitzustellen.

	![Bedarfsgesteuerten verknüpften HDInsight-Dienst bereitstellen](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Vergewissern Sie sich, dass in der Strukturansicht auf der linken Seite **AzureStorageLinkedService** und **HDInsightOnDemandLinkedService** angezeigt werden.

	![Strukturansicht mit verknüpften Diensten](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## Erstellen von Datasets
In diesem Schritt erstellen Sie Datasets, um die Eingabe- und Ausgabedaten für die Hive-Verarbeitung darzustellen. Diese Datasets verweisen auf den **AzureStorageLinkedService**, den Sie zuvor in diesem Tutorial erstellt haben. Der verknüpfte Dienst weist auf ein Azure Storage-Konto, und Datasets geben Container, Ordner und Dateiname in dem Speicher an, der Eingabe- und Ausgabedaten enthält.

### Erstellen eines Eingabedatasets

1. Klicken Sie im **Data Factory-Editor** in der Befehlszeile auf **... Weitere** und dann auf **Neues Dataset**, und wählen Sie **Azure-Blobspeicher**.

	![Neues Dataset](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in das Fenster „Draft-1“ ein. Im JSON-Codeausschnitt erstellen Sie ein Dataset mit dem Namen **AzureBlobInput**, das Eingabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus geben Sie an, dass die Eingabedaten im Blobcontainer **adfgetstarted** und im Ordner **inputdata** gespeichert werden.
		
		{
			"name": "AzureBlobInput",
		    "properties": {
		        "type": "AzureBlob",
		        "linkedServiceName": "AzureStorageLinkedService",
		        "typeProperties": {
		            "fileName": "input.log",
		            "folderPath": "adfgetstarted/inputdata",
		            "format": {
		                "type": "TextFormat",
		                "columnDelimiter": ","
		            }
		        },
		        "availability": {
		            "frequency": "Month",
		            "interval": 1
		        },
		        "external": true,
		        "policy": {}
		    }
		} 

	Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften, die im Codeausschnitt verwendet werden:

	| Eigenschaft | Beschreibung |
	| :------- | :---------- |
	| Typ | Die Type-Eigenschaft wird auf „AzureBlob“ festgelegt, da sich Daten im Azure-Blobspeicher befinden. |  
	| linkedServiceName | Verweist auf den „AzureStorageLinkedService“, den Sie zuvor erstellt haben. |
	| fileName | Diese Eigenschaft ist optional. Wenn Sie diese Eigenschaft nicht angeben, werden alle Dateien in „folderPath“ übernommen. In diesem Fall wird nur „input.log“ verarbeitet. |
	| Typ | Da die Protokolldateien im Textformat vorliegen, verwenden wir „TextFormat“. | 
	| columnDelimiter | Spalten werden in den Protokolldateien per Komma (,) voneinander getrennt. |
	| frequency/interval | „frequency“ wird auf „Month“ und „interval“ auf „1“ festgelegt, was bedeutet, dass die Eingabeslices monatlich verfügbar sind. | 
	| external | Diese Eigenschaft wird auf „true“ festgelegt, wenn die Eingabedaten nicht vom Data Factory-Dienst generiert werden. | 
	  	
3. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um das neu erstellte DataSet bereitzustellen. Das Dataset sollte in der Strukturansicht auf der linken Seite angezeigt werden.


### Erstellen des Ausgabedatasets
Nun erstellen Sie das Ausgabedataset, das die im Azure-Blobspeicher gespeicherten Ausgabedaten darstellt.

1. Klicken Sie im **Data Factory-Editor** in der Befehlszeile auf **... Weitere** und dann auf **Neues Dataset**, und wählen Sie **Azure-Blobspeicher**.
2. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in das Fenster „Draft-1“ ein. Im JSON-Codeausschnitt erstellen Sie ein Dataset mit dem Namen **AzureBlobOutput** und geben die Struktur der Daten an, die vom Hive-Skript erzeugt werden. Darüber hinaus geben Sie an, dass die Ergebnisse im Blobcontainer **adfgetstarted** und im Ordner **partitioneddata** gespeichert werden. Der Abschnitt **availability** gibt an, dass das Ausgabe-DataSet monatlich erzeugt wird.
	
		{
		  "name": "AzureBlobOutput",
		  "properties": {
		    "type": "AzureBlob",
		    "linkedServiceName": "AzureStorageLinkedService",
		    "typeProperties": {
		      "folderPath": "adfgetstarted/partitioneddata",
		      "format": {
		        "type": "TextFormat",
		        "columnDelimiter": ","
		      }
		    },
		    "availability": {
		      "frequency": "Month",
		      "interval": 1
		    }
		  }
		}

	Im Abschnitt **Erstellen des Eingabedatasets** werden diese Eigenschaften beschrieben. Sie legen die Eigenschaft „external“ nicht für ein Ausgabedataset fest, da das Dataset vom Data Factory-Dienst erstellt wird.
3. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um das neu erstellte DataSet bereitzustellen.
4. Überprüfen Sie, ob das Dataset erfolgreich erstellt wurde.

	![Strukturansicht mit verknüpften Diensten](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## Erstellen der Pipeline
In diesem Schritt erstellen Sie Ihre erste Pipeline mit einer **HDInsightHive**-Aktivität. Der Eingabeslice ist monatlich verfügbar („frequency“: „Month“, „interval“: „1“), der Ausgabeslice wird monatlich erstellt, und die scheduler-Eigenschaft für die Aktivität ist ebenfalls auf ein monatliches Intervall festgelegt. Die Einstellungen für das Ausgabedataset und den Aktivitätsplaner müssen übereinstimmen. Derzeit steuert das Ausgabedataset den Zeitplan, sodass Sie auch dann ein Ausgabedataset erstellen müssen, wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität keine Eingabe akzeptiert, können Sie das Erstellen des Eingabedatasets überspringen. Am Ende dieses Abschnitts werden die in der folgenden JSON verwendeten Eigenschaften erläutert.

1. Klicken Sie im **Data Factory-Editor** auf **(...) Weitere Befehle** und dann auf **Neue Pipeline**.
	
	![Schaltfläche "Neue Pipeline"](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in das Fenster „Draft-1“ ein.

	> [AZURE.IMPORTANT] Ersetzen Sie im JSON-Code **storageaccountname** durch den Namen Ihres Speicherkontos.
		
		{
		    "name": "MyFirstPipeline",
		    "properties": {
		        "description": "My first Azure Data Factory pipeline",
		        "activities": [
		            {
		                "type": "HDInsightHive",
		                "typeProperties": {
		                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
		                    "scriptLinkedService": "AzureStorageLinkedService",
		                    "defines": {
		                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
		                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
		                    }
		                },
		                "inputs": [
		                    {
		                        "name": "AzureBlobInput"
		                    }
		                ],
		                "outputs": [
		                    {
		                        "name": "AzureBlobOutput"
		                    }
		                ],
		                "policy": {
		                    "concurrency": 1,
		                    "retry": 3
		                },
		                "scheduler": {
		                    "frequency": "Month",
		                    "interval": 1
		                },
		                "name": "RunSampleHiveActivity",
		                "linkedServiceName": "HDInsightOnDemandLinkedService"
		            }
		        ],
		        "start": "2016-04-01T00:00:00Z",
		        "end": "2016-04-02T00:00:00Z",
		        "isPaused": false
		    }
		}
 
	Im JSON-Codeausschnitt erstellen Sie eine Pipeline, die aus einer einzelnen Aktivität besteht. Diese nutzt Hive, um Daten in einem HDInsight-Cluster Daten zu verarbeiten.
	
	Die Hive-Skriptdatei **partitionweblogs.hql** ist im Azure-Speicherkonto (das durch den scriptLinkedService-Dienst namens **AzureStorageLinkedService** angegeben ist) und im Ordner **script** im Container **adfgetstarted** gespeichert.

	Der Abschnitt **defines** dient zum Angeben der Laufzeiteinstellungen, die als Hive-Konfigurationswerte (z.B. „${hiveconf:inputtable}“, „${hiveconf:partitionedtable}“) an das Hive-Skript übergeben werden.

	Die Eigenschaften **start** und **end** der Pipeline geben den aktiven Zeitraum der Pipeline an.

	Im JSON-Code der Aktivität geben Sie an, dass das Hive-Skript auf der Computeinstanz ausgeführt wird, die vom **linkedServiceName** – **HDInsightOnDemandLinkedService** angegeben wurde.

	> [AZURE.NOTE] Einzelheiten zu JSON-Eigenschaften, die im Beispiel verwendet werden, finden Sie unter [Anatomie einer Pipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline).

3. Überprüfen Sie Folgendes:
	1. Die Datei **input.log** ist im Ordner **inputdata** des Containers **adfgetstarted** im Azure-Blobspeicher enthalten.
	2. Die Datei **partitionweblogs.hql** ist im Ordner **script** des Containers **adfgetstarted** im Azure-Blobspeicher enthalten. Führen Sie die vorbereitenden Schritte in der [Übersicht über das Tutorial](data-factory-build-your-first-pipeline.md) aus, wenn diese Dateien nicht vorhanden sind.
	3. Stellen Sie sicher, dass Sie im JSON-Code der Pipeline **storageaccountname** durch den Namen Ihres Speicherkontos ersetzt haben.
2. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um die Pipeline bereitzustellen. Da die Zeiten für **start** und **end** in der Vergangenheit festgelegt sind, und **isPaused** auf „false“ festgelegt ist, wird die Pipeline (Aktivität in der Pipeline) sofort nach der Bereitstellung ausgeführt.
4. Vergewissern Sie sich, dass die Pipeline in der Strukturansicht angezeigt wird.

	![Strukturansicht mit Pipeline](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Glückwunsch, Sie haben Ihre erste Pipeline erfolgreich erstellt!

## Überwachen der Pipeline

### Überwachen der Pipeline mit der Diagrammansicht

6. Klicken Sie auf **X**, um die Data Factory-Editor-Blätter zu schließen und zum Blatt „Data Factory“ zurückzukehren, und klicken Sie auf **Diagramm**.
  
	![Kachel "Diagramm"](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. In der Diagrammansicht sehen Sie eine Übersicht über die in diesem Tutorial verwendeten Pipelines und Datasets.
	
	![Diagrammansicht](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
8. Um alle Aktivitäten in der Pipeline anzuzeigen, klicken Sie im Diagramm mit der rechten Maustaste auf die Pipeline und klicken dann auf „Pipeline öffnen“.

	![Menü "Pipeline öffnen"](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Vergewissern Sie sich, dass Sie die HDInsightHive-Aktivität in der Pipeline sehen.
  
	![Ansicht „Pipeline öffnen“](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

	Um zurück zur vorherigen Ansicht zu navigieren, klicken Sie oben in der Breadcrumb-Leiste auf **Data Factory**.
10. Doppelklicken Sie in der **Diagrammansicht** auf das Dataset **AzureBlobInput**. Vergewissern Sie sich, dass der Slice sich im Zustand **Bereit** befindet. Es kann ein paar Minuten dauern, bis der Slice im Status „Bereit“ angezeigt wird. Wenn dies nicht geschieht, nachdem Sie einige Zeit gewartet haben, sollten Sie prüfen, ob Sie die Eingabedatei („input.log“) im richtigen Container („adfgetstarted“) und Ordner („inputdata“) platziert haben.

	![Eingabeslice im Status „Bereit“](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Klicken Sie auf **X**, um das Blatt **AzureBlobInput** zu schließen.
12. Doppelklicken Sie in der **Diagrammansicht** auf das Dataset **AzureBlobOutput**. Sie sehen den Slice, der gerade verarbeitet wird.

	![Datensatz](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Wenn die Verarbeitung abgeschlossen ist, wird der Slice mit dem Zustand **Bereit** angezeigt.
	>[AZURE.IMPORTANT] Die Erstellung eines bedarfsgesteuerten HDInsight-Clusters dauert in der Regel einige Zeit (etwa 20 Minuten). Daher ist damit zu rechnen, dass die Pipeline **etwa 30 Minuten** zum Verarbeiten des Slice benötigt.

	![Datensatz](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)
	
10. Sobald der Slice den Status **Bereit** hat, überprüfen Sie, ob die Ausgabedaten sich in Ihrem Blobspeicher im Ordner **partitioneddata** im Container **adfgetstarted** befinden.
 
	![Ausgabedaten](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Klicken Sie auf den Slice, um Details dazu auf dem Blatt **Datenslice** anzuzeigen.

	![Details zum Datenslice](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)
12. Klicken Sie in der Liste mit den **Aktivitätsausführungen** auf eine Aktivitätsausführung, um Details dazu (hier: die Hive-Aktivität) in einem Fenster mit dem Namen **Details zur Aktivitätsausführung** anzuzeigen.

	![Aktivitätsausführung – Details](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)
	
	In den Protokolldateien werden die ausgeführte Hive-Abfrage und Statusinformationen angezeigt. Diese Protokolle sind zur Behandlung von Problemen hilfreich. Ausführliche Informationen finden Sie im Artikel [Überwachen und Verwalten von Azure Data Factory-Pipelines](data-factory-monitor-manage-pipelines.md).

> [AZURE.IMPORTANT] Die Eingabedatei wird bei erfolgreicher Verarbeitung des Slice gelöscht. Wenn Sie den Slice erneut ausführen oder das Tutorial nochmals durchgehen möchten, laden Sie die Eingabedatei (input.log) daher in den Ordner „inputdata“ des Containers „adfgetstarted“ hoch.

### Überwachen der Pipeline mit der App „Überwachung und Verwaltung“
Sie können die App „Überwachung und Verwaltung“ auch zum Überwachen Ihrer Pipelines verwenden. Ausführliche Informationen zur Verwendung dieser App finden Sie unter [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der neuen App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md).

1. Klicken Sie auf der Startseite Ihrer Data Factory auf die Kachel **Überwachung und Verwaltung**.

	![Kachel „Überwachung und Verwaltung“](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Die **App „Überwachung und Verwaltung“** wird angezeigt. Ändern Sie die **Startzeit** und **Endzeit** in die Startzeit (04-01-2016 12:00 AM) und Endzeit (04-02-2016 12:00 AM) Ihrer Pipeline, und klicken Sie auf **Übernehmen**.

	![App „Überwachung und Verwaltung“](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Wählen Sie in der Liste **Aktivitätsfenster** ein Aktivitätsfenster aus, um die Details dazu anzuzeigen. ![Details zum Aktivitätsfenster](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


## Zusammenfassung 
In diesem Tutorial haben Sie eine Azure Data Factory zum Verarbeiten von Daten erstellt, indem Sie ein Hive-Skript in einem HDInsight Hadoop-Cluster ausgeführt haben. Sie haben den Data Factory-Editor im Azure-Portal verwendet, um die folgenden Schritte auszuführen:

1.	Sie haben eine Azure **Data Factory** erstellt.
2.	Sie haben zwei **verknüpfte Dienste** erstellt:
	1.	Einen verknüpften **Azure Storage**-Dienst zum Verknüpfen Ihres Azure-Blobspeichers, in dem die Eingabe- und Ausgabedateien der Data Factory enthalten sind.
	2.	Einen bedarfsgesteuerten verknüpften **Azure HDInsight**-Dienst zum Verknüpfen eines bedarfsgesteuerten HDInsight Hadoop-Clusters mit der Data Factory. Azure Data Factory erstellt einen HDInsight Hadoop-Cluster „just in time“, um Eingabedaten zu verarbeiten und Ausgabedaten zu erzeugen.
3.	Sie haben zwei **Datasets** erstellt, in denen Eingabe- und Ausgabedaten für eine HDInsight Hive-Aktivität in der Pipeline beschrieben werden.
4.	Sie haben eine **Pipeline** mit einer **HDInsight Hive**-Aktivität erstellt.

## Nächste Schritte
In diesem Artikel haben Sie eine Pipeline mit einer Transformationsaktivität (HDInsight-Aktivität) erstellt, die ein Hive-Skript in einem bedarfsgesteuerten HDInsight-Cluster ausführt. Informationen zum Verwenden einer Kopieraktivität zum Kopieren von Daten aus einem Azure-Blob in Azure SQL finden Sie unter [Lernprogramm: Kopieren von Daten aus einem Azure-Blob in Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## Weitere Informationen
| Thema | Beschreibung |
| :---- | :---- |
| [Transformationsaktivitäten von Daten](data-factory-data-transformation-activities.md) | Dieser Artikel enthält eine Liste mit Transformationsaktivitäten von Daten (z.B. die HDInsight Hive-Transformation, die Sie in diesem Tutorial verwendet haben), die von Azure Data Factory unterstützt werden. | 
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. |
| [Pipelines](data-factory-create-pipelines.md) | In diesem Artikel erhalten Sie Informationen zu Pipelines und Aktivitäten in Azure Data Factory und erfahren, wie diese zum Erstellen datengesteuerter End-to-End-Workflows für Ihr Szenario oder Ihr Unternehmen genutzt werden können. |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel enthält Informationen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Pipelines mit der Überwachungs-App](data-factory-monitor-manage-app.md) | In diesem Artikel wird das Überwachen, Verwalten und Debuggen von Pipelines mit der App für die Überwachung und Verwaltung beschrieben. 

  

<!---HONumber=AcomDC_0921_2016-->