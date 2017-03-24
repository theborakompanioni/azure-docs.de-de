---
title: "Ausführen von Sqoop-Aufträgen mit .NET und Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem HDInsight .NET SDK einen Sqoop-Import und -Export zwischen einem Hadoop-Cluster und einer Azure-SQL-Datenbank durchführen können."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: bb700c7de96712666bc4be1f8e430a2e94761f69
ms.openlocfilehash: 9cadb72e065f82de8e007b38e909d2bc07d18126
ms.lasthandoff: 01/24/2017


---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Ausführen von Sqoop-Aufträgen mithilfe des .NET SDK für Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie das HDInsight .NET SDK zum Ausführen von Sqoop-Aufträgen in HDInsight zum Importieren und Exportieren zwischen HDInsight-Cluster und Azure SQL-Datenbank oder SQL Server-Datenbank verwendet werden kann.

> [!NOTE]
> Die Schritte in diesem Artikel können in Windows- oder Linux-basierten HDInsight-Clustern angewendet werden, sie funktionieren jedoch nur von einem Windows-Client aus. Wählen Sie über die Registerkartenauswahl am Anfang dieses Artikels andere Methoden aus.
> 
> 

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

* **Einen Hadoop-Cluster in HDInsight**. Informationen finden Sie unter [Erstellen von Cluster und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Ausführen von Sqoop mit dem .NET SDK
Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. In diesem Abschnitt erstellen Sie eine C#-Konsolenanwendung zum Exportieren der hivesampletable in die SQL-Datenbanktabelle, die Sie zuvor in diesem Lernprogramm erstellt haben.

**So übermitteln Sie einen Sqoop-Job**

1. Erstellen Sie eine C#-Konsolenanwendung in Visual Studio.
2. Führen Sie in der Paket-Manager-Konsole von Visual Studio den folgenden NuGet-Befehl aus, um das Paket zu importieren.
   
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
4. Drücken Sie die Taste **F5** , um das Programm auszuführen. 

## <a name="limitations"></a>Einschränkungen
* Massenexport: Bei Linux-basiertem HDInsight unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector derzeit keine Masseneinfügungen.
* Batchverarbeitung: Wenn Sie in Linux-basiertem HDInsight zum Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

## <a name="next-steps"></a>Nächste Schritte
Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

* [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md): Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
* [Analysieren von Daten zu Flugverspätungen mit HDInsight](hdinsight-analyze-flight-delay-data.md): Verwenden von Hive zur Analyse von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in die SQL-Datenbank.
* [Hochladen von Daten in HDInsight](hdinsight-upload-data.md): Andere Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.


