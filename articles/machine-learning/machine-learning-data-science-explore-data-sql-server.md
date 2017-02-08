---
title: Durchsuchen von Daten auf einer SQL Server-VM in Azure | Microsoft Docs
description: Durchsuchen von auf einer SQL Server-VM in Azure gespeicherten Daten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1654c100c1c57b04a61cf2b9d2cf37d58cec1870


---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Durchsuchen von Daten auf einer SQL Server-VM in Azure
In diesem Dokument wird beschrieben, wie Sie Daten durchsuchen, die in einer SQL Server-VM in Azure gespeichert sind. Dies kann über eine Datenanalyse mithilfe von SQL oder über die Verwendung einer Programmiersprache wie Python erreicht werden.

Das folgende **Menü** enthält Links zu den Themen, in denen die Verwendung dieser Tools zum Untersuchen von Daten aus verschiedenen Speicherumgebungen beschrieben wird. Diese Aufgabe ist ein Teil des Cortana-Analyseprozesses (CAP).

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> Bei den SQL-Beispielanweisungen in diesem Dokument wird davon ausgegangen, dass die Daten auf einem SQL Server gespeichert sind. Wenn dies nicht der Fall ist, finden Sie im Cloud Data Science-Prozess Anweisungen zum Verschieben der Daten zu einer SQL Server-Instanz.
> 
> 

## <a name="a-namesql-dataexplorationaexplore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>Durchsuchen von SQL-Daten mit SQL-Skripts
Hier finden Sie einige SQL-Beispielskripts, die zum Durchsuchen von Daten auf einem SQL Server verwendet werden können.

1. Abrufen der Anzahl der Vorkommen pro Tag:
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Abrufen der Ebenen in einer Kategoriespalte:
   
    `select  distinct <column_name> from <databasename>`
3. Abrufen der Anzahl von Ebenen in Kombination zweier kategorischer Spalten: 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Abrufen der Verteilung der numerischen Spalten 
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Für ein praktisches Beispiel können Sie das [NYC Taxi-Dataset](http://www.andresmh.com/nyctaxitrips/) verwenden. Eine vollständige exemplarische Vorgehensweise finden Sie im IPython Notebook-Beispiel unter [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb).
> 
> 

## <a name="a-namepythonaexplore-sql-data-with-python"></a><a name="python"></a>Durchsuchen von SQL-Daten mit Python
Die Verwendung von Python zum Durchsuchen von Daten und zum Generieren von Funktionen mit Daten in SQL Server ähnelt der Datenverarbeitung in Azure-Blobs mit Python, die unter [Verarbeiten von Azure-Blobdaten in Ihrer Data Science-Umgebung](machine-learning-data-science-process-data-blob.md)beschrieben ist. Die Daten müssen aus der Datenbank in ein Pandas-DataFrame geladen werden, um dann weiter verarbeitet werden zu können. In diesem Abschnitt werden das Herstellen einer Verbindung mit der Datenbank und das Laden der Daten in den DataFrame beschrieben.

Das folgende Format für die Verbindungszeichenfolge kann verwendet werden, um aus Python mit „pyodbc“ eine Verbindung mit einer SQL Server-Datenbank herzustellen (ersetzen Sie „servername“, „dbname“, „username“ und „password“ durch Ihre Daten):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Die [Pandas-Bibliothek](http://pandas.pydata.org/) in Python bietet eine Vielzahl an Datenstrukturen und Datenanalysetools für die Datenbearbeitung zur Python-Programmierung. Der folgende Code liest die aus einer SQL Server-Datenbank zurückgegebenen Ergebnisse in ein Pandas-DataFrame ein:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Sie können nun mit dem Pandas-DataFrame arbeiten. Die Vorgehensweise wird im Thema [Verarbeiten von Azure-Blobdaten mit erweiterter Analyse](machine-learning-data-science-process-data-blob.md) beschrieben.

## <a name="cortana-analytics-process-in-action-example"></a>Beispiel: Cortana-Analyseprozess (CAP) in Aktion
Eine lückenlose exemplarische Vorgehensweise zur Verwendung des Team Data Science-Prozesses mit einem öffentlichen Dataset finden Sie unter [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md).




<!--HONumber=Nov16_HO3-->


