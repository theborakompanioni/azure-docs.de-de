---
title: "Einführung in R Server in Azure HDInsight | Microsoft Docs"
description: "Sie erhalten hier eine Einführung in R Server in HDInsight. Erfahren Sie, wie Sie R Server zum Erstellen von Anwendungen für Big Data-Analysen verwenden."
services: hdinsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: cd57f3a43142b3af3546eafd9749123fadd333c2
ms.openlocfilehash: 2f5cf1155f116b22fa28cf86bb8c87667df981b7


---
#<a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>Einführung in R Server und Open-Source-R-Funktionen in HDInsight

Mit Microsoft Azure HDInsight ist Microsoft R Server bei der Erstellung von HDInsight-Clustern in Azure jetzt als Option verfügbar. Diese neue Funktion ermöglicht Datenanalysten, Statistikern und R-Programmierern bei Bedarf den Zugriff auf skalierbare, verteilte Analysemethoden in HDInsight.

Die Größe der Cluster kann an bevorstehende Projekte und Aufgaben angepasst werden. Werden sie nicht mehr benötigt, können sie auch entfernt werden. Da diese Cluster Teil von Azure HDInsight sind, steht für sie rund um die Uhr Support für Unternehmen zur Verfügung, und das SLA garantiert eine Betriebszeit von 99,9%. Außerdem besteht die Möglichkeit zur flexiblen Integration in andere Komponenten des Azure-Ökosystems.

R Server in HDInsight bietet die neuesten Funktionen für die R-basierte Analyse praktisch beliebig großer Datasets, die in Azure-Blobspeicher oder Data Lake Store geladen wurden. Da R Server auf der Open Source-Sprache R basiert, profitieren Ihre Anwendungen von mehr als 8.000 Open Source-Paketen für R und von den Routinen in „ScaleR“. Dies ist das Big Data-Analysepaket von Microsoft, das Bestandteil von R Server ist.

Der Edgeknoten eines Clusters ist ein praktischer Ort für die Verbindungsherstellung mit dem Cluster und die Ausführung Ihrer R-Skripts. Mit einem Edgeknoten haben Sie die Möglichkeit, die parallelisierten verteilten Funktionen von ScaleR über die Kerne der Edgeknotenserver hinweg auszuführen. Außerdem können Sie sie über die Knoten des Clusters hinweg ausführen, indem Sie Hadoop MapReduce von ScaleR oder Spark-Berechnungskontexte verwenden.

Die Modelle oder Vorhersagen, die sich aus den Analysen ergeben, können für die lokale Verwendung heruntergeladen werden. Sie können auch an anderer Stelle in Azure operationalisiert werden, z.B. über einen [Azure Machine Learning Studio](http://studio.azureml.net)-[Webdienst](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Erste Schritte mit R in HDInsight
Für die Einbeziehung von R Server in einen HDInsight-Cluster müssen Sie beim Erstellen eines HDInsight-Clusters mit dem Azure-Portal den Typ des R Server-Clusters auswählen. Der Typ des R Server-Clusters bezieht R Server auf den Datenknoten des Clusters ein, und auf einem Edgeknoten als Landezone für R Server-basierte Analysen. Eine ausführliche Anleitung zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md).

## <a name="learn-about-data-storage-options"></a>Informationen zu Datenspeicheroptionen
Standardspeicher für das HDFS-Dateisystem des HDInsight-Clusters kann entweder einem Azure Storage-Konto oder einem Azure Data Lake Store zugeordnet werden. So wird sichergestellt, dass alle Daten, die während einer Analyse in den Clusterspeicher hochgeladen werden, von dauerhafter Art sind. Es gibt verschiedene Tools für die Datenübertragung in die von Ihnen gewählte Speicheroption, einschließlich der portalbasierten Hochladefunktion für das Speicherkonto und des [AzCopy](../storage/storage-use-azcopy.md)-Hilfsprogramms.

Unabhängig davon, ob Sie Azure Blob Storage oder Data Lake Store als primären Speicher für Ihren Cluster auswählen, haben Sie die Möglichkeit, während der Clusterbereitstellung Zugriff auf zusätzliche Blobspeicher und Data Lake Stores hinzuzufügen. Unter [Erste Schritte mit R Server in HDInsight (Vorschau)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-get-started) finden Sie Informationen zum Hinzufügen von Zugriff auf zusätzliche Konten und im ergänzenden Artikel [Azure Storage-Optionen für R Server in HDInsight (Vorschau)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-storage) Informationen zur Verwendung mehrerer Speicherkonten.

