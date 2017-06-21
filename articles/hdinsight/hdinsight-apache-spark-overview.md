---
title: "Einführung in Spark in Azure HDInsight | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Einführung in Spark in HDInsight und die verschiedenen Szenarien, in denen Sie Spark-Cluster in HDInsight verwenden können."
keywords: "Was ist Apache Spark, Spark-Cluster, Einführung in Spark, Spark in HDInsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 82334b9e-4629-4005-8147-19f875c8774e
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: acb80aa98cc978a906ccd6e4b4132a439e505bc8
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="introduction-to-spark-on-hdinsight"></a>Einführung in Spark in HDInsight

Dieser Artikel bietet eine Einführung in Spark in HDInsight. <a href="http://spark.apache.org/" target="_blank">Apache Spark</a> ist ein Open-Source-Framework für die Parallelverarbeitung, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von großen Datenmengen zu steigern. Ein Spark-Cluster in HDInsight ist kompatibel mit Azure Storage (WASB) und Azure Data Lake Store. Ihre vorhandenen in Azure gespeicherten Daten können also problemlos über einen Spark-Cluster verarbeitet werden.

Wenn Sie einen Spark-Cluster in HDInsight erstellen, erstellen Sie damit Azure-Computeressourcen, für die Spark installiert und konfiguriert ist. Es dauert nur ungefähr zehn Minuten, einen Spark-Cluster in HDInsight zu erstellen. Die zu verarbeitenden Daten werden in Azure Storage oder Azure Data Lake Store gespeichert. Weitere Informationen finden Sie unter [Verwenden von Azure Storage mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

**Informationen zum Erstellen eines Spark-Clusters in HDInsight** finden Sie unter [Schnellstart: Erstellen eines Spark-Clusters in HDInsight und Ausführen einer interaktiven Abfrage mithilfe von Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="what-is-apache-spark-on-azure-hdinsight"></a>Was ist Apache Spark in Azure HDInsight?
Spark-Cluster in HDInsight bieten einen vollständig verwalteten Spark-Dienst. Die Vorteile der Erstellung eines Spark-Clusters in HDInsight sind hier aufgeführt.

| Feature | Beschreibung |
| --- | --- |
| Einfache Erstellung von Spark-Clustern |Sie können einen neuen Spark-Cluster in HDInsight in wenigen Minuten erstellen, indem Sie das Azure-Portal, Azure PowerShell oder das HDInsight .NET SDK verwenden. Weitere Informationen finden Sie unter [Erste Schritte mit Spark-Clustern in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Einfache Bedienung |Spark-Cluster in HDInsight enthalten Jupyter und Zeppelin Notebooks. Diese können Sie für die interaktive Datenverarbeitung und -visualisierung verwenden.|
| REST-APIs |Spark-Cluster in HDInsight umfassen [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), einen auf der REST-API basierenden Spark-Auftragsserver, mit dem Benutzer Aufträge per Remotezugriff senden und überwachen können. |
| Unterstützung für Azure Data Lake-Speicher | Ein Spark-Cluster in HDInsight kann für die Verwendung von Azure Data Lake Store als zusätzlicher Speicher sowie als primärer Speicher konfiguriert werden (nur bei HDInsight 3.5-Clustern). Weitere Informationen zum Data Lake-Speicher finden Sie unter [Übersicht über Azure Data Lake-Speicher](../data-lake-store/data-lake-store-overview.md). |
| Integration in Azure-Dienste |Ein Spark-Cluster in HDInsight verfügt über einen Connector für Azure Event Hubs. Kunden können mit Event Hubs Streaminganwendungen erstellen. Dies ist eine Alternative zur Option [Kafka](http://kafka.apache.org/), die als Teil von Spark bereits verfügbar ist. |
| Unterstützung für R Server | Sie können eine R Server-Instanz in einem HDInsight Spark-Cluster einrichten, um verteilte R-Berechnungen mit den für einen Spark-Cluster garantierten Geschwindigkeiten auszuführen. Weitere Informationen finden Sie unter [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md)(Erste Schritte mit R Server in HDInsight). |
| Integration in Drittanbieter-IDEs | HDInsight bietet-Plug-Ins für IDEs wie IntelliJ IDEA und Eclipse, die Sie zum Erstellen und Übermitteln von Anwendungen an einen HDInsight Spark-Cluster verwenden können. Weitere Informationen finden Sie unter [Verwenden des Azure-Toolkits für IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md) und [Verwenden des Azure-Toolkits für Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md).|
| Gleichzeitige Abfragen |Spark-Cluster in HDInsight unterstützen gleichzeitige Abfragen. So können mehrere Abfragen von einem Benutzer oder mehrere Abfragen von unterschiedlichen Benutzern und Anwendungen dieselben Clusterressourcen verwenden. |
| Zwischenspeicherung auf SSDs |Sie können Daten entweder im Arbeitsspeicher oder auf SSDs zwischenspeichern, die an die Clusterknoten angefügt sind. Das Zwischenspeichern im Arbeitsspeicher liefert die beste Abfrageleistung, kann aber teuer sein. Das Zwischenspeichern auf SSDs ist eine hervorragende Möglichkeit zur Verbesserung der Abfrageleistung, ohne dass ein Cluster mit einer Größe erstellt werden muss, die für das Einfügen des gesamten Datasets in den Arbeitsspeicher ausreicht. |
| Integration in BI-Tools |Spark-Cluster in HDInsight enthalten Connectors für BI-Tools wie [Power BI](http://www.powerbi.com/) und [Tableau](http://www.tableau.com/products/desktop) für die Datenanalyse. |
| Vorinstallierte Anaconda-Bibliotheken |Für Spark-Cluster unter HDInsight sind Anaconda-Bibliotheken vorinstalliert. [Anaconda](http://docs.continuum.io/anaconda/) bietet ca. 200 Bibliotheken für Machine Learning, Datenanalyse, Visualisierung usw. |
| Skalierbarkeit |Obwohl Sie die Anzahl der Knoten im Cluster bei der Erstellung angeben können, sollten Sie den Cluster je nach Workload vergrößern oder verkleinern. Alle HDInsight-Cluster bieten die Möglichkeit, die Anzahl der Knoten im Cluster zu ändern. Darüber hinaus können Spark-Cluster ohne Datenverlust verworfen werden, da alle Daten in Azure Storage oder Data Lake Store gespeichert werden. |
| Support rund um die Uhr |Für Spark-Cluster in HDInsight ist rund um die Uhr professioneller Support verfügbar, und in der SLA ist eine Betriebszeit von 99,9 % angegeben. |

## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Welche Anwendungsfälle gibt es für Spark für HDInsight?
Spark-Cluster in HDInsight ermöglichen die folgenden Schlüsselszenarien.

### <a name="interactive-data-analysis-and-bi"></a>Interaktive Datenanalyse und BI
[Lernprogramm anzeigen](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark in HDInsight speichert Daten in Azure Storage oder Azure Data Lake Store. Experten und Entscheidungsträger in Unternehmen können diese Daten analysieren und Berichte damit erstellen und Microsoft Power BI verwenden, um aus den analysierten Daten interaktive Berichte anzufertigen. Analysten können mit unstrukturierten oder teilweise strukturierten Daten im Clusterspeicher beginnen, mit Notebooks ein Schema für die Daten definieren und dann mit Microsoft Power BI Datenmodelle erstellen. Spark-Cluster in HDInsight unterstützen auch eine Reihe von BI-Drittanbietertools wie Tableau. Daher ist es eine ideale Plattform für Datenanalysten, Unternehmensexperten und Entscheidungsträger.

### <a name="spark-machine-learning"></a>Spark Machine Learning
[Tutorial ansehen: Vorhersage von Gebäudetemperaturen mithilfe von HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Tutorial ansehen: Vorhersage von Lebensmittelüberwachungsergebnissen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark verfügt über [MLlib](http://spark.apache.org/mllib/), eine Machine Learning-Bibliothek, die auf Spark aufbaut und die Sie über ein Spark-Cluster in HDInsight verwenden können. Zu Spark-Clustern in HDInsight gehört zudem Anaconda, eine Python-Distribution mit vielen verschiedenen Paketen für Machine Learning. Wenn Sie dies mit der integrierten Unterstützung für Jupyter und Zeppelin Notebooks kombinieren, verfügen Sie über eine hochmoderne Umgebung zur Erstellung von Machine Learning-Anwendungen.

### <a name="spark-streaming-and-real-time-data-analysis"></a>Streaming und Echtzeit-Datenanalysen mit Spark
[Lernprogramm anzeigen](hdinsight-apache-spark-eventhub-streaming.md)

Spark-Cluster in HDInsight bieten umfassende Unterstützung für die Erstellung von Echtzeit-Analyselösungen. Spark verfügt zwar bereits über Connectors zum Erfassen von Daten aus den unterschiedlichsten Quellen, z. B. Kafka, Flume, Twitter, ZeroMQ oder TCP-Sockets, aber mit Spark in HDInsight wird zusätzlich noch die erstklassige Unterstützung für das Erfassen von Daten aus Azure Event Hubs hinzugefügt. Event Hubs sind der Warteschlangendienst, der in Azure am häufigsten verwendet wird. Da die Unterstützung von Event Hubs im Lieferumfang enthalten ist, sind Spark-Cluster in HDInsight eine ideale Plattform zum Erstellen einer Echtzeit-Analysepipeline.

## <a name="next-steps"></a>Welche Komponenten sind Bestandteile eines Spark-Clusters?
Spark-Cluster in HDInsight enthalten die folgenden Komponenten, die standardmäßig in den Clustern verfügbar sind.

* [Spark Core](https://spark.apache.org/docs/1.5.1/). Umfasst Spark Core, Spark SQL, Spark-Streaming-APIs, GraphX und MLlib.
* [Anaconda](http://docs.continuum.io/anaconda/)
* [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter-Notebook](https://jupyter.org)
* [Zeppelin-Notebook](http://zeppelin-project.org/)

Spark-Cluster in HDInsight verfügen auch über einen [ODBC-Treiber](http://go.microsoft.com/fwlink/?LinkId=616229) für die Konnektivität mit Spark-Clustern in HDInsight über BI-Tools wie Microsoft Power BI und Tableau.

## <a name="where-do-i-start"></a>Wo beginne ich?
Beginnen Sie mit der Erstellung eines Spark-Clusters in HDInsight. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Spark-Clusters in HDInsight (Linux) und Ausführen einer interaktiven Abfrage mit Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Nächste Schritte
### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)
* [Bekannte Probleme von Apache Spark in HDInsight](hdinsight-apache-spark-known-issues.md).

