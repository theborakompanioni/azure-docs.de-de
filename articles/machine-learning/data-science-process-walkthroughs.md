---
title: "Exemplarische Vorgehensweisen für den Team Data Science-Prozess | Microsoft Docs"
description: Exemplarische Vorgehensweisen zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aa63d5a5-25ee-4c4b-9a4c-7553b98d7f6e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: bradsev
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 6df3fc533b0c375cd85743a971c778071629bb7e
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
# <a name="team-data-science-process-walkthroughs"></a>Exemplarische Vorgehensweisen für den Team Data Science-Prozess
Die hier aufgeführten umfassenden **exemplarischen Vorgehensweisen** veranschaulichen jeweils die Schritte im Team Data Science-Prozess für **bestimmte Szenarios**. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer **intelligenten Anwendung** kombiniert werden. Die exemplarischen Vorgehensweisen sind nach der verwendeten **Plattform** gruppiert: Spark, HDInsight (Hadoop), Azure Data Lake und SQL Server.


## <a name="hdinsight-spark-using-pyspark-and-scala"></a>HDInsight Spark mit PySpark und Scala

### <a name="data-science-using-python-with-spark-on-azure"></a>Data Science unter Verwendung von Python mit Spark in Azure
In der exemplarischen Vorgehensweise [Übersicht über Data Science mit Spark in Azure HDInsight](machine-learning-data-science-spark-overview.md) wird der Team Data Science-Prozess in einem Szenario mit einem [Azure HDInsight Spark-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet, um Daten aus dem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York zu speichern, zu untersuchen und bei der Entwicklung von Features zu nutzen. 

### <a name="data-science-using-scala-with-spark-on-azure"></a>Data Science unter Verwendung von Scala mit Spark in Azure
Die exemplarische Vorgehensweise [Data Science unter Verwendung von Scala mit Spark in Azure](machine-learning-data-science-process-scala-walkthrough.md) zeigt die Verwendung von Scala für überwachte Machine Learning-Aufgaben mit der Machine Learning-Bibliothek (MLlib) von Spark und Spark ML-Paketen auf einem Azure HDInsight Spark-Cluster. Sie führt Sie durch die Aufgaben, aus denen der [Data Science-Prozess](http://aka.ms/datascienceprocess)besteht: Erfassen und Durchsuchen von Daten, Visualisierung, Featureentwicklung, Modellierung und Modellnutzung. Die erstellten Modelle umfassen logistische und lineare Regression, Random Forests und Gradient-Boosted-Strukturen.


## <a name="hdinsight-hadoop"></a>HDInsight Hadoop 

### <a name="use-hdinsight-hadoop-clusters"></a>HDInsight Hadoop-Cluster
In der exemplarischen Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von HDInsight Hadoop-Clustern](machine-learning-data-science-process-hive-walkthrough.md) wird zum Speichern und Untersuchen von Daten aus einem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York sowie zum Entwickeln entsprechender Features ein [Azure HDInsight Hadoop-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet.

### <a name="use-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Azure HDInsight Hadoop-Cluster in einem 1-TB-Dataset
In dem umfassenden Szenario der exemplarischen Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von Azure HDInsight Hadoop-Clustern in einem 1-TB-Dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) wird zum Speichern, Untersuchen und Komprimieren von Daten aus einem der öffentlich zugänglichen [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/)-Datasets sowie zum Entwickeln entsprechender Features ein [Azure HDInsight Hadoop-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet.


## <a name="azure-data-lake"></a>Azure Data Lake

### <a name="use-azure-data-lake-store-and-analytics"></a>Verwenden von Azure Data Lake Storage und Analytics
Unter [Skalierbare Data Science in Azure Data Lake: lückenlose exemplarische Vorgehensweise](machine-learning-data-science-process-data-lake-walkthrough.md) wird veranschaulicht, wie Sie Azure Data Lake auf der Grundlage des Taxi-Datasets für New York zur Untersuchung von Daten sowie für binäre Klassifizierungsaufgaben nutzen, um vorherzusagen, ob vom Fahrgast ein Trinkgeld gegeben wird. 


## <a name="sql-server-and-sql-data-warehouse"></a>SQL Server und SQL Data Warehouse 

### <a name="use-sql-data-warehouse"></a>SQL Data Warehouse
Die exemplarische Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Data Warehouse](machine-learning-data-science-process-sqldw-walkthrough.md) zeigt, wie Sie mit SQL Data Warehouse (SQL DW) Modelle für die Machine Learning-Klassifizierung und -Regression für ein öffentlich zugängliches Dataset mit Taxifahrten und Fahrpreisen für New York erstellen und bereitstellen.

### <a name="use-sql-server"></a>SQL Server
Die exemplarische Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md) zeigt, wie Sie mit SQL Server und einem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York Modelle für die Machine Learning-Klassifizierung und -Regression erstellen und bereitstellen.

### <a name="use-r-with-sql-server-r-services"></a>R mit SQL Server R-Diensten
Die exemplarische Vorgehensweise [Data Science End-to-End Walkthrough using SQL Server R Services](https://msdn.microsoft.com/library/mt612857.aspx) (Lückenlose exemplarische Vorgehensweise für Data Science mit SQL Server R-Diensten) bietet Data Scientists eine Kombination aus R-Code, SQL Server-Daten und benutzerdefinierten SQL-Funktionen für die Erstellung und Bereitstellung eines R-Modells für SQL Server.

### <a name="use-t-sql-with-sql-server-r-services"></a>T-SQL mit SQL Server R-Diensten
In der exemplarischen Vorgehensweise [In-Database Advanced Analytics for SQL Developers](https://msdn.microsoft.com/library/mt683480.aspx) (Datenbankinterne erweiterte Analysen für SQL-Entwickler) können SQL-Programmierer Erfahrungen mit der Erstellung einer erweiterten Analyselösung mit Transact-SQL unter Verwendung von SQL Server R-Diensten sammeln, um eine R-Lösung zu operationalisieren.


### <a name="use-t-sql-with-sql-server-python-services"></a>Verwenden von T-SQL mit SQL Server Python-Diensten
In der exemplarischen Vorgehensweise [In-Database Python Analytics for SQL Developers](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) (Datenbankinterne Python-Analyse für SQL-Entwickler) stellt erfahrenen SQL-Programmierern eine Machine Learning-Lösung in SQL Server bereit. Sie veranschaulicht, wie Python in eine Anwendung integriert werden kann, indem gespeicherten Prozeduren Python-Code hinzugefügt wird.

## <a name="whats-next"></a>Wie geht es weiter?
Eine Übersicht zu Themen, die Sie durch die Aufgaben führen, die den Data Science-Prozess in Azure umfassen, finden Sie unter [Data Science-Prozess](http://aka.ms/datascienceprocess). 


