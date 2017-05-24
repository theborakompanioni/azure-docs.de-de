---
title: "Verwenden eines Windows-Computers mit Hadoop in HDInsight – Azure | Microsoft-Dokumentation"
description: "Verwenden eines Windows-Computers mit Hadoop in HDInsight Verwalten und Abfragen von Clustern mit PowerShell, Visual Studio und Linux-Tools Entwickeln von Big Data-Lösungen mit .NET"
services: hdinsight
keywords: "Hadoop unter Windows, Hadoop für Windows"
author: cjgronlund
manager: jhubbard
ms.author: cgronlun
ms.date: 05/17/2017
ms.topic: article
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: e4f231c1f9b903d6cc7f2b062b30d2a072be8493
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="work-in-the-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Arbeiten im Hadoop-Ökosystem in HDInsight auf einem Windows-Computer

Erfahren Sie etwas über die Entwicklungs- und Verwaltungsoptionen auf einem Windows-Computer für die Arbeit im Hadoop-Ökosystem in HDInsight. 

HDInsight basiert auf Apache Hadoop und Hadoop-Komponenten sowie Open-Source-Technologien, die unter Linux entwickelt wurden. HDInsight 3.4 oder höher verwendet die Linux-Distribution Ubuntu als zugrunde liegendes Betriebssystem für den Cluster. Sie können aber auch von einem Windows-Client oder einer Windows-Entwicklungsumgebung aus mit HDInsight arbeiten.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Verwenden von PowerShell für Bereitstellungs- und Verwaltungsaufgaben
Azure PowerShell ist eine Skriptumgebung, mit der Sie Bereitstellungs- und Verwaltungsaufgaben in HDInsight von Windows aus steuern und automatisieren können.

Beispiele für Aufgaben, die Sie mit PowerShell erledigen können:

* [Erstellen von Clustern mit PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Ausführen von Hive-Abfragen mit PowerShell](hdinsight-hadoop-use-hive-powershell.md)
* [Verwalten von Clustern mit PowerShell](hdinsight-administer-use-powershell.md)

Führen Sie die Schritte zum [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) aus, um die neueste Version zu erhalten. Wenn Sie über Skripts verfügen, die zur Verwendung der neuen Cmdlets für Azure Resource Manager angepasst werden müssen, finden Sie unter [Migrieren zu Azure Resource Manager-basierten Entwicklungstools für HDInsight-Cluster](hdinsight-hadoop-development-using-azure-resource-manager.md) weitere Informationen.

