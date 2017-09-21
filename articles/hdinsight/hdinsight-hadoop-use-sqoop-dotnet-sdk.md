---
title: "Ausführen von Sqoop-Aufträgen mit .NET und HDInsight – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem HDInsight .NET-SDK einen Sqoop-Import und -Export zwischen einem Hadoop-Cluster und einer Azure SQL-Datenbank durchführen können."
keywords: Sqoop-Auftrag
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 2045cb714a6468d9d6fd054800c6e61a32bef390
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="run-sqoop-jobs-by-using-net-sdk-for-hadoop-in-hdinsight"></a>Ausführen von Sqoop-Aufträgen mithilfe des .NET-SDK für Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie das Azure HDInsight .NET-SDK zum Ausführen von Sqoop-Aufträgen in HDInsight zum Importieren und Exportieren zwischen HDInsight-Cluster und Azure SQL-Datenbank oder SQL Server-Datenbank verwendet werden kann.

> [!NOTE]
> Sie können die in diesem Artikel beschriebenen Verfahren zwar entweder mit einem Windows- oder Linux-basierten HDInsight-Cluster verwenden, doch sie funktionieren nur von einem Windows-Client aus. Um andere Methoden auszuwählen, verwenden Sie die Registerkartenauswahl am Anfang dieses Artikels.
> 

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* Einen Hadoop-Cluster in HDInsight. Weitere Informationen finden Sie unter [Erstellen von Cluster und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>Verwenden von Sqoop auf HDInsight-Clustern mit dem .NET-SDK
Das HDInsight .NET-SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. In diesem Abschnitt erstellen Sie eine C#-Konsolenanwendung zum Exportieren der „hivesampletable“ in die Azure SQL-Datenbanktabelle, die Sie zuvor in diesem Tutorial erstellt haben.

## <a name="submit-a-sqoop-job"></a>Übermitteln eines Sqoop-Auftrags

1. Erstellen Sie in Visual Studio eine C#-Konsolenanwendung.

2. Führen Sie in der Paket-Manager-Konsole von Visual Studio den folgenden NuGet-Befehl aus, um das Paket zu importieren:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

3. Verwenden Sie den folgenden Code in der Datei „Program.cs“:
   
        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }

4. Wählen Sie zum Ausführen des Programms die Taste **F5**. 

## <a name="limitations"></a>Einschränkungen
Für Linux-basiertes HDInsight gelten die folgenden Einschränkungen:

* Massenexport: Derzeit unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector keine Masseneinfügungen.

* Batchverarbeitung: Wenn Sie beim Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

## <a name="next-steps"></a>Nächste Schritte
Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

* [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md): Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
* [Analysieren von Daten zu Flugverspätungen mit HDInsight](hdinsight-analyze-flight-delay-data.md): Verwenden von Hive zur Analyse von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in die SQL-Datenbank.
* [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md): Andere Methoden zum Hochladen von Daten in HDInsight oder Azure Blob Storage.


