<properties 
	pageTitle="Azure Data Factory – Häufig gestellte Fragen" 
	description="Häufig gestellte Fragen zu Azure Data Factory" 
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
	ms.date="06/28/2016" 
	ms.author="spelluru"/>

# Azure Data Factory – Häufig gestellte Fragen

## Allgemeine Fragen

### Was ist Azure Data Factory?

Data Factory ist ein cloudbasierter Datenintegrationsdienst, der das **Verschieben und Transformieren von Daten automatisiert**. Genau wie ein Fertigungsbetrieb, in dem Anlagen Rohmaterialien verarbeiten und in Endprodukte umwandeln, organisiert Data Factory vorhandene Dienste so, dass Rohdaten gesammelt und in nutzbare Informationen transformiert werden.
 
Der Data Factory-Dienst ermöglicht das Erstellen von datengesteuerten Workflows zum Verschieben von Daten zwischen lokalen und cloudbasierten Datenspeichern sowie zum Verarbeiten/Transformieren von Daten mithilfe von Computediensten wie Azure HDInsight und Azure Data Lake Analytics. Nachdem Sie eine Pipeline erstellt haben, die die gewünschte Aktion ausführt, können Sie die regelmäßige Ausführung der Pipeline planen (stündlich, täglich, wöchentlich usw.).

Unter [Übersicht und Schlüsselkonzepte](data-factory-introduction.md) finden Sie weitere Informationen.

### Wo finde ich Preisinformationen zu Azure Data Factory?

Preisinformationen zu Azure Data Factory finden Sie auf der Seite [Data Factory – Preisübersicht][adf-pricing-details].

### F: Was sind die ersten Schritte mit Azure Data Factory?