## <a name="utilities-you-can-run-in-a-browser"></a>Hilfsprogramme für die Ausführung in einem Browser
Die folgenden Hilfsprogramme verfügen über eine Webbenutzeroberfläche, die in einem Browser ausgeführt wird:
* **[Azure Cloud Shell (Vorschau)](https://docs.microsoft.com/azure/cloud-shell/quickstart)** ist eine interaktive Befehlszeilen-Shell, die in Ihrem Browser und aus dem Azure-Portal heraus ausgeführt wird.
* Die **[Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md)** ist ein Hilfsprogramm zum Verwalten und Überwachen im Azure-Portal, mit dem Sie verschiedene Arten von Aufträgen verwalten können. Beispiele:
    * [Verwenden von Ambari mit der REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Hive-Ansicht in Ambari](hdinsight-hadoop-use-hive-ambari-view.md)
    * [Tez-Ansicht in Ambari](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Data Lake-Tools (Hadoop) für Visual Studio
Verwenden Sie die Data Lake-Tools für Visual Studio zum Bereitstellen und Verwalten von Storm-Topologien. Die Data Lake-Tools installieren auch das SCP.NET SDK, mit dem Sie C#-Storm-Topologien mit Visual Studio entwickeln können.

Bevor Sie mit den folgenden Beispielen fortfahren, [installieren und testen Sie die Data Lake-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md). 

Beispiele für Aufgaben, die Sie mit Visual Studio und den Data Lake-Tools für Visual Studio ausführen können:
* [Bereitstellen und Verwalten von Storm-Topologien in Visual Studio](hdinsight-storm-deploy-monitor-topology-linux.md)
* [Entwickeln von C#-Topologien für Storm mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) Die Tools enthalten Beispielvorlagen für Storm-Topologien, mit denen Sie Verbindungen mit Datenbanken wie Azure Cosmos DB und SQL-Datenbank herstellen können.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio und das .NET SDK 

Sie können Visual Studio mit dem .NET SDK verwenden, um Cluster zu verwalten und Big Data-Anwendungen zu entwickeln. Sie können auch andere IDEs für die folgenden Aufgaben verwenden, in den Beispielen wird jedoch Visual Studio gezeigt.

Beispiele für Aufgaben, die Sie mit dem .NET SDK in Visual Studio ausführen können:
* [Erstellen von Clustern und Arbeiten in HDInsight von einer .NET Framework-Anwendung aus](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Ausführen von Hive-Abfragen mit dem .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md)
* [Verwenden benutzerdefinierter C#-Funktionen mit Hive- und Pig-Streaming in Hadoop](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

> Tipp: Wenn Sie .NET-Lösungen für Windows-basierte HDInsight-Cluster ausführen, ist dies ein guter Zeitpunkt, eine Migration zu Linux-basierten Clustern zu planen. Weitere Informationen finden Sie unter [Migrieren von .NET-Lösungen für Windows-basierte HDInsight-Cluster zu Linux-basierten HDInsight-Clustern](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA und Eclipse-IDE für Spark-Cluster
Sowohl [Intellij IDEA](https://www.jetbrains.com/idea/download) als auch die [Eclipse-IDE](https://www.eclipse.org/downloads/) können für Folgendes verwendet werden:
* Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zugreifen auf Spark-Clusterressourcen
* Entwickeln und lokales Ausführen einer Scala Spark-Anwendung

In diesem Artikel wird Folgendes beschrieben: 
* Intellij IDEA: [Erstellen von Spark-Anwendungen mithilfe des Plug-Ins Azure-Toolkit für Intellij und des Scala SDK.](hdinsight-apache-spark-intellij-tool-plugin.md)
* Eclipse-IDE oder Scala-IDE für Eclipse: [Erstellen von Spark-Anwendungen und das Azure-Toolkit für Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Notebooks in Spark für Datenanalysten 
Apache Spark-Cluster in HDInsight enthalten Zeppelin-Notebooks und -Kernel, die mit Jupyter-Notebooks verwendet werden können. 

* [Erfahren Sie, wie Sie Kernel in Spark-Clustern mit Jupyter-Notebooks verwenden, um Spark-Anwendungen zu testen.](hdinsight-apache-spark-zeppelin-notebook.md)
* [Erfahren Sie, wie Sie Zeppelin-Notebooks in Spark-Clustern verwenden, um Spark-Aufträge auszuführen](hdinsight-apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Ausführen von Linux-basierten Tools und Technologien unter Windows

In Situationen, in denen Sie Tools oder Technologien verwenden müssen, die nur unter Linux verfügbar sind, sollten Sie die folgenden Möglichkeiten in Erwägung ziehen:

* **Bash (Beta) unter Windows 10** stellt ein Linux-Subsystem unter Windows bereit. Bash erlaubt die direkte Ausführung von Linux-Hilfsprogrammen, ohne eine dedizierte Linux-Installation verwalten zu müssen. [Installieren und Ausführen der Bash-Betaversion unter Windows 10](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Docker für Windows** ermöglicht den Zugriff auf viele Linux-basierten Tools und kann direkt unter Windows ausgeführt werden. Sie können Docker beispielsweise verwenden, um den Beeline-Client für Hive direkt unter Windows auszuführen. Sie können auch Docker verwenden, um ein lokales Jupyter-Notebook auszuführen und eine Remoteverbindung mit Spark in HDInsight herzustellen. [Erste Schritte mit Docker für Windows](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)** ermöglicht das Durchsuchen des Clusterdateisystems über eine SSH-Verbindung auf einer grafischen Benutzeroberfläche.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie nicht mit der Arbeit in Linux-basierten Clustern vertraut sind, lesen Sie die folgenden Artikel:
* [Einrichten von Hadoop-, Kafka-, Spark- oder anderen Clustern](hdinsight-hadoop-provision-linux-clusters.md)
* [Tipps zu HDInsight-Clustern unter Linux](hdinsight-hadoop-linux-information.md)
