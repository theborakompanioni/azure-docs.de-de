---
title: "Übermitteln von MapReduce-Aufträgen mit HDInsight .NET SDK | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie MapReduce-Jobs mit HDInsight .NET-SDK an Azure HDInsight Hadoop übermitteln können."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 03f3898182d405bced8ad41b281e2ce2037b8343
ms.lasthandoff: 03/04/2017


---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Ausführen von MapReduce-Jobs mit HDInsight .NET SDK
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Erfahren Sie, wie Sie MapReduce-Jobs mit HDInsight .NET SDK übermitteln können. HDInsight-Cluster weisen eine JAR-Datei mit einigen MapReduce-Beispielen auf. Die JAR-Datei ist */example/jars/hadoop-mapreduce-examples.jar*.  Eines der Beispiele ist *Wordcount*. Sie entwickeln ein C#-Konsolenanwendungsprojekt zur Übermittlung eines Wordcount-Auftrags.  Der Auftrag liest die Datei */example/data/gutenberg/davinci.txt*, und gibt die Ergebnisse an */example/data/davinciwordcount* aus.  Wenn Sie die Anwendung erneut ausführen möchten, müssen Sie den Ausgabeordner bereinigen.

> [!NOTE]
> Die Schritte in diesem Artikel müssen auf einem Windows-Client ausgeführt werden. Informationen zur Verwendung eines Linux-, OS X- oder Unix-Clients für Hive erhalten Sie, indem Sie die Registerkartenauswahl am Anfang des Artikels nutzen.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Einen Hadoop-Cluster in HDInsight**. Siehe [Erste Schritte bei der Verwendung von Linux-basiertem Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Übermitteln von MapReduce-Jobs mit HDInsight .NET SDK
Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. 

**So übermitteln Sie Aufträge**

1. Erstellen Sie eine C#-Konsolenanwendung in Visual Studio.
2. Führen Sie in der NuGet-Paket-Manager-Konsole den folgenden Befehl aus.
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Verwenden Sie den folgenden Code:
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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
   
                private const string DefaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
   
                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
4. Drücken Sie **F5** , um die Anwendung auszuführen.

Um den Auftrag erneut auszuführen, müssen Sie den Ordnernamen der Auftragsausgabe ändern. Dieser lautet im Beispiel „/example/data/davinciwordcount“.

Wenn der Auftrag erfolgreich abgeschlossen wurde, ist die Ausgabe leer. Um das Ergebnis des MapReduce-Auftrags anzuzeigen, untersuchen Sie im Azure-Portal den Standardspeichercontainer im Blobspeicher.  Der Dateiname lautet „part-r-00000“.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Möglichkeiten zum Erstellen von HDInsight-Clustern kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* Informationen zum Übermitteln eines Hive-Auftrags finden Sie unter [Ausführen von Hive-Abfragen per HDInsight .NET-SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).
* Erstellen von HDInsight Clustern siehe [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Verwalten von HDInsight Clustern siehe [Verwalten von Hadoop-Clustern in HDInsight](hdinsight-administer-use-portal-linux.md).
* Grundlegendes zu HDInsight .NET SDK finden Sie unter [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx).
* Weitere Informationen zur nicht interaktiven Authentifizierung bei Azure finden Sie unter [Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung](hdinsight-create-non-interactive-authentication-dotnet-applications.md).