Außerdem können Sie [Azure Files](../storage/storage-how-to-use-files-linux.md) als Speicheroption für den Edgeknoten wählen. Mit Azure Files können Sie eine Dateifreigabe, die in Azure Storage erstellt wurde, für das Linux-Dateisystem bereitstellen. Weitere Informationen zu Datenspeicheroptionen für R Server auf HDInsight-Clustern finden Sie unter [Azure Storage-Optionen für R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Zugreifen auf R Server im Cluster
Nach der Erstellung eines Clusters mit R Server können Sie auf dem Edgeknoten mit einem Browser eine Verbindung mit R Server herstellen, wenn Sie während der Bereitstellung entschieden haben, RStudio Server einzubeziehen, oder es später bzw. mithilfe von SSH/PuTTY für den Zugriff auf die R-Konsole hinzugefügt haben. Weitere Informationen zur Installation von RStudio Server nach dem Erstellen eines Clusters finden Sie unter [Installieren von RStudio mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Entwickeln und Ausführen von R-Skripts
Für von Ihnen geschriebene und ausgeführte R-Skripts können zusätzlich zu den parallelisierten und verteilten Routinen in der ScaleR-Bibliothek mehr als 8.000 Open Source-R-Pakete verwendet werden. Im Allgemeinen wird ein Skript, das mit R Server auf dem Edgeknoten ausgeführt wird, im R-Interpreter auf diesem Knoten ausgeführt. Ausgenommen hiervon sind die Schritte, die eine ScaleR-Funktion mit einem Berechnungskontext aufrufen, der auf Hadoop MapReduce (RxHadoopMR) oder Spark (RxSpark) festgelegt ist.

In diesen Fällen wird die Funktion über diejenigen Daten(aufgabe)knoten des Clusters ausgeführt, die den Daten zugeordnet sind, auf die verwiesen wird. Weitere Informationen zu den verschiedenen Optionen für Berechnungskontexte finden Sie unter [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operationalisieren eines Modells
Nach Abschluss der Datenmodellierung können Sie das Modell operationalisieren, um sowohl in Azure als auch lokal Vorhersagen über neue Daten treffen zu können. Dieser Prozess wird als Bewertung bezeichnet. Nachstehend finden Sie einige Beispiele:

### <a name="score-in-hdinsight"></a>Bewertung in HDInsight
Schreiben Sie für die Bewertung in HDInsight eine R-Funktion, mit der Ihr Modell aufgerufen wird, um Vorhersagen für eine neue Datendatei zu treffen, die Sie in Ihr Speicherkonto geladen haben. Speichern Sie die Vorhersagen dann wieder im Speicherkonto. Sie können die Routine bedarfsgesteuert auf dem Edgeknoten Ihres Clusters oder mithilfe eines geplanten Auftrags ausführen.  

### <a name="score-in-azure-machine-learning"></a>Bewertung in Azure Machine Learning
Verwenden Sie für die Bewertung mit einem Azure Machine Learning-Webdienst das Open Source Azure Machine Learning-R-Paket namens [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) , um Ihr Modell als Azure-Webdienst zu veröffentlichen. Der Einfachheit halber ist dieses Paket auf dem Edgeknoten vorinstalliert. Verwenden Sie als Nächstes die Funktion in Machine Learning, um eine Benutzeroberfläche für den Webdienst zu erstellen, und rufen Sie den Webdienst dann nach Bedarf für die Bewertung auf.

Wenn Sie diese Option wählen, müssen Sie zur Verwendung mit dem Webdienst alle ScaleR-Modellobjekte in entsprechende Open Source-Modellobjekte umwandeln. Die Umwandlung kann mithilfe von ScaleR-Koersionsfunktionen wie `as.randomForest()` für ensemblebasierte Modelle durchgeführt werden.

### <a name="score-on-premises"></a>Lokale Bewertung
Um nach der Erstellung Ihres Modells eine lokale Bewertung durchzuführen, können Sie das Modell in R serialisieren, herunterladen, deserialisieren und anschließend für die Bewertung neuer Daten verwenden. Sie können die Bewertung für neue Daten durchführen, indem Sie den weiter oben unter [Bewertung in HDInsight](#scoring-in-hdinsight) beschriebenen Ansatz verwenden oder [DeployR](https://deployr.revolutionanalytics.com/) nutzen.

## <a name="maintain-the-cluster"></a>Verwalten des Clusters
### <a name="install-and-maintain-r-packages"></a>Installieren und Verwalten von R-Paketen
Die meisten R-Pakete, die Sie verwenden, sind auf dem Edgeknoten erforderlich, da der Großteil Ihrer R-Skripts dort ausgeführt wird. Um auf dem Edgeknoten zusätzliche R-Pakete zu installieren, verwenden Sie in R die Methode `install.packages()` .

Wenn Sie nur Routinen aus der ScaleR-Bibliothek auf dem Cluster anwenden, müssen Sie in den meisten Fällen keine zusätzlichen R-Pakete auf den Datenknoten installieren. Sie benötigen aber unter Umständen zusätzliche Pakete, um die Verwendung der **rxExec**- oder **RxDataStep**-Ausführung auf den Datenknoten zu unterstützen.

In diesen Fällen können die zusätzlichen Pakete nach dem Erstellen des Clusters mithilfe einer Skriptaktion installiert werden. Weitere Informationen finden Sie unter [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Ändern der Hadoop MapReduce-Speichereinstellungen
Ein Cluster kann dahingehend geändert werden, dass er die Speichergröße, die R Server zur Verfügung steht, ändern kann, wenn ein MapReduce-Auftrag ausgeführt wird. Verwenden Sie zum Ändern eines Clusters die Apache Ambari-Benutzeroberfläche, die über das Blatt des Clusters im Azure-Portal verfügbar ist. Informationen zum Zugriff auf die Ambari-Benutzeroberfläche für Ihren Cluster finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

Sie können den für R Server zur Verfügung stehenden Speicher ändern, indem Sie beim Aufruf von **RxHadoopMR** das Argument „Hadoop switches“ wie folgt verwenden:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Skalieren des Clusters
Ein vorhandener Cluster kann über das Portal zentral hoch- oder herunterskaliert werden. Durch das Skalieren erzielen Sie die zusätzliche Kapazität, die Sie für umfangreichere Verarbeitungsaufgaben benötigen, oder Sie können einen im Leerlauf befindlichen Cluster wieder herunterskalieren. Eine Anleitung zum Skalieren eines Clusters finden Sie unter [Verwalten von HDInsight-Clustern](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Warten des Systems
Die Wartung wird auf den zugrundeliegenden Linux-VMs in einem HDInsight-Cluster außerhalb der Arbeitszeiten durchgeführt, um Betriebssystempatches und andere Updates anzuwenden. Normalerweise wird die Wartung jeden Montag und Donnerstag um 3:30 Uhr (Ortszeit der VM) durchgeführt. Die Updates werden so geplant, dass jeweils maximal ein Viertel des Clusters betroffen ist.  

Da die Hauptknoten redundant sind und nicht alle Datenknoten betroffen sind, werden alle Aufträge, die während dieses Zeitraums ausgeführt werden, unter Umständen verlangsamt. Sie sollten aber trotzdem vollständig ausgeführt werden. Jegliche benutzerdefinierte Software oder lokalen Daten, über die Sie ggf. verfügen, werden über diese Wartungsereignisse hinweg erhalten – sofern kein schwerwiegender Fehler auftritt, der eine Neuerstellung des Clusters erfordert.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Informationen zu IDE-Optionen für R Server in einem HDInsight-Cluster
Der Linux-Edgeknoten auf einem HDInsight-Cluster stellt die Landezone für R-basierte Analysen dar.  Neuere Versionen von HDInsight bieten eine Standardoption zum Installieren der Communityversion von [RStudio Server](https://www.rstudio.com/products/rstudio-server/) als browserbasierte IDE auf dem Edgeknoten. Die Verwendung von RStudio Server als IDE für die Entwicklung und Ausführung von R-Skripts kann erheblich produktiver sein als die ausschließliche Verwendung der R-Konsole. Wenn Sie entschieden haben, RStudio Server beim Erstellen des Clusters nicht hinzuzufügen, es aber später hinzufügen möchten, dann lesen Sie [Installieren von RStudio mit R Server in HDInsight (Vorschau)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-install-r-studio).+

Eine weitere vollständige IDE-Option ist die Installation einer Desktop-IDE, wie z.B. den vor kurzem angekündigten [R-Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) von Microsoft, RStudio oder des Eclipse-basierten [StatET](http://www.walware.de/goto/statet) von Walware und der Clusterzugriff durch Verwendung eines Remote-MapReduce- oder Spark-Rechenkontexts.  

Schließlich können Sie einfach nach dem Herstellen einer Verbindung über SSH oder PuTTY durch Eingabe von **R** an der Linux-Eingabeaufforderung auf die R Server-Verwaltungskonsole auf dem Edge-Knoten zugreifen. Die Verwendung der Konsolenschnittstelle wird verbessert, wenn Sie einen Texteditor für die R-Skriptentwicklung in einem anderen Fenster ausführen und Abschnitte Ihres Skripts nach Bedarf ausschneiden und in die R-Konsole einfügen.

## <a name="learn-about-pricing"></a>Informationen zu Preisen
Die Gebühren für einen HDInsight-Cluster mit R Server sind ähnlich strukturiert wie die Gebühren für die HDInsight-Standardcluster. Sie basieren auf der Größe der zugrundeliegenden VMs des Clusters sowie der Daten und Edgeknoten. Hinzu kommt eine stündliche Zusatzgebühr pro Kern. Weitere Informationen zu den Preisen für HDInsight und zur Verfügbarkeit einer kostenlosen Testversion für 30 Tage, finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Nächste Schritte
Folgen Sie den nachstehenden Links, um mehr über die Verwendung von R Server mit HDInsight-Clustern zu erfahren.

* [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md)
* [Hinzufügen von RStudio Server zu HDInsight (falls die Installation nicht bereits während der Clustererstellung durchgeführt wurde)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage-Optionen für R Server in HDInsight](hdinsight-hadoop-r-server-storage.md)



<!--HONumber=Jan17_HO2-->


