---
title: "Übermitteln von MapReduce-Aufträgen mit HDInsight .NET SDK – Azure| Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie MapReduce-Jobs mit HDInsight .NET-SDK an Azure HDInsight Hadoop übermitteln können."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 368589509b163cacf495fd0be893a8953fe2066e
ms.openlocfilehash: 015435270c31bafea0ebf5303b459338755c1410
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

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

* **Einen Hadoop-Cluster in HDInsight**. Siehe [Erste Schritte bei der Verwendung von Linux-basiertem Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Übermitteln von MapReduce-Jobs mit HDInsight .NET SDK
Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. 

**So übermitteln Sie Aufträge**

1. Erstellen Sie eine C#-Konsolenanwendung in Visual Studio.
2. Führen Sie in der NuGet-Paket-Manager-Konsole den folgenden Befehl aus:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Verwenden Sie den folgenden Code:
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";
   
                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
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
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName);
        
                    if (jobDetail.ExitValue == 0)
                    {
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference to a previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
        
                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
        
                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 
        
                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }
        
                    }
                }
            }
        }
4. Drücken Sie **F5** , um die Anwendung auszuführen.

Um den Auftrag erneut auszuführen, müssen Sie den Ordnernamen der Auftragsausgabe ändern. Dieser lautet im Beispiel „/example/data/davinciwordcount“.

Bei erfolgreichem Abschluss des Auftrags gibt die Anwendung den Inhalt der Ausgabedatei „part-r-00000“ aus.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Möglichkeiten zum Erstellen von HDInsight-Clustern kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* Informationen zum Übermitteln eines Hive-Auftrags finden Sie unter [Ausführen von Hive-Abfragen per HDInsight .NET-SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).
* Erstellen von HDInsight Clustern siehe [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Verwalten von HDInsight Clustern siehe [Verwalten von Hadoop-Clustern in HDInsight](hdinsight-administer-use-portal-linux.md).
* Grundlegendes zu HDInsight .NET SDK finden Sie unter [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx).
* Weitere Informationen zur nicht interaktiven Authentifizierung bei Azure finden Sie unter [Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung](hdinsight-create-non-interactive-authentication-dotnet-applications.md).


