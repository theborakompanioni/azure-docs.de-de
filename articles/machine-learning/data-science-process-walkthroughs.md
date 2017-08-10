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
ms.date: 07/25/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: a678700884b612cad6281eb8f3b74ce63a0ebb69
ms.openlocfilehash: c0f0ff599909234b2b43f2b8512d77f9053b8a09
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="team-data-science-process-walkthroughs"></a>Exemplarische Vorgehensweisen für den Team Data Science-Prozess
Die hier aufgeführten umfassenden **exemplarischen Vorgehensweisen** veranschaulichen jeweils die Schritte im Team Data Science-Prozess für **bestimmte Szenarios**. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer **intelligenten Anwendung** kombiniert werden. Die exemplarischen Vorgehensweisen sind nach verwendeter **Plattform** gruppiert: 

- Spark mit PySpark und Scala
- HDInsight (Hadoop)
- Azure Data Lake 
- SQL Server
- SQL Data Warehouse 


## <a name="hdinsight-spark-using-pyspark-and-scala"></a>HDInsight Spark mit PySpark und Scala

- In der exemplarischen Vorgehensweise [Übersicht über Data Science mit Spark in Azure HDInsight](machine-learning-data-science-spark-overview.md) wird der Team Data Science-Prozess in einem Szenario mit einem [Azure HDInsight Spark-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet, um Daten aus dem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York zu speichern, zu untersuchen und bei der Entwicklung von Features zu nutzen.

- Die exemplarische Vorgehensweise [Data Science unter Verwendung von Scala und Spark in Azure](machine-learning-data-science-process-scala-walkthrough.md) zeigt die Verwendung von Scala für überwachte Machine Learning-Aufgaben mit der Machine Learning-Bibliothek (MLlib) von Spark und Spark ML-Paketen auf einem Azure HDInsight Spark-Cluster. Sie führt Sie durch die Aufgaben, aus denen der [Data Science-Prozess](http://aka.ms/datascienceprocess)besteht: Erfassen und Durchsuchen von Daten, Visualisierung, Featureentwicklung, Modellierung und Modellnutzung. Die erstellten Modelle umfassen logistische und lineare Regression, Random Forests und Gradient-Boosted-Strukturen.


## <a name="hdinsight-hadoop"></a>HDInsight Hadoop 

- In der exemplarischen Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von Azure HDInsight Hadoop-Clustern](machine-learning-data-science-process-hive-walkthrough.md) wird zum Speichern und Untersuchen von Daten aus einem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York sowie zum Entwickeln entsprechender Features ein [Azure HDInsight Hadoop-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet.

- In der exemplarischen Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von Azure HDInsight Hadoop-Clustern in einem 1-TB-Dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) wird zum Speichern, Untersuchen und Komprimieren von Daten aus einem öffentlich zugänglichen [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/)-Dataset sowie zum Entwickeln entsprechender Features ein [Azure HDInsight Hadoop-Cluster](https://azure.microsoft.com/services/hdinsight/) verwendet.


## <a name="azure-data-lake"></a>Azure Data Lake

- Unter [Skalierbare Data Science mit Azure Data Lake: lückenlose exemplarische Vorgehensweise](machine-learning-data-science-process-data-lake-walkthrough.md) wird veranschaulicht, wie Sie Azure Data Lake auf der Grundlage des Taxi-Datasets für New York zur Untersuchung von Daten sowie für binäre Klassifizierungsaufgaben nutzen, um vorherzusagen, ob ein Fahrgast Trinkgeld gibt. 


## <a name="sql-server-and-sql-data-warehouse"></a>SQL Server und SQL Data Warehouse 

- Die exemplarische Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Data Warehouse](machine-learning-data-science-process-sqldw-walkthrough.md) zeigt, wie Sie mit SQL Data Warehouse (SQL DW) Modelle für die Machine Learning-Klassifizierung und -Regression für ein öffentlich zugängliches Dataset mit Taxifahrten und Fahrpreisen für New York erstellen und bereitstellen.

- Die exemplarische Vorgehensweise [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md) zeigt, wie Sie mit SQL Server und einem öffentlich zugänglichen Dataset mit Taxifahrten und Fahrpreisen für New York Modelle für die Machine Learning-Klassifizierung und -Regression erstellen und bereitstellen.

- Die exemplarische Vorgehensweise [End-to-end data science walkthrough for R and SQL Server](https://msdn.microsoft.com/library/mt612857.aspx) (Lückenlose exemplarische Vorgehensweise für Data Science für R und SQL Server) bietet Data Scientists eine Kombination aus R-Code, SQL Server-Daten und benutzerdefinierten SQL-Funktionen für die Erstellung und Bereitstellung eines R-Modells für SQL Server.

- In der exemplarischen Vorgehensweise [In-database R analytics for SQL developers (tutorial)](https://msdn.microsoft.com/library/mt683480.aspx) (Datenbankinterne R-Analysen für SQL-Entwickler (Tutorial)) können SQL-Programmierer Erfahrungen mit der Erstellung einer erweiterten Analyselösung mit Transact-SQL unter Verwendung von SQL Server R-Diensten sammeln, um eine R-Lösung zu operationalisieren.

- In der exemplarischen Vorgehensweise [In-Database Python Analytics for SQL Developers](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) (Datenbankinterne Python-Analyse für SQL-Entwickler) können SQL-Programmierer Erfahrungen mit der Erstellung einer Machine Learning-Lösung in SQL Server sammeln. Sie veranschaulicht, wie Python in eine Anwendung integriert werden kann, indem gespeicherten Prozeduren Python-Code hinzugefügt wird.

## <a name="whats-next"></a>Wie geht es weiter?
Eine Übersicht zu Themen, die Sie durch die Aufgaben führen, die den Data Science-Prozess in Azure umfassen, finden Sie unter [Data Science-Prozess](http://aka.ms/datascienceprocess). 


