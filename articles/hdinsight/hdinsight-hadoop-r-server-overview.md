<properties
	pageTitle="Was ist R in HDInsight? Einführung in R Server in HDInsight (Vorschau) | Microsoft Azure"
	description="Informationen zu R Server in HDInsight (Vorschau) und zur Verwendung von R zum Erstellen von Anwendungen für Big Data-Analysen."
	services="hdinsight"
	documentationCenter=""
	authors="jeffstokes72"
	manager="paulettm"
	editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/01/2016"
   ms.author="jeffstok"/>


#Übersicht: R Server in HDInsight (Vorschau)

Mit HDInsight Premium steht Ihnen bei der Erstellung eines HDInsight-Clusters in Azure neuerdings R Server als Option zur Verfügung. Diese neue Funktion ermöglicht Datenanalysten, Statistikern und R-Programmierern bei Bedarf den Zugriff auf skalierbare, verteilte Analysemethoden in HDInsight. Die Größe der Cluster kann an bevorstehende Projekte und Aufgaben angepasst werden. Werden Sie nicht mehr benötigt, können sie auch entfernt werden. Diese Cluster sind Teil von Azure HDInsight, für sie steht Ihnen rund um die Uhr Support auf Unternehmensebene zur Verfügung, die SLA garantiert eine Betriebszeit von 99,9 % und sie bieten die Flexibilität der Integration mit anderen Komponenten des Azure-Ökosystems.

>[AZURE.NOTE] R-Server ist nur mit HDInsight Premium verfügbar. Zum jetzigen Zeitpunkt ist HDInsight Premium nur für Hadoop- und Spark-Cluster verfügbar. Deshalb können Sie R-Server in HDInsight derzeit nur mit Hadoop und Spark-Clustern verwenden. Weitere Informationen finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Hadoop-Clusterversionen](hdinsight-component-versioning.md).