- Eine Übersicht über Azure Data Factory finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md).
- Ein Tutorial zum **Kopieren/Verschieben von Daten** mit der Kopieraktivität finden Sie unter [Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
- Ein Tutorial zum **Transformieren von Daten** mit der HDInsight Hive-Aktivität finden Sie unter [Verarbeiten von Daten durch Ausführen eines Hive-Skripts in einem Hadoop-Cluster](data-factory-build-your-first-pipeline.md).
  
### In welchen Regionen ist Data Factory verfügbar?
Data Factory ist in den Regionen **USA, Westen** sowie in **Europa, Norden** verfügbar. Die von Data Factory verwendeten Rechen- und Speicherdienste können in anderen Regionen verfügbar sein. Siehe [Unterstützte Regionen](data-factory-introduction.md#supported-regions).
 
### Welche Grenzwerte sind hinsichtlich der Anzahl der Data Factorys/Pipelines/Aktivitäten/Datasets gegeben?
 
Weitere Informationen finden Sie im Abschnitt **Einschränkungen von Azure Data Factory** unter dem Artikel [Begrenzungen, Kontingente und Einschränkungen von Azure-Abonnements und -Diensten](../azure-subscription-service-limits.md#data-factory-limits).

### Was sieht die Erstellung-/Entwicklungsumgebung im Azure Data Factory-Dienst aus?

Sie können Data Factorys mithilfe einer der folgenden Methoden erstellen:

- **Azure-Portal**: Die Data Factory-Blätter im Azure-Portal bieten eine umfassende Benutzeroberfläche zum Erstellen von Data Factorys und verknüpften Diensten. Der **Data Factory-Editor**, der auch Teil des Portals ist, ermöglicht Ihnen die einfache Erstellung von verknüpften Diensten, Tabellen, Datasets und Pipelines durch Angabe von JSON-Definitionen für diese Artefakte. Unter [Erstellen der ersten Azure Data Factory mit dem Azure-Portal](data-factory-build-your-first-pipeline-using-editor.md) finden Sie ein Beispiel für das Verwenden des Portals/Editors zum Erstellen und Bereitstellen einer Data Factory.

- **Visual Studio**: Sie können mit Visual Studio eine Azure Data Factory erstellen. Unter [Erstellen der ersten Azure Data Factory-Pipeline mit Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) finden Sie weitere Informationen.

- **Azure PowerShell**: Unter [Erstellen der ersten Azure Data Factory mit Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) finden Sie ein Tutorial und eine exemplarische Vorgehensweise zum Erstellen einer Data Factory mithilfe von PowerShell. In der [Data Factory-Cmdlet-Referenz][adf-powershell-reference] in der MSDN-Bibliothek finden Sie eine umfassende Dokumentation zu Data Factory-Cmdlets.
   
- **.NET-Klassenbibliothek** Sie können Data Factorys mithilfe des Data Factory .NET SDK programmgesteuert erstellen. Unter [Erstellen, Überwachen und Verwalten von Daten Factorys mit dem .NET SDK](data-factory-create-data-factories-programmatically.md) finden Sie eine exemplarische Vorgehensweise zum Erstellen einer Data Factory mit dem .NET SDK. Unter [Data Factory-Klassenbibliotheksreferenz][msdn-class-library-reference] finden Sie eine umfassende Dokumentation zum Data Factory .NET SDK.

- **REST-API** Sie können auch die vom Azure-Data Factory-Dienst verfügbar gemachte REST-API zum Erstellen und Bereitstellen von Data Factorys nutzen. Unter [Data Factory-REST-API][msdn-rest-api-reference] finden Sie eine umfassende Dokumentation zur Data Factory-REST-API.
 
- **Azure Resource Manager-Vorlage** Informationen hierzu finden Sie im [Tutorial: Erstellen der ersten Azure Data Factory mit einer Azure Resource Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md).

### Können Data Factorys umbenannt werden?
Nein. Wie bei anderen Azure-Ressourcen auch kann der Name einer Azure Data Factory nicht geändert werden.

### Kann ich eine Data Factory aus einem Azure-Abonnement in ein anderes verschieben? 
Ja. Verwenden Sie die Schaltfläche **Verschieben** auf Ihrem Data Factory-Blatt, wie unten dargestellt:

![Data Factory verschieben](media/data-factory-faq/move-data-factory.png)

## Aktivitäten – Häufig gestellte Fragen
### Welche verschiedenen Arten von Aktivitäten können in einer Data Factory-Pipeline verwendet werden? 

- [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) zum Verschieben von Daten.
- [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) zum Verarbeiten/Transformieren von Daten.

### Wann wird eine Aktivität ausgeführt?
Die Konfigurationseinstellung **availability** in der Ausgabedatentabelle bestimmt, wann die Aktivität erfolgt. Wenn Eingabedatasets angegeben sind, prüft die Aktivität, ob alle Eingabedatenabhängigkeiten erfüllt sind (den Status **ready** aufweisen), bevor die Ausführung beginnt.

## Kopieraktivität – Häufig gestellte Fragen
### Ist es besser, eine Pipeline mit mehreren Aktivitäten oder eine separate Pipeline für jede Aktivität einzurichten? 
Pipelines dienen zum Bündeln verwandter Aktivitäten. Natürlich können Sie die Aktivitäten in einer Pipeline halten, wenn die Tabellen, die diese verbinden, nicht von anderen Aktivitäten außerhalb der Pipeline genutzt werden. Auf diese Weise müssen Sie Pipelineaktivitäten nicht verknüpfen, damit diese sich aneinander ausrichten. Darüber hinaus kann die Datenintegrität in den Tabellen, die für die Pipeline intern sind, beim Aktualisieren der Pipeline besser beibehalten werden. Bei einer Pipelineaktualisierung werden alle Aktivitäten in der Pipeline beendet, entfernt und neu erstellt. Aus Erstellungssicht kann es auch einfacher sein, den Datenfluss innerhalb der zugehörigen Aktivitäten in einer JSON-Datei für die Pipeline nachzuverfolgen.

### Wo wird der Kopiervorgang ausgeführt? 

Ausführliche Informationen finden Sie im Abschnitt [Global verfügbare Datenverschiebung](data-factory-data-movement-activities.md#global). Kurz gesagt: Wenn ein lokaler Datenspeicher beteiligt ist, wird der Kopiervorgang vom Datenverwaltungsgateway in Ihrer lokalen Umgebung ausgeführt. Wenn Daten zwischen zwei Cloudspeichern bewegt werden, wird der Kopiervorgang in der Region ausgeführt, die dem Standort der Senke in der gleichen geografischen Region am nächsten liegt.


## HDInsight-Aktivität – Häufig gestellte Fragen

### In welchen Regionen wird HDInsight unterstützt?

Weitere Informationen finden Sie im Abschnitt zur geografischen Verfügbarkeit des folgenden Artikels oder unter [HDInsight – Preisübersicht][hdinsight-supported-regions].

### Welche Region wird von einem bedarfsgesteuerten HDInsight-Cluster verwendet?

Der bedarfsgesteuerte HDInsight-Cluster wird in derselben Region erstellt, in der sich der Speicher befindet, den Sie für die Verwendung mit dem Cluster angegeben haben.

### Wie können weitere Speicherkonten mit Ihrem HDInsight-Cluster verknüpft werden?

Wenn Sie Ihren eigenen HDInsight-Cluster (BYOC - Bring Your Own Cluster) verwenden, lesen Sie die folgenden Themen:

- [Verwenden eines HDInsight-Clusters mit alternativen Speicherkonten und Metastores][hdinsight-alternate-storage]
- [Verwenden zusätzlicher Speicherkonten mit HDInsight Hive][hdinsight-alternate-storage-2]

Wenn Sie einen bedarfsgesteuerten Cluster verwenden, der vom Data Factory-Dienst erstellt wird, müssen Sie zusätzliche Speicherkonten für den verknüpften- HDInsight-Dienst angeben, damit der Data Factory-Dienst diese in Ihrem Auftrag registrieren kann. Verwenden Sie in der JSON-Definition des bedarfsgesteuerten verknüpften Diensts die **additionalLinkedServiceNames**-Eigenschaft, um alternative Speicherkonten anzugeben, wie im folgenden JSON-Codeausschnitt gezeigt:
 
	{
	    "name": "MyHDInsightOnDemandLinkedService",
	    "properties":
	    {
	        "type": "HDInsightOnDemandLinkedService",
			"typeProperties": {
	        	"clusterSize": 1,
		        "timeToLive": "00:01:00",
		        "linkedServiceName": "LinkedService-SampleData",
		        "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ] 
			}
	    }
	} 

Im obigen Beispiel stellen "otherLinkedServiceName1" und "otherLinkedServiceName2" verknüpfte Dienste dar, deren Definitionen Anmeldeinformationen enthalten, die der HDInsight-Cluster für den Zugriff auf alternative Speicherkonten benötigt.

## Slices – Häufig gestellte Fragen

### Wieso weisen meine Eingabeslices nicht den Status „Bereit“ auf?  
Ein weit verbreiteter Fehler besteht darin, die **external**-Eigenschaft im Eingabedataset nicht auf **true** festzulegen, wenn die Eingabedaten für die Data Factory extern sind (also nicht von der Data Factory erstellt wurden).

Im folgenden Beispiel müssen Sie nur für **Dataset1** den Wert **external** auf „true“ festlegen.

**DataFactory1** Pipeline 1: Dataset1 -> Aktivität1 -> Dataset2 -> Aktivität2 -> Dataset3 Pipeline 2: Dataset3 -> Aktivität3 -> Dataset4

Wenn Sie über eine andere Data Factory mit einer Pipeline, die Dataset4 nimmt, verfügen (erstellen von Pipeline 2 in Data Factory 1), müssen Sie Dataset4 als externes Dataset markieren, da das Dataset von einer anderen Data Factory (DataFactory1, nicht DataFactory2) erstellt wird.

**DataFactory2** Pipeline 1: Dataset4 -> Aktivität4 -> Dataset5

Wenn die Eigenschaft „external“ richtig festgelegt wurde, überprüfen Sie, ob die Eingabedaten an dem Speicherort existieren, der in der Definition des Eingabedatasets angegeben wurde.

### Wie kann ein Slice zu einer anderen Zeit als Mitternacht ausgeführt werden, wenn der Slice täglich erstellt wird?
Verwenden Sie die **offset**-Eigenschaft, um die Zeit anzugeben, zu der der Slice erstellt werden soll. Weitere Informationen zu dieser Eigenschaft finden Sie im Abschnitt [Dataset: Availability](data-factory-create-datasets.md#Availability). Hier ist ein kurzes Beispiel:

	"availability":
	{
	    "frequency": "Day",
	    "interval": 1,
	    "offset": "06:00:00"
	}

Tägliche Slices starten anstatt zur Standardzeit (Mitternacht) um **6:00 Uhr**.

### Wie kann ich einen Slice erneut ausführen?
Sie können einen Slice auf eine der folgenden Arten erneut ausführen:

- Verwenden Sie die App „Überwachen und Verwalten“, um ein Aktivitätsfenster oder einen Slice erneut auszuführen. Anweisungen finden Sie unter [Wiederholen ausgewählter Aktivitätsfenster](data-factory-monitor-manage-app.md#re-run-selected-activity-windows).
- Klicken Sie im Portal auf der Befehlsleiste für den Slice auf dem Blatt **DATENSLICE** auf **Ausführen**.
- Führen Sie das Cmdlet **Set-AzureRmDataFactorySliceStatus** aus, wobei der Status des Slices auf **Waiting** festgelegt ist.
	
		Set-AzureRmDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00" 

Unter [Set-AzureRmDataFactorySliceStatus][set-azure-datafactory-slice-status] finden Sie ausführliche Informationen zum Cmdlet.

### Wie lange hat die Verarbeitung eines Slices gedauert?
Verwenden Sie den Aktivitätsfenster-Explorer in der App „Überwachen und Verwalten“, um zu erfahren, wie lange es gedauert hat, einen Datenslice zu verarbeiten. Weitere Informationen finden Sie unter [Aktivitätsfenster-Explorer](data-factory-monitor-manage-app.md#activity-window-explorer).

Sie können auch wie folgt im Azure-Portal vorgehen:

1. Klicken Sie auf der Kachel **Datasets** auf das Blatt **DATA FACTORY** für Ihre Data Factory.
2. Klicken Sie auf dem Blatt **Datasets** auf das gewünschte Dataset.
3. Wählen Sie auf dem Blatt **TABELLE** in der Liste **Zuletzt verwendete Slices** den gewünschten Slice aus.
4. Klicken Sie auf dem Blatt **DATENSLICE** in der Liste **Aktivitätsausführungen** auf die Aktivitätsausführung.
5. Klicken Sie auf der Kachel **Eigenschaften** auf das Blatt **DETAILS ZUR AKTIVITÄTSAUSFÜHRUNG**.
6. Daraufhin sollte das Feld **Dauer** mit einem Wert angezeigt werden. Dies ist die Verarbeitungszeit des Slices.

### Wie wird ein ausgeführter Slice beendet?
Wenn Sie die Ausführung der Pipeline beenden müssen, können Sie das Cmdlet [Suspend-AzureRmDataFactoryPipeline](https://msdn.microsoft.com/library/mt603721.aspx) verwenden. Derzeit werden laufende Sliceausführungen bei Anhalten der Pipeline nicht beendet. Sobald die laufenden Ausführungen abgeschlossen sind, wird kein zusätzlicher Slice ausgewählt.

Wenn Sie alle Ausführungen wirklich sofort beenden möchten, ist die einzige Möglichkeit das Löschen und erneute Erstellen der Pipeline. Wenn Sie die Pipeline löschen, müssen Sie keine Tabellen und verknüpften Dienste löschen, die von der Pipeline verwendet werden.


[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: https://msdn.microsoft.com/library/dn883654.aspx
[msdn-rest-api-reference]: https://msdn.microsoft.com/library/dn906738.aspx

[adf-powershell-reference]: https://msdn.microsoft.com/library/dn820234.aspx
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: https://msdn.microsoft.com/library/mt603522.aspx

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
 

<!---HONumber=AcomDC_0629_2016-->