R-Server in HDInsight bietet die neuesten Funktionen für die R-basierte Analyse großer Datasets, die in Azure Blob (WASB) geladen wurden. Da R-Server auf der Open Source-Sprache R basiert, profitieren Ihre Anwendungen von mehr als 8000 Open Source-Paketen für R und von den Routinen in Microsofts Big Data-Analysepaket ScaleR, das Bestandteil von R Server ist. Der Edgeknoten der Premium-Cluster stellt eine praktische Landezone für die Verbindung mit dem Cluster und die Ausführung Ihrer R-Skripts bereit. Mit einem Edgeknoten können Sie ScaleRs parallelisierte verteilte Funktionen entweder auf den Kernen des Edgeknotenservers oder auf den Knoten des Clusters ausführen. Hierfür müssen Sie ScaleRs Rechenkontexte Hadoop MapReduce oder Spark verwenden. Die aus Analysen entstehenden Modelle oder Vorhersagen können heruntergeladen werden (für die lokale Verwendung) oder an anderer Stelle in Azure operationalisiert werden, z.B. über einen [Azure Machine Learning Studio](http://studio.azureml.net) (AML)-[Webdienst](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## Erste Schritte mit R in HDInsight

Um R Server bei einem HDInsight-Cluster verwenden zu können, müssen Sie bei aktivierter Premium-Option im Azure-Portal einen Hadoop- oder Spark-Cluster erstellen. Beide Clustertypen verwenden die gleiche Konfiguration. Diese enthält R Server für die Datenknoten eines Clusters und einen Edgeknoten als Landezone für R Server-basierte Analysen. Eine ausführliche Anleitung zum Erstellen eines Clusters finden Sie unter [Getting started with R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (Verwenden von R Server in HDInsight).

## Datenspeicheroptionen

Als Standardspeicher für HDInsight-Cluster dient Azure Blob (WASB) mit dem HDFS-Dateisystem, das einem Blobcontainer zugeordnet ist. So wird sichergestellt, dass alle Daten, die in den Clusterspeicher hochgeladen oder in diesen während einer Analyse geschrieben werden, dauerhaft sind. Mithilfe des Dienstprogramms [AzCopy](../storage/storage-use-azcopy.md) lassen sich Daten auf einfache Art und Weise in und aus Blobs kopieren.

Zusätzlich zum Blob können Sie festlegen, dass Ihr Cluster [Azure Data Lake-Speicher](https://azure.microsoft.com/services/data-lake-store/) (Azure Data Lake Storage, ADLS) nutzt. Wenn Sie die Verwendung von ADLS hinzufügen, können Sie die Verwendung von Blob und ADLS für den HDFS-Speicher kombinieren.

Sie können auch [Azure Files](../storage/storage-how-to-use-files-linux.md) als Speicheroption für den Edgeknoten wählen. Mit Azure Files können Sie dem Linux-Dateisystem Dateifreigaben bereitstellen, die mit einem Azure Storage-Konto erstellt wurden. Weitere Informationen zu den Datenspeicheroptionen für R Server bei HDInsight-Clustern finden Sie unter [Azure Storage-Optionen für R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-storage.md).
  
## Zugreifen auf R Server im Cluster

Sobald Sie einen Cluster mit R Server erstellt haben, können Sie eine Verbindung zur R-Konsole auf dem Edgeknoten des Clusters herstellen. Dies kann mithilfe von SSH/Putty erfolgen oder alternativ über einen Browser, wenn Sie die integrierte Entwicklungsumgebung (IDE) für RStudio Server auf dem Edgeknoten installieren (optional). Weitere Informationen zur Installation von RStudio Server finden Sie unter [Installieren von RStudio mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-install-r-studio.md).

## Entwickeln und Ausführen von R-Skripts

Von Ihnen geschriebene und ausgeführte R-Skripts können zusätzlich zu den parallelisierten und verteilten Routinen in der ScaleR-Bibliothek mehr als 8000 Open Source-Pakete verwenden. Für die Ausführung von Skripts in R Server auf dem Edgeknoten wird der R-Interpreter verwendet. Dies gilt nicht für diejenigen Schritte, die eine ScaleR-Funktion mit dem Berechnungskontext Hadoop MapReduce (RXHadoopMR) oder Spark (RxSpark) aufrufen. In diesen Fällen wird die Funktion über diejenige Daten(aufgabe)knoten des Clusters ausgeführt, die den Daten zugeordnet sind, auf die verwiesen wird. Weitere Informationen zu den verschiedenen Optionen für Berechnungskontexte finden Sie unter [Rechenkontextoptionen für R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-compute-contexts.md).

## Operationalisieren eines Modells

Nach Abschluss der Datenmodellierung können Sie das Modell operationalisieren, um sowohl in Azure als auch lokal Vorhersagen über neue Daten treffen zu können („scoring“). Nachstehend finden Sie einige Beispiele:

### Scoring in HDInsight

Um ein Scoring in HDInsight durchzuführen, könnten Sie eine R-Funktion schreiben, die Ihr Modell aufruft, um Vorhersagen zu einer neuen, in Ihr Azure Storage-Konto geladenen Datendatei zu machen und die Vorhersagen wieder in das Storage-Konto speichern. Sie könnten die Routine bedarfsgesteuert auf dem Edgeknoten Ihres Clusters oder mithilfe eines geplanten Auftrags ausführen.

### Scoring in Azure Machine Learning 

Um mithilfe des Azure Machine Learning-Webdiensts ein Scoring durchzuführen, könnten Sie das [Open Source-Azure Machine Learning-R-Paket](http://www.inside-r.org/blogs/2015/11/18/enhancements-azureml-package-connect-r-azureml-studio) zum [Veröffentlichen Ihres Modells als ein Azure-Webdienst](http://www.r-bloggers.com/deploying-a-car-price-model-using-r-and-azureml/) verwenden und die Möglichkeiten in Azure Machine Learning zum Erstellen einer Benutzeroberfläche für den Webdienst verwenden. Den Webdienst könnten Sie dann nach Bedarf zum Scoring aufrufen. Wenn Sie diese Option wählen, müssen Sie zur Verwendung mit dem Webdienst alle ScaleR-Modellobjekte in entsprechende Open Source-Modellobjekte umwandeln. Dies kann mithilfe der ScaleR-Koersionsfunktionen wie `as.randomForest()` für ensemblebasierte Modelle durchgeführt werden.
  
### Lokales Scoring

Um nach der Erstellung Ihres Modells ein lokales Scoring durchzuführen, könnten Sie das Modell in R serialisieren, es lokal herunterladen, es deserialisieren und es anschließend zum Scoring neuer Daten verwenden. Sie können dies mithilfe der oben aufgeführten Vorgehensweise für [Scoring in HDInsight](#scoring-in-hdinsight) oder mithilfe von [DeployR](https://deployr.revolutionanalytics.com/) durchführen.

## Verwalten des Clusters 

### Installieren und Verwalten von R-Paketen

Die meisten der R-Pakete, die Sie verwenden, werden auf dem Edgeknoten benötigt, da die Mehrheit Ihrer R-Skripts dort ausgeführt wird. Um auf dem Edgeknoten zusätzliche R-Pakete zu installieren, verwenden Sie in R die Methode `install.packages()`.
  
Wenn Sie nur Routinen aus der ScaleR-Bibliothek auf den Cluster anwenden, müssen Sie in den meisten Fällen keine zusätzlichen R-Pakete auf den Datenknoten installieren. Jedoch benötigen Sie möglicherweise zusätzliche Pakete, um die Verwendung der **rxExec**- oder **RxDataStep**-Ausführung auf den Datenknoten zu unterstützen. In solchen Fällen müssen diese zusätzlichen Pakete nach dem Erstellen des Clusters mithilfe einer Skriptaktion angegeben werden. Weitere Informationen finden Sie unter [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md).
  
### Ändern der Hadoop-MapReduce-Speichereinstellungen 

Ein Cluster kann dahingehend geändert werden, dass er die Speichergröße, die R Server zur Verfügung steht, ändern kann, wenn ein MapReduce-Auftrag ausgeführt wird. Um dies durchzuführen, verwenden Sie die Ambari-Benutzeroberfläche, die Ihnen über das Azure-Portal-Blatt für Ihren Cluster zur Verfügung steht. Informationen zum Zugriff auf die Ambari-Benutzeroberfläche für Ihren Cluster finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

Sie können den für R Server zur Verfügung stehenden Speicher auch beim Aufrufen von **RxHadoopMR** ändern, indem Sie das Argument „Hadoop switches“ wie nachstehend gezeigt verwenden.
 
	hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### Skalieren des Clusters

Ein vorhandener Cluster kann über das Azure-Portal hoch- oder herunterskaliert werden. Dies kann verwendet werden, um die für eine größere Verarbeitung benötigte zusätzliche Kapazität zu schaffen oder um einen Cluster wieder herunterzuskalieren, wenn er inaktiv ist. Anweisungen zum Skalieren eines Clusters finden Sie unter [Verwalten von HDInsight-Clustern](hdinsight-administer-use-portal-linux.md).

### Systemwartung 

Die Wartung wird auf den zugrundeliegenden Linux-VMs in einem HDInsight-Cluster außerhalb der Arbeitszeiten durchgeführt, um Betriebssystempatches etc. anzuwenden. In der Regel erfolgt dies um 03:30 Uhr (basierend auf der Ortszeit der VM) immer montags und donnerstags. Updates werden so durchgeführt, dass immer nur maximal ¼ des Clusters auf einmal betroffen ist. Da die Hauptknoten redundant sind und nicht alle Datenknoten betroffen sind, werden alle Aufträge, die zu dieser Zeit ausgeführt werden, möglicherweise verlangsamt, sollten aber abgeschlossen werden. Jede möglicherweise installierte, benutzerdefinierte Software bzw. alle lokalen Daten werden über diese Wartungsereignisse hinweg erhalten, sofern kein schwerwiegender Fehler auftritt, der eine Neuerstellung des Clusters erfordert.

## IDE-Optionen für R Server auf HDInsight-Clustern

Der Linux-Edgeknoten auf einem HDInsight-Premium-Cluster stellt die Landezone für R-basierte Analysen dar. Nach dem Verbinden mit dem Cluster können Sie die Konsolenschnittstelle R Server starten, indem Sie „R“ in die Linux-Eingabeaufforderung eingeben. Die Verwendung der Konsolenschnittstelle wird verbessert, wenn Sie einen Texteditor für die R-Skriptentwicklung in einem anderen Fenster ausführen und Abschnitte Ihres Skripts nach Bedarf ausschneiden und in die R-Konsole einfügen.
  
Mit einer R-basierten IDE statt einem simplen Texteditor heben Sie die Entwicklung Ihrer R-Skripts auf eine neue Ebene. Beispiele hierfür sind die von Microsoft kürzlich angekündigten [R Tools for Visual Studio](https://www.visualstudio.com/de-DE/features/rtvs-vs.aspx) (RTVS), eine Reihe von Desktop- und Servertools von [RStudio](https://www.rstudio.com/products/rstudio-server/), oder die auf Eclipse basierende Entwicklungsumgebung [StatET](http://www.walware.de/goto/statet) von WalWare.
  
Eine weitere Möglichkeit ist das Installieren einer IDE auf dem Linux-Edgeknoten selbst. Eine beliebte Wahl in solchen Fällen ist [RStudio Server](https://www.rstudio.com/products/rstudio-server/), der eine browserbasierte IDE zur Verwendung durch Remoteclients bereitstellt. Das Installieren von RStudio Server auf dem Edgeknoten eines HDInsight-Premium-Clusters bietet eine umfassende IDE-Umgebung für die Entwicklung und Ausführung von R-Skripts mit R Server im Cluster und kann die Produktivität im Vergleich zur Standardverwendung der R-Konsole deutlich steigern. Weitere Informationen zur Verwendung von RStudio Server finden Sie unter [Installieren von RStudio mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-install-r-studio.md).

## Preise
 
Die Gebühren für einen HDInsight Premium-Cluster mit R Server sind ähnlich strukturiert wie die Gebühren für normale HDInsight-Cluster. Sie basieren auf der Größe der zugrundeliegenden VMs des Clusters sowie der Daten und Edgeknoten. Hinzu kommt eine stündliche Premium-Zusatzgebühr pro Kern. Weitere Informationen zu den Preisen für HDInsight Premium, einschließlich der Preise während der öffentlichen Vorschauphase und der Verfügbarkeit einer kostenlosen Testversion für 30 Tage, finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

## Nächste Schritte

Folgen Sie den nachstehenden Links, um mehr über die Verwendung von R Server mit HDInsight-Clustern zu erfahren.

- [Get started using R Server on HDInsight (Erste Schritte beim Verwenden von R Server in HDInsight)](hdinsight-hadoop-r-server-get-started.md)

- [Installing RStudio on HDInsight cluster with R Server (Installieren von RStudio auf einem HDInsight-Cluster mit R Server)](hdinsight-hadoop-r-server-install-r-studio.md)

- [Compute context options for R Server on HDInsight Premium (Optionen für Berechnungskontexte für R Server in HDInsight Premium)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Storage options for R Server on HDInsight Premium (Azure Storage-Optionen für R Server in HDInsight Premium)](hdinsight-hadoop-r-server-storage.md)

 

<!---HONumber=AcomDC_0608_2016-->